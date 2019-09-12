---
title: 再利用可能な EffectBehavior
description: ビヘイビアーは、コントロールにエフェクトを追加するために役立つ方法です。エフェクトを処理する定型コードを分離コード ファイルから削除します。 この記事では、Xamarin.Forms のビヘイビアーを作成および使用して、コントロールにエフェクトを追加する方法を示します。
ms.prod: xamarin
ms.assetid: A909B24D-960A-4023-AFF6-4B9256C55ADD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: ca03dce3bd39664a07b7bf56d22d7c2e000e931f
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70771999"
---
# <a name="reusable-effectbehavior"></a>再利用可能な EffectBehavior

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/behaviors-effectbehavior)

_ビヘイビアーは、コントロールにエフェクトを追加するために役立つ方法です。エフェクトを処理する定型コードを分離コード ファイルから削除します。この記事では、Xamarin.Forms のビヘイビアーを作成および使用して、コントロールにエフェクトを追加する方法を示します。_

## <a name="overview"></a>概要

`EffectBehavior` クラスは再利用可能な Xamarin.Forms カスタム ビヘイビアーであり、このビヘイビアーがコントロールにアタッチされるとき、コントロールに [`Effect`](xref:Xamarin.Forms.Effect) インスタンスを追加し、コントロールからデタッチされるとき、`Effect` インスタンスを削除します。

ビヘイビアーを使用するには、次のビヘイビアー プロパティを設定する必要があります。

- **グループ** – エフェクト クラスの [`ResolutionGroupName`](xref:Xamarin.Forms.ResolutionGroupNameAttribute) 属性の値。
- **名前** – エフェクト クラスの [`ExportEffect`](xref:Xamarin.Forms.ExportEffectAttribute) 属性の値。

エフェクトの詳細については、[エフェクト](~/xamarin-forms/app-fundamentals/effects/index.md)に関するページを参照してください。

> [!NOTE]
> `EffectBehavior` は、[エフェクト ビヘイビアー サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/behaviors-effectbehavior)に配置できるカスタム クラスであり、Xamarin.Forms の一部ではありません。

## <a name="creating-the-behavior"></a>ビヘイビアーの作成

`EffectBehavior` クラスは [`Behavior<T>`](xref:Xamarin.Forms.Behavior`1) クラスから派生します。`T` は [`View`](xref:Xamarin.Forms.View) です。 つまり、`EffectBehavior` クラスはあらゆる Xamarin.Forms コントロールにアタッチできます。

### <a name="implementing-bindable-properties"></a>バインド可能プロパティの実装

`EffectBehavior` クラスによって 2 つの [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) インスタンスが定義されます。これはビヘイビアーがコントロールにアタッチされるときにコントロールに [`Effect`](xref:Xamarin.Forms.Effect) を追加する目的で使用されます。 これらのプロパティを次のコード例に示します。

```csharp
public class EffectBehavior : Behavior<View>
{
  public static readonly BindableProperty GroupProperty =
    BindableProperty.Create ("Group", typeof(string), typeof(EffectBehavior), null);
  public static readonly BindableProperty NameProperty =
    BindableProperty.Create ("Name", typeof(string), typeof(EffectBehavior), null);

  public string Group {
    get { return (string)GetValue (GroupProperty); }
    set { SetValue (GroupProperty, value); }
  }

  public string Name {
    get { return(string)GetValue (NameProperty); }
    set { SetValue (NameProperty, value); }
  }
  ...
}
```

`EffectBehavior` が使用されるとき、`Group` プロパティをエフェクトの [`ResolutionGroupName`](xref:Xamarin.Forms.ResolutionGroupNameAttribute) 属性の値に設定する必要があります。 また、`Name` プロパティをエフェクト クラスの [`ExportEffect`](xref:Xamarin.Forms.ExportEffectAttribute) 属性の値に設定する必要があります。

### <a name="implementing-the-overrides"></a>オーバーライドの実装

次のコード例に示すように、[`Behavior<T>`](xref:Xamarin.Forms.Behavior`1) クラスの [`OnAttachedTo`](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) メソッドと [`OnDetachingFrom`](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) メソッドは `EffectBehavior` クラスによってオーバーライドされます。

```csharp
public class EffectBehavior : Behavior<View>
{
  ...
  protected override void OnAttachedTo (BindableObject bindable)
  {
    base.OnAttachedTo (bindable);
    AddEffect (bindable as View);
  }

  protected override void OnDetachingFrom (BindableObject bindable)
  {
    RemoveEffect (bindable as View);
    base.OnDetachingFrom (bindable);
  }
  ...
}
```

[`OnAttachedTo`](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) メソッドでは、`AddEffect` メソッドを呼び出してセットアップが実行され、アタッチされているコントロールがパラメーターとして渡されます。 [`OnDetachingFrom`](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) メソッドでは、`RemoveEffect` メソッドを呼び出してクリーンアップが実行され、アタッチされているコントロールがパラメーターとして渡されます。

