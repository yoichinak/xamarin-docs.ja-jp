---
title: iOS アプリのアーキテクチャ
description: このドキュメントでは、大まかに説明し、ネイティブコードとマネージコードが対話する方法、AOT のコンパイル、セレクター、レジストラー、アプリケーションの起動、およびジェネレーターについて説明します。
ms.prod: xamarin
ms.assetid: F40F2275-17DA-4B4D-9678-618FF25C6803
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/21/2017
ms.openlocfilehash: b0cece7f553d0169c311e6614428ed37c5c77813
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70768534"
---
# <a name="ios-app-architecture"></a>iOS アプリのアーキテクチャ

Xamarin.iOS アプリケーションは Mono 実行環境内で実行され、C# コードを ARM アセンブリ言語コードにコンパイルするために完全な事前コンパイル (AOT) を使用します。 これは、[OBJECTIVE-C ランタイム](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ObjCRuntimeRef/)とサイド バイ サイドで実行されます。 両方のランタイム環境は、UNIX 風のカーネル (具体的には[XNU](https://en.wikipedia.org/wiki/XNU)) の上で実行され、基になるネイティブまたはマネージ システムに開発者がアクセスできるように、ユーザー コードにさまざまな API を公開します。

次の図は、このアーキテクチャの基本的な概要を示しています。

