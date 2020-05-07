---
title: Xamarin. フォームの展開
description: Xamarin. Forms エキスパンダーコントロールは、任意のコンテンツをホストするための拡張可能なコンテナーを提供します。 コンテンツを表示または非表示にするには、エキスパンダーヘッダーをタップします。
ms.prod: xamarin
ms.assetid: 381DCB55-522D-4414-B45B-E8DD70AA9985
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/15/2020
ms.openlocfilehash: b1e573a6070a637ef2fdfa65bb0fc1375522fc3c
ms.sourcegitcommit: 443ecd9146fe2a7bbb9b5ab6d33c835876efcf1f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/06/2020
ms.locfileid: "82852494"
---
# <a name="xamarinforms-expander"></a>Xamarin. フォームの展開

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-expanderdemos/)

Xamarin. Forms `Expander`コントロールには、任意のコンテンツをホストするための拡張可能なコンテナーが用意されています。 コントロールはヘッダーとコンテンツを持ち、 `Expander`ヘッダーをタップすることによってコンテンツを表示または非表示にします。 `Expander`ヘッダーのみが表示されている`Expander`場合は、が*折りたたま*れています。 `Expander`コンテンツが表示されると`Expander` 、が*展開*されます。

次のスクリーンショットは`Expander` 、折りたたまれて展開された状態のを示しています。赤いボックスは、ヘッダーとコンテンツを示しています。

![展開された状態の展開コントロールのスクリーンショット (iOS と Android)](expander-images/expander.png "IOS と Android の展開")

> [!IMPORTANT]
> `Expander`は現在試験段階であり、 `Expander_Experimental`フラグを設定することによってのみ使用できます。 詳細については、「試験的な[フラグ](~/xamarin-forms/internals/experimental-flags.md)」を参照してください。
>
> また、 `Expander`コントロールは`Xamarin.Forms`名前空間に完全に実装されます。 そのため、Xamarin. Forms でサポートされているすべてのプラットフォームで使用できます。

コントロール`Expander`は、次のプロパティを定義します。

- `CollapseAnimationEasing`型[`Easing`](xref:Xamarin.Forms.Easing)の。これは、コンテンツの`Expander`折りたたみ時に適用されるイージング関数を表します。
- `CollapseAnimationLength`が折りたたまれ`uint`ているときのアニメーションの継続時間を定義する型の。 `Expander` このプロパティの既定値は250ミリ秒です。
- `Command`型`ICommand`の。これは、 `Expander`ヘッダーがタップされたときに実行されます。
- `CommandParameter`: `object` 型、`Command`に渡されるパラメーターです。
- `Content`が`Expander`展開さ[`View`](xref:Xamarin.Forms.View)れたときに表示されるコンテンツを定義する、型の。
- `ContentTemplate`型[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)の`Expander`。これは、のコンテンツを動的に拡張するために使用されるテンプレートです。
- `ExpandAnimationEasing`拡張中に[`Easing`](xref:Xamarin.Forms.Easing) `Expander`コンテンツに適用されるイージング関数を表す、型の。
- `ExpandAnimationLength`が`Expander`展開さ`uint`れたときのアニメーションの継続時間を定義する型の。 このプロパティの既定値は250ミリ秒です。
- `ForceUpdateSizeCommand`型`ICommand`の。のサイズが強制的に更新`Expander`されるときに実行されるコマンドを定義します。 このプロパティは、 `OneWayToSource`バインドモードを使用します。
- `Header`ヘッダーの内容[`View`](xref:Xamarin.Forms.View)を定義する型の。
- `IsExpanded`が展開さ`bool`れて`Expander`いるかどうかを判断する、型の。 このプロパティは、 `TwoWay`バインディングモードを使用し、既定値は`false`です。
- `Spacing`型`double`の。ヘッダーとその内容との間のスペースを表します。 このプロパティの既定値は 0 です。
- `State`の状態を`ExpanderState`表す、型の`Expander`。 このプロパティは、 `OneWayToSource`バインドモードを使用します。

