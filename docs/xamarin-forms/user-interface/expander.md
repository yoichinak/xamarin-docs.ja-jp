---
title: Xamarin.FormsExpander
description: Xamarin.Formsエキスパンダーコントロールは、任意のコンテンツをホストするための拡張可能なコンテナーを提供します。 コンテンツを表示または非表示にするには、エキスパンダーヘッダーをタップします。
ms.prod: xamarin
ms.assetid: 381DCB55-522D-4414-B45B-E8DD70AA9985
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/15/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 5e9afa0f6d27003891963af5715d5721e3129306
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84129572"
---
# <a name="xamarinforms-expander"></a>Xamarin.FormsExpander

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-expanderdemos/)

コントロールには、 Xamarin.Forms `Expander` 任意のコンテンツをホストするための拡張可能なコンテナーが用意されています。 コントロールはヘッダーとコンテンツを持ち、ヘッダーをタップすることによってコンテンツを表示または非表示にし `Expander` ます。 `Expander`ヘッダーのみが表示されている場合は、が折りたたまれてい `Expander` ます。 *collapsed* `Expander`コンテンツが表示されると `Expander` 、が*展開*されます。

次のスクリーンショットは、折りたたまれて展開された状態のを示してい `Expander` ます。赤いボックスは、ヘッダーとコンテンツを示しています。

![展開された状態の展開コントロールのスクリーンショット (iOS と Android)](expander-images/expander.png "IOS と Android の展開")

> [!IMPORTANT]
> `Expander`は現在試験段階であり、フラグを設定することによってのみ使用でき `Expander_Experimental` ます。 詳細については、「試験的な[フラグ](~/xamarin-forms/internals/experimental-flags.md)」を参照してください。
>
> また、コントロールは `Expander` 名前空間に完全に実装され `Xamarin.Forms` ます。 このため、でサポートされているすべてのプラットフォームで使用でき Xamarin.Forms ます。

コントロールは、 `Expander` 次のプロパティを定義します。

