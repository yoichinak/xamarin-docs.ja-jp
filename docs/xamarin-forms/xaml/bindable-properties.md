---
title: バインド可能なプロパティ
description: Xamarin.Forms では、共通言語ランタイム (CLR) のプロパティの機能がバインド可能なプロパティで拡張されます。 バインド可能なプロパティは、特殊な型、プロパティの Xamarin.Forms プロパティ システムによって、プロパティの値を追跡する場所です。 この記事では、バインド可能なプロパティは、の概要について説明し、作成し、それらを使用する方法を示します。
ms.prod: xamarin
ms.assetid: 1EE869D8-6FE1-45CA-A0AD-26EC7D032AD7
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 06/02/2016
ms.openlocfilehash: 7e1d3c82036ef703014ae548a6719937e89d22f4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="bindable-properties"></a>バインド可能なプロパティ

_Xamarin.Forms では、共通言語ランタイム (CLR) のプロパティの機能がバインド可能なプロパティで拡張されます。バインド可能なプロパティは、特殊な型、プロパティの Xamarin.Forms プロパティ システムによって、プロパティの値を追跡する場所です。この記事では、バインド可能なプロパティは、の概要について説明し、作成し、それらを使用する方法を示します。_

## <a name="overview"></a>概要

バインド可能なプロパティでは、CLR プロパティの機能を拡張を持つプロパティをバックアップすることによって、 [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/)型のバッキング フィールドを持つプロパティではなく、します。 バインド可能なプロパティの目的は、データ バインディング、スタイル、テンプレートをサポートするシステム プロパティを提供して、値は、親子のリレーションシップを介して設定されます。 さらに、バインド可能なプロパティは、既定値、プロパティの値、およびプロパティの変更を監視するためのコールバックの検証を提供できます。

プロパティは、次の機能の 1 つ以上をサポートするためにバインド可能なプロパティとして実装する必要があります。

- 有効なとして機能する*ターゲット*データ バインディングのプロパティです。
- 使用して、プロパティを設定、[スタイル](~/xamarin-forms/user-interface/styles/index.md)です。
- プロパティの型の既定値とは異なる既定のプロパティ値を提供します。
- プロパティの値を検証しています。
- プロパティの変更を監視します。

Xamarin.Forms バインド可能なプロパティの例として、 [ `Label.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/)、 [ `Button.BorderRadius` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.BorderRadius/)、および[ `StackLayout.Orientation`](https://developer.xamarin.com/api/property/Xamarin.Forms.StackLayout.Orientation/)です。 各バインド可能なプロパティには、対応する`public static readonly`型のプロパティ[ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/)同じクラスが公開されていると、バインド可能なプロパティの識別子です。 など、対応するバインド可能なプロパティの識別子を`Label.Text`プロパティは[ `Label.TextProperty`](https://developer.xamarin.com/api/field/Xamarin.Forms.Label.TextProperty/)です。

<a name="consuming-bindable-property" />

## <a name="creating-and-consuming-a-bindable-property"></a>作成およびバインド可能なプロパティの使用

バインド可能なプロパティを作成するプロセスは次のとおりです。

1. 作成、 [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/)のいずれかのインスタンス、 [ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/)メソッドのオーバー ロードします。
1. プロパティ アクセサーを定義します、 [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/)インスタンス。

なおすべて[ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) UI スレッドでインスタンスを作成する必要があります。 これは、UI スレッドで実行されるコードのみを取得またはバインド可能なプロパティの値を設定することを意味します。 ただし、`BindableProperty`インスタンスは、UI スレッドにマーシャ リングによって他のスレッドからアクセスできる、 [ `Device.BeginInvokeOnMainThread` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.BeginInvokeOnMainThread/p/System.Action/)メソッドです。

### <a name="creating-a-property"></a>プロパティを作成します。

作成する、`BindableProperty`インスタンス、外側のクラスから派生しなければなりません、 [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/)クラスです。 ただし、`BindableObject`クラスがクラスの階層構造に高いため、クラスの大部分がユーザー インターフェイスの機能のサポートのバインド可能なプロパティを使用します。

バインド可能なプロパティを宣言して作成できる、`public static readonly`型のプロパティ[ `BindableProperty`](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/)です。 1 つの戻り値にバインド可能なプロパティを設定する必要があります、 [ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/)メソッドのオーバー ロードします。 本体内で宣言をする必要があります[ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/)派生クラスには、メンバーの定義の外部でします。

作成するときに、少なくとも、識別子を指定する必要があります、 [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/)、と共に、次のパラメーター。

- 名前、 [ `BindableProperty`](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/)です。
- プロパティの型。
- 所有するオブジェクトの型。
- プロパティの既定値。 これにより、プロパティ常に値を返すこと、特定の既定値に設定されていると、プロパティの型の既定値と異なることができます。 既定値に復元するときに、 [ `ClearValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.ClearValue/p/Xamarin.Forms.BindableProperty/)バインド可能なプロパティのメソッドが呼び出されます。

