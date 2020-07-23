---
title: Xamarin.Forms のトリガー
description: この記事では、Xamarin.Forms のトリガーを使用して、XAML でのユーザー インターフェイスの変更に応答する方法について説明します。 トリガーを使用すると、イベントまたはプロパティの変更に基づいてコントロールの外観を変更するアクションを XAML での宣言として表すことができます。
ms.prod: xamarin
ms.assetid: 60460F57-63C6-4916-BBB5-A870F1DF53D7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/17/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 2a71f48fb9911267188e7aa4b4124cd9b7488d31
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86936475"
---
# <a name="xamarinforms-triggers"></a>Xamarin.Forms のトリガー

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithtriggers)

トリガーを使用すると、イベントまたはプロパティの変更に基づいてコントロールの外観を変更するアクションを XAML での宣言として表すことができます。 また、トリガーの特殊なグループである状態トリガーでは、[`VisualState`](xref:Xamarin.Forms.VisualState) を適用する条件を定義できます。

トリガーは、コントロールに直接割り当てることも、ページ レベルまたはアプリケーション レベルのリソース ディクショナリに追加して複数のコントロールに適用することもできます。

## <a name="property-triggers"></a>プロパティ トリガー

コントロールのトリガー コレクションに `Trigger` 要素を追加して、XAML 内だけでシンプルなトリガーを表現できます。
次の例では、フォーカスが設定されると背景色を `Entry` に変更するトリガーを示します。

```xaml
<Entry Placeholder="enter name">
    <Entry.Triggers>
        <Trigger TargetType="Entry"
                 Property="IsFocused" Value="True">
            <Setter Property="BackgroundColor" Value="Yellow" />
            <!-- multiple Setters elements are allowed -->
        </Trigger>
    </Entry.Triggers>
</Entry>
```

トリガーの宣言の重要な部分は次のとおりです。

- **TargetType** - トリガーを適用するコントロールの種類です。

- **Property** - コントロール上で監視するプロパティです。

- **Value** - 監視対象のプロパティがこの値になったら、トリガーをアクティブにします。

- **Setter** - トリガー条件が満たされたときの `Setter` 要素のコレクションを追加できます。 設定するには、`Property` と `Value` を指定する必要があります。

