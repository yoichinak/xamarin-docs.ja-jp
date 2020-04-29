---
title: 再利用可能な EventToCommandBehavior
description: ビヘイビアーを使用すると、コマンドとやりとりするように設計されていないコントロールにコマンドを関連付けることができます。 この記事では、Xamarin.Forms のビヘイビアーを作成および使用して、イベントが発生したときにコマンドを呼び出す方法を示します。
ms.prod: xamarin
ms.assetid: EC7F6556-9776-40B8-9424-A8094482A2F3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/09/2018
ms.openlocfilehash: 292a6aaaea4fb0f84138e04c88f001c72ddd096d
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "68650909"
---
# <a name="reusable-eventtocommandbehavior"></a>再利用可能な EventToCommandBehavior

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/behaviors-eventtocommandbehavior)

_ビヘイビアーを使用すると、コマンドとやりとりするように設計されていないコントロールにコマンドを関連付けることができます。この記事では、Xamarin.Forms のビヘイビアーを作成および使用して、イベントが発生したときにコマンドを呼び出す方法を示します。_

## <a name="overview"></a>概要

`EventToCommandBehavior` クラスは、"*任意の*" イベントの発生に応答してコマンドを実行する再利用可能な Xamarin.Forms カスタム ビヘイビアーです。 既定では、イベント用のイベント引数はコマンドに渡されます。必要に応じて、このイベント引数を [`IValueConverter`](xref:Xamarin.Forms.IValueConverter) 実装によって変換することができます。

ビヘイビアーを使用するには、次のビヘイビアー プロパティを設定する必要があります。

- **EventName** – ビヘイビアーがリッスンするイベントの名前です。
- **Command** – 実行する `ICommand` です。 アタッチされたコントロールの [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) 上に `ICommand` インスタンスがあることが、ビヘイビアーによって期待されています。このコントロールは親要素から継承されている場合があります。

次の省略可能なビヘイビアー プロパティを設定することもできます。

- **CommandParameter** – コマンドに渡される `object` です。
- **Converter** – "*ソース*" と "*ターゲット*" の間でバインド エンジンによってイベント引数データが受け渡しされるときにその形式を変更する [`IValueConverter`](xref:Xamarin.Forms.IValueConverter) 実装です。

> [!NOTE]
> `EventToCommandBehavior` は、[EventToCommand Behavior サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/behaviors-eventtocommandbehavior) に配置することができるカスタム クラスであり、Xamarin.Forms の一部ではありません。

## <a name="creating-the-behavior"></a>ビヘイビアーの作成

