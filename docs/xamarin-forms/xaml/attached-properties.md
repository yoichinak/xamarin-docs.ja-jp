---
title: アタッチされるプロパティ
description: 添付プロパティは、特殊な種類のバインド可能なプロパティを 1 つのクラスで定義されているが、その他のオブジェクトにアタッチされているを含むフォルダーを属性として XAML で認識可能なクラスおよびプロパティ名、ピリオドで区切られます。 この記事では、アタッチされるプロパティは、の概要について説明し、作成し、それらを使用する方法を示します。
ms.prod: xamarin
ms.assetid: 6E9DCDC3-A0E4-46A6-BAA9-4FEB6DF8A5A8
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 06/02/2016
ms.openlocfilehash: 5c903a39e5569c7ffedfff8eb8e6b0bd4071be9d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="attached-properties"></a>アタッチされるプロパティ

_添付プロパティは、特殊な種類のバインド可能なプロパティを 1 つのクラスで定義されているが、その他のオブジェクトにアタッチされているを含むフォルダーを属性として XAML で認識可能なクラスおよびプロパティ名、ピリオドで区切られます。この記事では、アタッチされるプロパティは、の概要について説明し、作成し、それらを使用する方法を示します。_

## <a name="overview"></a>概要

プロパティを使用する、独自のクラスが定義されていないプロパティの値を割り当てるオブジェクトをアタッチします。 たとえば、子要素を使用できますでは、ユーザー インターフェイスに表示する方法の場合は、その親要素を通知するためにプロパティがアタッチされます。 [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/)コントロールでは、行と列を設定して指定する子の`Grid.Row`と`Grid.Column`アタッチされるプロパティです。 `Grid.Row` および`Grid.Column`アタッチされるプロパティは、の子要素で設定されているため、`Grid`ではなく 、`Grid`自体です。

バインド可能なプロパティは、次のシナリオでの添付プロパティとして実装する必要があります。

- 以外のクラスの使用可能なプロパティ設定メカニズムを用意する必要がある場合にクラス定義です。
- ときに、クラスは、その他のクラスを簡単に統合する必要があるサービスを表します。

バインド可能なプロパティの詳細については、次を参照してください。[バインド可能なプロパティ](~/xamarin-forms/xaml/bindable-properties.md)です。

## <a name="creating-and-consuming-an-attached-property"></a>添付プロパティの作成と

添付プロパティを作成するプロセスは次のとおりです。

1. 作成、 [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/)のいずれかのインスタンス、 [ `CreateAttached` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.CreateAttached/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/)メソッドのオーバー ロードします。
1. 提供`static` `Get` *PropertyName*と`Set` *PropertyName*添付プロパティのアクセサーとしてメソッドです。

### <a name="creating-a-property"></a>プロパティを作成します。

派生するクラスのプロパティが作成される場所がありませんで他の種類を使用するため、添付プロパティを作成するときに[ `BindableObject`](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/)です。 ただし、*ターゲット*アクセサーのプロパティは、するか、派生元[ `BindableObject`](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/)です。

添付プロパティを宣言して作成することができます、`public static readonly`型のプロパティ[ `BindableProperty`](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/)です。 1 つの戻り値にバインド可能なプロパティを設定する必要があります、 [ `BindableProperty.CreateAttached` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.CreateAttached/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/)メソッドのオーバー ロードします。 宣言は、任意のメンバーの定義の外部では、所有するクラスの本体内でする必要があります。

添付プロパティの例を次のコードに示します。

```csharp
public static readonly BindableProperty HasShadowProperty =
  BindableProperty.CreateAttached ("HasShadow", typeof(bool), typeof(ShadowEffect), false);
```

こうと、添付プロパティの名前付き`HasShadow`、型の`bool`します。 プロパティが所有、`ShadowEffect`クラスの既定値は`false`します。 添付プロパティの名前付け規則は、添付プロパティの識別子がで指定されたプロパティ名に一致する必要があります、`CreateAttached`メソッド、"Property"が追加されます。 したがって、上記の例では、添付プロパティの識別子は`HasShadowProperty`します。

