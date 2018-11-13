---
title: 再利用可能な EventToCommandBehavior
description: 動作をしないコマンドと対話するように設計されたコントロールにコマンドを関連付けることができます。 この記事では、作成および使用イベントが発生したときにコマンドを呼び出す Xamarin.Forms の動作を示します。
ms.prod: xamarin
ms.assetid: EC7F6556-9776-40B8-9424-A8094482A2F3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/09/2018
ms.openlocfilehash: 8bf8f86cf708806d1c17b3fe4eda0755f98fd646
ms.sourcegitcommit: 03dfb4a2c20ad68515875b415e7d84ee9b0a8cb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2018
ms.locfileid: "51563187"
---
# <a name="reusable-eventtocommandbehavior"></a>再利用可能な EventToCommandBehavior

_動作をしないコマンドと対話するように設計されたコントロールにコマンドを関連付けることができます。この記事では、作成および使用イベントが発生したときにコマンドを呼び出す Xamarin.Forms の動作を示します。_

## <a name="overview"></a>概要

`EventToCommandBehavior`クラスへの応答でコマンドを実行します再利用可能な Xamarin.Forms カスタム動作は、*任意*イベントの発生します。 既定では、イベント引数のイベントが、コマンドに渡され、オプションでは、変換によって、 [ `IValueConverter` ](xref:Xamarin.Forms.IValueConverter)実装します。

動作を使用する次の動作のプロパティを設定する必要があります。

- **EventName** – 動作がリッスンするイベントの名前。
- **コマンド**–`ICommand`実行します。 動作が期待する、`ICommand`インスタンス、 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext)付属のコントロールは、親要素から継承する可能性があるのです。

次のオプションの動作のプロパティが設定することもできます。

- **CommandParameter** 、–`object`コマンドに渡すことです。
- **コンバーター** 、– [ `IValueConverter` ](xref:Xamarin.Forms.IValueConverter)実装間で渡されるイベント引数のデータの形式を変更する*ソース*と*ターゲット*、バインディング エンジン。

> [!NOTE]
> `EventToCommandBehavior`カスタム クラスに配置することですが、 [EventToCommand 動作のサンプル](https://developer.xamarin.com/samples/xamarin-forms/behaviors/eventtocommandbehavior/)Xamarin.Forms の一部でないとします。

## <a name="creating-the-behavior"></a>動作を作成します。

`EventToCommandBehavior`クラスから派生、`BehaviorBase<T>`から派生するクラスを[ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1)クラス。 目的、`BehaviorBase<T>`を必要とする Xamarin.Forms の動作の基本クラスを提供するクラスは、 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext)のアタッチされたコントロールに設定する動作。 これにより、動作にバインドして実行できる、`ICommand`で指定された、`Command`プロパティを使用すると、動作します。

`BehaviorBase<T>`クラスには、オーバーライド可能な[ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject))を設定するメソッド、 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext)の動作であり、オーバーライド可能な[ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject))をクリーンアップする方法、`BindingContext`します。 さらに、クラスに付属のコントロールへの参照を格納、`AssociatedObject`プロパティ。

### <a name="implementing-bindable-properties"></a>バインド可能なプロパティを実装します。

`EventToCommandBehavior`クラスは、4 つ定義[ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty)インスタンスは、ユーザーが実行されるにコマンドが定義されている場合、イベントが発生します。 これらのプロパティは次のコード例に示します。

```csharp
public class EventToCommandBehavior : BehaviorBase<View>
{
  public static readonly BindableProperty EventNameProperty =
    BindableProperty.Create ("EventName", typeof(string), typeof(EventToCommandBehavior), null, propertyChanged: OnEventNameChanged);
  public static readonly BindableProperty CommandProperty =
    BindableProperty.Create ("Command", typeof(ICommand), typeof(EventToCommandBehavior), null);
  public static readonly BindableProperty CommandParameterProperty =
    BindableProperty.Create ("CommandParameter", typeof(object), typeof(EventToCommandBehavior), null);
  public static readonly BindableProperty InputConverterProperty =
    BindableProperty.Create ("Converter", typeof(IValueConverter), typeof(EventToCommandBehavior), null);

  public string EventName { ... }
  public ICommand Command { ... }
  public object CommandParameter { ... }
  public IValueConverter Converter { ...  }
  ...
}
```

