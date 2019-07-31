---
title: iOS アプリのアーキテクチャ
description: このドキュメントでは、低レベルで説明するネイティブとマネージ コードの対話、AOT コンパイル、セレクター、レジストラー、アプリケーションの起動、およびコード ジェネレーターで Xamarin.iOS をについて説明します。
ms.prod: xamarin
ms.assetid: F40F2275-17DA-4B4D-9678-618FF25C6803
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 426c5ef5cc32877546ebb88cb485a81723816e6e
ms.sourcegitcommit: 58d8bbc19ead3eb535fb8248710d93ba0892e05d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/09/2019
ms.locfileid: "67675066"
---
# <a name="ios-app-architecture"></a>iOS アプリのアーキテクチャ

Xamarin.iOS アプリケーションは Mono 実行環境内で実行され、C# コードを ARM アセンブリ言語コードにコンパイルするために完全な事前コンパイル (AOT) を使用します。 これは、[OBJECTIVE-C ランタイム](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ObjCRuntimeRef/)とサイド バイ サイドで実行されます。 両方のランタイム環境は、UNIX 風のカーネル (具体的には[XNU](https://en.wikipedia.org/wiki/XNU)) の上で実行され、基になるネイティブまたはマネージ システムに開発者がアクセスできるように、ユーザー コードにさまざまな API を公開します。

次の図は、このアーキテクチャの基本的な概要を示します。

