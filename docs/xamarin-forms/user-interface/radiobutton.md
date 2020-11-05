---
title: Xamarin.Forms RadioButton
description: Xamarin.FormsRadioButton は、ユーザーがセットから1つのオプションを選択できるようにするためのボタンの一種です。 各オプションは1つのラジオボタンで表され、1つのグループ内で選択できるラジオボタンは1つだけです。
ms.prod: xamarin
ms.assetid: E2AA40E0-69A5-41DF-BFC4-C151CA657451
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/13/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 20ccf71771493afe56d4d899186c7e6330ee96dd
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93366582"
---
# <a name="no-locxamarinforms-radiobutton"></a>Xamarin.Forms RadioButton

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/userinterface-radiobuttondemos/)

Xamarin.Forms `RadioButton` は、ユーザーがセットから1つのオプションを選択できるようにするボタンの種類です。 各オプションは1つのラジオボタンで表され、1つのグループ内で選択できるラジオボタンは1つだけです。 `RadioButton`クラスは、クラスから継承され [`Button`](xref:Xamarin.Forms.Button) ます。

次のスクリーンショットは、 `RadioButton` iOS と Android で、クリアされた状態と選択された状態のオブジェクトを示しています。

![IOS と Android 上の選択された状態およびクリア状態の Radiobutton のスクリーンショット](radiobutton-images/radiobutton-states.png "IOS および Android の Radiobutton")

> [!IMPORTANT]
> `RadioButton` は現在試験段階であり、フラグを設定することによってのみ使用でき `RadioButton_Experimental` ます。 詳しくは、[試験的なフラグ](~/xamarin-forms/internals/experimental-flags.md)に関する記事を参照してください。

コントロールは、 `RadioButton` 次のプロパティを定義します。

- `IsChecked``bool`が選択されているかどうかを定義する型の `RadioButton` 。 このプロパティは、 `TwoWay` バインディングを使用し、既定値は `false` です。
- `GroupName`型の `string` 。これは、相互に排他的なコントロールを指定する名前を定義し `RadioButton` ます。 このプロパティの既定値は `null` です。

これらのプロパティは、[`BindableProperty`](xref:Xamarin.Forms.BindableProperty) オブジェクトが基になっています。つまり、これらは、データ バインディングの対象にすることができ、スタイルを設定できます。

コントロールは、 `RadioButton` `CheckedChanged` `IsChecked` ユーザーまたはプログラムによる操作によってプロパティが変更されたときに発生するイベントを定義します。 `CheckedChangedEventArgs`イベントに付随するオブジェクト `CheckedChanged` には、型のという名前のプロパティが1つあり `Value` `bool` ます。 イベントが発生すると、プロパティの値は `Value` プロパティの新しい値に設定され `IsChecked` ます。

また、クラスは、 `RadioButton` クラスから次の一般的に使用されるプロパティを継承し [`Button`](xref:Xamarin.Forms.Button) ます。

- [`Command`](xref:Xamarin.Forms.Button.Command)`ICommand`が選択されたときに実行される、型の `RadioButton` 。
- [`CommandParameter`](xref:Xamarin.Forms.Button.CommandParameter)型の `object` 。これは、に渡されるパラメーターです `Command` 。
- [`FontAttributes`](xref:Xamarin.Forms.Button.FontAttributes)[`FontAttributes`](xref:Xamarin.Forms.FontAttributes)テキストスタイルを決定する型の。
- [`FontFamily`](xref:Xamarin.Forms.Button.FontFamily)`string`フォントファミリを定義する型の。
- [`FontSize`](xref:Xamarin.Forms.Button.FontSize)`double`フォントサイズを定義する型の。
- [`Text`](xref:Xamarin.Forms.Button.Text)`string`表示されるテキストを定義する型の。
- [`TextColor`](xref:Xamarin.Forms.Button.TextColor)[`Color`](xref:Xamarin.Forms.Color)表示されるテキストの色を定義する型の。

コントロールの詳細については [`Button`](xref:Xamarin.Forms.Button) 、「 [ Xamarin.Forms ボタン](~/xamarin-forms/user-interface/button.md)」を参照してください。

## <a name="create-radiobuttons"></a>Radiobutton の作成

次の例は、XAML でオブジェクトをインスタンス化する方法を示してい `RadioButton` ます。

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

この例で `RadioButton` は、オブジェクトは同じ親コンテナー内に暗黙的にグループ化されています。 この XAML は、次のスクリーンショットに示されているような外観になります。

![IOS と Android での暗黙的にグループ化された Radiobutton のスクリーンショット](radiobutton-images/radiobuttons.png "IOS と Android で暗黙的にグループ化された Radiobutton")