これらのプロパティは、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)オブジェクトによって支えられています。これは、データバインディングのターゲットとスタイルを設定できることを意味します。

> [!NOTE]
> `Content`プロパティは`Expander`クラスの content プロパティであるため、XAML から明示的に設定する必要はありません。

`ExpanderState` 列挙体を使って、次のメンバーを定義できます。

- `Expanding`が展開`Expander`されていることを示します。
- `Expanded`が展開`Expander`されていることを示します。
- `Collapsing`が折りたたま`Expander`れていることを示します。
- `Collapsed`が折りたたまれ`Expander`ていることを示します。

また`Expander` 、このコントロールは`Tapped` 、 `Expander`ヘッダーがタップされたときに発生するイベントも定義します。 また、 `Expander`には、 `ForceUpdateSize`実行時にプログラムによっての`Expander`サイズを変更するために呼び出すことができるメソッドが用意されています。

## <a name="create-an-expander"></a>エキスパンダーを作成する

次の例は、 `Expander` XAML でをインスタンス化する方法を示しています。

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

この例では、 `Expander`が既定で折りたたまれ、ヘッダー [`Label`](xref:Xamarin.Forms.Label)としてが表示されます。 ヘッダーをタップすると、が`Expander`展開され、コンテンツ (子コントロールを[`Grid`](xref:Xamarin.Forms.Grid)含む) が表示されます。 `Expander`が展開されている場合、そのヘッダー `Expander`をタップすると、が折りたたまれます。

