---
title: Xamarin.Forms バインド可能なプロパティ
description: この記事では、バインド可能なプロパティの概要と、それらを作成して使用する方法を示します。
ms.prod: xamarin
ms.assetid: 1EE869D8-6FE1-45CA-A0AD-26EC7D032AD7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: df2cf99ef0ea1fcbb1b52dda7abb6c8cfdd2d2e7
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2020
ms.locfileid: "91561535"
---
# <a name="no-locxamarinforms-bindable-properties"></a>Xamarin.Forms バインド可能なプロパティ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/behaviors-eventtocommandbehavior)

バインド可能なプロパティは、プロパティをフィールドでバッキングするのではなく、型を使用してプロパティをバッキングすることによって、CLR プロパティの機能を拡張 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) します。 バインド可能なプロパティの目的は、データバインディング、スタイル、テンプレート、および親子関係を通じて設定される値をサポートするプロパティシステムを提供することです。 また、バインド可能なプロパティは、既定値、プロパティ値の検証、およびプロパティの変更を監視するコールバックを提供できます。

プロパティは、次の機能の1つ以上をサポートするために、バインド可能なプロパティとして実装する必要があります。

- データバインディングの有効な *ターゲット* プロパティとして機能します。
- [スタイル](~/xamarin-forms/user-interface/styles/index.md)を使用してプロパティを設定します。
- プロパティの型の既定値とは異なる既定のプロパティ値を提供します。
- プロパティの値を検証しています。
- プロパティの変更を監視します。

バインド可能なプロパティの例 Xamarin.Forms として、、、などがあり [`Label.Text`](xref:Xamarin.Forms.Label.Text) [`Button.BorderRadius`](xref:Xamarin.Forms.Button.BorderRadius) [`StackLayout.Orientation`](xref:Xamarin.Forms.StackLayout.Orientation) ます。 バインド可能な各プロパティには、 `public static readonly` [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 同じクラスで公開され、バインド可能なプロパティの識別子である、型の対応するフィールドがあります。 たとえば、プロパティの対応するバインド可能なプロパティ id `Label.Text` は [`Label.TextProperty`](xref:Xamarin.Forms.Label.TextProperty) です。

## <a name="create-a-bindable-property"></a>バインド可能なプロパティを作成する

バインド可能なプロパティを作成するプロセスは次のとおりです。

1. [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)メソッドオーバーロードのいずれかを使用してインスタンスを作成 [`BindableProperty.Create`](xref:Xamarin.Forms.BindableProperty.Create*) します。
1. インスタンスのプロパティアクセサーを定義 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) します。

すべての [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) インスタンスは、UI スレッド上に作成する必要があります。 これは、UI スレッドで実行されるコードだけが、バインド可能なプロパティの値を取得または設定できることを意味します。 ただし、 `BindableProperty` メソッドを使用して UI スレッドにマーシャリングすることによって、他のスレッドからインスタンスにアクセスでき [`Device.BeginInvokeOnMainThread`](xref:Xamarin.Forms.Device.BeginInvokeOnMainThread(System.Action)) ます。

### <a name="create-a-property"></a>プロパティを作成する

インスタンスを作成するには `BindableProperty` 、含んでいるクラスがクラスから派生している必要があり [`BindableObject`](xref:Xamarin.Forms.BindableObject) ます。 ただし、クラスは `BindableObject` クラス階層内で高いため、ユーザーインターフェイス機能に使用されるクラスの大部分は、バインド可能なプロパティをサポートしています。

