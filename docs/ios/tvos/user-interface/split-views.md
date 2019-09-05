---
title: Xamarin での tvOS Split ビューコントローラーの使用
description: このドキュメントでは、Xamarin でビルドされたアプリで tvOS split ビューを操作する方法について説明します。 ここでは、分割ビューコントローラーの概要、ストーリーボードでの使用方法、マスタービューと詳細ビューへのアクセス方法、マスタービューの表示と非表示の方法について説明します。
ms.prod: xamarin
ms.assetid: 21248CFB-5A94-4C19-B223-C72E0DC5F1D5
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/16/2017
ms.openlocfilehash: 9f770eaf3fcb68c17a7692e5b6433081234951e6
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70286377"
---
# <a name="working-with-tvos-split-view-controllers-in-xamarin"></a>Xamarin での tvOS Split ビューコントローラーの使用

分割ビューコントローラーは、マスタービューコントローラーと詳細ビューコントローラーを同時に画面上に並べて表示し、管理します。 分割ビューコントローラーを使用すると、マスタービュー (左側の小さなセクション) と詳細ビュー (右側の大きなセクション) に、フォーカスがある永続的なコンテンツが表示されます。

[![](split-views-images/intro01.png "分割ビューのサンプル")](split-views-images/intro01.png#lightbox)

<a name="About-Split-View-Controllers" />

## <a name="about-split-view-controllers"></a>分割ビューコントローラーについて

前に説明したように、分割ビューコントローラーは、サイドバイサイドで表示されるマスターと詳細ビューコントローラーを管理します。マスターは左側の小さなビューで、右側の詳細が表示されます。 

また、マスタービューコントローラーは、必要に応じて非表示にしたり表示したりすることができます。 

[![](split-views-images/intro02.png "マスタービューコントローラーの非表示")](split-views-images/intro02.png#lightbox)

分割ビューコントローラーは、フィルター可能なコンテンツの一覧を表示するために使用されることがよくあります。これには、マスタービューのカテゴリと詳細ビューのフィルター処理された結果が含まれます。 これは通常、左側にテーブルビューとして表示され、右側には[コレクションビュー](~/ios/tvos/user-interface/collection-views.md)として表示されます。

分割ビューコントローラーを必要とするユーザーインターフェイスを設計する場合、Apple では、変更されないマスターおよび詳細ビューコントローラーの使用が提案されます (構造体ではなくコンテンツのみが変更されます)。 ビューコントローラーをスワップアウトする必要がある場合は、(Master または Detail) を変更する必要があるビューコントローラーのベースとして、ナビゲーションコントローラーを使用することをお勧めします。

Apple では、分割ビューコントローラーの操作に関して次のような推奨事項があります。

- **適切な分割率を使用する**-既定では、分割ビューコントローラーは、マスタービューコントローラーの場合は画面の1/3、詳細ビューコントローラーの場合は 2 ~ 3 を使用します。 必要に応じて、50/50 split を使用できます。 コンテンツを画面に均等に表示するための適切なパーセンテージを選択します。
- **メインの選択範囲を保持**します。詳細ビューの内容は、マスタービューでのユーザーの選択に応じて変更できますが、マスタービューコンテンツは修正する必要があります。 また、マスタービューで現在選択されている項目を明確に表示する必要があります。
- **1 つの統合されたタイトルを使用**します。通常は、詳細ビューとマスタービューの両方でタイトルを使用するのではなく、1つの中央のタイトルを詳細ビューで使用します。

<a name="Split-View-Controllers-and-Storyboards" />

## <a name="split-view-controllers-and-storyboards"></a>ビューコントローラーとストーリーボードの分割

TvOS アプリで分割ビューコントローラーを操作する最も簡単な方法は、iOS デザイナーを使用してアプリの UI に追加することです。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. **Solution Pad**で、 `Main.storyboard`ファイルをダブルクリックして開き、編集します。
1. **分割ビューコントローラー**を**ツールボックス**からドラッグし、ビューにドロップします。 

    [![](split-views-images/activity01.png "分割ビューコントローラー")](split-views-images/activity01.png#lightbox)
1. 既定では、iOS Designer は、マスタービューにナビゲーションコントローラーとビューコントローラーをインストールします。 これがアプリの要件に合わない場合は、単純に削除します。
1. 既定のマスタービューを削除する場合は、新しいビューコントローラーをデザイン画面にドラッグします。 

    [![](split-views-images/activity02.png "ビューコントローラー")](split-views-images/activity02.png#lightbox)
1. コントロールをクリックし、分割ビューコントローラーから新しいマスタービューコントローラーにドラッグします。 
1. **ポップアップメニュー**から **[Master]** を選択します。 

    [![](split-views-images/activity03.png "ポップアップメニューから [Master] を選択します。")](split-views-images/activity03.png#lightbox)
1. マスタービューと詳細ビューの内容をデザインします。 

    [![](split-views-images/activity04.png "レイアウトの例")](split-views-images/activity04.png#lightbox)
1. C#コード内で UI コントロールを操作するには、 **Properties Pad**の [**ウィジェット] タブ**で**名前**を割り当てます。
1. 変更内容を保存し、Visual Studio for Mac に戻ります。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. **ソリューションエクスプローラー**で、 `Main.storyboard`ファイルをダブルクリックして開き、編集します。
1. **分割ビューコントローラー**を**ツールボックス**からドラッグし、ビューにドロップします。 

    [![](split-views-images/activity01-vs.png "分割ビューコントローラー")](split-views-images/activity01-vs.png#lightbox)
1. 既定では、iOS Designer は、マスタービューにナビゲーションコントローラーとビューコントローラーを追加します。 これがアプリの要件に合わない場合は、単純に削除します。
1. 既定のマスタービューを削除する場合は、新しいビューコントローラーをデザイン画面にドラッグします。 

    [![](split-views-images/activity02-vs.png "ビューコントローラー")](split-views-images/activity02-vs.png#lightbox)
1. コントロールをクリックし、分割ビューコントローラーから新しいマスタービューコントローラーにドラッグします。 
1. **ポップアップメニュー**から **[Master]** を選択します。 

    [![](split-views-images/activity03-vs.png "ポップアップメニューから [Master] を選択します。")](split-views-images/activity03-vs.png#lightbox)
1. マスタービューと詳細ビューの内容をデザインします。 

    [![](split-views-images/activity04.png "コンテンツのレイアウト")](split-views-images/activity04.png#lightbox)
1. コードC#内で UI コントロールを操作するには、**プロパティエクスプローラー**の [**ウィジェット] タブ**で**名前**を割り当てます。
1. 変更内容を保存します。

-----

ストーリーボードの操作の詳細については、「 [Hello, tvOS クイックスタートガイド](~/ios/tvos/get-started/hello-tvos.md)」を参照してください。

<a name="Working-with-Split-View-Controllers" />

## <a name="working-with-split-view-controllers"></a>分割ビューコントローラーの操作

既に説明したように、分割ビューコントローラーは、フィルター処理されたコンテンツをユーザーに表示する場合によく使用されます。 メインカテゴリは、マスタービューの左側に表示され、ユーザーの選択に基づいて詳細ビューの右側にフィルター処理された結果が表示されます。

<a name="Accessing-Master-and-Detail" />

### <a name="accessing-master-and-detail"></a>マスターと詳細へのアクセス

マスタービューコントローラーと詳細ビューコントローラーにプログラムでアクセスする必要がある`ViewControllers`場合は、分割ビューコントローラーのプロパティを使用します。 例えば:

```csharp
// Gain access to master and detail view controllers
var masterController = ViewControllers [0] as MasterViewController;
var detailController = ViewControllers [1] as DetailViewController;
```

これは配列として表示されます。この場合、マスタービューコントローラーの最初の要素 (0) と2番目の要素 (1) が詳細になります。

<a name="Accessing-Detail-from-Master" />

### <a name="accessing-detail-from-master"></a>マスターからの詳細情報へのアクセス

通常、マスターでのユーザーの選択に基づいて詳細情報が詳細ビューに表示されるため、マスターから詳細にアクセスする方法が必要になります。

これを行う最も簡単な方法は、マスタービューコントローラークラスでプロパティを公開することです。次に例を示します。

```csharp
public DetailViewController DetailController { get; set;}
```

分割ビューコントローラーで、 `ViewDidLoad`メソッドをオーバーライドし、2つのビューを連結します。 例えば:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Gain access to master and detail view controllers
    var masterController = ViewControllers [0] as MasterViewController;
    var detailController = ViewControllers [1] as DetailViewController;

    // Wire-up views
    masterController.SplitViewController = this;
    masterController.DetailController = detailController;
    detailController.SplitViewController = this;
}
```

マスターが必要に応じて新しいデータを提示するために使用できる、詳細ビューコントローラーのプロパティおよびメソッドを公開できます。

<a name="Showing-and-Hiding-Master" />

### <a name="showing-and-hiding-master"></a>マスターの表示と非表示

必要に応じて、分割ビューコントローラーの`PreferredDisplayMode`プロパティを使用して、マスタービューコントローラーの表示と非表示を切り替えることができます。 例えば:

```csharp
// Show hide split view
if (SplitViewController.DisplayMode == UISplitViewControllerDisplayMode.PrimaryHidden) {
    SplitViewController.PreferredDisplayMode = UISplitViewControllerDisplayMode.AllVisible;
} else {
    SplitViewController.PreferredDisplayMode = UISplitViewControllerDisplayMode.PrimaryHidden;
}
```

Enum `UISplitViewControllerDisplayMode`は、次のいずれかとしてマスタービューコントローラーを表示する方法を定義します。

- TvOS は、マスタービューと詳細ビューの表示を制御します。
- **Primaryhidden** -マスタービューコントローラーを非表示にします。
- **Allvisible** -マスタービューコントローラーと詳細ビューコントローラーの両方を横に並べて表示します。 これは通常の既定のプレゼンテーションです。
- **Primaryoverlay** -詳細ビューコントローラーはの下で拡張され、マスターによってカバーされます。

現在のプレゼンテーション状態を取得するには`DisplayMode` 、分割ビューコントローラーのプロパティを使用します。

<a name="Summary" />

## <a name="summary"></a>Summary

この記事では、tvOS アプリ内の分割ビューコントローラーの設計と操作について説明しました。



## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマンインターフェイスガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS のアプリプログラミングガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
