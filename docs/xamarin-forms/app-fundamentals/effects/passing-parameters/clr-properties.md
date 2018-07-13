---
title: 共通言語ランタイムのプロパティとして効果パラメーターの引き渡し
description: ランタイム プロパティの変更に応答しない効果パラメーターを定義する共通言語ランタイム (CLR) のプロパティを使用できます。 この記事では、CLR プロパティを使用して、効果にパラメーターを渡すを示します。
ms.prod: xamarin
ms.assetid: 4B50466C-5DBD-45DD-B1E6-BE9524C92F27
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/05/2016
ms.openlocfilehash: 1bb357b256a7cc6d52d1d92613f38cbf48400c4c
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995769"
---
# <a name="passing-effect-parameters-as-common-language-runtime-properties"></a>共通言語ランタイムのプロパティとして効果パラメーターの引き渡し

_ランタイム プロパティの変更に応答しない効果パラメーターを定義する共通言語ランタイム (CLR) のプロパティを使用できます。この記事では、CLR プロパティを使用して、効果にパラメーターを渡すを示します。_

ランタイム プロパティの変更に応答しない効果パラメーターを作成するプロセスは次のとおりです。

1. 作成、`public`クラスをサブクラスとして持つ、 [ `RoutingEffect` ](xref:Xamarin.Forms.RoutingEffect)クラス。 `RoutingEffect`クラスは、通常はプラットフォーム固有の内部の効果をラップするプラットフォームに依存しない効果を表します。
1. 解像度のグループ名、および各プラットフォームに固有のエフェクト クラスで指定された一意の ID の連結を渡して基底クラスのコンス トラクターを呼び出すコンス トラクターを作成します。
1. 効果に渡される各パラメーターのクラスにプロパティを追加します。

パラメーターは、効果をインスタンス化するときに、各プロパティの値を指定することでの効果に渡すことができます。

サンプル アプリケーションでは、`ShadowEffect`によって表示されるテキストに影を追加する、 [ `Label` ](xref:Xamarin.Forms.Label)コントロール。 次の図は、サンプル アプリケーションとそれらの間のリレーションシップ内の各プロジェクトの役割を示します。

![](clr-properties-images/shadow-effect.png "シャドウ効果プロジェクトの責任")

A [ `Label` ](xref:Xamarin.Forms.Label)の control 権限、`HomePage`によってカスタマイズされた、`LabelShadowEffect`各プラットフォーム固有プロジェクト。 パラメーターが渡される各`LabelShadowEffect`内でプロパティを介して、`ShadowEffect`クラス。 各`LabelShadowEffect`クラスから派生、`PlatformEffect`各プラットフォームのクラス。 によって表示されるテキストに追加されるシャドウでこの結果、`Label`コントロールが次のスクリーン ショットに示すようにします。

![](clr-properties-images/screenshots.png "各プラットフォームでのシャドウ効果")

## <a name="creating-effect-parameters"></a>パラメーターの効果を作成します。

A`public`クラスをサブクラスとして持つ、 [ `RoutingEffect` ](xref:Xamarin.Forms.RoutingEffect)の次のコード例に示す、効果のパラメーターを表すクラスを作成する必要があります。

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

`ShadowEffect`各プラットフォーム固有に渡されるパラメーターを表す 4 つのプロパティを含む`LabelShadowEffect`します。 クラスのコンス トラクターは、解像度のグループ名、および各プラットフォームに固有のエフェクト クラスで指定された一意の ID の連結で構成されるパラメーターを渡して基底クラスのコンス トラクターを呼び出します。 そのための新しいインスタンス、`MyCompany.LabelShadowEffect`をコントロールの追加は[ `Effects` ](xref:Xamarin.Forms.Element.Effects)コレクションと、`ShadowEffect`がインスタンス化されます。

## <a name="consuming-the-effect"></a>効果の使用

次の XAML コード例は、 [ `Label` ](xref:Xamarin.Forms.Label)コントロールを`ShadowEffect`がアタッチされています。

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

label.Effects.Add (new ShadowEffect {
  Radius = 5,
  Color = color,
  DistanceX = 5,
  DistanceY = 5
});
```

どちらのコードの例のインスタンスで、`ShadowEffect`をコントロールの追加される前に、各プロパティの指定されている値を持つクラスをインスタンス化[ `Effects` ](xref:Xamarin.Forms.Element.Effects)コレクション。 なお、`ShadowEffect.Color`プロパティはプラットフォーム固有の色の値を使用します。 詳細については、次を参照してください。[デバイス クラス](~/xamarin-forms/platform/device.md)します。

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

`OnAttached`メソッドの取得、`ShadowEffect`インスタンス、およびセット`Control.Layer`プロパティの影を作成する指定したプロパティの値。 この機能にラップされて、 `try` / `catch`ブロックに効果がアタッチされているコントロールがあるない場合に、`Control.Layer`プロパティ。 によって実装が指定されていない、`OnDetached`メソッド クリーンアップする必要がないので。

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

`OnAttached`メソッドの取得、`ShadowEffect`インスタンス、および呼び出し、 [ `TextView.SetShadowLayer` ](https://developer.xamarin.com/api/member/Android.Widget.TextView.SetShadowLayer/p/System.Single/System.Single/System.Single/Android.Graphics.Color/)メソッドを指定したプロパティ値を使用して影を作成します。 この機能にラップされて、 `try` / `catch`ブロックに効果がアタッチされているコントロールがあるない場合に、`Control.Layer`プロパティ。 によって実装が指定されていない、`OnDetached`メソッド クリーンアップする必要がないので。

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

ユニバーサル Windows プラットフォームは、シャドウ効果を行いません。 そのため、`LabelShadowEffect`両方のプラットフォームで実装では、2 つ目のオフセットを追加することで 1 つをシミュレート[ `Label` ](xref:Xamarin.Forms.Label)プライマリの背後にある`Label`。 `OnAttached`メソッドの取得、`ShadowEffect`インスタンスを新たに作成します`Label`、いくつかのレイアウト プロパティを設定し、`Label`します。 設定して影を作成し、 [ `TextColor` ](xref:Xamarin.Forms.Label.TextColor)、 [ `TranslationX` ](xref:Xamarin.Forms.VisualElement.TranslationX)、および[ `TranslationY` ](xref:Xamarin.Forms.VisualElement.TranslationY) の場所と色を制御するプロパティ`Label`. `shadowLabel`挿入し、プライマリの背後にあるオフセット`Label`します。 この機能にラップされて、 `try` / `catch`ブロックに効果がアタッチされているコントロールがあるない場合に、`Control.Layer`プロパティ。 によって実装が指定されていない、`OnDetached`メソッド クリーンアップする必要がないので。

## <a name="summary"></a>まとめ

この記事では、効果にパラメーターを渡す CLR プロパティの使用について説明しました。 ランタイム プロパティの変更に応答しない効果パラメーターを定義するには、CLR プロパティを使用することができます。


## <a name="related-links"></a>関連リンク

- [カスタム レンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [効果](xref:Xamarin.Forms.Effect)
- [PlatformEffect](xref:Xamarin.Forms.PlatformEffect`2)
- [RoutingEffect](xref:Xamarin.Forms.RoutingEffect)
- [シャドウ効果 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/effects/shadoweffect/)
