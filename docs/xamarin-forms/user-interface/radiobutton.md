---
title: Xamarin. Forms RadioButton
description: Xamarin. Forms RadioButton は、ユーザーがセットから1つのオプションを選択できるようにするためのボタンの一種です。 各オプションは1つのラジオボタンで表され、1つのグループ内で選択できるラジオボタンは1つだけです。
ms.prod: xamarin
ms.assetid: E2AA40E0-69A5-41DF-BFC4-C151CA657451
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/13/2020
ms.openlocfilehash: 128ab4f6f00adaf86afc08eba37bcb81d3a04a90
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/29/2020
ms.locfileid: "82533001"
---
# <a name="xamarinforms-radiobutton"></a>Xamarin. Forms RadioButton

[![](~/media/shared/download.png)サンプルをダウンロードするサンプルをダウンロードする](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-radiobuttondemos/)

Xamarin. フォーム`RadioButton`は、ユーザーがセットから1つのオプションを選択できるようにするためのボタンの一種です。 各オプションは1つのラジオボタンで表され、1つのグループ内で選択できるラジオボタンは1つだけです。 クラス`RadioButton`は、 [`Button`](xref:Xamarin.Forms.Button)クラスから継承されます。

次のスクリーンショット`RadioButton`は、IOS と Android で、クリアされた状態と選択された状態のオブジェクトを示しています。

![IOS と Android 上の選択された状態およびクリア状態の Radiobutton のスクリーンショット](radiobutton-images/radiobutton-states.png "IOS および Android の Radiobutton")

> [!IMPORTANT]
> `RadioButton`は現在試験段階であり、 `RadioButton_Experimental`フラグを設定することによってのみ使用できます。 詳細については、「試験的な[フラグ](~/xamarin-forms/internals/experimental-flags.md)」を参照してください。

コントロール`RadioButton`は、次のプロパティを定義します。

- `IsChecked`が選択さ`bool`れて`RadioButton`いるかどうかを定義する型の。 このプロパティは、 `TwoWay`バインディングを使用し、既定値は`false`です。
- `GroupName`型`string`の。これは、相互に排他的な`RadioButton`コントロールを指定する名前を定義します。 このプロパティの`null`既定値はです。

これらのプロパティは、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)オブジェクトによって支えられています。これは、データバインディングのターゲットとスタイルを設定できることを意味します。

コントロール`RadioButton`は、ユーザー `CheckedChanged`またはプログラムによる操作`IsChecked`によってプロパティが変更されたときに発生するイベントを定義します。 イベント`CheckedChangedEventArgs`に`CheckedChanged`付随するオブジェクトには、型`Value` `bool`のという名前のプロパティが1つあります。 イベントが発生すると、プロパティの値`Value`は`IsChecked`プロパティの新しい値に設定されます。

また、クラスは`RadioButton` 、 [`Button`](xref:Xamarin.Forms.Button)クラスから次の一般的に使用されるプロパティを継承します。

- [`Command`](xref:Xamarin.Forms.Button.Command)が選択さ`ICommand`れたときに実行される、型の。 `RadioButton`
- [`CommandParameter`](xref:Xamarin.Forms.Button.CommandParameter)型`object`の。これは、 `Command`に渡されるパラメーターです。
- [`FontAttributes`](xref:Xamarin.Forms.Button.FontAttributes)テキストスタイルを[`FontAttributes`](xref:Xamarin.Forms.FontAttributes)決定する型の。
- [`FontFamily`](xref:Xamarin.Forms.Button.FontFamily)フォントファミリを`string`定義する型の。
- [`FontSize`](xref:Xamarin.Forms.Button.FontSize)フォントサイズを`double`定義する型の。
- [`Text`](xref:Xamarin.Forms.Button.Text)表示される`string`テキストを定義する型の。
- [`TextColor`](xref:Xamarin.Forms.Button.TextColor)表示される[`Color`](xref:Xamarin.Forms.Color)テキストの色を定義する型の。

