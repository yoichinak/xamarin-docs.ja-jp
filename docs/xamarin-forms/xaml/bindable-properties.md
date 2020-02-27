---
title: Xamarin. フォームのバインド可能なプロパティ
description: この記事では、バインド可能なプロパティは、概要を示し、作成し、これらを使用する方法を示します。
ms.prod: xamarin
ms.assetid: 1EE869D8-6FE1-45CA-A0AD-26EC7D032AD7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/16/2020
ms.openlocfilehash: 56583aa0df676ae55e1b283f1a8e151b69a13d28
ms.sourcegitcommit: 10b4d7952d78f20f753372c53af6feb16918555c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/26/2020
ms.locfileid: "77635749"
---
# <a name="xamarinforms-bindable-properties"></a>Xamarin. フォームのバインド可能なプロパティ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/behaviors-eventtocommandbehavior)

バインド可能なプロパティは、プロパティをフィールドでバッキングするのではなく、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)型を使用してプロパティをバックアップすることで、CLR プロパティの機能を拡張します。 バインド可能なプロパティの目的は、プロパティ システムのデータ バインディング、スタイル、テンプレート、サポートを提供して、値は、親子のリレーションシップを設定します。 さらに、バインド可能なプロパティは、既定値、プロパティの値、およびプロパティの変更を監視するコールバックの検証を提供することができます。

プロパティは、次の機能の 1 つ以上をサポートするバインド可能なプロパティとして実装する必要があります。

- データバインディングの有効な*ターゲット*プロパティとして機能します。
- [スタイル](~/xamarin-forms/user-interface/styles/index.md)を使用してプロパティを設定します。
- プロパティの既定値は異なるプロパティの型の既定値を提供します。
- プロパティの値を検証しています。
- プロパティの変更を監視します。

Xamarin. フォームバインド可能プロパティの例としては、 [`Label.Text`](xref:Xamarin.Forms.Label.Text)、 [`Button.BorderRadius`](xref:Xamarin.Forms.Button.BorderRadius)、 [`StackLayout.Orientation`](xref:Xamarin.Forms.StackLayout.Orientation)があります。 バインド可能な各プロパティには、同じクラスで公開され、バインド可能なプロパティの識別子である、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)型の対応する `public static readonly` プロパティがあります。 たとえば、`Label.Text` プロパティの対応するバインド可能なプロパティ識別子は[`Label.TextProperty`](xref:Xamarin.Forms.Label.TextProperty)です。

## <a name="create-a-bindable-property"></a>バインド可能なプロパティを作成する

バインド可能なプロパティを作成するプロセスは次のとおりです。

1. [`BindableProperty.Create`](xref:Xamarin.Forms.BindableProperty.Create*)メソッドオーバーロードのいずれかを使用して、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)インスタンスを作成します。
1. [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)インスタンスのプロパティアクセサーを定義します。

すべての[`BindableProperty`](xref:Xamarin.Forms.BindableProperty)インスタンスは、UI スレッド上に作成する必要があります。 つまり、UI スレッドで実行されるコードのみが取得またはバインド可能なプロパティの値を設定できます。 ただし、 [`Device.BeginInvokeOnMainThread`](xref:Xamarin.Forms.Device.BeginInvokeOnMainThread(System.Action))メソッドを使用して UI スレッドにマーシャリングすることによって、他のスレッドから `BindableProperty` インスタンスにアクセスできます。

### <a name="create-a-property"></a>プロパティを作成する

`BindableProperty` インスタンスを作成するには、含んでいるクラスが[`BindableObject`](xref:Xamarin.Forms.BindableObject)クラスから派生している必要があります。 ただし、クラスの階層では `BindableObject` クラスが高いため、ユーザーインターフェイス機能に使用されるクラスの大部分は、バインド可能なプロパティをサポートしています。

バインド可能なプロパティを作成するには、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)型の `public static readonly` プロパティを宣言します。 バインド可能なプロパティは、 [`BindableProperty.Create`](xref:Xamarin.Forms.BindableProperty.Create(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate))メソッドオーバーロードのいずれかの戻り値に設定する必要があります。 宣言は[`BindableObject`](xref:Xamarin.Forms.BindableObject)派生クラスの本体内にあり、メンバー定義の外部にある必要があります。

少なくとも、次のパラメーターと共に[`BindableProperty`](xref:Xamarin.Forms.BindableProperty)を作成するときに識別子を指定する必要があります。

- [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)の名前。
- プロパティの型。
- 所有するオブジェクトの型。
- プロパティの既定値。 これによりが設定されていてもかまいませんプロパティの型の既定値と異なる場合、プロパティがその特定の既定値を常に返すこと。 既定値は、バインド可能なプロパティで[`ClearValue`](xref:Xamarin.Forms.BindableObject.ClearValue(Xamarin.Forms.BindableProperty))メソッドが呼び出されたときに復元されます。

次のコードでは、識別子と、次の 4 つの必須パラメーターの値のバインド可能なプロパティの例を示します。