作成時に指定できるパラメーターを含む、バインド可能なプロパティの作成の詳細については、次を参照してください。[バインド可能なプロパティの作成と](~/xamarin-forms/xaml/bindable-properties.md#consuming-bindable-property)です。

### <a name="creating-accessors"></a>アクセサーの作成

静的`Get` *PropertyName*と`Set` *PropertyName*メソッドが添付プロパティのアクセサーとして必要、それ以外の場合、プロパティ システムことはできませんを使用する、添付プロパティ。 `Get` *PropertyName*アクセサーは、次のシグネチャに従う必要があります。

```csharp
public static valueType GetPropertyName(BindableObject target)
```

`Get` *PropertyName*アクセサーは、対応する格納されている値を返す必要があります`BindableProperty`添付プロパティのフィールドです。 呼び出すことによってこれを行う、 [ `GetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.GetValue/p/Xamarin.Forms.BindableProperty/)メソッド、値を取得する対象のバインド可能なプロパティの識別子に渡すと、結果の値を必要な型をキャストします。

`Set` *PropertyName*アクセサーは、次のシグネチャに従う必要があります。

```csharp
public static void SetPropertyName(BindableObject target, valueType value)
```

`Set` *PropertyName*アクセサーは対応する値を設定する必要があります`BindableProperty`添付プロパティのフィールドです。 呼び出すことによってこれを行う、 [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/)メソッドを値、および設定する値を設定する対象のバインド可能なプロパティの識別子を渡します。

両方のアクセサーに、*ターゲット*オブジェクトは、するか、派生元[ `BindableObject`](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/)です。

次のコード例のアクセサーを示しています、`HasShadow`添付プロパティ。

```csharp
public static bool GetHasShadow (BindableObject view)
{
  return (bool)view.GetValue (HasShadowProperty);
}

public static void SetHasShadow (BindableObject view, bool value)
{
  view.SetValue (HasShadowProperty, value);
}
```

### <a name="consuming-an-attached-property"></a>添付プロパティの使用

添付プロパティの作成後は、XAML またはコードから使用できます。 XAML では、これは、共通言語ランタイム (CLR) 名前空間名、および必要に応じて、アセンブリ名を示す名前空間の宣言で、プレフィックスを持つ名前空間を宣言することによって実現されます。 詳細については、次を参照してください。 [XAML 名前空間](~/xamarin-forms/xaml/namespaces.md)です。

次のコード例では、カスタムの型を参照しているアプリケーション コードと同じアセンブリ内で定義されている添付プロパティを格納するカスタム型の XAML 名前空間を示しています。

```xaml
<ContentPage ... xmlns:local="clr-namespace:EffectsDemo" ...>
  ...
</ContentPage>
```

次の XAML コードの例に示されている特定のコントロールでとして添付プロパティを設定するときに、名前空間の宣言を使用しています。

```xaml
<Label Text="Label Shadow Effect" local:ShadowEffect.HasShadow="true" />
```

これと同じ C# コードの例は次のとおりです。

```csharp
var label = new Label { Text = "Label Shadow Effect" };
ShadowEffect.SetHasShadow (label, true);
```

### <a name="consuming-an-attached-property-with-a-style"></a>スタイルと添付プロパティの使用

アタッチされるプロパティは、スタイルをコントロールにも追加できます。 XAML コード例を次に、*明示的な*を使用するスタイル、`HasShadow`添付を適用できるプロパティ[ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)コントロール。

```xaml
<Style x:Key="ShadowEffectStyle" TargetType="Label">
  <Style.Setters>
    <Setter Property="local:ShadowEffect.HasShadow" Value="true" />
  </Style.Setters>
</Style>
```

[ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)に適用できる、 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)を設定してその[ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/)プロパティを`Style`を使用してインスタンス`StaticResource`マークアップ拡張機能で、次のコード例で示したようにします。

```xaml
<Label Text="Label Shadow Effect" Style="{StaticResource ShadowEffectStyle}" />
```

スタイルの詳細については、次を参照してください。[スタイル](~/xamarin-forms/user-interface/styles/index.md)です。

## <a name="advanced-scenarios"></a>高度なシナリオ

添付プロパティを作成するときに添付プロパティの高度なシナリオを有効に設定できる省略可能なパラメーターの数があります。 これには、プロパティの変更を検出し、プロパティ値を検証し、プロパティの値の強制型変換が含まれます。 詳細については、次を参照してください。[高度なシナリオ](~/xamarin-forms/xaml/bindable-properties.md#advanced)です。

## <a name="summary"></a>まとめ

この記事では、アタッチされるプロパティは、の概要について説明し、作成し、それらを使用する方法を示しました。 添付プロパティは、特殊な種類のクラスとピリオドで区切ったプロパティ名を含む属性として、他のオブジェクトにアタッチされ、XAML で認識可能な 1 つのクラスで定義されているバインド可能なプロパティです。


## <a name="related-links"></a>関連リンク

- [連結可能プロパティ](~/xamarin-forms/xaml/bindable-properties.md)
- [XAML 名前空間](~/xamarin-forms/xaml/namespaces.md)
- [影付き効果 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/effects/shadoweffect/)
- [BindableProperty](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/)
- [BindableObject](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/)
