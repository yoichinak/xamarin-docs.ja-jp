---
title: 再利用可能な EffectBehavior
description: ビヘイビアーとは、処理の分離コード ファイルからコード ボイラー プレート効果を削除するコントロールに効果を追加するために役立つアプローチです。 この記事では、コントロールに効果を追加する Xamarin.Forms の動作を使用してを示します。
ms.prod: xamarin
ms.assetid: A909B24D-960A-4023-AFF6-4B9256C55ADD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 1ce7eda6f556041cbffc3793b00e8e2cba44d3d0
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995782"
---
# <a name="reusable-effectbehavior"></a>再利用可能な EffectBehavior

_ビヘイビアーとは、処理の分離コード ファイルからコード ボイラー プレート効果を削除するコントロールに効果を追加するために役立つアプローチです。この記事では、コントロールに効果を追加する Xamarin.Forms の動作を使用してを示します。_

## <a name="overview"></a>概要

`EffectBehavior`クラスは、再利用可能な Xamarin.Forms カスタム動作を追加する、 [ `Effect` ](xref:Xamarin.Forms.Effect)動作がコントロールにアタッチされているし、削除するときのコントロールのインスタンス、`Effect`インスタンスの場合、動作は、コントロールからデタッチします。

動作を使用する次の動作のプロパティを設定する必要があります。

- **グループ**– の値、 [ `ResolutionGroupName` ](xref:Xamarin.Forms.ResolutionGroupNameAttribute)効果クラスの属性。
- **名前**– の値、 [ `ExportEffect` ](xref:Xamarin.Forms.ExportEffectAttribute)効果クラスの属性。

影響の詳細については、次を参照してください。[効果](~/xamarin-forms/app-fundamentals/effects/index.md)します。

## <a name="creating-the-behavior"></a>動作を作成します。

`EffectBehavior`クラスから派生、 [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1)クラス、`T`は、 [ `View`](xref:Xamarin.Forms.View)します。 つまり、`EffectBehavior`クラスは、Xamarin.Forms コントロールに関連付けることができます。

### <a name="implementing-bindable-properties"></a>バインド可能なプロパティを実装します。

`EffectBehavior`クラスは、2 つ定義[ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty)インスタンスで、追加するために使用、 [ `Effect` ](xref:Xamarin.Forms.Effect)動作がコントロールに関連付けられている場合、コントロールにします。 これらのプロパティは次のコード例に示します。

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

ときに、`EffectBehavior`が使用されて、`Group`プロパティの値に設定する必要があります、 [ `ResolutionGroupName` ](xref:Xamarin.Forms.ResolutionGroupNameAttribute)効果の属性。 さらに、`Name`プロパティの値に設定する必要があります、 [ `ExportEffect` ](xref:Xamarin.Forms.ExportEffectAttribute)効果クラスの属性。

### <a name="implementing-the-overrides"></a>オーバーライドを実装します。

`EffectBehavior`オーバーライド、 [ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject))と[ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject))のメソッド、 [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1)次のコードに示すように、クラス例:

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

[ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject))メソッドを呼び出してセットアップを実行します、`AddEffect`メソッド パラメーターとして接続されているコントロール。 [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject))メソッドを呼び出してクリーンアップを実行します、`RemoveEffect`メソッド パラメーターとして接続されているコントロール。

### <a name="implementing-the-behavior-functionality"></a>動作の機能を実装します。

動作の目的は、追加する、 [ `Effect` ](xref:Xamarin.Forms.Effect)で定義されている、`Group`と`Name`動作がコントロールに接続され、削除を制御するプロパティが、`Effect`動作の場合コントロールからデタッチします。 動作のコア機能は、次のコード例に示されます。

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

`AddEffect`への応答でメソッドが実行される、`EffectBehavior`パラメーターとして接続されているコントロールを受信して、コントロールにアタッチされます。 メソッドは、コントロールの取得の効果を追加する、 [ `Effects` ](xref:Xamarin.Forms.Element.Effects)コレクション。 `RemoveEffect`への応答でメソッドが実行される、`EffectBehavior`パラメーターとして接続されているコントロールを受け取ると、コントロールからデタッチされています。 メソッドは、コントロールから効果を削除する、 [ `Effects` ](xref:Xamarin.Forms.Element.Effects)コレクション。

`GetEffect`メソッドは、 [ `Effect.Resolve` ](xref:Xamarin.Forms.Effect.Resolve(System.String))を取得するメソッド、 [ `Effect`](xref:Xamarin.Forms.Effect)します。 この効果の連結を使用して検索、`Group`と`Name`プロパティの値。 プラットフォームは、効果に備わっていない場合、`Effect.Resolve`メソッドは以外`null`値。

## <a name="consuming-the-behavior"></a>動作の使用

`EffectBehavior`クラスに接続できる、 [ `Behaviors` ](xref:Xamarin.Forms.VisualElement.Behaviors)次の XAML コード例のように、コントロールのコレクション。

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

`Group`と`Name`動作のプロパティの値に設定されます、 [ `ResolutionGroupName` ](xref:Xamarin.Forms.ResolutionGroupNameAttribute)と[ `ExportEffect` ](xref:Xamarin.Forms.ExportEffectAttribute)効果クラスは、各プラットフォームに固有の属性プロジェクトです。

動作が関連付けられている場合、実行時に、 [ `Label` ](xref:Xamarin.Forms.Label)コントロール、`Xamarin.LabelShadowEffect`をコントロールの追加は[ `Effects` ](xref:Xamarin.Forms.Element.Effects)コレクション。 によって表示されるテキストに追加されるシャドウでこの結果、`Label`コントロールが次のスクリーン ショットに示すようにします。

![](effect-behavior-images/screenshots.png "EffectsBehavior でサンプル アプリケーション")

この動作を追加およびコントロールから効果を削除するを使用する利点は、ボイラー プレート効果処理コードを分離コード ファイルから削除できることです。

## <a name="summary"></a>まとめ

この記事では、コントロールに効果を追加する動作を使用して示されています。 `EffectBehavior`クラスは、再利用可能な Xamarin.Forms カスタム動作を追加する、 [ `Effect` ](xref:Xamarin.Forms.Effect)動作がコントロールにアタッチされているし、削除するときのコントロールのインスタンス、`Effect`インスタンスの場合、動作は、コントロールからデタッチします。


## <a name="related-links"></a>関連リンク

- [エフェクト](~/xamarin-forms/app-fundamentals/effects/index.md)
- [動作の効果 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/effectbehavior/)
- [動作](xref:Xamarin.Forms.Behavior)
- [動作<T>](xref:Xamarin.Forms.Behavior`1)
