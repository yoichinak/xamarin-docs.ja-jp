---
title: "再利用可能な EventToCommandBehavior"
description: "動作にコマンドをコマンドと対話するように設計されていないコントロールに関連付けるために使用できます。 この記事では、コマンドを呼び出すイベント発生時の Xamarin.Forms 動作の使用方法を示します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: EC7F6556-9776-40B8-9424-A8094482A2F3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: d82a1391feca9187cf2aca4394509447aeac6a18
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="reusable-eventtocommandbehavior"></a>再利用可能な EventToCommandBehavior

_動作にコマンドをコマンドと対話するように設計されていないコントロールに関連付けるために使用できます。この記事では、コマンドを呼び出すイベント発生時の Xamarin.Forms 動作の使用方法を示します。_

## <a name="overview"></a>概要

`EventToCommandBehavior`クラスは、再利用可能な Xamarin.Forms カスタム動作への応答でコマンドを実行する*任意*イベントの発生します。 既定では、イベント引数のイベントが、コマンドに渡され、オプションでは、変換によって、 [ `IValueConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/)実装します。

次の動作プロパティは、動作を使用して設定する必要があります。

- **EventName** – 動作をリッスンするイベントの名前。
- **コマンド**– **ICommand**を実行します。 動作が期待する、`ICommand`インスタンスに、 [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/)の付属のコントロールは、親要素から継承することがあります。

次のオプションの動作のプロパティが設定することもできます。

- **CommandParameter** –、`object`コマンドに渡されます。
- **コンバーター** –、 [ `IValueConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/)実装間で渡されるイベント引数のデータの形式を変更する*ソース*と*ターゲット*バインディング エンジンによってです。

## <a name="creating-the-behavior"></a>動作を作成します。

`EventToCommandBehavior`から派生したクラス、`BehaviorBase<T>`から派生するクラス、 [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/)クラスです。 目的、`BehaviorBase<T>`クラスは、Xamarin.Forms 動作が必要である場合、基底クラスを提供する、 [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/)添付コントロールに設定する動作をします。 これにより、動作がバインドして実行できる、`ICommand`によって指定された、`Command`プロパティを使用すると、動作します。

`BehaviorBase<T>`クラスは、オーバーライド可能な提供[ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/)を設定するメソッド、 [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/)の動作であり、オーバーライド可能な[ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/)をクリーンアップする方法、`BindingContext`です。 さらに、クラスに接続されているコントロールへの参照を格納、`AssociatedObject`プロパティです。

### <a name="implementing-bindable-properties"></a>バインド可能なプロパティを実装します。

`EventToCommandBehavior`クラスは、次の 4 つ定義[ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/)ユーザーを実行するインスタンスは、イベント発生時にコマンドを定義します。 これらのプロパティは次のコード例に示します。

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

ときに、`EventToCommandBehavior`クラスを使用すると、`Command`プロパティのデータにバインドされている必要があります、`ICommand`で定義されているイベントの発生時に実行される、`EventName`プロパティです。 検索する動作を求める、`ICommand`上、 [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/)付属するコントロールのです。

既定では、イベントのイベント引数は、コマンドに渡されます。 間で渡される、このデータを必要に応じて変換することができます*ソース*と*ターゲット*を指定して、バインド エンジンによって、 [ `IValueConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/)実装として、`Converter`プロパティの値。 代わりに、パラメーターに渡せるコマンドは、指定することによって、`CommandParameter`プロパティの値。

### <a name="implementing-the-overrides"></a>オーバーライドの実装

`EventToCommandBehavior`クラスのオーバーライド、 [ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/)と[ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/)のメソッド、`BehaviorBase<T>`クラスに、次のコード例に示すようにします。

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

[ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/)メソッドを呼び出してセットアップを実行する、`RegisterEvent`の値を渡して、メソッド、`EventName`プロパティをパラメーターとします。 [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/)メソッドを呼び出してクリーンアップを実行する、`DeregisterEvent`の値を渡して、メソッド、`EventName`プロパティをパラメーターとします。

### <a name="implementing-the-behavior-functionality"></a>動作の機能を実装します。

動作の目的は、によって定義されたコマンドを実行する、`Command`プロパティで定義されているイベントの発生時に、`EventName`プロパティです。 動作のコア機能は、次のコード例で表示されます。

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

`RegisterEvent`への応答でメソッドが実行される、`EventToCommandBehavior`の値を受け取るコントロール、およびそれにアタッチされている、`EventName`プロパティをパラメーターとします。 定義されているイベントを探しを試行し、`EventName`接続されているコントロール上のプロパティです。 イベントがある、その、`OnEvent`イベントのハンドラー メソッドであるメソッドを登録します。

`OnEvent`で定義されているイベントの発生時にメソッドが実行される、`EventName`プロパティです。 `Command`プロパティが有効な参照`ICommand`、メソッドに渡すパラメーターを取得しようとしました。、`ICommand`次のようにします。

- 場合、`CommandParameter`プロパティのパラメーターを定義するを取得します。
- それ以外の場合、`Converter`プロパティを定義、 [ `IValueConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/)コンバーターの実装が実行され、の間で渡されるイベント引数のデータを変換*ソース*と*ターゲット*バインディング エンジンによってです。
- それ以外の場合、イベント引数は、パラメーターと見なされます。

