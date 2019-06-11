---
title: Xamarin.iOS でのコレクション ビュー
description: コレクション ビューでは、任意のレイアウトで表示されるコンテンツを許可します。 簡単にもカスタム レイアウトをサポートしているときにすぐに使えるグリッドのようなレイアウトを作成できます。
ms.prod: xamarin
ms.assetid: F4B85F25-0CB5-4FEA-A3B5-D22FCDC81AE4
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/20/2017
ms.openlocfilehash: a93de9d60a515b6089b35a64eb8832c456c96557
ms.sourcegitcommit: 2eb8961dd7e2a3e06183923adab6e73ecb38a17f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/11/2019
ms.locfileid: "66827348"
---
# <a name="collection-views-in-xamarinios"></a>Xamarin.iOS でのコレクション ビュー

_コレクション ビューでは、任意のレイアウトで表示されるコンテンツを許可します。簡単にもカスタム レイアウトをサポートしているときにすぐに使えるグリッドのようなレイアウトを作成できます。_

コレクション ビューで使用できる、`UICollectionView`クラスは、iOS 6 のレイアウトを使用して、画面で、プレゼンテーションを複数の項目を紹介する新しい概念です。 データを提供するためのパターンを`UICollectionView`を項目を作成し、同じ委任とデータ ソースの iOS 開発でよく使用されるパターンこれら項目に従って操作します。

ただし、コレクション ビューが依存しないレイアウト サブシステムの使用、`UICollectionView`自体。 そのため、別のレイアウトを指定するだけ簡単に変更できますコレクション ビューの表示。

iOS と呼ばれるレイアウト クラスを提供する`UICollectionViewFlowLayout`など、追加作業なしで作成されるグリッド線ベースのレイアウトをできるようにします。 また、カスタム レイアウト作成することも想像できる任意のプレゼンテーションを許可します。

## <a name="uicollectionview-basics"></a>UICollectionView の基礎

`UICollectionView`クラスが 3 つの異なる項目の構成します。

-  **セル**– 各項目のデータに基づくビュー
-  **ビューの補助**– セクションに関連付けられているデータに基づくビュー。
-  **装飾ビュー** – 以外のデータ駆動型のレイアウトによって作成されたビュー

## <a name="cells"></a>セル

