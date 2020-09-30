---
title: Xamarin.Forms 切り替わり
description: Xamarin.Formsスイッチは、ユーザーが状態のオンとオフを切り替えるために操作できるボタンの種類です。 この記事では、Switch クラスを使用して、切り替え UI 要素を表示する方法について説明します。
ms.prod: xamarin
ms.assetId: B2F9CC65-481B-4323-8E77-C6BE29C90DE9
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 05/19/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 94f77fd70fee595efd341ff7372828b12661442d
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2020
ms.locfileid: "91561730"
---
# <a name="no-locxamarinforms-switch"></a>Xamarin.Forms 切り替わり

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-switchdemos/)

Xamarin.Forms [`Switch`](xref:Xamarin.Forms.Switch) コントロールは水平方向のトグルボタンで、ユーザーは値で表されるオンとオフの状態を切り替えることができ `boolean` ます。 クラスは、 `Switch` から継承さ [`View`](xref:Xamarin.Forms.View) れます。

次のスクリーンショットは、 `Switch` iOS と Android での **オン** と **オフ** の切り替え状態のコントロールを示しています。

![IOS と Android でのオンとオフの状態の切り替えのスクリーンショット](switch-images/switch-states-default.png "IOS と Android でのスイッチ")

コントロールは、 `Switch` 次のプロパティを定義します。

- [`IsToggled`](xref:Xamarin.Forms.Switch.IsToggled)が `boolean` オンかどうかを示す値です `Switch` 。 **on**
- [`OnColor`](xref:Xamarin.Forms.Switch.OnColor) は、が `Color` `Switch` 切り替えられるか、状態で表示さ **れるかに**影響するです。
- `ThumbColor` は、 `Color` スイッチのつまみのです。

