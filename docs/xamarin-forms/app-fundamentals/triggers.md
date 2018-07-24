---
title: Xamarin.Forms のトリガー
description: この記事では、Xamarin.Forms のトリガーを使用して、XAML でのユーザー インターフェイスの変更に応答する方法について説明します。 トリガーを使用すると、イベントまたはプロパティの変更に基づいてコントロールの外観を変更する操作を XAML で宣言できます。
ms.prod: xamarin
ms.assetid: 60460F57-63C6-4916-BBB5-A870F1DF53D7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/01/2016
ms.openlocfilehash: 954a0967e034e0321964e12ca0725ae2a85e3bc6
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995538"
---
# <a name="xamarinforms-triggers"></a>Xamarin.Forms のトリガー

トリガーを使用すると、イベントまたはプロパティの変更に基づいてコントロールの外観を変更する操作を XAML で宣言できます。

トリガーをコントロールに直接を割り当てたり、複数のコントロールに適用されるページごとまたはアプリケーション レベルのリソース ディクショナリに追加できます。

トリガーの 4 つの種類があります。

* [プロパティ トリガー](#property) -コントロールのプロパティが特定の値に設定されている場合に発生します。

* [データ トリガー](#data) - 別のコントロールのプロパティに基づくトリガーへのバインドのデータを使用します。

* [イベント トリガー](#event) -コントロールのイベントが発生したときに発生します。

* [複数のトリガー](#multi) -により、複数のトリガー条件、アクションが発生する前に設定します。

<a name="property" />

## <a name="property-triggers"></a>プロパティ トリガー

XAML、純粋に表現できる簡単なトリガーを追加、`Trigger`要素をコントロールのコレクションをトリガーします。
この例を変更するトリガーを`Entry`フォーカスを受け取ると、背景色。

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

* **TargetType** -トリガーを適用するコントロールの種類。

* **プロパティ**-監視されているコントロールのプロパティ。

* **値**-の値、監視対象のプロパティの発生時に、トリガーをアクティブ化の原因となります。

* **Set アクセス操作子**-一連の`Setter`要素を追加して、トリガー条件が満たされたとき。 指定する必要があります、`Property`と`Value`を設定します。

* **EnterActions と ExitActions** (非表示) - コードで記述されに加え (または instead of) に使用できる`Setter`要素。 [以下に示す](#enterexit)します。

### <a name="applying-a-trigger-using-a-style"></a>スタイルを使用してトリガーを適用します。

トリガーを追加することも、`Style`ページ、またはアプリケーションで、コントロールの宣言`ResourceDictionary`します。 この例は、暗黙的なスタイルを宣言します (ie。 ありません`Key`設定されている) すべてに適用することを意味する`Entry`ページ上のコントロール。

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

データ トリガーが発生する別のコントロールを監視するデータ バインドを使用して、`Setter`に呼び出されます。 代わりに、`Property`プロパティ トリガー属性は、設定、`Binding`属性を指定した値を監視します。

次の例は、データ バインディング構文を使用して`{Binding Source={x:Reference entry}, Path=Text.Length}`はどのように別のコントロールのプロパティを参照してください。 ときの長さ、 `entry` 0 の場合は、トリガーがアクティブ化します。 このサンプルでは、トリガーは、入力が空の場合に、ボタンを無効にします。

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

ヒント: 評価するときに`Path=Text.Length`(例: ターゲット プロパティの既定値を常に提供 `Text=""`) ので、そうでなければ`null`トリガーが予定どおりに動作しません。

指定するだけでなく`Setter`s が提供することも[`EnterActions`と`ExitActions`](#enterexit)します。

<a name="event" />

## <a name="event-triggers"></a>Event Triggers

`EventTrigger`要素にのみが必要です、`Event`プロパティなど`"Clicked"`で次の例。

```xaml
<EventTrigger Event="Clicked">
    <local:NumericValidationTriggerAction />
</EventTrigger>
```

あることに注意してくださいありません`Setter`要素がによって定義されたクラスへの参照ではなく`local:NumericValidationTriggerAction`必要があります、`xmlns:local`の XAML のページで宣言します。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:WorkingWithTriggers;assembly=WorkingWithTriggers"
```

クラス自体を実装して`TriggerAction`つまりのオーバーライドを提供する必要があります、`Invoke`トリガー イベントが発生するたびに呼び出されるメソッド。

トリガー アクションの実装が必要です。

* ジェネリック実装`TriggerAction<T>`クラスは、トリガーに適用されるコントロールの種類に対応するジェネリック パラメーターを使用します。 スーパークラスをなど、使用できる`VisualElement`さまざまなコントロールを使用したり、コントロールの種類を指定するトリガー アクションを記述する`Entry`します。

* 上書き、`Invoke`トリガーの条件が満たされるたびにこの呼び出されるメソッド。

* 必要に応じて、トリガーが宣言されている場合は、XAML で設定できるプロパティを公開 (など`Anchor`、 `Scale`、および`Length`この例では)。

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

トリガーの動作によって公開されるプロパティは、XAML の宣言で設定することができます。

```xaml
<EventTrigger Event="TextChanged">
    <local:NumericValidationTriggerAction />
</EventTrigger>
```

内のトリガーを共有する場合は注意を`ResourceDictionary`コントロール間での 1 つのインスタンスの共有はため、1 回構成されている任意の状態は、それらすべてに適用されます。

イベント トリガーがサポートしていないことに注意してください。`EnterActions`と`ExitActions`[以下に示す](#enterexit)します。    

<a name="multi" />

## <a name="multi-triggers"></a>複数のトリガー

A`MultiTrigger`次のような`Trigger`または`DataTrigger`点を除いては、複数の条件があります。 すべての条件は、前に満たす必要があります、 `Setter`s がトリガーされます。

2 つの異なる入力にバインドするボタンのトリガーの例を次に示します (`email`と`phone`)。

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

`Conditions`コレクションを含めたりも`PropertyCondition`要素には、このような。

```xaml
<PropertyCondition Property="Text" Value="OK" />
```

### <a name="building-a-require-all-multi-trigger"></a>「すべてが必要です」マルチ トリガーの作成

複数のトリガーは、すべての条件に該当する場合のみ、コントロールを更新します。 必要条件があるため注意が必要ですが (すべての入力が完了する必要があります、ログイン ページ) などの「すべてのフィールドの長さが 0」テスト"で Text.Length > 0" ですが、これは、XAML で表現することはできません。

これで、`IValueConverter`します。 変換の下のコンバーター コード、`Text.Length`にバインドを`bool`かフィールドが空かどうかを示します。


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

このコンバーターを使用して、複数のトリガーを最初に追加、ページのリソース ディクショナリ (カスタムと共に`xmlns:local`名前空間の定義)。

```xaml
<ResourceDictionary>
   <local:MultiTriggerConverter x:Key="dataHasBeenEntered" />
</ResourceDictionary>
```

XAML は、以下に示します。 最初の複数のトリガーの例から次の相違点に注意してください。

* ボタンには、`IsEnabled="false"`既定で設定します。
* 複数のトリガーの条件では、コンバーターを使用して、`Text.Length`にブール値。
* すべての条件の場合に`true`、setter は、ボタンの`IsEnabled`プロパティ`true`します。

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

これらのスクリーン ショットでは、上記 2 つの複数のトリガー例の違いを示します。 テキスト入力、画面の上部には、1 つだけで`Entry`が有効にするのに十分な**保存**ボタンをクリックします。
画面の下部に、**ログイン**まで、両方のフィールドがデータを含むボタンを非アクティブなままです。


![](triggers-images/multi-requireall.png "MultiTrigger 例")

<a name="enterexit" />

## <a name="enteractions-and-exitactions"></a>EnterActions と ExitActions

追加することで、トリガーが発生したときに、変更を実装する別の方法は、`EnterActions`と`ExitActions`コレクションと指定する`TriggerAction<T>`実装します。

行うことができます*両方*`EnterActions`と`ExitActions`だけでなく`Setter`、トリガーでがありますに注意してくださいを`Setter`s はすぐと呼ばれます (を待たず、`EnterAction`または`ExitAction`に完了)。 または、コード内のすべてを実行し、使用しない`Setter`を同時にすべて。

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

クラスが XAML で参照されているときに、常に、名前空間をなど、宣言する必要があります`xmlns:local`次のようにします。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:WorkingWithTriggers;assembly=WorkingWithTriggers"
```

`FadeTriggerAction`コードを次に示します。

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

注:`EnterActions`と`ExitActions`では無視されます**イベント トリガー**します。



## <a name="related-links"></a>関連リンク

- [トリガーのサンプル](https://developer.xamarin.com/samples/WorkingWithTriggers)
- [Xamarin.Forms API ドキュメント](xref:Xamarin.Forms.TriggerAction`1)
