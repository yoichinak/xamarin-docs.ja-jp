---
title: Xamarin.Mac アーキテクチャ
description: このガイドでは、低レベルで OBJECTIVE-C に Xamarin.Mac との関係について説明します。 これには、コンパイル、セレクター、レジストラー、アプリの起動、およびコード ジェネレーターなどの概念について説明します。
ms.prod: xamarin
ms.assetid: 74D1FF57-4F2A-4646-8669-003DE99671D4
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/12/2017
ms.openlocfilehash: d6d7557fed5ea0ca0719dcbddbda316340645320
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
ms.locfileid: "30785557"
---
# <a name="xamarinmac-architecture"></a>Xamarin.Mac アーキテクチャ

_このガイドでは、低レベルで OBJECTIVE-C に Xamarin.Mac との関係について説明します。これには、コンパイル、セレクター、レジストラー、アプリの起動、およびコード ジェネレーターなどの概念について説明します。_

## <a name="overview"></a>概要

Xamarin.Mac アプリケーションでは、Mono 実行環境内で実行し、Xamarin のコンパイラを使用してのみ-ジャスト イン タイム (JIT) 実行時にネイティブ コードにコンパイルは、中間言語 (IL) までをコンパイルします。 サイド バイ サイド実行 Objective C のランタイムとします。 両方のランタイム環境では、UNIX に似たカーネル、XNU 具体的には、上で実行し、開発者は基になるネイティブまたはマネージ システムへのアクセスを許可するユーザー コードにさまざまな Api を公開します。

次の図は、このアーキテクチャの基本的な概要を示しています。

