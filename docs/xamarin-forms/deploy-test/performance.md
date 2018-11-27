---
title: Xamarin.Forms のパフォーマンス
description: Xamarin.Forms アプリケーションのパフォーマンスを高めるための方法は多数あります。 これらの手法をすべて使用することで、CPU で実行される作業量や、アプリケーションで消費されるメモリ量を大幅に減らすことができます。 この記事では、これらの方法について説明します。
ms.prod: xamarin
ms.assetid: 0be84c56-6698-448d-be5a-b4205f1caa9f
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: d7719f231a6d70594985a1158340104d68367ffe
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998616"
---
# <a name="xamarinforms-performance"></a>Xamarin.Forms のパフォーマンス

_Xamarin.Forms アプリケーションのパフォーマンスを高めるための方法は多数あります。これらの手法をすべて使用することで、CPU で実行される作業量や、アプリケーションで消費されるメモリ量を大幅に減らすことができます。この記事では、これらの方法について説明します。_

> [!VIDEO https://youtube.com/embed/RZvdql3Ev0E]

**Evolve 2016: Xamarin.Forms でアプリのパフォーマンスを最適化する**

## <a name="overview"></a>概要

アプリケーションのパフォーマンスの低さは、さまざまな形でアプリケーション自身に現れます。 たとえば、アプリケーションが応答しなかったり、スクロールが遅くなったり、電池の寿命が減ったりすることがあります。 ただし、パフォーマンスを最適化するには、効率的なコードを実装するだけでは済みません。 アプリケーション パフォーマンスのユーザー エクスペリエンスも考慮する必要があります。 たとえば、操作の実行によって、ユーザーが他の操作を実行できない状況にならないようにすることで、ユーザー エクスペリエンスを改善できます。

Xamarin.Forms アプリケーションのパフォーマンスとユーザーの体感パフォーマンスを高めるための方法は多数あります。 それには以下が含まれます。

- [XAML コンパイラを有効にする](#xamlc)
- [適切なレイアウトを選択する](#correctlayout)
- [レイアウト圧縮を有効にする](#layoutcompression)
- [高速レンダラーを使用する](#fastrenderers)
- [不要なバインドを減らす](#databinding)
- [レイアウト パフォーマンスを最適化する](#optimizelayout)
- [ListView パフォーマンスを最適化する](#optimizelistview)
- [イメージ リソースを最適化する](#optimizeimages)
- [ビジュアル ツリーのサイズを減らす](#visualtree)
- [アプリケーション リソース ディクショナリのサイズを減らす](#resourcedictionary)
- [カスタム レンダラー パターンを使用する](#rendererpattern)

> [!NOTE]
>  この記事を読む前に、まず、「[Cross-Platform Performance](~/cross-platform/deploy-test/memory-perf-best-practices.md)」(クロスプラットフォーム パフォーマンス) をお読みください。Xamarin プラットフォームを使用してビルドされたアプリケーションのメモリ使用量とパフォーマンスを改善するための、プラットフォーム固有ではない手法について説明されています。

<a name="xamlc" />

## <a name="enable-the-xaml-compiler"></a>XAML コンパイラを有効にする

XAML は任意で、XAML コンパイラ (XAMLC) を利用し、中間言語 (IL) に直接コンパイルできます。 XAMLC にはさまざまな長所があります。

- XAML のコンパイル時チェックを実行し、エラーがあればユーザーに知らせます。
- XAML 要素の読み込みとインスタンス化の時間を短縮します。
- .xaml ファイルを含めないことで、最終アセンブリのファイル サイズを減らします。

XAMLC は下位互換性のために既定で無効になっています。 ただし、アセンブリ レベルとクラス レベルの両方で有効にできます。 詳しくは、「[Compiling XAML](~/xamarin-forms/xaml/xamlc.md)」 (XAML のコンパイル) を参照してください。

<a name="correctlayout" />

## <a name="choose-the-correct-layout"></a>適切なレイアウトを選択する

複数の子を表示できるが、子が 1 つしかないレイアウトは不経済です。 たとえば、次のコード例では、[`StackLayout`](xref:Xamarin.Forms.StackLayout) と 1 つの子を確認できます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DisplayImage.HomePage">
    <ContentPage.Content>
        <StackLayout>
            <Image Source="waterfront.jpg" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

これは無駄です。[`StackLayout`](xref:Xamarin.Forms.StackLayout) 要素は次のコード例のように削除してください。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DisplayImage.HomePage">
    <ContentPage.Content>
        <Image Source="waterfront.jpg" />
    </ContentPage.Content>
</ContentPage>
```

また、他のレイアウトを組み合わせて特定のレイアウトの外観を再現することはお控えください。不要なレイアウト計算が実行されます。 たとえば、[`StackLayout`](xref:Xamarin.Forms.StackLayout) インスタンスを組み合わせて、[`Grid`](xref:Xamarin.Forms.Grid) レイアウトを再現することはお止めください。 次のコード例は、この悪い行為の見本です。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Details.HomePage"
             Padding="0,20,0,0">
    <ContentPage.Content>
        <StackLayout>
            <StackLayout Orientation="Horizontal">
                <Label Text="Name:" />
                <Entry Placeholder="Enter your name" />
            </StackLayout>
            <StackLayout Orientation="Horizontal">
                <Label Text="Age:" />
                <Entry Placeholder="Enter your age" />
            </StackLayout>
            <StackLayout Orientation="Horizontal">
                <Label Text="Occupation:" />
                <Entry Placeholder="Enter your occupation" />
            </StackLayout>
            <StackLayout Orientation="Horizontal">
                <Label Text="Address:" />
                <Entry Placeholder="Enter your address" />
            </StackLayout>
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

不要なレイアウト計算が行われるため、不経済です。 代わりに、次のコード例のように、[`Grid`](xref:Xamarin.Forms.Grid) を利用すれば望ましいレイアウトを実現できます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Details.HomePage"
             Padding="0,20,0,0">
    <ContentPage.Content>
        <Grid>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="100" />
                <ColumnDefinition Width="*" />
            </Grid.ColumnDefinitions>
            <Grid.RowDefinitions>
                <RowDefinition Height="30" />
                <RowDefinition Height="30" />
                <RowDefinition Height="30" />
                <RowDefinition Height="30" />
            </Grid.RowDefinitions>
            <Label Text="Name:" />
            <Entry Grid.Column="1" Placeholder="Enter your name" />
            <Label Grid.Row="1" Text="Age:" />
            <Entry Grid.Row="1" Grid.Column="1" Placeholder="Enter your age" />
            <Label Grid.Row="2" Text="Occupation:" />
            <Entry Grid.Row="2" Grid.Column="1" Placeholder="Enter your occupation" />
            <Label Grid.Row="3" Text="Address:" />
            <Entry Grid.Row="3" Grid.Column="1" Placeholder="Enter your address" />
        </Grid>
    </ContentPage.Content>
</ContentPage>
```

<a name="layoutcompression" />

## <a name="enable-layout-compression"></a>レイアウト圧縮を有効にする

レイアウト圧縮は、ページのレンダリング パフォーマンスを向上させるために、指定したレイアウトをビジュアル ツリーから削除します。 この操作によって得られるパフォーマンスのメリットは、ページの複雑さ、使用しているオペレーティング システムのバージョン、このアプリケーションが実行されているデバイスによって異なります。 ただし、パフォーマンスが最も大きく向上するのは、古いデバイスの場合です。 詳細については、「[Layout Compression (レイアウト圧縮)](~/xamarin-forms/user-interface/layouts/layout-compression.md)」をご覧ください。

<a name="fastrenderers" />

## <a name="use-fast-renderers"></a>高速レンダラーを使用する

高速レンダラーは、結果として生じるネイティブのコントロール階層をフラット化することで、Android 上の Xamarin.Forms コントロールの増加を抑制し、レンダリング コストを削減します。 この操作では作成されるオブジェクトが少ないので、結果として、ビジュアル ツリーの複雑さが軽減され、メモリの使用量も少なくなり、パフォーマンスをさらに向上させることができます。 詳細については、「[Fast Renderers (高速レンダラー)](~/xamarin-forms/internals/fast-renderers.md)」をご覧ください。

<a name="databinding" />

## <a name="reduce-unnecessary-bindings"></a>不要なバインドを減らす

簡単に静的に設定できるコンテンツにはバインドを利用しないでください。 バインドする必要のないデータをバインドすることには何の利点もありません。バインドはコスト効果が高くありません。 たとえば、`Button.Text = "Accept"` を設定すると、値 "Accept" で [`Button.Text`](xref:Xamarin.Forms.Button.Text) を ViewModel `string` プロパティにバインドするよりオーバーヘッドが少なくなります。

<a name="optimizelayout" />

## <a name="optimize-layout-performance"></a>レイアウト パフォーマンスを最適化する

Xamarin.Forms 2 では、レイアウトの更新に影響する最適化されたレイアウト エンジンが導入されました。 可能な限り最高のレイアウト パフォーマンスを得るために、次のガイドラインに従ってください。

- [`Margin`](xref:Xamarin.Forms.View.Margin) プロパティの値を指定して、レイアウト階層の深さを減らし、ビューのネストが少ないレイアウトを生成できるようにします。 詳細については「[Margin と Padding](~/xamarin-forms/user-interface/layouts/margin-and-padding.md)」を参照してください。
- [`Grid`](xref:Xamarin.Forms.Grid) を使用するときは、[`Auto`](xref:Xamarin.Forms.GridLength.Auto) サイズに設定する行と列を可能な限り減らしてください。 自動サイズ調整された行または列はそれぞれ、レイアウト エンジンに追加のレイアウト計算を実行させることになります。 その代わりに可能であれば、固定サイズの行と列を使用してください。 あるいは、親のツリーがこのレイアウト ガイドラインに従うのであれば、[`GridUnitType.Star`](xref:Xamarin.Forms.GridUnitType.Star) 列挙値を利用し、比率によって領域を占めるように行と列を設定します。
- 必要でない限り、レイアウトの [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) と [`HorizontalOptions`](xref:Xamarin.Forms.View.VerticalOptions) プロパティは設定しないでください。 既定値の [`LayoutOptions.Fill`](xref:Xamarin.Forms.LayoutOptions.Fill) と [`LayoutOptions.FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand) で、レイアウトの最適化が最大になります。 このプロパティの変更にはコストがあり、メモリを消費します。これは、既定値を再設定した場合でも同じです。
- 可能であれば、[`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) は使用しないでください。 CPU で相当な量の作業を実行しなければならなくなります。
- [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) を使用するときは、可能な限り、[`AbsoluteLayout.AutoSize`](xref:Xamarin.Forms.AbsoluteLayout.AutoSize) プロパティを使用しないでください。
- [`StackLayout`](xref:Xamarin.Forms.StackLayout) を使用するときは、[`LayoutOptions.Expands`](xref:Xamarin.Forms.LayoutOptions.Expands) に設定する子を 1 つだけにしてください。 このプロパティにより、指定された子は、`StackLayout` がそれに与えられる最大の領域を占有します。このような計算を複数回実行することは無駄です。
- [`Layout`](xref:Xamarin.Forms.Layout) クラスのメソッドは呼び出さないでください。高いレイアウト計算が実行されることになります。 望ましいレイアウト動作は、[`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX) プロパティと [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY) プロパティを設定することで得られる場合が多いです。 あるいは、[`Layout<View>`](xref:Xamarin.Forms.Layout`1) クラスをサブクラスにして望ましいレイアウト動作を得ます。
- [`Label`](xref:Xamarin.Forms.Label) インスタンスは不必要に更新しないでください。ラベルのサイズを変更すると、画面レイアウト全体が再計算されることがあります。
- 必要でない限り、[`Label.VerticalTextAlignment`](xref:Xamarin.Forms.Label.VerticalTextAlignment) プロパティは設定しないでください。
- 可能であれば、[`Label`](xref:Xamarin.Forms.Label) インスタンスの [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) を [`NoWrap`](xref:Xamarin.Forms.LineBreakMode.NoWrap) に設定してください。

<a name="optimizelistview" />

## <a name="optimize-listview-performance"></a>ListView パフォーマンスを最適化する

[`ListView`](xref:Xamarin.Forms.ListView) コントロールを使用する場合、最適化が必要なさまざまなユーザー エクスペリエンスがあります。

- **初期化** – コントロールが作成されたときに始まり、項目が画面に表示されたときに終わる時間間隔。
- **スクロール** – 一覧をスクロール表示し、UI がタッチ ジェスチャに遅れないようにする機能。
- 項目の追加、削除、選択の**相互作用**。

[`ListView`](xref:Xamarin.Forms.ListView) コントロールは、アプリケーションがデータとセルのテンプレートを提供することを要求します。 その提供方法は、コントロールのパフォーマンスを大きな影響を与えます。 詳しくは、「[ListView のパフォーマンス](~/xamarin-forms/user-interface/listview/performance.md)」を参照してください。

<a name="optimizeimages" />

## <a name="optimize-image-resources"></a>イメージ リソースを最適化する

画像リソースを表示すると、アプリのメモリの占有領域が大幅に増える場合があります。 そのため、必要な場合にのみ作成し、アプリケーションで不要になったらすぐに解放する必要があります。 たとえば、アプリケーションがストリームからデータを読み込み、イメージを表示する場合、必要なときだけストリームが作成されるようにします。また、不要になったら、ストリームを解放するようにします。 これは、ページが作成されたときや [`Page.Appearing`](xref:Xamarin.Forms.Page.Appearing) イベントが発生したときにストリームを作成し、[`Page.Disappearing`](xref:Xamarin.Forms.Page.Disappearing) イベントが発生したときにストリームを解放することで達成されます。

[`ImageSource.FromUri`](xref:Xamarin.Forms.ImageSource.FromUri(System.Uri)) メソッドで表示するためにイメージをダウンロードするとき、[`UriImageSource.CachingEnabled`](xref:Xamarin.Forms.UriImageSource.CachingEnabled) プロパティを `true` に設定することで、ダウンロードしたイメージがキャッシュに保存されます。 詳細については、「[イメージの処理](~/xamarin-forms/user-interface/images.md)」を参照してください。

詳細については、「[イメージ リソースを最適化する](~/cross-platform/deploy-test/memory-perf-best-practices.md#optimizeimages)」を参照してください。

<a name="visualtree" />

## <a name="reduce-the-visual-tree-size"></a>ビジュアル ツリーのサイズを減らす

ページ上の要素の数を減らすと、ページのレンダリングが速くなります。 これは主に 2 つの手法で達成できます。 最初の手法は、表示されない要素を隠すことです。 各要素の [`IsVisible`](xref:Xamarin.Forms.VisualElement.IsVisible) プロパティは、要素をビジュアル ツリーに含めるかどうかを決定します。 そのため、ある要素が他の要素の後ろに隠れているために見えない場合、その要素を取り除くか、その `IsVisible` プロパティを `false` に設定します。

2 番目の手法は、不要な要素を取り除くことです。 たとえば、次のコード例では、一連の [`Label`](xref:Xamarin.Forms.Label) 要素を表示するページ レイアウトを確認できます。

```xaml
<ContentPage.Content>
    <StackLayout>
        <StackLayout Padding="20,20,0,0">
            <Label Text="Hello" />
        </StackLayout>
        <StackLayout Padding="20,20,0,0">
            <Label Text="Welcome to the App!" />
        </StackLayout>
        <StackLayout Padding="20,20,0,0">
            <Label Text="Downloading Data..." />
        </StackLayout>
    </StackLayout>
</ContentPage.Content>
```

要素の数を減らして同じページ レイアウトを維持できます。次のコード例をご覧ください。

```xaml
<ContentPage.Content>
  <StackLayout Padding="20,20,0,0" Spacing="25">
    <Label Text="Hello" />
    <Label Text="Welcome to the App!" />
    <Label Text="Downloading Data..." />
  </StackLayout>
</ContentPage.Content>
```

<a name="resourcedictionary" />

## <a name="reduce-the-application-resource-dictionary-size"></a>アプリケーション リソース ディクショナリのサイズを減らす

アプリケーション全体で使用されるリソースは、重複を回避するために、アプリケーションのリソース ディクショナリに保存してください。 アプリケーション全体で解析しなければならない XAML の量を減らすことができます。 次のコード例では、`HeadingLabelStyle` リソースを確認できます。このリソースはアプリケーション全体で使用されるため、アプリケーションのリソース ディクショナリに定義されています。

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
                xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Resources.App">
     <Application.Resources>
         <ResourceDictionary>
            <Style x:Key="HeadingLabelStyle" TargetType="Label">
                <Setter Property="HorizontalOptions" Value="Center" />
                <Setter Property="FontSize" Value="Large" />
                <Setter Property="TextColor" Value="Red" />
            </Style>
         </ResourceDictionary>
     </Application.Resources>
</Application>
```

ただし、あるページに固有の XAML は、アプリのリソース ディクショナリに含めるべきではありません。アプリのリソースは、ページが必要とするときではなく、アプリケーションの起動時に解析されるためです。リソースが起動ページではないページで使用される場合、そのページのリソース ディクショナリに置いてください。これにより、アプリケーションの起動時に解析される XAML が減ります。 次のコード例では、`HeadingLabelStyle` リソースを確認できます。このリソースは 1 つのページだけに存在するので、ページのリソース ディクショナリに定義されています。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Test.HomePage"
             Padding="0,20,0,0">
     <ContentPage.Resources>
          <ResourceDictionary>
            <Style x:Key="HeadingLabelStyle" TargetType="Label">
                <Setter Property="HorizontalOptions" Value="Center" />
                <Setter Property="FontSize" Value="Large" />
                <Setter Property="TextColor" Value="Red" />
            </Style>
        </ResourceDictionary>
     </ContentPage.Resources>
    <ContentPage.Content>
      ...
    </ContentPage.Content>
</ContentPage>

```

アプリケーション リソースについて詳しくは、「[`Working with Styles`](~/xamarin-forms/user-interface/styles/index.md)」を参照してください。

<a name="rendererpattern" />

## <a name="use-the-custom-renderer-pattern"></a>カスタム レンダラー パターンを使用する

ほとんどのレンダラー クラスは `OnElementChanged` メソッドを公開します。このメソッドは、該当するネイティブ コントロールをレンダリングするために、Xamarin.Forms カスタム コントロールの作成時に呼び出されます。 呼び出し後、プラットフォーム固有の各レンダラー クラスのカスタム レンダラー クラスは、ネイティブ コントロールをインスタンス化し、カスタマイズするために、このメソッドをオーバーライドします。 `SetNativeControl` メソッドはネイティブ コントロールのインスタンス化に使用されます。このメソッドはまた、コントロール参照を `Control` プロパティに割り当てます。

ただし、一部の状況では、`OnElementChanged` メソッドが複数回呼び出されることがあります。 そのため、パフォーマンスに影響を及ぼす可能性があるメモリ リークを防ぐため、新しいネイティブ コントロールをインスタンス化するときには慎重に行う必要があります。 カスタム レンダラーで新しいネイティブ コントロールをインスタンス化するときに使用する手法を次のコード例に示します。

```csharp
protected override void OnElementChanged (ElementChangedEventArgs<NativeListView> e)
{
  base.OnElementChanged (e);

  if (Control == null) {
    // Instantiate the native control
  }

  if (e.OldElement != null) {
    // Unsubscribe from event handlers and cleanup any resources
  }

  if (e.NewElement != null) {
    // Configure the control and subscribe to event handlers
  }
}
```

新しいネイティブ コントロールは、`Control` プロパティが `null` のとき、1 回だけインスタンス化します。 カスタム レンダラーが新しい Xamarin.Forms 要素に関連付けられるときにのみ、コントロールを設定し、イベント ハンドラーを購読します。 同様に、レンダラーが関連付けられている要素が変わるときにのみ、購読していたイベント ハンドラーを解除します。 この手法を採用すると、カスタム レンダラーが効率的に実行され、メモリ リークが発生しません。

カスタム レンダラーに関する詳細については、「[Customizing Controls on Each Platform](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)」 (各プラットフォームのコントロールをカスタマイズする) を参照してください。

## <a name="summary"></a>まとめ

この記事では、Xamarin.Forms アプリケーションのパフォーマンスを高めるための手法について説明しました。 これらの手法をすべて使用することで、CPU で実行される作業量や、アプリケーションで消費されるメモリ量を大幅に減らすことができます。


## <a name="related-links"></a>関連リンク

- [クロスプラットフォームのパフォーマンス](~/cross-platform/deploy-test/memory-perf-best-practices.md)
- [ListView のパフォーマンス](~/xamarin-forms/user-interface/listview/performance.md)
- [高速レンダラー](~/xamarin-forms/internals/fast-renderers.md)
- [レイアウトの圧縮](~/xamarin-forms/user-interface/layouts/layout-compression.md)
- [Xamarin.Forms イメージ拡大/縮小サンプル](https://developer.xamarin.com/samples/xamarin-forms/XamFormsImageResize/)
- [XamlCompilation](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute)
- [XamlCompilationOptions](xref:Xamarin.Forms.Xaml.XamlCompilationOptions)
