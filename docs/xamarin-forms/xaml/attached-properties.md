---
title: 添付プロパティ
description: この記事では、添付プロパティは、概要を示し、作成し、これらを使用する方法を示します。
ms.prod: xamarin
ms.assetid: 6E9DCDC3-A0E4-46A6-BAA9-4FEB6DF8A5A8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/02/2016
ms.openlocfilehash: 78dd2d3a63cd0e2b6ab1e6876dd82f49f5580f0b
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75489974"
---
# <a name="attached-properties"></a>添付プロパティ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-shadoweffect)


プロパティを使用する独自のクラスが定義されていないプロパティの値を代入するオブジェクトをアタッチします。 たとえば、子要素を使用できますには、ユーザー インターフェイスで表示するのには、親要素に通知するプロパティがアタッチされています。 [ `Grid` ](xref:Xamarin.Forms.Grid)行、列を設定して指定する子コントロールを使用できます、`Grid.Row`と`Grid.Column`添付プロパティ。 `Grid.Row` `Grid.Column`添付プロパティは、要素の子に設定されているため、`Grid`ではなくで、`Grid`自体。

バインド可能なプロパティは、次のシナリオで添付プロパティとして実装する必要があります。

- 以外のクラスの使用可能なプロパティ設定機構を用意する必要がある場合に、定義するクラスします。
- ときに、クラスは、他のクラスと簡単に統合する必要があるサービスを表します。

バインド可能なプロパティの詳細については、次を参照してください。[バインド可能なプロパティ](~/xamarin-forms/xaml/bindable-properties.md)します。

## <a name="create-an-attached-property"></a>添付プロパティを作成する

添付プロパティを作成するプロセスは次のとおりです。

1. 作成、 [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty)のいずれかのインスタンス、 [ `CreateAttached` ](xref:Xamarin.Forms.BindableProperty.CreateAttached*)メソッドのオーバー ロードします。
1. 提供`static` `Get` *PropertyName*と`Set` *PropertyName*添付プロパティのアクセサーとしてメソッド。

### <a name="create-a-property"></a>プロパティを作成する

派生するプロパティが作成されているクラスがない他の種類で使用するため、添付プロパティを作成するときに[ `BindableObject`](xref:Xamarin.Forms.BindableObject)します。 ただし、*ターゲット*のアクセサー プロパティは、するか、派生元の[ `BindableObject`](xref:Xamarin.Forms.BindableObject)します。

添付プロパティを宣言することで作成できる、`public static readonly`型のプロパティ[ `BindableProperty`](xref:Xamarin.Forms.BindableProperty)します。 1 つの戻り値にバインド可能なプロパティを設定する必要があります、 [ `BindableProperty.CreateAttached` ](xref:Xamarin.Forms.BindableProperty.CreateAttached(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate))メソッドのオーバー ロードします。 任意のメンバーの定義の外部で、所有元のクラスの本体内で宣言があります。

次のコードでは、添付プロパティの例を示します。

```csharp
public static readonly BindableProperty HasShadowProperty =
  BindableProperty.CreateAttached ("HasShadow", typeof(bool), typeof(ShadowEffect), false);
```

これは、という名前の添付プロパティを作成します。 `HasShadow`、型の`bool`します。 プロパティが所有、`ShadowEffect`クラスし、の既定値を持つ`false`します。 添付プロパティの名前付け規則では、添付プロパティの識別子がで指定されたプロパティ名に一致する必要があります、`CreateAttached`メソッドは、"Property"が追加されます。 そのため、上記の例では、添付プロパティの識別子は`HasShadowProperty`します。