ときに、`EventToCommandBehavior`クラスが使用されて、`Command`プロパティにバインドされたデータをする必要があります、`ICommand`で定義されているイベントの発生からへの応答で実行される、`EventName`プロパティ。 動作が検出される、`ICommand`上、 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext)付属のコントロールの。

既定では、コマンドにイベントのイベント引数が渡すされます。 間で受け渡されると、このデータを必要に応じて変換することができます*ソース*と*ターゲット*を指定して、バインディング エンジン、 [ `IValueConverter` ](xref:Xamarin.Forms.IValueConverter)実装として、`Converter`プロパティの値。 指定することで、パラメーターをコマンドに渡される代わりに、`CommandParameter`プロパティの値。

### <a name="implementing-the-overrides"></a>オーバーライドを実装します。

`EventToCommandBehavior`オーバーライド、 [ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject))と[ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject))のメソッド、`BehaviorBase<T>`クラスに、次のコード例に示すように。

```csharp
public class EventToCommandBehavior : BehaviorBase<View>
{
  ...
  protected override void OnAttachedTo (View bindable)
  {
    base.OnAttachedTo (bindable);
    RegisterEvent (EventName);
  }

  protected override void OnDetachingFrom (View bindable)
  {
    DeregisterEvent (EventName);
    base.OnDetachingFrom (bindable);
  }
  ...
}
```

[ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject))メソッドを呼び出してセットアップを実行します、`RegisterEvent`の値を渡して、メソッド、`EventName`プロパティをパラメーターとして。 [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject))メソッドを呼び出してクリーンアップを実行します、`DeregisterEvent`の値を渡して、メソッド、`EventName`プロパティをパラメーターとして。

### <a name="implementing-the-behavior-functionality"></a>動作の機能を実装します。

動作の目的は、によって定義されているコマンドを実行する、`Command`プロパティで定義されているイベントの発生からへの応答、`EventName`プロパティ。 動作のコア機能は、次のコード例に示されます。

```csharp
public class EventToCommandBehavior : BehaviorBase<View>
{
  ...
  void RegisterEvent (string name)
  {
    if (string.IsNullOrWhiteSpace (name)) {
      return;
    }

    EventInfo eventInfo = AssociatedObject.GetType ().GetRuntimeEvent (name);
    if (eventInfo == null) {
      throw new ArgumentException (string.Format ("EventToCommandBehavior: Can't register the '{0}' event.", EventName));
    }
    MethodInfo methodInfo = typeof(EventToCommandBehavior).GetTypeInfo ().GetDeclaredMethod ("OnEvent");
    eventHandler = methodInfo.CreateDelegate (eventInfo.EventHandlerType, this);
    eventInfo.AddEventHandler (AssociatedObject, eventHandler);
  }

  void OnEvent (object sender, object eventArgs)
  {
    if (Command == null) {
      return;
    }

    object resolvedParameter;
    if (CommandParameter != null) {
      resolvedParameter = CommandParameter;
    } else if (Converter != null) {
      resolvedParameter = Converter.Convert (eventArgs, typeof(object), null, null);
    } else {
      resolvedParameter = eventArgs;
    }        

    if (Command.CanExecute (resolvedParameter)) {
      Command.Execute (resolvedParameter);
    }
  }
  ...
}
```

`RegisterEvent`への応答でメソッドが実行される、`EventToCommandBehavior`の値を受け取ると、コントロールにアタッチする、`EventName`プロパティをパラメーターとして。 定義されているイベントを検索を試行し、`EventName`接続されているコントロールのプロパティ。 イベントは、格納されていること、`OnEvent`メソッドがイベントのハンドラー メソッドに登録されています。

`OnEvent`で定義されているイベントの発生からへの応答でメソッドが実行される、`EventName`プロパティ。 されるとき、`Command`プロパティが有効な参照`ICommand`、メソッドに渡すパラメーターを取得しようとしました。、`ICommand`次のようにします。

- 場合、`CommandParameter`パラメーターを定義するプロパティ、取得されます。
- の場合、`Converter`プロパティを定義、 [ `IValueConverter` ](xref:Xamarin.Forms.IValueConverter)コンバーターの実装が実行され、の間で渡されるイベント引数のデータを変換*ソース*と*ターゲット*バインディング エンジン。
- それ以外の場合、イベントの引数は、パラメーターと見なされます。

