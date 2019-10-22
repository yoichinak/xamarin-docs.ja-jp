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
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "68739156"
---
# <a name="xamarinforms-checkbox"></a>Xamarin. フォームチェックボックス

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-checkboxdemos/)

Xamarin `CheckBox` は、チェックまたは空にすることができるボタンの一種です。 チェックボックスがオンになっている場合は、オンになっていると見なされます。 チェックボックスが空の場合は、オフになっていると見なされます。

`CheckBox` は、`CheckBox` がチェックされるかどうかを示す、`IsChecked` という名前の `bool` プロパティを定義します。 このプロパティは、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)オブジェクトによってもサポートされます。これは、データバインディングのターゲットとしてスタイルを設定できることを意味します。

> [!NOTE]
> @No__t_0 バインド可能なプロパティには、 [`BindingMode.TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay)の既定のバインディングモードがあります。

`CheckBox` は、ユーザー操作によって、またはアプリケーションが `IsChecked` プロパティを設定するときに、`IsChecked` プロパティが変更されたときに発生する `CheckedChanged` イベントを定義します。 @No__t_1 イベントに付随する `CheckedChangedEventArgs` オブジェクトには、`bool` 型の `Value` という名前のプロパティが1つあります。 イベントが発生すると、`Value` プロパティの値が `IsChecked` プロパティの新しい値に設定されます。

## <a name="create-a-checkbox"></a>チェックボックスを作成する

次の例は、XAML で `CheckBox` をインスタンス化する方法を示しています。

```xaml
<CheckBox />
```

この XAML は、次のスクリーンショットに示されているような外観になります。

![IOS と Android の空のチェックボックスのスクリーンショット](checkbox-images/checkbox-empty.png "空のチェックボックス")

既定では、`CheckBox` は空です。 @No__t_0 を確認するには、ユーザー操作を使用するか、`IsChecked` プロパティを `true` に設定します。

```xaml
<CheckBox IsChecked="true" />
```

この XAML は、次のスクリーンショットに示されているような外観になります。

![IOS と Android のチェックボックスがオンになっているスクリーンショット](checkbox-images/checkbox-checked.png "チェックされたチェックボックス")

また、コードで `CheckBox` を作成することもできます。

```csharp
CheckBox checkBox = new CheckBox { IsChecked = true };
```

## <a name="respond-to-a-checkbox-changing-state"></a>チェックボックスの状態の変更に応答する

ユーザー操作によって、またはアプリケーションが `IsChecked` プロパティを設定したときに `IsChecked` プロパティが変更されると、`CheckedChanged` イベントが発生します。 このイベントのイベントハンドラーは、変更に応答するように登録できます。

```xaml
<CheckBox CheckedChanged="OnCheckBoxCheckedChanged" />
```

分離コードファイルには、`CheckedChanged` イベントのハンドラーが含まれています。

```csharp
void OnCheckBoxCheckedChanged(object sender, CheckedChangedEventArgs e)
{
    // Perform required operation after examining e.Value
}
```

@No__t_0 引数は、このイベントを担当する `CheckBox` です。 これを使用すると、`CheckBox` オブジェクトにアクセスしたり、同じ `CheckedChanged` イベントを共有する複数の `CheckBox` オブジェクトを区別したりできます。

または、`CheckedChanged` イベントのイベントハンドラーをコードに登録できます。

```csharp
CheckBox checkBox = new CheckBox { ... };
checkBox.CheckedChanged += (sender, e) =>
{
    // Perform required operation after examining e.Value
};
```

## <a name="data-bind-a-checkbox"></a>チェックボックスのデータバインド

@No__t_0 イベントハンドラーを削除するには、データバインディングとトリガーを使用して、確認または空の `CheckBox` に応答します。

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

この例では、 [`Label`](xref:Xamarin.Forms.Label)はデータトリガーでバインド式を使用して、`CheckBox` の `IsChecked` プロパティを監視します。 このプロパティが `true` になると、`Label` の `FontAttributes` と `FontSize` のプロパティが変更されます。 @No__t_0 プロパティが `false` に戻ると、`Label` の `FontAttributes` および `FontSize` プロパティが初期状態にリセットされます。

次のスクリーンショットでは、iOS のスクリーンショットに、`CheckBox` が空の場合の[`Label`](xref:Xamarin.Forms.Label)の書式設定が示されています。一方、Android のスクリーンショットでは、`CheckBox` が確認されたときに `Label` の書式が示されています。

[![IOS と Android でのデータバインドチェックボックスのスクリーンショット](checkbox-images/checkbox-databinding.png "データバインドチェックボックス")](checkbox-images/checkbox-databinding-large.png#lightbox "データバインドチェックボックス")

トリガーの詳細については、「 [Xamarin. Forms triggers](~/xamarin-forms/app-fundamentals/triggers.md)」を参照してください。

## <a name="disable-a-checkbox"></a>チェックボックスを無効にする

アプリケーションが、チェックされている `CheckBox` が有効な操作ではない状態になることがあります。 このような場合は、`IsEnabled` プロパティを `false` に設定することによって、`CheckBox` を無効にできます。

## <a name="checkbox-appearance"></a>チェックボックスの外観

[@No__t_2](xref:Xamarin.Forms.View)クラスから継承 `CheckBox` プロパティに加えて、`CheckBox` の色を[`Color`](xref:Xamarin.Forms.Color)に設定する `Color` プロパティも定義します。

```xaml
<CheckBox Color="Red" />
```

次のスクリーンショットは、一連のチェックされた `CheckBox` オブジェクトを示しています。各オブジェクトは、`Color` プロパティを別の[`Color`](xref:Xamarin.Forms.Color)に設定しています。

![IOS と Android の色分けされたチェックボックスのスクリーンショット](checkbox-images/checkbox-colors.png "色付きのチェックボックス")

## <a name="checkbox-visual-states"></a>チェックボックスの表示状態

`CheckBox` には `IsChecked` [`VisualState`](xref:Xamarin.Forms.VisualState)があります。これを使用すると、`CheckBox` がチェックされたときに変更を開始できます。

次の XAML の例は、`IsChecked` 状態の表示状態を定義する方法を示しています。

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

この例では、`IsChecked` [`VisualState`](xref:Xamarin.Forms.VisualState)は、`CheckBox` がチェックされると、その `Color` プロパティが緑色に設定されることを指定します。 @No__t_0 `VisualState` は、`CheckBox` が通常の状態であるときに、その `Color` プロパティが赤に設定されることを指定します。 したがって、全体的な効果として、`CheckBox` が空の場合は赤、チェックされる場合は緑色になります。

表示状態の詳細については、「 [Xamarin. Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [チェックボックスのデモ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-checkboxdemos/)
- [Xamarin. フォームトリガー](~/xamarin-forms/app-fundamentals/triggers.md)
- [Xamarin Forms State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)