セルには、コレクション ビューで提示されているデータ セット内の 1 つの項目を表すオブジェクトです。 各セルのインスタンスである、`UICollectionViewCell`クラスは、次の図に示すように、次の 3 つの異なるビューから構成されます。

 [![](uicollectionview-images/01-uicollectionviewcell.png "次のように各セルは 3 つの異なるビューで構成されます。")](uicollectionview-images/01-uicollectionviewcell.png#lightbox)

`UICollectionViewCell`クラスのこれらの各ビューは次のプロパティには。

-   `ContentView` このビューには、コンテンツが、セルが含まれています。 画面の最上位 z オーダーに表示します。
-   `SelectedBackgroundView` – セルの選択のサポートが組み込まれています。 このビューは、視覚的にセルが選択されていることを示すために使用されます。 すぐ下にレンダリングされた、`ContentView`セルを選択するとします。
-   `BackgroundView` – セルは、表示されるバック グラウンドも表示できます、`BackgroundView`します。 このビューが下に表示される、`SelectedBackgroundView`します。


設定して、`ContentView`よりも小さくするよう、`BackgroundView`と`SelectedBackgroundView`、`BackgroundView`視覚的にフレームのコンテンツの実行中に使用できる、`SelectedBackgroundView`が表示されます、セルを選択すると、次に示すよう。

 [![](uicollectionview-images/02-cells.png "別のセル要素")](uicollectionview-images/02-cells.png#lightbox)

継承することによって、上記のスクリーン ショットのセルが作成されます`UICollectionViewCell`と設定、 `ContentView`、`SelectedBackgroundView`と`BackgroundView`プロパティ、それぞれに、次のコードに示すようにします。

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

補助ビューは、の各セクションに関連付けられている情報を表示するビュー、`UICollectionView`します。 セルのような補助ビューは、データ駆動型です。 セルにデータ ソースから項目のデータが存在する、補助ビューは、本棚に書籍のカテゴリまたは音楽ライブラリ内の音楽のジャンルなどのセクションのデータを提供します。

たとえば、次の図に示すようには、補助ビューを特定のセクションのヘッダーを表示する使用可能性があります。

 [![](uicollectionview-images/02a-supplementary-view.png "次に示すように特定のセクションのヘッダーを表示する補助ビューが使用されます。")](uicollectionview-images/02a-supplementary-view.png#lightbox)

補助ビューを使用して、これには、最初に登録する必要が、`ViewDidLoad`メソッド。

```csharp
CollectionView.RegisterClassForSupplementaryView (typeof(Header), UICollectionElementKindSection.Header, headerId);
```

ビューを使用して返される必要がありますし、`GetViewForSupplementaryElement`を使用して作成された`DequeueReusableSupplementaryView`、継承と`UICollectionReusableView`します。 次のコード スニペットは、上記のスクリーン ショットに示すように SupplementaryView に生成されます。

```csharp
public override UICollectionReusableView GetViewForSupplementaryElement (UICollectionView collectionView, NSString elementKind, NSIndexPath indexPath)
        {
            var headerView = (Header)collectionView.DequeueReusableSupplementaryView (elementKind, headerId, indexPath);
            headerView.Text = "Supplementary View";
            return headerView;
        }

```

補助のビューは、ヘッダーとフッターだけより一般的です。
コレクション ビューに任意の場所に配置することができ、それらの外観を完全にカスタマイズ可能なため、すべてのビューで構成できます。

 <a name="Decoration_Views" />


## <a name="decoration-views"></a>装飾のビュー

装飾ビューは、純粋なビジュアルのビューで表示できる、`UICollectionView`します。 セルと補助ビューとは異なりがないデータ ドリブンです。 レイアウトのサブクラス内で常に作成され、コンテンツのレイアウトとして後で変更できます。 たとえば、装飾の表示される可能性がありますでコンテンツをスクロールするバック グラウンドのビューを提示する、`UICollectionView`以下に示すように。

 [![](uicollectionview-images/02c-decoration-view.png "背景が赤の装飾の表示")](uicollectionview-images/02c-decoration-view.png#lightbox)

 次のコード スニペットでは、バック グラウンドをサンプルでは赤に変更`CircleLayout`クラス。

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

IOS の他の部分と同様など`UITableView`と`MKMapView`、`UICollectionView`からデータを取得します、*データ ソース*を使用して Xamarin.iOS で公開される、 **`UICollectionViewDataSource`** クラス。 コンテンツを提供するため、このクラスは、`UICollectionView`など。

-  **セル**– から返された`GetCell`メソッド。
-  **ビューの補助**– から返された`GetViewForSupplementaryElement`メソッド。
-  **セクション数**– から返された`NumberOfSections`メソッド。 既定値は実装されていない場合は 1 です。
-  **セクション 1 項目数**– から返された`GetItemsCount`メソッド。

### <a name="uicollectionviewcontroller"></a>UICollectionViewController
便宜上、`UICollectionViewController`クラスは使用できます。これは自動的に次のセクションで説明されているが、両方のデリゲートを構成し、データ ソースの`UICollectionView`ビュー。

同様`UITableView`、`UICollectionView`クラスは、画面上にある項目のセルを取得するには、そのデータ ソースだけを呼び出します。
画面の外にスクロールするセルは、次の図に示すように再利用するためのキューに配置されます。

 [![](uicollectionview-images/03-cell-reuse.png "画面の外にスクロールするセルは、再利用するためのキューに次のように配置されます。")](uicollectionview-images/03-cell-reuse.png#lightbox)

セルの再利用を簡略化された`UICollectionView`と`UITableView`します。 1 つでない場合、再利用するキューで利用可能なセルがシステムに登録されているデータ ソースに直接セルを作成する必要がなくなります。 キューから削除して再利用のキューからのセルへの呼び出しを行うときにセルが使用できない iOS 型または登録された nib に基づいて自動的に作成されます。
同じ手法も補助ビューを使用できます。

たとえば、次のコードを登録する、`AnimalCell`クラス。

```csharp
static NSString animalCellId = new NSString ("AnimalCell");
CollectionView.RegisterClassForCell (typeof(AnimalCell), animalCellId);
```

ときに、`UICollectionView`その項目が、画面上にあるために、セルを必要があります、`UICollectionView`呼び出し、データ ソースの`GetCell`メソッド。 UITableView のしくみと同様に、このメソッドは、可能性のあるバックアップ データからのセルを構成するため、`AnimalCell`ここでクラスします。

次のコードの実装を示します`GetCell`を返す、`AnimalCell`インスタンス。

```csharp
public override UICollectionViewCell GetCell (UICollectionView collectionView, Foundation.NSIndexPath indexPath)
{
        var animalCell = (AnimalCell)collectionView.DequeueReusableCell (animalCellId, indexPath);

        var animal = animals [indexPath.Row];

        animalCell.Image = animal.Image;

        return animalCell;
}
```

呼び出し`DequeReusableCell`はセルがキューへの呼び出しで登録された型に基づいて作成で使用できない場合または、セルするか、解除キューに入れ、再利用するキューから、`CollectionView.RegisterClassForCell`します。

この場合、登録することによって、`AnimalCell`クラス iOS を新規に作成されます`AnimalCell`内部的にして返すデキュー セルへの呼び出しが行われたときに、animal クラスに含まれているし、表示用、に返されるイメージが構成されている後`UICollectionView`.

 <a name="Delegate" />


### <a name="delegate"></a>Delegate

`UICollectionView`クラス型のデリゲートを使用して`UICollectionViewDelegate`内のコンテンツとの対話をサポートするために、`UICollectionView`します。 これにより制御できます。

-  **選択範囲のセル**– セルが選択されているかどうかを決定します。
-  **セルの強調表示**– セルが現在変更されているかどうかを決定します。
-  **セルのメニュー** – を長押しジェスチャに応答内のセルに対して表示されるメニューです。


データ ソースと同様、`UICollectionViewController`のデリゲートを使用する既定の構成、`UICollectionView`します。

 <a name="Cell_HighLighting" />


#### <a name="cell-highlighting"></a>セルが強調表示

セルを押すと、セルの強調表示された状態に遷移して、までが選択されていないときに、ユーザーがセルから指を離したします。 実際に選択されている前に、セルの外観における一時的な変更ができます。 選択するときに、セルの`SelectedBackgroundView`が表示されます。 次の図は、選択範囲が発生した直前に強調表示された状態を示しています。

 [![](uicollectionview-images/04-cell-highlight.png "この図は、選択範囲が発生した直前に強調表示された状態を示しています")](uicollectionview-images/04-cell-highlight.png#lightbox)

強調表示を実装するために、`ItemHighlighted`と`ItemUnhighlighted`のメソッド、`UICollectionViewDelegate`ことができます。 たとえば、次のコードがの黄色の背景色を適用、`ContentView`セルが強調表示すると、上記の図のように強調、されていないときに背景が白。

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

既定で選択が有効になっている`UICollectionView`します。 選択を無効にするにはオーバーライド`ShouldHighlightItem`次に示すように false を返します。

```csharp
public override bool ShouldHighlightItem (UICollectionView collectionView, NSIndexPath indexPath)
{
        return false;
}
```

強調表示を無効にすると、セルを選択するプロセスも無効です。 さらがありますが、`ShouldSelectItem`メソッドが直接、選択範囲を制御する場合は`ShouldHighlightItem`は実装されており、false を返し、`ShouldSelectItem`は呼び出されません。

 `ShouldSelectItem` オンまたはオフに、項目単位で有効にする選択が可能と`ShouldHighlightItem`は実装されていません。 また、選択した場合、なしの強調表示場合`ShouldHighlightItem`は実装されており、中に、true を返します`ShouldSelectItem`false を返します。

 <a name="Cell_Menus" />


#### <a name="cell-menus"></a>セルのメニュー

内の各セルを`UICollectionView`切り取り、コピー、およびオプションでサポートされなければならない貼り付けを許可するメニューを表示できません。 セルに、[編集] メニューを作成します。

1.  オーバーライド`ShouldShowMenu`し、項目がメニューを表示する必要がある場合に true を返します。
1.  オーバーライド`CanPerformAction`切り取り、コピー、貼り付けのいずれかになるアクションごとに、項目が実行できる場合は true を返します。
1.  オーバーライド`PerformAction`編集を実行するのには、貼り付け操作のコピーします。


次のスクリーン ショットは、セルが時間の長い押されたときにメニューを表示します。

 [![](uicollectionview-images/04a-menu.png "このスクリーン ショットは、セルが時間の長い押されたときにメニューを表示します。")](uicollectionview-images/04a-menu.png#lightbox)

 <a name="Layout" />


## <a name="layout"></a>レイアウト

`UICollectionView` レイアウト システムにより、すべての要素、独立して管理するには、セル、補助ビューおよび装飾ビューの位置をサポート、`UICollectionView`自体。
レイアウト システムを使用して、アプリケーションはこの記事で説明しただけでなくカスタム レイアウトを提供するグリッドのようなものなどのレイアウトをサポートできます。

 <a name="Layout_Basics" />


### <a name="layout-basics"></a>レイアウトの基礎

レイアウトを`UICollectionView`から継承するクラスで定義されて`UICollectionViewLayout`します。 各項目のレイアウト属性を作成するため、レイアウトの実装は、`UICollectionView`します。 レイアウトを作成する 2 つの方法はあります。

-  組み込みの使用、`UICollectionViewFlowLayout`します。
-  継承することによってカスタム レイアウトを提供`UICollectionViewLayout`します。


 <a name="Flow_Layout" />


### <a name="flow-layout"></a>フロー レイアウト

`UICollectionViewFlowLayout`クラスは、前述したようにセルのグリッド内のコンテンツを配置するときにその適切な行ベースのレイアウトを提供します。

フロー レイアウトを使用します。

-  インスタンスを作成`UICollectionViewFlowLayout`:


```csharp
var layout = new UICollectionViewFlowLayout ();
```

-  インスタンスのコンス トラクターに渡す、 `UICollectionView` :


```csharp
simpleCollectionViewController = new SimpleCollectionViewController (layout);
```

これは、すべてのグリッド内のコンテンツをレイアウトするために必要です。 また、変更すると、印刷の向き、`UICollectionViewFlowLayout`処理内容を次に示すよう、適切に再配置。

 [![](uicollectionview-images/05-layout-orientation.png "向きの変更の例")](uicollectionview-images/05-layout-orientation.png#lightbox)

 <a name="Section_Inset" />


#### <a name="section-inset"></a>セクションの埋め込み

周囲のいくつかの領域を提供する、 `UIContentView`、レイアウトが、`SectionInset`型のプロパティ`UIEdgeInsets`します。 たとえば、次のコードはの各セクションを 50 ピクセル バッファーを提供します、`UIContentView`によって配置されたときに、 `UICollectionViewFlowLayout`:。

```csharp
var layout = new UICollectionViewFlowLayout ();
layout.SectionInset = new UIEdgeInsets (50,50,50,50);
```

これは、次に示すように、セクションの前後にスペースが得られます。

 [![](uicollectionview-images/06-sectioninset.png "次に示すように、セクションに関する間隔")](uicollectionview-images/06-sectioninset.png#lightbox)

 <a name="Subclassing_UICollectionViewFlowLayout" />


#### <a name="subclassing-uicollectionviewflowlayout"></a>UICollectionViewFlowLayout をサブクラス化

使用するエディションで`UICollectionViewFlowLayout`を直接この属性は線に沿ってコンテンツのレイアウトをさらにカスタマイズすることもサブクラスことができます。 たとえば、次に示すように、グリッドのセルをラップしませんが、代わりに水平方向のスクロール効果を持つ 1 つの行を作成するレイアウトを作成するこの使用できます。

 [![](uicollectionview-images/07-line-layout.png "水平方向スクロール効果を持つ 1 つの行")](uicollectionview-images/07-line-layout.png#lightbox)

サブクラス化してこれを実装する`UICollectionViewFlowLayout`が必要です。

-  レイアウトの種類に適用される任意のレイアウト プロパティまたはコンス トラクターでレイアウトのすべての項目を初期化しています。
-  オーバーライドする`ShouldInvalidateLayoutForBoundsChange`返す、これを true の境界、`UICollectionView`セルのレイアウトの変更は再計算されます。 これは、使用 centermost セルに適用する変換は、スクロール中に適用されますこの場合、コードを確認します。
-  オーバーライドする`TargetContentOffset`、centermost するセルの中央へのスナップ、`UICollectionView`としてスクロールが停止します。
-  オーバーライドする`LayoutAttributesForElementsInRect`の配列を返す`UICollectionViewLayoutAttributes`します。 各`UICollectionViewLayoutAttribute`プロパティなど、特定の項目をレイアウトする方法に関する情報が含まれています、 `Center` 、 `Size` 、`ZIndex`と`Transform3D`します。


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

使用するだけでなく`UICollectionViewFlowLayout`、レイアウトにから直接継承することによって完全にカスタマイズできます`UICollectionViewLayout`します。

キーを上書きする方法を示します。

-   `PrepareLayout` – レイアウト プロセス全体で使用される初期の幾何学的計算を実行するために使用します。
-   `CollectionViewContentSize` – コンテンツを表示するための領域のサイズを返します。
-   `LayoutAttributesForElementsInRect` – として、このメソッドの使用情報を提供する前に示した UICollectionViewFlowLayout の例で、`UICollectionView`レイアウトする方法について各項目。 ただしとは異なり、 `UICollectionViewFlowLayout` 、カスタム レイアウトを作成する場合は、選択したアイテムを配置することができます。


たとえば、次に示すようは、同じコンテンツを循環レイアウトで表示できます。

 [![](uicollectionview-images/08-circle-layout.png "循環カスタム レイアウトを次に示すよう")](uicollectionview-images/08-circle-layout.png#lightbox)

レイアウトについての強力なもの、グリッドのようなレイアウトから水平方向のスクロール レイアウトに変更することし、その後この循環レイアウトに提供されるレイアウト クラスのみが必要です、`UICollectionView`を変更します。 は nothing、 `UICollectionView`、そのデリゲートまたはデータ、コードの変更がすべてのソースします。


## <a name="changes-in-ios-9"></a>IOS 9 での変更


IOS 9、コレクション ビューで (`UICollectionView`) がサポートされますが、新しい既定のジェスチャ認識エンジンおよびいくつかの新しいサポート メソッドを追加することで、ボックスから項目を並べ替えをドラッグするようになりました。

これらの新しいメソッドを使用して、ドラッグすると、コレクション ビューで順序を変更し、順序変更プロセスのいずれかの段階中にアイテムの外観のカスタマイズのオプションを簡単に実装することができます。

[![](uicollectionview-images/intro01.png "順序変更プロセスの例")](uicollectionview-images/intro01.png#lightbox)

この記事では、ドラッグの順序を変更するを実装する Xamarin.iOS アプリケーションだけでなく、他の iOS 9 は、コレクション ビュー コントロールに加えられた変更の一部を見てをみましょう。

- [項目を簡単に並べ替え](#Easy-Reordering-of-Items)
    - [単純な並べ替えの例](#Simple-Reordering-Example)
    - [カスタムのジェスチャ認識エンジンの使用](#Using-a-Custom-Gesture-Recognizer)
    - [カスタム レイアウト、および並べ替え](#Custom-Layouts-and-Reording)
- [コレクションの変更の表示](#collection-view-changes)

<a name="Easy-Reordering-of-Items" />

## <a name="reordering-of-items"></a>項目を並べ替え

前述のように、iOS 9 のコレクション ビューに最も重要な変更のいずれかの簡単なドラッグの順序を変更する機能をすぐにしました。

コレクション ビューの並べ替えを追加する最も簡単な方法は、iOS 9 を使用する、`UICollectionViewController`します。
コレクション ビュー コント ローラーのようになりましたが、`InstallsStandardGestureForInteractiveMovement`プロパティで、標準的なを追加します。*ジェスチャ認識エンジン*コレクション内の項目の順序を変更するドラッグをサポートします。
既定値は、ため`true`、のみを実装する必要がある、`MoveItem`のメソッド、`UICollectionViewDataSource`ドラッグの並べ替えをサポートするクラス。 例:

```csharp
public override void MoveItem (UICollectionView collectionView, NSIndexPath sourceIndexPath, NSIndexPath destinationIndexPath)
{
    // Reorder our list of items
    ...
}
```
<a name="Simple-Reordering-Example" />

### <a name="simple-reordering-example"></a>単純な並べ替えの例

簡単な例として、新しい Xamarin.iOS プロジェクトを起動し、編集、 **Main.storyboard**ファイル。 ドラッグ、`UICollectionViewController`デザイン サーフェイスに。

[![](uicollectionview-images/quick01.png "UICollectionViewController を追加します。")](uicollectionview-images/quick01.png#lightbox)

(これは、ドキュメント アウトラインから実行する最も簡単なことでもかまいません)、コレクション ビューを選択します。 Properties Pad の [レイアウト] タブでは、次のスクリーン ショットに示すように、次のサイズを設定します。

- **セル サイズ**:Width: 60 |Height: 60
- **ヘッダー サイズ**:Width: 0 |Height: 0
- **フッターのサイズ**:Width: 0 |Height: 0
- **最小間隔**:セル – 8 |行: 8
- **セクションのくぼみ**:Top: 16 |下部にある – 16 |Left: 16 |右 – 16

[![](uicollectionview-images/quick04.png "コレクション ビューのサイズを設定します。")](uicollectionview-images/quick04.png#lightbox)

次に、既定のセルを編集します。
    - その背景色を青に変更します。
    - セルのタイトルとして機能するラベルを追加します。
    - 再利用の識別子を設定**セル**

[![](uicollectionview-images/quick02.png "既定値のセルを編集します。")](uicollectionview-images/quick02.png#lightbox)

サイズが変更されると、セル内で中央にラベルを保持する制約を追加します。

**プロパティ パッド**の_CollectionViewCell_設定と、**クラス**に`TextCollectionViewCell`:

[![](uicollectionview-images/quick05.png "クラスを TextCollectionViewCell に設定します。")](uicollectionview-images/quick05.png#lightbox)

設定、**コレクションの再利用可能なビュー**に`Cell`:

[![](uicollectionview-images/quick06.png "セルにコレクションの再利用可能なビューを設定します。")](uicollectionview-images/quick06.png#lightbox)

最後に、ラベルを選択し、名前を付けます`TextLabel`:

[![](uicollectionview-images/quick07.png "名前ラベル TextLabel")](uicollectionview-images/quick07.png#lightbox)

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

ここで、`Text`ラベルのプロパティが、セルのタイトルとして公開されるため、コードから設定できます。

新しい追加C#をプロジェクトにクラスし、それを呼び出す`WaterfallCollectionSource`します。 ファイルを編集し、次のようになります。

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

このクラスは、コレクション ビューのデータ ソースにして、コレクション内の各セルの情報を提供します。
なお、`MoveItem`メソッドを実装するコレクション内の項目のドラッグの並べ替えられたを許可します。

もう 1 つの新しい追加C#をプロジェクトにクラスし、それを呼び出す`WaterfallCollectionDelegate`します。 このファイルを編集し、次のようになります。

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

これは、コレクション ビューのデリゲートとして機能します。 コレクション ビューにユーザーと対話して、セルを強調表示するメソッドがオーバーライドされました。

最後の 1 つの追加C#をプロジェクトにクラスし、それを呼び出す`WaterfallCollectionView`します。 このファイルを編集し、次のようになります。

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

注意`DataSource`と`Delegate`前に作成したコレクション ビューがそのストーリー ボードから構築されたときに設定されます (または **.xib**ファイル)。

編集、 **Main.storyboard**ファイルを再びとコレクション ビューを選択し、切り替える、**プロパティ**します。 設定、**クラス**をカスタム`WaterfallCollectionView`上で定義したクラス。

UI に加えた変更を保存し、アプリを実行します。
ユーザーは、一覧から項目を選択しを新しい場所にドラッグ、項目の移動に他の項目が自動的にアニメーションするは。
ユーザーが新しい場所に項目をドロップ、ときに、その場所を引き続き使用します。 例:

[![](uicollectionview-images/intro01.png "新しい場所に項目をドラッグする例")](uicollectionview-images/intro01.png#lightbox)

<a name="Using-a-Custom-Gesture-Recognizer" />

### <a name="using-a-custom-gesture-recognizer"></a>カスタムのジェスチャ認識エンジンの使用

使用できない場合、`UICollectionViewController`標準を使用する必要がありますと`UIViewController`、またはドラッグ アンド ドロップ ジェスチャの詳細に制御を実行する場合は、独自のカスタム ジェスチャ レコグナイザーを作成し、ビューが読み込まれるコレクション ビューに追加します。 例:

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

ここで実装し、ドラッグ操作を制御するコレクション ビューに追加されたいくつかの新しいメソッドを使用しているしました。

 - `BeginInteractiveMovementForItem` -移動操作の開始をマークします。
 - `UpdateInteractiveMovementTargetPosition` -項目の位置が更新されると、送信です。
 - `EndInteractiveMovement` 項目の移動の末尾をマークします。
 - `CancelInteractiveMovement` -ユーザーの移動操作をキャンセルをマークします。

アプリケーションを実行すると、ドラッグ操作は、既定のコレクション ビューに付属しているジェスチャ レコグナイザーをドラッグするとまったく同じ動作が。

<a name="Custom-Layouts-and-Reording" />

### <a name="custom-layouts-and-reordering"></a>カスタム レイアウト、および並べ替え

Ios 9 では、いくつかの新しいメソッドがコレクション ビューでのドラッグの順序を変更して、カスタム レイアウトを使用する追加されました。 この機能を参照するには、コレクションにカスタム レイアウトを追加してみましょう。

最初に、新しい追加C#と呼ばれるクラス`WaterfallCollectionLayout`をプロジェクトにします。 編集し、次のようになります。

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

これは、カスタムの 2 つの列にウォーター フォール型レイアウト、コレクション ビューを提供するクラスを使用します。
キー値コーディングを使用して (を使用して、`WillChangeValue`と`DidChangeValue`メソッド) をこのクラスで、計算されたプロパティのデータ バインディングを提供します。

次に、編集、`WaterfallCollectionSource`し、次の変更と追加します。

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

これにより、それぞれの一覧に表示される項目のランダムな高さが作成されます。

次に、編集、`WaterfallCollectionView`クラスし、ヘルパーの次のプロパティを追加します。

```csharp
public WaterfallCollectionSource Source {
    get { return (WaterfallCollectionSource)DataSource; }
}
```

これは、ため、カスタム レイアウトからのデータ ソース (および項目の高さ) を取得しやすく、されます。

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

各項目のサイズを指定するイベントを設定、および、コレクション ビューに新しいレイアウトをアタッチします。 この、カスタム レイアウトのインスタンスを作成します。

Xamarin.iOS アプリをもう一度実行している場合、コレクション ビューは次のようになります。

[![](uicollectionview-images/custom01.png "コレクション ビューは次のようになります")](uicollectionview-images/custom01.png#lightbox)

まだ並べ替え-ドラッグ - 項目としてする前に、できるが、項目が削除すると、その新しい場所に合わせてサイズを変更するようになりました。

## <a name="collection-view-changes"></a>コレクションの変更の表示

次のセクションでは、iOS 9 で各コレクション ビュー クラスに加えられた変更についての詳細をみましょう。

### <a name="uicollectionview"></a>UICollectionView

次の変更または追加機能に加え、 `UICollectionView` iOS 9 のクラス。

 - `BeginInteractiveMovementForItem` – ドラッグ操作の開始をマークします。
 - `CancelInteractiveMovement` – コレクションの通知、ユーザーがドラッグ操作をキャンセルを表示します。
 - `EndInteractiveMovement` – コレクションの通知、ユーザーがドラッグ操作を完了したことを表示します。
 - `GetIndexPathsForVisibleSupplementaryElements` – を返します、`indexPath`のヘッダーまたはフッター コレクション ビューのセクションでします。
 - `GetSupplementaryView` – 特定のヘッダーまたはページ フッターを返します。
 - `GetVisibleSupplementaryViews` – すべての表示のヘッダーとフッターの一覧を返します。
 - `UpdateInteractiveMovementTargetPosition` – コレクションの通知、ユーザーが移動されたか、移動は、ドラッグ操作中にアイテムを表示します。

### <a name="uicollectionviewcontroller"></a>UICollectionViewController

次の変更または追加機能に加え、 `UICollectionViewController` iOS 9 のクラス。

 - `InstallsStandardGestureForInteractiveMovement` –`true`自動的にドラッグの並べ替えをサポートしている新しいジェスチャ認識エンジンが使用されます。
 - `CanMoveItem` – 特定の項目がドラッグの順序を変更できる場合は、コレクション ビューに通知します。
 - `GetTargetContentOffset` – 特定のコレクション ビューのアイテムのオフセットを取得するために使用します。
 - `GetTargetIndexPathForMove` 取得、`indexPath`のドラッグ操作の特定の項目。
 - `MoveItem` – 特定の項目の順序を一覧に移動します。


### <a name="uicollectionviewdatasource"></a>UICollectionViewDataSource

次の変更または追加機能に加え、 `UICollectionViewDataSource` iOS 9 のクラス。

 - `CanMoveItem` – 特定の項目がドラッグの順序を変更できる場合は、コレクション ビューに通知します。
 - `MoveItem` – 特定の項目の順序を一覧に移動します。

### <a name="uicollectionviewdelegate"></a>UICollectionViewDelegate

次の変更または追加機能に加え、 `UICollectionViewDelegate` iOS 9 のクラス。

 - `GetTargetContentOffset` – 特定のコレクション ビューのアイテムのオフセットを取得するために使用します。
 - `GetTargetIndexPathForMove` 取得、`indexPath`のドラッグ操作の特定の項目。

### <a name="uicollectionviewflowlayout"></a>UICollectionViewFlowLayout

次の変更または追加機能に加え、 `UICollectionViewFlowLayout` iOS 9 のクラス。

 - `SectionFootersPinToVisibleBounds` – セクション フッターが表示されるコレクション ビューの範囲に戻らない。
 - `SectionHeadersPinToVisibleBounds` – スティック セクション ヘッダーに表示されるコレクション ビューの境界。

### <a name="uicollectionviewlayout"></a>UICollectionViewLayout

次の変更または追加機能に加え、 `UICollectionViewLayout` iOS 9 のクラス。

 - `GetInvalidationContextForEndingInteractiveMovementOfItems` – ユーザーは、ドラッグを終了するかキャンセルすると、ドラッグ操作の最後に、無効化のコンテキストを返します。
 - `GetInvalidationContextForInteractivelyMovingItems` – ドラッグ操作の開始時の無効化のコンテキストを返します。
 - `GetLayoutAttributesForInteractivelyMovingItem` – 項目をドラッグするときに特定の項目のレイアウト属性を取得します。
 - `GetTargetIndexPathForInteractivelyMovingItem` – を返します、`indexPath`の項目をドラッグしたときに特定の時点になっている項目。

### <a name="uicollectionviewlayoutattributes"></a>UICollectionViewLayoutAttributes

次の変更または追加機能に加え、 `UICollectionViewLayoutAttributes` iOS 9 のクラス。

 - `CollisionBoundingPath` – ドラッグ操作中に 2 つの項目の衝突パスを返します。
 - `CollisionBoundsType` – 衝突の型を返します (として、 `UIDynamicItemCollisionBoundsType`) が、ドラッグ操作中に発生しました。

### <a name="uicollectionviewlayoutinvalidationcontext"></a>UICollectionViewLayoutInvalidationContext

次の変更または追加機能に加え、 `UICollectionViewLayoutInvalidationContext` iOS 9 のクラス。

 - `InteractiveMovementTarget` – ドラッグ操作のターゲット項目を返します。
 - `PreviousIndexPathsForInteractivelyMovingItems` – を返します、`indexPaths`ドラッグすると、操作の順序を変更に関連するその他の項目。
 - `TargetIndexPathsForInteractivelyMovingItems` – を返します、`indexPaths`の項目をドラッグの順序を変更する操作の結果として並べ替えられます。

### <a name="uicollectionviewsource"></a>UICollectionViewSource

次の変更または追加機能に加え、 `UICollectionViewSource` iOS 9 のクラス。

 - `CanMoveItem` – 特定の項目がドラッグの順序を変更できる場合は、コレクション ビューに通知します。
 - `GetTargetContentOffset` – ドラッグの順序を変更する操作で移動するアイテムのオフセットを返します。
 - `GetTargetIndexPathForMove` – を返します、`indexPath`のドラッグの順序を変更する操作中に移動する項目。
 - `MoveItem` – 特定の項目の順序を一覧に移動します。

## <a name="summary"></a>まとめ

この記事でが iOS 9 のコレクション ビューへの変更について説明し、Xamarin.iOS でそれらを実装する方法について説明しました。
コレクション ビューでの単純なドラッグの順序を変更するアクションの実装を説明しました並べ替え-ドラッグ; でのカスタム ジェスチャ レコグナイザーの使用およびドラッグの順序を変更するが、カスタム コレクション ビューのレイアウトにどのように影響するか。

## <a name="related-links"></a>関連リンク

- [iOS 9 のサンプル](https://developer.xamarin.com/samples/ios/iOS9/)
- [コレクション ビューのサンプル](https://developer.xamarin.com/samples/monotouch/ios9/CollectionView/)
- [SimpleCollectionView (サンプル)](https://developer.xamarin.com/samples/monotouch/SimpleCollectionView/)
- [イベント、プロトコル、デリゲート](~/ios/app-fundamentals/delegates-protocols-and-events.md)
- [テーブルおよびセルの使用](~/ios/user-interface/controls/tables/index.md)
