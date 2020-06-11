---
title: "添付プロパティ" の説明: "この記事では、添付プロパティの概要を示し、それらを作成して使用する方法を示します。"
ms. 製品: xamarin ms. assetid: 6E9DCDC3-A0E4-46A6-BAA9-4FEB6DF8A5A8: xamarin-forms author: davidbritch ms. author: dabritch ms. date: 06/02/2016 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="attached-properties"></a>添付プロパティ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-shadoweffect)


添付プロパティを使用すると、オブジェクトは、独自のクラスで定義されていないプロパティの値を割り当てることができます。 たとえば、子要素は添付プロパティを使用して、ユーザーインターフェイスにどのように表示されるかを親要素に知らせることができます。 コントロールでは、 [`Grid`](xref:Xamarin.Forms.Grid) `Grid.Row` プロパティと添付プロパティを設定することによって、子の行と列を指定でき `Grid.Column` ます。 `Grid.Row`と `Grid.Column` は、 `Grid` それ自体ではなく、の子である要素に設定されるため、添付プロパティです `Grid` 。

バインド可能なプロパティは、次のシナリオで添付プロパティとして実装する必要があります。

- 定義クラス以外のクラスに対してプロパティ設定機構を使用できるようにする必要がある場合。
- クラスが、他のクラスと簡単に統合する必要があるサービスを表す場合。

バインド可能なプロパティの詳細については、「バインド可能な[プロパティ](~/xamarin-forms/xaml/bindable-properties.md)」を参照してください。

## <a name="create-an-attached-property"></a>添付プロパティを作成する

添付プロパティを作成するプロセスは次のとおりです。

1. [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)メソッドオーバーロードのいずれかを使用してインスタンスを作成 [`CreateAttached`](xref:Xamarin.Forms.BindableProperty.CreateAttached*) します。
1. `static` `Get` *Propertyname* `Set` メソッドと*propertyname*メソッドを添付プロパティのアクセサーとして指定します。

### <a name="create-a-property"></a>プロパティを作成する

他の型で使用する添付プロパティを作成する場合、プロパティを作成するクラスはから派生する必要がありません [`BindableObject`](xref:Xamarin.Forms.BindableObject) 。 ただし、アクセサーの*ターゲット*プロパティは、またはから派生している必要があり [`BindableObject`](xref:Xamarin.Forms.BindableObject) ます。

添付プロパティを作成するには、 `public static readonly` 型のプロパティを宣言し [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) ます。 バインド可能なプロパティは、[ `BindableProperty.CreateAttached` ] (xref: のいずれかの戻り値に設定する必要があります Xamarin.Forms 。BindableProperty. CreateAttached られた (System.string, System.string, System.string, Xamarin.Forms system.object,)BindingMode、 Xamarin.Forms 。BindableProperty. ValidateValueDelegate、 Xamarin.Forms 。BindableProperty。 BindingPropertyChangedDelegate、 Xamarin.Forms 。BindableProperty。 Bindingpropertydelegate、 Xamarin.Forms 。BindableProperty. CoerceValueDelegate、 Xamarin.Forms 。BindableProperty. CreateDefaultValueDelegate)) メソッドオーバーロード。 宣言は、所有しているクラスの本体内で、メンバー定義の外部にある必要があります。

添付プロパティの例を次のコードに示します。

```csharp
public static readonly BindableProperty HasShadowProperty =
  BindableProperty.CreateAttached ("HasShadow", typeof(bool), typeof(ShadowEffect), false);
```

これにより、型のという名前の添付プロパティが作成され `HasShadow` `bool` ます。 プロパティはクラスによって所有され、 `ShadowEffect` 既定値は `false` です。 添付プロパティの名前付け規則は、添付プロパティの識別子が、メソッドに指定されたプロパティ名と一致する必要があり `CreateAttached` 、"property" が追加されていることです。 したがって、上記の例では、添付プロパティの識別子は `HasShadowProperty` です。

