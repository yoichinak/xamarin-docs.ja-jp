---
title: Xamarin. Mac アーキテクチャ
description: このガイドでは、Xamarin の Mac と、下位レベルの目標 C との関係について説明します。 ここでは、コンパイル、セレクター、レジストラー、アプリの起動、ジェネレーターなどの概念について説明します。
ms.prod: xamarin
ms.assetid: 74D1FF57-4F2A-4646-8669-003DE99671D4
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 04/12/2017
ms.openlocfilehash: 2c9bbd663257e937e35e062f03b4aa84813edb27
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70287786"
---
# <a name="xamarinmac-architecture"></a>Xamarin. Mac アーキテクチャ

_このガイドでは、Xamarin の Mac と、下位レベルの目標 C との関係について説明します。ここでは、コンパイル、セレクター、レジストラー、アプリの起動、ジェネレーターなどの概念について説明します。_

## <a name="overview"></a>概要

Xamarin アプリケーションは Mono 実行環境内で実行され、Xamarin のコンパイラを使用して中間言語 (IL) にコンパイルされます。これは、実行時にネイティブコードにコンパイルされます。 これは、目的 C ランタイムとサイドバイサイドで実行されます。 どちらのランタイム環境も、UNIX のようなカーネル (具体的には XNU) 上で実行され、さまざまな Api をユーザーコードに公開して、開発者が基になるネイティブシステムまたはマネージシステムにアクセスできるようにします。

次の図は、このアーキテクチャの基本的な概要を示しています。

