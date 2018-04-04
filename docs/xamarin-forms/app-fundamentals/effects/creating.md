---
title: 特殊効果を作成します。
description: 効果は、コントロールのカスタマイズを簡略化します。 この記事では、コントロールがフォーカスを取得するときに、入力コントロールの背景色を変更する効果を作成する方法を示します。
ms.prod: xamarin
ms.assetid: 9E2C8DB0-36A2-4F13-8E3C-A66D7021DB13
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2016
ms.openlocfilehash: 773636cf879439477a6f71e44f13ae66b8f10ea8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="creating-an-effect"></a>特殊効果を作成します。

_効果は、コントロールのカスタマイズを簡略化します。この記事では、コントロールがフォーカスを取得するときに、入力コントロールの背景色を変更する効果を作成する方法を示します。_

各プラットフォームに固有のプロジェクトで、特殊効果を作成するプロセスは次のとおりです。

1. サブクラスを作成、`PlatformEffect`クラスです。
1. 上書き、`OnAttached`コントロールをカスタマイズする方法と書き込みのロジックです。
1. 上書き、`OnDetached`メソッドと書き込みのロジックが必要な場合に、コントロールのカスタマイズをクリーンアップします。
1. 追加、 [ `ResolutionGroupName` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResolutionGroupNameAttribute/)効果クラスに属性します。 この属性は、特殊効果、その他の効果と同じ名前の衝突を回避するための会社ワイド名前空間を設定します。 この属性適用できることのみ 1 回プロジェクトごとに注意してください。
1. 追加、 [ `ExportEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ExportEffectAttribute/)効果クラスに属性します。 この属性は、影響をコントロールに適用する前に影響を検索すると、グループ名、Xamarin.Forms で使用される一意の ID を登録します。 属性は、次の 2 つのパラメーター –、効果、およびコントロールに適用する前に影響を検索するために使用する一意の文字列の型名を受け取ります。

効果は、適切なコントロールにアタッチして使用できます。

> [!NOTE]
> 各プラットフォームのプロジェクトで効果を提供する省略可能であります。 いずれかが登録されていない場合に効果を使用しようとしています。 には、何も null 以外の値が返されます。

サンプル アプリケーションは、`FocusEffect`フォーカスを得るとき、コントロールの背景色を変更します。 次の図は、両者間のリレーションシップと共に、サンプル アプリケーション内の各プロジェクトの役割を示しています。

![](creating-images/focus-effect.png "フォーカス効果プロジェクトの責任")

[ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)の control 権限、`HomePage`によってカスタマイズ、`FocusEffect`各プラットフォームに固有のプロジェクト内のクラスです。 各`FocusEffect`から派生したクラス、`PlatformEffect`各プラットフォームのクラスです。 これは、結果、`Entry`コントロールがフォーカスする場合に変更すると、次のスクリーン ショットに示すように、プラットフォーム固有の背景の色でレンダリングされるかを制御します。

![](creating-images/screenshots-1.png "各プラットフォームには影響を集中")
![](creating-images/screenshots-2.png "プラットフォームごとに影響を集中")

## <a name="creating-the-effect-on-each-platform"></a>各プラットフォームの効果を作成します。

次のセクションでは、説明のプラットフォームに固有の実装、`FocusEffect`クラスです。

## <a name="ios-project"></a>iOS プロジェクト

次のコード例は、 `FocusEffect` iOS プロジェクトの実装。

```csharp
using Xamarin.Forms;
using Xamarin.Forms.Platform.iOS;

[assembly:ResolutionGroupName ("MyCompany")]
[assembly:ExportEffect (typeof(FocusEffect), "FocusEffect")]
namespace EffectsDemo.iOS
{
    public class FocusEffect : PlatformEffect
    {
        UIColor backgroundColor;

        protected override void OnAttached ()
        {
            try {
                Control.BackgroundColor = backgroundColor = UIColor.FromRGB (204, 153, 255);
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }

        protected override void OnElementPropertyChanged (PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged (args);

            try {
                if (args.PropertyName == "IsFocused") {
                    if (Control.BackgroundColor == backgroundColor) {
                        Control.BackgroundColor = UIColor.White;
                    } else {
                        Control.BackgroundColor = backgroundColor;
                    }
                }
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }
    }
}
```

`OnAttached`メソッドのセット、`BackgroundColor`とライト紫色にコントロールのプロパティ、`UIColor.FromRGB`メソッド、し、またこの色をフィールドに格納します。 この機能をラップする`try` / `catch`ブロックに影響が接続されているコントロールがあるない場合に、`BackgroundColor`プロパティです。 によって実装が指定されていない、`OnDetached`メソッド クリーンアップする必要がないためです。

`OnElementPropertyChanged`オーバーライドが Xamarin.Forms コントロールにバインド可能なプロパティの変更に応答します。 ときに、 [ `IsFocused` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsFocused/)プロパティの変更、`BackgroundColor`コントロールのプロパティが白に変更されるコントロールにフォーカスがある場合、それ以外の場合ライト紫に変更されます。 この機能をラップする`try` / `catch`ブロックに影響が接続されているコントロールがあるない場合に、`BackgroundColor`プロパティです。

## <a name="android-project"></a>Android Project

次のコード例は、 `FocusEffect` Android プロジェクトの実装。

```csharp
using Xamarin.Forms;
using Xamarin.Forms.Platform.Android;

[assembly:ResolutionGroupName ("MyCompany")]
[assembly:ExportEffect (typeof(FocusEffect), "FocusEffect")]
namespace EffectsDemo.Droid
{
    public class FocusEffect : PlatformEffect
    {
        Android.Graphics.Color backgroundColor;

        protected override void OnAttached ()
        {
            try {
                backgroundColor = Android.Graphics.Color.LightGreen;
                Control.SetBackgroundColor (backgroundColor);

            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }

        protected override void OnElementPropertyChanged (System.ComponentModel.PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged (args);
            try {
                if (args.PropertyName == "IsFocused") {
                    if (((Android.Graphics.Drawables.ColorDrawable)Control.Background).Color == backgroundColor) {
                        Control.SetBackgroundColor (Android.Graphics.Color.Black);
                    } else {
                        Control.SetBackgroundColor (backgroundColor);
                    }
                }
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }
    }
}
```

`OnAttached`メソッドの呼び出し、`SetBackgroundColor`光を当てるコントロールの背景色を設定するメソッドを緑、およびフィールドにもこのカラーを格納します。 この機能をラップする`try` / `catch`ブロックに影響が接続されているコントロールがあるない場合に、`SetBackgroundColor`プロパティです。 によって実装が指定されていない、`OnDetached`メソッド クリーンアップする必要がないためです。

`OnElementPropertyChanged`オーバーライドが Xamarin.Forms コントロールにバインド可能なプロパティの変更に応答します。 ときに、 [ `IsFocused` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsFocused/)プロパティの変更、コントロールの背景色が白に変更されるコントロールにフォーカスがある場合、それ以外の場合明るい緑に変更されます。 この機能をラップする`try` / `catch`ブロックに影響が接続されているコントロールがあるない場合に、`BackgroundColor`プロパティです。

## <a name="windows-phone--universal-windows-platform-projects"></a>Windows Phone とユニバーサル Windows プラットフォーム プロジェクト

次のコード例は、 `FocusEffect` Windows Phone およびユニバーサル Windows プラットフォーム (UWP) プロジェクトの実装。

```csharp
using Xamarin.Forms;
using Xamarin.Forms.Platform.WinRT;

[assembly: ResolutionGroupName("MyCompany")]
[assembly: ExportEffect(typeof(FocusEffect), "FocusEffect")]
namespace EffectsDemo.WinPhone81
{
    public class FocusEffect : PlatformEffect
    {
        protected override void OnAttached()
        {
            try
            {
                (Control as Windows.UI.Xaml.Controls.Control).Background = new SolidColorBrush(Colors.Cyan);
                (Control as FormsTextBox).BackgroundFocusBrush = new SolidColorBrush(Colors.White);
            }
            catch (Exception ex)
            {
                Debug.WriteLine("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached()
        {
        }
    }
}
```

`OnAttached`メソッドのセット、`Background`シアン、およびセットへのコントロールのプロパティ、`BackgroundFocusBrush`プロパティを白にします。 この機能をラップする`try` / `catch`に効果が接続されているコントロールには、これらのプロパティがない場合にブロックします。 によって実装が指定されていない、`OnDetached`メソッド クリーンアップする必要がないためです。

## <a name="consuming-the-effect"></a>効果の使用

Xamarin.Forms ポータブル クラス ライブラリ (PCL) または共有ライブラリ プロジェクトから効果を使用するためのプロセスは次のとおりです。

1. 効果は、カスタマイズする、コントロールを宣言します。
1. コントロールの追加して、コントロールに影響をアタッチ[ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/)コレクション。

> [!NOTE]
> 効果インスタンスは、1 つのコントロールにのみアタッチできます。 このため、2 つのコントロールで使用するには、2 回、特殊効果を解決する必要があります。

## <a name="consuming-the-effect-in-xaml"></a>XAML の使用

XAML コード例を次に、 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)先のコントロール、`FocusEffect`がアタッチされています。

```xaml
<Entry Text="Effect attached to an Entry" ...>
    <Entry.Effects>
        <local:FocusEffect />
    </Entry.Effects>
    ...
</Entry>
```

`FocusEffect` PCL にクラスを XAML では、効果の消費をサポートします。 次のコード例に示します。

```csharp
public class FocusEffect : RoutingEffect
{
    public FocusEffect () : base ("MyCompany.FocusEffect")
    {
    }
}
```

`FocusEffect`クラスのサブクラス、 [ `RoutingEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/)クラスで、通常は、プラットフォーム固有の内部の効果をラップするプラットフォームに依存しない効果を表します。 `FocusEffect`クラスを連結した名前解決のグループ名から成るパラメーターを渡して基底クラスのコンス トラクターを呼び出します (を使用して指定、 [ `ResolutionGroupName` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResolutionGroupNameAttribute/)効果クラスの属性)、される一意の ID と使用して指定された、 [ `ExportEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ExportEffectAttribute/)効果クラスの属性です。 したがって、ときに、 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)は実行時の新しいインスタンスを初期化、`MyCompany.FocusEffect`をコントロールの追加[ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/)コレクション。

効果は、添付することもコントロールに、動作を使用してまたはアタッチされるプロパティを使用しています。 動作を使用して、コントロールへの効果のアタッチの詳細については、次を参照してください。[再利用可能な EffectBehavior](~/xamarin-forms/app-fundamentals/behaviors/reusable/effect-behavior.md)です。 添付プロパティを使用して、コントロールへの効果のアタッチの詳細については、次を参照してください。[効果にパラメーターを渡す](~/xamarin-forms/app-fundamentals/effects/passing-parameters/index.md)です。

## <a name="consuming-the-effect-in-cnum"></a>C での影響の使用&num;

該当するショートカットは、 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) c# では次のコード例で示すようにします。

```csharp
var entry = new Entry {
  Text = "Effect attached to an Entry",
  ...
};
```

`FocusEffect`にアタッチされて、`Entry`をコントロールの効果を追加することによってインスタンス[ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/)次のコード例で示したように、コレクション。

```csharp
public HomePageCS ()
{
  ...
  entry.Effects.Add (Effect.Resolve ("MyCompany.FocusEffect"));
  ...
}
```

[ `Effect.Resolve` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Effect.Resolve/p/System.String/)を返します、 [ `Effect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/)を連結した名前解決のグループ名は、指定した名前の (を使用して指定、 [ `ResolutionGroupName` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResolutionGroupNameAttribute/)効果クラスの属性)、および一意の ID を使用して指定された、 [ `ExportEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ExportEffectAttribute/)効果クラスの属性です。 プラットフォームには、効果が用意されていない場合、`Effect.Resolve`メソッドが返す以外`null`値。

## <a name="summary"></a>まとめ

この記事の背景色を変更する効果を作成する方法を説明する、 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)コントロールがフォーカスを取得する場合を制御します。


## <a name="related-links"></a>関連リンク

- [カスタム レンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [効果](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/)
- [PlatformEffect](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformEffect%3CTContainer,TControl%3E/)
- [背景色の効果 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/effects/backgroundcoloreffect/)
- [フォーカスの効果 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/effects/focuseffect/)
