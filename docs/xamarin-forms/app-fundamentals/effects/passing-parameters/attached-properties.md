---
title: 効果のパラメーターを添付プロパティとして渡す
description: 添付プロパティは、実行時のプロパティの変更に応答する効果のパラメーターの定義に使用できます。 この記事では、添付プロパティを使用して効果にパラメーターを渡す方法と、実行時にパラメーターを変更する方法を示します。
ms.prod: xamarin
ms.assetid: DFCDCB9F-17DD-4117-BD53-B4FB206BB387
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/05/2016
ms.openlocfilehash: d40e1657eb39543023490892b8765ee1fe956ec4
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68645370"
---
# <a name="passing-effect-parameters-as-attached-properties"></a>効果のパラメーターを添付プロパティとして渡す

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-shadoweffectruntimechange)

_添付プロパティは、実行時のプロパティの変更に応答する効果のパラメーターの定義に使用できます。この記事では、添付プロパティを使用して効果にパラメーターを渡す方法と、実行時にパラメーターを変更する方法を示します。_

実行時のプロパティの変更に応答する効果のパラメーターを作成するプロセスは、次のとおりです。

1. 効果に渡される各パラメーターの添付プロパティが含まれる `static` クラスを作成します。
1. 追加の添付プロパティをクラスに追加します。そのクラスは、添付されるコントロールに対する効果の追加または削除をコントロールするために使用されます。 この添付プロパティが、そのプロパティの値が変更されるときに実行される `propertyChanged` デリゲートを登録することを確認します。
1. 各添付プロパティの `static` getter と setter を作成します。
1. 効果を追加または削除する `propertyChanged` デリゲートにロジックを実装します。
1. 効果から命名された `static` クラス内に、入れ子になったクラスを実装します。これは `RoutingEffect` クラスをサブクラス化します。 コンストラクターについては、解決グループ名の連結を渡す基底クラスのコンストラクターと、各プラットフォーム固有の効果のクラスで指定された一意の ID を呼び出します。

その後、添付プロパティとプロパティの値を適切なコントロールに追加することで、パラメーターを効果に渡すことができます。 さらに、パラメーターは実行時に新しい添付プロパティの値を指定することで変更できます。

> [!NOTE]
> 添付プロパティは特殊な型のバインド可能なプロパティで、1 つのクラスで定義される一方で他のオブジェクトに添付され、XAML 内でピリオドで区切られたクラスとプロパティ名が含まれる属性として認識されます。 詳しくは、「[Attached Properties](~/xamarin-forms/xaml/attached-properties.md)」(添付プロパティ) をご覧ください。

サンプル アプリケーションは、[`Label`](xref:Xamarin.Forms.Label) コントロールによって表示されるテキストに影を追加する `ShadowEffect` を示します。 さらに、実行時に影の色を変更できます。 次の図に、サンプル アプリケーション内の各プロジェクトの役割とそれらの関係を示します。

![](attached-properties-images/shadow-effect.png "影効果プロジェクトの役割")

`HomePage` 上の [`Label`](xref:Xamarin.Forms.Label) コントロールが、各プラットフォーム固有のプロジェクト内の `LabelShadowEffect` によってカスタマイズされます。 パラメーターは `ShadowEffect` クラス内の添付プロパティを通じて各 `LabelShadowEffect` に渡されます。 各プラットフォームの `PlatformEffect` クラスから、各 `LabelShadowEffect` クラスが派生します。 これにより、次のスクリーンショットに示すように、`Label` コントロールによって表示されるテキストに影が追加されます。

![](attached-properties-images/screenshots.png "各プラットフォーム上の影効果")

## <a name="creating-effect-parameters"></a>効果のパラメーターの作成

次のコード例に示すように、効果のパラメーターを表すには `static` クラスを作成する必要があります。

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

`ShadowEffect` には 5 つの添付プロパティと、各添付プロパティの `static` の getter と setter が含まれています。 このうちの 4 つのプロパティは各プラットフォーム固有の `LabelShadowEffect` に渡されるパラメーターを表します。 また、`ShadowEffect` クラスは、`ShadowEffect` が添付されるコントロールに対する効果の追加または削除をコントロールするために使用される、`HasShadow` 添付プロパティを定義します。 この添付プロパティは、そのプロパティの値が変更されるときに実行される `OnHasShadowChanged` デリゲートを登録します。 このメソッドは、`HasShadow` 添付プロパティの値に基づいて効果を追加または削除します。

