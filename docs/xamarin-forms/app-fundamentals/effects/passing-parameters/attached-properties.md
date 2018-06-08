---
title: 添付プロパティとして効果のパラメーターの引き渡し
description: ランタイム プロパティの変更に応答する効果パラメーターを定義する添付プロパティを使用できます。 この記事では、効果、および実行時にパラメーターを変更するパラメーターを渡すプロパティを使用するアタッチを示します。
ms.prod: xamarin
ms.assetid: DFCDCB9F-17DD-4117-BD53-B4FB206BB387
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/05/2016
ms.openlocfilehash: 2ad27289fb7a4d34b9a951c8132f0147577dfc55
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/07/2018
ms.locfileid: "34847918"
---
# <a name="passing-effect-parameters-as-attached-properties"></a>添付プロパティとして効果のパラメーターの引き渡し

_ランタイム プロパティの変更に応答する効果パラメーターを定義する添付プロパティを使用できます。この記事では、効果、および実行時にパラメーターを変更するパラメーターを渡すプロパティを使用するアタッチを示します。_

ランタイム プロパティの変更に応答する効果パラメーターを作成するプロセスは次のとおりです。

1. 作成、`static`効果に渡される各パラメーターの添付プロパティを含むクラスです。
1. 添付プロパティの追加を追加または削除、クラスが接続されているコントロールに影響の制御に使用されるクラスに追加します。 このプロパティのレジスタがアタッチされていることを確認してください、`propertyChanged`プロパティの値が変更されたときに実行されるデリゲート。
1. 作成`static`添付プロパティの get アクセス操作子および set アクセス操作子ごとにします。
1. ロジックを実装、`propertyChanged`デリゲートを追加して、効果を削除します。
1. 内の入れ子になったクラスを実装する、`static`効果、どのサブクラス後という名前のクラス、`RoutingEffect`クラスです。 コンス トラクターで連結したもの、解像度グループ名、および各プラットフォームに固有の効果クラスで指定された一意の ID を渡して基底クラスのコンス トラクターを呼び出します。

パラメーターは、適切なコントロールに接続されているプロパティは、およびプロパティの値を追加することによって効果に渡すことができます。 さらに、新しい添付プロパティの値を指定することによって、実行時にパラメーターを変更できます。

> [!NOTE]
> 添付プロパティは、特殊な種類のクラスとピリオドで区切ったプロパティ名を含む属性として、他のオブジェクトにアタッチされ、XAML で認識可能な 1 つのクラスで定義されているバインド可能なプロパティです。 詳細については、次を参照してください。[添付プロパティ](~/xamarin-forms/xaml/attached-properties.md)です。

サンプル アプリケーションは、`ShadowEffect`によって表示されるテキストに影を追加する、 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)コントロール。 さらに、実行時に影の色を変更できます。 次の図は、両者間のリレーションシップと共に、サンプル アプリケーション内の各プロジェクトの役割を示しています。

![](attached-properties-images/shadow-effect.png "シャドウ効果のプロジェクトの責任")

A [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)の control 権限、`HomePage`によってカスタマイズ、`LabelShadowEffect`プラットフォーム固有のプロジェクトごとにします。 パラメーターが渡される各`LabelShadowEffect`経由でのプロパティをアタッチ、`ShadowEffect`クラスです。 各`LabelShadowEffect`から派生したクラス、`PlatformEffect`各プラットフォームのクラスです。 によって表示されるテキストに追加されているシャドウでこの結果、`Label`を制御する次のスクリーン ショットに示すようにします。

![](attached-properties-images/screenshots.png "各プラットフォームでシャドウ効果")

## <a name="creating-effect-parameters"></a>効果のパラメーターを作成します。

A`static`の次のコード例に示すように、効果パラメーターを表すクラスを作成する必要があります。

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

`ShadowEffect` 5 つの添付プロパティを持つ`static`添付プロパティの get アクセス操作子および set アクセス操作子ごとにします。 各プラットフォーム固有に渡されるパラメーターを表すこれらのプロパティのうち 4 つ`LabelShadowEffect`です。 `ShadowEffect`クラスも定義、`HasShadow`添付プロパティを追加または削除、コントロールに、その効果を制御するために使用する、`ShadowEffect`にクラスをアタッチします。 この添付プロパティに、プロパティの値が変更された時に実行される `OnHasShadowChanged` メソッドを登録します。 このメソッドを追加または削除の値に基づいて有効、`HasShadow`添付プロパティ。

