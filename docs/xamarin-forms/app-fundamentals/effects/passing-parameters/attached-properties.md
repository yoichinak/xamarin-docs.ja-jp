---
title: 添付プロパティとして効果パラメーターの引き渡し
description: ランタイム プロパティの変更に応答する効果のパラメーターを定義する添付プロパティを使用できます。 この記事では、効果、および実行時にパラメーターを変更するパラメーターを渡すプロパティを使用して接続されているを示します。
ms.prod: xamarin
ms.assetid: DFCDCB9F-17DD-4117-BD53-B4FB206BB387
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/05/2016
ms.openlocfilehash: 9483e424a74a88ce3f0eb49624bb5315551f2062
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996452"
---
# <a name="passing-effect-parameters-as-attached-properties"></a>添付プロパティとして効果パラメーターの引き渡し

_ランタイム プロパティの変更に応答する効果のパラメーターを定義する添付プロパティを使用できます。この記事では、効果、および実行時にパラメーターを変更するパラメーターを渡すプロパティを使用して接続されているを示します。_

ランタイム プロパティの変更に応答する効果のパラメーターを作成するプロセスは次のとおりです。

1. 作成、`static`効果に渡される各パラメーターの添付プロパティを含むクラスです。
1. 追加の添付プロパティを追加または削除、クラスが接続されているコントロールに影響の制御に使用されるクラスに追加します。 このプロパティのレジスタがアタッチされていることを確認、`propertyChanged`プロパティの値が変更されたときに実行されるデリゲート。
1. 作成`static`添付プロパティの getter および setter ごと。
1. 内のロジックを実装、`propertyChanged`デリゲートを追加して、効果を削除します。
1. 内で入れ子になったクラスを実装、 `static` 、効果、つまりサブクラス後という名前のクラス、`RoutingEffect`クラス。 コンス トラクターでは、解像度のグループ名、および各プラットフォームに固有のエフェクト クラスで指定された一意の ID の連結を渡して基底クラスのコンス トラクターを呼び出します。

パラメーターは、適切なコントロールに添付プロパティ、およびプロパティの値を追加することで効果に渡すできます。 さらに、新しい添付プロパティの値を指定することで、実行時にパラメーターを変更できます。

> [!NOTE]
> 添付プロパティは、特殊な種類のバインド可能なプロパティ、属性クラスとピリオドで区切ったプロパティ名が含まれていると、その他のオブジェクトにアタッチされていると、XAML で認識可能な 1 つのクラスで定義されています。 詳細については、次を参照してください。[添付プロパティ](~/xamarin-forms/xaml/attached-properties.md)します。

サンプル アプリケーションでは、`ShadowEffect`によって表示されるテキストに影を追加する、 [ `Label` ](xref:Xamarin.Forms.Label)コントロール。 さらに、実行時に影の色を変更できます。 次の図は、サンプル アプリケーションとそれらの間のリレーションシップ内の各プロジェクトの役割を示します。

![](attached-properties-images/shadow-effect.png "シャドウ効果プロジェクトの責任")

A [ `Label` ](xref:Xamarin.Forms.Label)の control 権限、`HomePage`によってカスタマイズされた、`LabelShadowEffect`各プラットフォーム固有プロジェクト。 パラメーターが渡される各`LabelShadowEffect`添付プロパティを通じて、`ShadowEffect`クラス。 各`LabelShadowEffect`クラスから派生、`PlatformEffect`各プラットフォームのクラス。 によって表示されるテキストに追加されるシャドウでこの結果、`Label`コントロールが次のスクリーン ショットに示すようにします。

![](attached-properties-images/screenshots.png "各プラットフォームでのシャドウ効果")

## <a name="creating-effect-parameters"></a>パラメーターの効果を作成します。

A`static`の次のコード例に示す、効果のパラメーターを表すクラスを作成する必要があります。