入れ子になった `LabelShadowEffect` クラスは [`RoutingEffect`](xref:Xamarin.Forms.RoutingEffect) クラスをサブクラス化し、効果の追加と削除をサポートします。 `RoutingEffect` クラスは、通常はプラットフォーム固有となる内部の効果をラップするプラットフォームに依存しない効果を表します。 これは、プラットフォーム固有の効果の型情報へのコンパイル時アクセスがないため、効果の削除プロセスを簡略化します。 `LabelShadowEffect` コンストラクターは、解決グループ名の連結を渡す基底クラスのコンストラクターと、各プラットフォーム固有の効果のクラスで指定された一意の ID を呼び出します。 これは、次に示すように、`OnHasShadowChanged` メソッド内の効果の追加と削除を有効にします。

- **効果の追加** - `LabelShadowEffect` の新しいインスタンスがコントロールの [`Effects`](xref:Xamarin.Forms.Element.Effects) コレクションに追加されます。 効果の追加に [`Effect.Resolve`](xref:Xamarin.Forms.Effect.Resolve(System.String)) メソッドを使用する代わりにこれが使用されます。
- **効果の削除** - コントロール内の [`Effects`](xref:Xamarin.Forms.Element.Effects) コレクションの `LabelShadowEffect` の最初のインスタンスが取得され、削除されます。

## <a name="consuming-the-effect"></a>効果の使用

各プラットフォーム固有の `LabelShadowEffect` は、次の XAML コード例に示すように、添付プロパティを [`Label`](xref:Xamarin.Forms.Label) コントロールに追加することで使用できます。

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

