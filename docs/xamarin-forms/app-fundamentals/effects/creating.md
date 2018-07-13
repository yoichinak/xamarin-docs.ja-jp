---
title: 効果を作成します。
description: 効果は、コントロールのカスタマイズを簡略化します。 この記事では、コントロールがフォーカスを獲得したときに、入力コントロールの背景色を変更する効果を作成する方法を示します。
ms.prod: xamarin
ms.assetid: 9E2C8DB0-36A2-4F13-8E3C-A66D7021DB13
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2016
ms.openlocfilehash: b29d83999724a35293882f7b9efc0158171c4fd2
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998161"
---
# <a name="creating-an-effect"></a>効果を作成します。

_効果は、コントロールのカスタマイズを簡略化します。この記事では、コントロールがフォーカスを獲得したときに、入力コントロールの背景色を変更する効果を作成する方法を示します。_

各プラットフォーム固有プロジェクトで効果を作成するためのプロセスは次のとおりです。

1. サブクラスを作成、`PlatformEffect`クラス。
1. 上書き、`OnAttached`制御をカスタマイズする方法と書き込みのロジック。
1. 上書き、`OnDetached`メソッドと書き込みのロジックを必要な場合は、コントロールのカスタマイズをクリーンアップします。
1. 追加、 [ `ResolutionGroupName` ](xref:Xamarin.Forms.ResolutionGroupNameAttribute)効果クラスに属性します。 この属性は、その他の効果と同じ名前の競合を防ぐ効果の会社のワイド名前空間を設定します。 この属性適用できることのみ 1 回プロジェクトごとに注意してください。
1. 追加、 [ `ExportEffect` ](xref:Xamarin.Forms.ExportEffectAttribute)効果クラスに属性します。 この属性は、コントロールに適用する前に、効果を検索するグループ名と共に、Xamarin.Forms で使用される一意の id の効果を登録します。 属性は、2 つのパラメーター –、効果、およびコントロールに適用する前に、効果を検索するために使用する一意の文字列の型名を受け取ります。

効果は、適切なコントロールにアタッチして使用できます。

> [!NOTE]
> 各プラットフォーム プロジェクトに効果を提供する省略可能になります。 1 つが登録されていない場合に効果を使用すると、何も実行しない null 以外の値を返します。

サンプル アプリケーションでは、`FocusEffect`フォーカスを得るときにコントロールの背景色を変更します。 次の図は、サンプル アプリケーションとそれらの間のリレーションシップ内の各プロジェクトの役割を示します。

![](creating-images/focus-effect.png "フォーカス効果プロジェクトの責任")

[ `Entry` ](xref:Xamarin.Forms.Entry)の control 権限、`HomePage`によってカスタマイズされた、`FocusEffect`各プラットフォーム固有プロジェクト内のクラス。 各`FocusEffect`クラスから派生、`PlatformEffect`各プラットフォームのクラス。 これは、結果、`Entry`次のスクリーン ショットに示すように、コントロールがフォーカスを獲得したときの変更、プラットフォーム固有のバック グラウンドの色でレンダリングされるかを制御します。

![](creating-images/screenshots-1.png "各プラットフォームで効果を集中")
![](creating-images/screenshots-2.png "集中各プラットフォームへの影響")

## <a name="creating-the-effect-on-each-platform"></a>各プラットフォームで効果を作成します。

次のセクションでは、プラットフォーム固有の実装の説明、`FocusEffect`クラス。

## <a name="ios-project"></a>iOS プロジェクト

次のコードサンプルでは、 iOS プロジェクトの `FocusEffect` 実装を示します。

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

`OnAttached`メソッドのセット、`BackgroundColor`ライト紫でコントロールのプロパティ、`UIColor.FromRGB`メソッド、しもこの色をフィールドに格納します。 この機能にラップされて、 `try` / `catch`ブロックに効果が接続されているコントロールがあるない場合に、`BackgroundColor`プロパティ。 によって実装が指定されていない、`OnDetached`メソッド クリーンアップする必要がないので。

`OnElementPropertyChanged`オーバーライドは、Xamarin.Forms コントロールにバインド可能なプロパティの変更に応答します。 ときに、 [ `IsFocused` ](xref:Xamarin.Forms.VisualElement.IsFocused)プロパティの変更、`BackgroundColor`コントロールのプロパティが白に変更されるコントロールにフォーカスがある場合、それ以外の場合ライト紫に変更されます。 この機能にラップされて、 `try` / `catch`ブロックに効果が接続されているコントロールがあるない場合に、`BackgroundColor`プロパティ。

## <a name="android-project"></a>Android プロジェクト

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

`OnAttached`メソッドの呼び出し、`SetBackgroundColor`光をコントロールの背景色を設定するメソッドを緑、およびフィールドにこの色も格納されます。 この機能にラップされて、 `try` / `catch`ブロックに効果が接続されているコントロールがあるない場合に、`SetBackgroundColor`プロパティ。 によって実装が指定されていない、`OnDetached`メソッド クリーンアップする必要がないので。

`OnElementPropertyChanged`オーバーライドは、Xamarin.Forms コントロールにバインド可能なプロパティの変更に応答します。 ときに、 [ `IsFocused` ](xref:Xamarin.Forms.VisualElement.IsFocused)プロパティの変更をコントロールの背景色が白に変更されるコントロールにフォーカスがある場合、それ以外の場合明るい緑に変更されます。 この機能にラップされて、 `try` / `catch`ブロックに効果が接続されているコントロールがあるない場合に、`BackgroundColor`プロパティ。