```csharp
public static readonly BindableProperty EventNameProperty =
  BindableProperty.Create ("EventName", typeof(string), typeof(EventToCommandBehavior), null);
```

これにより、`string`型の `EventName`という名前の[`BindableProperty`](xref:Xamarin.Forms.BindableProperty)インスタンスが作成されます。 このプロパティは `EventToCommandBehavior` クラスによって所有されており、既定値は `null`です。 バインド可能なプロパティの名前付け規則では、バインド可能なプロパティの識別子が、`Create` メソッドで指定されたプロパティ名と一致し、"Property" が追加されている必要があります。 したがって、上記の例では、バインド可能なプロパティ識別子は `EventNameProperty`です。

必要に応じて[`BindableProperty`](xref:Xamarin.Forms.BindableProperty)インスタンスを作成するときに、次のパラメーターを指定できます。

- バインド モード。 これは、プロパティ値の変更が反映されるまでの方向を指定に使用されます。 既定のバインディングモードでは、変更は*ソース*から*ターゲット*に反映されます。
- プロパティの値が設定されている場合に呼び出される検証デリゲート。 詳細については、「[検証コールバック](#validation-callbacks)」を参照してください。
- プロパティは、プロパティの値が変更されたときに呼び出されるデリゲートを変更します。 詳細については、「[プロパティの変更の検出](#detect-property-changes)」を参照してください。
- プロパティの値は変更時に呼び出されるデリゲートを変更するプロパティ。 このデリゲートは、プロパティが変更されたデリゲートとして同じシグニチャを持ちます。
- プロパティの値が変更されたときに呼び出される強制値デリゲート。 詳細については、「[強制値のコールバック](#coerce-value-callbacks)」を参照してください。
- 既定のプロパティ値を初期化するために使用される `Func`。 詳細については、「 [Func を使用した既定値の作成](#create-a-default-value-with-a-func)」を参照してください。

### <a name="create-accessors"></a>アクセサーの作成

プロパティ アクセサーは、プロパティ構文を使用してバインド可能なプロパティにアクセスする必要があります。 `Get` アクセサーは、対応するバインド可能なプロパティに含まれている値を返す必要があります。 これを実現するには、 [`GetValue`](xref:Xamarin.Forms.BindableObject.GetValue(Xamarin.Forms.BindableProperty))メソッドを呼び出し、値を取得するバインド可能なプロパティ識別子を渡してから、結果を必要な型にキャストします。 `Set` アクセサーは、対応するバインド可能なプロパティの値を設定する必要があります。 これは、 [`SetValue`](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object))メソッドを呼び出して、値を設定するバインド可能なプロパティ識別子と設定する値を渡すことによって実現できます。

次のコード例は、`EventName` バインド可能なプロパティのアクセサーを示しています。

```csharp
public string EventName
{
  get { return (string)GetValue (EventNameProperty); }
  set { SetValue (EventNameProperty, value); }
}
```

## <a name="consume-a-bindable-property"></a>バインド可能なプロパティを使用する

バインド可能なプロパティが作成されると、XAML またはコードから使用できます。 XAML では、これは、CLR 名前空間の名前および必要に応じて、アセンブリ名を示す名前空間宣言で、プレフィックスを持つ名前空間を宣言することによって実現されます。 詳細については、「 [XAML 名前空間](~/xamarin-forms/xaml/namespaces.md)」を参照してください。

次のコード例では、カスタムの型を参照しているアプリケーション コードと同じアセンブリ内で定義されているバインド可能なプロパティを含むカスタム型の XAML 名前空間を示しています。

```xaml
<ContentPage ... xmlns:local="clr-namespace:EventToCommandBehavior" ...>
  ...
</ContentPage>
```

名前空間宣言は、次の XAML コード例に示すように、`EventName` バインド可能なプロパティを設定するときに使用します。

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
listView.Behaviors.Add (new EventToCommandBehavior
{
  EventName = "ItemSelected",
  ...
});
```

## <a name="advanced-scenarios"></a>高度なシナリオ

[`BindableProperty`](xref:Xamarin.Forms.BindableProperty)インスタンスを作成する場合、高度なバインド可能なプロパティシナリオを有効にするために設定できるオプションのパラメーターがいくつかあります。 このセクションでは、これらのシナリオについて説明します。

### <a name="detect-property-changes"></a>プロパティの変更の検出

[`BindableProperty.Create`](xref:Xamarin.Forms.BindableProperty.Create(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate))メソッドの `propertyChanged` パラメーターを指定することによって、`static` プロパティによって変更されたコールバックメソッドをバインド可能なプロパティに登録できます。 指定されたコールバック メソッドは、バインド可能なプロパティの値が変更されたときに呼び出されます。

次のコード例は、`EventName` のバインド可能なプロパティが、`OnEventNameChanged` メソッドをプロパティ変更コールバックメソッドとして登録する方法を示しています。

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

プロパティ変更コールバックメソッドでは、 [`BindableObject`](xref:Xamarin.Forms.BindableObject)パラメーターを使用して、所有しているクラスのインスタンスが変更を報告したことを示し、2つの `object` パラメーターの値は、バインド可能なプロパティの新旧の値を表します。

### <a name="validation-callbacks"></a>検証コールバック

`static` 検証コールバックメソッドは、 [`BindableProperty.Create`](xref:Xamarin.Forms.BindableProperty.Create(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate))メソッドの `validateValue` パラメーターを指定することによって、バインド可能なプロパティで登録できます。 指定されたコールバック メソッドは、バインド可能なプロパティの値が設定されている場合に呼び出されます。

次のコード例は、`Angle` のバインド可能なプロパティが、`IsValidValue` メソッドを検証コールバックメソッドとして登録する方法を示しています。

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

検証コールバックには値が指定されており、値がプロパティに対して有効な場合は `true` を返し、それ以外の場合は `false`を返します。 検証コールバックによって `false`が返されると、例外が発生します。これは、開発者が処理する必要があります。 検証コールバック メソッドの一般的な用途は、バインド可能なプロパティが設定されている場合の整数または倍精度小数点数の値を制約します。 たとえば、`IsValidValue` メソッドは、プロパティ値が 0 ~ 360 の範囲の `double` であることを確認します。

### <a name="coerce-value-callbacks"></a>強制値のコールバック

`static` 強制値のコールバックメソッドは、 [`BindableProperty.Create`](xref:Xamarin.Forms.BindableProperty.Create(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate))メソッドの `coerceValue` パラメーターを指定することによって、バインド可能なプロパティで登録できます。 指定されたコールバック メソッドは、バインド可能なプロパティの値が変更されたときに呼び出されます。

> [!IMPORTANT]
> `BindableObject` 型には、強制値のコールバックを呼び出すことによって、`BindableProperty` 引数の値を強制的に再評価するために呼び出すことができる `CoerceValue` メソッドがあります。

プロパティの値が変更されたときに、バインド可能なプロパティの再評価を強制するコールバックを使用する値を強制します。 たとえば、1 つのバインド可能なプロパティの値が別のバインド可能なプロパティの値を超えていないことを確認する強制値コールバックを使用できます。

次のコード例は、`Angle` のバインド可能なプロパティが、`CoerceAngle` メソッドを強制値のコールバックメソッドとして登録する方法を示しています。

```csharp
public static readonly BindableProperty AngleProperty = BindableProperty.Create (
  "Angle", typeof(double), typeof(HomePage), 0.0, coerceValue: CoerceAngle);
public static readonly BindableProperty MaximumAngleProperty = BindableProperty.Create (
  "MaximumAngle", typeof(double), typeof(HomePage), 360.0, propertyChanged: ForceCoerceValue);
...

static object CoerceAngle (BindableObject bindable, object value)
{
  var homePage = bindable as HomePage;
  double input = (double)value;

  if (input > homePage.MaximumAngle)
  {
    input = homePage.MaximumAngle;
  }
  return input;
}

static void ForceCoerceValue(BindableObject bindable, object oldValue, object newValue)
{
  bindable.CoerceValue(AngleProperty);
}
```

`CoerceAngle` メソッドは `MaximumAngle` プロパティの値をチェックし、`Angle` プロパティ値がそれより大きい場合は、値を `MaximumAngle` プロパティ値に強制します。 また、`MaximumAngle` プロパティが変更されると、`Angle` プロパティで `CoerceValue` メソッドを呼び出すことによって強制値コールバックが呼び出されます。

### <a name="create-a-default-value-with-a-func"></a>Func を使用して既定値を作成する

`Func` は、次のコード例に示すように、バインド可能なプロパティの既定値を初期化するために使用できます。

```csharp
public static readonly BindableProperty SizeProperty =
  BindableProperty.Create ("Size", typeof(double), typeof(HomePage), 0.0,
  defaultValueCreator: bindable => Device.GetNamedSize (NamedSize.Large, (Label)bindable));
```

`defaultValueCreator` パラメーターは、 [`Device.GetNamedSize`](xref:Xamarin.Forms.Device.GetNamedSize(Xamarin.Forms.NamedSize,System.Type))メソッドを呼び出して、ネイティブプラットフォームの[`Label`](xref:Xamarin.Forms.Label)で使用されるフォントの名前付きサイズを表す `double` を返す `Func` に設定されます。

## <a name="related-links"></a>関連リンク

- [XAML 名前空間](~/xamarin-forms/xaml/namespaces.md)
- [イベントからコマンドへの動作 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/behaviors-eventtocommandbehavior)
- [検証コールバック (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-validationcallback)
- [強制値のコールバック (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-coercevaluecallback)
- [BindableProperty API](xref:Xamarin.Forms.BindableProperty)
- [BindableObject API](xref:Xamarin.Forms.BindableObject)
