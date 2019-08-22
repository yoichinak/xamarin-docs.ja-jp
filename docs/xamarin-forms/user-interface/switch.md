---
title: Xamarin. フォームスイッチ
description: Xamarin. フォームスイッチは、ユーザーが状態のオンとオフを切り替えるために操作できるボタンの種類です。 この記事では、Switch クラスを使用して、切り替え UI 要素を表示する方法について説明します。
ms.prod: xamarin
ms.assetId: B2F9CC65-481B-4323-8E77-C6BE29C90DE9
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 07/18/2019
ms.openlocfilehash: 1f2ef838287e32df5df42f73e4b43816d618552d
ms.sourcegitcommit: 5f972a757030a1f17f99177127b4b853816a1173
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/21/2019
ms.locfileid: "69887885"
---
# <a name="xamarinforms-switch"></a>Xamarin. フォームスイッチ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-switchdemos/)

Xamarin [`Switch`](xref:Xamarin.Forms.Switch)コントロールは水平方向のトグルボタンで、ユーザーは`boolean`値で表されるオンとオフの状態を切り替えることができます。 クラス`Switch`は、から[`View`](xref:Xamarin.Forms.View)継承されます。

次のスクリーンショットは`Switch` 、iOS と Android での**オン**と**オフ**の切り替え状態のコントロールを示しています。

![IOS と Android でのオンとオフの状態の切り替えのスクリーンショット](switch-images/switch-states-default.png "IOS と Android でのスイッチ")

コントロール`Switch`は、次の2つのプロパティを定義します。

* [`IsToggled`](xref:Xamarin.Forms.Switch.IsToggled)が**オンかどう**か`Switch`を示す値です。 `boolean`
* [`OnColor`](xref:Xamarin.Forms.Switch.OnColor)は、が切り替え`Switch`られるか、状態で表示されるかに影響するです。`Color`
* `ThumbColor`は、 `Color`スイッチのつまみのです。

これらのプロパティは[`BindableProperty`](xref:Xamarin.Forms.BindableProperty)オブジェクトによって支えられています。つまり、 `Switch`をスタイル設定し、データバインディングのターゲットにすることができます。

コントロール`Switch`は、ユーザー `Toggled`操作を使用するか、 `IsToggled`アプリケーションがプロパティを`IsToggled`設定するときに、プロパティが変更されたときに発生するイベントを定義します。 イベントに`Value` `bool`付随する`ToggledEventArgs`オブジェクトには、型のという名前のプロパティが1つあります。 `Toggled` イベントが発生すると、プロパティの値`Value`に`IsToggled`プロパティの新しい値が反映されます。

## <a name="create-a-switch"></a>スイッチを作成する

は`Switch` 、XAML でインスタンス化できます。 このプロパティを設定すると、 `Switch`を切り替えることができます。 `IsToggled` 既定では、 `IsToggled`プロパティは`false`です。 次の例は、省略可能`Switch` `IsToggled`なプロパティセットを使用して、XAML でをインスタンス化する方法を示しています。

```xaml
<Switch IsToggled="true"/>
```

は`Switch` 、コードで作成することもできます。

```csharp
Switch switchControl = new Switch { IsToggled = true };
```

## <a name="switch-appearance"></a>外観の切り替え

[`Switch`](xref:Xamarin.Forms.Switch) `Switch` `OnColor` `ThumbColor`クラスから継承されるプロパティに加えて、はプロパティとプロパティも定義します。 [`View`](xref:Xamarin.Forms.View) `Color` `Switch` `ThumbColor`プロパティを設定して、状態がオンの状態に切り替わるときの色を定義できます。また、プロパティを設定して、スイッチのつまみのを定義できます。 `OnColor` 次の例は、これらのプロパティ`Switch`を設定して、XAML でをインスタンス化する方法を示しています。

```xaml
<Switch OnColor="Orange"
        ThumbColor="Green" />
```

プロパティは、コードでを`Switch`作成するときに設定することもできます。

```csharp
Switch switch = new Switch { OnColor = Color.Orange, ThumbColor = Color.Green };
```

次のスクリーンショットは`Switch` 、プロパティ`OnColor`と`ThumbColor`プロパティが設定されている、**オン**と**オフ**の切り替え状態のを示しています。

![IOS と Android でのオンとオフの状態の切り替えのスクリーンショット](switch-images/switch-states-colors.png "IOS と Android でのスイッチ")

## <a name="respond-to-a-switch-state-change"></a>スイッチの状態の変更に応答する

ユーザー操作`IsToggled`によって、またはアプリケーションが`IsToggled`プロパティを設定するときに、プロパティ`Toggled`が変更されると、イベントが発生します。 このイベントのイベントハンドラーは、変更に応答するように登録できます。

```xaml
<Switch Toggled="OnToggled" />
```

分離コード ファイルにはハンドラーが含まれています、`Toggled`イベント。

```csharp
void OnToggled(object sender, ToggledEventArgs e)
{
    // Perform an action after examining e.Value
}
```

イベントハンドラーの`Switch` 引数は、このイベントの発生を`sender`担当するです。 `sender`プロパティを使用して`Switch`オブジェクトにアクセスしたり、同じ`Toggled`イベントハンドラーを`Switch`共有する複数のオブジェクトを区別したりできます。

イベント`Toggled`ハンドラーは、次のコードでも割り当てることができます。

```csharp
Switch switchControl = new Switch {...};
switchControl.Toggled += (sender, e) =>
{
    // Perform an action after examining e.Value
}
```

## <a name="data-bind-a-switch"></a>スイッチのデータバインド

イベントハンドラーを削除するには、データバインディングとトリガーを使用して`Switch` 、変化するトグル状態に応答します。 `Toggled`

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

この[`Label`](xref:Xamarin.Forms.Label)例では、は、 `DataTrigger`でバインド式を使用して`IsToggled` 、 `Switch`という名前`styleSwitch`ののプロパティを監視します。 このプロパティ`true` `Label`がになる`FontAttributes`と、 `FontSize`のプロパティとプロパティが変更されます。 プロパティが`IsToggled` `false`に戻ると`FontSize` 、のプロパティ`Label`とプロパティが初期状態にリセットさ`FontAttributes`れます。

トリガーの詳細については、「 [Xamarin. Forms triggers](~/xamarin-forms/app-fundamentals/triggers.md)」を参照してください。

## <a name="disable-a-switch"></a>スイッチを無効にする

アプリケーションは、 `Switch`切り替え対象が有効な操作ではない状態になる場合があります。 このような場合は`Switch` 、 `IsEnabled`プロパティをに設定する`false`ことで、を無効にすることができます。 これにより、 `Switch`ユーザーがを操作できなくなります。

## <a name="related-links"></a>関連リンク

* [デモの切り替え](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-switchdemos/)
* [Xamarin. フォームトリガー](~/xamarin-forms/app-fundamentals/triggers.md)
