---
title: Xamarin.Forms CheckBox
description: '[Xamarin. Forms] チェックボックスは、オンまたはオフにできるボタンの種類です。 チェックボックスがオンになっている場合は、オンになっていると見なされます。 チェックボックスが空の場合は、オフになっていると見なされます。'
ms.prod: xamarin
ms.assetid: B8B9268B-BCB8-42B9-B08C-C0F22C137238
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/11/2019
ms.openlocfilehash: 10b7c4c3478545863ef49a23ef0f1be777e7eda9
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/29/2020
ms.locfileid: "82517123"
---
# <a name="xamarinforms-checkbox"></a>Xamarin.Forms CheckBox

[![](~/media/shared/download.png)サンプルをダウンロードするサンプルをダウンロードする](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-checkboxdemos/)

Xamarin. Forms `CheckBox`は、チェックまたは空にすることができるボタンの一種です。 チェックボックスがオンになっている場合は、オンになっていると見なされます。 チェックボックスが空の場合は、オフになっていると見なされます。

`CheckBox`がチェック`bool`され`IsChecked`て`CheckBox`いるかどうかを示す、という名前のプロパティを定義します。 このプロパティは、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)オブジェクトによってもサポートされます。これは、データバインディングのターゲットとしてスタイルを設定できることを意味します。

> [!NOTE]
> バインド`IsChecked`可能なプロパティには、の既定[`BindingMode.TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay)のバインディングモードがあります。

`CheckBox`ユーザー操作`CheckedChanged`によって、または`IsChecked`アプリケーションによってプロパティが`IsChecked`設定されたときに、プロパティが変更されたときに発生するイベントを定義します。 イベント`CheckedChangedEventArgs`に`CheckedChanged`付随するオブジェクトには、型`Value` `bool`のという名前のプロパティが1つあります。 イベントが発生すると、プロパティの値`Value`は`IsChecked`プロパティの新しい値に設定されます。

## <a name="create-a-checkbox"></a>チェックボックスを作成する

次の例は、XAML でを`CheckBox`インスタンス化する方法を示しています。

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

![IOS と Android のチェックボックスがオンになっているスクリーンショット](checkbox-images/checkbox-checked.png "チェックされたチェックボックス")

また、コード`CheckBox`でを作成することもできます。

```csharp
CheckBox checkBox = new CheckBox { IsChecked = true };
```

## <a name="respond-to-a-checkbox-changing-state"></a>チェックボックスの状態の変更に応答する

ユーザー操作`IsChecked`によって、またはアプリケーションが`IsChecked`プロパティを設定するときに、プロパティ`CheckedChanged`が変更されると、イベントが発生します。 このイベントのイベントハンドラーは、変更に応答するように登録できます。

```xaml
<CheckBox CheckedChanged="OnCheckBoxCheckedChanged" />
```

分離コードファイルには、 `CheckedChanged`イベントのハンドラーが含まれています。

```csharp
void OnCheckBoxCheckedChanged(object sender, CheckedChangedEventArgs e)
{
    // Perform required operation after examining e.Value
}
```

`sender`引数は、この`CheckBox`イベントを担当するです。 これを使用すると、 `CheckBox`オブジェクトにアクセスしたり、同じ`CheckBox` `CheckedChanged`イベントハンドラーを共有する複数のオブジェクトを区別したりできます。

または、 `CheckedChanged`イベントのイベントハンドラーをコードに登録できます。

```csharp
CheckBox checkBox = new CheckBox { ... };
checkBox.CheckedChanged += (sender, e) =>
{
    // Perform required operation after examining e.Value
};
```

## <a name="data-bind-a-checkbox"></a>チェックボックスのデータバインド

`CheckedChanged`イベントハンドラーを削除するには、データバインディングとトリガーを使用して`CheckBox` 、チェックされるまたは空のに応答します。

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

この例では、 [`Label`](xref:Xamarin.Forms.Label)はデータトリガーでバインド式を使用して、 `IsChecked` `CheckBox`のプロパティを監視します。 このプロパティがに`true`なると`FontAttributes` 、 `FontSize`のプロパティと`Label`プロパティが変更されます。 プロパティが`IsChecked`に`false`戻ると、の`FontAttributes`プロパティ`FontSize`とプロパティ`Label`が初期状態にリセットされます。

次のスクリーンショット[`Label`](xref:Xamarin.Forms.Label)では、 `CheckBox`が空の場合、iOS のスクリーンショットに書式が表示され`Label`ます。一方`CheckBox` 、Android のスクリーンショットでは、がオンになっているときに書式が示されています。

[![IOS と Android でのデータバインドチェックボックスのスクリーンショット](checkbox-images/checkbox-databinding.png "データバインドチェックボックス")](checkbox-images/checkbox-databinding-large.png#lightbox "データバインドチェックボックス")

トリガーの詳細については、「 [Xamarin. Forms triggers](~/xamarin-forms/app-fundamentals/triggers.md)」を参照してください。

## <a name="disable-a-checkbox"></a>チェックボックスを無効にする

アプリケーションが、チェック`CheckBox`されるが有効な操作ではない状態になることがあります。 このような場合は`CheckBox` 、 `IsEnabled`プロパティをに設定する`false`ことで、を無効にすることができます。

## <a name="checkbox-appearance"></a>CheckBox の外観

[`View`](xref:Xamarin.Forms.View)クラスから継承される`CheckBox` `Color` [`Color`](xref:Xamarin.Forms.Color) `CheckBox`プロパティに加えて、は、その色をに設定するプロパティも定義します。

```xaml
<CheckBox Color="Red" />
```

次のスクリーンショットは、一連の`CheckBox`チェックされたオブジェクトを示し`Color`ています。各[`Color`](xref:Xamarin.Forms.Color)オブジェクトでは、プロパティが別のに設定されています。

![IOS と Android の色分けされたチェックボックスのスクリーンショット](checkbox-images/checkbox-colors.png "色付きのチェックボックス")

## <a name="checkbox-visual-states"></a>チェックボックスの表示状態

`CheckBox`には、がチェックされたときに、への`CheckBox`ビジュアル変更を開始するために使用できるがあります。 `IsChecked` [`VisualState`](xref:Xamarin.Forms.VisualState)

次の XAML の例は、 `IsChecked`状態の表示状態を定義する方法を示しています。

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

この例`IsChecked` [`VisualState`](xref:Xamarin.Forms.VisualState)では、をオンにする`CheckBox`と、 `Color`プロパティが緑色に設定されることを指定します。 `Normal` `VisualState`は、 `CheckBox`が通常の状態のときに、 `Color`プロパティを赤に設定することを指定します。 したがって、全体の効果とし`CheckBox`て、が空の場合は赤色になり、チェックされる場合は緑色になります。

表示状態の詳細については、「 [Xamarin. Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [チェックボックスのデモ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-checkboxdemos/)
- [Xamarin.Forms のトリガー](~/xamarin-forms/app-fundamentals/triggers.md)
- [Xamarin Forms State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)
