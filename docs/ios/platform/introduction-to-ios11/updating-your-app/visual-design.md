---
title: IOS 11 でビジュアル デザインの更新
description: このドキュメントでは、更新プログラムは、iOS 11 で導入されたビジュアル デザインについて説明します。 これには、ナビゲーション バー、コント ローラーの検索、余白、全画面表示のコンテンツ、およびテーブル ビューへの変更について説明します。
ms.prod: xamarin
ms.assetid: 7C300B94-0FAF-492E-A326-877419A1824B
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/13/2016
ms.openlocfilehash: c6351f2c25f8e31181c761aea1b471315a8a05e8
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50114517"
---
# <a name="visual-design-updates-in-ios-11"></a>IOS 11 でビジュアル デザインの更新

Ios 11 では、Apple は、ナビゲーション バー、検索バー、およびテーブル ビューの更新を含む新しいビジュアルの変更を導入しました。 さらに柔軟性を高めるための余白と全画面表示のコンテンツを許可する改良が加えられてがいます。 このガイドでは、これらの変更がについて説明します。 

Iphone X 向けの設計について具体的には、Apple をご覧ください。 [iphone X 向けの設計](https://developer.apple.com/videos/play/fall2017/801/)ビデオ。

## <a name="uikit"></a>UIKit

IOS エンドユーザーのアクセスできるようにする 11 に適応させる UIKit バー。

このような 1 つの変更はときに、ユーザーの時間の長いのバーに表示される新しい HUD 表示項目です。 これを有効にするには設定、`largeContentSizeImage`プロパティ`UIBarItem`を追加すると拡大画像を使用して、[資産カタログ](~/ios/app-fundamentals/images-icons/displaying-an-image.md):

```csharp
barItem.LargeContentSizeImage = UIImage.FromBundle("AccessibleImage");
```

### <a name="navigation-bar"></a>ナビゲーション バー
iOS 11 では、ナビゲーション バーのタイトルを読みやすくするために新しい機能が導入されました。 アプリを割り当てることによってこの大規模なタイトルを表示できる、`PrefersLargeTitles`プロパティを true にします。

```csharp
NavigationController.NavigationBar.PrefersLargeTitles = true;
```

アプリでより大きなタイトルの設定は、_すべて_次のスクリーン ショットに示すようより大きなアプリ間でのナビゲーション バーのタイトルが表示されます。

![大規模なナビゲーションのタイトル](visual-design-images/image7.png)

ナビゲーション バーで、大規模なタイトルが表示される場合を制御する設定、`LargeTitleDisplayMode`にナビゲーション項目を`Always`、 `Never`、または`Automatic`します。

### <a name="search-controller"></a>コント ローラーの検索

iOS 11 がやすく、ナビゲーション バーに直接検索コント ローラーを追加します。 検索のコント ローラーを作成すると、ナビゲーション バーに追加、`SearchController`プロパティ。

```csharp
NavigationItem.SearchController = searchController;
```

[![検索バーのタイトルが大規模なナビゲーション](visual-design-images/image8-sml.png)](visual-design-images/image8-sml.png#lightbox)

アプリの機能によっては、ユーザーがリストをスクロールするときに非表示にする、検索バーを望まない場合があります。 使用しても変更できます、`HidesSearchBarWhenScrolling`プロパティ。

## <a name="margins"></a>余白

Apple は – 新しいプロパティを作成しました。 `directionalLayoutMargins` – を使用してをビューとサブビュー間のスペースを設定できます。 使用`directionalLayoutMargins`で`leading`または`trailing`インセットします。 かどうか、システムが左から右または右から左の言語の場合に関係なく、アプリでの間隔は iOS によって適切に設定します。

IOS 10 とする前に、すべてのビューには、整列する余白の最小サイズが必要があります。 iOS 11 を使用してオーバーライドするオプションの導入`ViewRespectsSystemMinimumLayoutMargins`します。 たとえば、このプロパティを false に設定することができますを 0 に、edge くぼみを調整します。

```csharp
ViewRespectsSystemMinimumLayoutMargins = false;
View.LayoutMargins = UIEdgeInsets.Zero;
```
![Ios 11 では、ゼロ埋め込みイメージが表示された余白](visual-design-images/image9.png)

<a name="fullscreen" />

## <a name="full-screen-content"></a>全画面表示のコンテンツ

iOS 7[導入](~/ios/platform/introduction-to-ios7/ios7-ui.md#fullscreen)`topLayoutGuide`と`bottomLayoutGuide`は UIKit バーを非表示して、画面の表示領域では、ビューの制約の手段として。 IOS 11 の時に優先でこれらが削除されて、_安全領域_します。

安全領域は、アプリケーションと、ビューとスーパー ビューの間の制約を追加する方法の表示スペースについての考え方の新しい方法です。 たとえば、次の図があるとします。

[![安全領域 vs 上部と下部にあるレイアウト ガイド](visual-design-images/image10-sml.png)](visual-design-images/image10.png#lightbox)

以前は、ビューの追加が必要な上記の緑色の領域に表示される場合は制限することを_下部_の`TopLayoutGuide`と_上部_の`BottomLayoutGuide`します。 Ios 11 では代わりに制限することを_上部_と_下部_安全領域の。 以下に例を示します。

```csharp
var safeGuide = View.SafeAreaLayoutGuide;
imageView.TopAnchor.ConstraintEqualTo(safeGuide.TopAnchor).Active = true;
safeGuide.BottomAnchor.ConstraintEqualTo(imageView.BottomAnchor).Active = true;
```

## <a name="table-view"></a>テーブル ビュー

UITableView iOS 11 で、多数の小さいけれども重要な変更をしました。

既定では、ヘッダー、フッター、およびセルは今すぐ自動的にサイズ コンテンツに基づいてなります。 この自動サイズ調整動作のセットをオプトアウトする、 `EstimatedRowHeight`、 `EstimatedSectionHeaderHeight`、または`EstimatedSectionFooterHeight`をゼロにします。

ただし、一部の状況で (場合など既存 Storboards Interface Builder を使用する場合、または iOS Designer で UITableViewController を追加) 自己サイズ変更のセルを手動で有効にする必要があります。 これを行うには、設定することが、次のプロパティのセルやヘッダー、フッター、テーブル ビューでそれぞれを確認します。

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

iOS 11 では、行のアクションの機能を拡張します。 `UISwipeActionsConfiguration` ユーザー テーブル ビュー内の行のいずれかの方向にスワイプするときに実行される操作のセットを定義するが導入されました。 この動作は、ネイティブ Mail.app に似ています。 詳細については、次を参照してください。、[行アクション](~/ios/user-interface/controls/tables/row-action.md)ガイド。

テーブル ビューでは、iOS 11 でドラッグ アンド ドロップのサポートがあります。 詳細については、次を参照してください。、[ドラッグ アンド ドロップ](~/ios/platform/introduction-to-ios11/drag-and-drop.md#uitableview)ガイド。


## <a name="related-links"></a>関連リンク

- [IOS 11 (Apple) で新します。](https://developer.apple.com/ios/)
- [アプリ ストアの更新された製品のページ (Apple)](https://developer.apple.com/app-store/product-page/)
- [Iphone X 向けの設計 (Apple) (ビデオ)](https://developer.apple.com/videos/play/fall2017/801/)
- [IOS 11 (WWDC) (ビデオ) 用のアプリの更新](https://developer.apple.com/videos/play/wwdc2017/204/)

