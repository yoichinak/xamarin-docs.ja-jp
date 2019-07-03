---
title: Xamarin.Forms のチェック ボックス
description: Xamarin.Forms のチェック ボックスをオンには、チェック アウトか、または空にするボタンの一種です。 チェック ボックスをオンにした場合は、上にある見なしています。 チェック ボックスが空の場合は、オフになって見なしています。
ms.prod: xamarin
ms.assetid: B8B9268B-BCB8-42B9-B08C-C0F22C137238
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/11/2019
ms.openlocfilehash: 42631b1b67dc1d342e9f8666916604e68ee158d8
ms.sourcegitcommit: 0fd04ea3af7d6a6d6086525306523a5296eec0df
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/02/2019
ms.locfileid: "67517921"
---
# <a name="xamarinforms-checkbox"></a>Xamarin.Forms のチェック ボックス

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/CheckBoxDemos)

Xamarin.Forms`CheckBox`かボタンの種類が checked または空にするには。 チェック ボックスをオンにした場合は、上にある見なしています。 チェック ボックスが空の場合は、オフになって見なしています。

`CheckBox` 定義、`bool`という名前のプロパティ`IsChecked`を示すかどうか、`CheckBox`がチェックされます。 このプロパティはによる支援も、 [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty)オブジェクトで、スタイルを設定できる、および、データ バインディングのターゲットにすることを意味します。

> [!NOTE]
> `IsChecked`バインド可能なプロパティが既定のバインド モードの[ `BindingMode.TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay)します。

`CheckBox` 定義、`CheckedChanged`ときに発生したイベントの`IsChecked`プロパティの変更、ユーザー操作によって、またはアプリケーションを設定すると、`IsChecked`プロパティ。 `CheckedChangedEventArgs`に付属しているオブジェクト、`CheckedChanged`イベントという名前の 1 つのプロパティには、 `Value`、型の`bool`します。 ときにイベントが発生しての値、`Value`の新しい値に設定されて、`IsChecked`プロパティ。

## <a name="create-a-checkbox"></a>チェック ボックスを作成します。

次の例では、インスタンス化する方法を示しています、 `CheckBox` XAML で。

```xaml
<CheckBox />
```

この XAML は、次のスクリーン ショットに示すように外観が得られます。

![IOS と Android で、空のチェック ボックスのスクリーン ショット](checkbox-images/checkbox-empty.png "空のチェック ボックス")

既定で、`CheckBox`が空です。 `CheckBox`によってユーザーの操作、または設定によってチェックすることができます、`IsChecked`プロパティを`true`:

```xaml
<CheckBox IsChecked="true" />
```

この XAML は、次のスクリーン ショットに示すように外観が得られます。

![IOS と Android でのチェックされたチェック ボックスのスクリーン ショット](checkbox-images/checkbox-checked.png " チェック ボックスをオンになって")

または、`CheckBox`コードで作成できます。

```csharp
CheckBox checkBox = new CheckBox { IsChecked = true };
```

## <a name="respond-to-a-checkbox-changing-state"></a>状態を変更するチェック ボックスをオンに応答します。

ときに、`IsChecked`プロパティの変更、ユーザー操作によって、またはアプリケーションを設定すると、`IsChecked`プロパティ、`CheckedChanged`イベントが発生します。 このイベントのイベント ハンドラーは、その変更に対応する登録できます。

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

または、イベント ハンドラー、`CheckedChanged`コードでイベントを登録することができます。

```csharp
CheckBox checkBox = new CheckBox { ... };
checkBox.CheckedChanged += (sender, e) =>
{
    // Perform required operation after examining e.Value
};
```

## <a name="data-bind-a-checkbox"></a>データをバインドするチェック ボックス

`CheckedChanged`に対応するデータのバインドとトリガーを使用してイベント ハンドラーを取り除くことができます、 `CheckBox` checked または空にします。

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

この例で、 [ `Label` ](xref:Xamarin.Forms.Label)データ トリガーのバインディング式を使用して監視を`IsChecked`のプロパティ、`CheckBox`します。 このプロパティになった`true`、`FontAttributes`と`FontSize`のプロパティ、`Label`を変更します。 ときに、`IsChecked`プロパティを返します`false`、`FontAttributes`と`FontSize`のプロパティ、`Label`を初期状態にリセットされます。

IOS のスクリーン ショットは、次のスクリーン ショットでは、 [ `Label` ](xref:Xamarin.Forms.Label)ときに書式設定、 `CheckBox` Android スクリーン ショットは、空の場合は、`Label`ときに書式設定、`CheckBox`がチェックされます。

[![データのスクリーン ショットでは、iOS と Android でチェック ボックスをオンがバインドされている](checkbox-images/checkbox-databinding.png "チェック ボックスがデータにバインドされている")](checkbox-images/checkbox-databinding-large.png#lightbox "データ バインドのチェック ボックス")

トリガーの詳細については、次を参照してください。 [Xamarin.Forms トリガー](~/xamarin-forms/app-fundamentals/triggers.md)します。

## <a name="disable-a-checkbox"></a>チェック ボックスを無効にします。

アプリケーションが状態に入ることがあります、`CheckBox`チェックが有効な操作ではありません。 このような場合は、`CheckBox`設定で無効にすることができます、`IsEnabled`プロパティを`false`します。

## <a name="checkbox-appearance"></a>チェック ボックスの外観

プロパティに加えを`CheckBox`から継承、 [ `View` ](xref:Xamarin.Forms.View)クラス、`CheckBox`も定義、`Color`にその色を設定するプロパティを[ `Color` ](xref:Xamarin.Forms.Color):

```xaml
<CheckBox Color="Red" />
```

次のスクリーン ショットは、一連のチェックを表示する`CheckBox`オブジェクト、各オブジェクトには、その`Color`プロパティ別に設定[ `Color` ](xref:Xamarin.Forms.Color):

![IOS と Android での色つきのチェック ボックスのスクリーン ショット](checkbox-images/checkbox-colors.png "色つきのチェック ボックス")

## <a name="checkbox-visual-states"></a>表示状態のチェック ボックス

`CheckBox` `IsChecked` [ `VisualState` ](xref:Xamarin.Forms.VisualState)を視覚的な変更の開始に使用できる、`CheckBox`がチェックされます。

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

この例で、 `IsChecked` [ `VisualState` ](xref:Xamarin.Forms.VisualState)される場合、`CheckBox`がチェックされ、その`Color`プロパティが緑色に設定されます。 `Normal` `VisualState`される場合、`CheckBox`通常の状態では、その`Color`プロパティを red に設定されます。 そのため、全体的な影響は、`CheckBox`はオンの場合に、空、および緑がある場合に赤。

表示状態の詳細については、次を参照してください。 [Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)します。

## <a name="related-links"></a>関連リンク

- [チェック ボックスをオンのデモ (サンプル)](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/CheckBoxDemos)
- [Xamarin.Forms のトリガー](~/xamarin-forms/app-fundamentals/triggers.md)
- [Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)
