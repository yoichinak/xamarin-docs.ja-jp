---
title: バインド可能なプロパティ
description: この記事では、バインド可能なプロパティは、概要を示し、作成し、これらを使用する方法を示します。
ms.prod: xamarin
ms.assetid: 1EE869D8-6FE1-45CA-A0AD-26EC7D032AD7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/02/2016
ms.openlocfilehash: 59ec82c222208ff0e73b0b7b5226d416d3a53398
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68656596"
---
# <a name="bindable-properties"></a>バインド可能なプロパティ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/behaviors-eventtocommandbehavior)

_Xamarin.Forms では、共通言語ランタイム (CLR) のプロパティの機能は、バインド可能なプロパティが拡張されます。バインド可能なプロパティは、特殊な種類のプロパティ、プロパティの値が Xamarin.Forms プロパティ システムによって追跡されます。この記事では、バインド可能なプロパティは、概要を示し、作成し、これらを使用する方法を示します。_

## <a name="overview"></a>概要

バインド可能なプロパティを持つプロパティをバックアップすることで CLR プロパティの機能を拡張する、 [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty)バッキング フィールドのプロパティではなく、型。 バインド可能なプロパティの目的は、プロパティ システムのデータ バインディング、スタイル、テンプレート、サポートを提供して、値は、親子のリレーションシップを設定します。 さらに、バインド可能なプロパティは、既定値、プロパティの値、およびプロパティの変更を監視するコールバックの検証を提供することができます。

プロパティは、次の機能の 1 つ以上をサポートするバインド可能なプロパティとして実装する必要があります。

- 有効なとして機能する*ターゲット*データ バインディングのプロパティ。
- プロパティの設定、[スタイル](~/xamarin-forms/user-interface/styles/index.md)します。
- プロパティの既定値は異なるプロパティの型の既定値を提供します。
- プロパティの値を検証しています。
- プロパティの変更を監視します。

Xamarin.Forms のバインド可能なプロパティの例として、 [ `Label.Text` ](xref:Xamarin.Forms.Label.Text)、 [ `Button.BorderRadius` ](xref:Xamarin.Forms.Button.BorderRadius)、および[ `StackLayout.Orientation`](xref:Xamarin.Forms.StackLayout.Orientation)します。 各バインド可能なプロパティには、対応する`public static readonly`型のプロパティ[ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty)を同じクラスで公開されると、バインド可能なプロパティの識別子です。 例では、対応するバインド可能なプロパティ識別子、`Label.Text`プロパティは[ `Label.TextProperty`](xref:Xamarin.Forms.Label.TextProperty)します。

<a name="consuming-bindable-property" />

## <a name="creating-and-consuming-a-bindable-property"></a>作成およびバインド可能なプロパティを使用します。

バインド可能なプロパティを作成するプロセスは次のとおりです。

1. 作成、 [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty)のいずれかのインスタンス、 [ `BindableProperty.Create` ](xref:Xamarin.Forms.BindableProperty.Create*)メソッドのオーバー ロードします。
1. プロパティ アクセサーを定義、 [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty)インスタンス。

なおすべて[ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) UI スレッドでインスタンスを作成する必要があります。 つまり、UI スレッドで実行されるコードのみが取得またはバインド可能なプロパティの値を設定できます。 ただし、`BindableProperty`インスタンスで UI スレッドにマーシャ リングによって他のスレッドからアクセスできる、 [ `Device.BeginInvokeOnMainThread` ](xref:Xamarin.Forms.Device.BeginInvokeOnMainThread(System.Action))メソッド。

### <a name="creating-a-property"></a>プロパティを作成します。

作成する、`BindableProperty`インスタンス、外側のクラスから派生する必要があります、 [ `BindableObject` ](xref:Xamarin.Forms.BindableObject)クラス。 ただし、`BindableObject`クラスは、そのクラスの大部分がユーザー インターフェイス機能のサポートのバインド可能なプロパティの使用、クラス階層の上位。

バインド可能なプロパティを宣言することで作成できます、`public static readonly`型のプロパティ[ `BindableProperty`](xref:Xamarin.Forms.BindableProperty)します。 1 つの戻り値にバインド可能なプロパティを設定する必要があります、 [ `BindableProperty.Create` ](xref:Xamarin.Forms.BindableProperty.Create(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate))メソッドのオーバー ロードします。 本文内で宣言があります[ `BindableObject` ](xref:Xamarin.Forms.BindableObject)派生クラスでは、任意のメンバーの定義の外部で。

作成するときに、少なくとも、識別子を指定する必要があります、 [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty)、次のパラメーター。

