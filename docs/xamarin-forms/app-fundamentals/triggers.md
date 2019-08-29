---
title: Xamarin.Forms のトリガー
description: この記事では、Xamarin.Forms のトリガーを使用して、XAML でのユーザー インターフェイスの変更に応答する方法について説明します。 トリガーを使用すると、イベントまたはプロパティの変更に基づいてコントロールの外観を変更するアクションを XAML での宣言として表すことができます。
ms.prod: xamarin
ms.assetid: 60460F57-63C6-4916-BBB5-A870F1DF53D7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/01/2016
ms.openlocfilehash: d9e3055130a66fe240bf378ad2f63679e71bec14
ms.sourcegitcommit: 1dd7d09b60fcb1bf15ba54831ed3dd46aa5240cb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/28/2019
ms.locfileid: "70121140"
---
# <a name="xamarinforms-triggers"></a>Xamarin.Forms のトリガー

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithtriggers)

トリガーを使用すると、イベントまたはプロパティの変更に基づいてコントロールの外観を変更するアクションを XAML での宣言として表すことができます。

トリガーは、コントロールに直接割り当てることも、ページ レベルまたはアプリケーション レベルのリソース ディクショナリに追加して複数のコントロールに適用することもできます。

4 種類のトリガーがあります。

- [プロパティ トリガー](#property) -コントロールのプロパティが特定の値に設定されると発生します。

- [データ トリガー](#data) - データ バインディングを使用し、別のコントロールのプロパティに基づいてトリガーします。

- [イベント トリガー](#event) -コントロールでイベントが発生すると発生します。

- [マルチ トリガー](#multi) - アクションが発生する前に、複数のトリガー条件を設定できます。

<a name="property" />

## <a name="property-triggers"></a>プロパティ トリガー

コントロールのトリガー コレクションに `Trigger` 要素を追加して、XAML 内だけでシンプルなトリガーを表現できます。
次の例では、フォーカスが設定されると背景色を `Entry` に変更するトリガーを示します。

```xaml
<Entry Placeholder="enter name">
    <Entry.Triggers>
        <Trigger TargetType="Entry"
             Property="IsFocused" Value="True">
            <Setter Property="BackgroundColor" Value="Yellow" />
        </Trigger>
    </Entry.Triggers>
</Entry>
```

トリガーの宣言の重要な部分は次のとおりです。

- **TargetType** - トリガーを適用するコントロールの種類です。

- **Property** - コントロール上で監視するプロパティです。

- **Value** - 監視対象のプロパティがこの値になったら、トリガーをアクティブにします。

- **Setter** - トリガー条件が満たされたときの `Setter` 要素のコレクションを追加できます。 設定するには、`Property` と `Value` を指定する必要があります。

- **EnterActions と ExitActions** (示されていません) - コードで記述され、`Setter` 要素に加えて (または代えて) 使用できます。 これらについては、[後で説明します](#enterexit)。

### <a name="applying-a-trigger-using-a-style"></a>スタイルを使用してトリガーを適用する

トリガーは、コントロール、ページ、またはアプリケーションの `ResourceDictionary` の `Style` 宣言に追加することもできます。 次の例では、暗黙のスタイルを宣言しています (つまり、`Key` が設定されていません)。これは、ページ上のすべての `Entry` コントロールに適用されることを意味します。

```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        <Style TargetType="Entry">
                        <Style.Triggers>
                <Trigger TargetType="Entry"
                         Property="IsFocused" Value="True">
                    <Setter Property="BackgroundColor" Value="Yellow" />
                </Trigger>
            </Style.Triggers>
        </Style>
    </ResourceDictionary>
</ContentPage.Resources>
```

<a name="data" />

## <a name="data-triggers"></a>データ トリガー

データ トリガーでは、データ バインディングを使用して別のコントロールが監視され、`Setter` が呼び出されます。 プロパティ トリガーでの `Property` 属性の代わりに、指定した値を監視する `Binding` 属性を設定します。

次の例では、データ バインディング構文 `{Binding Source={x:Reference entry}, Path=Text.Length}` を使用しています
これは、別のコントロールのプロパティを参照する方法です。 `entry` の長さが 0 の場合、トリガーがアクティブにされます。 このサンプルでは、入力が空の場合、トリガーでボタンが無効にされます。

```xaml
<!-- the x:Name is referenced below in DataTrigger-->
<!-- tip: make sure to set the Text="" (or some other default) -->
<Entry x:Name="entry"
       Text=""
       Placeholder="required field" />

<Button x:Name="button" Text="Save"
        FontSize="Large"
        HorizontalOptions="Center">
    <Button.Triggers>
        <DataTrigger TargetType="Button"
                     Binding="{Binding Source={x:Reference entry},
                                       Path=Text.Length}"
                     Value="0">
            <Setter Property="IsEnabled" Value="False" />
        </DataTrigger>
    </Button.Triggers>
</Button>
```

> [!TIP]
> `Path=Text.Length` を評価するときは常に、ターゲット プロパティの既定値が提供されます (例: `Text=""`)。そうしないと `null` になって、トリガーが意図したとおりに動作しないためです。

`Setter` を指定するだけでなく、[`EnterActions` と `ExitActions`](#enterexit) を提供することもできます。

<a name="event" />

## <a name="event-triggers"></a>Event Triggers

次の例での `"Clicked"` のように、`EventTrigger` 要素では `Event` プロパティのみが必要です。

```xaml
<EventTrigger Event="Clicked">
    <local:NumericValidationTriggerAction />
</EventTrigger>
```

`Setter` 要素はなく、代わりにページの XAML で `xmlns:local` が宣言されている必要のある `local:NumericValidationTriggerAction` によって定義されたクラスへの参照があることに注意してください。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:WorkingWithTriggers;assembly=WorkingWithTriggers"
```

クラス自体では `TriggerAction` が実装されています。これは、トリガー イベントが発生するたびに呼び出される `Invoke` メソッドに対するオーバーライドを提供する必要があることを意味します。

トリガー アクションは次のように実装する必要があります。

- トリガーの適用対象であるコントロールの種類に対応するジェネリック パラメーターを使用してジェネリック `TriggerAction<T>` クラスを実装します。 `VisualElement` のようなスーパークラスを使用してさまざまなコントロールで動作するトリガーを記述したり、`Entry` のようなコントロールの種類を指定したりできます。

- `Invoke` メソッドをオーバーライドします。これは、トリガー条件が満たされるたびに呼び出されます。

- 必要に応じて、トリガーを宣言するときに XAML で設定できるプロパティを公開します (次の例の `Anchor`、`Scale`、`Length` など)。

```csharp
public class NumericValidationTriggerAction : TriggerAction<Entry>
{
    protected override void Invoke (Entry entry)
    {
        double result;
        bool isValid = Double.TryParse (entry.Text, out result);
        entry.TextColor = isValid ? Color.Default : Color.Red;
    }
}
```

トリガー アクションによって公開されるプロパティは、次のように XAML の宣言で設定できます。

```xaml
<EventTrigger Event="TextChanged">
    <local:NumericValidationTriggerAction />
</EventTrigger>
```

`ResourceDictionary` でトリガーを共有する場合は注意が必要です。コントロール間で 1 つのインスタンスが共有されるので、1 つで構成された状態がすべてに適用されます。

イベント トリガーでは、[後で説明する](#enterexit) `EnterActions` と `ExitActions` がサポートされていないことに注意してください。

<a name="multi" />

## <a name="multi-triggers"></a>マルチ トリガー

`MultiTrigger` は `Trigger` や `DataTrigger` と似ていますが、複数の条件を使用できる点が異なります。 `Setter` がトリガーされるには、すべての条件が満たされる必要があります。

2 つの異なる入力 (`email` と `phone`) にバインドされているボタンに対するトリガーの例を次に示します。

```xaml
<MultiTrigger TargetType="Button">
    <MultiTrigger.Conditions>
        <BindingCondition Binding="{Binding Source={x:Reference email},
                                   Path=Text.Length}"
                               Value="0" />
        <BindingCondition Binding="{Binding Source={x:Reference phone},
                                   Path=Text.Length}"
                               Value="0" />
    </MultiTrigger.Conditions>

  <Setter Property="IsEnabled" Value="False" />
    <!-- multiple Setter elements are allowed -->
</MultiTrigger>
```

`Conditions` コレクションには、次のように `PropertyCondition` 要素を含めることもできます。

```xaml
<PropertyCondition Property="Text" Value="OK" />
```

### <a name="building-a-require-all-multi-trigger"></a>"すべて必要" マルチ トリガーの作成

マルチ トリガーでは、すべての条件が真のときにのみコントロールが更新されます。 "すべてのフィールドの長さが 0" であることをテストするのは (すべての入力が完了する必要のあるログイン ページなど)、簡単ではありません。"Text.Length > 0 の場合" という条件が必要ですが、XAML でこれを表現することができないためです。

これは、`IValueConverter` を使用して行うことができます。 次のコンバーター コードでは、`Text.Length` のバインドが、フィールドが空かどうかを示す `bool` に変換されています。

```csharp
public class MultiTriggerConverter : IValueConverter
{
    public object Convert(object value, Type targetType,
        object parameter, CultureInfo culture)
    {
        if ((int)value > 0) // length > 0 ?
            return true;            // some data has been entered
        else
            return false;            // input is empty
    }

    public object ConvertBack(object value, Type targetType,
        object parameter, CultureInfo culture)
    {
        throw new NotSupportedException ();
    }
}
```

このコンバーターをマルチ トリガーで使用するには、最初に、ページのリソース ディクショナリに追加します (カスタム `xmlns:local` 名前空間の定義と共に)。

```xaml
<ResourceDictionary>
   <local:MultiTriggerConverter x:Key="dataHasBeenEntered" />
</ResourceDictionary>
```

XAML を以下に示します。 最初のマルチ トリガーの例と次の点が異なることに注意してください。

- ボタンには、`IsEnabled="false"` が既定で設定されます。
- マルチ トリガーの条件では、コンバーターを使用して `Text.Length` の値が `boolean` に変換されます。
- すべての条件が `true` の場合、セッターはボタンの `IsEnabled` プロパティを `true` にします。

```xaml
<Entry x:Name="user" Text="" Placeholder="user name" />

<Entry x:Name="pwd" Text="" Placeholder="password" />

<Button x:Name="loginButton" Text="Login"
        FontSize="Large"
        HorizontalOptions="Center"
        IsEnabled="false">
  <Button.Triggers>
    <MultiTrigger TargetType="Button">
      <MultiTrigger.Conditions>
        <BindingCondition Binding="{Binding Source={x:Reference user},
                              Path=Text.Length,
                              Converter={StaticResource dataHasBeenEntered}}"
                          Value="true" />
        <BindingCondition Binding="{Binding Source={x:Reference pwd},
                              Path=Text.Length,
                              Converter={StaticResource dataHasBeenEntered}}"
                          Value="true" />
      </MultiTrigger.Conditions>
      <Setter Property="IsEnabled" Value="True" />
    </MultiTrigger>
  </Button.Triggers>
</Button>
```

次のスクリーンショットは、上記の 2 つのマルチ トリガーの例の違いを示したものです。 画面の上部では、 **[Save]** ボタンを有効にするには、1 つの `Entry` へのテキスト入力だけで十分です。
画面の下部では、両方のフィールドにデータを入力するまで、 **[Login]** ボタンはアクティブになりません。

![](triggers-images/multi-requireall.png "マルチ トリガーの例")

<a name="enterexit" />

## <a name="enteractions-and-exitactions"></a>EnterActions と ExitActions

トリガーが発生したときの変更を実装するもう 1 つの方法は、`EnterActions` および `ExitActions` コレクションを追加して、`TriggerAction<T>` の実装を指定することです。

トリガーで `Setter` と共に `EnterActions` と `ExitActions` の "*両方*" を提供できますが、`Setter` はすぐに呼び出されることに注意してください (`EnterAction` または `ExitAction` が完了するのを待機しません)。 代わりに、コードですべてを実行し、`Setter` をまったく使用しないこともできます。

```xaml
<Entry Placeholder="enter job title">
    <Entry.Triggers>
        <Trigger TargetType="Entry"
                 Property="Entry.IsFocused" Value="True">
            <Trigger.EnterActions>
                <local:FadeTriggerAction StartsFrom="0"" />
            </Trigger.EnterActions>

            <Trigger.ExitActions>
                <local:FadeTriggerAction StartsFrom="1" />
            </Trigger.ExitActions>
                        <!-- You can use both Enter/Exit and Setter together if required -->
        </Trigger>
    </Entry.Triggers>
</Entry>
```

例のごとく、XAML でクラスを参照するときは、次のように `xmlns:local` などの名前空間を宣言する必要があります。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:WorkingWithTriggers;assembly=WorkingWithTriggers"
```

`FadeTriggerAction` のコードを次に示します。

```csharp
public class FadeTriggerAction : TriggerAction<VisualElement>
{
    public FadeTriggerAction() {}

    public int StartsFrom { set; get; }

    protected override void Invoke (VisualElement visual)
    {
            visual.Animate("", new Animation( (d)=>{
                var val = StartsFrom==1 ? d : 1-d;
                visual.BackgroundColor = Color.FromRgb(1, val, 1);

            }),
            length:1000, // milliseconds
            easing: Easing.Linear);
    }
}
```

注: `EnterActions` と `ExitActions` は、**イベント トリガー**では無視されます。



## <a name="related-links"></a>関連リンク

- [トリガーのサンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithtriggers)
- [Xamarin.Forms API ドキュメント](xref:Xamarin.Forms.TriggerAction`1)
