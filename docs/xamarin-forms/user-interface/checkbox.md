---
title: Xamarin. フォームチェックボックス
description: '[Xamarin. Forms] チェックボックスは、オンまたはオフにできるボタンの種類です。 チェックボックスがオンになっている場合は、オンになっていると見なされます。 チェックボックスが空の場合は、オフになっていると見なされます。'
ms.prod: xamarin
ms.assetid: B8B9268B-BCB8-42B9-B08C-C0F22C137238
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/11/2019
ms.openlocfilehash: f78ca9d2cf7a9e57b81c5d923c64b36a7982c4b0
ms.sourcegitcommit: c6e56545eafd8ff9e540d56aba32aa6232c5315f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/02/2019
ms.locfileid: "68739156"
---
# <a name="xamarinforms-checkbox"></a>Xamarin. フォームチェックボックス

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-checkboxdemos/)

Xamarin. Forms `CheckBox`は、チェックまたは空にすることができるボタンの一種です。 チェックボックスがオンになっている場合は、オンになっていると見なされます。 チェックボックスが空の場合は、オフになっていると見なされます。

`CheckBox`がチェック`bool`され`IsChecked`て`CheckBox`いるかどうかを示す、という名前のプロパティを定義します。 このプロパティは、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)オブジェクトによってもサポートされます。これは、データバインディングのターゲットとしてスタイルを設定できることを意味します。