- 名前、 [ `BindableProperty`](xref:Xamarin.Forms.BindableProperty)します。
- プロパティの型。
- 所有するオブジェクトの型。
- プロパティの既定値。 これによりが設定されていてもかまいませんプロパティの型の既定値と異なる場合、プロパティがその特定の既定値を常に返すこと。 既定値に復元するときに、 [ `ClearValue` ](xref:Xamarin.Forms.BindableObject.ClearValue(Xamarin.Forms.BindableProperty))バインド可能なプロパティでメソッドが呼び出されます。

次のコードでは、識別子と、次の 4 つの必須パラメーターの値のバインド可能なプロパティの例を示します。

```csharp
public static readonly BindableProperty EventNameProperty =
  BindableProperty.Create ("EventName", typeof(string), typeof(EventToCommandBehavior), null);
```

これを作成、 [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty)という名前のインスタンス`EventName`、型の`string`します。 プロパティが所有、`EventToCommandBehavior`クラスし、の既定値を持つ`null`します。 バインド可能なプロパティの名前付け規則では、バインド可能なプロパティの識別子がで指定されたプロパティ名に一致する必要があります、`Create`メソッドは、"Property"が追加されます。 そのため、上記の例では、バインド可能なプロパティの識別子は`EventNameProperty`します。

必要に応じて、作成するときに、 [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty)インスタンスを次のパラメーターを指定できます。