次のコードでは、識別子および 4 つの必須パラメーターの値のバインド可能なプロパティの例を示します。

```csharp
public static readonly BindableProperty EventNameProperty =
  BindableProperty.Create ("EventName", typeof(string), typeof(EventToCommandBehavior), null);
```

これを作成、 [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/)という名前のインスタンス`EventName`、型の`string`します。 プロパティが所有、`EventToCommandBehavior`クラスの既定値は`null`します。 バインド可能なプロパティの名前付け規則では、バインド可能なプロパティの識別子がで指定されたプロパティ名に一致する必要があります、`Create`メソッド、"Property"が追加されます。 したがって、上記の例では、バインド可能なプロパティの識別子は`EventNameProperty`します。

必要に応じて、作成するときに、 [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/)インスタンス、次のパラメーターを指定することができます。

- バインド モードです。 プロパティ値の変更が反映されるまでの方向を指定に使用されます。 既定のバインド モードで変更が反映されるまでから、*ソース*を*ターゲット*です。
- プロパティの値が設定されている場合に呼び出される検証デリゲート。 詳細については、次を参照してください。[検証コールバック](#validation)です。
- プロパティは、プロパティの値が変更されたときに呼び出されるデリゲートを変更します。 詳細については、次を参照してください。[検出プロパティが変更された](#propertychanges)です。
- プロパティ値が変わるときに呼び出されるデリゲートを変更するプロパティです。 このデリゲートは、プロパティの変更の代理として同じシグネチャを持ちます。
- プロパティの値が変更されたときに呼び出される強制値デリゲート。 詳細については、次を参照してください。 [Coerce 値コールバック](#coerce)です。
- A`Func`プロパティの既定値を初期化するために使用されます。 詳細については、次を参照してください。 [、Func と既定の値を作成する](#defaultfunc)です。

### <a name="creating-accessors"></a>アクセサーの作成

プロパティ アクセサーは、プロパティの構文を使用してバインド可能なプロパティにアクセスする必要があります。 `Get`アクセサーは、対応するバインド可能なプロパティに格納されている値を返す必要があります。 呼び出すことによってこれを行う、 [ `GetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.GetValue/p/Xamarin.Forms.BindableProperty/)メソッド、値を取得する対象のバインド可能なプロパティの識別子に渡すと、必要な型に結果をキャストします。 `Set`アクセサーは、対応するバインド可能なプロパティの値を設定する必要があります。 呼び出すことによってこれを行う、 [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/)メソッドを値、および設定する値を設定する対象のバインド可能なプロパティの識別子を渡します。

次のコード例のアクセサーを示しています、`EventName`バインド可能なプロパティ。

```csharp
public string EventName {
  get { return (string)GetValue (EventNameProperty); }
  set { SetValue (EventNameProperty, value); }
}
```

### <a name="consuming-a-bindable-property"></a>バインド可能なプロパティの使用

バインド可能なプロパティが作成した後は、XAML またはコードから使用できます。 XAML では、これは CLR 名前空間の名前、および必要に応じて、アセンブリ名を示す名前空間の宣言で、プレフィックスを持つ名前空間を宣言することによって実現されます。 詳細については、次を参照してください。 [XAML 名前空間](~/xamarin-forms/xaml/namespaces.md)です。

次のコード例では、カスタムの型を参照しているアプリケーション コードと同じアセンブリ内で定義されているバインド可能なプロパティを格納するカスタム型の XAML 名前空間を示しています。

```xaml
<ContentPage ... xmlns:local="clr-namespace:EventToCommandBehavior" ...>
  ...
</ContentPage>
```

設定するときに、名前空間宣言が使用される、 `EventName` XAML コードの例を次に示すよう、バインド可能なプロパティ。

```xaml
<ListView ...>
  <ListView.Behaviors>
    <local:EventToCommandBehavior EventName="ItemSelected" ... />
  </ListView.Behaviors>
</ListView>
```

これと同じ C# コードの例は次のとおりです。

```csharp
var listView = new ListView ();
listView.Behaviors.Add (new EventToCommandBehavior {
  EventName = "ItemSelected",
  ...
});
```

<a name="advanced" />

## <a name="advanced-scenarios"></a>高度なシナリオ

作成するときに、 [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/)インスタンスはさまざまなバインド可能なプロパティが高度なシナリオを有効に設定できる省略可能なパラメーターがあります。 このセクションでは、これらのシナリオについて説明します。

<a name="propertychanges" />

### <a name="detecting-property-changes"></a>プロパティの変更を検出します。

A`static`を指定してバインド可能なプロパティを使用してプロパティ変更のコールバック メソッドを登録することができます、`propertyChanged`のパラメーター、 [ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/)メソッドです。 指定されたコールバック メソッドは、バインド可能なプロパティの値が変更されたときに呼び出されます。

次のコード例に示す方法、`EventName`バインド可能なプロパティのレジスタ、`OnEventNameChanged`プロパティ変更のコールバック メソッドとしてメソッド。

```csharp
public static readonly BindableProperty EventNameProperty =
  BindableProperty.Create (
    "EventName", typeof(string), typeof(EventToCommandBehavior), null, propertyChanged: OnEventNameChanged);
...

static void OnEventNameChanged (BindableObject bindable, object oldValue, object newValue)
{
  // Property changed implementation goes here
}
```

プロパティ変更のコールバック メソッドで、 [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/)パラメーターは、所有するクラスのインスタンスが、変更、および 2 つの値を報告しましたを示すために使用`object`パラメーター、新旧の値を表しますバインド可能なプロパティです。

<a name="validation" />

### <a name="validation-callbacks"></a>検証コールバック

A`static`を指定してバインド可能なプロパティを使用して検証コールバック メソッドを登録することができます、`validateValue`のパラメーター、 [ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/)メソッドです。 バインド可能なプロパティの値が設定されている場合は、指定されたコールバック メソッドが呼び出されます。

次のコード例に示す方法、`Angle`バインド可能なプロパティのレジスタ、`IsValidValue`検証コールバック メソッドとしてメソッド。

```csharp
public static readonly BindableProperty AngleProperty =
  BindableProperty.Create ("Angle", typeof(double), typeof(HomePage), 0.0, validateValue: IsValidValue);
...

static bool IsValidValue (BindableObject view, object value)
{
  double result;
  bool isDouble = double.TryParse (value.ToString (), out result);
  return (result >= 0 && result <= 360);
}
```

検証コールバックは、値を使用し、返す必要があります`true`かどうか、値は、プロパティ、それ以外の場合`false`です。 検証コールバックを返す場合、例外が発生する`false`、開発者によってを処理する必要があります。 検証コールバック メソッドの一般的な使用は、バインド可能なプロパティが設定されている場合、整数または倍の値を制限するがします。 たとえば、`IsValidValue`メソッドをチェックするプロパティの値、 `double` 0 ~ 360 の範囲内です。

<a name="coerce" />

### <a name="coerce-value-callbacks"></a>強制値コールバック

A`static`強制値を指定してバインド可能なプロパティを使用してコールバック メソッドを登録することができます、`coerceValue`のパラメーター、 [ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/)メソッドです。 指定されたコールバック メソッドは、バインド可能なプロパティの値が変更されたときに呼び出されます。

コールバックが、プロパティの値が変更されたときに、バインド可能なプロパティの再評価を強制的に使用される値に変換します。 たとえば、1 つのバインド可能なプロパティの値が別のバインド可能なプロパティの値を超えていないことを確認する強制値のコールバックを使用できます。

次のコード例に示す方法、`Angle`バインド可能なプロパティのレジスタ、`CoerceAngle`強制値コールバック メソッドとしてメソッド。

```csharp
public static readonly BindableProperty AngleProperty = BindableProperty.Create (
  "Angle", typeof(double), typeof(HomePage), 0.0, coerceValue: CoerceAngle);
public static readonly BindableProperty MaximumAngleProperty = BindableProperty.Create (
  "MaximumAngle", typeof(double), typeof(HomePage), 360.0);
...

static object CoerceAngle (BindableObject bindable, object value)
{
  var homePage = bindable as HomePage;
  double input = (double)value;

  if (input > homePage.MaximumAngle) {
    input = homePage.MaximumAngle;
  }

  return input;
}
```

`CoerceAngle`メソッドの値を調べて、`MaximumAngle`プロパティ、場合に、`Angle`プロパティ値がよりも大きい、強制的に変換する値、`MaximumAngle`プロパティの値。

<a name="defaultfunc" />

### <a name="creating-a-default-value-with-a-func"></a>Func で、既定値を作成します。

A`Func`次のコード例に示すように、バインド可能なプロパティの既定値を初期化するために使用されることができます。

```csharp
public static readonly BindableProperty SizeProperty =
  BindableProperty.Create ("Size", typeof(double), typeof(HomePage), 0.0,
  defaultValueCreator: bindable => Device.GetNamedSize (NamedSize.Large, (Label)bindable));
```

`defaultValueCreator`にパラメーターが設定されている、`Func`を呼び出す、 [ `Device.GetNamedSize` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.GetNamedSize/p/Xamarin.Forms.NamedSize/System.Type/)を返すメソッドを`double`で使用されるフォントの名前付きサイズを表す、 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)ネイティブ プラットフォームでします。

## <a name="summary"></a>まとめ

この記事は、バインド可能なプロパティの概要について説明し、作成し、それらを使用する方法を示しました。 バインド可能なプロパティは、特殊な型、プロパティの Xamarin.Forms プロパティ システムによって、プロパティの値を追跡する場所です。


## <a name="related-links"></a>関連リンク

- [XAML 名前空間](~/xamarin-forms/xaml/namespaces.md)
- [イベントにコマンドの動作 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/eventtocommandbehavior/)
- [検証コールバック (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/xaml/validationcallback/)
- [Coerce 値コールバック (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/xaml/coercevaluecallback/)
- [BindableProperty](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/)
- [BindableObject](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/)
