---
title: でカスタムレイアウトを作成する Xamarin.Forms
description: この記事では、カスタムレイアウトクラスを記述する方法について説明し、向きを区別する WrapLayout クラスを示します。このクラスは、ページの横に子を並べて配置し、後続の子の表示を追加の行にラップします。
ms.prod: xamarin
ms.assetid: B0CFDB59-14E5-49E9-965A-3DCCEDAC2E31
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/29/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 63a939e7093bcbe52f1aed376253c7aa78b078bf
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2020
ms.locfileid: "91563849"
---
# <a name="create-a-custom-layout-in-no-locxamarinforms"></a>でカスタムレイアウトを作成する Xamarin.Forms

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-customlayout-wraplayout)

_Xamarin.Forms 5つのレイアウトクラス (StackLayout、AbsoluteLayout、RelativeLayout、Grid、および FlexLayout) を定義し、それぞれが異なる方法で子を整列します。ただし、で提供されていないレイアウトを使用してページコンテンツを整理する必要がある場合もあり Xamarin.Forms ます。この記事では、カスタムレイアウトクラスを記述する方法について説明し、向きを区別する WrapLayout クラスを示します。このクラスは、ページの横に子を並べて配置し、後続の子の表示を追加の行にラップします。_

では Xamarin.Forms 、すべてのレイアウトクラスが [`Layout<T>`](xref:Xamarin.Forms.Layout`1) クラスから派生し、ジェネリック型 [`View`](xref:Xamarin.Forms.View) とその派生型を制約します。 さらに、クラスは、 `Layout<T>` [`Layout`](xref:Xamarin.Forms.Layout) 子要素の配置とサイズ変更のための機構を提供するクラスから派生します。

すべてのビジュアル要素は、 *要求された* サイズと呼ばれる独自の優先サイズを決定します。 [`Page`](xref:Xamarin.Forms.Page)、 [`Layout`](xref:Xamarin.Forms.Layout) 、および [`Layout<View>`](xref:Xamarin.Forms.Layout`1) の派生型は、自身を基準とした子または子の位置とサイズを決定します。 したがって、レイアウトには親子リレーションシップが含まれます。このリレーションシップでは、親は子のサイズを決定しますが、要求された子のサイズに対応しようとします。

Xamarin.Formsカスタムレイアウトを作成するには、レイアウトと無効化のサイクルを十分に理解している必要があります。 ここでは、これらのサイクルについて説明します。

## <a name="layout"></a>レイアウト

レイアウトは、ビジュアルツリーの一番上からページを使用して開始され、ビジュアルツリーのすべての分岐を通じて、ページ上のすべてのビジュアル要素をカバーします。 他の要素の親である要素は、それ自体を基準にして子のサイズ設定と配置を行います。

