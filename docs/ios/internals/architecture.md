---
title: iOS のアーキテクチャ
description: Xamarin.iOS 低レベルでの探索
ms.prod: xamarin
ms.assetid: F40F2275-17DA-4B4D-9678-618FF25C6803
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 930b52e5b2a532e71594f26af79035db2cc5fb25
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="ios-architecture"></a>iOS のアーキテクチャ

Xamarin.iOS アプリケーションでは、Mono 実行環境内で実行およびフルの今後の時間 (AOT) コンパイルを使用して ARM アセンブリ言語を c# コードをコンパイルします。 サイド バイ サイド実行で、 [Objective C ランタイム](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ObjCRuntimeRef/)です。 具体的には、UNIX に似たカーネル上に両方のランタイム環境を実行[XNU](https://en.wikipedia.org/wiki/XNU)、および開発者は基になるネイティブまたはマネージ システムへのアクセスを許可するユーザー コードにさまざまな Api を公開します。

次の図は、このアーキテクチャの基本的な概要を示しています。

[ ![](architecture-images/ios-arch-small.png "この図は、基本的なアーキテクチャの概要、今後の時間 (AOT) コンパイル")](architecture-images/ios-arch.png#lightbox)

## <a name="native-and-managed-code-an-explanation"></a>ネイティブおよびマネージ コード: の説明

Xamarin の開発時に、用語*ネイティブおよびマネージ*コードはよく使用されます。 [マネージ コード](https://blogs.msdn.microsoft.com/brada/2004/01/09/what-is-managed-code/)によって管理されている、実行のあるコードが、 [.NET Framework 共通言語ランタイム](https://msdn.microsoft.com/en-us/library/8bs2ecf4(v=vs.110).aspx)、または Xamarin の場合: モノラル ランタイム。 これは、中間言語と呼ばれるものです。

ネイティブ コードには (たとえば、OBJECTIVE-C または ARM チップでは、偶数のコンパイル AOT コード) は、特定のプラットフォームでネイティブに実行されます。 このガイドでは、AOT がネイティブ コードに、マネージ コードをコンパイルする方法について説明し、使用を増加させるバインディングを使用して、Apple の iOS Api の状態もへのアクセス方法、Xamarin.iOS アプリケーションの動作について説明します。NET の BCL と c# などの高度な言語です。


## <a name="aot"></a>AOT

Xamarin プラットフォーム アプリケーションをコンパイルするときに、モノラル c# (f#) コンパイラが実行され、Microsoft Intermediate Language (MSIL) に、c# および f# コードがコンパイルされます。 Xamarin.Android、Xamarin.Mac アプリケーション、または、Xamarin.iOS アプリケーションでも、シミュレーターで実行している場合、 [.NET 共通言語ランタイム (CLR)](https://msdn.microsoft.com/en-us/library/8bs2ecf4(v=vs.110).aspx)タイム (JIT) コンパイラで、のみを使用して MSIL にコンパイルします。 これはネイティブ コードにコンパイルされ、実行時に、アプリケーションの適切なアーキテクチャで実行されることができます。

ただし、ios の場合、apple によってデバイスに動的に生成されたコードの実行を禁止されている設定のセキュリティの制限があります。
これらの安全性のプロトコルに従っていることを確認してくださいするには、Xamarin.iOS はマネージ コードをコンパイルするのに代わりにデータの前方の時間 (AOT) コンパイラを使用します。 必要に応じてデバイス、Apple の ARM ベースのプロセッサ上で展開されることがある LLVM と最適化、バイナリのネイティブの iOS が生成されます。 これが適合するしくみまとめての大まかなダイアグラムを次に示します。

[![](architecture-images/aot.png "これが適合するしくみ化の概要図")](architecture-images/aot-large.png#lightbox)

詳細については、制限事項の数を持つ AOT を使用して、[制限](~/ios/internals/limitations.md)ガイドです。 提供多数の機能強化 JIT 経由でスタートアップ時間、およびさまざまなパフォーマンスの最適化でのリダクション経由

これで、ネイティブ コードをソースからコードがコンパイルされた方法について説明してきました、見てみましょうフードの下部にどのように Xamarin.iOS により、完全ネイティブの iOS アプリケーションを作成するを参照してください。

## <a name="selectors"></a>セレクター

Xamarin である .NET および Apple にするために必要な 2 つの独立したエコシステム見える最終目標がスムーズなユーザー エクスペリエンスであることを確認することとして、簡素化するためにします。 2 つのランタイムが通信する方法の上のセクションに説明したようし、よく聞いたことにより、ネイティブの iOS Xamarin では使用する Api の用語 'のバインディング' のです。 バインディングがで詳しく説明されている、 [OBJECTIVE-C バインディング](~/cross-platform/macios/binding/overview.md)ドキュメントについては、ためここで見てみましょう、内部での iOS のしくみです。

最初に C# の場合、セレクターを使用して実行するには、OBJECTIVE-C を公開する方法があります。 セレクターは、オブジェクトまたはクラスに送信されるメッセージです。 Objective C を使用してこれは、 [objc_msgSend](~/cross-platform/macios/binding/overview.md)関数。
セレクターを使用する方法についてを参照してください、 [OBJECTIVE-C セレクター](~/ios/internals/objective-c-selectors.md)ガイドです。 存在するために、マネージ コードに関する何も認識 Objective C はより複雑な OBJECTIVE-C、マネージ コードを公開する方法もあります。 使用してこの問題を回避する*レジストラー*です。 これらは、次のセクションで詳しく説明します。

## <a name="registrars"></a>レジストラー

レジストラーが目標 C に公開するマネージ コードのコードで前述したように、 これは、NSObject から派生したすべてのマネージ クラスのリストを作成します。

- Objective C の既存のクラスをラップしていないすべてのクラス、そのクラスを作成新しい Objective C Objective C のメンバーを持つすべてのマネージ メンバーをミラーリングと、[`Export`] 属性です。

- – Objective C の各メンバーの実装では、ミラー化されたマネージ メンバーを呼び出す、コードが自動的に追加します。

次の疑似コードは、これを行う方法の例を示します。

**C# (マネージ コード)**

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

マネージ コードは、属性を含めることができます`[Register]`と`[Export]`レジストラーを使用して、オブジェクトが目標 C. に公開する必要があることを確認するには
`[Register]`属性を使用して、生成された既定の名前が不適切な場合に生成された OBJECTIVE-C クラスの名前を指定します。 NSObject から派生したすべてのクラスが目標 C. に自動的に登録されています。
必要な`[Export]`属性には、生成された OBJECTIVE-C クラスで使用されるセレクターである文字列が含まれています。

Xamarin.iOS – 動的および静的で使用されるレジストラーの 2 つの種類があります。


- **動的レジストラー** – 動的レジストラーは実行時に、アセンブリ内のすべての種類を登録します。 これは、によって提供される関数を使用して[OBJECTIVE-C のランタイム API](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ObjCRuntimeRef/)です。 したがって、動的レジストラーには、低速の起動より高速ながビルド時があります。 これは、iOS シミュレーターの既定値です。 ネイティブ機能 (通常 C)、領域と呼ばれるは、動的なレジストラーを使用する場合、メソッドの実装として使用されます。 これらは、異なるアーキテクチャによって異なります。

- **静的レジストラー** – 静的レジストラーはスタティック ライブラリにコンパイルし、実行可能ファイルにリンクすると、ビルド時に Objective C コードが生成されます。 これが迅速に開始できますが、ビルド時に時間がかかります。 デバイスのビルドの既定で使用されます。 静的レジストラーこともできます、iOS シミュレーターに渡すことによって`--registrar:static`として、`mtouch`次に示すように、プロジェクトのビルド オプションでは、属性します。

    [![](architecture-images/image1.png "追加 mtouch 引数の設定")](architecture-images/image1.png#lightbox)

IOS Xamarin.iOS で使用する型の登録のシステムの仕様の詳細についてを参照してください、[型レジストラー](~/ios/internals/registrar.md)ガイドです。

## <a name="application-launch"></a>アプリケーションの起動

Xamarin.iOS 実行可能ファイルのすべてのエントリ ポイントが呼び出される関数によって提供される`xamarin_main`モノラルを初期化します。

プロジェクトの種類に応じて、次のように行われます。

- 正規の iOS デバイスと tvOS アプリケーションでの Xamarin アプリで提供される管理対象の Main メソッドが呼び出されます。 これは管理 Main メソッドを呼び出して`UIApplication.Main`、目標 C. のエントリ ポイントがあります。 UIApplication.Main OBJECTIVE-C のバインドは、`UIApplicationMain`メソッドです。
- 拡張機能、ネイティブ関数 –`NSExtensionMain`または (`NSExtensionmain` WatchOS 拡張機能の) ライブラリを呼び出す Apple によって提供される – です。 これらのプロジェクトは、クラス ライブラリといない実行可能プロジェクトを実行する管理対象の Main メソッドはありません。

この起動シーケンスのすべてが、アプリは、地上から取得する方法を認識しているため、最終的な実行可能ファイルにそのリンクをスタティック ライブラリにコンパイルされます。

アプリが開始された時点でこのモノラルが実行されている、マネージ コードでは、ネイティブ コードを呼び出すし、コールバックされる方法を知る。 次のことを行う必要がありますを実際にはコントロールの追加を開始し、アプリを対話的です。


## <a name="generator"></a>ジェネレーター

Xamarin.iOS には、すべて 1 つの iOS API の定義が含まれています。 これらのいずれかを参照、 [github リポジトリの MaciOS](https://github.com/xamarin/xamarin-macios/tree/master/src)です。 これらの定義には、インターフェイス、属性を持つだけでなく、必要なメソッドとプロパティが含まれます。 たとえば、次のコードは、UIKit で、UIToolbar を定義するために使用[名前空間](https://github.com/xamarin/xamarin-macios/blob/master/src/uikit.cs#L11277-L11327)です。 メソッドとプロパティの数がインターフェイスであることに注意してください。

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

呼ばれる、ジェネレーター [ `btouch` ](https://github.com/xamarin/xamarin-macios/blob/master/src/btouch.cs) Xamarin.iOS でこれらの定義ファイルは、.NET ツールを使用して[一時アセンブリにコンパイルする](https://github.com/xamarin/xamarin-macios/blob/master/src/btouch.cs#L318)です。 ただし、この一時アセンブリは Objective C コードを呼び出すために使用可能ではありません。 ジェネレーターは、一時アセンブリを読み取るし、実行時に使用できる c# コードを生成します。
これは、理由、たとえば、定義 .cs ファイルにランダムな属性を追加する場合に表示されません出力されるコードでです。 ジェネレーターはそれを知るため`btouch`された一時アセンブリに出力することで探すことを認識しません。

Xamarin.iOS.dll が作成されたら、mtouch はすべてのコンポーネントを一緒にバンドルします。

大まかに言えば、これを行う、次のタスクを実行することで。

-   アプリ バンドルの構造を作成します。
-   マネージ アセンブリをコピーします。
-   リンクが有効になっている場合は、取り込みに使用されていない部分をアセンブリを最適化するマネージ リンカーを実行します。
-   AOT コンパイルします。
-   一連の静的のライブラリ (アセンブリごとに 1 つ)、ネイティブの実行可能ファイルにリンクされているを出力するため、ネイティブの実行可能ファイルは、ランチャー コード (静的) 場合は、レジストラー コードおよび AOT からのすべての出力のネイティブ実行ファイルを作成します。コンパイラ


詳細については、リンカーとその使用方法についてを参照してください、[リンカー](~/ios/deploy-test/linker.md)ガイドです。

## <a name="summary"></a>まとめ

このガイドは、Xamarin.iOS アプリ、および探索済み Xamarin.iOS 深さで Objective C の関係の AOT コンパイルで検索します。

## <a name="related-links"></a>関連リンク

- [制限事項](~/ios/internals/limitations.md)
- [Objective-C のバインド](~/cross-platform/macios/binding/overview.md)
- [Objective C セレクター](~/ios/internals/objective-c-selectors.md)
- [型のレジストラー](~/ios/internals/registrar.md)
- [リンカー](~/ios/deploy-test/linker.md)
