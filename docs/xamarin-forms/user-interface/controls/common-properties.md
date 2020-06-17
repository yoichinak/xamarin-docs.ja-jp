---
title: " Xamarin.Forms コモンコントロールのプロパティ、メソッド、およびイベントの説明:" この記事では、派生クラスで一般的に使用される、VisualElement クラスで定義されている共通のプロパティ、メソッド、およびイベントについて説明します。
ms. 製品: xamarin ms. assetId: 85A0CCF5-C1D8-40BB-927F-A4D944E5534D: xamarin-forms author: profexorgeek ms. author: jusjohns ms. date: 08/21/2019 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="xamarinforms-common-control-properties-methods-and-events"></a>Xamarin.Formsコモンコントロールのプロパティ、メソッド、およびイベント

クラスは、 Xamarin.Forms `VisualElement` アプリケーションで使用されるほとんどのコントロールの基本クラスです Xamarin.Forms 。 クラスは、 `VisualElement` 派生クラスで使用される多くの[プロパティ](#properties)、[メソッド](#methods)、および[イベント](#events)を定義します。

## <a name="properties"></a>プロパティ

インスタンスでは、次のプロパティを使用でき `VisualElement` ます。 完全な一覧については、「 [Visualelement API のプロパティ](xref:Xamarin.Forms.VisualElement#properties)」を参照してください。

### [`AnchorX`](xref:Xamarin.Forms.VisualElement.AnchorX)

プロパティは、 `AnchorX` `double` スケールや回転などの変換の X 軸の中心点を定義する値です。 既定値は0.5 です。

### [`AnchorY`](xref:Xamarin.Forms.VisualElement.AnchorY)

プロパティは、 `AnchorY` `double` スケールや回転などの変換の X 軸の中心点を定義する値です。 既定値は0.5 です。

### [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor)

プロパティは、 `BackgroundColor` `Color` コントロールの背景色を決定するです。 設定が解除されている場合、背景は、 `Color` 透明として表示される既定のオブジェクトになります。

### [`Behaviors`](xref:Xamarin.Forms.VisualElement.Behaviors)

`Behaviors`プロパティは `List` オブジェクトのです `Behavior` 。 ビヘイビアーを使用すると、要素をリストに追加することで、要素に再利用可能な機能をアタッチでき `Behaviors` ます。 クラスの詳細については `Behavior` 、「 [ Xamarin.Forms 動作](~/xamarin-forms/app-fundamentals/behaviors/index.md)」を参照してください。

### [`Bounds`](xref:Xamarin.Forms.VisualElement.Bounds)

プロパティは、 `Bounds` `Rectangle` コントロールによって占有される領域を表す読み取り専用のオブジェクトです。 `Bounds`プロパティ値は、レイアウトサイクル中に割り当てられます。 には、 `Rectangle` `struct` 四角形の交点と含有をテストするための便利なプロパティとメソッドが含まれています。 詳細については、「 [ Xamarin.Forms Rectangle API](xref:Xamarin.Forms.Rectangle)」を参照してください。

### [`Effects`](xref:Xamarin.Forms.Element.Effects)

プロパティは、 `Effects` から継承された `List` オブジェクトのです `Effect` `Element` (xref: Xamarin.Forms要素) クラス。 効果を使用すると、ネイティブコントロールをカスタマイズでき、通常は小さなスタイル変更に使用されます。 クラスの詳細については `Effect` 、「 [ Xamarin.Forms 効果](~/xamarin-forms/app-fundamentals/effects/index.md)」を参照してください。

### [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection)

`FlowDirection`プロパティは `FlowDirection` 列挙値です。 フロー方向は、、、またはに設定でき、 `MatchParent` `LeftToRight` `RightToLeft` レイアウトの順序と方向を決定します。 `FlowDirection`通常、プロパティは、右から左に読む言語をサポートするために使用されます。

### [`Height`](xref:Xamarin.Forms.VisualElement.Height)

プロパティは、 `Height` コントロールの描画された高さを記述する読み取り専用の `double` 値です。 プロパティは、 `Height` レイアウトサイクル中に計算され、直接設定することはできません。 コントロールの高さは、高さ[要求プロパティ](#heightrequest)を使用して要求できます。

### [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest)

プロパティは、 `HeightRequest` `double` コントロールの必要な高さを決定する値です。 コントロールの絶対高さが、要求された値と一致しない可能性があります。 詳細については、「[要求のプロパティ](#request-properties)」を参照してください。

### [`InputTransparent`](xref:Xamarin.Forms.VisualElement.InputTransparent)

プロパティは、 `InputTransparent` `bool` コントロールがユーザー入力を受け取るかどうかを決定するです。 既定値は `false` で、要素が入力を受け取ることを保証します。 このプロパティは、設定時に子要素に転送されます。 `InputTransparent`レイアウトクラスでプロパティをに設定する `true` と、レイアウト内のすべての要素が入力を受け取らなくなります。

### [`IsEnabled`](xref:Xamarin.Forms.VisualElement.IsEnabled)

`IsEnabled`プロパティは、 `bool` コントロールがユーザー入力に反応するかどうかを決定する値です。 既定値は `true` です。 このプロパティを false に設定すると、コントロールはユーザー入力を受け付けなくなります。

### [`IsFocused`](xref:Xamarin.Forms.VisualElement.IsFocused)

プロパティは、 `IsFocused` `bool` コントロールが現在フォーカスされているオブジェクトであるかどうかを示す値です。 [`Focus`](#focus)コントロールに対してメソッドを呼び出すと、 `IsFocused` 値が true に設定されます。 メソッドを呼び出す [`Unfocus`](#unfocus) と、このプロパティは false に設定されます。

### [`IsTabStop`](xref:Xamarin.Forms.VisualElement.IsTabStop)

プロパティは、 `IsTabStop` `bool` ユーザーが tab キーを使用してコントロールを進めるときにコントロールがフォーカスを受け取るかどうかを定義する値です。 このプロパティが false の場合、 `TabIndex` プロパティは無効になります。

### [`IsVisible`](xref:Xamarin.Forms.VisualElement.IsVisible)

プロパティは、 `IsVisible` `bool` コントロールが表示されるかどうかを決定する値です。 プロパティが false に設定されているコントロール `IsVisible` は表示されず、レイアウトサイクル中の領域の計算では考慮されず、ユーザー入力を受け付けることはできません。

### [`MinimumHeightRequest`](xref:Xamarin.Forms.VisualElement.MinimumHeightRequest)

プロパティは、 `MinimumHeightRequest` `double` 2 つの要素が制限された領域に対して競合する場合のオーバーフローの処理方法を決定する値です。 プロパティを設定すると、 `MinimumHeightRequest` レイアウトプロセスで、要求された最小の次元に要素をスケールダウンできます。 が指定されていない場合 `MinimumHeightRequest` 、既定値は-1 です。レイアウトプロセスでは、が最小値であると見なされ `HeightRequest` ます。 これは、値のない要素の `MinimumHeightRequest` 高さがスケーラブルではないことを意味します。

詳細については、「[最小要求プロパティ](#minimum-request-properties)」を参照してください。

### [`MinimumWidthRequest`](xref:Xamarin.Forms.VisualElement.MinimumWidthRequest)

プロパティは、 `MinimumWidthRequest` `double` 2 つの要素が制限された領域に対して競合する場合のオーバーフローの処理方法を決定する値です。 プロパティを設定すると、 `MinimumWidthRequest` レイアウトプロセスで、要求された最小の次元に要素をスケールダウンできます。 が指定されていない場合 `MinimumWidthRequest` 、既定値は-1 です。レイアウトプロセスでは、が最小値であると見なされ `WidthRequest` ます。 これは、値のない要素の `MinimumWidthRequest` 幅が拡張されないことを意味します。

詳細については、「[最小要求プロパティ](#minimum-request-properties)」を参照してください。

### [`Opacity`](xref:Xamarin.Forms.VisualElement.Opacity)

プロパティは、 `Opacity` `double` レンダリング中にコントロールの不透明度を決定する0から1の値です。 このプロパティの既定値は1.0 です。 0 ~ 1 の範囲外の値はクランプされます。 プロパティは `Opacity` 、プロパティがの場合にのみ適用され [`IsVisible`](#isvisible) `true` ます。 不透明度は反復的に適用されます。 したがって、親コントロールの不透明度が0.5 で、その子の不透明度が0.5 である場合、子は有効な0.25 不透明度の値を使用してレンダリングされます。 `Opacity`入力コントロールのプロパティを0に設定すると、動作が未定義になります。

### [`Parent`](xref:Xamarin.Forms.Element.Parent)

`Parent` プロパティは、`Element` クラスから継承されます。 このプロパティは `Element` 、コントロールの親であるオブジェクトです。 `Parent`プロパティは、通常、別の要素の子として追加されるときに、要素に対して自動的に設定されます。

### [`Resources`](xref:Xamarin.Forms.VisualElement.Resources)

`Resources`プロパティは、 `ResourceDictionary` 通常、XAML から実行時に設定されるキーと値のペアを使用して設定されるインスタンスです。 このディクショナリを使用すると、アプリケーション開発者は、XAML で定義されたオブジェクトをコンパイル時と実行時の両方で再利用できます。 ディクショナリ内のキーは、 `x:Key` XAML タグの属性から設定されます。 XAML から作成されたオブジェクトは、 `ResourceDictionary` 指定されたキーのに挿入されます。 初期化されます。

詳細については、「[リソースディクショナリ](~/xamarin-forms/xaml/resource-dictionaries.md)」を参照してください。

### [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation)

`Rotation`プロパティは 0 ~ `double` 360 の値で、Z 軸の回転角度を度単位で定義します。 このプロパティの既定値は 0 です。 回転は、値および値に対して相対的に適用され [`AnchorX`](#anchorx) [`AnchorY`](#anchory) ます。

### [`RotationX`](xref:Xamarin.Forms.VisualElement.RotationX)

`RotationX`プロパティは `double` 0 ~ 360 の値で、X 軸の回転角度を度単位で定義します。 このプロパティの既定値は 0 です。 回転は、値および値に対して相対的に適用され [`AnchorX`](#anchorx) [`AnchorY`](#anchory) ます。

### [`RotationY`](xref:Xamarin.Forms.VisualElement.RotationY)

プロパティは、 `RotationY` `double` Y 軸に対する回転を度数で定義する 0 ~ 360 の値です。 このプロパティの既定値は 0 です。 回転は、値および値に対して相対的に適用され [`AnchorX`](#anchorx) [`AnchorY`](#anchory) ます。

### [`Scale`](xref:Xamarin.Forms.VisualElement.Scale)

`Scale`プロパティは、 `double` コントロールのスケールを定義する値です。 このプロパティの既定値は1.0 です。 スケールは、値および値に対して相対的に適用され [`AnchorX`](#anchorx) [`AnchorY`](#anchory) ます。

### [`ScaleX`](xref:Xamarin.Forms.VisualElement.ScaleX)

プロパティは、 `ScaleX` `double` X 軸に沿ってコントロールのスケールを定義する値です。 このプロパティの既定値は1.0 です。 `ScaleX`プロパティは、値に対して相対的に適用され [`AnchorX`](#anchorx) ます。

### [`ScaleY`](xref:Xamarin.Forms.VisualElement.ScaleY)

プロパティは、 `ScaleY` `double` Y 軸に沿ったコントロールのスケールを定義する値です。 このプロパティの既定値は1.0 です。 `ScaleY`プロパティは、値に対して相対的に適用され [`AnchorY`](#anchory) ます。

### [`Style`](xref:Xamarin.Forms.NavigableElement.Style)

`Style` プロパティは、`NavigableElement` クラスから継承されます。 このプロパティは、クラスのインスタンスです `Style` 。 クラスには、 `Style` ビジュアル要素の外観と動作を定義するトリガー、セッター、および動作が含まれています。 詳細については、「 [ Xamarin.Forms XAML スタイル](~/xamarin-forms/user-interface/styles/xaml/index.md)」を参照してください。

### [`StyleClass`](xref:Xamarin.Forms.NavigableElement.StyleClass)

`StyleClass`プロパティは、 `string` クラスの名前を表すオブジェクトのリストです `Style` 。 このプロパティは、`NavigableElement` クラスから継承されます。 プロパティを使用すると、 `StyleClass` 複数のスタイル属性をインスタンスに適用でき `VisualElement` ます。 詳細については、「 [ Xamarin.Forms スタイルクラス](~/xamarin-forms/user-interface/styles/xaml/style-class.md)」を参照してください。

### [`TabIndex`](xref:Xamarin.Forms.VisualElement.TabIndex)

`TabIndex`プロパティは、 `int` tab キーを使用してコントロールを進めるときに制御順序を定義する値です。 プロパティは、 `TabIndex` クラスが実装するインターフェイスで定義されたプロパティの実装 `ITabStopElement` `VisualElement` です。

### [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX)

`TranslationX`プロパティは、 `double` X 軸に適用するデルタ平行移動を定義する値です。 翻訳はレイアウトの後に適用され、通常はアニメーションを適用するために使用されます。 親コンテナーの境界の外側にある要素を変換すると、入力が禁止されます。

詳細については、「 [」の Xamarin.Forms 「アニメーション](~/xamarin-forms/user-interface/animation/index.md)」を参照してください。

### [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY)

`TranslationY`プロパティは、 `double` Y 軸に適用するデルタ平行移動を定義する値です。 翻訳はレイアウトの後に適用され、通常はアニメーションを適用するために使用されます。 親コンテナーの境界の外側にある要素を変換すると、入力が禁止されます。

詳細については、「 [」の Xamarin.Forms 「アニメーション](~/xamarin-forms/user-interface/animation/index.md)」を参照してください。

### [`Triggers`](xref:Xamarin.Forms.VisualElement.Triggers)

プロパティは、 `Triggers` オブジェクトの読み取り専用 `List` です `TriggerBase` 。 トリガーを使用すると、アプリケーション開発者は、イベントまたはプロパティの変更に応じてコントロールの外観を変更するアクションを XAML で表現できます。 詳細については、「 [ Xamarin.Forms トリガー](~/xamarin-forms/app-fundamentals/triggers.md)」を参照してください。

### [`Visual`](xref:Xamarin.Forms.VisualElement.Visual)

`Visual`プロパティは、 `IVisual` レンダラーを作成し、インスタンスに対して選択的に適用できるようにするインスタンスです `VisualElement` 。 `Visual`プロパティはその親に一致するように設定されているため、コンポーネントでレンダラーを定義すると、そのコンポーネントのすべての子にも適用されます。 コントロールまたはその先祖にカスタムレンダラーが設定されていない場合は、既定の Xamarin.Forms レンダラーが使用されます。 詳細については、「 [ Xamarin.Forms ビジュアル](~/xamarin-forms/user-interface/visual/index.md)」を参照してください。

### [`Width`](xref:Xamarin.Forms.VisualElement.Width)

プロパティは、 `Width` `double` コントロールの描画幅を記述する読み取り専用の値です。 プロパティは、 `Width` レイアウトサイクル中に計算され、直接設定することはできません。 Width[要求プロパティ](#widthrequest)を使用して、コントロールの幅を要求できます。

### [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest)

プロパティは、 `WidthRequest` `double` コントロールの適切な幅を決定する値です。 コントロールの絶対幅が、要求された値と一致しない可能性があります。 詳細については、「[要求のプロパティ](#request-properties)」を参照してください。

### [`X`](xref:Xamarin.Forms.VisualElement.X)

`X`プロパティは、 `double` コントロールの現在の X 位置を記述する読み取り専用の値です。

### [`Y`](xref:Xamarin.Forms.VisualElement.Y)

`Y`プロパティは、 `double` コントロールの現在の Y 位置を記述する読み取り専用の値です。

## <a name="methods"></a>メソッド

クラスでは、次のメソッドを使用でき `VisualElement` ます。 完全な一覧については、「 [Visualelement API メソッド](xref:Xamarin.Forms.VisualElement#methods)」を参照してください。

### [`FindByName`](xref:Xamarin.Forms.Element.FindByName*)

`FindByName`メソッドはクラスから継承され、 `Element` 次のシグネチャを持ちます。

```csharp
public object FindByName (string name)
```

このメソッドは、指定された引数のすべての子要素を検索 `name` し、指定された名前を持つ要素を返します。 一致が見つからなかった場合は、`null` を返します。

### [`Focus`](xref:Xamarin.Forms.VisualElement.Focus)

メソッドは、 `Focus` 要素にフォーカスを設定しようとします。 このメソッドのシグネチャは次のとおりです。

```csharp
public bool Focus ()
```

この `Focus` メソッドは、 `true` キーボードフォーカスが正常に設定された場合はを返し、 `false` メソッド呼び出しでフォーカスが変更されなかった場合はを返します。 要素は、このメソッドが機能するためにフォーカスを受け取ることができる必要があります。 `Focus`オフスクリーンまたは実現されていない要素に対してメソッドを呼び出すと、動作が未定義になります。

### [`Unfocus`](xref:Xamarin.Forms.VisualElement.Unfocus)

メソッドは、 `Unfocus` 要素のフォーカスを解除しようとします。 このメソッドのシグネチャは次のとおりです。

```csharp
public void Unfocus ()
```

要素には、このメソッドが機能するために既にフォーカスがある必要があります。

## <a name="events"></a>イベント

クラスでは、次のイベントを使用でき `VisualElement` ます。 完全な一覧については、「 [ Xamarin.Forms Visualelement イベント](xref:Xamarin.Forms.VisualElement#events)」を参照してください。

### [`Focused`](xref:Xamarin.Forms.VisualElement.Focused)

`Focused`イベントは、 `VisualElement` インスタンスがフォーカスを受け取ったときに発生します。 このイベントはスタックを経由せず Xamarin.Forms 、ネイティブコントロールから直接取得されます。 このイベントは、プロパティ set アクセス操作子によって生成され [`IsFocused`](#isfocused) ます。

### [`SizeChanged`](xref:Xamarin.Forms.VisualElement.SizeChanged)

イベントは、 `SizeChanged` `VisualElement` インスタンス `Height` またはプロパティが変更されるたびに発生し `Width` ます。 開発者が変更後のイベントに応答するのではなく、サイズの変更に直接応答する場合は、代わりに仮想メソッドを実装する必要があり [`OnSizeAllocated`](xref:Xamarin.Forms.VisualElement.OnSizeAllocated*) ます。

### [`Unfocused`](xref:Xamarin.Forms.VisualElement.Unfocused)

イベントは、 `Unfocused` インスタンスがフォーカスを失ったときに発生し `VisualElement` ます。 このイベントはスタックを経由せず Xamarin.Forms 、ネイティブコントロールから直接取得されます。 このイベントは、プロパティ set アクセス操作子によって生成され [`IsFocused`](#isfocused) ます。

## <a name="units-of-measurement"></a>測定単位

Android、iOS、UWP の各プラットフォームには、デバイスによって異なる測定単位があります。 Xamarin.Formsでは、デバイスとプラットフォームの単位を正規化するプラットフォームに依存しない測定単位が使用されます。 には、1インチあたり160ユニットまたは 1 cm あたりの64ユニットがあり Xamarin.Forms ます。

## <a name="request-properties"></a>要求のプロパティ

名前に "request" が含まれているプロパティは、目的の値を定義しますが、実際に表示される値とは一致しない可能性があります。 たとえば、 `HeightRequest` は150に設定されている場合がありますが、レイアウトでは100ユニットの領域のみが許可されている場合、 `Height` コントロールのレンダリングは100のみになります。 表示されるサイズは、使用可能な領域と含まれるコンポーネントの影響を受けます。

## <a name="minimum-request-properties"></a>最小要求プロパティ

最小要求プロパティに `MinimumHeightRequest` は、とが含ま `MinimumWidthRequest` れます。これは、要素が互いにどのようにオーバーフローしているかをより正確に制御できるようにするためのものです。 ただし、これらのプロパティに関連するレイアウト動作には、重要な考慮事項がいくつかあります。

### <a name="unspecified-minimum-property-values"></a>指定されていない最小プロパティ値

最小値が設定されていない場合、minimum プロパティの既定値は-1 です。 レイアウトプロセスでは、この値は無視され、絶対値が最小値であると見なされます。 この動作の実際の結果として、最小値が指定されていない要素は圧縮され**ません**。 最小値を指定した要素**が縮小され**ます。

次の XAML は、水平方向に2つの要素を示してい `BoxView` `StackLayout` ます。

```xaml
<StackLayout Orientation="Horizontal">
    <BoxView HeightRequest="100" BackgroundColor="Purple" WidthRequest="500"></BoxView>
    <BoxView HeightRequest="100" BackgroundColor="Green" WidthRequest="500" MinimumWidthRequest="250"></BoxView>
</StackLayout>
```

最初の `BoxView` インスタンスは、幅500を要求し、最小幅を指定していません。 2番目のインスタンスは、 `BoxView` 幅500と最小幅250を要求します。 親 `StackLayout` 要素が、要求された幅で両方のコンポーネントを格納するのに十分な幅ではない場合、 `BoxView` レイアウトプロセスでは、他の有効な最小値が指定されていないため、最初のインスタンスは最小幅500を持つと見なされます。 2番目の `BoxView` インスタンスは、250にスケールダウンすることができ、幅が250単位に達するまで縮小されます。

最初の `BoxView` インスタンスが最小幅なしでスケールダウンする必要がある場合は、を `MinimumWidthRequest` 有効な値 (0 など) に設定する必要があります。

### <a name="minimum-and-absolute-property-values"></a>プロパティの最小値と絶対値

最小値が絶対値よりも大きい場合、動作は定義されていません。 たとえば、 `WidthRequest` が100に設定されている場合、 `MinimumWidthRequest` プロパティは100を超えないようにする必要があります。 最小プロパティ値を指定する場合は、絶対値が最小値よりも大きいことを保証するために、常に絶対値を指定する必要があります。

### <a name="minimum-properties-within-a-grid"></a>グリッド内の最小プロパティ

`Grid`レイアウトには、行と列の相対サイズを調整するための独自のシステムがあります。 レイアウト内でまたはを使用しても効果はあり `MinimumWidthRequest` `MinimumHeightRequest` `Grid` ません。 詳細については、「 [ Xamarin.Forms Grid](~/xamarin-forms/user-interface/layouts/grid.md)」を参照してください。

## <a name="related-links"></a>関連リンク

* [VisualElement API ドキュメント](xref:Xamarin.Forms.VisualElement)