`EventToCommandBehavior` クラスは、[`Behavior<T>`](xref:Xamarin.Forms.Behavior`1) クラスから派生した `BehaviorBase<T>` クラスから派生したものです。 `BehaviorBase<T>` クラスの目的は、ビヘイビアーの [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) をアタッチされたコントロールに設定することが求められる Xamarin.Forms ビヘイビアーに対して基底クラスを提供することにあります。 これにより、確実に、ビヘイビアーを使用するときに `ICommand` プロパティによって指定される `Command` にビヘイビアーがバインドされ、それがビヘイビアーによって実行されます。

`BehaviorBase<T>` クラスでは、ビヘイビアーの [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) を設定するオーバーライド可能な [`OnAttachedTo`](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) メソッドと、`BindingContext` をクリーンアップするオーバーライド可能な [`OnDetachingFrom`](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) メソッドが指定されます。 さらに、このクラスでは、アタッチされたコントロールへの参照が `AssociatedObject` プロパティに格納されます。

### <a name="implementing-bindable-properties"></a>バインド可能プロパティの実装

`EventToCommandBehavior` クラスでは、イベントが発生したときにユーザー定義コマンドを実行する 4 つの [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) インスタンスが定義されます。 これらのプロパティを次のコード例に示します。

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

`EventToCommandBehavior` クラスを使用する場合は、`Command` プロパティを、`EventName` プロパティで定義されたイベントの発生に応答して実行される `ICommand` にバインドされたデータとする必要があります。 アタッチされたコントロールの [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) 上に `ICommand` インスタンスがあることが、ビヘイビアーによって期待されています。

既定では、イベント用のイベント引数はコマンドに渡されます。 このデータは、"*ソース*" と "*ターゲット*" の間でバインド エンジンによって受け渡しされるときに、必要に応じて変換することができます。そのためには、[`IValueConverter`](xref:Xamarin.Forms.IValueConverter) 実装を `Converter` プロパティの値として指定します。 あるいは、`CommandParameter` プロパティの値を指定することで、パラメーターをコマンドに渡すことができます。

### <a name="implementing-the-overrides"></a>オーバーライドの実装

次のコード例に示すように、`BehaviorBase<T>` クラスの [`OnAttachedTo`](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) メソッドと [`OnDetachingFrom`](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) メソッドは `EventToCommandBehavior` クラスによってオーバーライドされます。

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

[`OnAttachedTo`](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) メソッドでは、`RegisterEvent` メソッドを呼び出してセットアップが実行され、`EventName` プロパティの値がパラメーターとして渡されます。 [`OnDetachingFrom`](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) メソッドでは、`DeregisterEvent` メソッドを呼び出してクリーンアップが実行され、`EventName` プロパティの値がパラメーターとして渡されます。

### <a name="implementing-the-behavior-functionality"></a>ビヘイビアー機能の実装

ビヘイビアーの目的は、`EventName` プロパティによって定義されたイベントの発生に応答して、`Command` プロパティで定義されたコマンドを実行することです。 ビヘイビアーの主な機能を次のコード例に示します。

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

コントロールにアタッチされている `EventName` に応答して `EventToCommandBehavior` メソッドが実行され、このメソッドでは `RegisterEvent` プロパティの値がパラメーターとして受信されます。 次に、このメソッドでは、アタッチされたコントロール上で、`EventName` プロパティに定義されているイベントの検索が試みられます。 イベントを検索できたら、`OnEvent` メソッドがイベント用のハンドラー メソッドとして登録されます。

`OnEvent` メソッドは、`EventName` プロパティに定義されたイベントの発生に応答して実行されます。 `Command` プロパティで有効な `ICommand` が参照されている場合、そのメソッドでは、次のように、`ICommand` に渡すパラメーターの取得が試みられます。

- `CommandParameter` プロパティにパラメーターが定義されている場合は、それが取得されます。
- あるいは、`Converter` プロパティに [`IValueConverter`](xref:Xamarin.Forms.IValueConverter) 実装が定義されている場合は、イベント引数データが "*ソース*" と "*ターゲット*" の間でバインド エンジンによって受け渡しされるとき、コンバーターが実行されイベント引数データが変換されます。
- それ以外の場合、イベント引数はパラメーターと見なされます。

[`CanExecute`](xref:Xamarin.Forms.Command.CanExecute(System.Object)) メソッドから `true` が返されると、データ バインドされた `ICommand` が実行され、パラメーターがコマンドに渡されます。

ここでは示していませんが、`EventToCommandBehavior` にも [`OnDetachingFrom`](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) メソッドによって実行される `DeregisterEvent` メソッドが含まれています。 `DeregisterEvent` メソッドを使用することで、`EventName` プロパティに定義されたイベントを検索して再登録すると、潜在的なメモリ リークがクリーンアップされます。

## <a name="consuming-the-behavior"></a>ビヘイビアーの使用

次の XAML コード例に示すように、コントロールの [`Behaviors`](xref:Xamarin.Forms.VisualElement.Behaviors) コレクションに `EventToCommandBehavior` クラスをアタッチできます。

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

ビヘイビアーの `Command` プロパティは関連する ViewModel の `OutputAgeCommand` プロパティにバインドされたデータです。これに対して、`Converter` プロパティは `SelectedItemConverter` インスタンスに設定されます。このインスタンスからは、[`SelectedItemChangedEventArgs`](xref:Xamarin.Forms.SelectedItemChangedEventArgs) からの [`ListView`](xref:Xamarin.Forms.ListView) の [`SelectedItem`](xref:Xamarin.Forms.ListView.SelectedItem) が返されます。

実行時、ビヘイビアーはコントロールとのやりとりに応答します。 [`ListView`](xref:Xamarin.Forms.ListView) において項目を選択すると、[`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) イベントが発生し、これによって、ViewModel の `OutputAgeCommand` が実行されます。 さらに、これによって、次のスクリーンショットに示すように、[`Label`](xref:Xamarin.Forms.Label) のバインド先である、ViewModel の `SelectedItemText` プロパティが更新されます。

[![](event-to-command-behavior-images/screenshots-sml.png "Sample Application with EventToCommandBehavior")](event-to-command-behavior-images/screenshots.png#lightbox "Sample Application with EventToCommandBehavior")

イベントが発生したときにこのビヘイビアーを使用してコマンドを実行することの利点は、コマンドとやりとりするように設計されていないコントロールにコマンドを関連付けできることです。 さらに、これによって、イベントを処理する定型コードが分離コード ファイルから削除されます。

## <a name="summary"></a>まとめ

この記事では、Xamarin.Forms のビヘイビアーを使用して、イベントが発生したときにコマンドを呼び出す方法を示します。 ビヘイビアーを使用すると、コマンドとやりとりするように設計されていないコントロールにコマンドを関連付けることができます。

## <a name="related-links"></a>関連リンク

- [EventToCommand Behavior (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/behaviors-eventtocommandbehavior)
- [Behavior](xref:Xamarin.Forms.Behavior)
- [Behavior&lt;T&gt;](xref:Xamarin.Forms.Behavior`1)