- `CollapseAnimationEasing`型の [`Easing`](xref:Xamarin.Forms.Easing) 。これは、コンテンツの折りたたみ時に適用されるイージング関数を表し `Expander` ます。
- `CollapseAnimationLength`が折りたたまれている `uint` ときのアニメーションの継続時間を定義する型の `Expander` 。 このプロパティの既定値は250ミリ秒です。
- `Command`型の `ICommand` 。これは、 `Expander` ヘッダーがタップされたときに実行されます。
- `CommandParameter`: `object` 型、`Command` に渡されるパラメーター。
- `Content`[`View`](xref:Xamarin.Forms.View)が展開されたときに表示されるコンテンツを定義する、型の `Expander` 。
- `ContentTemplate`型の [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 。これは、のコンテンツを動的に拡張するために使用されるテンプレートです `Expander` 。
- `ExpandAnimationEasing`[`Easing`](xref:Xamarin.Forms.Easing)拡張中にコンテンツに適用されるイージング関数を表す、型の。 `Expander`
- `ExpandAnimationLength``uint`が展開されたときのアニメーションの継続時間を定義する型の `Expander` 。 このプロパティの既定値は250ミリ秒です。
- `ForceUpdateSizeCommand`型の `ICommand` 。のサイズが強制的に更新されるときに実行されるコマンドを定義し `Expander` ます。 このプロパティは、 `OneWayToSource` バインドモードを使用します。
- `Header`[`View`](xref:Xamarin.Forms.View)ヘッダーの内容を定義する型の。
- `IsExpanded`が `bool` 展開されているかどうかを判断する、型の `Expander` 。 このプロパティは、 `TwoWay` バインディングモードを使用し、既定値は `false` です。
- `Spacing`型の `double` 。ヘッダーとその内容との間のスペースを表します。 このプロパティの既定値は 0 です。
- `State`の状態を表す、型の `ExpanderState` `Expander` 。 このプロパティは、 `OneWayToSource` バインドモードを使用します。

これらのプロパティは、オブジェクトによって支えられています [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 。これは、データバインディングのターゲットとスタイルを設定できることを意味します。

> [!NOTE]
> `Content`プロパティはクラスの content プロパティである `Expander` ため、XAML から明示的に設定する必要はありません。

`ExpanderState` 列挙体を使って、次のメンバーを定義できます。

- `Expanding`が展開されていることを示し `Expander` ます。
- `Expanded`が展開されていることを示し `Expander` ます。
- `Collapsing`が折りたたまれていることを示し `Expander` ます。
- `Collapsed`が折りたたまれていることを示し `Expander` ます。

また、この `Expander` コントロールは `Tapped` 、ヘッダーがタップされたときに発生するイベントも定義し `Expander` ます。 また、には、 `Expander` `ForceUpdateSize` 実行時にプログラムによってのサイズを変更するために呼び出すことができるメソッドが用意されてい `Expander` ます。

## <a name="create-an-expander"></a>エキスパンダーを作成する

次の例は、XAML でをインスタンス化する方法を示してい `Expander` ます。

```xaml
<Expander>
    <Expander.Header>
        <Label Text="Baboon"
               FontAttributes="Bold"
               FontSize="Medium" />
    </Expander.Header>
    <Grid Padding="10">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="Auto" />
        </Grid.ColumnDefinitions>
        <Image Source="http://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg"
               Aspect="AspectFill"
               HeightRequest="120"
               WidthRequest="120" />
        <Label Grid.Column="1"
               Text="Baboons are African and Arabian Old World monkeys belonging to the genus Papio, part of the subfamily Cercopithecinae."
               FontAttributes="Italic" />
    </Grid>
</Expander>
```

この例では、 `Expander` が既定で折りたたまれ、 [`Label`](xref:Xamarin.Forms.Label) ヘッダーとしてが表示されます。 ヘッダーをタップすると、が `Expander` 展開され、コンテンツ (子コントロールを含む) が表示され [`Grid`](xref:Xamarin.Forms.Grid) ます。 `Expander`が展開されている場合、そのヘッダーをタップすると、が折りたたまれ `Expander` ます。

> [!IMPORTANT]
> `Expander.Content`プロパティを暗黙的または明示的に設定すると、 `Expander` が折りたたまれている場合でも、コンテンツを含むページに移動するときにコンテンツが作成され `Expander` ます。 ただし、 `Expander.ContentTemplate` プロパティは、が初めて展開されるときにのみ増加するコンテンツに設定でき `Expander` ます。 詳細については、「[必要に応じて展開コンテンツを作成する](#create-expander-content-on-demand)」を参照してください。

また、 `Expander` コードでを作成することもできます。

```csharp
Expander expander = new Expander
{
    Header = new Label
    {
        Text = "Baboon",
        FontAttributes = FontAttributes.Bold,
        FontSize = Device.GetNamedSize(NamedSize.Medium, typeof(Label))
    }
};

Grid grid = new Grid
{
    Padding = new Thickness(10),
    ColumnDefinitions =
    {
        new ColumnDefinition { Width = GridLength.Auto },
        new ColumnDefinition { Width = GridLength.Auto }
    }
};

grid.Children.Add(new Image
{
    Source = "http://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg",
    Aspect = Aspect.AspectFill,
    HeightRequest = 120,
    WidthRequest = 120
});

grid.Children.Add(new Label
{
    Text = "Baboons are African and Arabian Old World monkeys belonging to the genus Papio, part of the subfamily Cercopithecinae.",
    FontAttributes = FontAttributes.Italic
}, 1, 0);

expander.Content = grid;
```

## <a name="create-expander-content-on-demand"></a>必要に応じて展開コンテンツを作成する

`Expander`コンテンツは、拡張に応じて、オンデマンドで作成でき `Expander` ます。 これを実現するには、プロパティをコンテンツを含むに設定し `Expander.ContentTemplate` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) ます。

```xaml
<Expander>
    <Expander.Header>
        <Label Text="{Binding Name}"
               FontAttributes="Bold"
               FontSize="Medium" />
    </Expander.Header>
    <Expander.ContentTemplate>
        <DataTemplate>
            <Grid Padding="10">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="Auto" />
                </Grid.ColumnDefinitions>
                <Image Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       HeightRequest="120"
                       WidthRequest="120" />
                <Label Grid.Column="1"
                       Text="{Binding Details}"
                       FontAttributes="Italic" />
            </Grid>
        </DataTemplate>
    </Expander.ContentTemplate>
</Expander>
```

この例では、が `Expander` 初めてを展開するときにのみコンテンツが増加してい `Expander` ます。

この方法の利点は、ページに複数のオブジェクトが含まれている場合 `Expander` 、のコンテンツ `Expander` が、ユーザーによって初めて展開されたときにのみ作成されることです。

## <a name="add-an-expansion-indicator"></a>拡張インジケーターの追加

を [`Image`](xref:Xamarin.Forms.Image) ヘッダーに追加すること `Expander` により、展開状態を視覚的に示すことができます。 は、 [`DataTrigger`](xref:Xamarin.Forms.DataTrigger) `Image` `Source` プロパティの値に基づいてプロパティを変更するにアタッチでき `Expander.IsExpanded` ます。

```xaml
<Expander>
    <Expander.Header>
        <Grid>
            <Label Text="{Binding Name}"
                   FontAttributes="Bold"
                   FontSize="Medium" />
            <Image Source="expand.png"
                   HorizontalOptions="End"
                   VerticalOptions="Start">
                <Image.Triggers>
                    <DataTrigger TargetType="Image"
                                 Binding="{Binding Source={RelativeSource AncestorType={x:Type Expander}}, Path=IsExpanded}"
                                 Value="True">
                        <Setter Property="Source"
                                Value="collapse.png" />
                    </DataTrigger>
                </Image.Triggers>
            </Image>
        </Grid>
    </Expander.Header>
    <Expander.ContentTemplate>
        <DataTemplate>
            <Grid Padding="10">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="Auto" />
                </Grid.ColumnDefinitions>
                <Image Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       HeightRequest="120"
                       WidthRequest="120" />
                <Label Grid.Column="1"
                       Text="{Binding Details}"
                       FontAttributes="Italic" />
            </Grid>
        </DataTemplate>
    </Expander.ContentTemplate>
</Expander>
```

この例では、 [`Image`](xref:Xamarin.Forms.Image) 既定でアイコンが表示され `expand` ます。

![IOS と Android における折りたたまれた状態の展開アイコンのスクリーンショット](expander-images/icon-expand.png "IOS と Android の expandd アイコン")

`IsExpanded`プロパティは、 `true` ヘッダーがタップされると、 `Expander` `collapse` アイコンが表示されるようになります。

![IOS と Android の展開状態にある展開アイコンのスクリーンショット](expander-images/icon-collapse.png "IOS と Android の expandd アイコン")

トリガーの詳細については、「 [ Xamarin.Forms トリガー](~/xamarin-forms/app-fundamentals/triggers.md)」を参照してください。

## <a name="define-the-space-between-header-and-content"></a>ヘッダーとコンテンツの間のスペースを定義する

既定では、のコンテンツは `Expander` ヘッダーの直下に表示されます。 ただし、この動作は、 `Spacing` `double` コンテンツとそのヘッダーの間の空白を表す値にプロパティを設定することによって変更できます。

```xaml
<Expander Spacing="50"
          IsExpanded="true">
    <Expander.Header>
        <Label Text="Baboon"
               FontAttributes="Bold"
               FontSize="Medium" />
    </Expander.Header>
    <Grid Padding="10">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="Auto" />
        </Grid.ColumnDefinitions>
        <Image Source="http://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg"
               Aspect="AspectFill"
               HeightRequest="120"
               WidthRequest="120" />
        <Label Grid.Column="1"
               Text="Baboons are African and Arabian Old World monkeys belonging to the genus Papio, part of the subfamily Cercopithecinae."
               FontAttributes="Italic" />
    </Grid>
</Expander>
```

この例では、 `Expander` コンテンツは、ヘッダーの下に50デバイスに依存しない単位で表示されます。

![IOS と Android でのスペーシングが設定された展開コントロールのスクリーンショット](expander-images/expander-spacing.png "IOS と Android でスペースが設定された展開")

## <a name="embed-an-expander-in-an-expander"></a>エキスパンダーを展開コントロールに埋め込む

のコンテンツを `Expander` 別のコントロールに設定して `Expander` 、複数のレベルの拡張を有効にすることができます。 次の XAML は、 `Expander` コンテンツが別のオブジェクトであるを示してい `Expander` ます。

```xaml
<Expander Spacing="10">
    <Expander.Header>
        <Label Text="{Binding Name}"
               FontAttributes="Bold"
               FontSize="Medium" />
    </Expander.Header>
    <Expander Padding="10"
              Spacing="10">
        <Expander.Header>
            <Label Text="{Binding Location}"
                   FontSize="Medium" />
        </Expander.Header>
            <Expander.ContentTemplate>
                <DataTemplate>
                    <Grid>
                        <Grid.ColumnDefinitions>
                            <ColumnDefinition Width="Auto" />
                            <ColumnDefinition Width="Auto" />
                        </Grid.ColumnDefinitions>
                        <Image Source="{Binding ImageUrl}"
                               Aspect="AspectFill"
                               HeightRequest="120"
                               WidthRequest="120" />
                        <Label Grid.Column="1"
                               Text="{Binding Details}"
                               FontAttributes="Italic" />
                    </Grid>
                </DataTemplate>
            </Expander.ContentTemplate>
    </Expander>
</Expander>
```

この例では、ルートヘッダーをタップすると、 `Expander` 子のヘッダーが示され `Expander` ます。

![IOS および Android での埋め込み展開コントロールのスクリーンショット](expander-images/embedded-expander1.png "IOS と Android の埋め込み展開コントロール")

子ヘッダーをタップする `Expander` と、そのコンテンツが拡大して表示されます。

![IOS および Android での埋め込み展開コントロールのスクリーンショット](expander-images/embedded-expander2.png "IOS と Android の埋め込み展開コントロール")

## <a name="define-the-expand-and-collapse-animation"></a>展開と折りたたみのアニメーションを定義する

拡大または縮小するときに発生するアニメーションは、 `Expander` `ExpandAnimationEasing` プロパティとプロパティを `CollapseAnimationEasing` 、に含まれるイージング関数のいずれかに設定する Xamarin.Forms か、カスタムイージング関数に設定することによって定義できます。 既定では、展開と折りたたみのアニメーションは250ミリ秒に対して行われます。 ただし、これらの期間は、 `ExpandAnimationLength` プロパティとプロパティを値に設定することによって変更でき `CollapseAnimationLength` `uint` ます。

次の XAML は、が `Expander` ユーザーによって展開または折りたたまれたときに発生するアニメーションを定義する例を示しています。

```xaml
<Expander ExpandAnimationEasing="{x:Static Easing.CubicIn}"
          ExpandAnimationLength="500"
          CollapseAnimationEasing="{x:Static Easing.CubicOut}"
          CollapseAnimationLength="500">
    <Expander.Header>
        <Label Text="{Binding Name}"
               FontAttributes="Bold"
               FontSize="Medium" />
    </Expander.Header>
    <Expander.ContentTemplate>
        <DataTemplate>
            <Grid Padding="10">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="Auto" />
                </Grid.ColumnDefinitions>
                <Image Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       HeightRequest="120"
                       WidthRequest="120" />
                <Label Grid.Column="1"
                       Text="{Binding Details}"
                       FontAttributes="Italic" />
            </Grid>
        </DataTemplate>
    </Expander.ContentTemplate>
</Expander>
```

この例では、 `CubicIn` イージング関数を使用すると、500ミリ秒上の展開アニメーションがゆっくり加速し、 `CubicOut` イージング関数は500ミリ秒に対する折りたたみアニメーションを迅速に減速します。

イージング関数の詳細については、「 [ Xamarin.Forms イージング関数](~/xamarin-forms/user-interface/animation/easing.md)」を参照してください。

## <a name="resize-an-expander-at-runtime"></a>実行時に展開コントロールのサイズを変更する

は、 `Expander` 実行時にメソッドを使用してプログラムでサイズを変更でき `ForceUpdateSize` ます。

`Expander`という名前のを指定した `expander` 場合、そのコンテンツにはが関連付けられたが含まれ [`Label`](xref:Xamarin.Forms.Label) `TapGestureRecognizer` ます。次のコード例では、メソッドの呼び出しを示してい `ForceUpdateSize` ます。

```csharp
void OnLabelTapped(object sender, EventArgs e)
{
    Label label = sender as Label;
    Expander expander = label.Parent.Parent.Parent as Expander;

    if (label.FontSize == Device.GetNamedSize(NamedSize.Default, label))
    {
        label.FontSize = Device.GetNamedSize(NamedSize.Large, label);
    }
    else
    {
        label.FontSize = Device.GetNamedSize(NamedSize.Default, label);
    }
    expander.ForceUpdateSize();
}
```

この例では、 `FontSize` [`Label`](xref:Xamarin.Forms.Label) がタップされると、のが変更 `Label` されます。 フォントのサイズが変化するため、メソッドを呼び出すことによってのサイズを更新する必要が `Expander` `ForceUpdateSize` あります。

## <a name="disable-an-expander"></a>エキスパンダーを無効にする

アプリケーションは、の展開 `Expander` が有効な操作ではない状態になる場合があります。 このような場合は、 `Expander` `IsEnabled` プロパティを false に設定することで、を無効にすることができます。 これにより、ユーザーがを展開または折りたたむことができなくなり `Expander` ます。

## <a name="related-links"></a>関連リンク

- [エキスパンダーのデモ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-expanderdemos/)
- [Xamarin.Formsイージング関数](~/xamarin-forms/user-interface/animation/easing.md)
- [Xamarin.Forms のトリガー](~/xamarin-forms/app-fundamentals/triggers.md)
- [Xamarin.Formsバインド可能なレイアウト](~/xamarin-forms/user-interface/layouts/bindable-layouts.md)
