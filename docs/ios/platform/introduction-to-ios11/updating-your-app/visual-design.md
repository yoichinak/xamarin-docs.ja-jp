---
title: IOS 11 でのビジュアルデザインの更新
description: このドキュメントでは、iOS 11 で導入されたビジュアルデザインの更新について説明します。 ナビゲーションバー、検索コントローラー、余白、全画面コンテンツ、およびテーブルビューの変更について説明します。
ms.prod: xamarin
ms.assetid: 7C300B94-0FAF-492E-A326-877419A1824B
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/13/2016
ms.openlocfilehash: e5a61af4cd8a09df3ffddb74658f646aa8edfa1f
ms.sourcegitcommit: ce4670de51e24116a944c778ee64585bd0aae0e1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/11/2020
ms.locfileid: "79088971"
---
# <a name="visual-design-updates-in-ios-11"></a>IOS 11 でのビジュアルデザインの更新

IOS 11 では、ナビゲーションバー、検索バー、テーブルビューの更新など、新しいビジュアルの変更が導入されました。 さらに、余白と全画面コンテンツの柔軟性を高めることができるように強化されました。 これらの変更については、このガイドで説明します。 

IPhone X 向けの設計に関する詳細については、「 [Iphone x 用](https://developer.apple.com/videos/play/fall2017/801/)の Apple の設計」ビデオをご覧ください。

## <a name="uikit"></a>UIKit

UIKit バーは、iOS 11 では、エンドユーザーがより使いやすくなるように変更されています。

このような変更の1つは、ユーザーがバー項目を長時間押したときに表示される新しい HUD ディスプレイです。 これを有効にするには、`UIBarItem` の [`largeContentSizeImage`] プロパティを設定し、[アセットカタログ](~/ios/app-fundamentals/images-icons/displaying-an-image.md)を使用してより大きなイメージを追加します。

```csharp
barItem.LargeContentSizeImage = UIImage.FromBundle("AccessibleImage");
```

### <a name="navigation-bar"></a>ナビゲーション バー
iOS 11 では、ナビゲーションバーのタイトルを読みやすくするための新機能が導入されました。 アプリでは、`PrefersLargeTitles` プロパティを true に割り当てることで、この大きなタイトルを表示できます。

```csharp
NavigationController.NavigationBar.PrefersLargeTitles = true;
```

アプリで大きなタイトルを設定すると、次のスクリーンショットに示すように、アプリ全体の_すべて_のナビゲーションバータイトルが大きく表示されます。

![大きなナビゲーションタイトル](visual-design-images/image7.png)

ナビゲーションバーに大きなタイトルが表示されるタイミングを制御するには、ナビゲーション項目の `LargeTitleDisplayMode` を `Always`、`Never`、または `Automatic`に設定します。

### <a name="search-controller"></a>検索コントローラー

iOS 11 では、検索コントローラーを簡単にナビゲーションバーに追加できました。 検索コントローラーを作成したら、`SearchController` プロパティを使用してナビゲーションバーに追加します。

```csharp
NavigationItem.SearchController = searchController;
```

[検索バーを使用して大きなナビゲーションタイトルを ![する](visual-design-images/image8-sml.png)](visual-design-images/image8-sml.png#lightbox)

アプリの機能によっては、ユーザーがリストをスクロールしたときに検索バーを非表示にすることがあります。 これは、`HidesSearchBarWhenScrolling` プロパティを使用して調整できます。

## <a name="margins"></a>余白

Apple では、ビューとサブビューの間隔を設定するために使用できる新しいプロパティ `directionalLayoutMargins` を作成しました。 `leading` または `trailing` インセットで `directionalLayoutMargins` を使用します。 システムが左から右または右から左のどちらの言語であるかにかかわらず、アプリの間隔は iOS によって適切に設定されます。

IOS 10 以前では、すべてのビューの余白の最小サイズが調整されていました。 iOS 11 では、`ViewRespectsSystemMinimumLayoutMargins`を使用して上書きするオプションが導入されました。 たとえば、このプロパティを false に設定すると、エッジのインセットをゼロに調整できます。

```csharp
ViewRespectsSystemMinimumLayoutMargins = false;
View.LayoutMargins = UIEdgeInsets.Zero;
```

![Ios 11 で余白がゼロの余白を示す画像](visual-design-images/image9.png)

<a name="fullscreen" />

## <a name="full-screen-content"></a>全画面コンテンツ

iOS 7 では、ビューを制限する方法として `topLayoutGuide` および `bottomLayoutGuide` が[導入](~/ios/platform/introduction-to-ios7/ios7-ui.md#fullscreen)されました。これにより、ビューは、uikit バーによって非表示にされ、画面の表示領域に表示されます。 これらは、iOS 11 では_安全な領域_を優先して非推奨とされています。

セーフ領域は、アプリケーションの表示領域と、ビューとスーパービューの間に制約がどのように追加されるかを検討する新しい方法です。 たとえば、次の図を考えてみます。

[![セーフエリア vs 上および下部レイアウトガイド](visual-design-images/image10-sml.png)](visual-design-images/image10.png#lightbox)

以前は、ビューを追加し、それを上の緑の領域に表示する必要がある場合は、それを `TopLayoutGuide` の_下部_と `BottomLayoutGuide`の_一番上_に固定します。 IOS 11 では、その代わりに、安全領域の_一番上_と_一番下_に固定します。 例を次に示します。

```csharp
var safeGuide = View.SafeAreaLayoutGuide;
imageView.TopAnchor.ConstraintEqualTo(safeGuide.TopAnchor).Active = true;
safeGuide.BottomAnchor.ConstraintEqualTo(imageView.BottomAnchor).Active = true;
```

## <a name="table-view"></a>テーブルビュー

UITableView には、iOS 11 では多数の小さな変更が加えられています。

既定では、ヘッダー、フッター、およびセルのサイズは、コンテンツに基づいて自動的に調整されます。 この自動サイズ変更動作を無効にするには、`EstimatedRowHeight`、`EstimatedSectionHeaderHeight`、または `EstimatedSectionFooterHeight` を0に設定します。

ただし、状況によっては (iOS デザイナーで UITableViewController を追加する場合や、Interface Builder の既存の Storboards を使用する場合など)、手動でサイズ変更するセルを手動で有効にすることが必要になる場合があります。 これを行うには、セル、ヘッダー、およびフッターのテーブルビューで、次のプロパティがそれぞれ設定されていることを確認します。

```csharp
// Cells
TableView.RowHeight = UITableView.AutomaticDimension;
TableView.EstimatedRowHeight = UITableView.AutomaticDimension;

// Header
TableView.SectionHeaderHeight = UITableView.AutomaticDimension;
TableView.EstimatedSectionHeaderHeight = 40f;

//Footer
TableView.SectionFooterHeight = UITableView.AutomaticDimension;
TableView.EstimatedSectionFooterHeight = 40f;

```

iOS 11 では、行アクションの機能が拡張されています。 `UISwipeActionsConfiguration` は、ユーザーがテーブルビューの行に対していずれかの方向でスワイプしたときに実行する一連のアクションを定義するために導入されました。 この動作は、ネイティブの電子メールアプリの動作に似ています。 詳細については、「[行アクション](~/ios/user-interface/controls/tables/row-action.md)ガイド」を参照してください。

テーブルビューでは、iOS 11 でのドラッグアンドドロップがサポートされています。 詳細については、「[ドラッグアンドドロップ](~/ios/platform/introduction-to-ios11/drag-and-drop.md#uitableview)ガイド」を参照してください。

## <a name="related-links"></a>関連リンク

- [IOS 11 (Apple) の新機能](https://developer.apple.com/ios/)
- [更新された App Store 製品ページ (Apple)](https://developer.apple.com/app-store/product-page/)
- [IPhone X (Apple) 用の設計 (ビデオ)](https://developer.apple.com/videos/play/fall2017/801/)
- [IOS 11 (WWDC) 用のアプリの更新 (ビデオ)](https://developer.apple.com/videos/play/wwdc2017/204/)
