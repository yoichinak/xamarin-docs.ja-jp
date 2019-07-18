---
title: カスタム レイアウトの作成
description: この記事では、カスタム レイアウト クラスを作成する方法と、ページ間で、その子を水平方向に整列し、追加の行に後続の子の表示をラップし、印刷の向きを区別する. WrapLayout クラスを説明します。
ms.prod: xamarin
ms.assetid: B0CFDB59-14E5-49E9-965A-3DCCEDAC2E31
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/29/2017
ms.openlocfilehash: 56f7a5308d15425bdedd7d9098882a072d90d1f7
ms.sourcegitcommit: 864f47c4f79fa588b65ff7f721367311ff2e8f8e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/24/2019
ms.locfileid: "64347053"
---
# <a name="creating-a-custom-layout"></a>カスタム レイアウトの作成

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/CustomLayout/WrapLayout/)

_Xamarin.Forms – StackLayout、AbsoluteLayout、[相対レイアウト]、およびグリッドの 4 つのレイアウト クラスを定義し、別の方法でその子を配置それぞれします。ただし、場合によっては必要 Xamarin.Forms が提供していないレイアウトを使用してページのコンテンツを整理します。この記事では、カスタム レイアウト クラスを作成する方法と、ページ間で、その子を水平方向に整列し、追加の行に後続の子の表示をラップし、印刷の向きを区別する. WrapLayout クラスを説明します。_

## <a name="overview"></a>概要

Xamarin.Forms でから派生してレイアウトのすべてのクラス、 [ `Layout<T>` ](xref:Xamarin.Forms.Layout`1)クラスおよびをジェネリック型を制約する[ `View` ](xref:Xamarin.Forms.View)とその派生型。 さらに、`Layout<T>`クラスから派生、 [ `Layout` ](xref:Xamarin.Forms.Layout)クラスを要素の位置とサイズ変更の子のメカニズムを提供します。

すべての視覚要素と呼ばれる独自の推奨されるサイズを判断する、*要求*サイズ。 [`Page`](xref:Xamarin.Forms.Page)、 [ `Layout` ](xref:Xamarin.Forms.Layout)、および[ `Layout<View>` ](xref:Xamarin.Forms.Layout`1)派生型は、子、または自身の基準とした、子のサイズと場所を決定する責任を負います。 そのため、レイアウトが、親では、その子のサイズにする必要がありますを決定、親子リレーションシップが含まれますが、子の要求されたサイズに合わせて、ましょう。

カスタム レイアウトを作成するには、Xamarin.Forms のレイアウトと無効化サイクルの理解が必要です。 これらのサイクルで説明するようになりました。

## <a name="layout"></a>レイアウト

Layout は、ページのビジュアル ツリーの上部から始まり、ページ上のすべての視覚要素を網羅するためにビジュアル ツリーのすべての分岐を進みます。 その他の要素の親である要素は、相対自体の子を配置してサイズ変更を行います。

[ `VisualElement` ](xref:Xamarin.Forms.VisualElement)クラスを定義、 [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags))レイアウトの操作の要素を測定するメソッドと[ `Layout` ](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle))メソッドを指定します。要素の四角形の領域内でレンダリングされます。 アプリケーションが起動し、最初のページが表示されます、*レイアウト サイクル*で構成される最初の`Measure`呼び出し、し`Layout`呼び出しを開始で、 [ `Page` ](xref:Xamarin.Forms.Page)オブジェクト。

1. レイアウト サイクルでは、すべての親要素が通話を担当、`Measure`その子のメソッド。
1. 呼び出し元の親のすべての要素は子が測定された後、`Layout`その子のメソッド。

このサイクルにより、ページ上のすべての visual 要素がへの呼び出しを受信するようになります、`Measure`と`Layout`メソッド。 プロセスは、次の図に示されます。

![](custom-images/layout-cycle.png "Xamarin.Forms のレイアウト サイクル")

> [!NOTE]
> レイアウトに影響する変更があった場合レイアウト サイクルにも、ビジュアル ツリーのサブセットに発生することに注意してください。 項目追加や削除などのコレクションからにはこれが含まれています、 [ `StackLayout`](xref:Xamarin.Forms.StackLayout)での変更、 [ `IsVisible` ](xref:Xamarin.Forms.VisualElement.IsVisible) 、要素または要素のサイズの変更のプロパティ。