バインド可能なプロパティを作成するには、 `public static readonly` 型のプロパティを宣言し [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) ます。 バインド可能なプロパティは、[ `BindableProperty.Create` ] (xref: のいずれかの戻り値に設定する必要があります Xamarin.Forms 。BindableProperty。 Create (System.string, System.string, System.string, System.object, Xamarin.Forms ...)BindingMode、 Xamarin.Forms 。BindableProperty. ValidateValueDelegate、 Xamarin.Forms 。BindableProperty。 BindingPropertyChangedDelegate、 Xamarin.Forms 。BindableProperty。 Bindingpropertydelegate、 Xamarin.Forms 。BindableProperty. CoerceValueDelegate、 Xamarin.Forms 。BindableProperty. CreateDefaultValueDelegate)) メソッドオーバーロード。 宣言は、派生クラスの本体内で [`BindableObject`](xref:Xamarin.Forms.BindableObject) 、メンバー定義の外部にある必要があります。

少なくとも、次のパラメーターと共に、を作成するときに識別子を指定する必要があり [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) ます。

- の名前 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 。
- プロパティの型。
- 所有しているオブジェクトの型。
- プロパティの既定値。 これにより、プロパティが設定解除されるときに常に特定の既定値が返されるようになります。また、プロパティの型の既定値とは異なる場合があります。 既定値は、[ `ClearValue` ] (xref: Xamarin.Forms .BindableObject。 ClearValue ( Xamarin.Forms .BindableProperty)) メソッドが、バインド可能なプロパティで呼び出されます。

> [!IMPORTANT]
> バインド可能なプロパティの名前付け規則は、バインド可能なプロパティの識別子が、メソッドに指定されたプロパティ名と一致する必要があり `Create` 、"property" が付加されていることです。 

次のコードは、バインド可能なプロパティの例を示しています。この例では、4つの必須パラメーターの識別子と値が使用されています。

```csharp
public static readonly BindableProperty EventNameProperty =
  BindableProperty.Create ("EventName", typeof(string), typeof(EventToCommandBehavior), null);
```

