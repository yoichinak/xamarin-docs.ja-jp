---
title: "ビジュアル デ ザインの更新プログラム"
description: "IOS 11 の新機能の検証"
ms.topic: article
ms.prod: xamarin
ms.assetid: 7C300B94-0FAF-492E-A326-877419A1824B
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/13/2016
ms.openlocfilehash: d66d8cd722aa9a7b6fe27db3f6128ee24309a1de
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="visual-design-updates"></a>ビジュアル デ ザインの更新プログラム

_IOS 11 の新機能の検証_

11、iOS では、Apple には、ナビゲーション バー、検索バー、およびテーブル ビューの更新を含む新しいビジュアルの変更が導入されました。 さらに機能強化に加えられた柔軟性を高めるための余白と全画面表示のコンテンツを許可します。 このガイドでは、これらの変更について説明します。

## <a name="uikit"></a>UIKit

IOS アクセスできるようにするよりエンドユーザー 11 で適応させる UIKit バー。

このような 1 つの変更は新しい HUD 表示バーのユーザー時間の長い-押すときに表示される項目。 これを有効にするには設定、`largeContentSizeImage`プロパティを`UIBarItem`経由で大きいイメージを追加し、[アセット カタログ](~/ios/app-fundamentals/images-icons/displaying-an-image.md):

```csharp
barItem.LargeContentSizeImage = UIImage.FromBundle("AccessibleImage");
```

### <a name="navigation-bar"></a>ナビゲーション バー
iOS 11 では、ナビゲーション バーのタイトルを読みやすくするための新機能が導入されました。 アプリは割り当てることによってこの大きいタイトルを表示することができます、`PrefersLargeTitles`プロパティを true にします。

```csharp
NavigationController.NavigationBar.PrefersLargeTitles = true;
```

アプリの設定より大きなタイトルは、_すべて_次のスクリーン ショットに示すようより大きなアプリ間でのナビゲーション バーのタイトルが表示されます。

![大規模なナビゲーション タイトル](visual-design-images/image7.png)

ナビゲーション バーで、大きいタイトルが表示される場合を制御する設定、`LargeTitleDisplayMode`するナビゲーション アイテム`Always`、 `Never`、または`Automatic`です。

### <a name="search-controller"></a>コント ローラーの検索

iOS 11 は、容易になります、ナビゲーション バーに直接検索コント ローラーを追加します。 検索のコント ローラーを作成した後、ナビゲーション バーに追加、`SearchController`プロパティ。

```csharp
NavigationItem.SearchController = searchController;
```

[![検索バーのタイトルが大規模なナビゲーション](visual-design-images/image8-sml.png)](visual-design-images/image8-sml.png#lightbox)

アプリの機能、によっては、ユーザーの一覧がスクロールされたときに非表示にするには、検索バーは好ましくありません。 使用しても変更できます、`HidesSearchBarWhenScrolling`プロパティです。

## <a name="margins"></a>余白

Apple の – 新しいプロパティが作成`directionalLayoutMargins`– ビューとサブビュー間隔の設定に使用できます。 使用して`directionalLayoutMargins`で`leading`または`trailing`インセットです。 かどうか、システムが左から右へまたは右から左への言語に関係なく、アプリ内のスペースは iOS で適切に設定します。

IOS 10 および前に、すべてのビューに最小の余白サイズが、整列する必要があります。 iOS 11 を使用して上書きするオプションが導入されました。`ViewRespectsSystemMinimumLayoutMargins`です。 たとえば、このプロパティを false に設定することができますを 0 に、エッジくぼみの調整。

```csharp
ViewRespectsSystemMinimumLayoutMargins = false;
View.LayoutMargins = UIEdgeInsets.Zero;
```
![Ios 11 でゼロの埋め込みにイメージを表示の余白](visual-design-images/image9.png)

<a name="fullscreen" />

## <a name="full-screen-content"></a>全画面表示の内容

iOS 7[導入](~/ios/platform/introduction-to-ios7/ios7-ui.md#fullscreen)`topLayoutGuide`と`bottomLayoutGuide`UIKit バーを非表示には、画面の表示領域では、されるように、ビューの制約を定義する手段として。 これらの代わりに iOS 11 で廃止された、_セーフ エリア_です。

安全な領域は、アプリケーションと、ビューとスーパー ビューの間の制約を追加する方法の表示スペース考えたの新しい方法です。 たとえば、次の図があるとします。

[![セーフ エリア vs 上部と下部にあるレイアウト ガイド](visual-design-images/image10-sml.png)](visual-design-images/image10.png#lightbox)

以前は、ビューを追加が必要な上記の緑色の領域に表示する場合は制約することを_下部_の`TopLayoutGuide`と_上部_の`BottomLayoutGuide`です。 11、iOS では代わりに制約することを_上部_と_下部_セーフ エリアのです。 以下に例を示します。

```csharp
var safeGuide = View.SafeAreaLayoutGuide;
imageView.TopAnchor.ConstraintEqualTo(safeGuide.TopAnchor).Active = true;
safeGuide.BottomAnchor.ConstraintEqualTo(imageView.BottomAnchor).Active = true;
```

## <a name="table-view"></a>テーブル ビュー

UITableView を iOS 11 では、多数のサイズは小さいが、大幅な変更を持っていました。

既定では、ヘッダー、フッター、およびセルはここで自動的にサイズ コンテンツに基づいてなります。 この自動サイズ調整の動作のセットから除外する、 `EstimatedRowHeight`、 `EstimatedSectionHeaderHeight`、または`EstimatedSectionFooterHeight`をゼロにします。

ただし、いくつかの状況では、(場合など既存 Storboards をインターフェイスのビルダーを使用した場合、iOS デザイナーまたは UITableViewController の追加) を自己サイズ変更のセルを手動で有効にする必要があります。 これを行うには、設定することが、次のプロパティのセルやヘッダー、フッター、テーブル ビューでそれぞれを確認します。

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

iOS 11 が行アクションの機能を拡張します。 `UISwipeActionsConfiguration` ユーザーが、どちらの方向テーブルのビュー内の行を読み取るときに実行される操作のセットを定義するが導入されました。 この動作は、ネイティブのある Mail.app に似ています。 詳細については、次を参照してください。、[行アクション](~/ios/user-interface/controls/tables/row-action.md)ガイドです。

テーブルのビューでは、iOS 11 でドラッグ アンド ドロップのサポートがあります。 詳細については、次を参照してください。、[ドラッグ アンド ドロップ](~/ios/platform/introduction-to-ios11/drag-and-drop.md#uitableview)ガイドです。


## <a name="related-links"></a>関連リンク

- [新機能 iOS 11 (Apple)](https://developer.apple.com/ios/)
- [アプリ ストアの更新された製品ページ (Apple)](https://developer.apple.com/app-store/product-page/)
- [更新、iOS 用アプリ 11 (WWDC) (ビデオ)](https://developer.apple.com/videos/play/wwdc2017/204/)
