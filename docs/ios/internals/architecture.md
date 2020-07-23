---
title: iOS アプリのアーキテクチャ
description: このドキュメントでは、大まかに説明し、ネイティブコードとマネージコードが対話する方法、AOT のコンパイル、セレクター、レジストラー、アプリケーションの起動、およびジェネレーターについて説明します。
ms.prod: xamarin
ms.assetid: F40F2275-17DA-4B4D-9678-618FF25C6803
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 832071023bfe2ea9d947946ff2b70428d93fc866
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86937544"
---
# <a name="ios-app-architecture"></a>iOS アプリのアーキテクチャ

Xamarin iOS アプリケーションは Mono 実行環境内で実行され、完全な事前 (AOT) コンパイルを使用して、C# コードを ARM アセンブリ言語にコンパイルします。 これは、[目的 C ランタイム](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ObjCRuntimeRef/)とサイドバイサイドで実行されます。 どちらのランタイム環境も、UNIX のようなカーネル (具体的には[XNU](https://en.wikipedia.org/wiki/XNU)) 上で実行され、さまざまな api をユーザーコードに公開して、開発者が基になるネイティブシステムまたはマネージシステムにアクセスできるようにします。

次の図は、このアーキテクチャの基本的な概要を示しています。

[![この図は、事前に (AOT) コンパイルアーキテクチャの基本的な概要を示しています。](architecture-images/ios-arch-small.png)](architecture-images/ios-arch.png#lightbox)

## <a name="native-and-managed-code-an-explanation"></a>ネイティブコードとマネージコード: 説明

Xamarin 用に開発する場合、*ネイティブコードとマネージ*コードの用語がよく使用されます。 [マネージコード](https://blogs.msdn.microsoft.com/brada/2004/01/09/what-is-managed-code/)は、 [.NET Framework 共通言語ランタイム](https://msdn.microsoft.com/library/8bs2ecf4(v=vs.110).aspx)、または Xamarin のケース (Mono ランタイム) によって管理される実行を持つコードです。 ここでは、中間言語を呼び出します。

ネイティブコードは、特定のプラットフォームでネイティブに実行されるコードです (たとえば、ARM チップでは、目標 C や AOT でコンパイルされたコードなど)。 このガイドでは、AOT によってマネージコードがネイティブコードにコンパイルされるしくみについて説明し、Xamarin アプリケーションの動作のしくみについて説明します。また、バインディングを使用して Apple の iOS Api を最大限に活用し、にアクセスすることもできます。NET の BCL と、C# などの高度な言語。

## <a name="aot"></a>AOT

Xamarin platform アプリケーションをコンパイルすると、Mono C# (または F #) コンパイラが実行され、C# と F # コードが Microsoft 中間言語 (MSIL) にコンパイルされます。 Xamarin Android、Xamarin. Mac アプリケーション、またはシミュレーターで xamarin iOS アプリケーションを実行している場合、 [.Net 共通言語ランタイム (CLR)](https://msdn.microsoft.com/library/8bs2ecf4(v=vs.110).aspx)はジャストインタイム (JIT) コンパイラを使用して MSIL をコンパイルします。 実行時に、これはネイティブコードにコンパイルされ、アプリケーションの適切なアーキテクチャで実行できます。

ただし、iOS には、Apple によって設定されたセキュリティ制限があります。これにより、デバイスで動的に生成されたコードを実行できません。
これらの安全性プロトコルに準拠していることを確認するために、Xamarin は、事前に (AOT) コンパイラを使用してマネージコードをコンパイルします。 これにより、必要に応じて、Apple の ARM ベースのプロセッサに展開できる、デバイスの LLVM で最適化されたネイティブの iOS バイナリが生成されます。 次の図は、これがどのように組み合わされているかを大まかに示しています。

[![これがどのように組み合わされるかを大まかに説明した図](architecture-images/aot.png)](architecture-images/aot-large.png#lightbox)

AOT の使用にはいくつかの制限事項があります。これについては、[制限事項](~/ios/internals/limitations.md)ガイドで詳しく説明します。 また、起動時間を短縮し、さまざまなパフォーマンスの最適化を行うことにより、JIT に対する多くの機能強化が提供されます。

これで、コードがソースコードからネイティブコードにコンパイルされた方法を説明しました。次は、Xamarin. iOS で完全にネイティブな iOS アプリケーションを作成する方法について説明します。

## <a name="selectors"></a>セレクター

Xamarin では、.NET と Apple という2つの個別のエコシステムがあり、最終的な目標がスムーズなユーザーエクスペリエンスであることを保証するために、可能な限り合理化する必要があります。 上記のセクションでは、2つのランタイムが通信する方法について説明しました。また、ネイティブ iOS Api を Xamarin で使用できるようにする "バインド" という用語についてもよく知られています。 バインディングの詳細については、 [C のバインド](~/cross-platform/macios/binding/overview.md)ドキュメントを参照してください。そのため、ここでは iOS が内部でどのように動作するかを見てみましょう。

まず、目標 C を C# に公開する方法が必要です。これは、セレクターを使用して行います。 セレクターは、オブジェクトまたはクラスに送信されるメッセージです。 目標 C では、 [objc_msgSend](~/cross-platform/macios/binding/overview.md)関数を使用して実行します。
セレクターの使用方法の詳細については、「[目標 C セレクター](~/ios/internals/objective-c-selectors.md)ガイド」を参照してください。 また、マネージコードを目的の C に公開する方法が必要です。これは、目的の C でマネージコードについて何も知られていないことが原因で、より複雑になります。 この問題を回避するには、*レジストラー*を使用します。 これらの詳細については、次のセクションで詳しく説明します。

## <a name="registrars"></a>機関

前述のように、レジストラーはマネージコードを目的の C に公開するコードです。 これを行うには、NSObject から派生したすべてのマネージクラスのリストを作成します。

- 既存の目的 C クラスをラップしていないすべてのクラスについて、[] 属性を持つすべてのマネージメンバーをミラーリングする目的の c メンバーを持つ新しい目標 C クラスを作成し `Export` ます。

- 各目標-C メンバーの実装では、ミラー化されたマネージメンバーを呼び出すために、コードが自動的に追加されます。

次の擬似コードは、その方法の例を示しています。

**C# (マネージコード)**

```csharp
 class MyViewController : UIViewController{
     [Export ("myFunc")]
     public void MyFunc ()
     {
     }
 }
```

**目的-C:**

```objectivec
@interface MyViewController : UIViewController { }

    -(void)myFunc;
@end

@implementation MyViewController {}

    -(void) myFunc
    {
        /* code to call the managed MyViewController.MyFunc method */
    }
@end

```

マネージコードには、 `[Register]` オブジェクトを目的の `[Export]` C に公開する必要があることをレジストラーが認識するために使用する属性、およびを含めることができます。
属性を使用して、生成された `[Register]` 目的の C クラスの名前を指定します (既定で生成される名前が適切でない場合)。 NSObject から派生したすべてのクラスは、自動的にに登録されます。
Required 属性には、 `[Export]` 生成された目的 C クラスで使用されるセレクターである文字列が含まれています。

Xamarin では、動的と静的の2種類のレジストラーが使用されています。

- **動的**レジストラー: 動的レジストラーは、実行時にアセンブリ内のすべての型の登録を行います。 これを行うには、[目的 C のランタイム API](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ObjCRuntimeRef/)によって提供される関数を使用します。 そのため、動的レジストラーは起動速度が遅くなり、ビルド時間が短縮されます。 これは、iOS シミュレーターの既定の設定です。 動的レジストラーを使用する場合、ネイティブ関数 (通常は C) は trampolines と呼ばれ、メソッドの実装として使用されます。 アーキテクチャによって異なります。

- **静的**レジストラー–静的レジストラーは、ビルド中に目標 C コードを生成し、その後、スタティックライブラリにコンパイルされ、実行可能ファイルにリンクされます。 これにより、起動が高速になりますが、ビルド時にかかる時間は長くなります。 これは、デバイスビルドに対して既定で使用されます。 静的レジストラーは、 `--registrar:static` `mtouch` 次に示すように、プロジェクトのビルドオプションで属性として渡すことによって、iOS シミュレーターでも使用できます。

    [![追加の mtouch 引数の設定](architecture-images/image1.png)](architecture-images/image1.png#lightbox)

Xamarin. iOS で使用される iOS の種類の登録システムの詳細については、 [「タイプレジスタ](~/ios/internals/registrar.md)ガイド」を参照してください。

## <a name="application-launch"></a>アプリケーションの起動

すべての Xamarin iOS 実行可能ファイルのエントリポイントは `xamarin_main` 、mono を初期化するという関数によって提供されます。

プロジェクトの種類に応じて、次の処理が行われます。

- 通常の iOS および tvOS アプリケーションでは、Xamarin アプリによって提供されるマネージ Main メソッドが呼び出されます。 次に、このマネージ Main メソッド `UIApplication.Main` はを呼び出します。これは、目的の C のエントリポイントです。 UIApplication。 Main は、目的 C のメソッドのバインディングです。 `UIApplicationMain`
- 拡張機能では、  `NSExtensionMain` `NSExtensionmain` Apple ライブラリによって提供されるネイティブ関数 (WatchOS 拡張機能) が呼び出されます。 これらのプロジェクトはクラスライブラリであり、実行可能なプロジェクトではないため、実行するマネージドの主要なメソッドはありません。

この起動シーケンスはすべてスタティックライブラリとしてコンパイルされ、最終的な実行可能ファイルにリンクされます。これにより、アプリは、グラウンドをオフにする方法を知ることができます。

この時点で、アプリが起動し、Mono が実行されています。マネージコードで、ネイティブコードを呼び出してコールバックする方法がわかっています。 次に行う必要があるのは、実際にコントロールの追加を開始し、アプリを対話型にすることです。

## <a name="generator"></a>Generator

Xamarin iOS には、すべての単一の iOS API の定義が含まれています。 [Macios github リポジトリ](https://github.com/xamarin/xamarin-macios/tree/master/src)でこれらのいずれかを参照できます。 これらの定義には、属性を持つインターフェイスだけでなく、必要なメソッドとプロパティも含まれています。 たとえば、次のコードは、UIKit[名前空間](https://github.com/xamarin/xamarin-macios/blob/master/src/uikit.cs#L11277-L11327)で UIToolbar を定義するために使用されます。 これは、いくつかのメソッドとプロパティを持つインターフェイスであることに注意してください。

```csharp
[BaseType (typeof (UIView))]
public interface UIToolbar : UIBarPositioning {
    [Export ("initWithFrame:")]
    IntPtr Constructor (CGRect frame);

    [Export ("barStyle")]
    UIBarStyle BarStyle { get; set; }

    [Export ("items", ArgumentSemantic.Copy)][NullAllowed]
    UIBarButtonItem [] Items { get; set; }

    [Export ("translucent", ArgumentSemantic.Assign)]
    bool Translucent { [Bind ("isTranslucent")] get; set; }

    // done manually so we can keep this "in sync" with 'Items' property
    //[Export ("setItems:animated:")][PostGet ("Items")]
    //void SetItems (UIBarButtonItem [] items, bool animated);

    [Since (5,0)]
    [Export ("setBackgroundImage:forToolbarPosition:barMetrics:")]
    [Appearance]
    void SetBackgroundImage ([NullAllowed] UIImage backgroundImage, UIToolbarPosition position, UIBarMetrics barMetrics);

    [Since (5,0)]
    [Export ("backgroundImageForToolbarPosition:barMetrics:")]
    [Appearance]
    UIImage GetBackgroundImage (UIToolbarPosition position, UIBarMetrics barMetrics);

    ...
}
```

Xamarin. iOS で呼び出されたジェネレーターは、 [`btouch`](https://github.com/xamarin/xamarin-macios/blob/master/src/btouch.cs) これらの定義ファイルを受け取り、.net ツールを使用して[それらを一時アセンブリにコンパイル](https://github.com/xamarin/xamarin-macios/blob/master/src/btouch.cs#L318)します。 ただし、この一時アセンブリは、目的の C コードを呼び出すことができません。 その後、ジェネレーターは、一時アセンブリを読み取り、実行時に使用できる C# コードを生成します。
このため、たとえば、ランダムな属性を定義の .cs ファイルに追加しても、出力されるコードには表示されません。 ジェネレーターはそれを認識しないため、 `btouch` 一時アセンブリ内でそれを検索して出力することはできません。

Xamarin.iOS.dll が作成されると、mtouch によってすべてのコンポーネントがまとめられます。

大まかに言えば、次のタスクを実行してこれを実現します。

- アプリバンドル構造を作成します。
- をマネージアセンブリにコピーします。
- リンクが有効になっている場合は、マネージリンカーを実行して、使用されていないパーツを取り込み、アセンブリを最適化します。
- AOT のコンパイル。
- ネイティブの実行可能ファイルを作成します。ネイティブの実行可能ファイルにリンクされている一連のスタティックライブラリ (アセンブリごとに1つ) が出力されます。これにより、ネイティブの実行可能ファイルはランチャーコード、レジストラーコード (静的な場合)、および AOT コンパイラからのすべての出力を構成します。

リンカーとその使用方法の詳細については、[リンカー](~/ios/deploy-test/linker.md)ガイドを参照してください。

## <a name="summary"></a>要約

このガイドでは、Xamarin iOS アプリの AOT のコンパイルと、Xamarin の詳細について詳しく説明しました。

## <a name="related-links"></a>関連リンク

- [制限事項](~/ios/internals/limitations.md)
- [Objective-C のバインディング](~/cross-platform/macios/binding/overview.md)
- [目標-C セレクター](~/ios/internals/objective-c-selectors.md)
- [型レジストラー](~/ios/internals/registrar.md)
- [リンカー](~/ios/deploy-test/linker.md)