また、 `RadioButton` コードでオブジェクトを作成することもできます。

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
- `GroupName`各オプションボタンのプロパティを同じ値に設定します。 これを明示的なグループ化と呼びます。

次の XAML の例では、 `RadioButton` プロパティを設定することによってオブジェクトを明示的にグループ化してい `GroupName` ます。

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

この例では、それぞれ `RadioButton` が同じ値を共有するため、相互に排他的です `GroupName` 。 この XAML は、次のスクリーンショットに示されているような外観になります。

![IOS と Android での明示的にグループ化された Radiobutton のスクリーンショット](radiobutton-images/grouped-radiobuttons.png "IOS と Android での明示的にグループ化された Radiobutton")

## <a name="respond-to-a-radiobutton-state-change"></a>RadioButton 状態の変更に応答する

ラジオ ボタンにはオンまたはオフの 2 つの状態があります。 オプションボタンが選択されている場合、その `IsChecked` プロパティはに `true` なります。 ラジオ ボタンが選択されていない場合、`IsChecked` プロパティは `false` です。 同じグループ内の別のラジオ ボタンをクリックするとラジオ ボタンをクリアにできますが、ボタンをもう一度クリックしてもクリアにすることはできません。 ただし、プログラムで `IsChecked` プロパティを `false` に設定すると、ラジオ ボタンをオフにすることができます。

`IsChecked`ユーザーまたはプログラムによる操作によってプロパティが変更されると、 `CheckedChanged` イベントが発生します。 このイベントのイベントハンドラーは、変更に応答するように登録できます。

```xaml
<RadioButton Text="Red"
             TextColor="Red"
             GroupName="colors"
             CheckedChanged="OnColorsRadioButtonCheckedChanged" />
```

分離コードには、イベントのハンドラーが含まれてい `CheckedChanged` ます。

```csharp
void OnColorsRadioButtonCheckedChanged(object sender, CheckedChangedEventArgs e)
{
    // Perform required operation
}
```

`sender`引数は、 `RadioButton` このイベントを担当するです。 これを使用すると、オブジェクトにアクセスし `RadioButton` たり、同じイベントハンドラーを共有する複数のオブジェクトを区別したりでき `RadioButton` `CheckedChanged` ます。

または、イベントのイベントハンドラーを `CheckedChanged` コードに登録できます。

```csharp
RadioButton radioButton = new RadioButton { ... };
radioButton.CheckedChanged += (sender, e) =>
{
    // Perform required operation
};
```

> [!NOTE]
> 状態の変化に対応するための別の方法として `RadioButton` 、を定義 `ICommand` し、それをプロパティに割り当てることが `RadioButton.Command` できます。 詳細については、「 [ボタン: コマンドインターフェイスの使用](~/xamarin-forms/user-interface/button.md#using-the-command-interface)」を参照してください。

## <a name="radiobutton-visual-states"></a>RadioButton の表示状態

`RadioButton` が選択されている場合に、 `IsChecked` [`VisualState`](xref:Xamarin.Forms.VisualState) ビジュアルの変更を開始するために使用できるがあり `RadioButton` ます。

次の XAML の例は、状態の表示状態を定義する方法を示してい `IsChecked` ます。

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

この例では、暗黙的な [`Style`](xref:Xamarin.Forms.Style) によって `RadioButton` オブジェクトがターゲットにされています。 が選択されている `IsChecked` [`VisualState`](xref:Xamarin.Forms.VisualState) 場合、 `RadioButton` その `TextColor` プロパティは、値1を使用して緑色に設定され `Opacity` ます。 は、 `Normal` `VisualState` がクリア状態のときに、 `RadioButton` `TextColor` 値0.5 を使用してそのプロパティを赤に設定することを指定し `Opacity` ます。 したがって、全体の効果として、がクリアされている場合は赤、部分的に透明になり、 `RadioButton` 透明度のない緑は選択されると、次のようになります。

![IOS と Android の表示状態によって設定された RadioButton の外観のスクリーンショット](radiobutton-images/ischecked-visualstate.png "IOS および Android での RadioButton の表示状態")

ビジュアルの状態の詳細については、「[Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)」をご覧ください。

## <a name="disable-a-radiobutton"></a>RadioButton を無効にする

アプリケーションが、チェックされるが有効な操作ではない状態になること `RadioButton` があります。 このような場合は、 `RadioButton` プロパティをに設定することで、を無効にすることができ `IsEnabled` `false` ます。

## <a name="related-links"></a>関連リンク

- [RadioButton のデモ (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-radiobuttondemos/)
- [Xamarin.Forms ボタン](~/xamarin-forms/user-interface/button.md)
- [Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)