```csharp
public static class ShadowEffect
{
  public static readonly BindableProperty HasShadowProperty =
    BindableProperty.CreateAttached ("HasShadow", typeof(bool), typeof(ShadowEffect), false, propertyChanged: OnHasShadowChanged);
  public static readonly BindableProperty ColorProperty =
    BindableProperty.CreateAttached ("Color", typeof(Color), typeof(ShadowEffect), Color.Default);
  public static readonly BindableProperty RadiusProperty =
    BindableProperty.CreateAttached ("Radius", typeof(double), typeof(ShadowEffect), 1.0);
  public static readonly BindableProperty DistanceXProperty =
    BindableProperty.CreateAttached ("DistanceX", typeof(double), typeof(ShadowEffect), 0.0);
  public static readonly BindableProperty DistanceYProperty =
    BindableProperty.CreateAttached ("DistanceY", typeof(double), typeof(ShadowEffect), 0.0);

  public static bool GetHasShadow (BindableObject view)
  {
    return (bool)view.GetValue (HasShadowProperty);
  }

  public static void SetHasShadow (BindableObject view, bool value)
  {
    view.SetValue (HasShadowProperty, value);
  }
  ...

  static void OnHasShadowChanged (BindableObject bindable, object oldValue, object newValue)
  {
    var view = bindable as View;
    if (view == null) {
      return;
    }

    bool hasShadow = (bool)newValue;
    if (hasShadow) {
      view.Effects.Add (new LabelShadowEffect ());
    } else {
      var toRemove = view.Effects.FirstOrDefault (e => e is LabelShadowEffect);
      if (toRemove != null) {
        view.Effects.Remove (toRemove);
      }
    }
  }

  class LabelShadowEffect : RoutingEffect
  {
    public LabelShadowEffect () : base ("MyCompany.LabelShadowEffect")
    {
    }
  }
}
```

`ShadowEffect` 5 つの添付プロパティを持つ`static`添付プロパティの getter および setter ごと。 各プラットフォーム固有に渡されるパラメーターを表す 4 つのこれらのプロパティ`LabelShadowEffect`します。 `ShadowEffect`クラスも定義、`HasShadow`添付プロパティを追加または削除のコントロールに影響を制御するために使用される、`ShadowEffect`にクラスが接続されています。 この添付プロパティに、プロパティの値が変更された時に実行される `OnHasShadowChanged` メソッドを登録します。 このメソッドを追加またはの値に基づいて効果の削除、`HasShadow`添付プロパティ。

入れ子になった`LabelShadowEffect`クラスをサブクラス、 [ `RoutingEffect` ](xref:Xamarin.Forms.RoutingEffect)クラスをサポートする効果の追加と削除。 `RoutingEffect`クラスは、通常はプラットフォーム固有の内部の効果をラップするプラットフォームに依存しない効果を表します。 プラットフォーム固有の効果の型情報をコンパイル時のアクセスがないために、効果の削除プロセスが簡略化します。 `LabelShadowEffect`コンス トラクターは、解像度のグループ名、および各プラットフォームに固有のエフェクト クラスで指定された一意の ID の連結で構成されるパラメーターを渡して基底クラスのコンス トラクターを呼び出します。 これにより、効果の追加と削除、`OnHasShadowChanged`メソッドは、次のようにします。

- **追加の影響を与える**– の新しいインスタンス、`LabelShadowEffect`をコントロールの追加は[ `Effects` ](xref:Xamarin.Forms.Element.Effects)コレクション。 使用してこれに置き換えられます、 [ `Effect.Resolve` ](xref:Xamarin.Forms.Effect.Resolve(System.String))効果を追加するメソッド。
- **削除の影響を与える**– の最初のインスタンス、 `LabelShadowEffect` 、コントロールの[ `Effects` ](xref:Xamarin.Forms.Element.Effects)コレクションが取得され、削除します。

## <a name="consuming-the-effect"></a>効果の使用

各プラットフォーム固有`LabelShadowEffect`する添付プロパティを追加することで使用できる、 [ `Label` ](xref:Xamarin.Forms.Label)制御、次の XAML コード例で示した。

```xaml
<Label Text="Label Shadow Effect" ...
       local:ShadowEffect.HasShadow="true" local:ShadowEffect.Radius="5"
       local:ShadowEffect.DistanceX="5" local:ShadowEffect.DistanceY="5">
  <local:ShadowEffect.Color>
    <OnPlatform x:TypeArguments="Color">
        <On Platform="iOS" Value="Black" />
        <On Platform="Android" Value="White" />
        <On Platform="UWP" Value="Red" />
    </OnPlatform>
  </local:ShadowEffect.Color>
</Label>
```

相当[ `Label` ](xref:Xamarin.Forms.Label) c# では次のコード例で示すようにします。

```csharp
var label = new Label {
  Text = "Label Shadow Effect",
  ...
};

Color color = Color.Default;
switch (Device.RuntimePlatform)
{
    case Device.iOS:
        color = Color.Black;
        break;
    case Device.Android:
        color = Color.White;
        break;
    case Device.UWP:
        color = Color.Red;
        break;
}

ShadowEffect.SetHasShadow (label, true);
ShadowEffect.SetRadius (label, 5);
ShadowEffect.SetDistanceX (label, 5);
ShadowEffect.SetDistanceY (label, 5);
ShadowEffect.SetColor (label, color));
```

