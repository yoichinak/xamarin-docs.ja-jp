---
title: Xamarin.Mac のアーキテクチャ
description: このガイドでは、低レベルでは、OBJECTIVE-C を Xamarin.Mac との関係について説明します。 これには、コンパイル、セレクター、レジストラー、アプリの起動、およびコード ジェネレーターなどの概念について説明します。
ms.prod: xamarin
ms.assetid: 74D1FF57-4F2A-4646-8669-003DE99671D4
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 04/12/2017
ms.openlocfilehash: 1ea38b527acaa89b9f25690de4e55664a7afd9e8
ms.sourcegitcommit: d09391c315336d36496880ef465a72b8974f2ac7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/13/2018
ms.locfileid: "51579831"
---
# <a name="xamarinmac-architecture"></a>Xamarin.Mac のアーキテクチャ

_このガイドでは、低レベルでは、OBJECTIVE-C を Xamarin.Mac との関係について説明します。これには、コンパイル、セレクター、レジストラー、アプリの起動、およびコード ジェネレーターなどの概念について説明します。_

## <a name="overview"></a>概要

Xamarin.Mac アプリケーションでは、Mono 実行環境内で実行され、Xamarin のコンパイラはジャスト-タイム (JIT) 実行時にネイティブ コードにコンパイルし、中間言語 (IL) までのコンパイルを使用しています。 サイド バイ サイドで実行、OBJECTIVE-C ランタイム。 両方のランタイム環境では、UNIX ライクなカーネル、XNU 具体的には、上で動作し、基になるネイティブまたはマネージ システムにアクセスでき、ユーザー コードにさまざまな Api を公開します。

次の図は、このアーキテクチャの基本的な概要を示します。