- **EnterActions と ExitActions** (示されていません) - コードで記述され、`Setter` 要素に加えて (または代えて) 使用できます。 これらについては、[後で説明します](#enteractions-and-exitactions)。

### <a name="applying-a-trigger-using-a-style"></a>スタイルを使用してトリガーを適用する

トリガーは、コントロール、ページ、またはアプリケーションの `ResourceDictionary` の `Style` 宣言に追加することもできます。 次の例では、暗黙的なスタイルを宣言しています (つまり、`Key` が設定されていません)。これは、ページ上のすべての `Entry` コントロールに適用されることを意味します。

```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        <Style TargetType="Entry">
                        <Style.Triggers>
                <Trigger TargetType="Entry"
                         Property="IsFocused" Value="True">
                    <Setter Property="BackgroundColor" Value="Yellow" />
                    <!-- multiple Setters elements are allowed -->
                </Trigger>
            </Style.Triggers>
        </Style>
    </ResourceDictionary>
</ContentPage.Resources>
```

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
            <!-- multiple Setters elements are allowed -->
        </DataTrigger>
    </Button.Triggers>
</Button>
```

> [!TIP]
> `Path=Text.Length` を評価するときは常に、ターゲット プロパティの既定値が提供されます (例: `Text=""`)。そうしないと `null` になって、トリガーが意図したとおりに動作しないためです。

`Setter` を指定するだけでなく、[`EnterActions` と `ExitActions`](#enteractions-and-exitactions) を提供することもできます。

## <a name="event-triggers"></a>Event triggers (イベント トリガー)

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

- 必要に応じて、トリガーを宣言するときに XAML で設定できるプロパティを公開します。 この例については、付属のサンプル アプリケーションの `VisualElementPopTriggerAction` クラスを参照してください。

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

これにより、イベント トリガーを XAML から使用できるようになります。

```xaml
<EventTrigger Event="TextChanged">
    <local:NumericValidationTriggerAction />
</EventTrigger>
```

`ResourceDictionary` でトリガーを共有する場合は注意が必要です。コントロール間で 1 つのインスタンスが共有されるので、1 つで構成された状態がすべてに適用されます。

イベント トリガーでは、[後で説明する](#enteractions-and-exitactions) `EnterActions` と `ExitActions` がサポートされていないことに注意してください。

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

![マルチ トリガーの例](triggers-images/multi-requireall.png)

## <a name="enteractions-and-exitactions"></a>EnterActions と ExitActions

トリガーが発生したときの変更を実装するもう 1 つの方法は、`EnterActions` および `ExitActions` コレクションを追加して、`TriggerAction<T>` の実装を指定することです。

[`EnterActions`](xref:Xamarin.Forms.TriggerBase.EnterActions) コレクションは、トリガー条件が満たされると呼び出される [`TriggerAction`](xref:Xamarin.Forms.TriggerAction) オブジェクトの `IList` を定義するのに使用されます。 [`ExitActions`](xref:Xamarin.Forms.TriggerBase.ExitActions) コレクションは、トリガー条件が満たされなくなったら呼び出される `TriggerAction` オブジェクトの `IList` を定義するのに使用されます。

> [!NOTE]
> `EnterActions` コレクションと `ExitActions` コレクションで定義されている [`TriggerAction`](xref:Xamarin.Forms.TriggerAction) オブジェクトは、[`EventTrigger`](xref:Xamarin.Forms.EventTrigger) クラスによって無視されます。    

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
    public int StartsFrom { set; get; }

    protected override void Invoke(VisualElement sender)
    {
        sender.Animate("FadeTriggerAction", new Animation((d) =>
        {
            var val = StartsFrom == 1 ? d : 1 - d;
            // so i was aiming for a different color, but then i liked the pink :)
            sender.BackgroundColor = Color.FromRgb(1, val, 1);
        }),
        length: 1000, // milliseconds
        easing: Easing.Linear);
    }
}
```

## <a name="state-triggers"></a>状態トリガー

状態トリガーは [`VisualState`](xref:Xamarin.Forms.VisualState) が適用される条件を定義するための特殊なトリガーのグループです。 

状態トリガーは、[`VisualState`](xref:Xamarin.Forms.VisualState) の [`StateTriggers`](xref:Xamarin.Forms.VisualState.StateTriggers) コレクションに追加されます。 このコレクションには、1 つの状態トリガーを含めることも、複数の状態トリガーを含めることもできます。 コレクション内のいずれかの状態トリガーがアクティブになっていると、[`VisualState`](xref:Xamarin.Forms.VisualState) が適用されます。

状態トリガーを使用してビジュアルの状態を制御する場合、Xamarin.Forms では、アクティブにするトリガー (および対応する [`VisualState`](xref:Xamarin.Forms.VisualState)) を決定するために、次の優先順位規則が使用されます。

1. [`StateTriggerBase`](xref:Xamarin.Forms.StateTriggerBase) から派生したトリガー。
1. [`MinWindowWidth`](xref:Xamarin.Forms.AdaptiveTrigger.MinWindowWidth) 条件の適用によってアクティブにされた [`AdaptiveTrigger`](xref:Xamarin.Forms.AdaptiveTrigger)。
1. [`MinWindowHeight`](xref:Xamarin.Forms.AdaptiveTrigger.MinWindowHeight) 条件の適用によってアクティブにされた [`AdaptiveTrigger`](xref:Xamarin.Forms.AdaptiveTrigger)。

複数のトリガーが同時にアクティブにされた場合 (たとえば、2 つのカスタム トリガー)、マークアップで最初に宣言されたトリガーが優先されます。

> [!NOTE]
> 状態トリガーは、[`Style`](xref:Xamarin.Forms.Style) で設定することも、要素で直接設定することもできます。

ビジュアルの状態の詳細については、「[Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)」をご覧ください。

### <a name="state-trigger"></a>状態トリガー

[`StateTriggerBase`](xref:Xamarin.Forms.StateTriggerBase) クラスから派生する [`StateTrigger`](xref:Xamarin.Forms.StateTrigger) クラスには、バインド可能な [`IsActive`](xref:Xamarin.Forms.StateTrigger.IsActive) プロパティがあります。 `StateTrigger` では、`IsActive` プロパティの値が変更されたときに [`VisualState`](xref:Xamarin.Forms.VisualState) の変更がトリガーされます。

すべての状態トリガーの基底クラスである [`StateTriggerBase`](xref:Xamarin.Forms.StateTriggerBase) クラスには、[`IsActive`](xref:Xamarin.Forms.StateTriggerBase.IsActive) プロパティと [`IsActiveChanged`](xref:Xamarin.Forms.StateTriggerBase.IsActiveChanged) イベントがあります。 このイベントは、[`VisualState`](xref:Xamarin.Forms.VisualState) が変更されるたびに発生します。 さらに、`StateTriggerBase` クラスには、オーバーライド可能な `OnAttached` メソッドと `OnDetached` メソッドがあります。

> [!IMPORTANT]
> バインド可能な [`StateTrigger.IsActive`](xref:Xamarin.Forms.StateTrigger.IsActive) プロパティによって、継承された [`StateTriggerBase.IsActive`](xref:Xamarin.Forms.StateTriggerBase.IsActive) プロパティが隠されます。

次の XAML の例は、[`StateTrigger`](xref:Xamarin.Forms.StateTrigger) オブジェクトを含む [`Style`](xref:Xamarin.Forms.Style) を示しています。

```xaml
<Style TargetType="Grid">
    <Setter Property="VisualStateManager.VisualStateGroups">
        <VisualStateGroupList>
            <VisualStateGroup>
                <VisualState x:Name="Checked">
                    <VisualState.StateTriggers>
                        <StateTrigger IsActive="{Binding IsToggled}"
                                      IsActiveChanged="OnCheckedStateIsActiveChanged" />
                    </VisualState.StateTriggers>
                    <VisualState.Setters>
                        <Setter Property="BackgroundColor"
                                Value="Black" />
                    </VisualState.Setters>
                </VisualState>
                <VisualState x:Name="Unchecked">
                    <VisualState.StateTriggers>
                        <StateTrigger IsActive="{Binding IsToggled, Converter={StaticResource inverseBooleanConverter}}"
                                      IsActiveChanged="OnUncheckedStateIsActiveChanged" />
                    </VisualState.StateTriggers>
                    <VisualState.Setters>
                        <Setter Property="BackgroundColor"
                                Value="White" />
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateGroupList>
    </Setter>
</Style>
```

この例では、暗黙的な [`Style`](xref:Xamarin.Forms.Style) によって [`Grid`](xref:Xamarin.Forms.Grid) オブジェクトがターゲットにされています。 バインドされたオブジェクトの `IsToggled` プロパティが `true` の場合、`Grid` の背景色は黒に設定されます。 バインドされたオブジェクトの `IsToggled` プロパティが `false` になると、[`VisualState`](xref:Xamarin.Forms.VisualState) の変更がトリガーされ、`Grid` の背景色が白になります。

さらに、[`VisualState`](xref:Xamarin.Forms.VisualState) の変更が発生するたびに、`VisualState` の [`IsActiveChanged`](xref:Xamarin.Forms.StateTriggerBase.IsActiveChanged) イベントが発生します。 各 `VisualState` によって、このイベントに対するイベント ハンドラーが登録されます。

```csharp
void OnCheckedStateIsActiveChanged(object sender, EventArgs e)
{
    StateTriggerBase stateTrigger = sender as StateTriggerBase;
    Console.WriteLine($"Checked state active: {stateTrigger.IsActive}");
}

void OnUncheckedStateIsActiveChanged(object sender, EventArgs e)
{
    StateTriggerBase stateTrigger = sender as StateTriggerBase;
    Console.WriteLine($"Unchecked state active: {stateTrigger.IsActive}");
}
```

この例では、[`IsActiveChanged`](xref:Xamarin.Forms.StateTriggerBase.IsActiveChanged) イベントに対するハンドラーが起動すると、ハンドラーによって [`VisualState`](xref:Xamarin.Forms.VisualState) がアクティブかどうかを示す情報が出力されます。 たとえば、ビジュアルの状態が `Checked` から `Unchecked` に変更されると、次のメッセージがコンソール ウィンドウに出力されます。

```
Checked state active: False
Unchecked state active: True
```

> [!NOTE]
> カスタム状態のトリガーを作成するには、[`StateTriggerBase`](xref:Xamarin.Forms.StateTriggerBase) クラスから派生させ、`OnAttached` メソッドと `OnDetached` メソッドをオーバーライドして、必要な登録とクリーンアップを実行します。

### <a name="adaptive-trigger"></a>アダプティブ トリガー

[`AdaptiveTrigger`](xref:Xamarin.Forms.AdaptiveTrigger) では、ウィンドウが指定した高さまたは幅になると、[`VisualState`](xref:Xamarin.Forms.VisualState) の変更がトリガーされます。 このトリガーには、次の 2 つのバインド可能なプロパティがあります。

- [`MinWindowHeight`](xref:Xamarin.Forms.AdaptiveTrigger.MinWindowHeight) (`double` 型)。[`VisualState`](xref:Xamarin.Forms.VisualState) が適用されるウィンドウの最小の高さを示します。
- [`MinWindowWidth`](xref:Xamarin.Forms.AdaptiveTrigger.MinWindowHeight) (`double` 型)。[`VisualState`](xref:Xamarin.Forms.VisualState) が適用されるウィンドウの最小の幅を示します。

> [!NOTE]
> [`AdaptiveTrigger`](xref:Xamarin.Forms.AdaptiveTrigger) は [`StateTriggerBase`](xref:Xamarin.Forms.StateTriggerBase) クラスから派生しているため、[`IsActiveChanged`](xref:Xamarin.Forms.StateTriggerBase.IsActiveChanged) イベントに対してイベント ハンドラーをアタッチできます。

次の XAML の例は、[`AdaptiveTrigger`](xref:Xamarin.Forms.AdaptiveTrigger) オブジェクトを含む [`Style`](xref:Xamarin.Forms.Style) を示しています。

```xaml
<Style TargetType="StackLayout">
    <Setter Property="VisualStateManager.VisualStateGroups">
        <VisualStateGroupList>
            <VisualStateGroup>
                <VisualState x:Name="Vertical">
                    <VisualState.StateTriggers>
                        <AdaptiveTrigger MinWindowWidth="0" />
                    </VisualState.StateTriggers>
                    <VisualState.Setters>
                        <Setter Property="Orientation"
                                Value="Vertical" />
                    </VisualState.Setters>
                </VisualState>
                <VisualState x:Name="Horizontal">
                    <VisualState.StateTriggers>
                        <AdaptiveTrigger MinWindowWidth="800" />
                    </VisualState.StateTriggers>
                    <VisualState.Setters>
                        <Setter Property="Orientation"
                                Value="Horizontal" />
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateGroupList>
    </Setter>
</Style>
```

この例では、暗黙的な [`Style`](xref:Xamarin.Forms.Style) によって [`StackLayout`](xref:Xamarin.Forms.StackLayout) オブジェクトがターゲットにされています。 ウィンドウの幅が、デバイスに依存しない単位で 0 から 800 である場合、`Style` が適用される `StackLayout` オブジェクトの向きは垂直方向になります。 ウィンドウの幅が、デバイスに依存しない単位で 800 以上である場合、[`VisualState`](xref:Xamarin.Forms.VisualState) の変更がトリガーされ、`StackLayout` の向きが水平に変わります。

![垂直の StackLayout VisualState](triggers-images/adaptivetrigger-vertical.png "AdaptiveTrigger の例")
![水平の StackLayout VisualState](triggers-images/adaptivetrigger-horizontal.png "AdaptiveTrigger の例")

[`MinWindowHeight`](xref:Xamarin.Forms.AdaptiveTrigger.MinWindowHeight) および [`MinWindowWidth`](xref:Xamarin.Forms.AdaptiveTrigger.MinWindowHeight) プロパティは、個別に使用することも、相互に組み合わせて使用することもできます。 次の XAML では、両方のプロパティを設定する例を示します。

```xaml
<AdaptiveTrigger MinWindowWidth="800"
                 MinWindowHeight="1200"/>
```

この例では、[`AdaptiveTrigger`](xref:Xamarin.Forms.AdaptiveTrigger) によって、現在のウィンドウの幅がデバイスに依存しない単位で 800 以上であり、かつ現在のウィンドウの高さがデバイスに依存しない単位で 1200 以上である場合に、対応する [`VisualState`](xref:Xamarin.Forms.VisualState) が適用されることが示されています。

### <a name="compare-state-trigger"></a>状態トリガーの比較

[`CompareStateTrigger`](xref:Xamarin.Forms.CompareStateTrigger) では、プロパティが特定の値と等しいときに [`VisualState`](xref:Xamarin.Forms.VisualState) の変更がトリガーされます。 このトリガーには、次の 2 つのバインド可能なプロパティがあります。

- [`Property`](xref:Xamarin.Forms.CompareStateTrigger.Property) (`object` 型)。トリガーによって比較されているプロパティを示します。
- [`Value`](xref:Xamarin.Forms.CompareStateTrigger.Value) (`object` 型)。[`VisualState`](xref:Xamarin.Forms.VisualState) が適用される値を示します。

> [!NOTE]
> [`CompareStateTrigger`](xref:Xamarin.Forms.CompareStateTrigger) は [`StateTriggerBase`](xref:Xamarin.Forms.StateTriggerBase) クラスから派生しているため、[`IsActiveChanged`](xref:Xamarin.Forms.StateTriggerBase.IsActiveChanged) イベントに対してイベント ハンドラーをアタッチできます。

次の XAML の例は、[`CompareStateTrigger`](xref:Xamarin.Forms.CompareStateTrigger) オブジェクトを含む [`Style`](xref:Xamarin.Forms.Style) を示しています。

```xaml
<Style TargetType="Grid">
    <Setter Property="VisualStateManager.VisualStateGroups">
        <VisualStateGroupList>
            <VisualStateGroup>
                <VisualState x:Name="Checked">
                    <VisualState.StateTriggers>
                        <CompareStateTrigger Property="{Binding Source={x:Reference checkBox}, Path=IsChecked}"
                                             Value="True" />
                    </VisualState.StateTriggers>
                    <VisualState.Setters>
                        <Setter Property="BackgroundColor"
                                Value="Black" />
                    </VisualState.Setters>
                </VisualState>
                <VisualState x:Name="Unchecked">
                    <VisualState.StateTriggers>
                        <CompareStateTrigger Property="{Binding Source={x:Reference checkBox}, Path=IsChecked}"
                                             Value="False" />
                    </VisualState.StateTriggers>
                    <VisualState.Setters>
                        <Setter Property="BackgroundColor"
                                Value="White" />
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateGroupList>
    </Setter>
</Style>
...
<Grid>
    <Frame BackgroundColor="White"
           CornerRadius="12"
           Margin="24"
           HorizontalOptions="Center"
           VerticalOptions="Center">
        <StackLayout Orientation="Horizontal">
            <CheckBox x:Name="checkBox"
                      VerticalOptions="Center" />
            <Label Text="Check the CheckBox to modify the Grid background color."
                   VerticalOptions="Center" />
        </StackLayout>
    </Frame>
</Grid>
```

この例では、暗黙的な [`Style`](xref:Xamarin.Forms.Style) によって [`Grid`](xref:Xamarin.Forms.Grid) オブジェクトがターゲットにされています。 [`CheckBox`](xref:Xamarin.Forms.CheckBox) の [`IsChecked`](xref:Xamarin.Forms.CheckBox.IsChecked) プロパティが `false` の場合、`Grid` の背景色は白に設定されます。 `CheckBox.IsChecked` プロパティが `true` になると、[`VisualState`](xref:Xamarin.Forms.VisualState) の変更がトリガーされ、`Grid` の背景色が黒になります。

[![トリガーされたビジュアルの状態の変更のスクリーンショット (iOS および Android)](triggers-images/comparestatetrigger-unchecked.png "CompareStateTrigger の例")](triggers-images/comparestatetrigger-unchecked-large.png#lightbox "CompareStateTrigger の例")
[![トリガーされたビジュアルの状態の変更のスクリーンショット (iOS および Android)](triggers-images/comparestatetrigger-checked.png "CompareStateTrigger の例")](triggers-images/comparestatetrigger-unchecked-large.png#lightbox "CompareStateTrigger の例")

### <a name="device-state-trigger"></a>デバイスの状態トリガー

[`DeviceStateTrigger`](xref:Xamarin.Forms.DeviceStateTrigger) では、アプリが実行されているデバイスのプラットフォームに基づいて [`VisualState`](xref:Xamarin.Forms.VisualState) の変更がトリガーされます。 このトリガーには、1 つのバインド可能なプロパティがあります。

- [`Device`](xref:Xamarin.Forms.DeviceStateTrigger.Device) (`string` 型)。[`VisualState`](xref:Xamarin.Forms.VisualState) が適用されるデバイスのプラットフォームを示します。

> [!NOTE]
> [`DeviceStateTrigger`](xref:Xamarin.Forms.DeviceStateTrigger) は [`StateTriggerBase`](xref:Xamarin.Forms.StateTriggerBase) クラスから派生しているため、[`IsActiveChanged`](xref:Xamarin.Forms.StateTriggerBase.IsActiveChanged) イベントに対してイベント ハンドラーをアタッチできます。

次の XAML の例は、`DeviceStateTrigger` オブジェクトを含む [`Style`](xref:Xamarin.Forms.Style) を示しています。

```xaml
<Style x:Key="DeviceStateTriggerPageStyle"
       TargetType="ContentPage">
    <Setter Property="VisualStateManager.VisualStateGroups">
        <VisualStateGroupList>
            <VisualStateGroup>
                <VisualState x:Name="iOS">
                    <VisualState.StateTriggers>
                        <DeviceStateTrigger Device="iOS" />
                    </VisualState.StateTriggers>
                    <VisualState.Setters>
                        <Setter Property="BackgroundColor"
                                Value="Silver" />
                    </VisualState.Setters>
                </VisualState>
                <VisualState x:Name="Android">
                    <VisualState.StateTriggers>
                        <DeviceStateTrigger Device="Android" />
                    </VisualState.StateTriggers>
                    <VisualState.Setters>
                        <Setter Property="BackgroundColor"
                                Value="#2196F3" />
                    </VisualState.Setters>
                </VisualState>
                <VisualState x:Name="UWP">
                    <VisualState.StateTriggers>
                        <DeviceStateTrigger Device="UWP" />
                    </VisualState.StateTriggers>
                    <VisualState.Setters>
                        <Setter Property="BackgroundColor"
                                Value="Aquamarine" />
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateGroupList>
    </Setter>
</Style>
```

この例では、明示的な [`Style`](xref:Xamarin.Forms.Style) によって [`ContentPage`](xref:Xamarin.Forms.ContentPage) オブジェクトがターゲットにされています。 このスタイルを使用する `ContentPage` オブジェクトでは、その背景色が、iOS ではシルバーに、Android では淡い青に、UWP ではアクアマリンに設定されます。 次のスクリーンショットは、iOS と Android で生成されるページを示しています。

[![トリガーされたビジュアルの状態の変更のスクリーンショット (iOS および Android)](triggers-images/devicestatetrigger.png "DeviceStateTrigger の例")](triggers-images/devicestatetrigger-large.png#lightbox "DeviceStateTrigger の例")

### <a name="orientation-state-trigger"></a>向きの状態トリガー

[`OrientationStateTrigger`](xref:Xamarin.Forms.OrientationStateTrigger) では、デバイスの向きが変更されたときに [`VisualState`](xref:Xamarin.Forms.VisualState) の変更がトリガーされます。 このトリガーには、1 つのバインド可能なプロパティがあります。

- [`Orientation`](xref:Xamarin.Forms.OrientationStateTrigger.Orientation) ([`DeviceOrientation`](xref:Xamarin.Forms.Internals.DeviceOrientation) 型)。[`VisualState`](xref:Xamarin.Forms.VisualState) が適用される向きを示します。

> [!NOTE]
> [`OrientationStateTrigger`](xref:Xamarin.Forms.OrientationStateTrigger) は [`StateTriggerBase`](xref:Xamarin.Forms.StateTriggerBase) クラスから派生しているため、[`IsActiveChanged`](xref:Xamarin.Forms.StateTriggerBase.IsActiveChanged) イベントに対してイベント ハンドラーをアタッチできます。

次の XAML の例は、`OrientationStateTrigger` オブジェクトを含む [`Style`](xref:Xamarin.Forms.Style) を示しています。

```xaml
<Style x:Key="OrientationStateTriggerPageStyle"
       TargetType="ContentPage">
    <Setter Property="VisualStateManager.VisualStateGroups">
        <VisualStateGroupList>
            <VisualStateGroup>
                <VisualState x:Name="Portrait">
                    <VisualState.StateTriggers>
                        <OrientationStateTrigger Orientation="Portrait" />
                    </VisualState.StateTriggers>
                    <VisualState.Setters>
                        <Setter Property="BackgroundColor"
                                Value="Silver" />
                    </VisualState.Setters>
                </VisualState>
                <VisualState x:Name="Landscape">
                    <VisualState.StateTriggers>
                        <OrientationStateTrigger Orientation="Landscape" />
                    </VisualState.StateTriggers>
                    <VisualState.Setters>
                        <Setter Property="BackgroundColor"
                                Value="White" />
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateGroupList>
    </Setter>
</Style>
```

この例では、明示的な [`Style`](xref:Xamarin.Forms.Style) によって [`ContentPage`](xref:Xamarin.Forms.ContentPage) オブジェクトがターゲットにされています。 このスタイルを使用する `ContentPage` オブジェクトでは、縦向きのときはその背景色がシルバーに、横向きのときはその背景色が白に設定されます。

## <a name="related-links"></a>関連リンク

- [トリガーのサンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithtriggers)
- [Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)
- [Xamarin.Forms トリガー API](xref:Xamarin.Forms.TriggerAction`1)