作成時に指定できるパラメーターなど、バインド可能なプロパティの作成の詳細については、「[バインド可能なプロパティの作成](~/xamarin-forms/xaml/bindable-properties.md#consume-a-bindable-property)」を参照してください。

### <a name="create-accessors"></a>アクセサーの作成

静的`Get` *PropertyName*と`Set` *PropertyName*添付プロパティのアクセサーとして必要なメソッドは、それ以外の場合、プロパティ システムことはできませんを使用する、添付プロパティ。 `Get` *PropertyName*アクセサーは、次のシグネチャに従う必要があります。

```csharp
public static valueType GetPropertyName(BindableObject target)
```

`Get` *PropertyName*アクセサーは、対応する格納されている値を返す必要があります`BindableProperty`添付プロパティのフィールド。 これは、呼び出すことによって実現できます、 [ `GetValue` ](xref:Xamarin.Forms.BindableObject.GetValue(Xamarin.Forms.BindableProperty))メソッドを値を取得するバインド可能なプロパティの識別子を渡すし、結果の値を必要な型にキャストします。

`Set` *PropertyName*アクセサーは、次のシグネチャに従う必要があります。

```csharp
public static void SetPropertyName(BindableObject target, valueType value)
```

`Set` *PropertyName*アクセサーは、対応する値を設定する必要があります`BindableProperty`添付プロパティのフィールド。 これは、呼び出すことによって実現できます、 [ `SetValue` ](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object))値、および設定する値を設定する対象のバインド可能なプロパティの識別子を渡す方法です。

両方のアクセサーに、*ターゲット*オブジェクトは、するか、派生元の[ `BindableObject`](xref:Xamarin.Forms.BindableObject)します。

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

### <a name="consume-an-attached-property"></a>添付プロパティを使用する

添付プロパティが作成されると、XAML またはコードから使用できます。 XAML では、これは、共通言語ランタイム (CLR) 名前空間の名前および必要に応じて、アセンブリ名を示す名前空間宣言で、プレフィックスを持つ名前空間を宣言することによって実現されます。 詳細については、次を参照してください。 [XAML 名前空間](~/xamarin-forms/xaml/namespaces.md)します。

次のコード例では、カスタムの型を参照しているアプリケーション コードと同じアセンブリ内で定義されている添付プロパティを含むカスタム型の XAML 名前空間を示しています。

```xaml
<ContentPage ... xmlns:local="clr-namespace:EffectsDemo" ...>
  ...
</ContentPage>
```

次の XAML コード例に示されている特定のコントロールに接続されているプロパティを設定するときは、名前空間宣言を使用しています。

```xaml
<Label Text="Label Shadow Effect" local:ShadowEffect.HasShadow="true" />
```

これと同じ C# コードの例は次のとおりです。

```csharp
var label = new Label { Text = "Label Shadow Effect" };
ShadowEffect.SetHasShadow (label, true);
```

### <a name="consume-an-attached-property-with-a-style"></a>添付プロパティをスタイルで使用する

添付プロパティは、スタイルでコントロールにも追加できます。 次の XAML コード例は、*明示的な*を使用するスタイル、`HasShadow`添付、プロパティに適用できる[ `Label` ](xref:Xamarin.Forms.Label)コントロール。

```xaml
<Style x:Key="ShadowEffectStyle" TargetType="Label">
  <Style.Setters>
    <Setter Property="local:ShadowEffect.HasShadow" Value="true" />
  </Style.Setters>
</Style>
```

[`Style`](xref:Xamarin.Forms.Style) は、次のコード例に示すように、`StaticResource` マークアップ拡張を使用して自身の [`Style`](xref:Xamarin.Forms.NavigableElement.Style) プロパティを `Style` インスタンスに設定することで、[`Label`](xref:Xamarin.Forms.Label) に適用できます。

```xaml
<Label Text="Label Shadow Effect" Style="{StaticResource ShadowEffectStyle}" />
```

スタイルについて詳しくは、[スタイル](~/xamarin-forms/user-interface/styles/index.md)に関する記事をご覧ください。

## <a name="advanced-scenarios"></a>高度なシナリオ

添付プロパティを作成するときに、さまざまな添付プロパティの高度なシナリオを有効に設定できる省略可能なパラメーターがあります。 これには、プロパティの変更を検出し、プロパティの値を検証し、プロパティ値の強制型変換が含まれます。 詳細については、「[高度なシナリオ](~/xamarin-forms/xaml/bindable-properties.md#advanced-scenarios)」を参照してください。

## <a name="related-links"></a>関連リンク

- [連結可能プロパティ](~/xamarin-forms/xaml/bindable-properties.md)
- [XAML 名前空間](~/xamarin-forms/xaml/namespaces.md)
- [影エフェクト (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-shadoweffect)
- [BindableProperty API](xref:Xamarin.Forms.BindableProperty)
- [BindableObject API](xref:Xamarin.Forms.BindableObject)