ShadowEffect.SetHasShadow (label, true);
ShadowEffect.SetRadius (label, 5);
ShadowEffect.SetDistanceX (label, 5);
ShadowEffect.SetDistanceY (label, 5);
ShadowEffect.SetColor (label, color));
```

`ShadowEffect.HasShadow` 添付プロパティを `true` に設定すると、[`Label`](xref:Xamarin.Forms.Label) コントロールに対して `LabelShadowEffect` を追加または削除する、`ShadowEffect.OnHasShadowChanged` メソッドを実行します。 両方のコード例で、`ShadowEffect.Color` 添付プロパティはプラットフォーム固有の色の値を提供します。 詳しくは、「[Device Class](~/xamarin-forms/platform/device.md)」(デバイス クラス) をご覧ください。

さらに、[`Button`](xref:Xamarin.Forms.Button) は実行時に影の色を変更できます。 `Button` がクリックされると、次のコードは `ShadowEffect.Color` 添付プロパティを設定することで、影の色を変更します。

```csharp
ShadowEffect.SetColor (label, Color.Teal);
```

### <a name="consuming-the-effect-with-a-style"></a>スタイルでの効果の使用

添付プロパティをコントロールに追加することで使用できる効果は、スタイルでも使用できます。 次の XAML コード例は、[`Label`](xref:Xamarin.Forms.Label) コントロールに適用できる、影効果の "*明示的な*" スタイルを示します。

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

[`Style`](xref:Xamarin.Forms.Style) は、次のコード例に示すように、`StaticResource` マークアップ拡張を使用して自身の [`Style`](xref:Xamarin.Forms.NavigableElement.Style) プロパティを `Style` インスタンスに設定することで、[`Label`](xref:Xamarin.Forms.Label) に適用できます。

```xaml
<Label Text="Label Shadow Effect" ... Style="{StaticResource ShadowEffectStyle}" />
```

スタイルについて詳しくは、「[Styles](~/xamarin-forms/user-interface/styles/index.md)」(スタイル) をご覧ください。

## <a name="creating-the-effect-on-each-platform"></a>各プラットフォームでの効果の作成

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

`OnAttached` メソッドは、`ShadowEffect` getter を使用して添付プロパティの値を取得し、`Control.Layer` のプロパティをそのプロパティの値に設定して影を作成するメソッドを呼び出します。 効果が添付されているコントロールに `Control.Layer` プロパティがない場合に備えて、この機能が `try`/`catch` ブロック内にラップされます。 クリーンアップする必要がないので、`OnDetached` メソッドによる実装は提供されません。

#### <a name="responding-to-property-changes"></a>プロパティの変更への応答

`ShadowEffect` 添付プロパティのいずれかの値が実行時に変更されると、その効果はその変更を表示することで応答する必要があります。 プラットフォーム固有の効果クラス内のオーバーライドされたバージョンの `OnElementPropertyChanged` メソッドは、次のコード例に示すように、バインド可能なプロパティの変更に応答する場所です。

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

`ShadowEffect` 添付プロパティの該当する値が変更されると、`OnElementPropertyChanged` メソッドが影の半径、色、またはオフセットを更新します。 このオーバーライドは何度も呼び出されることがあるため、変更になったプロパティのチェックは常に行われる必要があります。

### <a name="android-project"></a>Android プロジェクト

Android プロジェクト用の `LabelShadowEffect` の実装を次のコード例に示します。

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

`OnAttached` メソッドは、`ShadowEffect` getter を使用して添付プロパティの値を取得し、[`TextView.SetShadowLayer`](xref:Android.Widget.TextView.SetShadowLayer*) メソッドを呼び出してそのプロパティの値を使用して影を作成するメソッドを呼び出します。 効果が添付されているコントロールに `Control.Layer` プロパティがない場合に備えて、この機能が `try`/`catch` ブロック内にラップされます。 クリーンアップする必要がないので、`OnDetached` メソッドによる実装は提供されません。

#### <a name="responding-to-property-changes"></a>プロパティの変更への応答

`ShadowEffect` 添付プロパティのいずれかの値が実行時に変更されると、その効果はその変更を表示することで応答する必要があります。 プラットフォーム固有の効果クラス内のオーバーライドされたバージョンの `OnElementPropertyChanged` メソッドは、次のコード例に示すように、バインド可能なプロパティの変更に応答する場所です。

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

`ShadowEffect` 添付プロパティの該当する値が変更されると、`OnElementPropertyChanged` メソッドが影の半径、色、またはオフセットを更新します。 このオーバーライドは何度も呼び出されることがあるため、変更になったプロパティのチェックは常に行われる必要があります。

### <a name="universal-windows-platform-project"></a>ユニバーサル Windows プラットフォーム プロジェクト

ユニバーサル Windows プラットフォーム (UWP) プロジェクト用の `LabelShadowEffect` の実装を次のコード例に示します。

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

ユニバーサル Windows プラットフォームは影効果を提供しないため、両方のプラットフォーム上の `LabelShadowEffect` 実装は 2 番目のオフセット [`Label`](xref:Xamarin.Forms.Label) をプライマリ `Label` の背後に追加することで、その 1 つをシミュレートします。 `OnAttached` メソッドは新しい `Label` を作成し、`Label` にいくつかのレイアウト プロパティを設定します。 その後、`ShadowEffect` の getter を使用して添付プロパティの値を取得するメソッドを呼び出し、[`TextColor`](xref:Xamarin.Forms.Label.TextColor)、[`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX)、および [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY) の各プロパティを設定して `Label` の色と場所をコントロールすることで影を作成します。 これにより、`shadowLabel` のプライマリ `Label` の背後にオフセットが挿入されます。 効果が添付されているコントロールに `Control.Layer` プロパティがない場合に備えて、この機能が `try`/`catch` ブロック内にラップされます。 クリーンアップする必要がないので、`OnDetached` メソッドによる実装は提供されません。

#### <a name="responding-to-property-changes"></a>プロパティの変更への応答

`ShadowEffect` 添付プロパティのいずれかの値が実行時に変更されると、その効果はその変更を表示することで応答する必要があります。 プラットフォーム固有の効果クラス内のオーバーライドされたバージョンの `OnElementPropertyChanged` メソッドは、次のコード例に示すように、バインド可能なプロパティの変更に応答する場所です。

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

`ShadowEffect` 添付プロパティの該当する値が変更されると、`OnElementPropertyChanged` メソッドが影の色またはオフセットを更新します。 このオーバーライドは何度も呼び出されることがあるため、変更になったプロパティのチェックは常に行われる必要があります。

## <a name="summary"></a>まとめ

この記事では、添付プロパティを使って効果にパラメーターを渡す方法と、実行時にパラメーターを変更する方法を示しました。 添付プロパティは、実行時のプロパティの変更に応答する効果のパラメーターの定義に使用できます。


## <a name="related-links"></a>関連リンク

- [カスタム レンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [効果](xref:Xamarin.Forms.Effect)
- [PlatformEffect](xref:Xamarin.Forms.PlatformEffect`2)
- [RoutingEffect](xref:Xamarin.Forms.RoutingEffect)
- [影効果 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-shadoweffectruntimechange)