### <a name="implementing-the-behavior-functionality"></a>ビヘイビアー機能の実装

ビヘイビアーの目的は、ビヘイビアーがコントロールにアタッチされるとき、`Group` プロパティと `Name` プロパティで定義されている [`Effect`](xref:Xamarin.Forms.Effect) をコントロールに追加し、ビヘイビアーがコントロールからデタッチされるとき、`Effect` を削除することです。 ビヘイビアーの主な機能を次のコード例に示します。

```csharp
public class EffectBehavior : Behavior<View>
{
  ...
  void AddEffect (View view)
  {
    var effect = GetEffect ();
    if (effect != null) {
      view.Effects.Add (GetEffect ());
    }
  }

  void RemoveEffect (View view)
  {
    var effect = GetEffect ();
    if (effect != null) {
      view.Effects.Remove (GetEffect ());
    }
  }

  Effect GetEffect ()
  {
    if (!string.IsNullOrWhiteSpace (Group) && !string.IsNullOrWhiteSpace (Name)) {
      return Effect.Resolve (string.Format ("{0}.{1}", Group, Name));
    }
    return null;
  }
}
```

コントロールにアタッチされている `EffectBehavior` に応答して `AddEffect` メソッドが実行されます。このメソッドでは、アタッチされているコントロールがパラメーターとして受け取られます。 次に、このメソッドによって、取得したエフェクトがコントロールの [`Effects`](xref:Xamarin.Forms.Element.Effects) コレクションに追加されます。 コントロールからデタッチされている `EffectBehavior` に応答して `RemoveEffect` メソッドが実行されます。このメソッドでは、アタッチされているコントロールがパラメーターとして受け取られます。 次に、このメソッドによって、コントロールの [`Effects`](xref:Xamarin.Forms.Element.Effects) コレクションからエフェクトが削除されます。

`GetEffect` メソッドでは、[`Effect.Resolve`](xref:Xamarin.Forms.Effect.Resolve(System.String)) メソッドを使用して [`Effect`](xref:Xamarin.Forms.Effect) が取得されます。 エフェクトは、`Group` プロパティ値と `Name` プロパティ値の連結によって配置されます。 プラットフォームでエフェクトが提供されない場合、`Effect.Resolve` メソッドでは、`null` 以外の値が返されます。

## <a name="consuming-the-behavior"></a>ビヘイビアーの使用

次の XAML コード例に示すように、コントロールの [`Behaviors`](xref:Xamarin.Forms.VisualElement.Behaviors) コレクションに `EffectBehavior` クラスをアタッチできます。

```xaml
<Label Text="Label Shadow Effect" ...>
  <Label.Behaviors>
    <local:EffectBehavior Group="Xamarin" Name="LabelShadowEffect" />
  </Label.Behaviors>
</Label>
```

これと同じ C# コードの例は次のとおりです。

```csharp
var label = new Label {
  Text = "Label Shadow Effect",
  ...
};
label.Behaviors.Add (new EffectBehavior {
  Group = "Xamarin",
  Name = "LabelShadowEffect"
});
```

ビヘイビアーの `Group` プロパティと `Name` プロパティは、プラットフォーム固有のプロジェクトごとに、エフェクト クラスの [`ResolutionGroupName`](xref:Xamarin.Forms.ResolutionGroupNameAttribute) 属性と [`ExportEffect`](xref:Xamarin.Forms.ExportEffectAttribute) 属性の値に設定されます。

実行時、ビヘイビアーが [`Label`](xref:Xamarin.Forms.Label) コントロールにアタッチされていると、コントロールの [`Effects`](xref:Xamarin.Forms.Element.Effects) コレクションに `Xamarin.LabelShadowEffect` が追加されます。 これにより、次のスクリーンショットに示すように、`Label` コントロールによって表示されるテキストにシャドウが追加されます。

![](effect-behavior-images/screenshots.png "EffectsBehavior を含むサンプル アプリケーション")

このビヘイビアーを使用してコントロールとの間でエフェクトを追加/削除することの長所は、エフェクトを処理する定型コードを分離コード ファイルから削除できることです。

## <a name="summary"></a>まとめ

この記事では、ビヘイビアーを使用してエフェクトをコントロールに追加しました。 `EffectBehavior` クラスは再利用可能な Xamarin.Forms カスタム ビヘイビアーであり、このビヘイビアーがコントロールにアタッチされるとき、コントロールに [`Effect`](xref:Xamarin.Forms.Effect) インスタンスを追加し、コントロールからデタッチされるとき、`Effect` インスタンスを削除します。

## <a name="related-links"></a>関連リンク

- [エフェクト](~/xamarin-forms/app-fundamentals/effects/index.md)
- [エフェクト ビヘイビアー (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/behaviors-effectbehavior)
- [Behavior](xref:Xamarin.Forms.Behavior)
- [Behavior&lt;T&gt;](xref:Xamarin.Forms.Behavior`1)