作成時に指定できるパラメーターなど、バインド可能なプロパティの作成の詳細については、「[バインド可能なプロパティの作成](~/xamarin-forms/xaml/bindable-properties.md#consume-a-bindable-property)」を参照してください。

### <a name="create-accessors"></a>アクセサーの作成

Static `Get` *PropertyName*および `Set` *propertyname*メソッドは添付プロパティのアクセサーとして必要です。それ以外の場合、プロパティシステムは添付プロパティを使用できません。 `Get` *PropertyName*アクセサーは、次のシグネチャに準拠している必要があります。

```csharp
public static valueType GetPropertyName(BindableObject target)
```

`Get` *PropertyName*アクセサーは、添付プロパティの対応するフィールドに格納されている値を返す必要があり `BindableProperty` ます。 これは、[ `GetValue` ] (xref: を呼び出すことによって実現できます Xamarin.Forms 。BindableObject。 GetValue ( Xamarin.Forms .BindableProperty) メソッドを使用して、値を取得するバインド可能なプロパティ識別子を渡し、その結果の値を必要な型にキャストします。

`Set` *PropertyName*アクセサーは、次のシグネチャに準拠している必要があります。

```csharp
public static void SetPropertyName(BindableObject target, valueType value)
```

`Set` *PropertyName*アクセサーは、添付プロパティの対応するフィールドの値を設定する必要があり `BindableProperty` ます。 これは、[ `SetValue` ] (xref: を呼び出すことによって実現できます Xamarin.Forms 。BindableObject. SetValue ( Xamarin.Forms .BindableProperty, System.object) メソッドを使用して、値を設定するバインド可能なプロパティ識別子と設定する値を渡します。

両方のアクセサーでは、*ターゲット*オブジェクトはであるか、またはから派生している必要があり [`BindableObject`](xref:Xamarin.Forms.BindableObject) ます。

次のコード例は、添付プロパティのアクセサーを示してい `HasShadow` ます。

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

添付プロパティが作成されると、XAML またはコードから使用できるようになります。 XAML では、これはプレフィックスを持つ名前空間を宣言することで実現されます。これには、共通言語ランタイム (CLR) の名前空間名を示す名前空間宣言と、必要に応じてアセンブリ名を指定します。 詳細については、「 [XAML 名前空間](~/xamarin-forms/xaml/namespaces.md)」を参照してください。

次のコード例は、添付プロパティを含むカスタム型の XAML 名前空間を示しています。これは、カスタム型を参照しているアプリケーションコードと同じアセンブリ内で定義されています。

```xaml
<ContentPage ... xmlns:local="clr-namespace:EffectsDemo" ...>
  ...
</ContentPage>
```

次の XAML コード例に示すように、特定のコントロールで添付プロパティを設定するときに、名前空間宣言が使用されます。

```xaml
<Label Text="Label Shadow Effect" local:ShadowEffect.HasShadow="true" />
```

これと同じ C# コードの例は次のとおりです。

```csharp
var label = new Label { Text = "Label Shadow Effect" };
ShadowEffect.SetHasShadow (label, true);
```

### <a name="consume-an-attached-property-with-a-style"></a>添付プロパティをスタイルで使用する

添付プロパティは、スタイルによってコントロールに追加することもできます。 次の XAML コード例は、コントロールに適用できる添付プロパティを使用する*明示的*なスタイルを示してい `HasShadow` [`Label`](xref:Xamarin.Forms.Label) ます。

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

スタイルについて詳しくは、「[Styles](~/xamarin-forms/user-interface/styles/index.md)」(スタイル) をご覧ください。

## <a name="advanced-scenarios"></a>高度なシナリオ

添付プロパティを作成するときに、添付プロパティの高度なシナリオを有効にするために設定できるオプションのパラメーターがいくつかあります。 これには、プロパティの変更の検出、プロパティ値の検証、プロパティ値の強制型変換が含まれます。 詳細については、「[高度なシナリオ](~/xamarin-forms/xaml/bindable-properties.md#advanced-scenarios)」を参照してください。

## <a name="related-links"></a>関連リンク

- [連結可能プロパティ](~/xamarin-forms/xaml/bindable-properties.md)
- [XAML 名前空間](~/xamarin-forms/xaml/namespaces.md)
- [影エフェクト (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-shadoweffect)
- [BindableProperty API](xref:Xamarin.Forms.BindableProperty)
- [BindableObject API](xref:Xamarin.Forms.BindableObject)
