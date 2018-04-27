---
title: 共通言語ランタイムのプロパティとして効果のパラメーターの引き渡し
description: ランタイム プロパティの変更に応答しなかった効果パラメーターを定義する、共通言語ランタイム (CLR) のプロパティを使用できます。 この記事では、特殊効果にパラメーターを渡すの CLR プロパティの使用方法を示します。
ms.prod: xamarin
ms.assetid: 4B50466C-5DBD-45DD-B1E6-BE9524C92F27
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/05/2016
ms.openlocfilehash: c913ea56af423631c48fb9ee6d8dcb95028a4144
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2018
---
# <a name="passing-effect-parameters-as-common-language-runtime-properties"></a>共通言語ランタイムのプロパティとして効果のパラメーターの引き渡し

_ランタイム プロパティの変更に応答しなかった効果パラメーターを定義する、共通言語ランタイム (CLR) のプロパティを使用できます。この記事では、特殊効果にパラメーターを渡すの CLR プロパティの使用方法を示します。_

ランタイム プロパティの変更に応答しなかった効果パラメーターを作成するプロセスは次のとおりです。

1. 作成、`public`クラスをサブクラス化する、 [ `RoutingEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/)クラスです。 `RoutingEffect`クラスは、通常は、プラットフォーム固有の内部の効果をラップするプラットフォームに依存しない効果を表します。
1. 連結したもの、解像度グループ名、および各プラットフォームに固有の効果クラスで指定された一意の ID を渡して基底クラスのコンス トラクターを呼び出すコンス トラクターを作成します。
1. 効果に渡される各パラメーターのクラスにプロパティを追加します。

パラメーターは、効果をインスタンス化するときに、各プロパティの値を指定することによって効果に渡すことができます。

サンプル アプリケーションは、`ShadowEffect`によって表示されるテキストに影を追加する、 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)コントロール。 次の図は、両者間のリレーションシップと共に、サンプル アプリケーション内の各プロジェクトの役割を示しています。

![](clr-properties-images/shadow-effect.png "シャドウ効果のプロジェクトの責任")

A [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)の control 権限、`HomePage`によってカスタマイズ、`LabelShadowEffect`プラットフォーム固有のプロジェクトごとにします。 パラメーターが渡される各`LabelShadowEffect`でプロパティを介して、`ShadowEffect`クラスです。 各`LabelShadowEffect`から派生したクラス、`PlatformEffect`各プラットフォームのクラスです。 によって表示されるテキストに追加されているシャドウでこの結果、`Label`を制御する次のスクリーン ショットに示すようにします。

![](clr-properties-images/screenshots.png "各プラットフォームでシャドウ効果")

## <a name="creating-effect-parameters"></a>効果のパラメーターを作成します。

A`public`クラスをサブクラス化する、 [ `RoutingEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/)の次のコード例に示すように、効果パラメーターを表すクラスを作成する必要があります。

```csharp
public class ShadowEffect : RoutingEffect
{
  public float Radius { get; set; }

  public Color Color { get; set; }

  public float DistanceX { get; set; }

  public float DistanceY { get; set; }

  public ShadowEffect () : base ("MyCompany.LabelShadowEffect")
  {         
  }
}
```

`ShadowEffect`を各プラットフォーム固有に渡されるパラメーターを表す 4 つのプロパティを含む`LabelShadowEffect`です。 クラスのコンス トラクターは、解像度のグループ名、および各プラットフォームに固有の効果クラスで指定された一意の ID の連結で構成されるパラメーターを渡して基底クラスのコンス トラクターを呼び出します。 そのための新しいインスタンス、`MyCompany.LabelShadowEffect`をコントロールの追加は[ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/)コレクションときに、`ShadowEffect`がインスタンス化します。

## <a name="consuming-the-effect"></a>効果の使用

XAML コード例を次に、 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)先のコントロール、`ShadowEffect`がアタッチされています。

```xaml
<Label Text="Label Shadow Effect" ...>
  <Label.Effects>
    <local:ShadowEffect Radius="5" DistanceX="5" DistanceY="5">
      <local:ShadowEffect.Color>
        <OnPlatform x:TypeArguments="Color">
            <On Platform="iOS" Value="Black" />
            <On Platform="Android" Value="White" />
            <On Platform="UWP" Value="Red" />
        </OnPlatform>
      </local:ShadowEffect.Color>
    </local:ShadowEffect>
  </Label.Effects>
</Label>
```

該当するショートカットは、 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) c# では次のコード例で示すようにします。

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

label.Effects.Add (new ShadowEffect {
  Radius = 5,
  Color = color,
  DistanceX = 5,
  DistanceY = 5
});
```

両方のコード例については、のインスタンスで、`ShadowEffect`をコントロールの追加される前に、各プロパティに指定されている値を持つクラスをインスタンス化[ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/)コレクション。 なお、`ShadowEffect.Color`プロパティがプラットフォーム固有の色の値を使用します。 詳細については、次を参照してください。[デバイス クラス](~/xamarin-forms/platform/device.md)です。

## <a name="creating-the-effect-on-each-platform"></a>各プラットフォームの効果を作成します。

次のセクションでは、説明のプラットフォームに固有の実装、`LabelShadowEffect`クラスです。

### <a name="ios-project"></a>iOS プロジェクト

次のコード例は、 `LabelShadowEffect` iOS プロジェクトの実装。

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
                var effect = (ShadowEffect)Element.Effects.FirstOrDefault (e => e is ShadowEffect);
                if (effect != null) {
                    Control.Layer.CornerRadius = effect.Radius;
                    Control.Layer.ShadowColor = effect.Color.ToCGColor ();
                    Control.Layer.ShadowOffset = new CGSize (effect.DistanceX, effect.DistanceY);
                    Control.Layer.ShadowOpacity = 1.0f;
                }
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }
    }
}
```

