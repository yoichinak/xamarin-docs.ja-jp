---
title: 再利用可能な EffectBehavior
description: 動作は、処理の分離コード ファイルからコード ボイラー プレート効果を削除する、コントロールに、特殊効果を追加するために有効な方法です。 この記事では、コントロールを追加する、特殊効果、Xamarin.Forms 動作を使用してを示します。
ms.prod: xamarin
ms.assetid: A909B24D-960A-4023-AFF6-4B9256C55ADD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: a1612d1e87f0e05c859babd93fd03ac9a5736b47
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
ms.locfileid: "30785056"
---
# <a name="reusable-effectbehavior"></a>再利用可能な EffectBehavior

_動作は、処理の分離コード ファイルからコード ボイラー プレート効果を削除する、コントロールに、特殊効果を追加するために有効な方法です。この記事では、コントロールを追加する、特殊効果、Xamarin.Forms 動作を使用してを示します。_

## <a name="overview"></a>概要

`EffectBehavior`クラスは、再利用可能な Xamarin.Forms カスタム動作を追加する、 [ `Effect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/)動作が、コントロールに接続し、削除するときのコントロールのインスタンス、`Effect`インスタンスの動作はの場合コントロールからデタッチします。

次の動作プロパティは、動作を使用して設定する必要があります。

- **グループ**– の値、 [ `ResolutionGroupName` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResolutionGroupNameAttribute/)効果クラスの属性です。
- **名前**– の値、 [ `ExportEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ExportEffectAttribute/)効果クラスの属性です。

影響の詳細については、次を参照してください。[効果](~/xamarin-forms/app-fundamentals/effects/index.md)です。

## <a name="creating-the-behavior"></a>動作を作成します。

`EffectBehavior`から派生したクラス、 [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/)クラス、場所`T`は、 [ `View`](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)です。 つまり、`EffectBehavior`クラスは、Xamarin.Forms コントロールに関連付けることができます。

### <a name="implementing-bindable-properties"></a>バインド可能なプロパティを実装します。

`EffectBehavior`クラスは、2 つの定義[ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/)追加に使用されるインスタンス、 [ `Effect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/)コントロールをコントロールに、ビヘイビアーがアタッチされているときにします。 これらのプロパティは次のコード例に示します。

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

ときに、`EffectBehavior`を消費、`Group`プロパティは、の値に設定する必要があります、 [ `ResolutionGroupName` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResolutionGroupNameAttribute/)効果の属性です。 さらに、`Name`プロパティは、の値に設定する必要があります、 [ `ExportEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ExportEffectAttribute/)効果クラスの属性です。

### <a name="implementing-the-overrides"></a>オーバーライドの実装

`EffectBehavior`クラスのオーバーライド、 [ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/)と[ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/)のメソッド、 [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/)次のコードに示すように、クラス例:

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

[ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/)メソッドを呼び出してセットアップを実行する、`AddEffect`メソッド パラメーターとして接続されているコントロール。 [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/)メソッドを呼び出してクリーンアップを実行する、`RemoveEffect`メソッド パラメーターとして接続されているコントロール。

### <a name="implementing-the-behavior-functionality"></a>動作の機能を実装します。

動作の目的は、追加する、 [ `Effect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/)で定義されている、`Group`と`Name`動作は、コントロールに接続し、削除するときのコントロールのプロパティ、`Effect`ときの動作はコントロールからデタッチします。 動作のコア機能は、次のコード例で表示されます。

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

`AddEffect`への応答でメソッドが実行される、`EffectBehavior`パラメーターとして接続されているコントロールを受け取るコントロール、およびそれにアタッチされています。 このメソッドは、コントロールに取得した効果を追加する、 [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/)コレクション。 `RemoveEffect`への応答でメソッドが実行される、`EffectBehavior`パラメーターとして接続されているコントロールを受け取ると、コントロールからデタッチされています。 メソッドは、コントロールから効果を削除する、 [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/)コレクション。

`GetEffect`メソッドを使用、 [ `Effect.Resolve` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Effect.Resolve/p/System.String/)を取得する方法、 [ `Effect`](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/)です。 連結したもの使用効果がある、`Group`と`Name`プロパティの値。 プラットフォームには、効果が用意されていない場合、`Effect.Resolve`メソッドが返す以外`null`値。

## <a name="consuming-the-behavior"></a>動作の使用

`EffectBehavior`に添付できるクラス、 [ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/)の XAML コードの例を次に示すように、コントロールのコレクション。

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

`Group`と`Name`動作のプロパティの値に設定されます、 [ `ResolutionGroupName` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResolutionGroupNameAttribute/)と[ `ExportEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ExportEffectAttribute/)各プラットフォームに固有の効果のクラスの属性プロジェクトです。

ビヘイビアーがアタッチされているときに、実行時に、 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)コントロール、`Xamarin.LabelShadowEffect`をコントロールの追加は[ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/)コレクション。 によって表示されるテキストに追加されているシャドウでこの結果、`Label`を制御する次のスクリーン ショットに示すようにします。

![](effect-behavior-images/screenshots.png "EffectsBehavior でサンプル アプリケーション")

追加およびコントロールからの効果を削除するこの動作を使用する利点は、ボイラー プレート効果処理コードを分離コード ファイルから削除できることです。

## <a name="summary"></a>まとめ

この記事では、コントロールに効果を追加する動作を使用して示されています。 `EffectBehavior`クラスは、再利用可能な Xamarin.Forms カスタム動作を追加する、 [ `Effect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/)動作が、コントロールに接続し、削除するときのコントロールのインスタンス、`Effect`インスタンスの動作はの場合コントロールからデタッチします。


## <a name="related-links"></a>関連リンク

- [エフェクト](~/xamarin-forms/app-fundamentals/effects/index.md)
- [動作の効果 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/effectbehavior/)
- [動作](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/)
- [動作<T>](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/)