バインドされたデータ`ICommand`を実行して、提供されるコマンドにパラメーターに渡すこと、 [ `CanExecute` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Command.CanExecute/p/System.Object/)メソッドを返します。`true`です。

ここでは、示されていませんが、`EventToCommandBehavior`も含まれています、`DeregisterEvent`メソッドによって実行される、 [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/)メソッドです。 `DeregisterEvent`見つけてで定義されているイベントの登録を解除するメソッドが使用される、`EventName`潜在的なメモリ リークをクリーンアップするには、プロパティです。

## <a name="consuming-the-behavior"></a>動作の使用

`EventToCommandBehavior`に添付できるクラス、 [ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/)の XAML コードの例を次に示すように、コントロールのコレクション。

```xaml
<ListView ItemsSource="{Binding People}">
  <ListView.Behaviors>
    <local:EventToCommandBehavior EventName="ItemSelected" Command="{Binding OutputAgeCommand}"
                                  Converter="{StaticResource SelectedItemConverter}" />
  </ListView.Behaviors>
</ListView>
<Label Text="{Binding SelectedItemText}" />
```

これと同じ C# コードの例は次のとおりです。

```csharp
var listView = new ListView ();
listView.SetBinding (ItemsView<Cell>.ItemsSourceProperty, "People");
listView.Behaviors.Add (new EventToCommandBehavior {
  EventName = "ItemSelected",
  Command = ((HomePageViewModel)BindingContext).OutputAgeCommand,
  Converter = new SelectedItemEventArgsToSelectedItemConverter ()
});

var selectedItemLabel = new Label ();
selectedItemLabel.SetBinding (Label.TextProperty, "SelectedItemText");
```

`Command`動作のプロパティがデータにバインドされている、`OutputAgeCommand`プロパティ、関連付けられている ViewModel の中に、`Converter`プロパティに設定されている、`SelectedItemConverter`インスタンスは、どのを返します、 [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SelectedItem/)の[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)から、 [ `SelectedItemChangedEventArgs`](https://developer.xamarin.com/api/type/Xamarin.Forms.SelectedItemChangedEventArgs/)です。

実行時に、動作は、コントロールとの対話に応答します。 項目が選択した場合、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)、 [ `ItemSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemSelected/)を実行するイベントは起動、 `OutputAgeCommand` ViewModel にします。 この更新、ViewModel`SelectedItemText`プロパティを[ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)次のスクリーン ショットに示すようにバインドします。

[![](event-to-command-behavior-images/screenshots-sml.png "サンプル アプリケーションで EventToCommandBehavior")](event-to-command-behavior-images/screenshots.png#lightbox "EventToCommandBehavior でアプリケーションのサンプル")

イベントが発生したときにコマンドを実行するこの動作を使用する利点は、コマンドがコマンドとの対話に設計されていないコントロールを関連付けできることです。 さらに、分離コード ファイルからボイラー プレート イベント処理コードを削除します。

## <a name="summary"></a>まとめ

この記事では、イベント発生時にコマンドを呼び出す Xamarin.Forms 動作を使用して示されています。 動作にコマンドをコマンドと対話するように設計されていないコントロールに関連付けるために使用できます。


## <a name="related-links"></a>関連リンク

- [EventToCommand 動作 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/eventtocommandbehavior/)
- [動作](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/)
- [動作<T>](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/)
