---
title: Xamarin. Forms コモンコントロールのプロパティ、メソッド、およびイベント
description: この記事では、派生クラスで一般的に使用される、VisualElement クラスで定義されている共通のプロパティ、メソッド、およびイベントについて説明します。
ms.prod: xamarin
ms.assetId: 85A0CCF5-C1D8-40BB-927F-A4D944E5534D
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 08/21/2019
ms.openlocfilehash: 6d10e665c6461655440ddfb2c524cb56a14337f6
ms.sourcegitcommit: 1dd7d09b60fcb1bf15ba54831ed3dd46aa5240cb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/28/2019
ms.locfileid: "70121344"
---
# <a name="xamarinforms-common-control-properties-methods-and-events"></a>Xamarin. Forms コモンコントロールのプロパティ、メソッド、およびイベント

Xamarin. forms `VisualElement`クラスは、xamarin アプリケーションで使用されるほとんどのコントロールの基本クラスです。 クラス`VisualElement`は、派生クラスで使用される多くの[プロパティ](#properties)、[メソッド](#methods)、および[イベント](#events)を定義します。

## <a name="properties"></a>プロパティ

インスタンスで`VisualElement`は、次のプロパティを使用できます。 完全な一覧については、「 [Visualelement API のプロパティ](xref:Xamarin.Forms.VisualElement#properties)」を参照してください。

### [`AnchorX`](xref:Xamarin.Forms.VisualElement.AnchorX)

プロパティは、スケール`double`や回転などの変換の X 軸の中心点を定義する値です。 `AnchorX` 既定値は0.5 です。

### [`AnchorY`](xref:Xamarin.Forms.VisualElement.AnchorY)

プロパティは、スケール`double`や回転などの変換の X 軸の中心点を定義する値です。 `AnchorY` 既定値は0.5 です。

### [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor)

プロパティは、コントロールの背景色を決定するです。`Color` `BackgroundColor` 設定が解除されている場合、 `Color`背景は、透明として表示される既定のオブジェクトになります。

### [`Behaviors`](xref:Xamarin.Forms.VisualElement.Behaviors)

プロパティはオブジェクト`Behavior`のです。 `List` `Behaviors` ビヘイビアーを使用すると、 `Behaviors`要素をリストに追加することで、要素に再利用可能な機能をアタッチできます。 クラスの`Behavior`詳細については、「 [Xamarin の動作](~/xamarin-forms/app-fundamentals/behaviors/index.md)」を参照してください。

### [`Bounds`](xref:Xamarin.Forms.VisualElement.Bounds)

プロパティは、コントロールによって`Rectangle`占有される領域を表す読み取り専用のオブジェクトです。 `Bounds` `Bounds`プロパティ値は、レイアウトサイクル中に割り当てられます。 に`Rectangle`は、 `struct`四角形の交点と含有をテストするための便利なプロパティとメソッドが含まれています。 詳細については、「 [Xamarin. Forms RECTANGLE API](xref:Xamarin.Forms.Rectangle)」を参照してください。

### [`Effects`](xref:Xamarin.Forms.Element.Effects)

プロパティは、 `Element`( `List` xref `Effect` : Xamarin. Forms. Element) クラスから継承されたオブジェクトのです。 `Effects` 効果を使用すると、ネイティブコントロールをカスタマイズでき、通常は小さなスタイル変更に使用されます。 クラスの`Effect`詳細については、「 [Xamarin の効果](~/xamarin-forms/app-fundamentals/effects/index.md)」を参照してください。

### [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection)

`FlowDirection` プロパティ`FlowDirection`は列挙値です。 フロー方向は、 `MatchParent` `LeftToRight`、、または`RightToLeft`に設定でき、レイアウトの順序と方向を決定します。 通常`FlowDirection` 、プロパティは、右から左に読む言語をサポートするために使用されます。

### [`Height`](xref:Xamarin.Forms.VisualElement.Height)

プロパティは、コントロールの描画さ`double`れた高さを記述する読み取り専用の値です。 `Height` `Height`プロパティは、レイアウトサイクル中に計算され、直接設定することはできません。 コントロールの高さは、高さ[要求プロパティ](#heightrequest)を使用して要求できます。

### [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest)

プロパティは、コントロールの必要な高さを決定する値です。`double` `HeightRequest` コントロールの絶対高さが、要求された値と一致しない可能性があります。 詳細については、「[要求のプロパティ](#request-properties)」を参照してください。

### [`InputTransparent`](xref:Xamarin.Forms.VisualElement.InputTransparent)

プロパティは、コントロールがユーザー入力を受け取るかどうかを決定するです。`bool` `InputTransparent` 既定値は`false`で、要素が入力を受け取ることを保証します。 このプロパティは、設定時に子要素に転送されます。 レイアウトクラスで`true` プロパティをに設定すると、レイアウト内のすべての要素が入力を受け取ら`InputTransparent`なくなります。

### [`IsEnabled`](xref:Xamarin.Forms.VisualElement.IsEnabled)

プロパティは、コントロールがユーザー入力に反応するかどうかを決定する値です。`bool` `IsEnabled` 既定値は `true` です。 このプロパティを false に設定すると、コントロールはユーザー入力を受け付けなくなります。

### [`IsFocused`](xref:Xamarin.Forms.VisualElement.IsFocused)

プロパティは、コントロールが現在フォーカスされているオブジェクトであるかどうかを示す値です。`bool` `IsFocused` コントロールに[`Focus`](#focus)対してメソッドを呼び出すと、 `IsFocused`値が true に設定されます。 メソッドを[`Unfocus`](#unfocus)呼び出すと、このプロパティは false に設定されます。

### [`IsTabStop`](xref:Xamarin.Forms.VisualElement.IsTabStop)

プロパティは、ユーザー `bool`が tab キーを使用してコントロールを進めるときにコントロールがフォーカスを受け取るかどうかを定義する値です。 `IsTabStop` このプロパティが false の場合、 `TabIndex`プロパティは無効になります。

### [`IsVisible`](xref:Xamarin.Forms.VisualElement.IsVisible)

プロパティは、コントロール`bool`が表示されるかどうかを決定する値です。 `IsVisible` プロパティが`IsVisible` false に設定されているコントロールは表示されず、レイアウトサイクル中の領域の計算では考慮されず、ユーザー入力を受け付けることはできません。

### [`MinimumHeightRequest`](xref:Xamarin.Forms.VisualElement.MinimumHeightRequest)

プロパティは、2 `double`つの要素が制限された領域に対して競合する場合のオーバーフローの処理方法を決定する値です。 `MinimumHeightRequest` プロパティを`MinimumHeightRequest`設定すると、レイアウトプロセスで、要求された最小の次元に要素をスケールダウンできます。 が指定`MinimumHeightRequest`されていない場合、既定値は-1 です。レイアウトプロセス`HeightRequest`では、が最小値であると見なされます。 これは、値のない`MinimumHeightRequest`要素の高さがスケーラブルではないことを意味します。

詳細については、「[最小要求プロパティ](#minimum-request-properties)」を参照してください。

### [`MinimumWidthRequest`](xref:Xamarin.Forms.VisualElement.MinimumWidthRequest)

プロパティは、2 `double`つの要素が制限された領域に対して競合する場合のオーバーフローの処理方法を決定する値です。 `MinimumWidthRequest` プロパティを`MinimumWidthRequest`設定すると、レイアウトプロセスで、要求された最小の次元に要素をスケールダウンできます。 が指定`MinimumWidthRequest`されていない場合、既定値は-1 です。レイアウトプロセス`WidthRequest`では、が最小値であると見なされます。 これは、値のない`MinimumWidthRequest`要素の幅が拡張されないことを意味します。

詳細については、「[最小要求プロパティ](#minimum-request-properties)」を参照してください。

### [`Opacity`](xref:Xamarin.Forms.VisualElement.Opacity)

プロパティ`Opacity`は、レンダリング`double`中にコントロールの不透明度を決定する0から1の値です。 このプロパティの既定値は1.0 です。 0 ~ 1 の範囲外の値はクランプされます。 プロパティは、 [`IsVisible`](#isvisible)プロパティが`true`の場合にのみ適用されます。 `Opacity` 不透明度は反復的に適用されます。 したがって、親コントロールの不透明度が0.5 で、その子の不透明度が0.5 である場合、子は有効な0.25 不透明度の値を使用してレンダリングされます。 入力コントロール`Opacity`のプロパティを0に設定すると、動作が未定義になります。

### [`Parent`](xref:Xamarin.Forms.Element.Parent)

`Parent` プロパティは、`Element` クラスから継承されます。 このプロパティは、 `Element`コントロールの親であるオブジェクトです。 `Parent`プロパティは、通常、別の要素の子として追加されるときに、要素に対して自動的に設定されます。

### [`Resources`](xref:Xamarin.Forms.VisualElement.Resources)

プロパティは、通常、XAML から実行時に設定されるキーと値のペアを使用して設定されるインスタンスです。`ResourceDictionary` `Resources` このディクショナリを使用すると、アプリケーション開発者は、XAML で定義されたオブジェクトをコンパイル時と実行時の両方で再利用できます。 ディクショナリ内のキーは、XAML タグの`x:Key`属性から設定されます。 XAML から作成されたオブジェクトは、 `ResourceDictionary`指定されたキーのに挿入されます。 初期化されます。

詳細については、「[リソースディクショナリ](~/xamarin-forms/xaml/resource-dictionaries.md)」を参照してください。

### [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation)

`Rotation` プロパティ`double`は 0 ~ 360 の値で、Z 軸の回転角度を度単位で定義します。 このプロパティの既定値は0です。 回転は、値[`AnchorX`](#anchorx)および値に対して[`AnchorY`](#anchory)相対的に適用されます。

### [`RotationX`](xref:Xamarin.Forms.VisualElement.RotationX)

`RotationX` プロパティ`double`は 0 ~ 360 の値で、X 軸の回転角度を度単位で定義します。 このプロパティの既定値は0です。 回転は、値[`AnchorX`](#anchorx)および値に対して[`AnchorY`](#anchory)相対的に適用されます。

### [`RotationY`](xref:Xamarin.Forms.VisualElement.RotationY)

プロパティは、Y `double`軸に対する回転を度数で定義する 0 ~ 360 の値です。 `RotationY` このプロパティの既定値は0です。 回転は、値[`AnchorX`](#anchorx)および値に対して[`AnchorY`](#anchory)相対的に適用されます。

### [`Scale`](xref:Xamarin.Forms.VisualElement.Scale)

プロパティは、コントロールのスケールを定義する値です。`double` `Scale` このプロパティの既定値は1.0 です。 スケールは、値[`AnchorX`](#anchorx)および値に対して[`AnchorY`](#anchory)相対的に適用されます。

### [`ScaleX`](xref:Xamarin.Forms.VisualElement.ScaleX)

プロパティは、X 軸に沿ってコントロールのスケールを定義する値です。`double` `ScaleX` このプロパティの既定値は1.0 です。 プロパティは、値に対して[`AnchorX`](#anchorx)相対的に適用されます。 `ScaleX`

### [`ScaleY`](xref:Xamarin.Forms.VisualElement.ScaleY)

プロパティは、Y 軸に沿ったコントロールのスケールを定義する値です。`double` `ScaleY` このプロパティの既定値は1.0 です。 プロパティは、値に対して[`AnchorY`](#anchory)相対的に適用されます。 `ScaleY`

### [`Style`](xref:Xamarin.Forms.NavigableElement.Style)

`Style` プロパティは、`NavigableElement` クラスから継承されます。 このプロパティは、 `Style`クラスのインスタンスです。 クラス`Style`には、ビジュアル要素の外観と動作を定義するトリガー、セッター、および動作が含まれています。 詳細については、「 [Xamarin の XAML スタイル](~/xamarin-forms/user-interface/styles/xaml/index.md)」を参照してください。

### [`StyleClass`](xref:Xamarin.Forms.NavigableElement.StyleClass)

プロパティは、クラスの`Style`名前`string`を表すオブジェクトのリストです。 `StyleClass` このプロパティは、`NavigableElement` クラスから継承されます。 プロパティを使用すると、複数のスタイル属性を`VisualElement`インスタンスに適用できます。 `StyleClass` 詳細については、「 [Xamarin のスタイルクラス](~/xamarin-forms/user-interface/styles/xaml/style-class.md)」を参照してください。

### [`TabIndex`](xref:Xamarin.Forms.VisualElement.TabIndex)

プロパティは、tab キーを使用してコントロールを進めるときに制御順序を定義する値です。`int` `TabIndex` プロパティは、 `VisualElement`クラスが実装する`ITabStopElement`インターフェイスで定義されたプロパティの実装です。 `TabIndex`

### [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX)

プロパティは、X 軸に適用するデルタ平行移動を定義する値です。`double` `TranslationX` 翻訳はレイアウトの後に適用され、通常はアニメーションを適用するために使用されます。 親コンテナーの境界の外側にある要素を変換すると、入力が禁止されます。

詳細については、「 [Xamarin. Forms でのアニメーション](~/xamarin-forms/user-interface/animation/index.md)」を参照してください。

### [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY)

プロパティは、Y 軸に適用するデルタ平行移動を定義する値です。`double` `TranslationY` 翻訳はレイアウトの後に適用され、通常はアニメーションを適用するために使用されます。 親コンテナーの境界の外側にある要素を変換すると、入力が禁止されます。

詳細については、「 [Xamarin. Forms でのアニメーション](~/xamarin-forms/user-interface/animation/index.md)」を参照してください。

### [`Triggers`](xref:Xamarin.Forms.VisualElement.Triggers)

プロパティは、オブジェクト`List`の`TriggerBase`読み取り専用です。 `Triggers` トリガーを使用すると、アプリケーション開発者は、イベントまたはプロパティの変更に応じてコントロールの外観を変更するアクションを XAML で表現できます。 詳細については、「 [Xamarin. Forms Triggers](~/xamarin-forms/app-fundamentals/triggers.md)」を参照してください。

### [`Visual`](xref:Xamarin.Forms.VisualElement.Visual)

プロパティ`Visual` は、レンダラーを`VisualElement`作成し、インスタンスに対して選択的に適用できるようにするインスタンスです。`IVisual` `Visual`プロパティはその親に一致するように設定されているため、コンポーネントでレンダラーを定義すると、そのコンポーネントのすべての子にも適用されます。 コントロールまたはその先祖にカスタムレンダラーが設定されていない場合は、既定の Xamarin. Forms レンダラーが使用されます。 詳細については、「 [Xamarin. フォームビジュアル](~/xamarin-forms/user-interface/visual/index.md)」を参照してください。

### [`Width`](xref:Xamarin.Forms.VisualElement.Width)

プロパティは、コントロールの描画幅`double`を記述する読み取り専用の値です。 `Width` `Width`プロパティは、レイアウトサイクル中に計算され、直接設定することはできません。 Width[要求プロパティ](#widthrequest)を使用して、コントロールの幅を要求できます。

### [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest)

プロパティは、コントロールの適切な幅を決定する値です。`double` `WidthRequest` コントロールの絶対幅が、要求された値と一致しない可能性があります。 詳細については、「[要求のプロパティ](#request-properties)」を参照してください。

### [`X`](xref:Xamarin.Forms.VisualElement.X)

プロパティは、コントロールの現在の`double` X 位置を記述する読み取り専用の値です。 `X`

### [`Y`](xref:Xamarin.Forms.VisualElement.Y)

プロパティは、コントロールの現在の`double` Y 位置を記述する読み取り専用の値です。 `Y`

## <a name="methods"></a>メソッド

`VisualElement`クラスでは、次のメソッドを使用できます。 完全な一覧については、「 [Visualelement API メソッド](xref:Xamarin.Forms.VisualElement#methods)」を参照してください。

### [`FindByName`](xref:Xamarin.Forms.Element.FindByName*)

メソッドは`Element`クラスから継承され、次のシグネチャを持ちます。 `FindByName`

```csharp
public object FindByName (string name)
```

このメソッドは、指定された`name`引数のすべての子要素を検索し、指定された名前を持つ要素を返します。 一致するものが見つからない`null`場合は、が返されます。

### [`Focus`](xref:Xamarin.Forms.VisualElement.Focus)

メソッド`Focus`は、要素にフォーカスを設定しようとします。 このメソッドのシグネチャは次のとおりです。

```csharp
public bool Focus ()
```

この`Focus`メソッドは`true` 、キーボード`false`フォーカスが正常に設定された場合はを返し、メソッド呼び出しでフォーカスが変更されなかった場合はを返します。 要素は、このメソッドが機能するためにフォーカスを受け取ることができる必要があります。 オフスクリーン`Focus`または実現されていない要素に対してメソッドを呼び出すと、動作が未定義になります。

### [`Unfocus`](xref:Xamarin.Forms.VisualElement.Unfocus)

メソッド`Unfocus`は、要素のフォーカスを解除しようとします。 このメソッドのシグネチャは次のとおりです。

```csharp
public void Unfocus ()
```

要素には、このメソッドが機能するために既にフォーカスがある必要があります。

## <a name="events"></a>イベント

`VisualElement`クラスでは、次のイベントを使用できます。 完全な一覧については、「 [Xamarin の VisualElement イベント](xref:Xamarin.Forms.VisualElement#events)」を参照してください。

### [`Focused`](xref:Xamarin.Forms.VisualElement.Focused)

イベントは、インスタンスが`VisualElement`フォーカスを受け取ったときに発生します。 `Focused` このイベントは、Xamarin. Forms スタックではバブルされません。ネイティブコントロールから直接受信します。 このイベントは、 [`IsFocused`](#isfocused)プロパティ set アクセス操作子によって生成されます。

### [`SizeChanged`](xref:Xamarin.Forms.VisualElement.SizeChanged)

イベントは、 `VisualElement`インスタンス`Height`または`Width`プロパティが変更されるたびに発生します。 `SizeChanged` 開発者が変更後のイベントに応答するのではなく、サイズの変更に直接応答する場合は、 [`OnSizeAllocated`](xref:Xamarin.Forms.VisualElement.OnSizeAllocated*)代わりに仮想メソッドを実装する必要があります。

### [`Unfocused`](xref:Xamarin.Forms.VisualElement.Unfocused)

イベントは、インスタンスがフォーカス`VisualElement`を失ったときに発生します。 `Unfocused` このイベントは、Xamarin. Forms スタックではバブルされません。ネイティブコントロールから直接受信します。 このイベントは、 [`IsFocused`](#isfocused)プロパティ set アクセス操作子によって生成されます。

## <a name="units-of-measurement"></a>測定単位

Android、iOS、UWP の各プラットフォームには、デバイスによって異なる測定単位があります。 Xamarin では、プラットフォームに依存しない測定単位を使用して、デバイスとプラットフォームの単位を正規化します。 Xamarin. Forms には、1インチあたり160ユニットまたは 1 cm あたりの64ユニットがあります。

## <a name="request-properties"></a>要求のプロパティ

名前に "request" が含まれているプロパティは、目的の値を定義しますが、実際に表示される値とは一致しない可能性があります。 たとえば、 `HeightRequest`は150に設定されている場合がありますが、レイアウトでは100ユニット`Height`の領域のみが許可されている場合、コントロールのレンダリングは100のみになります。 表示されるサイズは、使用可能な領域と含まれるコンポーネントの影響を受けます。

## <a name="minimum-request-properties"></a>最小要求プロパティ

最小要求プロパティに`MinimumHeightRequest`は`MinimumWidthRequest`、とが含まれます。これは、要素が互いにどのようにオーバーフローしているかをより正確に制御できるようにするためのものです。 ただし、これらのプロパティに関連するレイアウト動作には、重要な考慮事項がいくつかあります。

### <a name="unspecified-minimum-property-values"></a>指定されていない最小プロパティ値

最小値が設定されていない場合、minimum プロパティの既定値は-1 です。 レイアウトプロセスでは、この値は無視され、絶対値が最小値であると見なされます。 この動作の実際の結果として、最小値が指定されていない要素は圧縮され**ません**。 最小値を指定した要素**が縮小され**ます。

次の XAML は、 `BoxView`水平方向`StackLayout`に2つの要素を示しています。

```xaml
<StackLayout Orientation="Horizontal">
    <BoxView HeightRequest="100" BackgroundColor="Purple" WidthRequest="500"></BoxView>
    <BoxView HeightRequest="100" BackgroundColor="Green" WidthRequest="500" MinimumWidthRequest="250"></BoxView>
</StackLayout>
```

最初`BoxView`のインスタンスは、幅500を要求し、最小幅を指定していません。 2番`BoxView`目のインスタンスは、幅500と最小幅250を要求します。 親`StackLayout`要素が、要求された幅で両方のコンポーネントを格納するのに十分`BoxView`な幅ではない場合、レイアウトプロセスでは、他の有効な最小値が指定されていないため、最初のインスタンスは最小幅500を持つと見なされます。 2番`BoxView`目のインスタンスは、250にスケールダウンすることができ、幅が250単位に達するまで縮小されます。

最初`BoxView`のインスタンスが最小幅なしでスケールダウンする必要がある場合は、 `MinimumWidthRequest`を有効な値 (0 など) に設定する必要があります。

### <a name="minimum-and-absolute-property-values"></a>プロパティの最小値と絶対値

最小値が絶対値よりも大きい場合、動作は定義されていません。 たとえば、が 100 `MinimumWidthRequest`に設定されている`WidthRequest`場合、プロパティは100を超えないようにする必要があります。 最小プロパティ値を指定する場合は、絶対値が最小値よりも大きいことを保証するために、常に絶対値を指定する必要があります。

### <a name="minimum-properties-within-a-grid"></a>グリッド内の最小プロパティ

`Grid`レイアウトには、行と列の相対サイズを調整するための独自のシステムがあります。 レイアウト内`MinimumHeightRequest`で`MinimumWidthRequest`またはを使用しても効果はありません。 `Grid` 詳細については、「 [Xamarin. Forms Grid](~/xamarin-forms/user-interface/layouts/grid.md)」を参照してください。

## <a name="related-links"></a>関連リンク

* [VisualElement API ドキュメント](xref:Xamarin.Forms.VisualElement)
