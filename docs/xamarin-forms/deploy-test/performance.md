---
title: Xamarin.Forms アプリ パフォーマンスの改善
description: Xamarin.Forms アプリケーションのパフォーマンスを高めるための方法は多数あります。 これらの手法をすべて使用することで、CPU で実行される作業量や、アプリケーションで消費されるメモリ量を大幅に減らすことができます。
ms.prod: xamarin
ms.assetid: 0be84c56-6698-448d-be5a-b4205f1caa9f
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/01/2019
ms.openlocfilehash: 0841cb0cbe97644f3bb53105887f3adadf9bf6c5
ms.sourcegitcommit: 266e75fa6893d3732e4e2c0c8e79c62be2804468
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/06/2019
ms.locfileid: "68820946"
---
# <a name="improve-xamarinforms-app-performance"></a>Xamarin.Forms アプリ パフォーマンスの改善

> [!VIDEO https://youtube.com/embed/RZvdql3Ev0E]

**Evolve 2016:Xamarin.Forms でアプリのパフォーマンスを最適化する**

アプリケーションのパフォーマンスの低さは、さまざまな形でアプリケーションに現れます。 たとえば、アプリケーションが応答しなかったり、スクロールが遅くなったり、デバイス電池の寿命が縮まったりすることがあります。 ただし、パフォーマンスを最適化するには、単に効率的なコードを実装するだけでは不十分です。 アプリケーション パフォーマンスに関わるユーザー エクスペリエンスも考慮する必要があります。 たとえば、操作の実行によって、ユーザーが他の操作を実行できない状況にならないようにすることで、ユーザー エクスペリエンスを改善できます。

Xamarin.Forms アプリケーションのパフォーマンスとユーザーの体感パフォーマンスを高めるための方法は数多くあります。 これらの手法をすべて使用することで、CPU で実行される作業量や、アプリケーションで消費されるメモリ量を大幅に減らすことができます。

> [!NOTE]
> この記事を読む前に、まず、「[Cross-Platform Performance](~/cross-platform/deploy-test/memory-perf-best-practices.md)」(クロスプラットフォーム パフォーマンス) をお読みください。Xamarin プラットフォームを使用してビルドされたアプリケーションのメモリ使用量とパフォーマンスを改善するための、プラットフォーム固有ではない手法について説明されています。

## <a name="enable-the-xaml-compiler"></a>XAML コンパイラを有効にする

XAML は任意で、XAML コンパイラ (XAMLC) を利用し、中間言語 (IL) に直接コンパイルできます。 XAMLC にはさまざまな長所があります。

- XAML のコンパイル時チェックを実行し、エラーがあればユーザーに知らせます。
- XAML 要素の読み込みとインスタンス化の時間を短縮します。
- .xaml ファイルを含めないことで、最終アセンブリのファイル サイズを減らします。

XAMLC は、新しい Xamarin. Forms ソリューションでは既定で有効になっています。 ただし、旧版のソリューションであれば、有効にしなければならない場合があります。 詳しくは、「[Compiling XAML](~/xamarin-forms/xaml/xamlc.md)」 (XAML のコンパイル) を参照してください。

## <a name="use-compiled-bindings"></a>コンパイル済みのバインドを使用する

コンパイル済みのバインドでは、リフレクションを利用する実行時ではなく、コンパイル時にバインド式を解決することで、Xamarin.Forms アプリケーションでのデータ バインディングのパフォーマンスが向上します。 バインド式をコンパイルすると、通常、従来のバインドの 8 から 20 倍の速さでバインドを解決するコンパイル済みのコードが生成されます。 詳しくは、「[コンパイル済みのバインド](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md)」を参照してください。

## <a name="reduce-unnecessary-bindings"></a>不要なバインドを減らす

簡単に静的に設定できるコンテンツにはバインドを利用しないでください。 バインドする必要のないデータをバインドすることには何の利点もありません。バインドはコスト効果が高くありません。 たとえば、`Button.Text = "Accept"` を設定すると、値 "Accept" で [`Button.Text`](xref:Xamarin.Forms.Button.Text) を ViewModel `string` プロパティにバインドするよりオーバーヘッドが少なくなります。

## <a name="use-fast-renderers"></a>高速レンダラーを使用する

高速レンダラーは、結果として生じるネイティブのコントロール階層をフラット化することで、Android 上の Xamarin.Forms コントロールの増加を抑制し、レンダリング コストを削減します。 この操作では作成されるオブジェクトが少ないので、結果として、ビジュアル ツリーの複雑さが軽減され、メモリの使用量も少なくなり、パフォーマンスをさらに向上させることができます。

Xamarin. Forms 4.0 以降では、`FormsAppCompatActivity` を対象とするすべてのアプリケーションで、既定で高速レンダラーが使用されます。 詳細については、「[Fast Renderers (高速レンダラー)](~/xamarin-forms/internals/fast-renderers.md)」をご覧ください。

## <a name="enable-startup-tracing-on-android"></a>Android でスタートアップ トレースを有効にする

Android の事前 (AOT) コンパイルでは、より大きな APK の作成を犠牲にすることで、ジャスト イン タイム (JIT) アプリケーションの起動オーバーヘッドとメモリ使用量が最小限に抑えられます。 別の方法としては、スタートアップ トレースの使用があります。この場合、従来の AOT コンパイルとは違い、Android APK のサイズと起動時間との折り合いになります。

アプリケーションの大部分を可能な限りアンマネージド コードにコンパイルする代わりに、スタートアップ トレースでは、空の Xamarin.Forms アプリケーションでアプリケーション スタートアップの最も負荷がかかる部分を表わすマネージド メソッドのセットのみをコンパイルします。 この方法では、従来の AOT コンパイルとは異なり、APK のサイズが縮小されますが、起動が同様に改善されます。

## <a name="enable-layout-compression"></a>レイアウト圧縮を有効にする

レイアウト圧縮は、ページのレンダリング パフォーマンスを向上させるために、指定したレイアウトをビジュアル ツリーから削除します。 この操作によって得られるパフォーマンスのメリットは、ページの複雑さ、使用しているオペレーティング システムのバージョン、このアプリケーションが実行されているデバイスによって異なります。 ただし、パフォーマンスが最も大きく向上するのは、古いデバイスの場合です。 詳細については、「[Layout Compression (レイアウト圧縮)](~/xamarin-forms/user-interface/layouts/layout-compression.md)」をご覧ください。

## <a name="choose-the-correct-layout"></a>適切なレイアウトを選択する

複数の子を表示できるが、子が 1 つしかないレイアウトは不経済です。 たとえば、次のコード例では、[`StackLayout`](xref:Xamarin.Forms.StackLayout) と 1 つの子を確認できます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DisplayImage.HomePage">
    <StackLayout>
        <Image Source="waterfront.jpg" />
    </StackLayout>
</ContentPage>
```

これは無駄です。[`StackLayout`](xref:Xamarin.Forms.StackLayout) 要素は次のコード例のように削除してください。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DisplayImage.HomePage">
    <Image Source="waterfront.jpg" />
</ContentPage>
```

また、他のレイアウトを組み合わせて特定のレイアウトの外観を再現することはお控えください。不要なレイアウト計算が実行されます。 たとえば、[`Grid`](xref:Xamarin.Forms.Grid) インスタンスを組み合わせて、[`StackLayout`](xref:Xamarin.Forms.StackLayout) レイアウトを再現しないでください。 次のコード例は、この悪い見本です。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Details.HomePage"
             Padding="0,20,0,0">
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
</ContentPage>
```

不要なレイアウト計算が行われるため、不経済です。 代わりに、次のコード例のように、[`Grid`](xref:Xamarin.Forms.Grid) を利用すれば望ましいレイアウトを実現できます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Details.HomePage"
             Padding="0,20,0,0">
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
</ContentPage>
```

## <a name="optimize-layout-performance"></a>レイアウトのパフォーマンスを最適化する

可能な限り最高のレイアウト パフォーマンスを得るために、次のガイドラインに従ってください。

- [`Margin`](xref:Xamarin.Forms.View.Margin) プロパティの値を指定して、レイアウト階層の深さを減らし、ビューのネストが少ないレイアウトを生成できるようにします。 詳細については「[Margin と Padding](~/xamarin-forms/user-interface/layouts/margin-and-padding.md)」を参照してください。
- [`Grid`](xref:Xamarin.Forms.Grid) を使用するときは、[`Auto`](xref:Xamarin.Forms.GridLength.Auto) サイズに設定する行と列を可能な限り減らしてください。 自動サイズ調整された行または列はそれぞれ、レイアウト エンジンに追加のレイアウト計算を実行させることになります。 その代わりに可能であれば、固定サイズの行と列を使用してください。 あるいは、親のツリーがこのレイアウト ガイドラインに従うのであれば、[`GridUnitType.Star`](xref:Xamarin.Forms.GridUnitType.Star) 列挙値を利用し、比率によって領域を占めるように行と列を設定します。
- 必要でない限り、レイアウトの [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) と ´´[`HorizontalOptions`](xref:Xamarin.Forms.View.VerticalOptions) プロパティは設定しないでください。 既定値の [`LayoutOptions.Fill`](xref:Xamarin.Forms.LayoutOptions.Fill) と [`LayoutOptions.FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand) で、レイアウトの最適化が最大になります。 このプロパティの変更にはコストがあり、メモリを消費します。これは、既定値を再設定した場合でも同じです。
- 可能であれば、[`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) は使用しないでください。 CPU で相当な量の作業を実行しなければならなくなります。
- [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) を使用するときは、可能な限り、[`AbsoluteLayout.AutoSize`](xref:Xamarin.Forms.AbsoluteLayout.AutoSize) プロパティを使用しないでください。
- [`StackLayout`](xref:Xamarin.Forms.StackLayout) を使用するときは、[`LayoutOptions.Expands`](xref:Xamarin.Forms.LayoutOptions.Expands) に設定する子を 1 つだけにしてください。 このプロパティにより、指定された子は、`StackLayout` がそれに与えられる最大の領域を占有します。このような計算を複数回実行することは無駄です。
- [`Layout`](xref:Xamarin.Forms.Layout) クラスのメソッドの呼び出しは避けてください。負荷のかかるレイアウト計算が実行されていまいます。 望ましいレイアウト動作は、[`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX) プロパティと [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY) プロパティを設定することで得られる場合が多いです。 あるいは、[`Layout<View>`](xref:Xamarin.Forms.Layout`1) クラスをサブクラスにして望ましいレイアウト動作を得ます。
- [`Label`](xref:Xamarin.Forms.Label) インスタンスは不必要に更新しないでください。ラベルのサイズを変更すると、画面レイアウト全体が再計算されることがあります。
- 必要でない限り、[`Label.VerticalTextAlignment`](xref:Xamarin.Forms.Label.VerticalTextAlignment) プロパティは設定しないでください。
- 可能であれば、[`Label`](xref:Xamarin.Forms.Label) インスタンスの [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) を [`NoWrap`](xref:Xamarin.Forms.LineBreakMode.NoWrap) に設定してください。

## <a name="choose-a-dependency-injection-container-carefully"></a>依存関係挿入コンテナーを慎重に選択する

依存関係挿入コンテナーにより、モバイル アプリケーションに追加のパフォーマンス制約が導入されます。 コンテナーで型を登録し、解決すると、パフォーマンス コストが発生します。特に、アプリのページ ナビゲーションごとに依存関係が再構築される場合には、各型を作成するために、コンテナーでリフレクションが使用されるからです。 依存関係が多いか深い場合、作成コストが大幅に増加する可能性があります。 また、通常、アプリケーションの起動中に発生する型の登録は、使用されるコンテナーによっては、起動時間に顕著な影響を与える可能性があります。

代替手段としては、ファクトリを使用して手動実装することで、依存関係挿入の性能を上げることができます。

## <a name="create-shell-applications"></a>シェル アプリケーションを作成する

Xamarin.Forms シェル アプリケーションでは、ポップアップとタブに基づいて、厳格なナビゲーション エクスペリエンスを提供しています。 アプリケーション ユーザー エクスペリエンスをシェルで実装できる場合、そうすることが推奨されます。 シェル アプリケーションは、芳しくない起動体験を避ける目的で役立ちます。["TabbedPage"](xref:Xamarin.Forms.TabbedPage) を使用するアプリケーションで発生するようにアプリケーションの起動時ではなく、ナビゲーションに対する応答で必要に応じてページが作成されるためです。 詳細は、「[Xamarin.Forms シェル](~/xamarin-forms/app-fundamentals/shell/index.md)」を参照してください。

## <a name="use-collectionview-instead-of-listview"></a>ListView の代わりに CollectionView を使用する

[`CollectionView`](xref:Xamarin.Forms.CollectionView) は、さまざまなレイアウト仕様を使用してデータを一覧表示するためのビューです。 [`ListView`](xref:Xamarin.Forms.ListView) の柔軟で高性能な代替となります。 詳細は、「[Xamarin.Forms CollectionView](~/xamarin-forms/user-interface/collectionview/index.md)」を参照してください。

## <a name="optimize-listview-performance"></a>ListView パフォーマンスを最適化する

[`ListView`](xref:Xamarin.Forms.ListView) を使用する場合、最適化が必要なさまざまなユーザー エクスペリエンスがあります。

- **初期化** – コントロールが作成されたときに始まり、項目が画面に表示されたときに終わる時間間隔。
- **スクロール** – 一覧をスクロール表示し、UI がタッチ ジェスチャに遅れないようにする機能。
- 項目の追加、削除、選択の**相互作用**。

[`ListView`](xref:Xamarin.Forms.ListView) コントロールは、アプリケーションがデータとセルのテンプレートを提供することを要求します。 その提供方法は、コントロールのパフォーマンスを大きな影響を与えます。 詳しくは、「[ListView のパフォーマンス](~/xamarin-forms/user-interface/listview/performance.md)」を参照してください。

## <a name="optimize-image-resources"></a>イメージ リソースを最適化する

画像リソースを表示すると、アプリケーションのメモリの占有領域が大幅に増える場合があります。 そのため、必要な場合にのみ作成し、アプリケーションで不要になったらすぐに解放する必要があります。 たとえば、アプリケーションがストリームからデータを読み込み、イメージを表示する場合、必要なときだけストリームが作成されるようにします。また、不要になったら、ストリームを解放するようにします。 これは、ページが作成されたときや [`Page.Appearing`](xref:Xamarin.Forms.Page.Appearing) イベントが発生したときにストリームを作成し、[`Page.Disappearing`](xref:Xamarin.Forms.Page.Disappearing) イベントが発生したときにストリームを解放することで達成されます。

[`ImageSource.FromUri`](xref:Xamarin.Forms.ImageSource.FromUri(System.Uri)) メソッドで表示するためにイメージをダウンロードするとき、[`UriImageSource.CachingEnabled`](xref:Xamarin.Forms.UriImageSource.CachingEnabled) プロパティを `true` に設定することで、ダウンロードしたイメージがキャッシュに保存されます。 詳細については、「[イメージの処理](~/xamarin-forms/user-interface/images.md)」を参照してください。

詳細については、「[イメージ リソースを最適化する](~/cross-platform/deploy-test/memory-perf-best-practices.md#optimizeimages)」を参照してください。

## <a name="reduce-the-visual-tree-size"></a>ビジュアル ツリーのサイズを減らす

ページ上の要素の数を減らすと、ページのレンダリングが速くなります。 これは主に 2 つの手法で達成できます。 最初の手法は、表示されない要素を隠すことです。 各要素の [`IsVisible`](xref:Xamarin.Forms.VisualElement.IsVisible) プロパティは、要素をビジュアル ツリーに含めるかどうかを決定します。 そのため、ある要素が他の要素の後ろに隠れているために見えない場合、その要素を取り除くか、その `IsVisible` プロパティを `false` に設定します。

2 番目の手法は、不要な要素を取り除くことです。 たとえば、次のコード例では、複数の [`Label`](xref:Xamarin.Forms.Label) オブジェクトが含まれるページ レイアウトを確認できます。

```xaml
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
```

要素の数を減らして同じページ レイアウトを維持できます。次のコード例をご覧ください。

```xaml
<StackLayout Padding="20,35,20,20" Spacing="25">
  <Label Text="Hello" />
  <Label Text="Welcome to the App!" />
  <Label Text="Downloading Data..." />
</StackLayout>
```

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

ただし、あるページに固有の XAML は、アプリケーションのリソース ディクショナリに含めるべきではありません。アプリのリソースは、ページが必要とするときではなく、アプリケーションの起動時に解析されるためです。 リソースが起動ページではないページで使用される場合、そのページのリソース ディクショナリに置いてください。これにより、アプリケーションの起動時に解析される XAML が減ります。 次のコード例では、`HeadingLabelStyle` リソースを確認できます。このリソースは 1 つのページだけに存在するので、ページのリソース ディクショナリに定義されています。

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
    ...
</ContentPage>
```

アプリケーション リソースについて詳しくは、「[XAML スタイル](~/xamarin-forms/user-interface/styles/xaml/index.md)」を参照してください。

## <a name="use-the-custom-renderer-pattern"></a>カスタム レンダラー パターンを使用する

ほとんどの Xamarin.Forms レンダラー クラスは `OnElementChanged` メソッドを公開します。このメソッドは、該当するネイティブ コントロールをレンダリングするために、Xamarin.Forms カスタム コントロールの作成時に呼び出されます。 呼び出し後、各プラットフォーム プロジェクトのカスタム レンダラー クラスにより、ネイティブ コントロールをインスタンス化し、カスタマイズするために、このメソッドがオーバーライドされます。 `SetNativeControl` メソッドはネイティブ コントロールのインスタンス化に使用されます。このメソッドはまた、コントロール参照を `Control` プロパティに割り当てます。

ただし、一部の状況では、`OnElementChanged` メソッドが複数回呼び出されることがあります。 そのため、パフォーマンスに影響を及ぼす可能性があるメモリ リークを防ぐため、新しいネイティブ コントロールをインスタンス化するときには慎重に行う必要があります。 カスタム レンダラーで新しいネイティブ コントロールをインスタンス化するときに使用する手法を次のコード例に示します。

```csharp
protected override void OnElementChanged (ElementChangedEventArgs<NativeListView> e)
{
  base.OnElementChanged (e);

  if (e.OldElement != null)
  {
    // Unsubscribe from event handlers and cleanup any resources
  }

  if (e.NewElement != null)
  {
    if (Control == null)
    {
      // Instantiate the native control with the SetNativeControl method
    }
    // Configure the control and subscribe to event handlers
  }
}
```

新しいネイティブ コントロールは、`Control` プロパティが `null` のとき、1 回だけインスタンス化します。 さらに、カスタム レンダラーが新しい Xamarin.Forms 要素に関連付けられるときにのみ、コントロールを作成および構成し、イベント ハンドラーを登録する必要があります。 同様に、レンダラーが関連付けられている要素が変わるときにのみ、サブスクライブしていたイベント ハンドラーを登録解除します。 この手法を採用すると、カスタム レンダラーが効率的に実行され、メモリ リークが発生しません。

> [!IMPORTANT]
> `SetNativeControl` メソッドは、`e.NewElement` プロパティが `null` ではなく、`Control` プロパティが `null` の場合にのみ呼び出します。

カスタム レンダラーに関する詳細については、「[Customizing Controls on Each Platform](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)」 (各プラットフォームのコントロールをカスタマイズする) を参照してください。

## <a name="related-links"></a>関連リンク

- [クロスプラットフォームのパフォーマンス](~/cross-platform/deploy-test/memory-perf-best-practices.md)
- [XAML をコンパイルする](~/xamarin-forms/xaml/xamlc.md)
- [コンパイル済みのバインディング](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md)
- [高速レンダラー](~/xamarin-forms/internals/fast-renderers.md)
- [レイアウトの圧縮](~/xamarin-forms/user-interface/layouts/layout-compression.md)
- [Xamarin.Forms シェル](~/xamarin-forms/app-fundamentals/shell/index.md)
- [Xamarin.Forms CollectionView](~/xamarin-forms/user-interface/collectionview/index.md)
- [ListView のパフォーマンス](~/xamarin-forms/user-interface/listview/performance.md)
- [イメージ リソースを最適化する](~/cross-platform/deploy-test/memory-perf-best-practices.md#optimizeimages)
- [XAML スタイル](~/xamarin-forms/user-interface/styles/xaml/index.md)
- [各プラットフォームでコントロールをカスタマイズする](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