## <a name="universal-windows-platform-projects"></a>ユニバーサル Windows プラットフォーム プロジェクト

次のコード例は、`FocusEffect`ユニバーサル Windows プラットフォーム (UWP) プロジェクトの実装。

```csharp
using Xamarin.Forms;
using Xamarin.Forms.Platform.UWP;

[assembly: ResolutionGroupName("MyCompany")]
[assembly: ExportEffect(typeof(FocusEffect), "FocusEffect")]
namespace EffectsDemo.UWP
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

`OnAttached`メソッドのセット、`Background`シアン、およびセットへのコントロールのプロパティ、`BackgroundFocusBrush`プロパティを白にします。 この機能にラップされて、 `try` / `catch`ブロックする場合に効果が接続されているコントロールにこれらのプロパティが不足しています。 によって実装が指定されていない、`OnDetached`メソッド クリーンアップする必要がないので。

## <a name="consuming-the-effect"></a>効果の使用

Xamarin.Forms .NET Standard ライブラリまたは共有ライブラリ プロジェクトから効果を使用するためのプロセスは次のとおりです。

1. 効果はカスタマイズするコントロールを宣言します。
1. コントロールの追加することによってコントロールに影響をアタッチ[ `Effects` ](xref:Xamarin.Forms.Element.Effects)コレクション。

> [!NOTE]
> 効果インスタンスは、1 つのコントロールにのみ接続できます。 このため、効果は、2 つのコントロールで使用するには、2 回解決する必要があります。

## <a name="consuming-the-effect-in-xaml"></a>XAML で効果の使用

次の XAML コード例は、 [ `Entry` ](xref:Xamarin.Forms.Entry)コントロールを`FocusEffect`がアタッチされています。

```xaml
<Entry Text="Effect attached to an Entry" ...>
    <Entry.Effects>
        <local:FocusEffect />
    </Entry.Effects>
    ...
</Entry>
```

`FocusEffect` .NET Standard ライブラリのクラスが XAML で効果の消費をサポートし、次のコード例に示した。

```csharp
public class FocusEffect : RoutingEffect
{
    public FocusEffect () : base ("MyCompany.FocusEffect")
    {
    }
}
```

`FocusEffect`クラスのサブクラスの[ `RoutingEffect` ](xref:Xamarin.Forms.RoutingEffect)クラスで、通常はプラットフォーム固有の内部の効果をラップするプラットフォームに依存しない効果を表します。 `FocusEffect`クラスは、解像度のグループ名の連結で構成されるパラメーターを渡して基底クラスのコンス トラクターを呼び出します (を使用して指定、 [ `ResolutionGroupName` ](xref:Xamarin.Forms.ResolutionGroupNameAttribute)効果クラスの属性)、一意の ID と使用して指定された、 [ `ExportEffect` ](xref:Xamarin.Forms.ExportEffectAttribute)効果クラスの属性。 そのため、ときに、 [ `Entry` ](xref:Xamarin.Forms.Entry)の新しいインスタンスを実行時に初期化されます、`MyCompany.FocusEffect`をコントロールの追加は[ `Effects` ](xref:Xamarin.Forms.Element.Effects)コレクション。

効果は、動作を使用してコントロールにも接続するまたはを使用して添付プロパティ。 動作を使用して、コントロールに効果を添付する方法の詳細については、次を参照してください。[再利用可能な EffectBehavior](~/xamarin-forms/app-fundamentals/behaviors/reusable/effect-behavior.md)します。 添付プロパティを使用して、コントロールに効果を添付する方法の詳細については、次を参照してください。[効果にパラメーターを渡す](~/xamarin-forms/app-fundamentals/effects/passing-parameters/index.md)します。

## <a name="consuming-the-effect-in-cnum"></a>C で効果の使用&num;

相当[ `Entry` ](xref:Xamarin.Forms.Entry) c# では次のコード例で示すようにします。

```csharp
var entry = new Entry {
  Text = "Effect attached to an Entry",
  ...
};
```

`FocusEffect`にアタッチされて、`Entry`をコントロールの効果を追加することによってインスタンス[ `Effects` ](xref:Xamarin.Forms.Element.Effects)の次のコード例に示す、コレクション。

```csharp
public HomePageCS ()
{
  ...
  entry.Effects.Add (Effect.Resolve ("MyCompany.FocusEffect"));
  ...
}
```

[ `Effect.Resolve` ](xref:Xamarin.Forms.Effect.Resolve(System.String))を返します、 [ `Effect` ](xref:Xamarin.Forms.Effect)解像度のグループ名の連結は、指定した名前に対する (を使用して指定、 [ `ResolutionGroupName` ](xref:Xamarin.Forms.ResolutionGroupNameAttribute)効果クラスの属性)、および一意の ID を使用して指定された、 [ `ExportEffect` ](xref:Xamarin.Forms.ExportEffectAttribute)効果クラスの属性。 プラットフォームは、効果に備わっていない場合、`Effect.Resolve`メソッドは以外`null`値。

## <a name="summary"></a>まとめ

この記事では、の背景色を変更する効果を作成する方法を示しました、 [ `Entry` ](xref:Xamarin.Forms.Entry)コントロールがフォーカスを取得するときを制御します。


## <a name="related-links"></a>関連リンク

- [カスタム レンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [効果](xref:Xamarin.Forms.Effect)
- [PlatformEffect](xref:Xamarin.Forms.PlatformEffect`2)
- [背景色の効果 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/effects/backgroundcoloreffect/)
- [フォーカスの効果 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/effects/focuseffect/)