すべての Xamarin.Forms クラスを持つ、`Content`または`Children`プロパティが、オーバーライド可能な[ `LayoutChildren` ](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double))メソッド。 派生するクラスをカスタム レイアウト[ `Layout<View>` ](xref:Xamarin.Forms.Layout`1)このメソッドをオーバーライドしていることを確認する必要があります、 [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags))と[ `Layout` ](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle))メソッド目的のカスタム レイアウトを提供する、すべての要素の子で呼び出されます。

さらに、すべてのクラスから派生した[ `Layout` ](xref:Xamarin.Forms.Layout)または[ `Layout<View>` ](xref:Xamarin.Forms.Layout`1)オーバーライドする必要があります、 [ `OnMeasure` ](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double))メソッドで、レイアウトのクラスには呼び出すことであるために必要なサイズを決定、 [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags))その子のメソッド。

> [!NOTE]
> 要素に基づくサイズを決定する*制約*領域の量は、要素の親内の要素の使用を示します。 渡される制約、 [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags))と[ `OnMeasure` ](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double))メソッドの範囲は 0 ~`Double.PositiveInfinity`します。 要素が*制約付き*、または*が完全に拘束*への呼び出しを受信すると、その[ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags))メソッドの有限の引数を持つ要素が制限されます特定のサイズ。 要素が*制約のない*、または*部分的に制約付き*への呼び出しを受信すると、その`Measure`と等しく、少なくとも 1 つの引数を持つメソッド`Double.PositiveInfinity`– 無限の制約として使用できます。自動サイズ調整を示すとして考えることです。

## <a name="invalidation"></a>無効化

Invalidation は、ページ上の要素の変更によって新しいレイアウトサイクルを引き起こさせるための処理です。 要素は、サイズと位置が正しくなくなった時に、無効と見なされます。 たとえば場合、 [ `FontSize` ](xref:Xamarin.Forms.Button.FontSize)のプロパティを[ `Button` ](xref:Xamarin.Forms.Button) 、変更、`Button`適切なサイズがある不要になったため、無効にするといいます。 サイズ変更、`Button`残りのページ レイアウトの変更の波及効果をし、必要があります。

要素を無効に自体を呼び出すことによって、 [ `InvalidateMeasure` ](xref:Xamarin.Forms.VisualElement.InvalidateMeasure)メソッドでは、一般に、要素のプロパティが変更されたとき、要素の新しいサイズを可能性があります。 このメソッドが発生した、 [ `MeasureInvalidated` ](xref:Xamarin.Forms.VisualElement.MeasureInvalidated)イベントで、新しいレイアウト サイクルをトリガーする要素の親を処理します。

[ `Layout` ](xref:Xamarin.Forms.Layout)クラスのハンドラーを設定する、 [ `MeasureInvalidated` ](xref:Xamarin.Forms.VisualElement.MeasureInvalidated)イベントに追加されたすべての子をその`Content`プロパティまたは`Children`コレクションし、ハンドラーをデタッチしますときに、。子が削除されます。 そのため、子を持つビジュアル ツリー内のすべての要素はその子のいずれかのサイズを変更するたびに通知します。 次の図は、ビジュアル ツリー内の要素のサイズの変更が、ツリーのリップル変更を発生する方法を示しています。

![](custom-images/invalidation.png "ビジュアル ツリー内の無効化")

ただし、`Layout`クラスは、ページのレイアウトに子のサイズ変更の影響を制限しようとしています。 レイアウトがサイズの制約付きの場合は、し、子のサイズ変更には影響しませんビジュアル ツリーの親のレイアウトより高い値です。 ただし、通常、レイアウトのサイズの変更に影響を与えますレイアウトがその子に配置します。 そのため、レイアウトのサイズの変更には、レイアウトのレイアウト サイクルが起動し、レイアウトへの呼び出しを受け取ります。 その[ `OnMeasure` ](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double))と[ `LayoutChildren` ](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double))メソッド。

