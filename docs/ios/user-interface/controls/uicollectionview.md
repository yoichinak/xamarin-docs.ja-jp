---
title: コレクション ビュー
description: コレクション ビューは、任意のレイアウトを使用して表示するコンテンツを許可します。 これらは、カスタム レイアウトをサポートしているときに既定でグリッドのようなレイアウトを容易に作成できるようにします。
ms.prod: xamarin
ms.assetid: F4B85F25-0CB5-4FEA-A3B5-D22FCDC81AE4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 75ad331a265c14892f101b1aa7956d2cde3beec8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="collection-views"></a>コレクション ビュー

_コレクション ビューは、任意のレイアウトを使用して表示するコンテンツを許可します。これらは、カスタム レイアウトをサポートしているときに既定でグリッドのようなレイアウトを容易に作成できるようにします。_

利用可能なコレクション ビュー、`UICollectionView`クラス、レイアウトを使用して、画面上の複数の項目の表示を紹介する iOS 6 での新しい概念です。 データを提供するためのパターン、`UICollectionView`項目を作成し、iOS 開発で使用される一般的なパターンを同じ委任とデータ ソース アイテムに従ってこれらと対話します。

ただし、コレクション ビューとは別のレイアウト サブシステムの使用、`UICollectionView`自体です。 そのため、別のレイアウトを提供するだけ簡単に変更できますコレクション ビューの表示。

iOS と呼ばれるレイアウト クラスを提供する`UICollectionViewFlowLayout`追加作業を作成するグリッドなどのレイアウトの行に基づくことができます。 また、カスタム レイアウトも作成できます想像できるとおり、プレゼンテーションを許可します。

## <a name="uicollectionview-basics"></a>UICollectionView の基礎

`UICollectionView`クラスで構成される 3 つの異なるアイテム。

-  **セル**– 項目ごとのビューのデータ ドリブン
-  **補助ビュー** – セクションに関連付けられたデータ ドリブンのビューです。
-  **装飾ビュー** – 以外のデータ駆動型のレイアウトによって作成されたビュー

## <a name="cells"></a>セル

