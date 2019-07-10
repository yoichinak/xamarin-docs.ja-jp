---
title: Xamarin.Forms のスイッチ
description: Xamarin.Forms のスイッチには、オンとオフの状態を切り替えるユーザーによって操作できるボタンの一種です。 この記事では、スイッチのクラスを使用して、切り替えの UI 要素を表示する方法について説明します。
ms.prod: xamarin
ms.assetId: B2F9CC65-481B-4323-8E77-C6BE29C90DE9
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 07/03/2019
ms.openlocfilehash: 22a17f9a916d94a3a0f44a451512de43c943e95a
ms.sourcegitcommit: 58d8bbc19ead3eb535fb8248710d93ba0892e05d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/09/2019
ms.locfileid: "67675040"
---
# <a name="xamarinforms-switch"></a>Xamarin.Forms のスイッチ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/SwitchDemos)

Xamarin.Forms [ `Switch` ](xref:Xamarin.Forms.Switch)水平方向のトグル ボタンを切り替えるオンとオフの状態によって表される、ユーザーが操作できるは、`boolean`値。 `Switch`クラスから継承[ `View`](xref:Xamarin.Forms.View)します。

次のスクリーン ショット、`Switch`を制御、**で**と**オフ**iOS と Android での状態を切り替えます。

![スクリーン ショットのスイッチのオン/オフ状態、iOS と Android で](switch-images/switch-states-default.png "iOS および Android でスイッチ")

`Switch`コントロールは、2 つのプロパティを定義します。

* [`OnColor`](xref:Xamarin.Forms.Switch.OnColor) `Color`に影響する方法、`Switch`が切り替えられてでレンダリングまたは**で**、状態。
* [`IsToggled`](xref:Xamarin.Forms.Switch.IsToggled) `boolean`を示す値かどうか、`Switch`は**で**します。

これらのプロパティが支え、 [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty)オブジェクト、つまり、`Switch`スタイルを設定できるし、データ バインディングのターゲットにします。

`Switch`コントロールを定義、`Toggled`ときに発生するイベント、`IsToggled`プロパティの変更、ユーザー操作によって、またはアプリケーションを設定すると、`IsToggled`プロパティ。 `ToggledEventArgs`に付属しているオブジェクト、`Toggled`イベントという名前の 1 つのプロパティには、 `Value`、型の`bool`します。 ときにイベントが発生しての値、`Value`プロパティの新しい値を反映する、`IsToggled`プロパティ。

## <a name="create-a-switch"></a>スイッチを作成します。

A `Switch` XAML でインスタンス化することができます。 その`IsToggled`を切り替えるプロパティを設定することができます、`Switch`します。 既定で、`IsToggled`プロパティは`false`します。 次の例では、インスタンス化する方法を示しています、 `Switch` XAML で、オプションで`IsToggled`プロパティ セット。

```xaml
<Switch IsToggled="true"/>
```

A`Switch`コードで作成することもできます。

```csharp
Switch switch = new Switch { IsToggled = true };
```

### <a name="switch-style-properties"></a>スタイル プロパティを切り替える

`OnColor`プロパティの設定を定義することができます、`Switch`色 に切り替えるときにその**で**状態。 次の例では、インスタンス化する方法を示しています、`Switch`を XAML で、`OnColor`プロパティ セット。

```xaml
<Switch OnColor="Orange" />
```

`OnColor`作成するときに、プロパティを設定することも、`Switch`コード。

```csharp
Switch switch = new Switch { OnColor = Color.Orange };
```

次のスクリーン ショット、`Switch`でその**で**と**オフ**のオン/オフ状態、`OnColor`プロパティに設定`Color.Orange`iOS と Android で。

![スクリーン ショットのスイッチのオン/オフ状態、iOS と Android で](switch-images/switch-states-oncolor.png "iOS および Android でスイッチ")

## <a name="respond-to-a-switch-state-change"></a>スイッチの状態の変更に対応します。

ときに、`IsToggled`プロパティの変更、ユーザー操作によって、またはアプリケーションを設定すると、`IsToggled`プロパティ、`Toggled`イベントが発生します。 このイベントのイベント ハンドラーは、その変更に対応する登録できます。

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

`sender`イベント ハンドラーの引数は、`Switch`このイベントを発生させる責任を負います。 使用することができます、`sender`プロパティにアクセスする、`Switch`オブジェクト、または複数を区別する`Switch`オブジェクトが同じ共有`Toggled`イベント ハンドラー。

`Toggled`イベント ハンドラーがコードに割り当てることもできます。

```csharp
Switch switch = new Switch {...};
switch.Toggled += (sender, e) =>
{
    // Perform an action after examining e.Value
}
```

## <a name="data-bind-a-switch"></a>データをバインドするスイッチ

`Toggled`に対応するデータのバインドとトリガーを使用してイベント ハンドラーを取り除くことができます、`Switch`トグル状態を変更します。

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

この例で、 [ `Label` ](xref:Xamarin.Forms.Label)内のバインディング式を使用して、`DataTrigger`を監視する、`IsToggled`のプロパティ、`Switch`という`styleSwitch`。 このプロパティになった`true`、`FontAttributes`と`FontSize`のプロパティ、`Label`変更されます。 ときに、`IsToggled`プロパティを返します`false`、`FontAttributes`と`FontSize`のプロパティ、`Label`を初期状態にリセットされます。

トリガーについては、次を参照してください。 [Xamarin.Forms トリガー](~/xamarin-forms/app-fundamentals/triggers.md)します。

## <a name="disable-a-switch"></a>スイッチを無効にします。

状態を入力できるは、アプリケーションで、`Switch`化が有効な操作ではありません。 このような場合は、`Switch`設定で無効にすることができます、`IsEnabled`プロパティを`false`します。 これにより、ユーザーが操作をできないように、`Switch`します。

## <a name="related-links"></a>関連リンク

* [スイッチのデモ](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/SwitchDemos)
* [Xamarin.Forms のトリガー](~/xamarin-forms/app-fundamentals/triggers.md)