`OnAttached`メソッドの取得、`ShadowEffect`インスタンス、およびセット`Control.Layer`プロパティをシャドウを作成する指定したプロパティの値にします。 この機能をラップする`try` / `catch`ブロックに影響がアタッチされているコントロールがあるない場合に、`Control.Layer`プロパティです。 によって実装が指定されていない、`OnDetached`メソッド クリーンアップする必要がないためです。

### <a name="android-project"></a>Android プロジェクト

次のコード例は、 `LabelShadowEffect` Android プロジェクトの実装。

```csharp
[assembly:ResolutionGroupName ("MyCompany")]
[assembly:ExportEffect (typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace EffectsDemo.Droid
{
    public class LabelShadowEffect : PlatformEffect
    {
        protected override void OnAttached ()
        {
            try {
                var control = Control as Android.Widget.TextView;
                var effect = (ShadowEffect)Element.Effects.FirstOrDefault (e => e is ShadowEffect);
                if (effect != null) {
                    float radius = effect.Radius;
                    float distanceX = effect.DistanceX;
                    float distanceY = effect.DistanceY;
                    Android.Graphics.Color color = effect.Color.ToAndroid ();
                    control.SetShadowLayer (radius, distanceX, distanceY, color);
                }
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }
    }
}
```

`OnAttached`メソッドの取得、`ShadowEffect`インスタンス、および呼び出し、 [ `TextView.SetShadowLayer` ](https://developer.xamarin.com/api/member/Android.Widget.TextView.SetShadowLayer/p/System.Single/System.Single/System.Single/Android.Graphics.Color/)メソッドを指定したプロパティ値を使用してシャドウを作成します。 この機能をラップする`try` / `catch`ブロックに影響がアタッチされているコントロールがあるない場合に、`Control.Layer`プロパティです。 によって実装が指定されていない、`OnDetached`メソッド クリーンアップする必要がないためです。

### <a name="universal-windows-platform-project"></a>ユニバーサル Windows プラットフォーム プロジェクト

次のコード例は、`LabelShadowEffect`ユニバーサル Windows プラットフォーム (UWP) プロジェクトの実装。

```csharp
[assembly: ResolutionGroupName ("Xamarin")]
[assembly: ExportEffect (typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace EffectsDemo.UWP
{
    public class LabelShadowEffect : PlatformEffect
    {
        bool shadowAdded = false;

        protected override void OnAttached ()
        {
            try {
                if (!shadowAdded) {
                    var effect = (ShadowEffect)Element.Effects.FirstOrDefault (e => e is ShadowEffect);
                    if (effect != null) {
                        var textBlock = Control as Windows.UI.Xaml.Controls.TextBlock;
                        var shadowLabel = new Label ();
                        shadowLabel.Text = textBlock.Text;
                        shadowLabel.FontAttributes = FontAttributes.Bold;
                        shadowLabel.HorizontalOptions = LayoutOptions.Center;
                        shadowLabel.VerticalOptions = LayoutOptions.CenterAndExpand;
                        shadowLabel.TextColor = effect.Color;
                        shadowLabel.TranslationX = effect.DistanceX;
                        shadowLabel.TranslationY = effect.DistanceY;

                        ((Grid)Element.Parent).Children.Insert (0, shadowLabel);
                        shadowAdded = true;
                    }
                }
            } catch (Exception ex) {
                Debug.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }
    }
}
```

ユニバーサル Windows プラットフォームが影付き効果を提供しないため、`LabelShadowEffect`両方のプラットフォームで実装された 2 つ目のオフセットを追加することでシミュレートする[ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)プライマリの背後にある`Label`です。 `OnAttached`メソッドの取得、`ShadowEffect`インスタンスの新しく作成`Label`、いくつかのレイアウト プロパティを設定し、`Label`です。 設定して影を作成し、 [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextColor/)、 [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/)、および[ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/) の位置と色を制御するプロパティ`Label`. `shadowLabel`が挿入される、プライマリの背後にあるオフセット`Label`です。 この機能をラップする`try` / `catch`ブロックに影響がアタッチされているコントロールがあるない場合に、`Control.Layer`プロパティです。 によって実装が指定されていない、`OnDetached`メソッド クリーンアップする必要がないためです。

## <a name="summary"></a>まとめ

この記事は、CLR プロパティを使用して、影響を及ぼすパラメーターを渡す説明しました。 ランタイム プロパティの変更に応答しなかった効果パラメーターを定義する CLR プロパティを使用できます。


## <a name="related-links"></a>関連リンク

- [カスタム レンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [効果](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/)
- [PlatformEffect](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformEffect%3CTContainer,TControl%3E/)
- [RoutingEffect](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/)
- [影付き効果 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/effects/shadoweffect/)