設定、`ShadowEffect.HasShadow`添付プロパティを`true`実行、`ShadowEffect.OnHasShadowChanged`メソッドを追加または削除、`LabelShadowEffect`を[ `Label` ](xref:Xamarin.Forms.Label)コントロール。 両方のコード例では、`ShadowEffect.Color`添付プロパティは、プラットフォーム固有のカラー値を提供します。 詳細については、次を参照してください。[デバイス クラス](~/xamarin-forms/platform/device.md)します。

さらに、 [ `Button` ](xref:Xamarin.Forms.Button)により、実行時に変更する影の色。 ときに、`Button`がクリックされた、影の色を設定して、次のコード変更、`ShadowEffect.Color`添付プロパティ。

```csharp
ShadowEffect.SetColor (label, Color.Teal);
```

### <a name="consuming-the-effect-with-a-style"></a>スタイルで効果の使用

添付プロパティをコントロールに追加することで消費可能な効果は、スタイルによっても使用できます。 次の XAML コード例は、*明示的な*のシャドウ効果に適用できるスタイル[ `Label` ](xref:Xamarin.Forms.Label)コントロール。

```xaml
<Style x:Key="ShadowEffectStyle" TargetType="Label">
  <Style.Setters>
    <Setter Property="local:ShadowEffect.HasShadow" Value="True" />
    <Setter Property="local:ShadowEffect.Radius" Value="5" />
    <Setter Property="local:ShadowEffect.DistanceX" Value="5" />
    <Setter Property="local:ShadowEffect.DistanceY" Value="5" />
  </Style.Setters>
</Style>
```

[ `Style` ](xref:Xamarin.Forms.Style)に適用できる、 [ `Label` ](xref:Xamarin.Forms.Label)を設定してその[ `Style` ](xref:Xamarin.Forms.VisualElement.Style)プロパティを`Style`インスタンス、を使用して`StaticResource`マークアップ拡張機能で、次のコード例で示した。

```xaml
<Label Text="Label Shadow Effect" ... Style="{StaticResource ShadowEffectStyle}" />
```

スタイルの詳細については、次を参照してください。[スタイル](~/xamarin-forms/user-interface/styles/index.md)します。

## <a name="creating-the-effect-on-each-platform"></a>各プラットフォームで効果を作成します。

次のセクションでは、プラットフォーム固有の実装の説明、`LabelShadowEffect`クラス。

### <a name="ios-project"></a>iOS プロジェクト

次のコードサンプルでは、 iOS プロジェクトの `LabelShadowEffect` 実装を示します。

```csharp
[assembly:ResolutionGroupName ("MyCompany")]
[assembly:ExportEffect (typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace EffectsDemo.iOS
{
    public class LabelShadowEffect : PlatformEffect
    {
        protected override void OnAttached ()
        {
            try {
                UpdateRadius ();
                UpdateColor ();
                UpdateOffset ();
                Control.Layer.ShadowOpacity = 1.0f;
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }
        ...

        void UpdateRadius ()
        {
            Control.Layer.CornerRadius = (nfloat)ShadowEffect.GetRadius (Element);
        }

        void UpdateColor ()
        {
            Control.Layer.ShadowColor = ShadowEffect.GetColor (Element).ToCGColor ();
        }

        void UpdateOffset ()
        {
            Control.Layer.ShadowOffset = new CGSize (
                (double)ShadowEffect.GetDistanceX (Element),
                (double)ShadowEffect.GetDistanceY (Element));
        }
    }
```

`OnAttached`メソッドを使用して添付プロパティの値を取得するメソッドを呼び出し、 `ShadowEffect` getter、および設定`Control.Layer`プロパティをシャドウを付けるプロパティの値にします。 この機能にラップされて、 `try` / `catch`ブロックに効果がアタッチされているコントロールがあるない場合に、`Control.Layer`プロパティ。 によって実装が指定されていない、`OnDetached`メソッド クリーンアップする必要がないので。

#### <a name="responding-to-property-changes"></a>プロパティの変更に応答します。

場合、`ShadowEffect`プロパティ値の変更時に、変更内容を表示することで応答する効果のニーズをアタッチします。 オーバーライドされたバージョン、`OnElementPropertyChanged`手法、プラットフォーム固有のエフェクト クラスでは、バインド可能なプロパティの変更に対応するための次のコード例に示す。