[![](architecture-images/ios-arch-small.png "この図では、事前の Time (AOT) コンパイルのアーキテクチャの基本的な概要を示しています。")](architecture-images/ios-arch.png#lightbox)

## <a name="native-and-managed-code-an-explanation"></a>ネイティブおよびマネージ コードの場合:詳細について

Xamarin で開発するときには、*ネイティブ コードとマネージ コード*という用語がよく使用されます。 [マネージ コード](https://blogs.msdn.microsoft.com/brada/2004/01/09/what-is-managed-code/)は、 [.NET Framework 共通言語ランタイム](https://msdn.microsoft.com/library/8bs2ecf4(v=vs.110).aspx)、 または、 Xamarin の場合は Mono ランタイムによって実行が管理されるコードです。 これは、中間言語と呼ばれるものです。

ネイティブ コードとは、（例として、　Objective-C はもちろん、ARM チップ上で、AOT コンパイルされたコードも含む）特定のプラットフォームでネイティブに実行されるコードです。 このガイドでは、AOT がマネージコードをネイティブコードにコンパイルする方法、そして、　Xamarin.iOS アプリケーションがバインディングを使用して Apple の iOS API をフルに活用し、一方で、.NET の BCL と C# などの高度な言語にもアクセスしながら、どのように動作するかについて説明します。

## <a name="aot"></a>AOT

Mono、Xamarin プラットフォーム アプリケーションをコンパイルするときにC#(またはF#) コンパイラが実行され、コンパイルは、C#とF#コード Microsoft Intermediate Language (MSIL) にします。 Xamarin.Android、Xamarin.Mac アプリケーションの場合、または Xamarin.iOS アプリケーションでも、シミュレーターで実行している場合、 [.NET 共通言語ランタイム (CLR)](https://msdn.microsoft.com/library/8bs2ecf4(v=vs.110).aspx)タイム (JIT) コンパイラですればを使用して MSIL にコンパイルします。 これはネイティブ コードにコンパイル、実行時に、アプリケーションの適切なアーキテクチャで実行されることができます。

ただし、ios では、Apple で、デバイスに動的に生成されたコードを実行することによって設定されるセキュリティの制限があります。
私たちは、これらの安全性のプロトコルに従うことを確認するには、Xamarin.iOS は、事前の Time (AOT) コンパイラをマネージ コードをコンパイルするのに代わりに使用します。 必要に応じてデバイス、Apple の ARM ベースのプロセッサにデプロイできる LLVM の最適化、バイナリのネイティブの iOS が生成されます。 これがどのまとめての大まかな図は、次に示します。

[![](architecture-images/aot.png "どのようにこの適合化の概要図")](architecture-images/aot-large.png#lightbox)

詳細については、制限事項の数値を持つ AOT を使用して、[制限](~/ios/internals/limitations.md)ガイド。 また、さまざまな点が強化 JIT 経由で、起動時間とさまざまなパフォーマンスの最適化を削減することを

ネイティブ コードにコードをソースからコンパイルする方法について学習しましたが、これで内部的に Xamarin.iOS を使用してできる完全なネイティブ iOS アプリケーションを記述する方法を確認してみましょう

## <a name="selectors"></a>セレクター

Xamarin では、.NET と Apple の 2 つの独立したエコシステムがあり、最終目標であるスムーズなユーザー エクスペリエンスを実現するために、.NET と Apple を出来るだけシームレスに見えるように結びつける必要があります。 上のセクションでは 2 つのランタイムがどのように通信するかを見てきました。ネイティブの iOS API を Xamarin で使用できるようにする 'バインド' という用語をよくご存知でしょう。 バインドについては[OBJECTIVE-C のバインド](~/cross-platform/macios/binding/overview.md)のドキュメントで詳しく説明されています。 よってここでは iOS が内部的にどのように機能するのかを見ていきましょう。

最初に、OBJECTIVE-C を公開する方法がありますがC#、これは、セレクターを使用して行われます。 セレクターは、オブジェクトまたはクラスに送信されるメッセージです。 Objective C を使用してこれには、 [objc_msgSend](~/cross-platform/macios/binding/overview.md)関数。
セレクターの使用に関する詳細についてを参照してください、 [OBJECTIVE-C セレクター](~/ios/internals/objective-c-selectors.md)ガイド。 Objective C をマネージ コードについて何も知らないという事実が原因でより複雑なある OBJECTIVE-C では、マネージ コードを公開する方法もありますがあります。 使用してこれを回避するには、*レジストラー*します。 これらは、次のセクションで詳しく説明します。

## <a name="registrars"></a>レジストラー

レジストラーが OBJECTIVE-C にその公開のマネージ コードのコードで前述したように、 これは、NSObject から派生したすべてのマネージ クラスのリストを作成します。

- Objective-C クラスがラップされていないすべての既存のクラスに対して、 [`Export`] 属性を持つすべてのマネージ メンバーをミラー化した Objective-C のメンバーを持つ新しい Objective-C クラスを作成します。

- Objective-C の各メンバーの実装では、ミラー化されたマネージ メンバーを呼び出すためのコードが自動的に追加されます。

次の擬似コードでは、これを行う方法の例を示します。

**C#(マネージ コード)**

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

マネージ コードは、属性を含めることができます`[Register]`と`[Export]`レジストラーを使用して、オブジェクトを OBJECTIVE-C に公開される必要があることを確認するには
`[Register]`属性を使用して、生成された既定の名前が適していない場合に生成された OBJECTIVE-C クラスの名前を指定します。 NSObject から派生したすべてのクラスは、OBJECTIVE-C と自動的に登録します。
必要な`[Export]`属性には、生成された OBJECTIVE-C のクラスで使用されるセレクターである文字列が含まれています。

動的および静的 – Xamarin.iOS で使用されているレジストラーの 2 種類あります。


- **動的なレジストラー** – 動的レジストラーは実行時に、アセンブリ内のすべての種類の登録。 これはによって提供される関数を使用して、 [OBJECTIVE-C のランタイム API](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ObjCRuntimeRef/)します。 したがって、動的なレジストラーには、低速スタートアップより高速なが、ビルド時間があります。 これは、iOS シミュレーターの既定値です。 領域と呼ばれます (通常は C) でネイティブ関数は、動的なレジストラーを使用する場合、メソッドの実装として使用されます。 これらは、さまざまなアーキテクチャとは異なります。

- **静的なレジストラー** – 静的レジストラーでは、スタティック ライブラリにコンパイルされ、実行可能ファイルにリンクし、ビルド中に OBJECTIVE-C コードが生成されます。 これにより、高速スタートアップが、ビルド時に時間がかかります。 これは、デバイスのビルドの場合、既定で使用されます。 静的なレジストラーこともできますを iOS シミュレーターで渡すことによって`--registrar:static`として、`mtouch`プロジェクトのビルド オプションでは、次に示すように属性します。

    [![](architecture-images/image1.png "その他の mtouch 引数の設定")](architecture-images/image1.png#lightbox)

IOS Xamarin.iOS で使用される型の登録システムの詳細についてを参照してください、[型レジストラー](~/ios/internals/registrar.md)ガイド。

## <a name="application-launch"></a>アプリケーションの起動

すべての Xamarin.iOS の実行可能ファイルのエントリ ポイントが呼び出される関数によって提供される`xamarin_main`mono を初期化します。

プロジェクトの種類に応じて、次のように行われます。

- 標準的な iOS と tvOS アプリケーションの場合は、Xamarin アプリによって提供される、管理対象の Main メソッドが呼び出されます。 これは管理 Main メソッドを呼び出して`UIApplication.Main`OBJECTIVE-C のエントリ ポイントであります。 UIApplication.Main for OBJECTIVE-C のバインドは、`UIApplicationMain`メソッド。
- 拡張機能のネイティブ関数 – `NSExtensionMain`または (`NSExtensionmain` WatchOS 拡張機能の) ライブラリを呼び出す Apple によって提供される – します。 これらのプロジェクトは、クラス ライブラリとプロジェクトのない実行可能ファイルは、管理対象の Main メソッドを実行することはありません。

この起動シーケンスのすべては、アプリは、地上から取得する方法を認識できるようにし、最終的な実行可能ファイルにリンクされる、スタティック ライブラリにコンパイルされます。

アプリが開始されるこの時点で、Mono が実行されている、マネージ コードではおよびネイティブ コードを呼び出すし、コールバックの方法を知っています。 実際にコントロールの追加を開始し、対話型アプリケーションを作成するに次のことです。


## <a name="generator"></a>ジェネレーター

Xamarin.iOS には、すべて 1 つの iOS API の定義が含まれています。 これらのいずれかを参照できます、 [MaciOS github リポジトリ](https://github.com/xamarin/xamarin-macios/tree/master/src)します。 これらの定義には、インターフェイス、属性を持つだけでなく、必要なメソッドとプロパティが含まれます。 UIKit、UIToolbar を定義する次のコードを使用するなど、[名前空間](https://github.com/xamarin/xamarin-macios/blob/master/src/uikit.cs#L11277-L11327)します。 メソッドとプロパティの数がインターフェイスであることを確認します。

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

ジェネレーターと呼ばれる[ `btouch` ](https://github.com/xamarin/xamarin-macios/blob/master/src/btouch.cs) 、Xamarin.iOS では、これらの定義ファイルとは .NET ツールを使用して[一時アセンブリにコンパイル](https://github.com/xamarin/xamarin-macios/blob/master/src/btouch.cs#L318)します。 ただし、この一時アセンブリは Objective C コードを呼び出すために有効ではありません。 ジェネレーターは、し、一時アセンブリの読み取りを生成C#実行時に使用できるコード。
これは、理由など、定義の .cs ファイルにランダムな属性を追加する場合に表示されませんで出力されるコード。 ジェネレーターは、それについて認識しないため`btouch`一時アセンブリに出力することで探すように認識しません。

Xamarin.iOS.dll が作成されたら、mtouch はすべてのコンポーネントをまとめてバンドルします。

大まかに言えば、これを行う、次のタスクを実行しています。

-   アプリ バンドルの構造を作成します。
-   マネージ アセンブリをコピーします。
-   リンクが有効になっている場合は、取り込みに使用されていない部分で、アセンブリを最適化するためにマネージ リンカーを実行します。
-   AOT コンパイルします。
-   一連の静的のライブラリ (アセンブリごとに 1 つ)、ネイティブの実行可能ファイルにリンクされているを出力して、ネイティブの実行可能ファイルは、ランチャー コード (静的) 場合は、レジストラー コードおよび AOT からのすべての出力のネイティブ実行ファイルを作成します。コンパイラ


詳細については、リンカーとその使用方法についてを参照してください、[リンカー](~/ios/deploy-test/linker.md)ガイド。

## <a name="summary"></a>まとめ

このガイドは、Xamarin.iOS アプリ、および探索 Xamarin.iOS と OBJECTIVE-C の詳細の関係の AOT コンパイルに検索します。

## <a name="related-links"></a>関連リンク

- [制限事項](~/ios/internals/limitations.md)
- [Objective-C のバインド](~/cross-platform/macios/binding/overview.md)
- [OBJECTIVE-C セレクター](~/ios/internals/objective-c-selectors.md)
- [型のレジストラー](~/ios/internals/registrar.md)
- [リンカー](~/ios/deploy-test/linker.md)