[ `Layout` ](xref:Xamarin.Forms.Layout)クラスも定義、 [ `InvalidateLayout` ](xref:Xamarin.Forms.Layout.InvalidateLayout)メソッドへのような目的を持つ、 [ `InvalidateMeasure` ](xref:Xamarin.Forms.VisualElement.InvalidateMeasure)メソッド。 `InvalidateLayout`レイアウトの配置し、その子のサイズに影響する変更が加えられるたびに、メソッドを呼び出す必要があります。 たとえば、`Layout`クラスを呼び出す、`InvalidateLayout`メソッドの子が追加またはレイアウトから削除されるたびにします。

[ `InvalidateLayout` ](xref:Xamarin.Forms.Layout.InvalidateLayout)の反復的な呼び出しを最小限に抑えるのキャッシュ実装をオーバーライドする、 [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags))レイアウトの子のメソッド。 オーバーライドする、`InvalidateLayout`メソッドの子を追加またはレイアウトから削除するときに通知を提供します。 同様に、 [ `OnChildMeasureInvalidated` ](xref:Xamarin.Forms.Layout.OnChildMeasureInvalidated)レイアウトの子のいずれかのサイズが変更されたときに通知を提供するメソッドをオーバーライドすることができます。 両方のメソッド オーバーライドのカスタム レイアウト キャッシュをクリアして応答します。 詳細については、次を参照してください。[計算とデータのキャッシュ](#caching)します。

## <a name="creating-a-custom-layout"></a>カスタム レイアウトの作成

カスタム レイアウトを作成するプロセスは次のとおりです。

1. `Layout<View>` クラスから派生するクラスを作成します。 詳細については、次を参照してください。[作成、WrapLayout](#creating)します。
1. [*省略可能な*] レイアウト クラスで設定するパラメーターのバインド可能なプロパティでサポートされるプロパティを追加します。 詳細については、次を参照してください。[プロパティはバインド可能なプロパティでサポートを追加する](#adding_properties)します。
1. 上書き、 [ `OnMeasure` ](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double))メソッドを呼び出す、 [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags))レイアウトのすべての子、および戻り値の要求サイズ、レイアウトのメソッド。 詳細については、次を参照してください。 [OnMeasure メソッドをオーバーライドする](#onmeasure)します。
1. 上書き、 [ `LayoutChildren` ](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double))メソッドを呼び出す、 [ `Layout` ](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle))レイアウトのすべての子のメソッド。 呼び出しに失敗し、 [ `Layout` ](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle))レイアウトでそれぞれの子のメソッドが適切なサイズまたは位置を受信しない子になります、そのため、子なりませんページに表示します。 詳細については、次を参照してください。 [LayoutChildren メソッドをオーバーライドする](#layoutchildren)します。

  > [!NOTE]
>  内の子を列挙するときに、 [ `OnMeasure` ](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double))と[ `LayoutChildren` ](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double))オーバーライド、すべての子のスキップを[ `IsVisible` ](xref:Xamarin.Forms.VisualElement.IsVisible) に設定されて`false`. カスタム レイアウトが非表示の子の余地を残すされないようになります。