[![](architecture-images/ios-arch-small.png "この図は、事前に (AOT) コンパイルアーキテクチャの基本的な概要を示しています。")](architecture-images/ios-arch.png#lightbox)

## <a name="native-and-managed-code-an-explanation"></a>ネイティブ コードとマネージ コードについて説明

Xamarin で開発するときには、*ネイティブ コードとマネージ コード*という用語がよく使用されます。 [マネージ コード](https://blogs.msdn.microsoft.com/brada/2004/01/09/what-is-managed-code/)は、 [.NET Framework 共通言語ランタイム](https://msdn.microsoft.com/library/8bs2ecf4(v=vs.110).aspx)、 または、 Xamarin の場合は Mono ランタイムによって実行が管理されるコードです。 ここでは、中間言語を呼び出します。

ネイティブ コードとは、（例として、　Objective-C はもちろん、ARM チップ上で、AOT コンパイルされたコードも含む）特定のプラットフォームでネイティブに実行されるコードです。 このガイドでは、AOT がマネージコードをネイティブコードにコンパイルする方法、そして、　Xamarin.iOS アプリケーションがバインディングを使用して Apple の iOS API をフルに活用し、一方で、.NET の BCL と C# などの高度な言語にもアクセスしながら、どのように動作するかについて説明します。

## <a name="aot"></a>AOT

Xamarin プラットフォーム アプリケーションをコンパイルするときに、Mono C#(またはF#) コンパイラが実行され、C#とF#コードを Microsoft Intermediate Language (MSIL) にコンパイルします。 Xamarin.Android、Xamarin.Mac アプリケーションの場合、または Xamarin.iOS アプリケーションでも、シミュレーターで実行している場合、 [.NET 共通言語ランタイム (CLR)](https://msdn.microsoft.com/library/8bs2ecf4(v=vs.110).aspx)は Just in Time (JIT) コンパイラを使用して MSIL にコンパイルします。 実行時にこれはネイティブ コードにコンパイルされ、アプリケーションの適切なアーキテクチャで実行できます。

ただし、iOS では、Apple によって設定された、デバイス上で動的に生成されたコードの実行を禁止するセキュリティの制限があります。
これらの安全性のプロトコルを確実に守るために、Xamarin.iOS は、代わりに Ahead of Time (AOT) コンパイラをマネージ コードをコンパイルするのに使用します。 これにより、必要に応じて LLVM によってデバイス用に最適化されたネイティブの iOS バイナリが生成され、Apple の ARM ベースのプロセッサにデプロイできます。 これがどのように適合するのかをまとめた大まかな図を次に示します。

[![](architecture-images/aot.png "これがどのように組み合わされるかを大まかに説明した図")](architecture-images/aot-large.png#lightbox)

AOT の使用には多くの制限事項があります。それらの詳細については、[制限](~/ios/internals/limitations.md)ガイドで説明されています。 また、起動時間の短縮とさまざまなパフォーマンスの最適化によって、JIT に比べてさまざまな点が強化されています。

ソースからネイティブ コードにコードをコンパイルする方法について学習しましたので、Xamarin.iOS を使用して完全なネイティブ iOS アプリケーションを記述する方法を確認するために、内部を見てみましょう。

## <a name="selectors"></a>セレクター

Xamarin では、.NET と Apple の 2 つの独立したエコシステムがあり、最終目標であるスムーズなユーザー エクスペリエンスを実現するために、.NET と Apple を出来るだけシームレスに見えるように結びつける必要があります。 上のセクションでは 2 つのランタイムがどのように通信するかを見てきました。ネイティブの iOS API を Xamarin で使用できるようにする 'バインド' という用語をよくご存知でしょう。 バインドについては[OBJECTIVE-C のバインド](~/cross-platform/macios/binding/overview.md)のドキュメントで詳しく説明されています。 よってここでは iOS が内部的にどのように機能するのかを見ていきましょう。

まず、セレクターを使用して、目的の C をにC#公開する方法が必要です。 セレクターは、オブジェクトまたはクラスに送信されるメッセージです。 目標 C では、 [objc_msgSend](~/cross-platform/macios/binding/overview.md)関数を使用します。
セレクターの使用方法の詳細については、「[Objective-C セレクター](~/ios/internals/objective-c-selectors.md) ガイド」を参照してください。 また、マネージコードを目的の C に公開する方法が必要です。これは、目的の C でマネージコードについて何も知られていないことが原因で、より複雑になります。 この問題を回避するには、*レジストラー*を使用します。 これらの詳細については、次のセクションで詳しく説明します。

## <a name="registrars"></a>機関

前述したように、レジストラーはマネージ コードを Objective-C に公開するコードです。 これは、NSObject から派生したすべてのマネージ クラスのリストを作成することによって行われます。

- Objective-C クラスがラップされていないすべての既存のクラスに対して、 [`Export`] 属性を持つすべてのマネージ メンバーをミラー化した Objective-C のメンバーを持つ新しい Objective-C クラスを作成します。

- Objective-C の各メンバーの実装では、ミラー化されたマネージ メンバーを呼び出すためのコードが自動的に追加されます。

次の擬似コードは、その方法の例を示しています。

**C#(マネージコード)**

```csharp
 class MyViewController : UIViewController{
     [Export ("myFunc")]
     public void MyFunc ()
     {
     }
 }
```

**Objective-C:**

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

マネージコードには、オブジェクトを目的`[Register]`の`[Export]`C に公開する必要があることをレジストラーが認識するために使用する属性、およびを含めることができます。
`[Register]`属性を使用して、生成された目的の C クラスの名前を指定します (既定で生成される名前が適切でない場合)。 NSObject から派生したすべてのクラスは、自動的にに登録されます。
Required `[Export]`属性には、生成された目的 C クラスで使用されるセレクターである文字列が含まれています。

Xamarin では、動的と静的の2種類のレジストラーが使用されています。

- **動的**レジストラー: 動的レジストラーは、実行時にアセンブリ内のすべての型の登録を行います。 これを行うには、[目的 C のランタイム API](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ObjCRuntimeRef/)によって提供される関数を使用します。 そのため、動的レジストラーは起動速度が遅くなり、ビルド時間が短縮されます。 これは、iOS シミュレーターの既定の設定です。 動的レジストラーを使用する場合、ネイティブ関数 (通常は C) は trampolines と呼ばれ、メソッドの実装として使用されます。 アーキテクチャによって異なります。

- **静的**レジストラー–静的レジストラーは、ビルド中に目標 C コードを生成し、その後、スタティックライブラリにコンパイルされ、実行可能ファイルにリンクされます。 これにより、起動が高速になりますが、ビルド時にかかる時間は長くなります。 これは、デバイスビルドに対して既定で使用されます。 静的レジストラーは、次に示すように、プロジェクトのビルド`--registrar:static`オプションで`mtouch`属性として渡すことによって、iOS シミュレーターでも使用できます。

    [![](architecture-images/image1.png "追加の mtouch 引数の設定")](architecture-images/image1.png#lightbox)

Xamarin. iOS で使用される iOS の種類の登録システムの詳細については、 [「タイプレジスタ](~/ios/internals/registrar.md)ガイド」を参照してください。

## <a name="application-launch"></a>アプリケーションの起動

すべての Xamarin iOS 実行可能ファイルのエントリポイントは、mono を初期化`xamarin_main`するという関数によって提供されます。

プロジェクトの種類に応じて、次の処理が行われます。

- 通常の iOS および tvOS アプリケーションでは、Xamarin アプリによって提供されるマネージ Main メソッドが呼び出されます。 次に、このマネージ Main `UIApplication.Main`メソッドはを呼び出します。これは、目的の C のエントリポイントです。 Uiapplication。 Main は、目的 C の`UIApplicationMain`メソッドのバインディングです。
- 拡張機能では、Apple ライブラリ `NSExtensionMain`によっ`NSExtensionmain`て提供されるネイティブ関数 (WatchOS 拡張機能) が呼び出されます。 これらのプロジェクトはクラスライブラリであり、実行可能なプロジェクトではないため、実行するマネージドの主要なメソッドはありません。

この起動シーケンスはすべてスタティックライブラリとしてコンパイルされ、最終的な実行可能ファイルにリンクされます。これにより、アプリは、グラウンドをオフにする方法を知ることができます。

この時点で、アプリが起動し、Mono が実行されています。マネージコードで、ネイティブコードを呼び出してコールバックする方法がわかっています。 次に行う必要があるのは、実際にコントロールの追加を開始し、アプリを対話型にすることです。

## <a name="generator"></a>ジェネレーター

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

Xamarin. iOS で[`btouch`](https://github.com/xamarin/xamarin-macios/blob/master/src/btouch.cs)呼び出されたジェネレーターは、これらの定義ファイルを受け取り、.net ツールを使用して[それらを一時アセンブリにコンパイル](https://github.com/xamarin/xamarin-macios/blob/master/src/btouch.cs#L318)します。 ただし、この一時アセンブリは、Objective-C コードを呼び出すことができません。 その後、ジェネレーターは一時アセンブリを読み取り、実行時に使用できる C# コードを生成します。
このため、たとえば、ランダムな属性を定義の .cs ファイルに追加しても、出力されるコードには表示されません。 ジェネレーターはそれを認識しないため`btouch` 、一時アセンブリ内でそれを検索して出力することはできません。

Xamarin が作成されると、mtouch によってすべてのコンポーネントがバンドルされます。

大まかに言えば、次のタスクを実行してこれを実現します。

- アプリバンドル構造を作成します。
- をマネージアセンブリにコピーします。
- リンクが有効になっている場合は、マネージリンカーを実行して、使用されていないパーツを取り込み、アセンブリを最適化します。
- AOT のコンパイル。
- ネイティブの実行可能ファイルを作成します。これにより、ネイティブの実行可能ファイルにリンクされている一連のスタティックライブラリ (アセンブリごとに1つ) が出力されます。これにより、ネイティブの実行可能ファイルはランチャーコード、レジストラーコード (静的な場合)、および AOT からのすべての出力を構成します。コンパイラー

リンカーとその使用方法の詳細については、[リンカー](~/ios/deploy-test/linker.md)ガイドを参照してください。

## <a name="summary"></a>まとめ

このガイドでは、Xamarin iOS アプリの AOT のコンパイルと、Xamarin の詳細について詳しく説明しました。

## <a name="related-links"></a>関連リンク

- [制限事項](~/ios/internals/limitations.md)
- [Objective-C のバインド](~/cross-platform/macios/binding/overview.md)
- [目標-C セレクター](~/ios/internals/objective-c-selectors.md)
- [型レジストラー](~/ios/internals/registrar.md)
- [リンカー](~/ios/deploy-test/linker.md)