コントロールの[`Button`](xref:Xamarin.Forms.Button)詳細については、「 [Xamarin. フォームボタン](~/xamarin-forms/user-interface/button.md)」を参照してください。

## <a name="create-radiobuttons"></a>Radiobutton の作成

次の例は、XAML で`RadioButton`オブジェクトをインスタンス化する方法を示しています。

```xaml
<StackLayout>
    <Label Text="What's your favorite animal?" />
    <RadioButton Text="Cat" />
    <RadioButton Text="Dog" />
    <RadioButton Text="Elephant" />
    <RadioButton Text="Monkey"
                 IsChecked="true" />
</StackLayout>
```

この例では`RadioButton` 、オブジェクトは同じ親コンテナー内に暗黙的にグループ化されています。 この XAML は、次のスクリーンショットに示されているような外観になります。

![IOS と Android での暗黙的にグループ化された Radiobutton のスクリーンショット](radiobutton-images/radiobuttons.png "IOS と Android で暗黙的にグループ化された Radiobutton")

また、 `RadioButton`コードでオブジェクトを作成することもできます。

```csharp
StackLayout stackLayout = new StackLayout
{
    Children =
    {
        new Label { Text = "What's your favorite animal?" },
        new RadioButton { Text = "Cat" },
        new RadioButton { Text = "Dog" },
        new RadioButton { Text = "Elephant" },
        new RadioButton { Text = "Monkey", IsChecked = true }
    }
};
```

## <a name="group-radiobuttons"></a>グループの Radiobutton

オプションボタンはグループで機能し、オプションボタンをグループ化する方法は2つあります。

- 同じ親コンテナー内に配置します。 これは暗黙的なグループ化と呼ばれます。
- 各オプションボタン`GroupName`のプロパティを同じ値に設定します。 これを明示的なグループ化と呼びます。

次の XAML の例では`RadioButton` 、 `GroupName`プロパティを設定することによってオブジェクトを明示的にグループ化しています。

```xaml
<Label Text="What's your favorite color?" />
<RadioButton Text="Red"
             TextColor="Red"
             GroupName="colors" />
<RadioButton Text="Green"
             TextColor="Green"
             GroupName="colors" />
<RadioButton Text="Blue"
             TextColor="Blue"
             GroupName="colors" />
<RadioButton Text="Other"
             GroupName="colors" />
```

この例では、 `RadioButton`それぞれが同じ`GroupName`値を共有するため、相互に排他的です。 この XAML は、次のスクリーンショットに示されているような外観になります。

![IOS と Android での明示的にグループ化された Radiobutton のスクリーンショット](radiobutton-images/grouped-radiobuttons.png "IOS と Android での明示的にグループ化された Radiobutton")

## <a name="respond-to-a-radiobutton-state-change"></a>RadioButton 状態の変更に応答する

ラジオ ボタンには選択またはクリアの 2 つの状態があります。 オプションボタンが選択されている`IsChecked`場合、 `true`そのプロパティはになります。 オプションボタンがオフの場合、 `IsChecked`プロパティは`false`になります。 同じグループ内の別のラジオ ボタンをクリックするとラジオ ボタンをクリアにできますが、ボタンをもう一度クリックしてもクリアにすることはできません。 ただし、 `IsChecked`プロパティをに設定する`false`と、オプションボタンをプログラムでクリアできます。

ユーザーまた`IsChecked`はプログラムによる操作によってプロパティが変更`CheckedChanged`されると、イベントが発生します。 このイベントのイベントハンドラーは、変更に応答するように登録できます。

```xaml
<RadioButton Text="Red"
             TextColor="Red"
             GroupName="colors"
             CheckedChanged="OnColorsRadioButtonCheckedChanged" />
```

分離コードには、 `CheckedChanged`イベントのハンドラーが含まれています。

```csharp
void OnColorsRadioButtonCheckedChanged(object sender, CheckedChangedEventArgs e)
{
    // Perform required operation
}
```