[![アーキテクチャの基本的な概要を示す図](architecture-images/mac-arch.png "アーキテクチャの基本的な概要を示す図")](architecture-images/mac-arch-large.png#lightbox)

### <a name="native-and-managed-code"></a>ネイティブコードとマネージコード

Xamarin 用に開発する場合、*ネイティブ*コードと*マネージ*コードという用語がよく使用されます。 マネージコードは、.NET Framework 共通言語ランタイム、または Xamarin のケース (Mono ランタイム) によって管理される実行を持つコードです。

ネイティブ コードとは、（例として、　Objective-C はもちろん、ARM チップ上で、AOT コンパイルされたコードも含む）特定のプラットフォームでネイティブに実行されるコードです。 このガイドでは、マネージコードがどのようにネイティブコードにコンパイルされるかについて説明し、Xamarin アプリケーションの動作のしくみについて説明します。また、バインディングを使用して Apple の Mac Api を完全に使用し、にもアクセスできるようにします。NET の BCL となどの高度な言語C#。

## <a name="requirements"></a>必要条件

Xamarin.Mac を使って macOS アプリケーションを開発するには、以下のものが必要です。

- MacOS Sierra (10.12) 以上を実行している Mac。
- 最新バージョンの Xcode ( [App Store](https://itunes.apple.com/us/app/xcode/id497799835?mt=12)からインストールされます)
- 最新バージョンの Xamarin. Mac と Visual Studio for Mac

Xamarin.Mac で作成された Mac アプリケーションを実行するには、次のシステム要件があります。

- 10.7 以上の Mac OS X 実行されている Mac。

## <a name="compilation"></a>コンパイル

Xamarin platform アプリケーションをコンパイルするC#と、Mono (またはF#) コンパイラが実行され、 C#とF#コードが Microsoft 中間言語 (MSIL または IL) にコンパイルされます。 次に、実行時に*ジャストインタイム (JIT)* コンパイラを使用してネイティブコードをコンパイルし、必要に応じて適切なアーキテクチャで実行できるようにします。

これは、AOT コンパイルを使用する Xamarin とは対照的です。 AOT コンパイラを使用する場合、すべてのアセンブリとその中のすべてのメソッドがビルド時にコンパイルされます。 JIT では、コンパイルは、実行されたメソッドに対してのみ要求時に行われます。

Xamarin アプリケーションでは、Mono は通常、アプリバンドルに埋め込まれます ("**埋め込み mono**" と呼ばれます)。 従来の Xamarin. Mac API を使用する場合、アプリケーションは**システム Mono**を使用することができますが、これは Unified API ではサポートされていません。 システム Mono とは、オペレーティングシステムにインストールされている Mono を指します。 アプリケーションの起動時に、Xamarin アプリケーションはこれを使用します。

## <a name="selectors"></a>セレクター

Xamarin では、.NET と Apple の 2 つの独立したエコシステムがあり、最終目標であるスムーズなユーザー エクスペリエンスを実現するために、.NET と Apple を出来るだけシームレスに見えるように結びつける必要があります。 上記のセクションでは、2つのランタイムが通信する方法について説明しました。また、ネイティブ Mac Api を Xamarin で使用できるようにする "バインド" という用語についてもよく知られています。 バインディングの詳細については、 [C のバインドに関するドキュメント](~/mac/platform/binding.md)で説明します。そのため、ここでは、Xamarin. Mac が内部でどのように機能するかを見てみましょう。

まず、セレクターを使用して、目的の C をにC#公開する方法が必要です。 セレクターは、オブジェクトまたはクラスに送信されるメッセージです。 目標 C では、 [objc_msgSend](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ObjCRuntimeRef/index.html)関数を使用します。 セレクターの使用方法の詳細については、「iOS の[目標 C セレクター](~/ios/internals/objective-c-selectors.md)ガイド」を参照してください。 また、マネージコードを目的の C に公開する方法が必要です。これは、目的の C でマネージコードについて何も知られていないことが原因で、より複雑になります。 この問題を回避するには、[レジストラー](~/mac/internals/registrar.md)を使用します。 詳細については、次のセクションで説明します。

## <a name="registrar"></a>レジストラー

前述したように、レジストラーはマネージ コードを Objective-C に公開するコードです。 これは、NSObject から派生したすべてのマネージ クラスのリストを作成することによって行われます。

- 既存の目的 c クラスをラップしていないすべてのクラスに対して、属性を`[Export]`持つすべてのマネージメンバーをミラーリングする目的の c メンバーを持つ新しい目標 c クラスを作成します。
- Objective-C の各メンバーの実装では、ミラー化されたマネージ メンバーを呼び出すためのコードが自動的に追加されます。

次の擬似コードは、その方法の例を示しています。

**C#(マネージコード):**

```csharp
class MyViewController : UIViewController{
    [Export ("myFunc")]
    public void MyFunc ()
    {
    }
 }
 ```

**目的-C (ネイティブコード):**

```objc
@interface MyViewController : UIViewController
 - (void)myFunc;
@end 

@implementation MyViewController
- (void)myFunc {
    // Code to call the managed C# MyFunc method in MyViewController
}
@end
```

マネージコードには、オブジェクトを目的`[Register]`の`[Export]`C に公開する必要があることをレジストラーが認識するために使用する属性、およびを含めることができます。 既定の生成された名前が適切でない場合に備えて、生成された目的の C クラスの名前を指定するには、[Register] 属性を使用します。 NSObject から派生したすべてのクラスは、自動的にに登録されます。 必須の [Export] 属性には、生成された目的 C クラスで使用されるセレクターである文字列が含まれています。

Xamarin では、動的と静的の2種類のレジストラーが使用されています。

- 動的レジストラー–これは、すべての Xamarin. Mac ビルドの既定のレジストラーです。 動的レジストラーは、実行時にアセンブリ内のすべての型の登録を行います。 これを行うには、目的 C のランタイム API によって提供される関数を使用します。 そのため、動的レジストラーは起動速度が遅くなり、ビルド時間が短縮されます。 動的レジストラーを使用する場合、ネイティブ関数 (通常は C) は trampolines と呼ばれ、メソッドの実装として使用されます。 アーキテクチャによって異なります。
- 静的レジストラー–静的レジストラーは、ビルド中に目標 C コードを生成し、その後、スタティックライブラリにコンパイルされ、実行可能ファイルにリンクされます。 これにより、起動が高速になりますが、ビルド時にかかる時間は長くなります。

## <a name="application-launch"></a>アプリケーションの起動

Xamarin. Mac スタートアップロジックは、埋め込みまたはシステム Mono が使用されているかどうかによって異なります。 Xamarin. Mac アプリケーションの起動のコードと手順を確認するには、xamarin-macios パブリックリポジトリの[起動ヘッダー](https://github.com/xamarin/xamarin-macios/blob/master/runtime/xamarin/launch.h)ファイルを参照してください。

## <a name="generator"></a>ジェネレーター

Xamarin. Mac には、すべての Mac API の定義が含まれています。 [Macios github リポジトリ](https://github.com/xamarin/xamarin-macios/tree/master/src)でこれらのいずれかを参照できます。 これらの定義には、属性を持つインターフェイスだけでなく、必要なメソッドとプロパティも含まれています。 たとえば、次のコードは、 [Appkit 名前空間](https://github.com/xamarin/xamarin-macios/blob/master/src/appkit.cs#L1465-L1526)で nsbox を定義するために使用されます。 これは、いくつかのメソッドとプロパティを持つインターフェイスであることに注意してください。

```csharp
[BaseType (typeof (NSView))]
public interface NSBox {

    …

    [Export ("borderRect")]
    CGRect BorderRect { get; }

    [Export ("titleRect")]
    CGRect TitleRect { get; }

    [Export ("titleCell")]
    NSObject TitleCell { get; }

    [Export ("sizeToFit")]
    void SizeToFit ();

    [Export ("contentViewMargins")]
    CGSize ContentViewMargins { get; set; }

    [Export ("setFrameFromContentFrame:")]
    void SetFrameFromContentFrame (CGRect contentFrame);

    …

}
```

このジェネレーターは、 `bmac` Xamarin. Mac で呼び出され、これらの定義ファイルを受け取り、.net ツールを使用してそれらを一時アセンブリにコンパイルします。 ただし、この一時アセンブリは、目的の C コードを呼び出すことができません。 その後、ジェネレーターは一時アセンブリを読み取りC# 、実行時に使用できるコードを生成します。 このため、たとえば、ランダムな属性を定義の .cs ファイルに追加しても、出力されるコードには表示されません。 ジェネレーターはそれを認識しないため`bmac` 、一時アセンブリ内でそれを出力するように検索することはできません。

Xamarin. .dll が作成されると、パッケージャー `mmp`によってすべてのコンポーネントがバンドルされます。

大まかに言えば、次のタスクを実行してこれを実現します。

- アプリバンドル構造を作成します。
- をマネージアセンブリにコピーします。
- リンクが有効になっている場合は、マネージリンカーを実行して、使用されていない部分を削除してアセンブリを最適化します。
- ランチャーアプリケーションを作成します。ランチャーコードにリンクし、静的モードの場合はレジストラーコードと共に説明します。

これはユーザーのビルドプロセスの一部として実行され、ユーザーコードを、Xamarin. .dll を参照するアセンブリにコンパイル`mmp`して実行し、パッケージにすることができます。

リンカーとその使用方法の詳細については、iOS[リンカー](~/ios/deploy-test/linker.md)ガイドを参照してください。

## <a name="summary"></a>Summary

このガイドでは、Xamarin. Mac アプリのコンパイルについて見てきました。さらに、Xamarin. Mac とその関係を説明しました。