> [!NOTE]
> バインド`IsChecked`可能なプロパティには、の既定[`BindingMode.TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay)のバインディングモードがあります。

`CheckBox`ユーザー操作`CheckedChanged`によって、または`IsChecked`アプリケーションによってプロパティが`IsChecked`設定されたときに、プロパティが変更されたときに発生するイベントを定義します。 イベントに`Value` `bool`付随する`CheckedChangedEventArgs`オブジェクトには、型のという名前のプロパティが1つあります。 `CheckedChanged` イベントが発生すると、プロパティの値`Value`は`IsChecked`プロパティの新しい値に設定されます。

## <a name="create-a-checkbox"></a>チェックボックスを作成する

次の例では、インスタンス化する方法を示しています、 `CheckBox` XAML で。

```xaml
<CheckBox />
```

この XAML は、次のスクリーンショットに示されているような外観になります。

![IOS と Android の空のチェックボックスのスクリーンショット](checkbox-images/checkbox-empty.png "空のチェックボックス")

既定では、 `CheckBox`は空です。 は`CheckBox` 、ユーザー操作によって確認することも、 `IsChecked`プロパティを`true`に設定することによって確認することもできます。

```xaml
<CheckBox IsChecked="true" />
```

この XAML は、次のスクリーンショットに示されているような外観になります。

![IOS と Android のチェックボックスがオン]になっているスクリーンショットチェックされた(checkbox-images/checkbox-checked.png "チェックボックス")

また、コード`CheckBox`でを作成することもできます。

```csharp
CheckBox checkBox = new CheckBox { IsChecked = true };
```

## <a name="respond-to-a-checkbox-changing-state"></a>チェックボックスの状態の変更に応答する

ユーザー操作`IsChecked`によって、またはアプリケーションが`IsChecked`プロパティを設定するときに、プロパティ`CheckedChanged`が変更されると、イベントが発生します。 このイベントのイベントハンドラーは、変更に応答するように登録できます。

```xaml
<CheckBox CheckedChanged="OnCheckBoxCheckedChanged" />
```

分離コード ファイルにはハンドラーが含まれています、`CheckedChanged`イベント。

```csharp
void OnCheckBoxCheckedChanged(object sender, CheckedChangedEventArgs e)
{
    // Perform required operation after examining e.Value
}
```

`sender`引数は、`CheckBox`このイベントを担当します。 これを使用して、アクセスすることができます、`CheckBox`オブジェクト、または複数を区別する`CheckBox`オブジェクトが同じ共有`CheckedChanged`イベント。

または、 `CheckedChanged`イベントのイベントハンドラーをコードに登録できます。

```csharp
CheckBox checkBox = new CheckBox { ... };
checkBox.CheckedChanged += (sender, e) =>
{
    // Perform required operation after examining e.Value
};
```

## <a name="data-bind-a-checkbox"></a>チェックボックスのデータバインド

イベントハンドラーを削除するには、データバインディングとトリガーを使用して`CheckBox` 、チェックされるまたは空のに応答します。 `CheckedChanged`

```xaml
<CheckBox x:Name="checkBox" />
<Label Text="Lorem ipsum dolor sit amet, elit rutrum, enim hendrerit augue vitae praesent sed non, lorem aenean quis praesent pede.">
    <Label.Triggers>
        <DataTrigger TargetType="Label"
                     Binding="{Binding Source={x:Reference checkBox}, Path=IsChecked}"
                     Value="true">
            <Setter Property="FontAttributes"
                    Value="Italic, Bold" />
            <Setter Property="FontSize"
                    Value="Large" />
        </DataTrigger>
    </Label.Triggers>
</Label>
```

この例では、 [`Label`](xref:Xamarin.Forms.Label)はデータトリガーでバインド式を使用して、 `IsChecked` `CheckBox`のプロパティを監視します。 このプロパティがに`true`なる`FontAttributes`と、 `FontSize`の`Label`プロパティとプロパティが変更されます。 プロパティが`IsChecked` `false`に戻ると`FontSize` 、のプロパティ`Label`とプロパティが初期状態にリセットさ`FontAttributes`れます。

次のスクリーン[`Label`](xref:Xamarin.Forms.Label)ショットでは、 `CheckBox`が空の場合、iOS のスクリーンショットに書式が表示されます。一方`CheckBox` 、Android のスクリーンショットでは、がオンになっているときに`Label`書式が示されています。

[![iOS と Android のデータバインドされたチェックボックスのスクリーンショット](checkbox-images/checkbox-databinding.png "データバインドされたチェックボックス")](checkbox-images/checkbox-databinding-large.png#lightbox "データバインドされたチェックボックス")

トリガーの詳細については、「 [Xamarin. Forms triggers](~/xamarin-forms/app-fundamentals/triggers.md)」を参照してください。

## <a name="disable-a-checkbox"></a>チェックボックスを無効にする

アプリケーションが、チェック`CheckBox`されるが有効な操作ではない状態になることがあります。 このような場合は`CheckBox` 、 `IsEnabled`プロパティをに設定する`false`ことで、を無効にすることができます。

## <a name="checkbox-appearance"></a>チェックボックスの外観

`CheckBox` `CheckBox` [クラスから継承`Color`](xref:Xamarin.Forms.Color)されるプロパティに加えて、は、その色をに設定するプロパティも定義します。`Color` [`View`](xref:Xamarin.Forms.View)

```xaml
<CheckBox Color="Red" />
```

次のスクリーンショットは、一連の`CheckBox`チェックされたオブジェクトを示し`Color`ています。各[`Color`](xref:Xamarin.Forms.Color)オブジェクトでは、プロパティが別のに設定されています。

![IOS と Android の色分けされたチェックボックスのスクリーンショット](checkbox-images/checkbox-colors.png "色付きのチェックボックス")

## <a name="checkbox-visual-states"></a>チェックボックスの表示状態

`CheckBox`には、がチェックされたときに、への`CheckBox`ビジュアル変更を開始するために使用できるがあります。 `IsChecked` [`VisualState`](xref:Xamarin.Forms.VisualState)

次の XAML の例のビジュアル状態を定義する方法を示しています、`IsChecked`状態。

```xaml
<CheckBox ...>
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">
                <VisualState.Setters>
                    <Setter Property="Color"
                            Value="Red" />
                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="IsChecked">
                <VisualState.Setters>
                    <Setter Property="Color"
                            Value="Green" />
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</CheckBox>
```

この例`IsChecked` `CheckBox`では、をオン`Color`にすると、プロパティが緑色に設定されることを[指定します。`VisualState`](xref:Xamarin.Forms.VisualState) `Normal` は、`CheckBox`が通常の状態`Color`のときに、プロパティを赤に設定することを指定します。`VisualState` したがって、全体の効果とし`CheckBox`て、が空の場合は赤色になり、チェックされる場合は緑色になります。

表示状態の詳細については、「 [Xamarin. Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [チェックボックスのデモ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-checkboxdemos/)
- [Xamarin. フォームトリガー](~/xamarin-forms/app-fundamentals/triggers.md)
- [Xamarin Forms State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)