セルは、コレクション ビューで提示されているデータ セット内で 1 つの項目を表すオブジェクトです。 各セルは、のインスタンス、`UICollectionViewCell`から構成される 3 つの異なるビューでは、次の図に示すようにクラス。

 [![](uicollectionview-images/01-uicollectionviewcell.png "次のように各セルは 3 つの異なるビューで構成されます。")](uicollectionview-images/01-uicollectionviewcell.png#lightbox)

`UICollectionViewCell`クラスにはこれらの各ビューの次のプロパティ。

-   `ContentView` このビューには、セルに表示するコンテンツが含まれています。 これは、画面の最上位の z オーダーでレンダリングされます。
-   `SelectedBackgroundView` – セルの選択のサポートが組み込まれています。 このビューは、セルが選択されていることを視覚的に表すために使用されます。 すぐ下にレンダリングされた、`ContentView`セルを選択するとします。
-   `BackgroundView` – セルは、によって提示されるバック グラウンドにも表示できます、`BackgroundView`です。 このビューが下に表示される、`SelectedBackgroundView`です。


設定して、`ContentView`よりも小さいことなど、`BackgroundView`と`SelectedBackgroundView`、`BackgroundView`視覚的に、コンテンツ、while を囲むために使用できる、`SelectedBackgroundView`が表示されます、セルを選択すると、次のように。

 [![](uicollectionview-images/02-cells.png "別のセル要素")](uicollectionview-images/02-cells.png#lightbox)

継承することで、上記のスクリーン ショットでセルが作成されます`UICollectionViewCell`と設定、 `ContentView`、`SelectedBackgroundView`と`BackgroundView`プロパティ、それぞれに、次のコードに示すようにします。

```csharp
public class AnimalCell : UICollectionViewCell
{
        UIImageView imageView;

        [Export ("initWithFrame:")]
        public AnimalCell (CGRect frame) : base (frame)
        {
            BackgroundView = new UIView{BackgroundColor = UIColor.Orange};

            SelectedBackgroundView = new UIView{BackgroundColor = UIColor.Green};

            ContentView.Layer.BorderColor = UIColor.LightGray.CGColor;
            ContentView.Layer.BorderWidth = 2.0f;
            ContentView.BackgroundColor = UIColor.White;
            ContentView.Transform = CGAffineTransform.MakeScale (0.8f, 0.8f);

            imageView = new UIImageView (UIImage.FromBundle ("placeholder.png"));
            imageView.Center = ContentView.Center;
            imageView.Transform = CGAffineTransform.MakeScale (0.7f, 0.7f);

            ContentView.AddSubview (imageView);
        }

        public UIImage Image {
            set {
                imageView.Image = value;
            }
        }
}
```

 <a name="Supplementary_Views" />


## <a name="supplementary-views"></a>補助ビュー

補助ビューは、の各セクションに関連付けられている情報を表示するビュー、`UICollectionView`です。 セルと同様には、補足のビューは、データ ドリブンです。 セルは、データ ソースの項目データを表示、場所、補助ビューは、book 本棚のカテゴリまたは音楽ライブラリ内の音楽のジャンルなどのセクションのデータを提供します。

たとえば、補助ビューは次の図に示すように、特定のセクションのヘッダーを表示する使用可能性があります。

 [![](uicollectionview-images/02a-supplementary-view.png "次のように、特定のセクションのヘッダーを表示する補助ビューが使用されます。")](uicollectionview-images/02a-supplementary-view.png#lightbox)

補助ビューを使用して、そのには、最初に登録する必要が、`ViewDidLoad`メソッド。

```csharp
CollectionView.RegisterClassForSupplementaryView (typeof(Header), UICollectionElementKindSection.Header, headerId);
```

次に、ビューを使用して返される必要があります`GetViewForSupplementaryElement`を使用して作成された、`DequeueReusableSupplementaryView`から継承して`UICollectionReusableView`です。 次のコード スニペットは、上記のスクリーン ショットに示すように SupplementaryView に生成されます。

```csharp
public override UICollectionReusableView GetViewForSupplementaryElement (UICollectionView collectionView, NSString elementKind, NSIndexPath indexPath)
        {
            var headerView = (Header)collectionView.DequeueReusableSupplementaryView (elementKind, headerId, indexPath);
            headerView.Text = "Supplementary View";
            return headerView;
        }

```

補足のビューは、ヘッダーとフッターだけより一般的です。
コレクション ビューに任意の場所に配置することができ、外観を完全にカスタマイズ可能なこと、すべてのビューで構成できます。

 <a name="Decoration_Views" />


## <a name="decoration-views"></a>装飾ビュー

装飾ビューでは単なるビジュアルに表示されることがある、`UICollectionView`です。 セルおよび補助ビューとは異なりがないデータ ドリブンします。 レイアウトのサブクラス内で常に作成し、コンテンツのレイアウトとして後で変更できます。 コンテンツをスクロールするバック グラウンド ビューを提示する装飾ビューを使用でしたなど、`UICollectionView`次のように、します。

 [![](uicollectionview-images/02c-decoration-view.png "背景が赤の装飾の表示")](uicollectionview-images/02c-decoration-view.png#lightbox)

 次のコード スニペットは、サンプルが赤に背景を変更`CircleLayout`クラス。

 ```csharp
 public class MyDecorationView : UICollectionReusableView
    {
        [Export ("initWithFrame:")]
        public MyDecorationView (CGRect frame) : base (frame)
        {
            BackgroundColor = UIColor.Red;
        }
    }
 ```


## <a name="data-source"></a>データ ソース

同様に、iOS の他の部分など`UITableView`と`MKMapView`、`UICollectionView`からデータを取得、*データ ソース*、Xamarin.iOS 経由で公開されているが、 **`UICollectionViewDataSource`**クラスです。 コンテンツを提供するため、このクラスは、`UICollectionView`など。

-  **セル**– から返された`GetCell`メソッドです。
-  **補助ビュー** – から返された`GetViewForSupplementaryElement`メソッドです。
-  **セクション数**– から返された`NumberOfSections`メソッドです。 既定値は実装されていない場合は 1 です。
-  **セクションごとの項目数**– から返された`GetItemsCount`メソッドです。

### <a name="uicollectionviewcontroller"></a>UICollectionViewController
便宜上、`UICollectionViewController`クラスが使用可能です。これは、自動的に構成されている次のセクションで説明されているが、両方のデリゲートとのデータ ソース、`UICollectionView`ビュー。

同様に`UITableView`、`UICollectionView`クラスは、画面上にある項目のセルを取得するには、そのデータ ソースのみを呼び出します。
画面の外にスクロールしたセルは、次の図に示すようにで再利用できるように、キューに保存されます。

 [![](uicollectionview-images/03-cell-reuse.png "画面の外にスクロールするセルを配置しているで再利用するためにキューに次のように")](uicollectionview-images/03-cell-reuse.png#lightbox)

セルを再利用を簡略化された`UICollectionView`と`UITableView`です。 1 つでない場合、再利用するキューで利用可能なセルがシステムに登録されているデータ ソースに直接セルを作成する必要がなくなります。 IOS キューから削除して再利用するキューからのセルへの呼び出しを行うときにセルが使用できないと、型または登録された nib に基づいて自動的にそれが作成されます。
同じ手法も補助ビューを使用できます。

たとえば、次のコードを登録する、`AnimalCell`クラス。

```csharp
static NSString animalCellId = new NSString ("AnimalCell");
CollectionView.RegisterClassForCell (typeof(AnimalCell), animalCellId);
```

ときに、`UICollectionView`その項目が画面上にあるために、セルを必要な`UICollectionView`呼び出しデータ ソースの`GetCell`メソッドです。 UITableView でこの数式の動作と同様に、このメソッドは、セルになるバックアップ データを構成するため、`AnimalCell`ここではクラスです。

次のコードの実装を示しています。`GetCell`を返す、`AnimalCell`インスタンス。

```csharp
public override UICollectionViewCell GetCell (UICollectionView collectionView, Foundation.NSIndexPath indexPath)
{
        var animalCell = (AnimalCell)collectionView.DequeueReusableCell (animalCellId, indexPath);

        var animal = animals [indexPath.Row];

        animalCell.Image = animal.Image;

        return animalCell;
}
```

呼び出し`DequeReusableCell`セルはか、除外のキューを再利用キューから、または、セルがへの呼び出しで登録された型に基づいて作成、キューで利用できない場合は、`CollectionView.RegisterClassForCell`です。

ここで、登録することによって、`AnimalCell`クラス、iOS を新規に作成されます`AnimalCell`内部的にして返すキューから削除して、セルへの呼び出しが行われたときに animal クラスに含まれているし、に表示するために返されるイメージで構成されている後`UICollectionView`.

 <a name="Delegate" />


### <a name="delegate"></a>Delegate

`UICollectionView`クラス型のデリゲートを使用して`UICollectionViewDelegate`内のコンテンツとの対話をサポートするために、`UICollectionView`です。 これによりの制御。

-  **選択範囲のセルの**– セルが選択されているかどうかを決定します。
-  **セルの強調表示**– セルが現在アクセスされているかどうかを決定します。
-  **セルのメニュー** – 長いキーを押してジェスチャに応答内のセルに対して表示されるメニュー。


データ ソースと同様、`UICollectionViewController`のデリゲートである既定の構成は、`UICollectionView`です。

 <a name="Cell_HighLighting" />


#### <a name="cell-highlighting"></a>セルが強調表示

セルが押される、セルの強調表示された状態に遷移とまで選択されていないときにユーザーがセルから指を離したです。 これにより、一時的な変更のセルの外観が実際に選択されている前にします。 選択するときに、セルの`SelectedBackgroundView`が表示されます。 次の図は、選択は、発生する前に、強調表示された状態を示しています。

 [![](uicollectionview-images/04-cell-highlight.png "選択は、発生する前に、次の図は強調表示された状態を示しています。")](uicollectionview-images/04-cell-highlight.png#lightbox)

強調表示を実装する、`ItemHighlighted`と`ItemUnhighlighted`のメソッド、`UICollectionViewDelegate`使用できます。 たとえば、次のコードがの黄色の背景色を適用、`ContentView`セルが強調表示すると、上記の図に示すように強調、されていないときに背景が白。

```csharp
public override void ItemHighlighted (UICollectionView collectionView, NSIndexPath indexPath)
{
        var cell = collectionView.CellForItem(indexPath);
        cell.ContentView.BackgroundColor = UIColor.Yellow;
}

public override void ItemUnhighlighted (UICollectionView collectionView, NSIndexPath indexPath)
{
        var cell = collectionView.CellForItem(indexPath);
        cell.ContentView.BackgroundColor = UIColor.White;
}
```

 <a name="Disabling_Selection" />


#### <a name="disabling-selection"></a>選択範囲を無効にします。

既定で選択範囲が有効になっている`UICollectionView`です。 選択範囲を無効にするオーバーライド`ShouldHighlightItem`次に示すように false を返します。

```csharp
public override bool ShouldHighlightItem (UICollectionView collectionView, NSIndexPath indexPath)
{
        return false;
}
```

強調表示が無効になっているときにも、セルを選択した場合のプロセスが無効です。 また、クライアントがも、`ShouldSelectItem`メソッドが直接、選択範囲を制御する場合は`ShouldHighlightItem`は実装されており、false を返します`ShouldSelectItem`は呼び出されません。

 `ShouldSelectItem` 有効、オンまたはオフにする項目によっては、な選択が可能と`ShouldHighlightItem`は実装されていません。 また、選択、なしの強調表示場合`ShouldHighlightItem`は実装されており、中に、true を返します`ShouldSelectItem`false を返します。

 <a name="Cell_Menus" />


#### <a name="cell-menus"></a>セルのメニュー

各セルに、`UICollectionView`切り取り、コピー、および必要に応じて、サポートされるために貼り付けを許可するメニューを表示できません。 セルに [編集] メニューを作成します。

1.  オーバーライド`ShouldShowMenu`し、項目がメニューを表示する必要がある場合は true を返します。
1.  オーバーライド`CanPerformAction`し切り取り、コピー、貼り付けのいずれかとなるアクションごとに、項目を実行できる、true を返します。
1.  オーバーライド`PerformAction`貼り付け操作のコピーを実行するには、編集します。


次のスクリーン ショットは、セルが長い押されたときに、メニューを表示します。

 [![](uicollectionview-images/04a-menu.png "このスクリーン ショットは、セルが長い押されたときにメニューを表示します。")](uicollectionview-images/04a-menu.png#lightbox)

 <a name="Layout" />


## <a name="layout"></a>レイアウト

`UICollectionView` すべての要素、独立して管理するには、セル、補助ビューおよび装飾ビューの位置をできるレイアウト システムをサポートしている、`UICollectionView`自体です。
レイアウト システムを使用して、アプリケーションはこの記事に見られたとともに、カスタム レイアウト グリッドのようなものなどのレイアウトをサポートできます。

 <a name="Layout_Basics" />


### <a name="layout-basics"></a>レイアウトの基礎

レイアウト、`UICollectionView`から継承するクラスで定義された`UICollectionViewLayout`です。 各項目のレイアウト属性を作成するため、レイアウト実装は、`UICollectionView`です。 これにはレイアウトを作成する 2 つの方法があります。

-  組み込みを使用して`UICollectionViewFlowLayout`です。
-  継承することで、カスタム レイアウトを提供`UICollectionViewLayout`です。


 <a name="Flow_Layout" />


### <a name="flow-layout"></a>フロー レイアウト

`UICollectionViewFlowLayout`クラスはそのまで見てきたように、セルのグリッド内のコンテンツを配置するために適切な行ベースのレイアウトを提供します。

フロー レイアウトを使用します。

-  インスタンスを作成する`UICollectionViewFlowLayout`:


```csharp
var layout = new UICollectionViewFlowLayout ();
```

-  インスタンスのコンス トラクターに渡す、 `UICollectionView` :


```csharp
simpleCollectionViewController = new SimpleCollectionViewController (layout);
```

これは、すべてのグリッド内のコンテンツをレイアウトするために必要です。 また、変更すると、印刷の向き、`UICollectionViewFlowLayout`処理内容を次のように、適切に再配置。

 [![](uicollectionview-images/05-layout-orientation.png "向きの変更の例")](uicollectionview-images/05-layout-orientation.png#lightbox)

 <a name="Section_Inset" />


#### <a name="section-inset"></a>セクションの埋め込み

一部の領域の周りを提供する、 `UIContentView`、レイアウトが、`SectionInset`型のプロパティ`UIEdgeInsets`です。 次のコードでの各セクションで囲む 50 ピクセル バッファーを提供するなど、`UIContentView`によって配置されたときに、 `UICollectionViewFlowLayout`:

```csharp
var layout = new UICollectionViewFlowLayout ();
layout.SectionInset = new UIEdgeInsets (50,50,50,50);
```

これは、結果は、次のように、セクションの前後に空白になります。

 [![](uicollectionview-images/06-sectioninset.png "次に示すように、セクションを間隔")](uicollectionview-images/06-sectioninset.png#lightbox)

 <a name="Subclassing_UICollectionViewFlowLayout" />


#### <a name="subclassing-uicollectionviewflowlayout"></a>UICollectionViewFlowLayout をサブクラス化

使用するエディションで`UICollectionViewFlowLayout`直接、そのことができますもサブクラス化をさらに、線に沿ったコンテンツのレイアウトをカスタマイズします。 たとえば、次に示すように、グリッドにセルをラップしませんが、代わりに水平方向のスクロール効果のある 1 つの行を作成するレイアウトを作成するこの使用できます。

 [![](uicollectionview-images/07-line-layout.png "水平スクロール効果のある 1 つの行")](uicollectionview-images/07-line-layout.png#lightbox)

これを実装するサブクラス化して`UICollectionViewFlowLayout`が必要です。

-  レイアウトの種類に適用される任意のレイアウト プロパティまたはコンス トラクターでレイアウトのすべての項目を初期化します。
-  オーバーライドする`ShouldInvalidateLayoutForBoundsChange`返される、これを true する場合の境界、`UICollectionView`セルのレイアウトの変更は再計算されます。 これは、使用、コードをここではことを確認 centermost セルに適用される変換は、スクロール中に適用されます。
-  オーバーライドする`TargetContentOffset`、centermost をセルの中央に合わせる、`UICollectionView`としてスクロールが停止します。
-  オーバーライドする`LayoutAttributesForElementsInRect`の配列を返す`UICollectionViewLayoutAttributes`です。 各`UICollectionViewLayoutAttribute`などのプロパティを含む特定のアイテムのレイアウト方法に関する情報が含まれています、 `Center` 、 `Size` 、`ZIndex`と`Transform3D`です。


次のコードは、このような実装を示しています。

```csharp
using System;
using CoreGraphics;
using Foundation;
using UIKit;
using CoreGraphics;
using CoreAnimation;

namespace SimpleCollectionView
{
    public class LineLayout : UICollectionViewFlowLayout
    {
        public const float ITEM_SIZE = 200.0f;
        public const int ACTIVE_DISTANCE = 200;
        public const float ZOOM_FACTOR = 0.3f;

        public LineLayout ()
        {
            ItemSize = new CGSize (ITEM_SIZE, ITEM_SIZE);
            ScrollDirection = UICollectionViewScrollDirection.Horizontal;
            SectionInset = new UIEdgeInsets (400,0,400,0);
            MinimumLineSpacing = 50.0f;
        }

        public override bool ShouldInvalidateLayoutForBoundsChange (CGRect newBounds)
        {
            return true;
        }

        public override UICollectionViewLayoutAttributes[] LayoutAttributesForElementsInRect (CGRect rect)
        {
            var array = base.LayoutAttributesForElementsInRect (rect);
            var visibleRect = new CGRect (CollectionView.ContentOffset, CollectionView.Bounds.Size);

            foreach (var attributes in array) {
                if (attributes.Frame.IntersectsWith (rect)) {
                    float distance = (float)(visibleRect.GetMidX () - attributes.Center.X);
                    float normalizedDistance = distance / ACTIVE_DISTANCE;
                    if (Math.Abs (distance) < ACTIVE_DISTANCE) {
                        float zoom = 1 + ZOOM_FACTOR * (1 - Math.Abs (normalizedDistance));
                        attributes.Transform3D = CATransform3D.MakeScale (zoom, zoom, 1.0f);
                        attributes.ZIndex = 1;
                    }
                }
            }
            return array;
        }

        public override CGPoint TargetContentOffset (CGPoint proposedContentOffset, CGPoint scrollingVelocity)
        {
            float offSetAdjustment = float.MaxValue;
            float horizontalCenter = (float)(proposedContentOffset.X + (this.CollectionView.Bounds.Size.Width / 2.0));
            CGRect targetRect = new CGRect (proposedContentOffset.X, 0.0f, this.CollectionView.Bounds.Size.Width, this.CollectionView.Bounds.Size.Height);
            var array = base.LayoutAttributesForElementsInRect (targetRect);
            foreach (var layoutAttributes in array) {
                float itemHorizontalCenter = (float)layoutAttributes.Center.X;
                if (Math.Abs (itemHorizontalCenter - horizontalCenter) < Math.Abs (offSetAdjustment)) {
                    offSetAdjustment = itemHorizontalCenter - horizontalCenter;
                }
            }
            return new CGPoint (proposedContentOffset.X + offSetAdjustment, proposedContentOffset.Y);
        }

    }
}
```

 <a name="Custom_Layout" />


### <a name="custom-layout"></a>カスタム レイアウト

使用するだけでなく`UICollectionViewFlowLayout`、レイアウトにから直接継承することによって完全にカスタマイズできます`UICollectionViewLayout`です。

キーを上書きする方法を示します。

-   `PrepareLayout` – レイアウト プロセス全体で使用される初期の幾何学的計算の実行に使用されます。
-   `CollectionViewContentSize` – コンテンツを表示するための領域のサイズを返します。
-   `LayoutAttributesForElementsInRect` – 情報を提供する前に示した UICollectionViewFlowLayout 例として、このメソッドが使用されるものと、`UICollectionView`レイアウトする方法についての各項目。 ただしとは異なり、 `UICollectionViewFlowLayout` 、カスタム レイアウトを作成する場合は、選択したアイテムを配置することができます。


たとえば、次のようには、同じコンテンツを循環のレイアウトで表示でした。

 [![](uicollectionview-images/08-circle-layout.png "循環カスタム レイアウトを次に示すよう")](uicollectionview-images/08-circle-layout.png#lightbox)

レイアウト優れている点はグリッド レイアウトから水平方向のスクロール レイアウトに変更することと、後でこの循環レイアウトに提供されるレイアウト クラスのみが必要です、`UICollectionView`を変更します。 場合は nothing、 `UICollectionView`、そのデリゲートのデータ ソース コードの変更にします。


## <a name="changes-in-ios-9"></a>IOS 9 の変更


IOS 9、コレクション ビュー内で (`UICollectionView`) ここではサポートしていますが新しい既定のジェスチャ レコグナイザーといくつかの新しいサポート メソッドを追加することで、ボックスから項目の並べ替えをドラッグします。

これらの新しいメソッドを使用して、ドラッグすると、コレクション ビュー内の順序を変更し、並べ替え処理の段階で、項目の外観のカスタマイズのオプションがありますを簡単に実装できます。

[![](uicollectionview-images/intro01.png "並べ替えのプロセスの例")](uicollectionview-images/intro01.png#lightbox)

この記事では、Xamarin.iOS アプリケーションだけでなく他の iOS 9 が加えられた変更をコレクション ビュー コントロールのドラッグで並べ替えを実装することを確認をみましょう。

- [項目の簡単な並べ替え](#Easy-Reordering-of-Items)
    - [並べ替えの簡単な例](#Simple-Reordering-Example)
    - [カスタム ジェスチャ認識エンジンの使用](#Using-a-Custom-Gesture-Recognizer)
    - [カスタム レイアウトや並べ替え](#Custom-Layouts-and-Reording)
- [コレクションの変更の表示](#Collection-View-Changes)

<a name="Easy-Reordering-of-Items" />

## <a name="reordering-of-items"></a>項目の並べ替え

前述のように、iOS 9 のコレクション ビューに最も重要な変更のいずれかがすぐ使いやすいドラッグの並べ替え機能の追加です。

コレクション ビューに並べ替えを追加する最も簡単な方法は、iOS 9 を使用する、`UICollectionViewController`です。
コレクション ビューのコント ローラーになりました、`InstallsStandardGestureForInteractiveMovement`プロパティで、追加の標準*ジェスチャ レコグナイザー*コレクション内の項目の順序を変更するドラッグをサポートします。
既定値はため`true`、のみを実装する必要がある、`MoveItem`のメソッド、`UICollectionViewDataSource`ドラッグの並べ替えをサポートするクラス。 例えば:

```csharp
public override void MoveItem (UICollectionView collectionView, NSIndexPath sourceIndexPath, NSIndexPath destinationIndexPath)
{
    // Reorder our list of items
    ...
}
```
<a name="Simple-Reordering-Example" />

### <a name="simple-reordering-example"></a>並べ替えの簡単な例

簡単な例として、新しい Xamarin.iOS プロジェクトを開始し、編集、 **Main.storyboard**ファイル。 ドラッグ、`UICollectionViewController`デザイン サーフェイスに。

[![](uicollectionview-images/quick01.png "UICollectionViewController を追加します。")](uicollectionview-images/quick01.png#lightbox)

(これは、ドキュメント アウトラインから実行する最も簡単な使用できない可能性があります)、コレクション ビューを選択します。 パッドのプロパティの [レイアウト] タブでは、次のスクリーン ショットに示すように、次のサイズを設定します。

- **セル サイズ**: 幅 – 60 |高さ – 60
- **ヘッダー サイズ**: 幅 – 0 |高さ – 0
- **フッターのサイズ**: 幅 – 0 |高さ – 0
- **最小間隔**: セル: 8 の |行: 8
- **セクションのくぼみ**: Top – 16 |下部にある – 16 |左 – 16 |右: 16

[![](uicollectionview-images/quick04.png "コレクション ビューのサイズを設定します。")](uicollectionview-images/quick04.png#lightbox)

次に、既定のセルを編集します。
    - その背景色を青に変更します。
    - セルのタイトルとして機能するラベルを追加します。
    - 再利用する識別子に設定**セル**

[![](uicollectionview-images/quick02.png "既定値のセルを編集します。")](uicollectionview-images/quick02.png#lightbox)

サイズが変わるように、セルの内部中央にラベルを保持する制約を追加します。

**プロパティ パッド**の_CollectionViewCell_設定と、**クラス**に`TextCollectionViewCell`:

[![](uicollectionview-images/quick05.png "クラスを TextCollectionViewCell に設定します。")](uicollectionview-images/quick05.png#lightbox)

設定、**コレクションの再利用可能なビュー**に`Cell`:

[![](uicollectionview-images/quick06.png "セルに、コレクションの再利用可能なビューを設定します。")](uicollectionview-images/quick06.png#lightbox)

最後に、ラベルを選択し、名前を付けます`TextLabel`:

[![](uicollectionview-images/quick07.png "name label TextLabel")](uicollectionview-images/quick07.png#lightbox)

編集、`TextCollectionViewCell`クラスし、次のプロパティを追加します。

```csharp
using System;
using Foundation;
using UIKit;

namespace CollectionView
{
    public partial class TextCollectionViewCell : UICollectionViewCell
    {
        #region Computed Properties
        public string Title {
            get { return TextLabel.Text; }
            set { TextLabel.Text = value; }
        }
        #endregion

        #region Constructors
        public TextCollectionViewCell (IntPtr handle) : base (handle)
        {
        }
        #endregion
    }
}
```

ここで、`Text`ラベルのプロパティが、セルのタイトルとして公開されるため、コードから設定することができます。

新しい c# クラスをプロジェクトに追加し、それを呼び出す`WaterfallCollectionSource`です。 ファイルを編集し、次のようになります。

```csharp
using System;
using Foundation;
using UIKit;
using System.Collections.Generic;

namespace CollectionView
{
    public class WaterfallCollectionSource : UICollectionViewDataSource
    {
        #region Computed Properties
        public WaterfallCollectionView CollectionView { get; set;}
        public List<int> Numbers { get; set; } = new List<int> ();
        #endregion

        #region Constructors
        public WaterfallCollectionSource (WaterfallCollectionView collectionView)
        {
            // Initialize
            CollectionView = collectionView;

            // Init numbers collection
            for (int n = 0; n < 100; ++n) {
                Numbers.Add (n);
            }
        }
        #endregion

        #region Override Methods
        public override nint NumberOfSections (UICollectionView collectionView) {
            // We only have one section
            return 1;
        }

        public override nint GetItemsCount (UICollectionView collectionView, nint section) {
            // Return the number of items
            return Numbers.Count;
        }

        public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
        {
            // Get a reusable cell and set {~~it's~>its~~} title from the item
            var cell = collectionView.DequeueReusableCell ("Cell", indexPath) as TextCollectionViewCell;
            cell.Title = Numbers [(int)indexPath.Item].ToString();

            return cell;
        }

        public override bool CanMoveItem (UICollectionView collectionView, NSIndexPath indexPath) {
            // We can always move items
            return true;
        }

        public override void MoveItem (UICollectionView collectionView, NSIndexPath sourceIndexPath, NSIndexPath destinationIndexPath)
        {
            // Reorder our list of items
            var item = Numbers [(int)sourceIndexPath.Item];
            Numbers.RemoveAt ((int)sourceIndexPath.Item);
            Numbers.Insert ((int)destinationIndexPath.Item, item);
        }
        #endregion
    }
}
```

このクラスは、コレクション ビューのデータ ソースにして、コレクション内の各セルに関する情報を提供します。
注意して、`MoveItem`するコレクション内の項目をドラッグの並べ替えられたを許可するメソッドを実装します。

別の新しい c# クラスをプロジェクトに追加し、それを呼び出す`WaterfallCollectionDelegate`です。 このファイルを編集し、次のようになります。

```csharp
using System;
using Foundation;
using UIKit;
using System.Collections.Generic;

namespace CollectionView
{
    public class WaterfallCollectionDelegate : UICollectionViewDelegate
    {
        #region Computed Properties
        public WaterfallCollectionView CollectionView { get; set;}
        #endregion

        #region Constructors
        public WaterfallCollectionDelegate (WaterfallCollectionView collectionView)
        {

            // Initialize
            CollectionView = collectionView;

        }
        #endregion

        #region Overrides Methods
        public override bool ShouldHighlightItem (UICollectionView collectionView, NSIndexPath indexPath) {
            // Always allow for highlighting
            return true;
        }

        public override void ItemHighlighted (UICollectionView collectionView, NSIndexPath indexPath)
        {
            // Get cell and change to green background
            var cell = collectionView.CellForItem(indexPath);
            cell.ContentView.BackgroundColor = UIColor.FromRGB(183,208,57);
        }

        public override void ItemUnhighlighted (UICollectionView collectionView, NSIndexPath indexPath)
        {
            // Get cell and return to blue background
            var cell = collectionView.CellForItem(indexPath);
            cell.ContentView.BackgroundColor = UIColor.FromRGB(164,205,255);
        }
        #endregion
    }
}
```

これは、コレクション ビューの代理として機能します。 コレクション ビュー内で、ユーザーが操作には、セルを強調表示するメソッドがオーバーライドされました。

1 つの最後 c# クラスをプロジェクトに追加し、それを呼び出す`WaterfallCollectionView`です。 このファイルを編集し、次のようになります。

```csharp
using System;
using UIKit;
using System.Collections.Generic;
using Foundation;

namespace CollectionView
{
    [Register("WaterfallCollectionView")]
    public class WaterfallCollectionView : UICollectionView
    {

        #region Constructors
        public WaterfallCollectionView (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

            // Initialize
            DataSource = new WaterfallCollectionSource(this);
            Delegate = new WaterfallCollectionDelegate(this);

        }
        #endregion
    }
}
```

注意して`DataSource`と`Delegate`上に作成したコレクション ビューは、ストーリー ボードから構築されたときに設定されます (または**.xib**ファイル)。

編集、 **Main.storyboard**ファイルを再びコレクション ビューを選択しに切り替えると、**プロパティ**です。 設定、**クラス**をカスタム`WaterfallCollectionView`上で定義したクラス。

UI に対して行った変更を保存し、アプリを実行します。
ユーザーは、一覧から項目を選択しを新しい場所にドラッグする場合、他のアイテムがアニメーション化自動的にアイテムの移動を邪魔にします。
ユーザーは、新しい位置に項目をドロップ、ときに、その場所に固定されます。 例えば:

[![](uicollectionview-images/intro01.png "新しい場所に項目をドラッグすることの例")](uicollectionview-images/intro01.png#lightbox)

<a name="Using-a-Custom-Gesture-Recognizer" />

### <a name="using-a-custom-gesture-recognizer"></a>カスタム ジェスチャ認識エンジンの使用

使用できない場合、`UICollectionViewController`標準を使用する必要があります`UIViewController`、または、ドラッグ アンド ドロップ ジェスチャより詳細に制御を実行する場合は、独自のカスタム ジェスチャ レコグナイザーを作成し、ビューを読み込むときに、コレクション ビューに追加します。 例えば:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Create a custom gesture recognizer
    var longPressGesture = new UILongPressGestureRecognizer ((gesture) => {

        // Take action based on state
        switch(gesture.State) {
        case UIGestureRecognizerState.Began:
            var selectedIndexPath = CollectionView.IndexPathForItemAtPoint(gesture.LocationInView(View));
            if (selectedIndexPath !=null) {
                CollectionView.BeginInteractiveMovementForItem(selectedIndexPath);
            }
            break;
        case UIGestureRecognizerState.Changed:
            CollectionView.UpdateInteractiveMovementTargetPosition(gesture.LocationInView(View));
            break;
        case UIGestureRecognizerState.Ended:
            CollectionView.EndInteractiveMovement();
            break;
        default:
            CollectionView.CancelInteractiveMovement();
            break;
        }

    });

    // Add the custom recognizer to the collection view
    CollectionView.AddGestureRecognizer(longPressGesture);
}
```

ここで実装し、ドラッグ操作を制御する、コレクション ビューに追加されたいくつかの新しいメソッドを使用します。

 - `BeginInteractiveMovementForItem` 移動操作の開始をマークします。
 - `UpdateInteractiveMovementTargetPosition` 送信、項目の場所が更新されます。
 - `EndInteractiveMovement` 項目の移動の末尾をマークします。
 - `CancelInteractiveMovement` -ユーザーが、移動操作を取り消したをマークします。

アプリケーションを実行すると、ドラッグ操作は、既定値は、コレクション ビューに付属しているジェスチャ レコグナイザーをドラッグと同じように機能します。

<a name="Custom-Layouts-and-Reording" />

### <a name="custom-layouts-and-reordering"></a>カスタム レイアウトや並べ替え

IOS 9、コレクション ビューにドラッグ、並べ替え、カスタム レイアウトを使用するいくつかの新しいメソッドが追加されています。 この機能を探索するには、コレクションに、カスタム レイアウトを追加してみましょう。

最初に、新しい c# クラスを追加`WaterfallCollectionLayout`をプロジェクトにします。 これを編集し、次のようになります。

```csharp
using System;
using Foundation;
using UIKit;
using System.Collections.Generic;
using CoreGraphics;

namespace CollectionView
{
    [Register("WaterfallCollectionLayout")]
    public class WaterfallCollectionLayout : UICollectionViewLayout
    {
        #region Private Variables
        private int columnCount = 2;
        private nfloat minimumColumnSpacing = 10;
        private nfloat minimumInterItemSpacing = 10;
        private nfloat headerHeight = 0.0f;
        private nfloat footerHeight = 0.0f;
        private UIEdgeInsets sectionInset = new UIEdgeInsets(0, 0, 0, 0);
        private WaterfallCollectionRenderDirection itemRenderDirection = WaterfallCollectionRenderDirection.ShortestFirst;
        private Dictionary<nint,UICollectionViewLayoutAttributes> headersAttributes = new Dictionary<nint, UICollectionViewLayoutAttributes>();
        private Dictionary<nint,UICollectionViewLayoutAttributes> footersAttributes = new Dictionary<nint, UICollectionViewLayoutAttributes>();
        private List<CGRect> unionRects = new List<CGRect>();
        private List<nfloat> columnHeights = new List<nfloat>();
        private List<UICollectionViewLayoutAttributes> allItemAttributes = new List<UICollectionViewLayoutAttributes>();
        private List<List<UICollectionViewLayoutAttributes>> sectionItemAttributes = new List<List<UICollectionViewLayoutAttributes>>();
        private nfloat unionSize = 20;
        #endregion

        #region Computed Properties
        [Export("ColumnCount")]
        public int ColumnCount {
            get { return columnCount; }
            set {
                WillChangeValue ("ColumnCount");
                columnCount = value;
                DidChangeValue ("ColumnCount");

                InvalidateLayout ();
            }
        }

        [Export("MinimumColumnSpacing")]
        public nfloat MinimumColumnSpacing {
            get { return minimumColumnSpacing; }
            set {
                WillChangeValue ("MinimumColumnSpacing");
                minimumColumnSpacing = value;
                DidChangeValue ("MinimumColumnSpacing");

                InvalidateLayout ();
            }
        }

        [Export("MinimumInterItemSpacing")]
        public nfloat MinimumInterItemSpacing {
            get { return minimumInterItemSpacing; }
            set {
                WillChangeValue ("MinimumInterItemSpacing");
                minimumInterItemSpacing = value;
                DidChangeValue ("MinimumInterItemSpacing");

                InvalidateLayout ();
            }
        }

        [Export("HeaderHeight")]
        public nfloat HeaderHeight {
            get { return headerHeight; }
            set {
                WillChangeValue ("HeaderHeight");
                headerHeight = value;
                DidChangeValue ("HeaderHeight");

                InvalidateLayout ();
            }
        }

        [Export("FooterHeight")]
        public nfloat FooterHeight {
            get { return footerHeight; }
            set {
                WillChangeValue ("FooterHeight");
                footerHeight = value;
                DidChangeValue ("FooterHeight");

                InvalidateLayout ();
            }
        }

        [Export("SectionInset")]
        public UIEdgeInsets SectionInset {
            get { return sectionInset; }
            set {
                WillChangeValue ("SectionInset");
                sectionInset = value;
                DidChangeValue ("SectionInset");

                InvalidateLayout ();
            }
        }

        [Export("ItemRenderDirection")]
        public WaterfallCollectionRenderDirection ItemRenderDirection {
            get { return itemRenderDirection; }
            set {
                WillChangeValue ("ItemRenderDirection");
                itemRenderDirection = value;
                DidChangeValue ("ItemRenderDirection");

                InvalidateLayout ();
            }
        }
        #endregion

        #region Constructors
        public WaterfallCollectionLayout ()
        {
        }

        public WaterfallCollectionLayout(NSCoder coder) : base(coder) {

        }
        #endregion

        #region Public Methods
        public nfloat ItemWidthInSectionAtIndex(int section) {

            var width = CollectionView.Bounds.Width - SectionInset.Left - SectionInset.Right;
            return (nfloat)Math.Floor ((width - ((ColumnCount - 1) * MinimumColumnSpacing)) / ColumnCount);
        }
        #endregion

        #region Override Methods
        public override void PrepareLayout ()
        {
            base.PrepareLayout ();

            // Get the number of sections
            var numberofSections = CollectionView.NumberOfSections();
            if (numberofSections == 0)
                return;

            // Reset collections
            headersAttributes.Clear ();
            footersAttributes.Clear ();
            unionRects.Clear ();
            columnHeights.Clear ();
            allItemAttributes.Clear ();
            sectionItemAttributes.Clear ();

            // Initialize column heights
            for (int n = 0; n < ColumnCount; n++) {
                columnHeights.Add ((nfloat)0);
            }

            // Process all sections
            nfloat top = 0.0f;
            var attributes = new UICollectionViewLayoutAttributes ();
            var columnIndex = 0;
            for (nint section = 0; section < numberofSections; ++section) {
                // Calculate section specific metrics
                var minimumInterItemSpacing = (MinimumInterItemSpacingForSection == null) ? MinimumColumnSpacing :
                    MinimumInterItemSpacingForSection (CollectionView, this, section);

                // Calculate widths
                var width = CollectionView.Bounds.Width - SectionInset.Left - SectionInset.Right;
                var itemWidth = (nfloat)Math.Floor ((width - ((ColumnCount - 1) * MinimumColumnSpacing)) / ColumnCount);

                // Calculate section header
                var heightHeader = (HeightForHeader == null) ? HeaderHeight :
                    HeightForHeader (CollectionView, this, section);

                if (heightHeader > 0) {
                    attributes = UICollectionViewLayoutAttributes.CreateForSupplementaryView (UICollectionElementKindSection.Header, NSIndexPath.FromRowSection (0, section));
                    attributes.Frame = new CGRect (0, top, CollectionView.Bounds.Width, heightHeader);
                    headersAttributes.Add (section, attributes);
                    allItemAttributes.Add (attributes);

                    top = attributes.Frame.GetMaxY ();
                }

                top += SectionInset.Top;
                for (int n = 0; n < ColumnCount; n++) {
                    columnHeights [n] = top;
                }

                // Calculate Section Items
                var itemCount = CollectionView.NumberOfItemsInSection(section);
                List<UICollectionViewLayoutAttributes> itemAttributes = new List<UICollectionViewLayoutAttributes> ();

                for (nint n = 0; n < itemCount; n++) {
                    var indexPath = NSIndexPath.FromRowSection (n, section);
                    columnIndex = NextColumnIndexForItem (n);
                    var xOffset = SectionInset.Left + (itemWidth + MinimumColumnSpacing) * (nfloat)columnIndex;
                    var yOffset = columnHeights [columnIndex];
                    var itemSize = (SizeForItem == null) ? new CGSize (0, 0) : SizeForItem (CollectionView, this, indexPath);
                    nfloat itemHeight = 0.0f;

                    if (itemSize.Height > 0.0f && itemSize.Width > 0.0f) {
                        itemHeight = (nfloat)Math.Floor (itemSize.Height * itemWidth / itemSize.Width);
                    }

                    attributes = UICollectionViewLayoutAttributes.CreateForCell (indexPath);
                    attributes.Frame = new CGRect (xOffset, yOffset, itemWidth, itemHeight);
                    itemAttributes.Add (attributes);
                    allItemAttributes.Add (attributes);
                    columnHeights [columnIndex] = attributes.Frame.GetMaxY () + MinimumInterItemSpacing;
                }
                sectionItemAttributes.Add (itemAttributes);

                // Calculate Section Footer
                nfloat footerHeight = 0.0f;
                columnIndex = LongestColumnIndex();
                top = columnHeights [columnIndex] - MinimumInterItemSpacing + SectionInset.Bottom;
                footerHeight = (HeightForFooter == null) ? FooterHeight : HeightForFooter(CollectionView, this, section);

                if (footerHeight > 0) {
                    attributes = UICollectionViewLayoutAttributes.CreateForSupplementaryView (UICollectionElementKindSection.Footer, NSIndexPath.FromRowSection (0, section));
                    attributes.Frame = new CGRect (0, top, CollectionView.Bounds.Width, footerHeight);
                    footersAttributes.Add (section, attributes);
                    allItemAttributes.Add (attributes);
                    top = attributes.Frame.GetMaxY ();
                }

                for (int n = 0; n < ColumnCount; n++) {
                    columnHeights [n] = top;
                }
            }

            var i =0;
            var attrs = allItemAttributes.Count;
            while(i < attrs) {
                var rect1 = allItemAttributes [i].Frame;
                i = (int)Math.Min (i + unionSize, attrs) - 1;
                var rect2 = allItemAttributes [i].Frame;
                unionRects.Add (CGRect.Union (rect1, rect2));
                i++;
            }

        }

        public override CGSize CollectionViewContentSize {
            get {
                if (CollectionView.NumberOfSections () == 0) {
                    return new CGSize (0, 0);
                }

                var contentSize = CollectionView.Bounds.Size;
                contentSize.Height = columnHeights [0];
                return contentSize;
            }
        }

        public override UICollectionViewLayoutAttributes LayoutAttributesForItem (NSIndexPath indexPath)
        {
            if (indexPath.Section >= sectionItemAttributes.Count) {
                return null;
            }

            if (indexPath.Item >= sectionItemAttributes [indexPath.Section].Count) {
                return null;
            }

            var list = sectionItemAttributes [indexPath.Section];
            return list [(int)indexPath.Item];
        }

        public override UICollectionViewLayoutAttributes LayoutAttributesForSupplementaryView (NSString kind, NSIndexPath indexPath)
        {
            var attributes = new UICollectionViewLayoutAttributes ();

            switch (kind) {
            case "header":
                attributes = headersAttributes [indexPath.Section];
                break;
            case "footer":
                attributes = footersAttributes [indexPath.Section];
                break;
            }

            return attributes;
        }

        public override UICollectionViewLayoutAttributes[] LayoutAttributesForElementsInRect (CGRect rect)
        {
            var begin = 0;
            var end = unionRects.Count;
            List<UICollectionViewLayoutAttributes> attrs = new List<UICollectionViewLayoutAttributes> ();


            for (int i = 0; i < end; i++) {
                if (rect.IntersectsWith(unionRects[i])) {
                    begin = i * (int)unionSize;
                }
            }

            for (int i = end - 1; i >= 0; i--) {
                if (rect.IntersectsWith (unionRects [i])) {
                    end = (int)Math.Min ((i + 1) * (int)unionSize, allItemAttributes.Count);
                    break;
                }
            }

            for (int i = begin; i < end; i++) {
                var attr = allItemAttributes [i];
                if (rect.IntersectsWith (attr.Frame)) {
                    attrs.Add (attr);
                }
            }

            return attrs.ToArray();
        }

        public override bool ShouldInvalidateLayoutForBoundsChange (CGRect newBounds)
        {
            var oldBounds = CollectionView.Bounds;
            return (newBounds.Width != oldBounds.Width);
        }
        #endregion

        #region Private Methods
        private int ShortestColumnIndex() {
            var index = 0;
            var shortestHeight = nfloat.MaxValue;
            var n = 0;

            // Scan each column for the shortest height
            foreach (nfloat height in columnHeights) {
                if (height < shortestHeight) {
                    shortestHeight = height;
                    index = n;
                }
                ++n;
            }

            return index;
        }

        private int LongestColumnIndex() {
            var index = 0;
            var longestHeight = nfloat.MinValue;
            var n = 0;

            // Scan each column for the shortest height
            foreach (nfloat height in columnHeights) {
                if (height > longestHeight) {
                    longestHeight = height;
                    index = n;
                }
                ++n;
            }

            return index;
        }

        private int NextColumnIndexForItem(nint item) {
            var index = 0;

            switch (ItemRenderDirection) {
            case WaterfallCollectionRenderDirection.ShortestFirst:
                index = ShortestColumnIndex ();
                break;
            case WaterfallCollectionRenderDirection.LeftToRight:
                index = ColumnCount;
                break;
            case WaterfallCollectionRenderDirection.RightToLeft:
                index = (ColumnCount - 1) - ((int)item / ColumnCount);
                break;
            }

            return index;
        }
        #endregion

        #region Events
        public delegate CGSize WaterfallCollectionSizeDelegate(UICollectionView collectionView, WaterfallCollectionLayout layout, NSIndexPath indexPath);
        public delegate nfloat WaterfallCollectionFloatDelegate(UICollectionView collectionView, WaterfallCollectionLayout layout, nint section);
        public delegate UIEdgeInsets WaterfallCollectionEdgeInsetsDelegate(UICollectionView collectionView, WaterfallCollectionLayout layout, nint section);

        public event WaterfallCollectionSizeDelegate SizeForItem;
        public event WaterfallCollectionFloatDelegate HeightForHeader;
        public event WaterfallCollectionFloatDelegate HeightForFooter;
        public event WaterfallCollectionEdgeInsetsDelegate InsetForSection;
        public event WaterfallCollectionFloatDelegate MinimumInterItemSpacingForSection;
        #endregion
    }
}
```

これは、次の 2 つのカスタムの列、コレクション ビューにウォーター フォール型のレイアウトを提供するクラスを使用します。
コード、コーディングのキー値を使用して (を使用して、`WillChangeValue`と`DidChangeValue`メソッド) をこのクラスで、計算済みプロパティのデータ バインディングを提供します。

次に、編集、`WaterfallCollectionSource`し、次の追加と変更。

```csharp
private Random rnd = new Random();
...

public List<nfloat> Heights { get; set; } = new List<nfloat> ();
...

public WaterfallCollectionSource (WaterfallCollectionView collectionView)
{
    // Initialize
    CollectionView = collectionView;

    // Init numbers collection
    for (int n = 0; n < 100; ++n) {
        Numbers.Add (n);
        Heights.Add (rnd.Next (0, 100) + 40.0f);
    }
}
```

これにより、一覧に表示される項目ごとのランダムな高さが作成されます。

次に、編集、`WaterfallCollectionView`クラスし、ヘルパーの次のプロパティを追加します。

```csharp
public WaterfallCollectionSource Source {
    get { return (WaterfallCollectionSource)DataSource; }
}
```

これは容易にできるように、データ ソース (および項目の高さ) では、カスタム レイアウトから取得します。

最後に、ビュー コント ローラーを編集し、次のコードを追加します。

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    var waterfallLayout = new WaterfallCollectionLayout ();

    // Wireup events
    waterfallLayout.SizeForItem += (collectionView, layout, indexPath) => {
        var collection = collectionView as WaterfallCollectionView;
        return new CGSize((View.Bounds.Width-40)/3,collection.Source.Heights[(int)indexPath.Item]);
    };

    // Attach the custom layout to the collection
    CollectionView.SetCollectionViewLayout(waterfallLayout, false);
}
```

各アイテムのサイズを提供するイベントを設定、および、コレクション ビューに新しいレイアウトをアタッチこれ、カスタム レイアウトのインスタンスを作成します。

Xamarin.iOS アプリを再度実行して、コレクション ビューは、次のようになりますようになりました。

[![](uicollectionview-images/custom01.png "コレクション ビューは次のようになります")](uicollectionview-images/custom01.png#lightbox)

並べ替えドラッグのまだ項目としてする前に、ごことができますが、項目はこれらを削除しているときに、新しい場所に合わせてサイズを変更するようになりました。

## <a name="collection-view-changes"></a>コレクションの変更の表示

次のセクションでは、iOS 9 コレクション ビュー内の各クラスに加えられた変更についての詳細をみましょう。

### <a name="uicollectionview"></a>UICollectionView

次の変更または追加機能に加えられた、 `UICollectionView` iOS 9 のクラス。

 - `BeginInteractiveMovementForItem` – は、ドラッグ操作の開始をマークします。
 - `CancelInteractiveMovement` – コレクションの通知ビュー、ユーザーがドラッグ操作をキャンセルされたことです。
 - `EndInteractiveMovement` – コレクションの通知ビュー、ユーザーがドラッグ操作を完了したことです。
 - `GetIndexPathsForVisibleSupplementaryElements` – を返します、`indexPath`のヘッダーまたはフッター コレクション ビューのセクションでします。
 - `GetSupplementaryView` – 特定のヘッダーまたはページ フッターを返します。
 - `GetVisibleSupplementaryViews` – すべての表示されているヘッダーとフッターの一覧を返します。
 - `UpdateInteractiveMovementTargetPosition` – コレクションの通知、ユーザーが移動されたか、移動は、ドラッグ操作中にアイテムを表示します。

### <a name="uicollectionviewcontroller"></a>UICollectionViewController

次の変更または追加機能に加えられた、 `UICollectionViewController` iOS 9 のクラス。

 - `InstallsStandardGestureForInteractiveMovement` –`true`自動的にドラッグの並べ替えをサポートする新しいジェスチャ認識エンジンが使用されます。
 - `CanMoveItem` – 特定のアイテムにドラッグの順序を変更できる場合は、コレクション ビューに通知します。
 - `GetTargetContentOffset` – 特定のコレクション アイテムの表示のオフセットを取得するために使用します。
 - `GetTargetIndexPathForMove` – を取得、`indexPath`のドラッグ操作について、指定された項目。
 - `MoveItem` – 特定のアイテムの順序を一覧に移動します。


### <a name="uicollectionviewdatasource"></a>UICollectionViewDataSource

次の変更または追加機能に加えられた、 `UICollectionViewDataSource` iOS 9 のクラス。

 - `CanMoveItem` – 特定のアイテムにドラッグの順序を変更できる場合は、コレクション ビューに通知します。
 - `MoveItem` – 特定のアイテムの順序を一覧に移動します。

### <a name="uicollectionviewdelegate"></a>UICollectionViewDelegate

次の変更または追加機能に加えられた、 `UICollectionViewDelegate` iOS 9 のクラス。

 - `GetTargetContentOffset` – 特定のコレクション アイテムの表示のオフセットを取得するために使用します。
 - `GetTargetIndexPathForMove` – を取得、`indexPath`のドラッグ操作について、指定された項目。

### <a name="uicollectionviewflowlayout"></a>UICollectionViewFlowLayout

次の変更または追加機能に加えられた、 `UICollectionViewFlowLayout` iOS 9 のクラス。

 - `SectionFootersPinToVisibleBounds` – セクションのフッターを表示されているコレクション ビューの境界を見やすいようにアレンジします。
 - `SectionHeadersPinToVisibleBounds` – 表示されているコレクション ビューの境界をセクション ヘッダーを見やすいようにアレンジします。

### <a name="uicollectionviewlayout"></a>UICollectionViewLayout

次の変更または追加機能に加えられた、 `UICollectionViewLayout` iOS 9 のクラス。

 - `GetInvalidationContextForEndingInteractiveMovementOfItems` – ユーザーは、ドラッグを終了するかをキャンセルする場合に、ドラッグ操作の最後に、無効化コンテキストを返します。
 - `GetInvalidationContextForInteractivelyMovingItems` – は、ドラッグ操作の開始時の無効化コンテキストを返します。
 - `GetLayoutAttributesForInteractivelyMovingItem` 項目をドラッグするときに特定のアイテムのレイアウト属性を取得します。
 - `GetTargetIndexPathForInteractivelyMovingItem` – を返します、`indexPath`項目をドラッグしたときに、特定の位置にある項目の。

### <a name="uicollectionviewlayoutattributes"></a>UICollectionViewLayoutAttributes

次の変更または追加機能に加えられた、 `UICollectionViewLayoutAttributes` iOS 9 のクラス。

 - `CollisionBoundingPath` – ドラッグ操作中に 2 つの項目の競合のパスを返します。
 - `CollisionBoundsType` – 競合の種類を返します (として、 `UIDynamicItemCollisionBoundsType`) がドラッグ操作中に発生しました。

### <a name="uicollectionviewlayoutinvalidationcontext"></a>UICollectionViewLayoutInvalidationContext

次の変更または追加機能に加えられた、 `UICollectionViewLayoutInvalidationContext` iOS 9 のクラス。

 - `InteractiveMovementTarget` – は、ドラッグ操作のターゲット項目を返します。
 - `PreviousIndexPathsForInteractivelyMovingItems` – を返します、`indexPaths`他の項目のドラッグすると、操作の順序を変更に関係します。
 - `TargetIndexPathsForInteractivelyMovingItems` – を返します、`indexPaths`の項目をドラッグ-並べ替え操作の結果として並べ替えられます。

### <a name="uicollectionviewsource"></a>UICollectionViewSource

次の変更または追加機能に加えられた、 `UICollectionViewSource` iOS 9 のクラス。

 - `CanMoveItem` – 特定のアイテムにドラッグの順序を変更できる場合は、コレクション ビューに通知します。
 - `GetTargetContentOffset` – ドラッグ-並べ替え操作で移動するアイテムのオフセットを返します。
 - `GetTargetIndexPathForMove` – を返します、`indexPath`ドラッグ-並べ替え操作中に移動する項目の。
 - `MoveItem` – 特定のアイテムの順序を一覧に移動します。

## <a name="summary"></a>まとめ

この記事が iOS 9 のコレクション ビューに加えた変更を説明し、Xamarin.iOS でそれらを実装する方法について説明します。
コレクション ビューの単純なドラッグ-並べ替えアクションを実装することを説明ドラッグの順序変更; でのカスタム ジェスチャ レコグナイザーの使用ドラッグの並べ替えがカスタム コレクション ビューのレイアウトに影響します。

## <a name="related-links"></a>関連リンク

- [iOS 9 のサンプル](https://developer.xamarin.com/samples/ios/iOS9/)
- [コレクション ビューのサンプル](https://developer.xamarin.com/samples/monotouch/ios9/CollectionView/)
- [SimpleCollectionView (サンプル)](https://developer.xamarin.com/samples/SimpleCollectionView/)
- [イベント、プロトコル、デリゲート](~/ios/app-fundamentals/delegates-protocols-and-events.md)
- [テーブルおよびセルの使用](~/ios/user-interface/controls/tables/index.md)
