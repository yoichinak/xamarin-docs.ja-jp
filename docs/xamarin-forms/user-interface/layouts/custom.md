---
title: カスタム レイアウトの作成
description: Xamarin.Forms は StackLayout、AbsoluteLayout、[相対レイアウト]、およびグリッドで、4 つのレイアウト クラスを定義し、別の方法でその子を整列それぞれします。 ただし、場合によっては必要 Xamarin.Forms では提供されないレイアウトを使用してページの内容を整理します。 この記事では、カスタム レイアウト クラスを作成する方法について説明し、その子をページ全体で水平方向に整列し、追加の行に後続の子の表示をラップする方向を区別 WrapLayout クラスを示します。
ms.prod: xamarin
ms.assetid: B0CFDB59-14E5-49E9-965A-3DCCEDAC2E31
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/29/2017
ms.openlocfilehash: f0728ac110fcf86f44a5ccb5ddd80b00af1b8d62
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="creating-a-custom-layout"></a>カスタム レイアウトの作成

_Xamarin.Forms は StackLayout、AbsoluteLayout、[相対レイアウト]、およびグリッドで、4 つのレイアウト クラスを定義し、別の方法でその子を整列それぞれします。ただし、場合によっては必要 Xamarin.Forms では提供されないレイアウトを使用してページの内容を整理します。この記事では、カスタム レイアウト クラスを作成する方法について説明し、その子をページ全体で水平方向に整列し、追加の行に後続の子の表示をラップする方向を区別 WrapLayout クラスを示します。_

## <a name="overview"></a>概要

Xamarin.Forms ですべてのレイアウトのクラス、 [ `Layout<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/)クラスし、するジェネリック型の制約[ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)とその派生型です。 さらに、`Layout<T>`クラスから派生、 [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/)クラスで、要素の配置と子のサイズ変更機構を提供します。

すべての visual 要素と呼ばれる独自の推奨サイズを判断する、*要求*サイズ。 [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)、 [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/)、および[ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/)派生型は、子、または基準自体の子のサイズと場所を判断します。 そのため、レイアウトが、親が決定されるその子のサイズにする必要があります、親子リレーションシップが含まれますがの子の要求されたサイズに合わせてましょう。

カスタム レイアウトを作成するには、Xamarin.Forms のレイアウトと無効化のサイクルの確実な理解が必要です。 これらのサイクルで説明するようになりました。

## <a name="layout"></a>レイアウト

レイアウトがビジュアル ツリーをページの上部にあるが開始され、ページ上のすべての visual 要素が含まれるようにビジュアル ツリーのすべての分岐進みます。 その他の要素の親である要素は、相対自体の子を配置してサイズ変更を行います。

[ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/)クラスを定義、 [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/)レイアウト操作を表す要素を測定するメソッドと[ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/)を指定するメソッド四角形の領域では、要素内に表示されます。 アプリケーションを起動し、最初のページが表示されます、*レイアウト サイクル*で構成される最初の`Measure`呼び出し、し`Layout`呼び出し、開始、 [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)オブジェクト。

1. レイアウト サイクル中に親のすべての要素は、通話、`Measure`その子のメソッドです。
1. 親のすべての要素が通話を担当する子が測定された後、`Layout`その子のメソッドです。

このサイクルはにより、ページ上のすべての visual 要素がへの呼び出しを受信する、`Measure`と`Layout`メソッドです。 プロセスは、次の図に表示されます。

![](custom-images/layout-cycle.png "Xamarin.Forms レイアウト サイクル")

> [!NOTE]
> いるレイアウト サイクルも発生する可能性がビジュアル ツリーのサブセットで、レイアウトに影響する変更が生じた場合に注意してください。 追加またはなどでのコレクションから削除されるアイテムが含まれます、 [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)、変更、 [ `IsVisible` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsVisible/) 、要素または要素のサイズの変更のプロパティです。

持つすべての Xamarin.Forms クラス、`Content`または`Children`プロパティは、オーバーライド可能な[ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/)メソッドです。 カスタム レイアウトのクラスから派生する[ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/)ことを確認して、このメソッドをオーバーライドする必要があります、 [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/)と[ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/)メソッド目的のカスタム レイアウトを提供する、すべての要素の子で呼び出されます。

さらに、すべてのクラスから派生した[ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/)または[ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/)オーバーライドする必要があります、 [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/)はレイアウト クラスであるメソッド呼び出すことでするに必要なサイズを決定、 [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/)その子のメソッドです。

> [!NOTE]
> 要素に基づくサイズを決定する*制約*要素の親内の要素の領域の量があるかを示すです。 渡される制約、 [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/)と[ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/)メソッドの範囲は 0 ~`Double.PositiveInfinity`します。 要素が*制約*、または*が完全に拘束*への呼び出しを受信すると、その[ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/)メソッド - 有限引数を持つ要素が制限されます特定のサイズ。 要素が*制約のない*、または*部分的に制約付き*への呼び出しを受信すると、その`Measure`メソッドには、少なくとも 1 つの引数と等しい`Double.PositiveInfinity`– 無限の制約を指定できますサイズの自動変更を示すと考えます。

## <a name="invalidation"></a>無効化

無効化は、ページ上の要素の変更が新しいレイアウト サイクルをトリガーするプロセスです。 要素は不要になったがインストールされている正しいサイズや位置が、無効と見なされます。 たとえば場合、 [ `FontSize` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.FontSize/)のプロパティ、 [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) 、変更、`Button`は無効であることは、適切なサイズと呼ばれます。 サイズ変更、`Button`レイアウト ページの残りの手順で変更の波及効果をし、必要があります。

要素を無効にそれ自体を呼び出すことによって、 [ `InvalidateMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.InvalidateMeasure()/)メソッド、一般に、要素のプロパティが変更された時点の要素の新しいサイズで発生した可能性があります。 このメソッドが呼び出さ、 [ `MeasureInvalidated` ](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.MeasureInvalidated/)新しいレイアウト サイクルをトリガーする要素の親を処理するイベントです。

[ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/)クラスのハンドラーを設定する、 [ `MeasureInvalidated` ](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.MeasureInvalidated/)に追加されたすべての子でイベントをその`Content`プロパティまたは`Children`コレクション、ハンドラーをデタッチし、ときに、子が削除されます。 したがって、子を持つビジュアル ツリー内のすべての要素はその子のいずれかのサイズを変更するたびに警告します。 次の図は、ビジュアル ツリー内の要素のサイズの変更によって、ツリーに波及変更可能性があります。

![](custom-images/invalidation.png "ビジュアル ツリー内の無効化")

ただし、`Layout`クラスは、ページのレイアウトの子供のサイズの変更の影響を制限しようとしています。 レイアウトがサイズの制約付きの場合は、し、子のサイズ変更には影響しませんビジュアル ツリーの親レイアウトより高い。 ただし、通常、レイアウトのサイズの変更に影響を与えるレイアウトがその子を整列する方法です。 そのため、任意のレイアウトのサイズの変更が、レイアウトのレイアウト サイクルを起動し、レイアウトへの呼び出しを受け取ります。 その[ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/)と[ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/)メソッドです。

[ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/)クラスも定義、 [ `InvalidateLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.InvalidateLayout()/)メソッドへのような目的を持つ、 [ `InvalidateMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.InvalidateMeasure()/)メソッドです。 `InvalidateLayout`レイアウトの配置し、その子のサイズに影響する変更が加えられるたびに、メソッドを呼び出す必要があります。 たとえば、`Layout`クラスを呼び出します、`InvalidateLayout`メソッドの子を追加またはレイアウトから削除されるたびにします。