[![アーキテクチャの基本的な概要を示す図](architecture-images/mac-arch.png "アーキテクチャの基本的な概要を示す図")](architecture-images/mac-arch-large.png#lightbox)

### <a name="native-and-managed-code"></a>ネイティブおよびマネージ コード

、Xamarin の開発時に、用語*ネイティブ*と*マネージ*コードはよく使用されます。 マネージ コードがマネージ コードで、.NET Framework 共通言語ランタイムで、または Xamarin のケースでそれを実行するコードは、: モノラル ランタイム。

ネイティブ コードには (たとえば、OBJECTIVE-C または ARM チップでは、偶数のコンパイル AOT コード) は、特定のプラットフォームでネイティブに実行されます。 このガイドにアクセスすることも中にバインディングを使用して、Apple の Mac Api の使用を増加させるマネージ コードのネイティブ コードにコンパイルされ、Xamarin.Mac アプリケーションの動作方法について説明しますについて説明します。NET の BCL と c# などの高度な言語です。

## <a name="requirements"></a>要件

Xamarin.Mac を使って macOS アプリケーションを開発するには、以下のものが必要です。

- Mac 実行 macOS Sierra (10.12) 以上です。
- Xcode の最新バージョン (からインストールされている、 [App Store](https://itunes.apple.com/us/app/xcode/id497799835?mt=12))
- Xamarin.Mac と Visual Studio for Mac の最新バージョン

Xamarin.Mac で作成された Mac アプリケーションを実行するには、次のシステム要件があります。

- X 10.7 以上 Mac OS を実行している Mac。

## <a name="compilation"></a>コンパイル

Xamarin プラットフォーム アプリケーションをコンパイルするときに、モノラル c# (f#) コンパイラが実行され、Microsoft Intermediate Language (MSIL または IL) に、c# および f# コードがコンパイルされます。 Xamarin.Mac を使用して、*ジャスト イン タイム (JIT)* 必要に応じて、適切なアーキテクチャでの実行を許可する、ネイティブ コードをコンパイルする実行時にコンパイラです。

これは、Xamarin.iOS AOT コンパイルを使用するとは対照的です。 AOT コンパイラを使用する場合、ビルド時にすべてのアセンブリとそれらに含まれるすべてのメソッドはコンパイルされます。 JIT でコンパイルにのみこれらのメソッドが実行を要求時に発生します。

モノラルは通常、アプリ バンドルに埋め込ま Xamarin.Mac アプリケーション (と呼ばれます**埋め込みモノラル**)。 代わりに、アプリケーションを使用でしたクラシック Xamarin.Mac API を使用する場合**システム モノラル**、ただし、これはサポートされず、Unified API です。 システム モノラルは、オペレーティング システムにインストールされている Mono を指します。 アプリケーションの起動時にはこの Xamarin.Mac アプリは使用します。

## <a name="selectors"></a>セレクター

Xamarin である .NET および Apple にするために必要な 2 つの独立したエコシステム見える最終目標がスムーズなユーザー エクスペリエンスであることを確認することとして、簡素化するためにします。 2 つのランタイムが通信する方法の上のセクションに説明したようし、よく聞いたことにより、Xamarin で使用されるネイティブ Mac Api 用語 'のバインディング' のです。 バインディングがで詳しく説明されている、 [OBJECTIVE-C バインディング ドキュメント](~/mac/platform/binding.md)ここでは、ので、Xamarin.Mac が内部でどのように機能するかを見てみましょう。

最初に C# の場合、セレクターを使用して実行するには、OBJECTIVE-C を公開する方法があります。 セレクターは、オブジェクトまたはクラスに送信されるメッセージです。 Objective C を使用してこれは、 [objc_msgSend](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ObjCRuntimeRef/index.html)関数。 セレクターを使用する方法については、iOS を参照してください[OBJECTIVE-C セレクター](~/ios/internals/objective-c-selectors.md)ガイドです。 存在するために、マネージ コードに関する何も認識 Objective C はより複雑な OBJECTIVE-C、マネージ コードを公開する方法もあります。 使用してこの問題を回避する、[レジストラー](~/mac/internals/registrar.md)です。 これは、次のセクションで詳しく説明します。

## <a name="registrar"></a>レジストラー

レジストラーが目標 C に公開するマネージ コードのコードで前述したように、 これは、NSObject から派生したすべてのマネージ クラスのリストを作成します。

- Objective C の既存のクラスをラップしていないすべてのクラス、そのクラスを作成新しい Objective C Objective C のメンバーを持つすべてのマネージ メンバーをミラーリングと、`[Export]`属性。
- – Objective C の各メンバーの実装では、ミラー化されたマネージ メンバーを呼び出す、コードが自動的に追加します。

次の疑似コードは、これを行う方法の例を示します。

**C# の場合 (マネージ コード):**

```csharp
class MyViewController : UIViewController{
    [Export ("myFunc")]
    public void MyFunc ()
    {
    }
 }
 ```

**目標-c (ネイティブ コード):**

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

マネージ コードは、属性を含めることができます`[Register]`と`[Export]`レジストラーを使用して、オブジェクトが目標 C. に公開する必要があることを確認するには [レジスタ] 属性は、生成された既定の名前が不適切な場合に生成された OBJECTIVE-C クラスの名前を指定に使用します。 NSObject から派生したすべてのクラスが目標 C. に自動的に登録されています。 [エクスポート] の必須の属性には、生成された OBJECTIVE-C クラスで使用されるセレクターである文字列が含まれています。

レジストラー Xamarin.Mac – 動的および静的では使用の 2 つの種類があります。

- 動的レジストラー – これは、すべての Xamarin.Mac ビルドの既定レジストラーです。 動的レジストラーでは、実行時に、アセンブリ内のすべての種類の登録。 この機能を使用するには、OBJECTIVE-C のランタイム API によって提供される関数を使用します。 したがって、動的レジストラーには、低速の起動より高速ながビルド時があります。 ネイティブ機能 (通常 C)、領域と呼ばれるは、動的なレジストラーを使用する場合、メソッドの実装として使用されます。 これらは、異なるアーキテクチャによって異なります。
- 静的レジストラー – 静的レジストラーでは、スタティック ライブラリにコンパイルされ、実行可能ファイルにリンクされている、ビルド時に Objective C コードが生成されます。 これが迅速に開始できますが、ビルド時に時間がかかります。

## <a name="application-launch"></a>アプリケーションの起動

Xamarin.Mac スタートアップ ロジックによって異なります埋め込まれているかどうかまたはシステム モノラルが使用されます。 アプリケーションの起動を Xamarin.Mac の手順とコードを表示するを参照してください、[起動ヘッダー](https://github.com/xamarin/xamarin-macios/blob/master/runtime/xamarin/launch.h) xamarin macios パブリック リポジトリ内のファイルです。

## <a name="generator"></a>ジェネレーター

Xamarin.Mac には、Mac のすべての API の定義が含まれています。 これらのいずれかを参照、 [github リポジトリの MaciOS](https://github.com/xamarin/xamarin-macios/tree/master/src)です。 これらの定義には、インターフェイス、属性を持つだけでなく、必要なメソッドとプロパティが含まれます。 たとえば、次のコードは、NSBox での定義に使用、 [AppKit 名前空間](https://github.com/xamarin/xamarin-macios/blob/master/src/appkit.cs#L1465-L1526)です。 メソッドとプロパティの数がインターフェイスであることに注意してください。

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

呼ばれる、ジェネレーター `bmac` Xamarin.Mac でこれらの定義ファイルを受け取り、一時アセンブリにコンパイルする .NET ツールを使用します。 ただし、この一時アセンブリは Objective C コードを呼び出すために使用可能ではありません。 ジェネレーターは、一時アセンブリを読み取るし、実行時に使用できる c# コードを生成します。 これは、理由、たとえば、定義 .cs ファイルにランダムな属性を追加する場合に表示されません出力されるコードでです。 ジェネレーターが把握して、および`bmac`された一時アセンブリに出力することで探すことを認識しません。

Xamarin.Mac.dll が作成されたら、パッケー ジャー、 `mmp`、すべてのコンポーネントを一緒にバンドルされます。

大まかに言えば、これを行う、次のタスクを実行することで。

- アプリ バンドルの構造を作成します。
- マネージ アセンブリをコピーします。
- リンクが有効になっている場合は、使用されていない部分を削除することで、アセンブリを最適化するために、マネージ リンカーを実行します。
- 静的なモードの場合、registar コードと共に話しランチャー コード内のリンク、ランチャー アプリケーションを作成します。

これは、ユーザーの一部として実行は、その参照の Xamarin.Mac.dll と実行にユーザー コードをアセンブリにコンパイルされるプロセスをビルドし、`mmp`パッケージを作成するには

詳細については、リンカーとその使用方法についてを参照してください、iOS[リンカー](~/ios/deploy-test/linker.md)ガイドです。

## <a name="summary"></a>まとめ

このガイドは Xamarin.Mac アプリ、および探索済み Xamarin.Mac 目標 C への関係のコンパイル時参照