入れ子になった`LabelShadowEffect`クラス、どのサブクラス、 [ `RoutingEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/)クラスをサポートしている効果の追加と削除されます。 `RoutingEffect`クラスは、通常は、プラットフォーム固有の内部の効果をラップするプラットフォームに依存しない効果を表します。 プラットフォーム固有の効果の種類の情報へのコンパイル時アクセスは存在しないために、効果削除プロセスが簡略化します。 `LabelShadowEffect`コンス トラクターは、解像度グループ名、および各プラットフォームに固有の効果クラスで指定された一意の ID の連結で構成されるパラメーターを渡して基底クラスのコンス トラクターを呼び出します。 これにより、効果の追加と削除、`OnHasShadowChanged`メソッドは、次のようにします。

- **追加の影響を与える**– の新しいインスタンス、`LabelShadowEffect`をコントロールの追加は[ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/)コレクション。 これは、置換を使用して、 [ `Effect.Resolve` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Effect.Resolve/p/System.String/)効果を追加するメソッド。
- **削除の影響を与える**– の最初のインスタンス、 `LabelShadowEffect` 、コントロールの[ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/)コレクションが取得され、削除します。

## <a name="consuming-the-effect"></a>効果の使用

各プラットフォームに応じた`LabelShadowEffect`にアタッチされるプロパティを追加することで消費されることができます、 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)コントロールが次の XAML コードの例に示されています。

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

ShadowEffect.SetHasShadow (label, true);
ShadowEffect.SetRadius (label, 5);
ShadowEffect.SetDistanceX (label, 5);
ShadowEffect.SetDistanceY (label, 5);
ShadowEffect.SetColor (label, color));
```

設定、`ShadowEffect.HasShadow`添付プロパティ`true`実行、`ShadowEffect.OnHasShadowChanged`メソッドを追加または削除、`LabelShadowEffect`を[ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)コントロール。 両方のコード例で、`ShadowEffect.Color`添付プロパティがプラットフォーム固有の色の値を提供します。 詳細については、次を参照してください。[デバイス クラス](~/xamarin-forms/platform/device.md)です。

さらに、 [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/)により、実行時に変更する影の色。 ときに、`Button`がクリックすると、影の色を設定して、次のコード変更、`ShadowEffect.Color`添付プロパティ。

```csharp
ShadowEffect.SetColor (label, Color.Teal);
```

### <a name="consuming-the-effect-with-a-style"></a>スタイルと効果の使用

コントロールにアタッチされるプロパティを追加することで利用できる効果をスタイルで使用できます。 XAML コード例を次に、*明示的な*スタイルを適用できるシャドウ効果を[ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)コントロール。

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

[ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)に適用できる、 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)を設定してその[ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/)プロパティを`Style`を使用してインスタンス`StaticResource`マークアップ拡張機能で、次のコード例で示したようにします。

```xaml
<Label Text="Label Shadow Effect" ... Style="{StaticResource ShadowEffectStyle}" />
```

スタイルの詳細については、次を参照してください。[スタイル](~/xamarin-forms/user-interface/styles/index.md)です。

## <a name="creating-the-effect-on-each-platform"></a>各プラットフォームの効果を作成します。

次のセクションでは、説明のプラットフォームに固有の実装、`LabelShadowEffect`クラスです。

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

`OnAttached`メソッドを使用して添付プロパティの値を取得するメソッドを呼び出します、 `ShadowEffect` getter、および設定`Control.Layer`プロパティをシャドウを作成するプロパティの値にします。 この機能をラップする`try` / `catch`ブロックに影響がアタッチされているコントロールがあるない場合に、`Control.Layer`プロパティです。 によって実装が指定されていない、`OnDetached`メソッド クリーンアップする必要がないためです。

#### <a name="responding-to-property-changes"></a>プロパティが変更された場合の対処