1. [*省略可能な*] オーバーライド、 [ `InvalidateLayout` ](xref:Xamarin.Forms.Layout.InvalidateLayout)に子が追加またはレイアウトから削除するときに通知するメソッド。 詳細については、次を参照してください。 [InvalidateLayout メソッドをオーバーライドする](#invalidatelayout)します。
1. [*省略可能な*] オーバーライド、 [ `OnChildMeasureInvalidated` ](xref:Xamarin.Forms.Layout.OnChildMeasureInvalidated)レイアウトの子のいずれかのサイズが変更されたときに通知を受け取るメソッド。 詳細については、次を参照してください。 [OnChildMeasureInvalidated メソッドをオーバーライドする](#onchildmeasureinvalidated)します。

> [!NOTE]
> なお、 [ `OnMeasure` ](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double))レイアウトのサイズがその子ではなく、その親によって拘束される場合、オーバーライドは呼び出されなくなっています。 制約の一方または両方が有限でない場合、またはレイアウトのクラスがある既定ではない場合、オーバーライドが呼び出される、 [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions)または[ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions)プロパティの値。 このため、 [ `LayoutChildren` ](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double))オーバーライドは、中に取得された子のサイズに依存できない、 [ `OnMeasure` ](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double))メソッドの呼び出し。 代わりに、`LayoutChildren`呼び出す必要があります、 [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags))メソッドを呼び出す前に、レイアウトの子を[ `Layout` ](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle))メソッド。 子のサイズを取得する代わりに、`OnMeasure`オーバーライドは、後で回避するためにキャッシュできる`Measure`で呼び出し、`LayoutChildren`のオーバーライドでは、レイアウトのクラスは、サイズをもう一度取得する必要がある場合を把握する必要があります。 詳細については、次を参照してください。[計算とレイアウトのデータをキャッシュ](#caching)します。

レイアウトのクラスに追加することで使用できます、 [ `Page`](xref:Xamarin.Forms.Page)レイアウトに子を追加するとします。 詳細については、次を参照してください。[消費、WrapLayout](#consuming)します。

<a name="creating" />

### <a name="creating-a-wraplayout"></a>WrapLayout を作成します。

サンプル アプリケーションでは、印刷の向き区別`WrapLayout`クラスを追加の行に後続の子の表示をラップし、ページ間で、その子を水平方向に整列します。

`WrapLayout`と呼ばれるそれぞれの子クラスが同じ大きさの領域を割り当て、*セル サイズ*子の最大サイズに基づいて、します。 セルのサイズをセル内で配置できるよりも小さい子供がに基づいて、 [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions)と[ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions)プロパティの値。

`WrapLayout`クラス定義が次のコード例に示すようにします。

```csharp
public class WrapLayout : Layout<View>
{
  Dictionary<Size, LayoutData> layoutDataCache = new Dictionary<Size, LayoutData>();
  ...
}
```

<a name="caching" />

#### <a name="calculating-and-caching-layout-data"></a>計算とレイアウトのデータのキャッシュ

`LayoutData`構造体には、さまざまなプロパティで子のコレクションに関するデータを格納します。

- `VisibleChildCount` – レイアウトで表示される子の数。
- `CellSize` – レイアウトのサイズを調整するすべての子の最大サイズ。
- `Rows` -行の数。
- `Columns` -列の数。

`layoutDataCache`フィールドは複数の格納に使用`LayoutData`値。 アプリケーションの起動時、2 つ`LayoutData`にキャッシュされるオブジェクト、 `layoutDataCache` – に対する制約の引数のいずれか、現在の向きのディクショナリ、`OnMeasure`のオーバーライドでは、1 つは、`width`と`height`引数`LayoutChildren`をオーバーライドします。 、横方向にデバイスを回転するときに、`OnMeasure`をオーバーライドし、`LayoutChildren`オーバーライドが再度呼び出される、その結果、別の 2 つ`LayoutData`ディクショナリにキャッシュされているオブジェクト。 ただし、縦向きにデバイスを返すときにそれ以上の計算は必要ありませんので、`layoutDataCache`既に必要なデータ。

次のコード例は、`GetLayoutData`のプロパティを計算するメソッド、`LayoutData`構造化された特定のサイズに基づいています。

```csharp
LayoutData GetLayoutData(double width, double height)
{
  Size size = new Size(width, height);

  // Check if cached information is available.
  if (layoutDataCache.ContainsKey(size))
  {
    return layoutDataCache[size];
  }

  int visibleChildCount = 0;
  Size maxChildSize = new Size();
  int rows = 0;
  int columns = 0;
  LayoutData layoutData = new LayoutData();

  // Enumerate through all the children.
  foreach (View child in Children)
  {
    // Skip invisible children.
    if (!child.IsVisible)
      continue;

    // Count the visible children.
    visibleChildCount++;

    // Get the child's requested size.
    SizeRequest childSizeRequest = child.Measure(Double.PositiveInfinity, Double.PositiveInfinity);

    // Accumulate the maximum child size.
    maxChildSize.Width = Math.Max(maxChildSize.Width, childSizeRequest.Request.Width);
    maxChildSize.Height = Math.Max(maxChildSize.Height, childSizeRequest.Request.Height);
  }

  if (visibleChildCount != 0)
  {
    // Calculate the number of rows and columns.
    if (Double.IsPositiveInfinity(width))
    {
      columns = visibleChildCount;
      rows = 1;
    }
    else
    {
      columns = (int)((width + ColumnSpacing) / (maxChildSize.Width + ColumnSpacing));
      columns = Math.Max(1, columns);
      rows = (visibleChildCount + columns - 1) / columns;
    }

    // Now maximize the cell size based on the layout size.
    Size cellSize = new Size();

    if (Double.IsPositiveInfinity(width))
      cellSize.Width = maxChildSize.Width;
    else
      cellSize.Width = (width - ColumnSpacing * (columns - 1)) / columns;

    if (Double.IsPositiveInfinity(height))
      cellSize.Height = maxChildSize.Height;
    else
      cellSize.Height = (height - RowSpacing * (rows - 1)) / rows;

    layoutData = new LayoutData(visibleChildCount, cellSize, rows, columns);
  }

  layoutDataCache.Add(size, layoutData);
  return layoutData;
}
```

`GetLayoutData`メソッドは、次の操作を実行します。

- 計算されるかどうかを判断します`LayoutData`値がキャッシュ内に既にと使用できる場合は、それを返します。
- それ以外の場合、呼び出すすべての子を列挙、`Measure`無限の幅と高さをそれぞれの子のメソッドを子の最大サイズを決定します。
- 行と、必要な列の数を計算しのディメンションに基づく子のセルのサイズを計算、表示されている少なくとも 1 つの子があること、`WrapLayout`します。 セルのサイズは、子の最大サイズよりも少し広く通常がいる場合もありますより小さい場合、`WrapLayout`の最も高い子広い子または高さが十分な幅します。
- 新しい格納`LayoutData`キャッシュ内の値。

<a name="adding_properties" />

#### <a name="adding-properties-backed-by-bindable-properties"></a>バインド可能なプロパティでサポートされるプロパティを追加します。

`WrapLayout`クラス定義`ColumnSpacing`と`RowSpacing`プロパティ、値がレイアウトでは、列の行を区切るために使用して、バインド可能なプロパティによってバックアップされています。 バインド可能なプロパティは次のコード例に示します。

```csharp
public static readonly BindableProperty ColumnSpacingProperty = BindableProperty.Create(
  "ColumnSpacing",
  typeof(double),
  typeof(WrapLayout),
  5.0,
  propertyChanged: (bindable, oldvalue, newvalue) =>
  {
    ((WrapLayout)bindable).InvalidateLayout();
  });

public static readonly BindableProperty RowSpacingProperty = BindableProperty.Create(
  "RowSpacing",
  typeof(double),
  typeof(WrapLayout),
  5.0,
  propertyChanged: (bindable, oldvalue, newvalue) =>
  {
    ((WrapLayout)bindable).InvalidateLayout();
  });
```

各バインド可能なプロパティのプロパティ変更ハンドラーを呼び出す、`InvalidateLayout`新しいレイアウトをトリガーするメソッドのオーバーライドを渡す、`WrapLayout`します。 詳細については、次を参照してください。 [InvalidateLayout メソッドをオーバーライドする](#invalidatelayout)と[OnChildMeasureInvalidated メソッドをオーバーライドする](#onchildmeasureinvalidated)します。

<a name="onmeasure" />

#### <a name="overriding-the-onmeasure-method"></a>OnMeasure メソッドをオーバーライドします。

`OnMeasure`オーバーライドが次のコード例で示すようにします。

```csharp
protected override SizeRequest OnMeasure(double widthConstraint, double heightConstraint)
{
  LayoutData layoutData = GetLayoutData(widthConstraint, heightConstraint);
  if (layoutData.VisibleChildCount == 0)
  {
    return new SizeRequest();
  }

  Size totalSize = new Size(layoutData.CellSize.Width * layoutData.Columns + ColumnSpacing * (layoutData.Columns - 1),
                layoutData.CellSize.Height * layoutData.Rows + RowSpacing * (layoutData.Rows - 1));
  return new SizeRequest(totalSize);
}
```

オーバーライドを呼び出す、`GetLayoutData`メソッドとコンストラクトを`SizeRequest`も考慮しながら、返されるデータ オブジェクト、`RowSpacing`と`ColumnSpacing`プロパティの値。 詳細については、`GetLayoutData`メソッドを参照してください[計算とデータのキャッシュ](#caching)します。

> [!IMPORTANT]
> [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags))と[ `OnMeasure` ](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double))メソッドを返すことによって、無限のディメンションを要求する必要がない、 [ `SizeRequest` ](xref:Xamarin.Forms.SizeRequest)プロパティに設定された値`Double.PositiveInfinity`. ただし、制約の引数の少なくとも 1 つ`OnMeasure`できる`Double.PositiveInfinity`します。

<a name="layoutchildren" />

#### <a name="overriding-the-layoutchildren-method"></a>LayoutChildren メソッドをオーバーライドします。

`LayoutChildren`オーバーライドが次のコード例で示すようにします。

```csharp
protected override void LayoutChildren(double x, double y, double width, double height)
{
  LayoutData layoutData = GetLayoutData(width, height);

  if (layoutData.VisibleChildCount == 0)
  {
    return;
  }

  double xChild = x;
  double yChild = y;
  int row = 0;
  int column = 0;

  foreach (View child in Children)
  {
    if (!child.IsVisible)
    {
      continue;
    }

    LayoutChildIntoBoundingRegion(child, new Rectangle(new Point(xChild, yChild), layoutData.CellSize));
    if (++column == layoutData.Columns)
    {
      column = 0;
      row++;
      xChild = x;
      yChild += RowSpacing + layoutData.CellSize.Height;
    }
    else
    {
      xChild += ColumnSpacing + layoutData.CellSize.Width;
    }
  }
}
```

呼び出しで、上書きを開始、`GetLayoutData`メソッド、およびすべてのサイズし、それぞれの子のセル内の位置に子を列挙します。 呼び出すことによってこれは、 [ `LayoutChildIntoBoundingRegion` ](xref:Xamarin.Forms.Layout.LayoutChildIntoBoundingRegion(Xamarin.Forms.VisualElement,Xamarin.Forms.Rectangle))メソッドに基づいて四角形内の子の位置を使用すると、 [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions)と[ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions)プロパティの値。 これは、子への呼び出しに相当[ `Layout` ](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle))メソッド。

> [!NOTE]
> 渡される四角形に注意してください、`LayoutChildIntoBoundingRegion`メソッドには、子が存在する領域全体が含まれています。

詳細については、`GetLayoutData`メソッドを参照してください[計算とデータのキャッシュ](#caching)します。

<a name="invalidatelayout" />

#### <a name="overriding-the-invalidatelayout-method"></a>InvalidateLayout メソッドをオーバーライドします。

[ `InvalidateLayout` ](xref:Xamarin.Forms.Layout.InvalidateLayout)オーバーライドが呼び出されるの子が追加またはレイアウト、または 1 つを削除するときの`WrapLayout`プロパティの変更の値を次のコード例に示すようにします。

```csharp
protected override void InvalidateLayout()
{
  base.InvalidateLayout();
  layoutInfoCache.Clear();
}
```

オーバーライドでは、レイアウトを無効にし、すべてのキャッシュされたレイアウト情報を破棄します。

> [!NOTE]
> 停止する、 [ `Layout` ](xref:Xamarin.Forms.Layout)クラスを呼び出す、 [ `InvalidateLayout` ](xref:Xamarin.Forms.Layout.InvalidateLayout)子が追加または、レイアウトから削除されるたびに、メソッドのオーバーライド、 [ `ShouldInvalidateOnChildAdded` ](xref:Xamarin.Forms.Layout.ShouldInvalidateOnChildAdded(Xamarin.Forms.View))[ `ShouldInvalidateOnChildRemoved` ](xref:Xamarin.Forms.Layout.ShouldInvalidateOnChildRemoved(Xamarin.Forms.View))メソッド、および戻り値`false`します。 レイアウトのクラスの子の追加または削除されたときに、カスタム プロセスを実装できます。

<a name="onchildmeasureinvalidated" />

#### <a name="overriding-the-onchildmeasureinvalidated-method"></a>OnChildMeasureInvalidated メソッドをオーバーライドします。

[ `OnChildMeasureInvalidated` ](xref:Xamarin.Forms.Layout.OnChildMeasureInvalidated)サイズを変更し、次のコード例に示したレイアウトの子のいずれかと、オーバーライドが呼び出されます。

```csharp
protected override void OnChildMeasureInvalidated()
{
  base.OnChildMeasureInvalidated();
  layoutInfoCache.Clear();
}
```

オーバーライドは、子レイアウトを無効にし、すべてのキャッシュされたレイアウト情報を破棄します。

<a name="consuming" />

### <a name="consuming-the-wraplayout"></a>WrapLayout の使用

`WrapLayout`に配置することでクラスを使用できる、 [ `Page` ](xref:Xamarin.Forms.Page) XAML コードの例を次に示すように派生型。

```xaml
<ContentPage ... xmlns:local="clr-namespace:ImageWrapLayout">
    <ScrollView Margin="0,20,0,20">
        <local:WrapLayout x:Name="wrapLayout" />
    </ScrollView>
</ContentPage>
```

同等の c# コードは、以下に示します。

```csharp
public class ImageWrapLayoutPageCS : ContentPage
{
  WrapLayout wrapLayout;

  public ImageWrapLayoutPageCS()
  {
    wrapLayout = new WrapLayout();

    Content = new ScrollView
    {
      Margin = new Thickness(0, 20, 0, 20),
      Content = wrapLayout
    };
  }
  ...
}
```

子を追加することができますし、`WrapLayout`必要に応じて。 次のコード例に示す[ `Image` ](xref:Xamarin.Forms.Image)要素に追加される、 `WrapLayout`:

```csharp
protected override async void OnAppearing()
{
    base.OnAppearing();

    var images = await GetImageListAsync();
    if (images != null)
    {
        foreach (var photo in images.Photos)
        {
            var image = new Image
            {
                Source = ImageSource.FromUri(new Uri(photo))
            };
            wrapLayout.Children.Add(image);
        }
    }
}

async Task<ImageList> GetImageListAsync()
{
    try
    {
        string requestUri = "https://raw.githubusercontent.com/xamarin/docs-archive/master/Images/stock/small/stock.json";
        string result = await _client.GetStringAsync(requestUri);
        return JsonConvert.DeserializeObject<ImageList>(result);
    }
    catch (Exception ex)
    {
        Debug.WriteLine($"\tERROR: {ex.Message}");
    }

    return null;
}
```

ときに、ページを含む、`WrapLayout`が表示されたら、サンプル アプリケーションは非同期的に写真のリストを格納しているリモートの JSON ファイルにアクセスする、作成、 [ `Image` ](xref:Xamarin.Forms.Image)要素ごとの写真、および、に追加します`WrapLayout`. 次のスクリーン ショットに示すように外観が発生します。

![](custom-images/portait-screenshots.png "サンプル アプリケーションの縦のスクリーン ショット")

次のスクリーン ショットに示す、`WrapLayout`は横向きに回転された後。

![](custom-images/landscape-ios.png "サンプル iOS アプリケーションの横のスクリーン ショット")
![](custom-images/landscape-android.png "サンプル Android アプリケーション ランドス ケープ スクリーン ショット")
![](custom-images/landscape-uwp.png "サンプル UWP アプリケーションの横のスクリーン ショット")

各行の列の数は、写真のサイズ、画面の幅、およびデバイスに依存しない単位あたりのピクセルの数によって異なります。 [ `Image` ](xref:Xamarin.Forms.Image)要素は、写真を非同期的に読み込むため、`WrapLayout`クラスの呼び出し頻度を受け取りますその[ `LayoutChildren` ](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double))メソッドとして各`Image`。要素は、読み込まれた写真に基づく新しいサイズを受け取ります。

## <a name="related-links"></a>関連リンク

- [WrapLayout (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/CustomLayout/WrapLayout/)
- [カスタム レイアウト](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter26.md)
- [Xamarin.Forms でのカスタム レイアウトを作成する (ビデオ)](https://evolve.xamarin.com/session/56e20f83bad314273ca4d81c)
- [レイアウト<T>](xref:Xamarin.Forms.Layout`1)
- [レイアウト](xref:Xamarin.Forms.Layout)
- [VisualElement](xref:Xamarin.Forms.VisualElement)