`sender`引数は、この`RadioButton`イベントを担当するです。 これを使用すると、 `RadioButton`オブジェクトにアクセスしたり、同じ`RadioButton` `CheckedChanged`イベントハンドラーを共有する複数のオブジェクトを区別したりできます。

または、 `CheckedChanged`イベントのイベントハンドラーをコードに登録できます。

```csharp
RadioButton radioButton = new RadioButton { ... };
radioButton.CheckedChanged += (sender, e) =>
{
    // Perform required operation
};
```

> [!NOTE]
> `RadioButton`状態の変化に対応するための別の方法とし`ICommand`て、を定義し`RadioButton.Command` 、それをプロパティに割り当てることができます。 詳細については、「[ボタン: コマンドインターフェイスの使用](~/xamarin-forms/user-interface/button.md#using-the-command-interface)」を参照してください。

## <a name="radiobutton-visual-states"></a>RadioButton の表示状態

`RadioButton``IsChecked` [`VisualState`](xref:Xamarin.Forms.VisualState)が選択されている場合`RadioButton`に、ビジュアルの変更を開始するために使用できるがあります。

次の XAML の例は、 `IsChecked`状態の表示状態を定義する方法を示しています。

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <Style TargetType="RadioButton">
            <Setter Property="VisualStateManager.VisualStateGroups">
                <VisualStateGroupList>
                    <VisualStateGroup x:Name="CommonStates">
                        <VisualState x:Name="Normal">
                            <VisualState.Setters>
                                <Setter Property="TextColor"
                                        Value="Red" />
                                <Setter Property="Opacity"
                                        Value="0.5" />
                            </VisualState.Setters>
                        </VisualState>
                        <VisualState x:Name="IsChecked">
                            <VisualState.Setters>
                                <Setter Property="TextColor"
                                        Value="Green" />
                                <Setter Property="Opacity"
                                        Value="1" />
                            </VisualState.Setters>
                        </VisualState>
                    </VisualStateGroup>
                </VisualStateGroupList>
            </Setter>
        </Style>
    </ContentPage.Resources>
    <StackLayout>
        <Label Text="What's your favorite mode of transport?" />
        <RadioButton Text="Car"
                     CheckedChanged="OnRadioButtonCheckedChanged" />
        <RadioButton Text="Bike"
                     CheckedChanged="OnRadioButtonCheckedChanged" />
        <RadioButton Text="Train"
                     CheckedChanged="OnRadioButtonCheckedChanged" />
        <RadioButton Text="Walking"
                     CheckedChanged="OnRadioButtonCheckedChanged" />
    </StackLayout>
</ContentPage>
```

この例では、暗黙的[`Style`](xref:Xamarin.Forms.Style)な`RadioButton`ターゲットオブジェクトです。 `IsChecked` `Opacity`が選択されている場合、その`TextColor`プロパティは、値1を使用して緑色に設定されます。 [`VisualState`](xref:Xamarin.Forms.VisualState) `RadioButton` `Normal` `VisualState`は、 `RadioButton`がクリア状態のときに、 `TextColor` `Opacity`値0.5 を使用してそのプロパティを赤に設定することを指定します。 したがって、全体の効果として`RadioButton` 、がクリアされている場合は赤、部分的に透明になり、透明度のない緑は選択されると、次のようになります。

![IOS と Android の表示状態によって設定された RadioButton の外観のスクリーンショット](radiobutton-images/ischecked-visualstate.png "IOS および Android での RadioButton の表示状態")

表示状態の詳細については、「 [Xamarin. Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)」を参照してください。

## <a name="disable-a-radiobutton"></a>RadioButton を無効にする

アプリケーションが、チェック`RadioButton`されるが有効な操作ではない状態になることがあります。 このような場合は`RadioButton` 、 `IsEnabled`プロパティをに設定する`false`ことで、を無効にすることができます。

## <a name="related-links"></a>関連リンク

- [RadioButton のデモ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-radiobuttondemos/)
- [[Xamarin. フォーム] ボタン](~/xamarin-forms/user-interface/button.md)
- [Xamarin Forms State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)