これにより [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 、型のという名前のインスタンスが作成さ `EventNameProperty` `string` れます。 プロパティはクラスによって所有され、 `EventToCommandBehavior` 既定値は `null` です。

必要に応じて、インスタンスを作成するときに、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 次のパラメーターを指定できます。

- バインド モード。 これは、プロパティ値の変更が反映される方向を指定するために使用されます。 既定のバインディングモードでは、変更は *ソース* から *ターゲット*に反映されます。
- プロパティ値が設定されたときに呼び出される検証デリゲート。 詳細については、「 [検証コールバック](#validation-callbacks)」を参照してください。
- プロパティ値が変更されたときに呼び出されるプロパティ変更デリゲート。 詳細については、「 [プロパティの変更の検出](#detect-property-changes)」を参照してください。
- プロパティ値が変更されたときに呼び出されるプロパティ変更デリゲート。 このデリゲートには、プロパティ変更デリゲートと同じシグネチャがあります。
- プロパティ値が変更されたときに呼び出される強制値デリゲート。 詳細については、「 [強制値のコールバック](#coerce-value-callbacks)」を参照してください。
- `Func`既定のプロパティ値を初期化するために使用される。 詳細については、「 [Func を使用した既定値の作成](#create-a-default-value-with-a-func)」を参照してください。

### <a name="create-accessors"></a>アクセサーの作成

プロパティアクセサーは、プロパティの構文を使用して、バインド可能なプロパティにアクセスするために必要です。 アクセサーは、 `Get` 対応するバインド可能なプロパティに格納されている値を返す必要があります。 これは、[ `GetValue` ] (xref: を呼び出すことによって実現できます Xamarin.Forms 。BindableObject。 GetValue ( Xamarin.Forms .BindableProperty) メソッドを使用して、値を取得するバインド可能なプロパティ識別子を渡し、その結果を必要な型にキャストします。 アクセサーは、 `Set` 対応するバインド可能なプロパティの値を設定する必要があります。 これは、[ `SetValue` ] (xref: を呼び出すことによって実現できます Xamarin.Forms 。BindableObject. SetValue ( Xamarin.Forms .BindableProperty, System.object) メソッドを使用して、値を設定するバインド可能なプロパティ識別子と設定する値を渡します。

次のコード例は、バインド可能なプロパティのアクセサーを示してい `EventName` ます。

```csharp
public string EventName
{
  get { return (string)GetValue (EventNameProperty); }
  set { SetValue (EventNameProperty, value); }
}
```

## <a name="consume-a-bindable-property"></a>バインド可能なプロパティを使用する

バインド可能なプロパティが作成されると、XAML またはコードから使用できるようになります。 XAML では、これはプレフィックスを持つ名前空間を宣言することで実現されます。これには、CLR 名前空間の名前を示す名前空間宣言と、必要に応じてアセンブリ名を指定します。 詳細については、「 [XAML 名前空間](~/xamarin-forms/xaml/namespaces.md)」を参照してください。

次のコード例は、バインド可能なプロパティを含むカスタム型の XAML 名前空間を示しています。これは、カスタム型を参照しているアプリケーションコードと同じアセンブリ内で定義されています。

```xaml
<ContentPage ... xmlns:local="clr-namespace:EventToCommandBehavior" ...>
  ...
</ContentPage>
```

名前空間宣言は、 `EventName` 次の XAML コード例に示すように、バインド可能なプロパティを設定するときに使用します。

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

インスタンスを作成するときに [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 、高度なバインド可能なプロパティシナリオを有効にするために設定できるオプションのパラメーターがいくつかあります。 このセクションでは、これらのシナリオについて説明します。

### <a name="detect-property-changes"></a>プロパティの変更の検出

`static`プロパティによって変更されたコールバックメソッドは `propertyChanged` 、[ `BindableProperty.Create` ] (xref: のパラメーターを指定することによって、バインド可能なプロパティで登録できます Xamarin.Forms 。BindableProperty。 Create (System.string, System.string, System.string, System.object, Xamarin.Forms ...)BindingMode、 Xamarin.Forms 。BindableProperty. ValidateValueDelegate、 Xamarin.Forms 。BindableProperty。 BindingPropertyChangedDelegate、 Xamarin.Forms 。BindableProperty。 Bindingpropertydelegate、 Xamarin.Forms 。BindableProperty. CoerceValueDelegate、 Xamarin.Forms 。BindableProperty. CreateDefaultValueDelegate) メソッド。 バインド可能なプロパティの値が変更されると、指定したコールバックメソッドが呼び出されます。

次のコード例は、 `EventName` バインド可能なプロパティが `OnEventNameChanged` メソッドをプロパティ変更コールバックメソッドとして登録する方法を示しています。

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

プロパティ変更コールバックメソッドでは、パラメーターを使用して、所有して [`BindableObject`](xref:Xamarin.Forms.BindableObject) いるクラスのインスタンスが変更を報告したことを示します。2つのパラメーターの値は、バインド可能な `object` プロパティの新旧の値を表します。

### <a name="validation-callbacks"></a>検証コールバック

`static`検証コールバックメソッドは `validateValue` 、[ `BindableProperty.Create` ] (xref: のパラメーターを指定することによって、バインド可能なプロパティで登録できます Xamarin.Forms 。BindableProperty。 Create (System.string, System.string, System.string, System.object, Xamarin.Forms ...)BindingMode、 Xamarin.Forms 。BindableProperty. ValidateValueDelegate、 Xamarin.Forms 。BindableProperty。 BindingPropertyChangedDelegate、 Xamarin.Forms 。BindableProperty。 Bindingpropertydelegate、 Xamarin.Forms 。BindableProperty. CoerceValueDelegate、 Xamarin.Forms 。BindableProperty. CreateDefaultValueDelegate) メソッド。 バインド可能なプロパティの値が設定されると、指定されたコールバックメソッドが呼び出されます。

次のコード例は、バインド可能な `Angle` プロパティが `IsValidValue` メソッドを検証コールバックメソッドとして登録する方法を示しています。

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

検証コールバックには値が指定され、 `true` 値がプロパティに対して有効な場合はを返します。それ以外の場合はを返し `false` ます。 検証コールバックから制御が戻ると、例外が発生し `false` ます。これは、開発者が処理する必要があります。 検証コールバックメソッドの一般的な使用方法は、バインド可能なプロパティが設定されている場合に整数または倍精度浮動小数点数の値を制限することです。 たとえば、メソッドは、 `IsValidValue` プロパティ値が `double` 0 ~ 360 の範囲内であることを確認します。

### <a name="coerce-value-callbacks"></a>強制値のコールバック

`static`強制値のコールバックメソッドは `coerceValue` 、[ `BindableProperty.Create` ] (xref: のパラメーターを指定することによって、バインド可能なプロパティで登録できます Xamarin.Forms 。BindableProperty。 Create (System.string, System.string, System.string, System.object, Xamarin.Forms ...)BindingMode、 Xamarin.Forms 。BindableProperty. ValidateValueDelegate、 Xamarin.Forms 。BindableProperty。 BindingPropertyChangedDelegate、 Xamarin.Forms 。BindableProperty。 Bindingpropertydelegate、 Xamarin.Forms 。BindableProperty. CoerceValueDelegate、 Xamarin.Forms 。BindableProperty. CreateDefaultValueDelegate) メソッド。 バインド可能なプロパティの値が変更されると、指定したコールバックメソッドが呼び出されます。

> [!IMPORTANT]
> `BindableObject`型には、 `CoerceValue` `BindableProperty` 強制値コールバックを呼び出すことによって、引数の値を強制的に再評価するために呼び出すことができるメソッドがあります。

強制値コールバックは、プロパティの値が変更されたときに、バインド可能なプロパティを強制的に再評価するために使用されます。 たとえば、強制値のコールバックを使用すると、1つのバインド可能なプロパティの値が、別のバインド可能なプロパティの値を超えないようにすることができます。

次のコード例は、 `Angle` バインド可能なプロパティが `CoerceAngle` メソッドを強制値のコールバックメソッドとして登録する方法を示しています。

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

メソッドは、プロパティ `CoerceAngle` の値をチェックし `MaximumAngle` `Angle` ます。プロパティ値がそれより大きい場合は、値をプロパティ値に強制的に変換し `MaximumAngle` ます。 さらに、プロパティが変更された場合、 `MaximumAngle` `Angle` メソッドを呼び出すことによって、プロパティに対して強制値コールバックが呼び出され `CoerceValue` ます。

### <a name="create-a-default-value-with-a-func"></a>Func を使用して既定値を作成する

は、 `Func` 次のコード例に示すように、バインド可能なプロパティの既定値を初期化するために使用できます。

```csharp
public static readonly BindableProperty SizeProperty =
  BindableProperty.Create ("Size", typeof(double), typeof(HomePage), 0.0,
  defaultValueCreator: bindable => Device.GetNamedSize (NamedSize.Large, (Label)bindable));
```

`defaultValueCreator`パラメーターは、 `Func` [ `Device.GetNamedSize` ] (xref: を呼び出すに設定されます Xamarin.Forms 。デバイス. GetNamedSize ( Xamarin.Forms .NamedSize, system.string) メソッドを返します。このメソッドは、 `double` ネイティブプラットフォームので使用されるフォントの名前付きサイズを表すを返し [`Label`](xref:Xamarin.Forms.Label) ます。

## <a name="related-links"></a>関連リンク

- [XAML 名前空間](~/xamarin-forms/xaml/namespaces.md)
- [イベントからコマンドへの動作 (サンプル)](/samples/xamarin/xamarin-forms-samples/behaviors-eventtocommandbehavior)
- [検証コールバック (サンプル)](/samples/xamarin/xamarin-forms-samples/xaml-validationcallback)
- [強制値のコールバック (サンプル)](/samples/xamarin/xamarin-forms-samples/xaml-coercevaluecallback)
- [BindableProperty API](xref:Xamarin.Forms.BindableProperty)
- [BindableObject API](xref:Xamarin.Forms.BindableObject)