```csharp
public class LabelShadowEffect : PlatformEffect
{
  ...
  protected override void OnElementPropertyChanged (PropertyChangedEventArgs args)
  {
    if (args.PropertyName == ShadowEffect.RadiusProperty.PropertyName) {
      UpdateRadius ();
    } else if (args.PropertyName == ShadowEffect.ColorProperty.PropertyName) {
      UpdateColor ();
    } else if (args.PropertyName == ShadowEffect.DistanceXProperty.PropertyName ||
               args.PropertyName == ShadowEffect.DistanceYProperty.PropertyName) {
      UpdateOffset ();
    }
  }
  ...
}
```

`OnElementPropertyChanged`メソッドは、radius、色、または、影のオフセットに更新されている、適切な`ShadowEffect`添付プロパティの値が変更されました。 この上書きは、何度も呼び出すことが、変更されているプロパティのチェックを行った常にする必要があります。

### <a name="android-project"></a>Android プロジェクト

次のコード例は、 `LabelShadowEffect` Android プロジェクトの実装。

```csharp
[assembly:ResolutionGroupName ("MyCompany")]
[assembly:ExportEffect (typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace EffectsDemo.Droid
{
    public class LabelShadowEffect : PlatformEffect
    {
        Android.Widget.TextView control;
        Android.Graphics.Color color;
        float radius, distanceX, distanceY;

        protected override void OnAttached ()
        {
            try {
                control = Control as Android.Widget.TextView;
                UpdateRadius ();
                UpdateColor ();
                UpdateOffset ();
                UpdateControl ();
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }
        ...

        void UpdateControl ()
        {
            if (control != null) {
                control.SetShadowLayer (radius, distanceX, distanceY, color);
            }
        }

        void UpdateRadius ()
        {
            radius = (float)ShadowEffect.GetRadius (Element);
        }

        void UpdateColor ()
        {
            color = ShadowEffect.GetColor (Element).ToAndroid ();
        }

        void UpdateOffset ()
        {
            distanceX = (float)ShadowEffect.GetDistanceX (Element);
            distanceY = (float)ShadowEffect.GetDistanceY (Element);
        }
    }
```

