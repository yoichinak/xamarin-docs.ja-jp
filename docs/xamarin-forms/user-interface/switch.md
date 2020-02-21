---
title: Xamarin. フォームスイッチ
description: Xamarin. フォームスイッチは、ユーザーが状態のオンとオフを切り替えるために操作できるボタンの種類です。 この記事では、Switch クラスを使用して、切り替え UI 要素を表示する方法について説明します。
ms.prod: xamarin
ms.assetId: B2F9CC65-481B-4323-8E77-C6BE29C90DE9
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 07/18/2019
ms.openlocfilehash: 88655aabdbd32db63aaf3330a18b0ad8105ea26c
ms.sourcegitcommit: b751605179bef8eee2df92cb484011a7dceb6fda
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/20/2020
ms.locfileid: "77506538"
---
# <a name="xamarinforms-switch"></a>Xamarin. フォームスイッチ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-switchdemos/)

Xamarin [`Switch`](xref:Xamarin.Forms.Switch)コントロールは水平方向のトグルボタンであり、ユーザーはこのボタンを使用して、`boolean` 値で表されるオンとオフの状態を切り替えることができます。 `Switch` クラスは[`View`](xref:Xamarin.Forms.View)から継承されます。

次のスクリーンショットは、iOS および Android での**オン**と**オフ**の切り替え状態の `Switch` コントロールを示しています。

![IOS と Android でのオンとオフの状態の切り替えのスクリーンショット](switch-images/switch-states-default.png "IOS と Android でのスイッチ")

`Switch` コントロールは、次のプロパティを定義します。

* [`IsToggled`](xref:Xamarin.Forms.Switch.IsToggled)は、`Switch` が**オン**かどうかを示す `boolean` 値です。
* [`OnColor`](xref:Xamarin.Forms.Switch.OnColor)は、切り替えられた**状態または状態での**`Switch` の表示方法に影響する `Color` です。
* `ThumbColor` は、スイッチのつまみの `Color` です。

これらのプロパティは、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)オブジェクトによって支えられています。つまり、`Switch` のスタイルを設定し、データバインディングのターゲットにすることができます。

`Switch` コントロールは、ユーザー操作によって、またはアプリケーションが `IsToggled` プロパティを設定するときに、`IsToggled` プロパティが変更されたときに発生する `Toggled` イベントを定義します。 `Toggled` イベントに付随する `ToggledEventArgs` オブジェクトには、`bool`型の `Value`という名前のプロパティが1つあります。 イベントが発生すると、`Value` プロパティの値に `IsToggled` プロパティの新しい値が反映されます。

## <a name="create-a-switch"></a>スイッチを作成する

`Switch` は、XAML でインスタンス化できます。 `IsToggled` プロパティを設定して、`Switch`を切り替えることができます。 既定では、`IsToggled` プロパティは `false`です。 次の例は、省略可能な `IsToggled` プロパティセットを使用して、XAML で `Switch` をインスタンス化する方法を示しています。

```xaml
<Switch IsToggled="true"/>
```

コードでは、`Switch` を作成することもできます。

```csharp
Switch switchControl = new Switch { IsToggled = true };
```

## <a name="switch-appearance"></a>外観の切り替え

[`View`](xref:Xamarin.Forms.View)クラスから継承[`Switch`](xref:Xamarin.Forms.Switch)プロパティに加えて、`Switch` `OnColor` および `ThumbColor` のプロパティも定義します。 `OnColor` プロパティを設定して、`Switch` 色を**オン**状態に切り替えたときの色を定義できます。また、`ThumbColor` プロパティを設定して、スイッチのつまみの `Color` を定義できます。 次の例は、これらのプロパティを設定して、XAML で `Switch` をインスタンス化する方法を示しています。

```xaml
<Switch OnColor="Orange"
        ThumbColor="Green" />
```

プロパティは、コードで `Switch` を作成するときに設定することもできます。

```csharp
Switch switch = new Switch { OnColor = Color.Orange, ThumbColor = Color.Green };
```

次のスクリーンショットは、`OnColor` と `ThumbColor` のプロパティが設定されている、**オン**と**オフ**の切り替え状態の `Switch` を示しています。

![IOS と Android でのオンとオフの状態の切り替えのスクリーンショット](switch-images/switch-states-colors.png "IOS と Android でのスイッチ")

## <a name="respond-to-a-switch-state-change"></a>スイッチの状態の変更に応答する

ユーザー操作によって、またはアプリケーションが `IsToggled` プロパティを設定したときに `IsToggled` プロパティが変更されると、`Toggled` イベントが発生します。 このイベントのイベントハンドラーは、変更に応答するように登録できます。

```xaml
<Switch Toggled="OnToggled" />
```

分離コードファイルには、`Toggled` イベントのハンドラーが含まれています。

```csharp
void OnToggled(object sender, ToggledEventArgs e)
{
    // Perform an action after examining e.Value
}
```

イベントハンドラーの `sender` 引数は、このイベントの発生を担当する `Switch` です。 `sender` プロパティを使用して、`Switch` オブジェクトにアクセスしたり、同じ `Toggled` イベントハンドラーを共有する複数の `Switch` オブジェクトを区別したりできます。

`Toggled` イベントハンドラーは、次のコードでも割り当てることができます。

```csharp
Switch switchControl = new Switch {...};
switchControl.Toggled += (sender, e) =>
{
    // Perform an action after examining e.Value
}
```

## <a name="data-bind-a-switch"></a>スイッチのデータバインド

`Toggled` イベントハンドラーを削除するには、データバインディングとトリガーを使用して、`Switch` 変化するトグル状態に応答します。

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

この例では、 [`Label`](xref:Xamarin.Forms.Label)は `DataTrigger` 内のバインド式を使用して、`styleSwitch`という名前の `Switch` の `IsToggled` プロパティを監視します。 このプロパティが `true`になると、`Label` の `FontAttributes` と `FontSize` のプロパティが変更されます。 `IsToggled` プロパティが `false`に戻ると、`Label` の `FontAttributes` および `FontSize` プロパティが初期状態にリセットされます。

トリガーの詳細については、「 [Xamarin. Forms triggers](~/xamarin-forms/app-fundamentals/triggers.md)」を参照してください。

## <a name="disable-a-switch"></a>スイッチを無効にする

アプリケーションは、切り替えられている `Switch` が有効な操作ではない状態になる可能性があります。 このような場合は、`IsEnabled` プロパティを `false`に設定することによって、`Switch` を無効にできます。 これにより、ユーザーが `Switch`を操作できなくなります。

## <a name="related-links"></a>関連リンク

* [デモの切り替え](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-switchdemos/)
* [Xamarin. フォームトリガー](~/xamarin-forms/app-fundamentals/triggers.md)
