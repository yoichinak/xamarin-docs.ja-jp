---
title: title:"共通言語ランタイム プロパティとしてエフェクトのパラメーターを渡す" の説明:"共通言語ランタイム (CLR) プロパティは、実行時のプロパティの変更に応答しないエフェクトのパラメーターの定義に使用できます。
description: 'この記事では、CLR プロパティを使用してエフェクトにパラメーターを渡す方法について説明します。" ms.prod: xamarin ms.assetid:4B50466C-5DBD-45DD-B1E6-BE9524C92F27 ms.technology: xamarin-forms author: davidbritch ms.author: dabritch ms.date:08/05/2016 no-loc: [Xamarin.Forms, Xamarin.Essentials]'
ms.prod: xamarin
ms.assetid: 4B50466C-5DBD-45DD-B1E6-BE9524C92F27
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/05/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 37d870509e034f4c23afba60fa055965ed9df4de
ms.sourcegitcommit: ea9269b5d9e3d68b61bb428560a10034117ee457
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/10/2020
ms.locfileid: "84138867"
---
# <a name="passing-effect-parameters-as-common-language-runtime-properties"></a>共通言語ランタイム プロパティとしてエフェクトのパラメーターを渡す

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-shadoweffect)

_共通言語ランタイム (CLR) プロパティは、実行時のプロパティの変更に応答しない効果のパラメーターの定義に使用できます。この記事では、CLR プロパティを使用して効果にパラメーターを渡す方法について説明します。_

実行時のプロパティの変更に応答しないエフェクトのパラメーターを作成するプロセスは、次のとおりです。

1. [`RoutingEffect`](xref:Xamarin.Forms.RoutingEffect) クラスをサブクラス化する `public` クラスを作成します。 `RoutingEffect` クラスは、通常はプラットフォーム固有となる内部のエフェクトをラップするプラットフォームに依存しないエフェクトを表します。
1. 解決グループ名と、各プラットフォーム固有のエフェクトクラスで指定された一意の ID を連結したものを渡して、基底クラス コンストラクターを呼び出すコンストラクターを作成します。
1. エフェクトに渡す各パラメーターのクラスにプロパティを追加します。

エフェクトをインスタンス化するときに各プロパティの値を指定することで、パラメーターをエフェクトに渡すことができます。

このサンプル アプリケーションは、[`Label`](xref:Xamarin.Forms.Label) コントロールによって表示されるテキストに影を追加する `ShadowEffect` を示しています。 次の図に、サンプル アプリケーション内の各プロジェクトの役割と、それらの関係を示します。

![](clr-properties-images/shadow-effect.png "Shadow Effect Project Responsibilities")

`HomePage` 上の [`Label`](xref:Xamarin.Forms.Label) コントロールは、各プラットフォーム固有のプロジェクト内の `LabelShadowEffect` によってカスタマイズされます。 パラメーターは `ShadowEffect` クラス内のプロパティを介して各 `LabelShadowEffect` に渡されます。 各プラットフォームの `PlatformEffect` クラスから、各 `LabelShadowEffect` クラスが派生します。 これにより、次のスクリーンショットに示すように、`Label` コントロールによって表示されるテキストに影が追加されます。

![](clr-properties-images/screenshots.png "Shadow Effect on each Platform")

## <a name="creating-effect-parameters"></a>エフェクトのパラメーターの作成

次のコード例に示すように、エフェクトのパラメーターを表すには、[`RoutingEffect`](xref:Xamarin.Forms.RoutingEffect) クラスをサブクラス化する `public` クラスを作成する必要があります。

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

`ShadowEffect` には、各プラットフォーム固有の `LabelShadowEffect` に渡されるパラメーターを表す 4 つのプロパティが含まれています。 クラス コンストラクターから基底クラスのコンストラクターが呼び出され、解像度グループ名と、各プラットフォーム固有のエフェクト クラスで指定された一意の ID を連結したもので構成されるパラメーターが渡されます。 そのため、`ShadowEffect` がインスタンス化されると、`MyCompany.LabelShadowEffect` の新しいインスタンスがコントロールの [`Effects`](xref:Xamarin.Forms.Element.Effects) コレクションに追加されます。