いずれかの場合、`ShadowEffect`プロパティ値変更時に、変更内容を表示することによって応答する効果のニーズをアタッチします。 オーバーライドのバージョン、`OnElementPropertyChanged`プラットフォーム固有の効果クラスでのメソッドは、次のコード例に示すようにバインド可能なプロパティの変更に応答する場所は。

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

`OnElementPropertyChanged`メソッドは、radius、色、または、影のオフセットを更新します。 提供される、適切な`ShadowEffect`添付プロパティの値が変更されました。 このオーバーライドは、何度も呼び出すことができるよう、変更されているプロパティのチェックを実行常にする必要があります。

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

`OnAttached`メソッドを使用して添付プロパティの値を取得するメソッドを呼び出します、`ShadowEffect`の getter を呼び出すメソッドを呼び出すと、 [ `TextView.SetShadowLayer` ](https://developer.xamarin.com/api/member/Android.Widget.TextView.SetShadowLayer/p/System.Single/System.Single/System.Single/Android.Graphics.Color/)プロパティ値を使用してシャドウを作成する方法です。 この機能をラップする`try` / `catch`ブロックに影響がアタッチされているコントロールがあるない場合に、`Control.Layer`プロパティです。 によって実装が指定されていない、`OnDetached`メソッド クリーンアップする必要がないためです。

#### <a name="responding-to-property-changes"></a>プロパティが変更された場合の対処

いずれかの場合、`ShadowEffect`プロパティ値変更時に、変更内容を表示することによって応答する効果のニーズをアタッチします。 オーバーライドのバージョン、`OnElementPropertyChanged`プラットフォーム固有の効果クラスでのメソッドは、次のコード例に示すようにバインド可能なプロパティの変更に応答する場所は。

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

`OnElementPropertyChanged`メソッドは、radius、色、または、影のオフセットを更新します。 提供される、適切な`ShadowEffect`添付プロパティの値が変更されました。 このオーバーライドは、何度も呼び出すことができるよう、変更されているプロパティのチェックを実行常にする必要があります。

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

ユニバーサル Windows プラットフォームが影付き効果を提供しないため、`LabelShadowEffect`両方のプラットフォームで実装された 2 つ目のオフセットを追加することでシミュレートする[ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)プライマリの背後にある`Label`です。 `OnAttached`メソッドが、新たに作成`Label`のいくつかのレイアウト プロパティを設定し、`Label`です。 使用して添付プロパティの値を取得するメソッドを呼び出して、 `ShadowEffect` get アクセス操作子を設定して影を作成し、 [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextColor/)、 [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/)、および[ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/)の位置と色を制御するプロパティ、`Label`です。 `shadowLabel`が挿入される、プライマリの背後にあるオフセット`Label`です。 この機能をラップする`try` / `catch`ブロックに影響がアタッチされているコントロールがあるない場合に、`Control.Layer`プロパティです。 によって実装が指定されていない、`OnDetached`メソッド クリーンアップする必要がないためです。

#### <a name="responding-to-property-changes"></a>プロパティが変更された場合の対処

いずれかの場合、`ShadowEffect`プロパティ値変更時に、変更内容を表示することによって応答する効果のニーズをアタッチします。 オーバーライドのバージョン、`OnElementPropertyChanged`プラットフォーム固有の効果クラスでのメソッドは、次のコード例に示すようにバインド可能なプロパティの変更に応答する場所は。

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

`OnElementPropertyChanged`メソッドは、色や、影のオフセットを更新します。 提供される、適切な`ShadowEffect`添付プロパティの値が変更されました。 このオーバーライドは、何度も呼び出すことができるよう、変更されているプロパティのチェックを実行常にする必要があります。

## <a name="summary"></a>まとめ

この記事は、効果、および実行時にパラメーターを変更するパラメーターを渡すプロパティを使用して接続されている説明しました。 ランタイム プロパティの変更に応答する効果パラメーターを定義する添付プロパティを使用できます。


## <a name="related-links"></a>関連リンク

- [カスタム レンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [効果](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/)
- [PlatformEffect](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformEffect%3CTContainer,TControl%3E/)
- [RoutingEffect](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/)
- [影付き効果 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/effects/shadoweffectruntimechange/)