> [!IMPORTANT]
> `Expander.Content`プロパティを暗黙的または明示的に設定すると`Expander` 、 `Expander`が折りたたまれている場合でも、コンテンツを含むページに移動するときにコンテンツが作成されます。 ただし、プロパティ`Expander.ContentTemplate`は、が`Expander`初めて展開されるときにのみ増加するコンテンツに設定できます。 詳細については、「[必要に応じて展開コンテンツを作成する](#create-expander-content-on-demand)」を参照してください。

また、コード`Expander`でを作成することもできます。

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

`Expander`コンテンツは、 `Expander`拡張に応じて、オンデマンドで作成できます。 これを実現するには、 `Expander.ContentTemplate`プロパティ[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)をコンテンツを含むに設定します。

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

この例では、 `Expander`が初めてを`Expander`展開するときにのみコンテンツが増加しています。

この方法の利点は、ページに複数`Expander`のオブジェクトが含まれている場合、 `Expander`のコンテンツが、ユーザーによって初めて展開されたときにのみ作成されることです。

## <a name="add-an-expansion-indicator"></a>拡張インジケーターの追加

を[`Image`](xref:Xamarin.Forms.Image) `Expander`ヘッダーに追加することにより、展開状態を視覚的に示すことができます。 は[`DataTrigger`](xref:Xamarin.Forms.DataTrigger) 、 `Expander.IsExpanded`プロパティの値に`Image`基づいて`Source`プロパティを変更するにアタッチできます。

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

この例では[`Image`](xref:Xamarin.Forms.Image) 、既定で`expand`アイコンが表示されます。

![IOS と Android における折りたたまれた状態の展開アイコンのスクリーンショット](expander-images/icon-expand.png "IOS と Android の expandd アイコン")

プロパティ`IsExpanded`は、 `true` `Expander`ヘッダーがタップされると、アイコンが`collapse`表示されるようになります。

![IOS と Android の展開状態にある展開アイコンのスクリーンショット](expander-images/icon-collapse.png "IOS と Android の expandd アイコン")

トリガーの詳細については、「 [Xamarin. Forms triggers](~/xamarin-forms/app-fundamentals/triggers.md)」を参照してください。

## <a name="define-the-space-between-header-and-content"></a>ヘッダーとコンテンツの間のスペースを定義する

既定では`Expander` 、のコンテンツはヘッダーの直下に表示されます。 ただし、この動作は、コンテンツとそのヘッダー `Spacing`の間の`double`空白を表す値にプロパティを設定することによって変更できます。

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

この例では、 `Expander`コンテンツは、ヘッダーの下に50デバイスに依存しない単位で表示されます。

![IOS と Android でのスペーシングが設定された展開コントロールのスクリーンショット](expander-images/expander-spacing.png "IOS と Android でスペースが設定された展開")

## <a name="embed-an-expander-in-an-expander"></a>エキスパンダーを展開コントロールに埋め込む

のコンテンツを別`Expander` `Expander`のコントロールに設定して、複数のレベルの拡張を有効にすることができます。 次の XAML は、 `Expander`コンテンツが別`Expander`のオブジェクトであるを示しています。

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

この例では、ルート`Expander`ヘッダーをタップすると、子`Expander`のヘッダーが示されます。

![IOS および Android での埋め込み展開コントロールのスクリーンショット](expander-images/embedded-expander1.png "IOS と Android の埋め込み展開コントロール")

子`Expander`ヘッダーをタップすると、そのコンテンツが拡大して表示されます。

![IOS および Android での埋め込み展開コントロールのスクリーンショット](expander-images/embedded-expander2.png "IOS と Android の埋め込み展開コントロール")

## <a name="define-the-expand-and-collapse-animation"></a>展開と折りたたみのアニメーションを定義する

展開または折りたたみを定義するときに発生するアニメーションを定義する`ExpandAnimationEasing`に`CollapseAnimationEasing`は、プロパティとプロパティを、Xamarin. Forms またはカスタムイージング関数に含まれるイージング関数のいずれかに設定します。 `Expander` 既定では、展開と折りたたみのアニメーションは250ミリ秒に対して行われます。 ただし、これらの期間は、プロパティ`ExpandAnimationLength`と`CollapseAnimationLength`プロパティを値に`uint`設定することによって変更できます。

次の XAML は、 `Expander`がユーザーによって展開または折りたたまれたときに発生するアニメーションを定義する例を示しています。

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

この例では、 `CubicIn`イージング関数を使用すると、500ミリ秒上の展開アニメーション`CubicOut`がゆっくり加速し、イージング関数は500ミリ秒に対する折りたたみアニメーションを迅速に減速します。

イージング関数の詳細については、「 [Xamarin のイージング関数](~/xamarin-forms/user-interface/animation/easing.md)」を参照してください。

## <a name="resize-an-expander-at-runtime"></a>実行時に展開コントロールのサイズを変更する

は`Expander` 、実行時に`ForceUpdateSize`メソッドを使用してプログラムでサイズを変更できます。

`Expander`という名前`expander`のを指定した[`Label`](xref:Xamarin.Forms.Label)場合、その`TapGestureRecognizer`コンテンツにはが関連付けられたが含まれ`ForceUpdateSize`ます。次のコード例では、メソッドの呼び出しを示しています。

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

この例では、 `FontSize` `Label`がタップ[`Label`](xref:Xamarin.Forms.Label)されると、のが変更されます。 フォントのサイズが変化するため、 `Expander` `ForceUpdateSize`メソッドを呼び出すことによってのサイズを更新する必要があります。

## <a name="disable-an-expander"></a>エキスパンダーを無効にする

アプリケーションは、 `Expander`の展開が有効な操作ではない状態になる場合があります。 このような場合は`Expander` 、 `IsEnabled`プロパティを false に設定することで、を無効にすることができます。 これにより、ユーザーがを`Expander`展開または折りたたむことができなくなります。

## <a name="related-links"></a>関連リンク

- [エキスパンダーのデモ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-expanderdemos/)
- [Xamarin. フォームのイージング関数](~/xamarin-forms/user-interface/animation/easing.md)
- [Xamarin.Forms のトリガー](~/xamarin-forms/app-fundamentals/triggers.md)
- [Xamarin. フォームバインド可能なレイアウト](~/xamarin-forms/user-interface/layouts/bindable-layouts.md)