- バインド モード。 これは、プロパティ値の変更が反映されるまでの方向を指定に使用されます。 既定のバインド モードで変更が反映されます、*ソース*を*ターゲット*します。
- プロパティの値が設定されている場合に呼び出される検証デリゲート。 詳細については、次を参照してください。[検証コールバック](#validation)します。
- プロパティは、プロパティの値が変更されたときに呼び出されるデリゲートを変更します。 詳細については、次を参照してください。[プロパティ変更の検出](#propertychanges)します。
- プロパティの値は変更時に呼び出されるデリゲートを変更するプロパティ。 このデリゲートは、プロパティが変更されたデリゲートとして同じシグニチャを持ちます。
- プロパティの値が変更されたときに呼び出される強制値デリゲート。 詳細については、次を参照してください。[強制値コールバック](#coerce)します。
- A`Func`プロパティの既定値を初期化するために使用されます。 詳細については、次を参照してください。 [Func を既定値を作成する](#defaultfunc)します。

### <a name="creating-accessors"></a>アクセサーの作成

プロパティ アクセサーは、プロパティ構文を使用してバインド可能なプロパティにアクセスする必要があります。 `Get`アクセサーは、対応するバインド可能なプロパティに格納されている値を返す必要があります。 これは、呼び出すことによって実現できます、 [ `GetValue` ](xref:Xamarin.Forms.BindableObject.GetValue(Xamarin.Forms.BindableProperty))メソッドを値を取得するバインド可能なプロパティの識別子を渡すと、必要な型に結果をキャストします。 `Set`アクセサーは、対応するバインド可能なプロパティの値を設定する必要があります。 これは、呼び出すことによって実現できます、 [ `SetValue` ](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object))値、および設定する値を設定する対象のバインド可能なプロパティの識別子を渡す方法です。

次のコード例のアクセサーを示しています、`EventName`バインド可能なプロパティ。

```csharp
public string EventName {
  get { return (string)GetValue (EventNameProperty); }
  set { SetValue (EventNameProperty, value); }
}
```

### <a name="consuming-a-bindable-property"></a>バインド可能なプロパティの使用

バインド可能なプロパティが作成されると、XAML またはコードから使用できます。 XAML では、これは、CLR 名前空間の名前および必要に応じて、アセンブリ名を示す名前空間宣言で、プレフィックスを持つ名前空間を宣言することによって実現されます。 詳細については、次を参照してください。 [XAML 名前空間](~/xamarin-forms/xaml/namespaces.md)します。

次のコード例では、カスタムの型を参照しているアプリケーション コードと同じアセンブリ内で定義されているバインド可能なプロパティを含むカスタム型の XAML 名前空間を示しています。

```xaml
<ContentPage ... xmlns:local="clr-namespace:EventToCommandBehavior" ...>
  ...
</ContentPage>
```

設定するときに、名前空間宣言を使用して、`EventName`の XAML コードの例を次のとおり、バインド可能なプロパティ。

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

作成するときに、 [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty)インスタンス、さまざまなバインド可能なプロパティが高度なシナリオを有効に設定できるオプションのパラメータがあります。 このセクションでは、これらのシナリオについて説明します。

<a name="propertychanges" />

### <a name="detecting-property-changes"></a>プロパティの変更の検出

A`static`プロパティ変更コールバック メソッドを指定することでバインド可能なプロパティに登録することができます、`propertyChanged`のパラメーター、 [ `BindableProperty.Create` ](xref:Xamarin.Forms.BindableProperty.Create(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate))メソッド。 指定されたコールバック メソッドは、バインド可能なプロパティの値が変更されたときに呼び出されます。

次のコード例に示す方法、`EventName`プロパティのバインド可能なレジスタ、`OnEventNameChanged`プロパティ変更コールバック メソッドとしてメソッド。

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

プロパティ変更コールバック メソッドで、 [ `BindableObject` ](xref:Xamarin.Forms.BindableObject)パラメーターは、所有元クラスのインスタンスは、変更、および 2 つの値が報告を示すために使用`object`パラメーターは、新旧の値を表します。バインド可能なプロパティ。

<a name="validation" />

### <a name="validation-callbacks"></a>検証コールバック

A`static`を指定してバインド可能なプロパティを使用して検証コールバック メソッドを登録することができます、`validateValue`のパラメーター、 [ `BindableProperty.Create` ](xref:Xamarin.Forms.BindableProperty.Create(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate))メソッド。 指定されたコールバック メソッドは、バインド可能なプロパティの値が設定されている場合に呼び出されます。

次のコード例に示す方法、`Angle`プロパティのバインド可能なレジスタ、`IsValidValue`検証コールバック メソッドとしてメソッド。

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

検証コールバックは、の値に提供され、返す必要があります`true`かどうか、値が有効で、プロパティ、それ以外の場合`false`します。 検証コールバックを返す場合、例外が発生する`false`、開発者によってこれを処理する必要があります。 検証コールバック メソッドの一般的な用途は、バインド可能なプロパティが設定されている場合の整数または倍精度小数点数の値を制約します。 たとえば、`IsValidValue`プロパティ値があるメソッドを確認します、`double`範囲 0 ~ 360 です。

<a name="coerce" />

### <a name="coerce-value-callbacks"></a>強制値コールバック

A`static`強制値コールバック メソッドを指定することでバインド可能なプロパティに登録することができます、`coerceValue`のパラメーター、 [ `BindableProperty.Create` ](xref:Xamarin.Forms.BindableProperty.Create(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate))メソッド。 指定されたコールバック メソッドは、バインド可能なプロパティの値が変更されたときに呼び出されます。

プロパティの値が変更されたときに、バインド可能なプロパティの再評価を強制するコールバックを使用する値を強制します。 たとえば、1 つのバインド可能なプロパティの値が別のバインド可能なプロパティの値を超えていないことを確認する強制値コールバックを使用できます。

次のコード例に示す方法、`Angle`プロパティのバインド可能なレジスタ、`CoerceAngle`強制値コールバック メソッドとしてメソッド。

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

`CoerceAngle`メソッドの値をチェックする、`MaximumAngle`プロパティ、場合に、`Angle`プロパティ値がよりも大きい、強制的に変換する値、`MaximumAngle`プロパティの値。

<a name="defaultfunc" />

### <a name="creating-a-default-value-with-a-func"></a>Func の既定値を作成します。

A`Func`次のコード例に示すように、バインド可能なプロパティの既定値を初期化するために使用できます。

```csharp
public static readonly BindableProperty SizeProperty =
  BindableProperty.Create ("Size", typeof(double), typeof(HomePage), 0.0,
  defaultValueCreator: bindable => Device.GetNamedSize (NamedSize.Large, (Label)bindable));
```

`defaultValueCreator`パラメーターに設定されて、`Func`を呼び出す、 [ `Device.GetNamedSize` ](xref:Xamarin.Forms.Device.GetNamedSize(Xamarin.Forms.NamedSize,System.Type))を返すメソッドを`double`で使用されるフォントの名前付きのサイズを表す、 [ `Label` ](xref:Xamarin.Forms.Label) 、ネイティブ プラットフォームで。

## <a name="summary"></a>まとめ

この記事では、バインド可能なプロパティの概要について説明し、作成し、これらを使用する方法を示しました。 バインド可能なプロパティは、特殊な種類のプロパティ、プロパティの値が Xamarin.Forms プロパティ システムによって追跡されます。


## <a name="related-links"></a>関連リンク

- [XAML 名前空間](~/xamarin-forms/xaml/namespaces.md)
- [イベントをコマンドの動作 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/behaviors-eventtocommandbehavior)
- [検証コールバック (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-validationcallback)
- [強制値コールバック (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-coercevaluecallback)
- [BindableProperty](xref:Xamarin.Forms.BindableProperty)
- [BindableObject](xref:Xamarin.Forms.BindableObject)
