---
タイトル: " Xamarin.Forms checkbox" 説明: チェックボックスは、チェック Xamarin.Forms または空にすることができるボタンの種類です。 チェックボックスがオンになっている場合は、オンになっていると見なされます。 チェックボックスが空の場合は、オフになっていると見なされます。
ms. 製品: xamarin ms. assetid: B8B9268B-BCB8-42B9-B08C-C0F22C137238: xamarin-forms author: davidbritch: dabritch ms. date: 06/11/2019 no loc:
- "Xamarin.Forms"
- "Xamarin.Essentials"

---

# <a name="xamarinforms-checkbox"></a>Xamarin.Forms CheckBox

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/userinterface-checkboxdemos/)

は、 Xamarin.Forms `CheckBox` チェックを行うか空にすることができるボタンの種類です。 チェックボックスがオンになっている場合は、オンになっていると見なされます。 チェックボックスが空の場合は、オフになっていると見なされます。

`CheckBox``bool` `IsChecked` がチェックされているかどうかを示す、という名前のプロパティを定義し `CheckBox` ます。 このプロパティは、オブジェクトによってもサポートされます。これは、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) データバインディングのターゲットとしてスタイルを設定できることを意味します。

> [!NOTE]
> バインド可能なプロパティには、 `IsChecked` の既定のバインディングモードがあり [`BindingMode.TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay) ます。

`CheckBox``CheckedChanged` `IsChecked` ユーザー操作によって、またはアプリケーションによってプロパティが設定されたときに、プロパティが変更されたときに発生するイベントを定義し `IsChecked` ます。 `CheckedChangedEventArgs`イベントに付随するオブジェクト `CheckedChanged` には、型のという名前のプロパティが1つあり `Value` `bool` ます。 イベントが発生すると、プロパティの値は `Value` プロパティの新しい値に設定され `IsChecked` ます。

## <a name="create-a-checkbox"></a>チェックボックスを作成する

次の例は、XAML でをインスタンス化する方法を示してい `CheckBox` ます。

```xaml
<CheckBox />
```

この XAML は、次のスクリーンショットに示されているような外観になります。

![IOS と Android の空のチェックボックスのスクリーンショット](checkbox-images/checkbox-empty.png "空のチェックボックス")

既定では、 `CheckBox` は空です。 は、 `CheckBox` ユーザー操作によって確認することも、プロパティをに設定することによって確認することもでき `IsChecked` `true` ます。

```xaml
<CheckBox IsChecked="true" />
```

この XAML は、次のスクリーンショットに示されているような外観になります。

![IOS と Android のチェックボックスがオンになっているスクリーンショット](checkbox-images/checkbox-checked.png "チェックされたチェックボックス")

また、 `CheckBox` コードでを作成することもできます。

```csharp
CheckBox checkBox = new CheckBox { IsChecked = true };
```

## <a name="respond-to-a-checkbox-changing-state"></a>チェックボックスの状態の変更に応答する

`IsChecked`ユーザー操作によって、またはアプリケーションがプロパティを設定するときに、プロパティが変更されると、 `IsChecked` `CheckedChanged` イベントが発生します。 このイベントのイベントハンドラーは、変更に応答するように登録できます。

```xaml
<CheckBox CheckedChanged="OnCheckBoxCheckedChanged" />
```

分離コードファイルには、イベントのハンドラーが含まれてい `CheckedChanged` ます。

```csharp
void OnCheckBoxCheckedChanged(object sender, CheckedChangedEventArgs e)
{
    // Perform required operation after examining e.Value
}
```

`sender`引数は、 `CheckBox` このイベントを担当するです。 これを使用すると、オブジェクトにアクセスし `CheckBox` たり、同じイベントハンドラーを共有する複数のオブジェクトを区別したりでき `CheckBox` `CheckedChanged` ます。

または、イベントのイベントハンドラーを `CheckedChanged` コードに登録できます。

```csharp
CheckBox checkBox = new CheckBox { ... };
checkBox.CheckedChanged += (sender, e) =>
{
    // Perform required operation after examining e.Value
};
```

## <a name="data-bind-a-checkbox"></a>チェックボックスのデータバインド

イベントハンドラーを削除するには、 `CheckedChanged` データバインディングとトリガーを使用して、 `CheckBox` チェックされるまたは空のに応答します。

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

この例では、は [`Label`](xref:Xamarin.Forms.Label) データトリガーでバインド式を使用して `IsChecked` 、のプロパティを監視し `CheckBox` ます。 このプロパティがになると、 `true` `FontAttributes` `FontSize` のプロパティとプロパティが変更され `Label` ます。 プロパティが `IsChecked` に戻ると、 `false` `FontAttributes` `FontSize` のプロパティとプロパティ `Label` が初期状態にリセットされます。

次のスクリーンショットでは、が空の場合、iOS のスクリーンショットに書式が表示 [`Label`](xref:Xamarin.Forms.Label) `CheckBox` されます。一方、Android のスクリーンショットでは、がオンになっているときに書式が示されて `Label` `CheckBox` います。

[![IOS と Android でのデータバインドチェックボックスのスクリーンショット](checkbox-images/checkbox-databinding.png "データバインドチェックボックス")](checkbox-images/checkbox-databinding-large.png#lightbox "データバインドチェックボックス")

トリガーの詳細については、「 [ Xamarin.Forms トリガー](~/xamarin-forms/app-fundamentals/triggers.md)」を参照してください。

## <a name="disable-a-checkbox"></a>チェックボックスを無効にする

アプリケーションが、チェックされるが有効な操作ではない状態になること `CheckBox` があります。 このような場合は、 `CheckBox` プロパティをに設定することで、を無効にすることができ `IsEnabled` `false` ます。

## <a name="checkbox-appearance"></a>CheckBox の外観

クラスから継承されるプロパティに加えて `CheckBox` [`View`](xref:Xamarin.Forms.View) 、 `CheckBox` は、 `Color` その色をに設定するプロパティも定義し [`Color`](xref:Xamarin.Forms.Color) ます。

```xaml
<CheckBox Color="Red" />
```

次のスクリーンショットは、一連のチェックされたオブジェクトを示して `CheckBox` います。各オブジェクトでは、 `Color` プロパティが別のに設定されてい [`Color`](xref:Xamarin.Forms.Color) ます。

![IOS と Android の色分けされたチェックボックスのスクリーンショット](checkbox-images/checkbox-colors.png "色付きのチェックボックス")

## <a name="checkbox-visual-states"></a>チェックボックスの表示状態

`CheckBox` には、が `IsChecked` [`VisualState`](xref:Xamarin.Forms.VisualState) チェックされたときに、へのビジュアル変更を開始するために使用できるがあり `CheckBox` ます。

次の XAML の例は、状態の表示状態を定義する方法を示してい `IsChecked` ます。

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

この例では、をオンにする `IsChecked` [`VisualState`](xref:Xamarin.Forms.VisualState) と、 `CheckBox` `Color` プロパティが緑色に設定されることを指定します。 は `Normal` `VisualState` 、 `CheckBox` が通常の状態のときに、プロパティを赤に設定することを指定し `Color` ます。 したがって、全体の効果として、が空の場合は赤色になり、チェックされる場合は緑色になり `CheckBox` ます。

ビジュアルの状態の詳細については、「[Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)」をご覧ください。

## <a name="related-links"></a>関連リンク

- [チェックボックスのデモ (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-checkboxdemos/)
- [Xamarin.Forms のトリガー](~/xamarin-forms/app-fundamentals/triggers.md)
- [Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)