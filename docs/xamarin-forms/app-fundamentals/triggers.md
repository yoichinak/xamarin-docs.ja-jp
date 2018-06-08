---
title: トリガー
description: Xaml ユーザー インターフェイスの変更に応答してください。
ms.prod: xamarin
ms.assetid: 60460F57-63C6-4916-BBB5-A870F1DF53D7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/01/2016
ms.openlocfilehash: af5912736e2a2bd7d3347d4aa199faa3fdfe41c7
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/07/2018
ms.locfileid: "34846446"
---
# <a name="triggers"></a>トリガー

トリガーを使用すると、イベントまたはプロパティの変更に基づいてコントロールの外観を変更する XAML の宣言によってアクションを表すことができます。

トリガーに直接割り当てるコントロール、または複数のコントロールに適用されるページごとまたはアプリケーション レベルのリソース ディクショナリに追加できます。

トリガーの 4 つの種類があります。

* [プロパティ トリガー](#property) -コントロールのプロパティが特定の値に設定されている場合に発生します。

* [データ トリガー](#data) - 使用へのデータ バインディングの別のコントロールのプロパティに基づいてトリガーします。

* [イベント トリガー](#event) -コントロールにイベントが発生したときに発生します。

* [複数のトリガー](#multi) -により、複数のトリガー条件、アクションが発生する前に設定します。

<a name="property" />

## <a name="property-triggers"></a>プロパティ トリガー

純粋な XAML では、単純なトリガーを表すことができますを追加する、`Trigger`をコントロールの要素がコレクションをトリガーします。
この例は、トリガーを変更する、`Entry`フォーカスを受け取ると、背景色。

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

* **TargetType**のトリガーを適用するコントロールの種類。

* **プロパティ**-監視されているコントロールのプロパティです。

* **値**-値、監視対象のプロパティの発生時に、原因となった、トリガーをアクティブ化します。

* **Set アクセス操作子**-のコレクション`Setter`要素を追加して、トリガー条件が満たされたとき。 指定する必要があります、`Property`と`Value`を設定します。

* **EnterActions と ExitActions** (非表示) - コードで記述され、に加えて (または instead of) を使用することができます`Setter`要素。 [以下に示す](#enterexit)です。

### <a name="applying-a-trigger-using-a-style"></a>スタイルを使用してトリガーを適用します。

トリガーを追加することも、`Style`ページ、またはアプリケーション内のコントロールでの宣言に`ResourceDictionary`です。 この例は、暗黙的なスタイルを宣言しています (ie。 ありません`Key`設定されている) すべてに適用することを意味する`Entry`ページ上のコントロールです。

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

## <a name="data-triggers"></a>データのトリガー

データのトリガーが発生する他のコントロールを監視するデータ バインドを使用して、`Setter`に呼び出されることができます。 代わりに、`Property`プロパティ トリガー内で属性は、設定、`Binding`を監視するため、指定された値属性。

次の例は、データ バインディングの構文を使用して`{Binding Source={x:Reference entry}, Path=Text.Length}`はどのように別のコントロールのプロパティを参照してください。 ときの長さ、 `entry` 0 の場合は、トリガーを起動します。 このサンプルでは、トリガーは、入力が空の場合に、ボタンを無効にします。

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

ヒント: 評価するときに`Path=Text.Length`(ターゲット プロパティの既定値を常に提供します。 `Text=""`) ためにそれ以外の場合、`null`トリガーは、予定どおりに動作しません。

指定するだけでなく`Setter`提供することも s [ `EnterActions`と`ExitActions`](#enterexit)です。

<a name="event" />

## <a name="event-triggers"></a>Event Triggers

`EventTrigger`要素にのみ必要ですが、`Event`プロパティなど`"Clicked"`次の例です。

```xaml
<EventTrigger Event="Clicked">
    <local:NumericValidationTriggerAction />
</EventTrigger>
```

あることに注意してくださいありません`Setter`要素がによって定義されるクラスへの参照ではなく`local:NumericValidationTriggerAction`する必要があります、`xmlns:local`宣言するのには、ページ内の XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:WorkingWithTriggers;assembly=WorkingWithTriggers"
```

クラス自体は、実装`TriggerAction`のオーバーライドを提供する必要がありますが、`Invoke`トリガー イベントが発生するたびに呼び出されるメソッド。

トリガーのアクションの実装を行ってください。

* ジェネリックを実装する`TriggerAction<T>`トリガーに適用されるコントロールの種類に対応するジェネリック パラメーターを持つ、クラスです。 スーパークラスをなど、使用できる`VisualElement`さまざまなコントロールを操作またはコントロールの種類を指定するトリガー アクションを記述する`Entry`です。

* 上書き、 `Invoke` - このメソッドは、トリガーの条件が満たされています。

* 必要に応じて、トリガーが宣言されている場合、XAML で設定できるプロパティを公開 (など`Anchor`、 `Scale`、および`Length`この例では)。

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

トリガーを共有する場合は注意が必要な`ResourceDictionary`、1 つのインスタンスが共有するコントロール間で 1 回構成されているいずれかの状態は、それらすべてに適用されます。

イベント トリガーがサポートしていないことに注意してください`EnterActions`と`ExitActions`[以下に示す](#enterexit)です。    

<a name="multi" />

## <a name="multi-triggers"></a>複数のトリガー

A`MultiTrigger`に似た、`Trigger`または`DataTrigger`複数の条件がある点が異なります。 すべての条件は、前に満たす必要があります、 `Setter`s をトリガーします。

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

`Conditions`コレクションも含まれている`PropertyCondition`次のような要素。

```xaml
<PropertyCondition Property="Text" Value="OK" />
```

### <a name="building-a-require-all-multi-trigger"></a>「すべてが必要」マルチ トリガーの作成

複数のトリガーは、すべての条件に当てはまる場合にのみ、コントロールを更新します。 条件のため注意が必要である (すべての入力を完了する必要があります、ログイン ページ) などの「すべてのフィールドの長さが 0」テスト"で Text.Length > 0" が、これは、XAML で表現することはできません。

これで、`IValueConverter`です。 コンバーター コード変換の下、`Text.Length`にバインドする`bool`かフィールドが空かどうかを示します。


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

このコンバーターを使用して、複数のトリガーを最初にページのリソースの辞書に追加 (カスタムと共に`xmlns:local`名前空間の定義)。

```xaml
<ResourceDictionary>
   <local:MultiTriggerConverter x:Key="dataHasBeenEntered" />
</ResourceDictionary>
```

XAML を次に示します。 最初の複数のトリガーの例から次の相違点に注意してください。

* ボタンには、`IsEnabled="false"`既定で設定します。
* 複数のトリガーの条件では、コンバーターを使用して有効にする、`Text.Length`にブール値。
* 条件がすべて処理される`true`、セッターは、ボタンの`IsEnabled`プロパティ`true`です。

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

これらのスクリーン ショットは、上記 2 つの複数のトリガー例の違いを示しています。 テキストが 1 つの入力、画面の最上部に、`Entry`だけで有効にするには、**保存**ボタンをクリックします。
画面の下部に、**ログイン**ボタンは、両方のフィールドがデータを格納するまでアクティブです。


![](triggers-images/multi-requireall.png "MultiTrigger 例")

<a name="enterexit" />

## <a name="enteractions-and-exitactions"></a>EnterActions と ExitActions

追加することで、トリガーが発生したときに、変更を実装する別の方法は、`EnterActions`と`ExitActions`コレクションを指定して`TriggerAction<T>`実装します。

使用できる*両方*`EnterActions`と`ExitActions`だけでなく`Setter`、トリガー内の s が、注意してくださいを`Setter`s はすぐと呼ばれます (を待ちません、`EnterAction`または`ExitAction`に完了)。 また、コード内のすべての実行し、使用しない`Setter`すべてで s。

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

クラスが XAML で参照されているときに、常に、宣言が必要に、名前空間など`xmlns:local`次に示すようにします。

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

注:`EnterActions`と`ExitActions`では無視されます**イベント トリガー**です。



## <a name="related-links"></a>関連リンク

- [トリガーのサンプル](https://developer.xamarin.com/samples/WorkingWithTriggers)
- [Xamarin.Forms API ドキュメント](https://developer.xamarin.com/api/type/Xamarin.Forms.TriggerAction%3CT%3E/)