[ `InvalidateLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.InvalidateLayout()/)の反復的な呼び出しを最小限に抑えるキャッシュを実装するためにオーバーライドできる、 [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/)レイアウトの子供のメソッドです。 オーバーライドする、`InvalidateLayout`メソッドが子を追加またはレイアウトから削除するときの通知を提供します。 同様に、 [ `OnChildMeasureInvalidated` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.OnChildMeasureInvalidated()/)レイアウトの子供のいずれかのサイズが変更されたときに通知を提供するメソッドをオーバーライドすることができます。 両方のメソッドのオーバーライドのカスタム レイアウトをキャッシュをオフにして応答します。 詳細については、次を参照してください。[計算とデータをキャッシュ](#caching)です。

## <a name="creating-a-custom-layout"></a>カスタム レイアウトの作成

カスタム レイアウトを作成するプロセスは次のとおりです。

1. `Layout<View>` クラスから派生するクラスを作成します。 詳細については、次を参照してください。 [、WrapLayout を作成する](#creating)です。
1. [*省略可能な*] 支えられてレイアウト クラスに設定する必要があります、パラメーターのバインド可能なプロパティのプロパティを追加します。 詳細については、次を参照してください。[を追加するプロパティをバインド可能なプロパティで補助](#adding_properties)です。
1. 上書き、 [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/)メソッドを呼び出す、 [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/)レイアウトのレイアウトのすべての子、および要求されたサイズの戻り値のメソッドです。 詳細については、次を参照してください。 [OnMeasure メソッドをオーバーライドする](#onmeasure)です。
1. 上書き、 [ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/)メソッドを呼び出す、 [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/)レイアウトのすべての子のメソッドです。 呼び出しの失敗、 [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/)適切なサイズまたは位置を受信しない子レイアウトでそれぞれの子のメソッドが表示され、そのため、子はならないページ上に表示します。 詳細については、次を参照してください。 [LayoutChildren メソッドをオーバーライドする](#layoutchildren)です。

  > [!NOTE]
>  内の子を列挙するときに、 [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/)と[ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/)の上書きがすべての子をスキップが[ `IsVisible` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsVisible/) にプロパティが設定されている`false`. 非表示の子のカスタム レイアウトされません余裕ようになります。

1. [*省略可能な*] オーバーライド、 [ `InvalidateLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.InvalidateLayout()/)子が追加またはレイアウトから削除されたときに通知するメソッド。 詳細については、次を参照してください。 [InvalidateLayout メソッドをオーバーライドする](#invalidatelayout)です。
1. [*省略可能な*] オーバーライド、 [ `OnChildMeasureInvalidated` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.OnChildMeasureInvalidated()/)レイアウトの子供のいずれかのサイズが変更されたときに通知するメソッド。 詳細については、次を参照してください。 [OnChildMeasureInvalidated メソッドをオーバーライドする](#onchildmeasureinvalidated)です。

> [!NOTE]
> なお、 [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/)レイアウトのサイズがその子ではなく、その親によって管理される場合、オーバーライドが呼び出されるされません。 制約の一方または両方が有限の数値でない場合、またはレイアウトのクラスがある既定ではない場合に、オーバーライドが呼び出されるただし、 [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/)または[ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/)プロパティの値。 このため、 [ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/)オーバーライドが中に取得された子のサイズに依存できない、 [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/)メソッドの呼び出しです。 代わりに、`LayoutChildren`呼び出す必要があります、 [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/)メソッドを呼び出す前に、レイアウトの子供、 [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/)メソッドです。 子ノードのサイズを取得する代わりに、`OnMeasure`を後で回避するのにはオーバーライドをキャッシュできる`Measure`で呼び出し、 `LayoutChildren` override、ただし、レイアウトのクラスはサイズが再度取得する必要がある場合を知る必要があります。 詳細については、次を参照してください。[計算とレイアウトのデータのキャッシュ](#caching)です。

レイアウトのクラスに追加することで、利用できる、 [ `Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)レイアウトに子を追加するとします。 詳細については、次を参照してください。[消費、WrapLayout](#consuming)です。

<a name="creating" />

### <a name="creating-a-wraplayout"></a>WrapLayout を作成します。

サンプル アプリケーションを示します向き依存型`WrapLayout`ページの上、その子を水平方向に整列し、追加の行に後続の子の表示をラップするクラス。

`WrapLayout`クラスでは、同じ量の領域を割り当てると呼ばれる、それぞれの子の*セル サイズ*子の最大サイズに基づきます。 子セルのサイズはセル内部に配置することができますよりも小さいがに基づいて、 [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/)と[ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/)プロパティの値。

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

`LayoutData`構造の子のコレクションに関するデータをさまざまなプロパティに保存します。

- `VisibleChildCount` – レイアウトで表示される子の数。
- `CellSize` -レイアウトのサイズを調整して、すべての子の最大サイズ。
- `Rows` – 行の数。
- `Columns` -列の数。

`layoutDataCache`フィールドは複数の格納に使用`LayoutData`値。 アプリケーションの開始時、2 つ`LayoutData`にオブジェクトがキャッシュされます、 `layoutDataCache` – 制約の引数のいずれか、現在の向きのディクショナリ、`OnMeasure`上書き、および 1 つずつ、`width`と`height`引数`LayoutChildren`をオーバーライドします。 横方向にデバイスを回転させるときに、`OnMeasure`をオーバーライドおよび`LayoutChildren`上書きもう一度呼び出される、その他の 2 つの結果`LayoutData`をディクショナリにキャッシュされるオブジェクト。 ただし、縦向きにデバイスを返すときにそれ以上の計算は必要ありませんので、`layoutDataCache`が既に必要なデータです。

次のコード例は、`GetLayoutData`のプロパティを計算するメソッド、`LayoutData`構造化された特定のサイズに基づきます。

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

- 計算されるかどうかを決定`LayoutData`値がキャッシュ内に既にと使用可能になる場合は、それを取得します。
- それ以外の場合、呼び出すすべての子を列挙、`Measure`無限の幅と高さをそれぞれの子のメソッドを子の最大サイズを決定します。
- 表示されている、少なくとも 1 つの子は、その行と、必要な列の数を計算のディメンションに基づく子セルのサイズを計算、`WrapLayout`です。 セルのサイズが通常、子の最大サイズよりも太くなってわずかですが、させることがも小さい場合、`WrapLayout`幅が狭い最も幅の広い子または十分な縦の最も高い子にします。
- 新しい格納`LayoutData`キャッシュ内の値。

<a name="adding_properties" />

#### <a name="adding-properties-backed-by-bindable-properties"></a>プロパティのバインド可能なプロパティでバックアップを追加します。

`WrapLayout`クラスを定義`ColumnSpacing`と`RowSpacing`プロパティ、レイアウトでは、列の行を区切るための値が使用され、バインド可能なプロパティでバックアップされています。 バインド可能なプロパティは次のコード例に示します。

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

各バインド可能なプロパティのプロパティ変更ハンドラーを呼び出す、`InvalidateLayout`新しいレイアウトをトリガーするメソッドのオーバーライドを渡す、`WrapLayout`です。 詳細については、次を参照してください。 [InvalidateLayout メソッドをオーバーライドする](#invalidatelayout)と[OnChildMeasureInvalidated メソッドをオーバーライドする](#onchildmeasureinvalidated)です。

<a name="onmeasure" />

#### <a name="overriding-the-onmeasure-method"></a>OnMeasure メソッドのオーバーライド

`OnMeasure`オーバーライドが次のコード例に示すようにします。

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

オーバーライドを呼び出す、`GetLayoutData`メソッドとコンス トラクター、`SizeRequest`オブジェクトも考慮に入れているときに、返されたデータから、`RowSpacing`と`ColumnSpacing`プロパティの値。 詳細については、`GetLayoutData`メソッドを参照してください[計算とデータをキャッシュ](#caching)です。

> [!IMPORTANT]
> [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/)と[ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/)メソッドを返すことによって、無限のディメンションを要求すべきことはありません、 [ `SizeRequest` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SizeRequest/)に設定されたプロパティの値`Double.PositiveInfinity`. ただし、制約の引数の少なくとも 1 つ`OnMeasure`できます`Double.PositiveInfinity`です。

<a name="layoutchildren" />

#### <a name="overriding-the-layoutchildren-method"></a>LayoutChildren メソッドのオーバーライド

`LayoutChildren`オーバーライドが次のコード例に示すようにします。

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

オーバーライドがへの呼び出しで始まり、`GetLayoutData`メソッド、しのすべてのサイズし、それぞれの子のセル内の位置にそれらを子を列挙します。 呼び出すことによってこれは、 [ `LayoutChildIntoBoundingRegion` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildIntoBoundingRegion/p/Xamarin.Forms.VisualElement/Xamarin.Forms.Rectangle/)に基づいて四角形内にある子の配置に使用するメソッド、 [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/)と[ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/)プロパティの値。 これは、子への呼び出しに相当[ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/)メソッドです。

> [!NOTE]
> 渡される四角形を`LayoutChildIntoBoundingRegion`メソッドには子が存在する領域全体が含まれています。

詳細については、`GetLayoutData`メソッドを参照してください[計算とデータをキャッシュ](#caching)です。

<a name="invalidatelayout" />

#### <a name="overriding-the-invalidatelayout-method"></a>InvalidateLayout メソッドのオーバーライド

[ `InvalidateLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.InvalidateLayout()/)オーバーライドが呼び出されるの子の追加または削除がレイアウトから、またはいずれかときの`WrapLayout`の次のコード例に示すように、プロパティの変更が値します。

```csharp
protected override void InvalidateLayout()
{
  base.InvalidateLayout();
  layoutInfoCache.Clear();
}
```

上書きは、レイアウトを無効にし、すべてのキャッシュされたレイアウト情報を破棄します。

> [!NOTE]
> 停止する、 [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/)クラスを呼び出す、 [ `InvalidateLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.InvalidateLayout()/)子が追加または、レイアウトから削除されるたびにメソッドをオーバーライドする、 [ `ShouldInvalidateOnChildAdded` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.ShouldInvalidateOnChildAdded/p/Xamarin.Forms.View/)および[ `ShouldInvalidateOnChildRemoved` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.ShouldInvalidateOnChildRemoved/p/Xamarin.Forms.View/)メソッド、および戻り値`false`です。 レイアウトのクラスの子が追加または削除されたときに、カスタム プロセスを実装できます。

<a name="onchildmeasureinvalidated" />

#### <a name="overriding-the-onchildmeasureinvalidated-method"></a>OnChildMeasureInvalidated メソッドのオーバーライド

[ `OnChildMeasureInvalidated` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.OnChildMeasureInvalidated()/) 、サイズ変更は次のコード例で示すようにレイアウトの子供のいずれかとオーバーライドが呼び出されます。

```csharp
protected override void OnChildMeasureInvalidated()
{
  base.OnChildMeasureInvalidated();
  layoutInfoCache.Clear();
}
```

上書きは、子のレイアウトを無効にし、すべてのキャッシュされたレイアウト情報を破棄します。

<a name="consuming" />

### <a name="consuming-the-wraplayout"></a>WrapLayout の使用

`WrapLayout`クラスに配置することで利用できる、 [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) XAML コードの例を次に示すように型を派生します。

```xaml
<ContentPage ... xmlns:local="clr-namespace:ImageWrapLayout">
    <ScrollView Margin="0,20,0,20">
        <local:WrapLayout x:Name="wrapLayout" />
    </ScrollView>
</ContentPage>
```

同等の c# コードは、次に示します。

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

子を追加することができますし、`WrapLayout`必要に応じて。 次のコード例に示す[ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/)要素の追加、 `WrapLayout`:

```csharp
protected override async void OnAppearing()
{
  base.OnAppearing();

  var images = await GetImageListAsync();
  foreach (var photo in images.Photos)
  {
    var image = new Image
    {
      Source = ImageSource.FromUri(new Uri(photo + string.Format("?width={0}&height={0}&mode=max", Device.RuntimePlatform == Device.UWP ? 120 : 240)))
    };
    wrapLayout.Children.Add(image);
  }
}

async Task<ImageList> GetImageListAsync()
{
  var requestUri = "https://docs.xamarin.com/demo/stock.json";
  using (var client = new HttpClient())
  {
    var result = await client.GetStringAsync(requestUri);
    return JsonConvert.DeserializeObject<ImageList>(result);
  }
}
```

ときに、ページを含む、`WrapLayout`が表示されたら、サンプル アプリケーションが非同期的に写真の一覧を含むリモート JSON ファイルにアクセスする、作成、 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/)要素ごとの写真、および、に追加`WrapLayout`. これは、結果、次のスクリーン ショットに示すように表示されます。

![](custom-images/portait-screenshots.png "サンプル アプリケーションの縦のスクリーン ショット")

次のスクリーン ショットに示さ、`WrapLayout`を横向きに回転した後。

![](custom-images/landscape-ios.png "IOS アプリケーションのランドス ケープ スクリーン ショットをサンプル")
![](custom-images/landscape-android.png "サンプル Android アプリケーション ランドス ケープのスクリーン ショット")
 ![ ] (custom-images/landscape-uwp.png "サンプル UWP アプリケーションの横のスクリーン ショット")

各行の列の数は、写真のサイズ、画面の幅、およびデバイスに依存しない単位あたりのピクセルの数によって異なります。 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/)要素が、写真を非同期的に読み込むと、そのため、`WrapLayout`クラスへの呼び出しの頻度が表示されます、 [ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/)メソッドとして各`Image`要素は、読み込まれた写真に基づく新しいサイズを受け取ります。

## <a name="summary"></a>まとめ

カスタム レイアウト クラスを作成する方法を説明し、印刷の向き依存型のデモンストレーションをこの記事`WrapLayout`ページの上、その子を水平方向に整列し、追加の行に後続の子の表示をラップするクラス。


## <a name="related-links"></a>関連リンク

- [WrapLayout (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/CustomLayout/WrapLayout/)
- [カスタム レイアウト](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter26.md)
- [Xamarin.Forms でのカスタム レイアウトを作成する (ビデオ)](https://evolve.xamarin.com/session/56e20f83bad314273ca4d81c)
- [レイアウト<T>](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/)
- [レイアウト](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/)
- [VisualElement](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/)