これらのプロパティはオブジェクトによって支えられています [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 。つまり、を `Switch` スタイル設定し、データバインディングのターゲットにすることができます。

コントロールは、 `Switch` `Toggled` `IsToggled` ユーザー操作を使用するか、アプリケーションがプロパティを設定するときに、プロパティが変更されたときに発生するイベントを定義し `IsToggled` ます。 `ToggledEventArgs`イベントに付随するオブジェクト `Toggled` には、型のという名前のプロパティが1つあり `Value` `bool` ます。 イベントが発生すると、プロパティの値に `Value` プロパティの新しい値が反映され `IsToggled` ます。

## <a name="create-a-switch"></a>スイッチを作成する

は、 `Switch` XAML でインスタンス化できます。 この `IsToggled` プロパティを設定すると、を切り替えることができ `Switch` ます。 既定では、 `IsToggled` プロパティは `false` です。 次の例は、 `Switch` 省略可能なプロパティセットを使用して、XAML でをインスタンス化する方法を示してい `IsToggled` ます。

```xaml
<Switch IsToggled="true"/>
```

は、 `Switch` コードで作成することもできます。

```csharp
Switch switchControl = new Switch { IsToggled = true };
```

## <a name="switch-appearance"></a>外観の切り替え

クラスから継承されるプロパティに加えて [`Switch`](xref:Xamarin.Forms.Switch) [`View`](xref:Xamarin.Forms.View) 、は `Switch` `OnColor` プロパティとプロパティも定義し `ThumbColor` ます。 プロパティを設定して、 `OnColor` 状態が `Switch` **オン** の状態に切り替わるときの色を定義できます。また、プロパティを設定して、 `ThumbColor` スイッチのつまみのを定義でき `Color` ます。 次の例は、これらのプロパティを設定して、XAML でをインスタンス化する方法を示してい `Switch` ます。

```xaml
<Switch OnColor="Orange"
        ThumbColor="Green" />
```

プロパティは、コードでを作成するときに設定することもでき `Switch` ます。

```csharp
Switch switch = new Switch { OnColor = Color.Orange, ThumbColor = Color.Green };
```

次のスクリーンショットは、 `Switch` プロパティとプロパティが設定されている、 **オン** と **オフ** の切り替え状態のを示してい `OnColor` `ThumbColor` ます。

![IOS と Android でのオンとオフの状態の切り替えのスクリーンショット](switch-images/switch-states-colors.png "IOS と Android でのスイッチ")

## <a name="respond-to-a-switch-state-change"></a>スイッチの状態の変更に応答する

`IsToggled`ユーザー操作によって、またはアプリケーションがプロパティを設定するときに、プロパティが変更されると、 `IsToggled` `Toggled` イベントが発生します。 このイベントのイベントハンドラーは、変更に応答するように登録できます。

```xaml
<Switch Toggled="OnToggled" />
```

分離コードファイルには、イベントのハンドラーが含まれてい `Toggled` ます。

```csharp
void OnToggled(object sender, ToggledEventArgs e)
{
    // Perform an action after examining e.Value
}
```

`sender`イベントハンドラーの引数は、 `Switch` このイベントの発生を担当するです。 プロパティを使用し `sender` てオブジェクトにアクセスしたり `Switch` 、同じイベントハンドラーを共有する複数のオブジェクトを区別したりでき `Switch` `Toggled` ます。

`Toggled`イベントハンドラーは、次のコードでも割り当てることができます。

```csharp
Switch switchControl = new Switch {...};
switchControl.Toggled += (sender, e) =>
{
    // Perform an action after examining e.Value
}
```

## <a name="data-bind-a-switch"></a>スイッチのデータバインド

イベントハンドラーを削除するには、 `Toggled` データバインディングとトリガーを使用して、変化するトグル状態に応答し `Switch` ます。

```xaml
<Switch x:Name="styleSwitch" />
<Label Text="Lorem ipsum dolor sit amet, elit rutrum, enim hendrerit augue vitae praesent sed non, lorem aenean quis praesent pede.">
    <Label.Triggers>
        <DataTrigger TargetType="Label"
                     Binding="{Binding Source={x:Reference styleSwitch}, Path=IsToggled}"
                     Value="true">
            <Setter Property="FontAttributes"
                    Value="Italic, Bold" />
            <Setter Property="FontSize"
                    Value="Large" />
        </DataTrigger>
    </Label.Triggers>
</Label>
```

この例では、は、 [`Label`](xref:Xamarin.Forms.Label) でバインド式を使用して `DataTrigger` `IsToggled` 、という名前ののプロパティを監視し `Switch` `styleSwitch` ます。 このプロパティがになると、 `true` `FontAttributes` `FontSize` のプロパティとプロパティ `Label` が変更されます。 プロパティが `IsToggled` に戻ると、 `false` `FontAttributes` `FontSize` のプロパティとプロパティ `Label` が初期状態にリセットされます。

トリガーの詳細については、「 [ Xamarin.Forms トリガー](~/xamarin-forms/app-fundamentals/triggers.md)」を参照してください。

## <a name="switch-visual-states"></a>ビジュアル状態の切り替え

[`Switch`](xref:Xamarin.Forms.Switch)`On`プロパティが `Off` 変更されたときにビジュアルの変更を開始するために使用できるおよびの状態があり [`IsToggled`](xref:Xamarin.Forms.Switch.IsToggled) ます。

次の XAML の例は、との状態の表示状態を定義する方法を示してい `On` `Off` ます。

```xaml
<Switch IsToggled="True">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="On">
                <VisualState.Setters>
                    <Setter Property="ThumbColor"
                            Value="MediumSpringGreen" />
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="Off">
                <VisualState.Setters>
                    <Setter Property="ThumbColor"
                            Value="Red" />
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Switch>
```

この例では、 `On` [`VisualState`](xref:Xamarin.Forms.VisualState) プロパティがの場合、 [`IsToggled`](xref:Xamarin.Forms.Switch.IsToggled) `true` プロパティは `ThumbColor` 中 spring green に設定されることを指定しています。 `Off` `VisualState` `IsToggled` プロパティがの場合 `false` 、プロパティは赤に設定されることを指定し `ThumbColor` ます。 したがって、全体の効果として、が off の位置にある場合は `Switch` thumb が赤色になり、が on の位置にある場合はその thumb がミディアムスプリンググリーンになり `Switch` ます。

![IOS と Android](switch-images/on-visualstate.png "VisualState の切り替え") 
 での Visualstate の切り替えのスクリーンショット![IOS と Android でのスイッチオフ VisualState のスクリーンショット](switch-images/off-visualstate.png "VisualState の切り替え")

ビジュアルの状態の詳細については、「[Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)」をご覧ください。

## <a name="disable-a-switch"></a>スイッチを無効にする

アプリケーションは、切り替え対象が有効な操作ではない状態になる場合があり `Switch` ます。 このような場合は、 `Switch` プロパティをに設定することで、を無効にすることができ `IsEnabled` `false` ます。 これにより、ユーザーがを操作できなくなり `Switch` ます。

## <a name="related-links"></a>関連リンク

- [デモの切り替え](/samples/xamarin/xamarin-forms-samples/userinterface-switchdemos/)
- [Xamarin.Forms のトリガー](~/xamarin-forms/app-fundamentals/triggers.md)
- [Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)