バインドされたデータ`ICommand`を実行して、提供するコマンドにパラメーターに渡すこと、 [ `CanExecute` ](xref:Xamarin.Forms.Command.CanExecute(System.Object))メソッドを返します。`true`します。

ここでは、示されていませんが、`EventToCommandBehavior`も含まれています、`DeregisterEvent`メソッドによって実行される、 [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject))メソッド。 `DeregisterEvent`見つけてで定義されているイベントの登録を解除するメソッドを使用、`EventName`プロパティは、潜在的なメモリ リークをクリーンアップします。

## <a name="consuming-the-behavior"></a>動作の使用

`EventToCommandBehavior`クラスに接続できる、 [ `Behaviors` ](xref:Xamarin.Forms.VisualElement.Behaviors)次の XAML コード例のように、コントロールのコレクション。

```xaml
<ListView ItemsSource="{Binding People}">
    <ListView.ItemTemplate>
        <DataTemplate>
            <TextCell Text="{Binding Name}" />
        </DataTemplate>
    </ListView.ItemTemplate>
    <ListView.Behaviors>
        <local:EventToCommandBehavior EventName="ItemSelected" Command="{Binding OutputAgeCommand}" Converter="{StaticResource SelectedItemConverter}" />
    </ListView.Behaviors>
</ListView>
<Label Text="{Binding SelectedItemText}" />
```

これと同じ C# コードの例は次のとおりです。

```csharp
var listView = new ListView();
listView.SetBinding(ItemsView<Cell>.ItemsSourceProperty, "People");
listView.ItemTemplate = new DataTemplate(() =>
{
    var textCell = new TextCell();
    textCell.SetBinding(TextCell.TextProperty, "Name");
    return textCell;
});
listView.Behaviors.Add(new EventToCommandBehavior
{
    EventName = "ItemSelected",
    Command = ((HomePageViewModel)BindingContext).OutputAgeCommand,
    Converter = new SelectedItemEventArgsToSelectedItemConverter()
});

var selectedItemLabel = new Label();
selectedItemLabel.SetBinding(Label.TextProperty, "SelectedItemText");
```

`Command`動作のプロパティにバインドされたデータは、`OutputAgeCommand`プロパティ、関連付けられているビューモデルの中に、`Converter`プロパティに設定されて、`SelectedItemConverter`インスタンスを返す、 [ `SelectedItem` ](xref:Xamarin.Forms.ListView.SelectedItem)の[ `ListView` ](xref:Xamarin.Forms.ListView)から、 [ `SelectedItemChangedEventArgs`](xref:Xamarin.Forms.SelectedItemChangedEventArgs)します。

実行時に、動作は、コントロールとの対話に応答します。 項目を選択すると、 [ `ListView` ](xref:Xamarin.Forms.ListView)、 [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected)が実行されるイベントは起動、`OutputAgeCommand`ビューモデルでします。 ビューモデルをさらにこの更新`SelectedItemText`プロパティを[ `Label` ](xref:Xamarin.Forms.Label)に、次のスクリーン ショットに示すようにバインドします。

[![](event-to-command-behavior-images/screenshots-sml.png "サンプル アプリケーションで EventToCommandBehavior")](event-to-command-behavior-images/screenshots.png#lightbox "EventToCommandBehavior でアプリケーションのサンプル")

イベントが発生したときにコマンドを実行するこの動作を使用する利点は、コマンドがコマンドと対話するように設計でした。 コントロールに関連付けできることです。 さらに、分離コード ファイルからボイラー プレート イベント処理コードを削除します。

## <a name="summary"></a>まとめ

この記事では、イベント発生時にコマンドを呼び出す Xamarin.Forms の動作を使用して示されています。 動作をしないコマンドと対話するように設計されたコントロールにコマンドを関連付けることができます。

## <a name="related-links"></a>関連リンク

- [EventToCommand 動作 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/eventtocommandbehavior/)
- [動作](xref:Xamarin.Forms.Behavior)
- [動作&lt;T&gt;](xref:Xamarin.Forms.Behavior`1)