## <a name="consuming-the-effect"></a>エフェクトを使用する

`ShadowEffect` がアタッチされている [`Label`](xref:Xamarin.Forms.Label) コントロールを次の XAML コード例に示します。

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

C# での同等の [`Label`](xref:Xamarin.Forms.Label) を次のコード例に示します。

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

どちらのコード例でも、`ShadowEffect` クラスのインスタンスは、各プロパティに指定された値でインスタンス化されてから、コントロールの [`Effects`](xref:Xamarin.Forms.Element.Effects) コレクションに追加されます。 `ShadowEffect.Color` プロパティにはプラットフォーム固有の色の値が使用されることに注意してください。 詳細については、[デバイス クラス](~/xamarin-forms/platform/device.md)に関するページを参照してください。

## <a name="creating-the-effect-on-each-platform"></a>各プラットフォーム上でエフェクトを作成する

以下のセクションでは、`LabelShadowEffect` クラスのプラットフォーム固有の実装について説明します。

### <a name="ios-project"></a>iOS プロジェクト

iOS プロジェクト用の `LabelShadowEffect` の実装を次のコード例に示します。

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
                    Control.Layer.ShadowRadius = effect.Radius;
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

`OnAttached` メソッドで `ShadowEffect` インスタンスを取得し、指定されたプロパティ値に `Control.Layer` プロパティを設定して影を作成します。 エフェクトがアタッチされているコントロールに `Control.Layer` プロパティがない場合に備えて、この機能が `try`/`catch` ブロック内にラップされます。 クリーンアップする必要がないので、`OnDetached` メソッドによる実装は提供されません。

### <a name="android-project"></a>Android プロジェクト

Android プロジェクト用の `LabelShadowEffect` の実装を次のコード例に示します。

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

`OnAttached` メソッドで `ShadowEffect` インスタンスを取得し、指定されたプロパティ値を使用して [`TextView.SetShadowLayer`](xref:Android.Widget.TextView.SetShadowLayer*) メソッドを呼び出して影を作成します。 エフェクトがアタッチされているコントロールに `Control.Layer` プロパティがない場合に備えて、この機能が `try`/`catch` ブロック内にラップされます。 クリーンアップする必要がないので、`OnDetached` メソッドによる実装は提供されません。

### <a name="universal-windows-platform-project"></a>ユニバーサル Windows プラットフォーム プロジェクト

ユニバーサル Windows プラットフォーム (UWP) プロジェクト用の `LabelShadowEffect` の実装を次のコード例に示します。

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

ユニバーサル Windows プラットフォームは影エフェクトを提供しないため、両方のプラットフォーム上の `LabelShadowEffect` 実装は 2 番目のオフセット [`Label`](xref:Xamarin.Forms.Label) をプライマリ `Label` の背後に追加することで、その 1 つをシミュレートします。 `OnAttached` メソッドによって `ShadowEffect` インスタンスが取得され、新しい `Label` を作成され、`Label` にいくつかのレイアウト プロパティが設定されます。 次に、`Label` の色と場所を制御する [`TextColor`](xref:Xamarin.Forms.Label.TextColor)、[`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX)、[`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY) プロパティを設定することで影が作成されます。 これにより、`shadowLabel` のプライマリ `Label` の背後にオフセットが挿入されます。 エフェクトがアタッチされているコントロールに `Control.Layer` プロパティがない場合に備えて、この機能が `try`/`catch` ブロック内にラップされます。 クリーンアップする必要がないので、`OnDetached` メソッドによる実装は提供されません。

## <a name="summary"></a>まとめ

この記事では、CLR プロパティを使用してエフェクトにパラメーターを渡す方法について説明しました。 CLR プロパティは、実行時のプロパティの変更に応答しないエフェクトのパラメーターの定義に使用できます。

## <a name="related-links"></a>関連リンク

- [カスタム レンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [Effect](xref:Xamarin.Forms.Effect)
- [PlatformEffect](xref:Xamarin.Forms.PlatformEffect`2)
- [RoutingEffect](xref:Xamarin.Forms.RoutingEffect)
- [影エフェクト (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-shadoweffect)