`OnAttached`メソッドを使用して添付プロパティの値を取得するメソッドを呼び出します、`ShadowEffect`の getter メソッドを呼び出すと、 [ `TextView.SetShadowLayer` ](https://developer.xamarin.com/api/member/Android.Widget.TextView.SetShadowLayer/p/System.Single/System.Single/System.Single/Android.Graphics.Color/)プロパティ値を使用して影を作成する方法。 この機能にラップされて、 `try` / `catch`ブロックに効果がアタッチされているコントロールがあるない場合に、`Control.Layer`プロパティ。 によって実装が指定されていない、`OnDetached`メソッド クリーンアップする必要がないので。

#### <a name="responding-to-property-changes"></a>プロパティの変更に応答します。

場合、`ShadowEffect`プロパティ値の変更時に、変更内容を表示することで応答する効果のニーズをアタッチします。 オーバーライドされたバージョン、`OnElementPropertyChanged`手法、プラットフォーム固有のエフェクト クラスでは、バインド可能なプロパティの変更に対応するための次のコード例に示す。

```csharp
public class LabelShadowEffect : PlatformEffect
{
  ...
  protected override void OnElementPropertyChanged (PropertyChangedEventArgs args)
  {
    if (args.PropertyName == ShadowEffect.RadiusProperty.PropertyName) {
      UpdateRadius ();
      UpdateControl ();
    } else if (args.PropertyName == ShadowEffect.ColorProperty.PropertyName) {
      UpdateColor ();
      UpdateControl ();
    } else if (args.PropertyName == ShadowEffect.DistanceXProperty.PropertyName ||
               args.PropertyName == ShadowEffect.DistanceYProperty.PropertyName) {
      UpdateOffset ();
      UpdateControl ();
    }
  }
  ...
}
```

`OnElementPropertyChanged`メソッドは、radius、色、または、影のオフセットに更新されている、適切な`ShadowEffect`添付プロパティの値が変更されました。 この上書きは、何度も呼び出すことが、変更されているプロパティのチェックを行った常にする必要があります。

### <a name="universal-windows-platform-project"></a>ユニバーサル Windows プラットフォーム プロジェクト

次のコード例は、`LabelShadowEffect`ユニバーサル Windows プラットフォーム (UWP) プロジェクトの実装。

```csharp
[assembly: ResolutionGroupName ("MyCompany")]
[assembly: ExportEffect (typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace EffectsDemo.UWP
{
    public class LabelShadowEffect : PlatformEffect
    {
        Label shadowLabel;
        bool shadowAdded = false;

        protected override void OnAttached ()
        {
            try {
                if (!shadowAdded) {
                    var textBlock = Control as Windows.UI.Xaml.Controls.TextBlock;

                    shadowLabel = new Label ();
                    shadowLabel.Text = textBlock.Text;
                    shadowLabel.FontAttributes = FontAttributes.Bold;
                    shadowLabel.HorizontalOptions = LayoutOptions.Center;
                    shadowLabel.VerticalOptions = LayoutOptions.CenterAndExpand;

                    UpdateColor ();
                    UpdateOffset ();

                    ((Grid)Element.Parent).Children.Insert (0, shadowLabel);
                    shadowAdded = true;
                }
            } catch (Exception ex) {
                Debug.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }
        ...

        void UpdateColor ()
        {
            shadowLabel.TextColor = ShadowEffect.GetColor (Element);
        }

        void UpdateOffset ()
        {
            shadowLabel.TranslationX = ShadowEffect.GetDistanceX (Element);
            shadowLabel.TranslationY = ShadowEffect.GetDistanceY (Element);
        }
    }
}
```

ユニバーサル Windows プラットフォームは、シャドウ効果を行いません。 そのため、`LabelShadowEffect`両方のプラットフォームで実装では、2 つ目のオフセットを追加することで 1 つをシミュレート[ `Label` ](xref:Xamarin.Forms.Label)プライマリの背後にある`Label`。 `OnAttached`メソッドを作成、新しい`Label`のいくつかのレイアウト プロパティを設定し、`Label`します。 使用して添付プロパティの値を取得するメソッドを呼び出して、 `ShadowEffect` get アクセス操作子、設定して影を作成し、 [ `TextColor` ](xref:Xamarin.Forms.Label.TextColor)、 [ `TranslationX` ](xref:Xamarin.Forms.VisualElement.TranslationX)、および[ `TranslationY` ](xref:Xamarin.Forms.VisualElement.TranslationY)の場所と色を制御するプロパティ、`Label`します。 `shadowLabel`挿入し、プライマリの背後にあるオフセット`Label`します。 この機能にラップされて、 `try` / `catch`ブロックに効果がアタッチされているコントロールがあるない場合に、`Control.Layer`プロパティ。 によって実装が指定されていない、`OnDetached`メソッド クリーンアップする必要がないので。

#### <a name="responding-to-property-changes"></a>プロパティの変更に応答します。

場合、`ShadowEffect`プロパティ値の変更時に、変更内容を表示することで応答する効果のニーズをアタッチします。 オーバーライドされたバージョン、`OnElementPropertyChanged`手法、プラットフォーム固有のエフェクト クラスでは、バインド可能なプロパティの変更に対応するための次のコード例に示す。

```csharp
public class LabelShadowEffect : PlatformEffect
{
  ...
  protected override void OnElementPropertyChanged (PropertyChangedEventArgs args)
  {
    if (args.PropertyName == ShadowEffect.ColorProperty.PropertyName) {
      UpdateColor ();
    } else if (args.PropertyName == ShadowEffect.DistanceXProperty.PropertyName ||
                      args.PropertyName == ShadowEffect.DistanceYProperty.PropertyName) {
      UpdateOffset ();
    }
  }
  ...
}
```

`OnElementPropertyChanged`色または影のオフセットにメソッドが更新されている、適切な`ShadowEffect`添付プロパティの値が変更されました。 この上書きは、何度も呼び出すことが、変更されているプロパティのチェックを行った常にする必要があります。

## <a name="summary"></a>まとめ

この記事では、効果、および実行時にパラメーターを変更するパラメーターを渡すプロパティを使用して接続されている説明しました。 ランタイム プロパティの変更に応答する効果のパラメーターを定義する添付プロパティを使用できます。


## <a name="related-links"></a>関連リンク

- [カスタム レンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [効果](xref:Xamarin.Forms.Effect)
- [PlatformEffect](xref:Xamarin.Forms.PlatformEffect`2)
- [RoutingEffect](xref:Xamarin.Forms.RoutingEffect)
- [シャドウ効果 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/effects/shadoweffectruntimechange/)