[![アーキテクチャの基本的な概要を示す図](architecture-images/mac-arch.png "アーキテクチャの基本的な概要を示す図")](architecture-images/mac-arch-large.png#lightbox)

### <a name="native-and-managed-code"></a>ネイティブおよびマネージ コード

Xamarin を開発するときに、用語*ネイティブ*と*管理*コードはよく使用されます。 マネージ コードが .NET Framework 共通言語ランタイム、または Xamarin のケースで管理の実行には: Mono ランタイム。

ネイティブ コードは、(たとえば、OBJECTIVE-C または、ARM チップでのも AOT コンパイル コード) は、特定のプラットフォームでネイティブに実行されるコードです。 このガイドは、マネージ コードが、ネイティブ コードにコンパイルされ、Xamarin.Mac アプリケーションの動作方法について説明しますもアクセスすることができますが、バインドを使用して Apple の Mac Api の使用について説明します。NET の BCL となどの高度な言語C#します。

## <a name="requirements"></a>必要条件

Xamarin.Mac を使って macOS アプリケーションを開発するには、以下のものが必要です。

- Mac の実行中 macOS Sierra (10.12) 以降。
- 最新バージョンの Xcode (からインストールされている、 [App Store](https://itunes.apple.com/us/app/xcode/id497799835?mt=12))
- Xamarin.Mac と Visual Studio for Mac の最新バージョン

Xamarin.Mac で作成された Mac アプリケーションを実行するには、次のシステム要件があります。

- X 10.7 以上を Mac OS を実行している Mac。

## <a name="compilation"></a>コンパイル

Mono、Xamarin プラットフォーム アプリケーションをコンパイルするときにC#(またはF#) コンパイラが実行され、コンパイルは、C#とF#に Microsoft Intermediate Language (MSIL または IL) コード。 Xamarin.Mac を使用して、 *Just in Time (JIT)* 時、必要に応じて、適切なアーキテクチャでの実行を許可する、ネイティブ コードをコンパイルするのにはコンパイラ。

これは、AOT コンパイルを使用する Xamarin.iOS とは対照的です。 AOT コンパイラを使用する場合、すべてのアセンブリとそれらに含まれるすべてのメソッドは、ビルド時にコンパイルされます。 、JIT でコンパイルにのみ実行されるメソッドの要求時に発生します。

Mono は通常、アプリ バンドルに埋め込ま、Xamarin.Mac アプリケーションで (と呼ばれます**埋め込み Mono**)。 クラシックの Xamarin.Mac API を使用する場合、アプリケーションを使用して、でした代わりに**システム Mono**、ただし、これではサポートされず、Unified API。 システムの Mono は、オペレーティング システムにインストールされている Mono を指します。 アプリケーションの起動時には、Xamarin.Mac アプリはこれで使用されます。

## <a name="selectors"></a>セレクター

Xamarin では、.NET と Apple、させる必要がある 2 つの独立したエコシステムがある最終目標は、スムーズなユーザー エクスペリエンスであることを確認することとして、合理的なようにします。 2 つのランタイムが通信する方法、上のセクションで説明した、非常にうまくの用語 'バインド' により、Xamarin で使用されるネイティブの Mac Api 聞いたかもしれませんが。 バインドがで詳しく説明した、 [OBJECTIVE-C バインディング ドキュメント](~/mac/platform/binding.md)ここでは、ため、内部的には、Xamarin.Mac のしくみを見てみましょう。

最初に、OBJECTIVE-C を公開する方法がありますがC#、これは、セレクターを使用して行われます。 セレクターは、オブジェクトまたはクラスに送信されるメッセージです。 Objective C を使用してこれには、 [objc_msgSend](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ObjCRuntimeRef/index.html)関数。 セレクターの使用に関する詳細については、iOS を参照してください[OBJECTIVE-C セレクター](~/ios/internals/objective-c-selectors.md)ガイド。 Objective C をマネージ コードについて何も知らないという事実が原因でより複雑なある OBJECTIVE-C では、マネージ コードを公開する方法もありますがあります。 これを回避するには、使用して、[レジストラー](~/mac/internals/registrar.md)します。 これは、次のセクションで詳しく説明します。

## <a name="registrar"></a>レジストラー

レジストラーが OBJECTIVE-C にその公開のマネージ コードのコードで前述したように、 これは、NSObject から派生したすべてのマネージ クラスのリストを作成します。

- Objective C のメンバーを持つすべての管理対象メンバーのミラーリングと新しい Objective C クラスを作成、既存の OBJECTIVE-C クラスがラップされていないすべてのクラスを`[Export]`属性。
- – Objective C の各メンバーの実装では、ミラー化された管理対象のメンバーを呼び出す、コードが自動的に追加します。

次の擬似コードでは、これを行う方法の例を示します。

**C#(マネージ コード):**

```csharp
class MyViewController : UIViewController{
    [Export ("myFunc")]
    public void MyFunc ()
    {
    }
 }
 ```

**Objective C (ネイティブ コード):**

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

マネージ コードは、属性を含めることができます`[Register]`と`[Export]`レジストラーを使用して、オブジェクトを OBJECTIVE-C に公開される必要があることを確認するには [レジスタ] 属性は、生成された既定の名前が適していない場合に生成された OBJECTIVE-C クラスの名前を指定に使用されます。 NSObject から派生したすべてのクラスは、OBJECTIVE-C と自動的に登録します。 [エクスポート] の必須の属性には、文字列で、生成された OBJECTIVE-C のクラスで使用されるセレクターが含まれています。

動的および静的 – Xamarin.Mac で使用されているレジストラーの 2 種類あります。

- 動的なレジストラー – これは、すべての Xamarin.Mac ビルドの既定のレジストラーです。 動的なレジストラーでは、実行時に、アセンブリ内のすべての種類の登録を行います。 この機能を使用するには、OBJECTIVE-C のランタイム API によって提供される関数を使用します。 したがって、動的なレジストラーには、低速スタートアップより高速なが、ビルド時間があります。 領域と呼ばれます (通常は C) でネイティブ関数は、動的なレジストラーを使用する場合、メソッドの実装として使用されます。 これらは、さまざまなアーキテクチャとは異なります。
- 静的レジストラー – 静的レジストラーでは、スタティック ライブラリにコンパイルされ、実行可能ファイルにリンクされていると、ビルド中に OBJECTIVE-C コードを生成します。 これにより、高速スタートアップが、ビルド時に時間がかかります。

## <a name="application-launch"></a>アプリケーションの起動

Xamarin.Mac のスタートアップ ロジックは埋め込まれているかどうかに応じて異なります。 またはシステム Mono が使用されます。 コードと Xamarin.Mac アプリケーションの起動の手順を表示するを参照してください、[起動ヘッダー](https://github.com/xamarin/xamarin-macios/blob/master/runtime/xamarin/launch.h) xamarin macios パブリック リポジトリ内のファイル。

## <a name="generator"></a>ジェネレーター

Xamarin.Mac には、Mac のすべての API の定義が含まれています。 これらのいずれかを参照できます、 [MaciOS github リポジトリ](https://github.com/xamarin/xamarin-macios/tree/master/src)します。 これらの定義には、インターフェイス、属性を持つだけでなく、必要なメソッドとプロパティが含まれます。 NSBox を定義する次のコードを使用するなど、 [AppKit 名前空間](https://github.com/xamarin/xamarin-macios/blob/master/src/appkit.cs#L1465-L1526)します。 メソッドとプロパティの数がインターフェイスであることを確認します。

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

ジェネレーターと呼ばれる`bmac`Xamarin.Mac でこれらの定義ファイルを受け取り、一時アセンブリにコンパイルする .NET ツールを使用します。 ただし、この一時アセンブリは Objective C コードを呼び出すために有効ではありません。 ジェネレーターは、し、一時アセンブリの読み取りを生成C#実行時に使用できるコード。 これは、理由など、定義の .cs ファイルにランダムな属性を追加する場合に表示されませんで出力されるコード。 コード ジェネレーターは、それについて認識しないため`bmac`一時アセンブリに出力することで探すように認識しません。

Xamarin.Mac.dll が作成されたら、パッケー ジャー、 `mmp`、すべてのコンポーネントを一緒にバンドルされます。

大まかに言えば、これを行う、次のタスクを実行しています。

- アプリ バンドルの構造を作成します。
- マネージ アセンブリをコピーします。
- リンクが有効になっている場合は、使用されていない部分を削除することによって、アセンブリを最適化するためにマネージ リンカーを実行します。
- 静的モードの場合、レジストラー コードと共に話題ランチャー コード リンク、ランチャー アプリケーションを作成します。

これは、ユーザーの一部として実行 が参照する Xamarin.Mac.dll と実行、ユーザー コードをアセンブリにコンパイルするプロセスを構築し、`mmp`パッケージにするには

詳細については、リンカーとその使用方法についてを参照してください、iOS[リンカー](~/ios/deploy-test/linker.md)ガイド。

## <a name="summary"></a>まとめ

このガイドは、Xamarin.Mac アプリ、および探索 Xamarin.Mac OBJECTIVE-C との関係のコンパイル時参照