[`VisualElement`](xref:Xamarin.Forms.VisualElement)クラスは、[ `Measure` ] (xref: を定義します Xamarin.Forms 。VisualElement. Measure (system.string, system.string, Xamarin.Forms .MeasureFlags)) を使用して、レイアウト操作の要素を測定し、[ `Layout` ] (xref: Xamarin.Forms .VisualElement。 Layout ( Xamarin.Forms .四角形)。要素がレンダリングされる四角形の領域を指定するメソッド。 アプリケーションが起動して最初のページが表示されると、最初の呼び出しで構成される *レイアウトサイクル* が呼び出され、 `Measure` 次に `Layout` が呼び出され [`Page`](xref:Xamarin.Forms.Page) ます。

1. レイアウトサイクル中は、すべての親要素が `Measure` その子のメソッドを呼び出します。
1. 子を測定した後は、すべての親要素が `Layout` その子のメソッドを呼び出します。

このサイクルにより、ページ上のすべてのビジュアル要素がメソッドとメソッドの呼び出しを受け取るようになり `Measure` `Layout` ます。 このプロセスを次の図に示します。

![::: なし (Xamarin. Forms)::: レイアウトサイクル](custom-images/layout-cycle.png)

> [!NOTE]
> レイアウトに影響を与える変更があった場合、ビジュアルツリーのサブセットにもレイアウトサイクルが発生する可能性があることに注意してください。 これには、のなどのコレクションに対して追加または削除される項目、 [`StackLayout`](xref:Xamarin.Forms.StackLayout) [`IsVisible`](xref:Xamarin.Forms.VisualElement.IsVisible) 要素のプロパティの変更、要素のサイズの変更などが含まれます。

Xamarin.Formsまたはプロパティを持つすべてのクラス `Content` には、 `Children` オーバーライド可能なメソッドがあり [`LayoutChildren`](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) ます。 から派生するカスタムレイアウトクラス [`Layout<View>`](xref:Xamarin.Forms.Layout`1) は、このメソッドをオーバーライドして、[ `Measure` ] (xref: Xamarin.Forms .VisualElement. Measure (system.string, system.string, Xamarin.Forms .MeasureFlags)) と [ `Layout` ] (xref: Xamarin.Forms .VisualElement。 Layout ( Xamarin.Forms .四角形)) メソッドが、必要なカスタムレイアウトを提供するために、すべての要素の子で呼び出されます。

また、またはから派生したすべてのクラスは、 [`Layout`](xref:Xamarin.Forms.Layout) [`Layout<View>`](xref:Xamarin.Forms.Layout`1) メソッドをオーバーライドする必要があり [`OnMeasure`](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) ます。これは、[ `Measure` ] (xref: を呼び出すことによって、レイアウトクラスが必要とするサイズを決定 Xamarin.Forms します。VisualElement. Measure (system.string, system.string, Xamarin.Forms .MeasureFlags)) の子のメソッド。

> [!NOTE]
> 要素は、要素の親内の要素に使用できる領域を示す *制約*に基づいて、サイズを決定します。 [ `Measure` ] (Xref: に渡される制約 Xamarin.FormsVisualElement. Measure (system.string, system.string, Xamarin.Forms .MeasureFlags)) と [`OnMeasure`](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) メソッドの範囲は 0 ~ `Double.PositiveInfinity` です。 要素は、 *constrained*[] (xref: に対する呼び出しを受け取ると、制約を受けたり、*完全に制約*されたりします `Measure` Xamarin.Forms 。VisualElement. Measure (system.string, system.string, Xamarin.Forms .MeasureFlags)) メソッドが無限でないことを示します。要素は特定のサイズに制限されます。 要素は、少なくとも1つの引数を持つメソッドの呼び出しを受け取ると、 *制約*がない、または *部分的に制約*され `Measure` `Double.PositiveInfinity` ます。無限制約は、自動サイズ調整を示すと考えることができます。

## <a name="invalidation"></a>無効化

無効化とは、ページ上の要素の変更によって新しいレイアウトサイクルがトリガーされるプロセスです。 適切なサイズまたは位置がなくなった場合、要素は無効と見なされます。 たとえば、のプロパティが変更された場合、は [`FontSize`](xref:Xamarin.Forms.Button.FontSize) [`Button`](xref:Xamarin.Forms.Button) 適切な `Button` サイズでなくなるため、は無効と見なされます。 のサイズを変更すると、 `Button` ページの残りの部分を通じてレイアウトを変更した場合に波及効果が得られます。

要素は、メソッドを呼び出すことによって無効に [`InvalidateMeasure`](xref:Xamarin.Forms.VisualElement.InvalidateMeasure) なります。通常、要素のプロパティが変更されると、要素の新しいサイズが変更される可能性があります。 このメソッドは、 [`MeasureInvalidated`](xref:Xamarin.Forms.VisualElement.MeasureInvalidated) イベントを発生させます。このイベントは、要素の親ハンドルを使用して新しいレイアウトサイクルをトリガーします。

クラスは、 [`Layout`](xref:Xamarin.Forms.Layout) [`MeasureInvalidated`](xref:Xamarin.Forms.VisualElement.MeasureInvalidated) プロパティまたはコレクションに追加されたすべての子のイベントのハンドラーを設定 `Content` し、 `Children` 子が削除されたときにハンドラーをデタッチします。 そのため、子を持つビジュアルツリー内のすべての要素は、その子の1つがサイズ変更されるたびに警告されます。 次の図は、ビジュアルツリー内の要素のサイズを変更することによって、ツリーがどのように変更されるかを示しています。

![ビジュアルツリー内の無効化](custom-images/invalidation.png)

ただし、クラスは、 `Layout` ページのレイアウトに対する子のサイズの変更の影響を制限しようとします。 レイアウトのサイズが制限されている場合、子のサイズを変更しても、ビジュアルツリー内の親のレイアウトよりも大きくなることはありません。 ただし、通常、レイアウトのサイズを変更すると、レイアウトによって子がどのように配置されるかに影響します。 したがって、レイアウトのサイズを変更すると、レイアウトのレイアウトサイクルが開始され、レイアウトはメソッドとメソッドの呼び出しを受け取り [`OnMeasure`](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) [`LayoutChildren`](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) ます。

[`Layout`](xref:Xamarin.Forms.Layout)また、クラスは、 [`InvalidateLayout`](xref:Xamarin.Forms.Layout.InvalidateLayout) メソッドと同様の目的を持つメソッドも定義し [`InvalidateMeasure`](xref:Xamarin.Forms.VisualElement.InvalidateMeasure) ます。 メソッドは、 `InvalidateLayout` レイアウトの位置と子のサイズに影響を与える変更が行われるたびに呼び出される必要があります。 たとえば、クラスは、 `Layout` `InvalidateLayout` レイアウトに対して子が追加または削除されるたびに、メソッドを呼び出します。

を [`InvalidateLayout`](xref:Xamarin.Forms.Layout.InvalidateLayout) オーバーライドしてキャッシュを実装すると、[ `Measure` ] (xref: Xamarin.Forms .VisualElement. Measure (system.string, system.string, Xamarin.Forms .MeasureFlags)) レイアウトの子のメソッド。 メソッドをオーバーライドする `InvalidateLayout` と、レイアウトに対して子が追加または削除されたときに通知が表示されます。 同様に、メソッドをオーバーライドして、 [`OnChildMeasureInvalidated`](xref:Xamarin.Forms.Layout.OnChildMeasureInvalidated) レイアウトの子のいずれかが変更されたときに通知を提供することもできます。 どちらの方法でも、カスタムレイアウトはキャッシュをクリアすることによって応答する必要があります。 詳細については、「 [レイアウトデータの計算とキャッシュ](#calculate-and-cache-layout-data)」を参照してください。

## <a name="create-a-custom-layout"></a>カスタムレイアウトを作成する

カスタムレイアウトを作成するプロセスは次のとおりです。

1. `Layout<View>` クラスから派生するクラスを作成します。 詳細については、「 [Create a WrapLayout](#create-a-wraplayout)」を参照してください。
1. [*省略可能*]レイアウトクラスに設定する必要のあるパラメーターについて、バインド可能なプロパティによってサポートされるプロパティを追加します。 詳細については、「バインド可能な [プロパティによってサポート](#add-properties-backed-by-bindable-properties)されるプロパティの追加」を参照してください。
1. メソッドをオーバーライドし [`OnMeasure`](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) て [ `Measure` ] (xref: を呼び出し Xamarin.Forms ます。VisualElement. Measure (system.string, system.string, Xamarin.Forms .MeasureFlags)) メソッドを使用して、レイアウトのすべての子に対して、要求されたサイズを返します。 詳細については、「 [OnMeasure メソッドのオーバーライド](#override-the-onmeasure-method)」を参照してください。
1. メソッドをオーバーライドし [`LayoutChildren`](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) て [ `Layout` ] (xref: を呼び出し Xamarin.Forms ます。VisualElement。 Layout ( Xamarin.Forms .四角形)) メソッドをすべてのレイアウトの子に対してメソッドします。 [ `Layout` ] (Xref: を呼び出すことができません Xamarin.Forms 。VisualElement。 Layout ( Xamarin.Forms .四角形)) メソッドは、レイアウト内の各子に対して、正しいサイズまたは位置を受け取らないようにします。そのため、子はページ上に表示されなくなります。 詳細については、「 [LayoutChildren メソッドのオーバーライド](#override-the-layoutchildren-method)」を参照してください。

    > [!NOTE]
    > で子を列挙し、をオーバーライドする場合は [`OnMeasure`](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) [`LayoutChildren`](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) 、 [`IsVisible`](xref:Xamarin.Forms.VisualElement.IsVisible) プロパティがに設定されている子をスキップし `false` ます。 これにより、カスタムレイアウトでは、非表示の子の領域が残されることがなくなります。

1. [*省略可能*]メソッドをオーバーライドして、 [`InvalidateLayout`](xref:Xamarin.Forms.Layout.InvalidateLayout) 子がレイアウトに追加されたとき、またはレイアウトから削除されたときに通知されるようにします。 詳細については、「 [InvalidateLayout メソッドのオーバーライド](#override-the-invalidatelayout-method)」を参照してください。
1. [*省略可能*]メソッドをオーバーライドして [`OnChildMeasureInvalidated`](xref:Xamarin.Forms.Layout.OnChildMeasureInvalidated) 、レイアウトの子のいずれかが変更されたときに通知されるようにします。 詳細については、「 [OnChildMeasureInvalidated メソッドのオーバーライド](#override-the-onchildmeasureinvalidated-method)」を参照してください。

> [!NOTE]
> [`OnMeasure`](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double))レイアウトのサイズが子ではなく親によって制御されている場合、オーバーライドは呼び出されないことに注意してください。 ただし、制約の一方または両方が無限の場合、または layout クラスに既定 [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) 値またはプロパティ値がない場合は、オーバーライドが呼び出され [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) ます。 このため、オーバーライドは、 [`LayoutChildren`](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) メソッドの呼び出し中に取得された子のサイズに依存することはできません [`OnMeasure`](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) 。 代わりに、は `LayoutChildren` [ `Measure` ] (xref: を呼び出す必要があり Xamarin.Forms ます。VisualElement. Measure (system.string, system.string, Xamarin.Forms .MeasureFlags) メソッドを呼び出す前に、レイアウトの子のメソッドを呼び出します `Layout` Xamarin.Forms 。VisualElement。 Layout ( Xamarin.Forms .四角形)) メソッド。 または、オーバーライドで取得した子のサイズをキャッシュして、 `OnMeasure` オーバーライドでの後の呼び出しを回避することができ `Measure` `LayoutChildren` ます。ただし、レイアウトクラスは、サイズを再度取得する必要があるタイミングを知る必要があります。 詳細については、「 [レイアウトデータの計算とキャッシュ](#calculate-and-cache-layout-data)」を参照してください。

レイアウトクラスをに追加し、レイアウトに子を追加することによって、レイアウトクラスを使用でき [`Page`](xref:Xamarin.Forms.Page) ます。 詳細については、「 [WrapLayout の使用](#consume-the-wraplayout)」を参照してください。

### <a name="create-a-wraplayout"></a>WrapLayout を作成する

サンプルアプリケーションでは、 `WrapLayout` ページの横に子を並べて配置し、後続の子の表示を追加の行にラップする、向きを区別するクラスを示しています。

クラスは、子 `WrapLayout` の最大サイズに基づいて、 *セルサイズ*と呼ばれる各子に同じ領域を割り当てます。 セルのサイズより小さい子は、その [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) プロパティ値とプロパティ値に基づいてセル内に配置でき [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) ます。

`WrapLayout`クラス定義を次のコード例に示します。

```csharp
public class WrapLayout : Layout<View>
{
  Dictionary<Size, LayoutData> layoutDataCache = new Dictionary<Size, LayoutData>();
  ...
}
```

#### <a name="calculate-and-cache-layout-data"></a>レイアウトデータの計算とキャッシュ

構造体は、いくつかの `LayoutData` プロパティの子のコレクションに関するデータを格納します。

- `VisibleChildCount` –レイアウトに表示される子の数。
- `CellSize` –レイアウトのサイズに合わせて調整されたすべての子の最大サイズ。
- `Rows` –行の数。
- `Columns` –列の数。

フィールドは、 `layoutDataCache` 複数の値を格納するために使用され `LayoutData` ます。 アプリケーションが起動すると、2つの `LayoutData` オブジェクトが現在の方向に対してディクショナリにキャッシュされます。これは、 `layoutDataCache` オーバーライドへの制約引数、 `OnMeasure` `width` および `height` オーバーライドの引数と引数 `LayoutChildren` に対して1つです。 デバイスを横向きに回転させると、 `OnMeasure` オーバーライドと `LayoutChildren` オーバーライドが再び呼び出されます。これにより、別の2つの `LayoutData` オブジェクトがディクショナリにキャッシュされます。 ただし、デバイスを縦向きに戻す場合は、 `layoutDataCache` に必要なデータが既にあるため、これ以上の計算は必要ありません。

次のコード例は、 `GetLayoutData` 特定のサイズに基づいて構造化されたのプロパティを計算するメソッドを示してい `LayoutData` ます。

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

- 計算値がキャッシュに既に存在するかどうかを判断 `LayoutData` し、使用可能な場合はそれを返します。
- それ以外の場合は、すべての子を列挙し、 `Measure` 無限の幅と高さを使用して各子のメソッドを呼び出し、子の最大サイズを決定します。
- 少なくとも1つの子が表示されている場合は、必要な行数と列数を計算し、の寸法に基づいて子のセルサイズを計算し `WrapLayout` ます。 セルのサイズは通常、最大子のサイズよりも少し広くなることに注意してくださいが、が `WrapLayout` 最も幅の広い子に対して十分な幅でない場合や、最も高い子にとって十分な高さでない場合も、小さくなる可能性があります。
- 新しい `LayoutData` 値はキャッシュに格納されます。

#### <a name="add-properties-backed-by-bindable-properties"></a>バインド可能なプロパティによってサポートされるプロパティの追加

`WrapLayout`クラスは、 `ColumnSpacing` `RowSpacing` プロパティとプロパティを定義します。これらの値は、レイアウト内の行と列を分離するために使用され、バインド可能なプロパティによってサポートされます。 バインド可能なプロパティを次のコード例に示します。

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

各バインド可能なプロパティのプロパティ変更ハンドラーは、メソッドのオーバーライドを呼び出して `InvalidateLayout` 、で新しいレイアウトパスをトリガーし `WrapLayout` ます。 詳細については、「 [InvalidateLayout メソッドをオーバーライド](#override-the-invalidatelayout-method) し、 [OnChildMeasureInvalidated メソッドをオーバーライド](#override-the-onchildmeasureinvalidated-method)する」を参照してください。

#### <a name="override-the-onmeasure-method"></a>OnMeasure メソッドのオーバーライド

`OnMeasure`オーバーライドを次のコード例に示します。

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

オーバーライドは、メソッドを呼び出し、 `GetLayoutData` `SizeRequest` 返されたデータからオブジェクトを構築します。また、 `RowSpacing` プロパティとプロパティの値も考慮に入れ `ColumnSpacing` ます。 メソッドの詳細については `GetLayoutData` 、「 [レイアウトデータの計算とキャッシュ](#calculate-and-cache-layout-data)」を参照してください。

> [!IMPORTANT]
> [ `Measure` ] (Xref: Xamarin.FormsVisualElement. Measure (system.string, system.string, Xamarin.Forms .MeasureFlags)) と [`OnMeasure`](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) メソッドは、 [`SizeRequest`](xref:Xamarin.Forms.SizeRequest) プロパティがに設定された値を返すことによって、無限ディメンションを要求しないようにする必要があり `Double.PositiveInfinity` ます。 ただし、の制約引数の少なくとも1つをにする `OnMeasure` ことができ `Double.PositiveInfinity` ます。

#### <a name="override-the-layoutchildren-method"></a>LayoutChildren メソッドのオーバーライド

`LayoutChildren`オーバーライドを次のコード例に示します。

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

オーバーライドは、メソッドの呼び出しで開始され、 `GetLayoutData` すべての子のサイズを列挙し、各子のセル内に配置します。 これは、[] (xref: を呼び出すことで実現され `LayoutChildIntoBoundingRegion` Xamarin.Forms ます。LayoutChildIntoBoundingRegion ( Xamarin.Forms .VisualElement、 Xamarin.Forms 。Rectangle)) メソッド。このメソッドは、四角形内の子を、その [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) プロパティ値およびプロパティ値に基づいて配置するために使用され [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) ます。 これは、子の [] (xref: を呼び出すことに相当し `Layout` Xamarin.Forms ます。VisualElement。 Layout ( Xamarin.Forms .四角形)) メソッド。

> [!NOTE]
> メソッドに渡される四角形には `LayoutChildIntoBoundingRegion` 、子が存在できる領域全体が含まれていることに注意してください。

メソッドの詳細については `GetLayoutData` 、「 [レイアウトデータの計算とキャッシュ](#calculate-and-cache-layout-data)」を参照してください。

#### <a name="override-the-invalidatelayout-method"></a>InvalidateLayout メソッドのオーバーライド

[`InvalidateLayout`](xref:Xamarin.Forms.Layout.InvalidateLayout)オーバーライドは、次のコード例に示すように、レイアウトに対して子が追加または削除されたとき、またはいずれかのプロパティが値を変更したときに呼び出され `WrapLayout` ます。

```csharp
protected override void InvalidateLayout()
{
  base.InvalidateLayout();
  layoutInfoCache.Clear();
}
```

オーバーライドはレイアウトを無効にし、キャッシュされたすべてのレイアウト情報を破棄します。

> [!NOTE]
> [`Layout`](xref:Xamarin.Forms.Layout)レイアウトに対して子が追加または削除されるたびに、メソッドを呼び出すクラスを停止するには、 [`InvalidateLayout`](xref:Xamarin.Forms.Layout.InvalidateLayout) [ `ShouldInvalidateOnChildAdded` ] (xref: をオーバーライドします Xamarin.Forms 。ShouldInvalidateOnChildAdded ( Xamarin.Forms .ビュー) と [ `ShouldInvalidateOnChildRemoved` ] (xref: Xamarin.Forms .ShouldInvalidateOnChildRemoved ( Xamarin.Forms .) メソッドを返し、を返し `false` ます。 レイアウトクラスは、子が追加または削除されたときにカスタムプロセスを実装できます。

#### <a name="override-the-onchildmeasureinvalidated-method"></a>OnChildMeasureInvalidated メソッドのオーバーライド

[`OnChildMeasureInvalidated`](xref:Xamarin.Forms.Layout.OnChildMeasureInvalidated)オーバーライドは、レイアウトの子の1つがサイズを変更したときに呼び出され、次のコード例に示します。

```csharp
protected override void OnChildMeasureInvalidated()
{
  base.OnChildMeasureInvalidated();
  layoutInfoCache.Clear();
}
```

オーバーライドは子レイアウトを無効にし、キャッシュされたすべてのレイアウト情報を破棄します。

### <a name="consume-the-wraplayout"></a>WrapLayout を使用する

クラスは、 `WrapLayout` [`Page`](xref:Xamarin.Forms.Page) 次の XAML コード例に示すように、派生型に配置することによって使用できます。

```xaml
<ContentPage ... xmlns:local="clr-namespace:ImageWrapLayout">
    <ScrollView Margin="0,20,0,20">
        <local:WrapLayout x:Name="wrapLayout" />
    </ScrollView>
</ContentPage>
```

同等の C# コードを次に示します。

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

その後、必要に応じて、子をに追加でき `WrapLayout` ます。 [`Image`](xref:Xamarin.Forms.Image)に追加される要素を次のコード例に示し `WrapLayout` ます。

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

が含まれているページが表示されると、 `WrapLayout` サンプルアプリケーションは、写真のリストを含むリモート JSON ファイルに非同期にアクセスし、 [`Image`](xref:Xamarin.Forms.Image) 各写真の要素を作成してに追加し `WrapLayout` ます。 この結果、次のスクリーンショットに示すような外観が表示されます。

![サンプルアプリケーションの縦のスクリーンショット](custom-images/portait-screenshots.png)

次のスクリーンショットは、 `WrapLayout` 横方向に回転した後のを示しています。

![サンプル iOS アプリケーションランドスケープスクリーンショット ](custom-images/landscape-ios.png)
 ![ サンプル Android アプリケーションランドスケープスクリーンショット ](custom-images/landscape-android.png)
 ![ サンプル UWP アプリケーションランドスケープスクリーンショット](custom-images/landscape-uwp.png)

各行の列数は、写真のサイズ、画面の幅、デバイスに依存しない単位ごとのピクセル数によって異なります。 [`Image`](xref:Xamarin.Forms.Image)要素は写真を非同期に読み込みます。そのため、 `WrapLayout` [`LayoutChildren`](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) 読み込まれ `Image` た写真に基づいて各要素が新しいサイズを受け取るため、クラスはメソッドを頻繁に呼び出します。

## <a name="related-links"></a>関連リンク

- [WrapLayout (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-customlayout-wraplayout)
- [カスタム レイアウト](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter26.md)
- [でのカスタムレイアウトの作成 Xamarin.Forms (ビデオ)](https://www.youtube.com/watch?v=sxjOqNZFhKU)
- [レイアウト\<T>](xref:Xamarin.Forms.Layout`1)
- [レイアウト](xref:Xamarin.Forms.Layout)
- [VisualElement](xref:Xamarin.Forms.VisualElement)