---
title: 効果の作成
description: 効果により、コントロールのカスタマイズが簡略化されます。 この記事では、入力コントロールがフォーカスを取得したときにコントロールの背景色を変更する効果の作成方法を示します。
ms.prod: xamarin
ms.assetid: 9E2C8DB0-36A2-4F13-8E3C-A66D7021DB13
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2016
ms.openlocfilehash: f1c18c30a2da019fb9d1c09fac17c9f095dafedc
ms.sourcegitcommit: c6e56545eafd8ff9e540d56aba32aa6232c5315f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/02/2019
ms.locfileid: "68739346"
---
# <a name="creating-an-effect"></a>効果の作成

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-focuseffect)

"_効果により、コントロールのカスタマイズが簡略化されます。この記事では、入力コントロールがフォーカスを取得したときにコントロールの背景色を変更する効果の作成方法を示します。_ "

各プラットフォーム固有のプロジェクトで効果を作成するプロセスは次のとおりです。

1. `PlatformEffect` クラスのサブクラスを作成します。
1. `OnAttached` メソッドをオーバーライドし、コントロールをカスタマイズするロジックを記述します。
1. `OnDetached` メソッドをオーバーライドし、コントロールのカスタマイズをクリーンアップするロジックを記述します (必要な場合)。
1. [`ResolutionGroupName`](xref:Xamarin.Forms.ResolutionGroupNameAttribute) 属性を効果クラスに追加します。 この属性により、会社全体の名前空間が設定され、同じ名前を持つその他の効果との競合が防止されます。 この属性はプロジェクトごとに 1 回のみ 適用できることに注意してください。
1. [`ExportEffect`](xref:Xamarin.Forms.ExportEffectAttribute) 属性を効果クラスに追加します。 この属性によって、Xamarin.Forms で使用される一意の ID とコントロールに適用する前に効果を検索するためのグループ名付きで効果が登録されます。 この属性では、効果の種類名と、コントロールに適用する前に効果を検索するために使用される一意の文字列という 2 つのパラメーターが使用されます。

その後、適切なコントロールにアタッチすることで、効果を使用できます。

> [!NOTE]
> 各プラットフォーム プロジェクトに効果を提供するかどうかは任意です。 効果が登録されていない場合にそれを使用しようとすると、何も実行しない null 以外の値が返されます。

サンプル アプリケーションでは、コントロールがフォーカスを取得したときに背景色を変更する `FocusEffect` を示します。 次の図に、サンプル アプリケーション内の各プロジェクトの役割とそれらの関係を示します。

![](creating-images/focus-effect.png "フォーカス効果プロジェクトの責任")

`HomePage` 上の [`Entry`](xref:Xamarin.Forms.Entry) コントロールが、各プラットフォーム固有のプロジェクト内の `FocusEffect` クラスによってカスタマイズされます。 各プラットフォームの `PlatformEffect` クラスから、各 `FocusEffect` クラスが派生します。 この結果、次のスクリーンショットに示すように、プラットフォーム固有の背景色を使用して `Entry` コントロールがレンダリングされ、フォーカスを取得するとその色が変更されます。

![](creating-images/screenshots-1.png "各プラットフォームでのフォーカス効果")
![](creating-images/screenshots-2.png "各プラットフォームでのフォーカス効果")

## <a name="creating-the-effect-on-each-platform"></a>各プラットフォームでの効果の作成

次のセクションで、`FocusEffect` クラスのプラットフォーム固有の実装について説明します。

## <a name="ios-project"></a>iOS プロジェクト

iOS プロジェクト用の `FocusEffect` の実装を次のコード例に示します。

```csharp
using Xamarin.Forms;
using Xamarin.Forms.Platform.iOS;

[assembly:ResolutionGroupName ("MyCompany")]
[assembly:ExportEffect (typeof(EffectsDemo.iOS.FocusEffect), nameof(EffectsDemo.iOS.FocusEffect))]
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

`OnAttached` メソッドでは、`UIColor.FromRGB` メソッドを使用してコントロールを紫色にすると共にその色をフィールドに格納する `BackgroundColor` プロパティ が設定されます。 効果がアタッチされているコントロールに `BackgroundColor` プロパティがない場合に備えて、この機能が `try`/`catch` ブロック内にラップされます。 クリーンアップする必要がないので、`OnDetached` メソッドによる実装は提供されません。

`OnElementPropertyChanged` オーバーライドで、Xamarin.Forms コントロールのバインド可能プロパティの変更に応答します。 [`IsFocused`](xref:Xamarin.Forms.VisualElement.IsFocused) プロパティが変更されたときに、コントロールがフォーカスを取得している場合は、コントロールの `BackgroundColor` プロパティが白に変更され、それ以外の場合は明るい紫に変更されます。 効果がアタッチされているコントロールに `BackgroundColor` プロパティがない場合に備えて、この機能が `try`/`catch` ブロック内にラップされます。

## <a name="android-project"></a>Android プロジェクト

Android プロジェクト用の `FocusEffect` の実装を次のコード例に示します。

```csharp
using System;
using Xamarin.Forms;
using Xamarin.Forms.Platform.Android;

[assembly: ResolutionGroupName("MyCompany")]
[assembly: ExportEffect(typeof(EffectsDemo.Droid.FocusEffect), nameof(EffectsDemo.Droid.FocusEffect))]
namespace EffectsDemo.Droid
{
    public class FocusEffect : PlatformEffect
    {
        Android.Graphics.Color originalBackgroundColor = new Android.Graphics.Color(0, 0, 0, 0);
        Android.Graphics.Color backgroundColor;

        protected override void OnAttached()
        {
            try
            {
                backgroundColor = Android.Graphics.Color.LightGreen;
                Control.SetBackgroundColor(backgroundColor);
            }
            catch (Exception ex)
            {
                Console.WriteLine("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached()
        {
        }

        protected override void OnElementPropertyChanged(System.ComponentModel.PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged(args);
            try
            {
                if (args.PropertyName == "IsFocused")
                {
                    if (((Android.Graphics.Drawables.ColorDrawable)Control.Background).Color == backgroundColor)
                    {
                        Control.SetBackgroundColor(originalBackgroundColor);
                    }
                    else
                    {
                        Control.SetBackgroundColor(backgroundColor);
                    }
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine("Cannot set property on attached control. Error: ", ex.Message);
            }
        }
    }
}
```

`OnAttached` メソッドでは、`SetBackgroundColor` メソッドを呼び出して、コントロールの背景色を明るい緑にすると共にその色をフィールドに格納します。 効果がアタッチされているコントロールに `SetBackgroundColor` プロパティがない場合に備えて、この機能が `try`/`catch` ブロック内にラップされます。 クリーンアップする必要がないので、`OnDetached` メソッドによる実装は提供されません。

`OnElementPropertyChanged` オーバーライドで、Xamarin.Forms コントロールのバインド可能プロパティの変更に応答します。 [`IsFocused`](xref:Xamarin.Forms.VisualElement.IsFocused) プロパティが変更されたときに、コントロールがフォーカスを取得している場合は、コントロールの背景色が白に変更され、それ以外の場合は明るい緑に変更されます。 効果がアタッチされているコントロールに `BackgroundColor` プロパティがない場合に備えて、この機能が `try`/`catch` ブロック内にラップされます。

## <a name="universal-windows-platform-projects"></a>ユニバーサル Windows プラットフォーム プロジェクト

ユニバーサル Windows プラットフォーム (UWP) プロジェクト用の `FocusEffect` の実装を次のコード例に示します。

```csharp
using Xamarin.Forms;
using Xamarin.Forms.Platform.UWP;

[assembly: ResolutionGroupName("MyCompany")]
[assembly: ExportEffect(typeof(EffectsDemo.UWP.FocusEffect), nameof(EffectsDemo.UWP.FocusEffect))]
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

`OnAttached` メソッドでは、コントロールの `Background` プロパティをシアンに設定し、`BackgroundFocusBrush` プロパティを白に設定します。 効果がアタッチされているコントロールにこれらのプロパティがない場合に備えて、この機能が `try`/`catch` ブロック内にラップされます。 クリーンアップする必要がないので、`OnDetached` メソッドによる実装は提供されません。

## <a name="consuming-the-effect"></a>効果の使用

Xamarin.Forms .NET Standard ライブラリまたは共有ライブラリ プロジェクトから効果を使用するプロセスは次のとおりです。

1. 効果によってカスタマイズされるコントロールを宣言します。
1. コントロールの [`Effects`](xref:Xamarin.Forms.Element.Effects) コレクションに追加することによって、効果をコントロールにアタッチします。

> [!NOTE]
> 効果インスタンスは単一のコントロールにのみアタッチできます。 このため、2 つのコントロールで使用するには、効果を 2 回解決する必要があります。

## <a name="consuming-the-effect-in-xaml"></a>XAML での効果の使用

`FocusEffect` がアタッチされている [`Entry`](xref:Xamarin.Forms.Entry) コントロールを、次の XAML コード例に示します。

```xaml
<Entry Text="Effect attached to an Entry" ...>
    <Entry.Effects>
        <local:FocusEffect />
    </Entry.Effects>
    ...
</Entry>
```

.NET Standard ライブラリ内の `FocusEffect` クラスでは、XAML での効果の使用がサポートされます。これを次のコード例に示します。

```csharp
public class FocusEffect : RoutingEffect
{
    public FocusEffect () : base ($"MyCompany.{nameof(FocusEffect)}")
    {
    }
}
```

`FocusEffect` クラスで [`RoutingEffect`](xref:Xamarin.Forms.RoutingEffect) クラスがサブクラス化されます。これは、通常はプラットフォーム固有である内部効果をラップするプラットフォームに依存しない効果を表します。 `FocusEffect` クラスで基底クラス コンストラクターが呼び出され、解決グループ名 (効果クラスの [`ResolutionGroupName`](xref:Xamarin.Forms.ResolutionGroupNameAttribute) 属性を使用して指定されます) と効果クラスの [`ExportEffect`](xref:Xamarin.Forms.ExportEffectAttribute) 属性を使用して指定された一意の ID の連結を構成するパラメーターが渡されます。 このため、実行時に [`Entry`](xref:Xamarin.Forms.Entry) が初期化されるときに、`MyCompany.FocusEffect` の新しいインスタンスがコントロールの [`Effects`](xref:Xamarin.Forms.Element.Effects) コレクションに追加されます。

ビヘイビアーを使用するか、アタッチされるプロパティを使用して、効果をアタッチすることもできます。 ビヘイビアーを使用してコントロールに効果をアタッチする方法の詳細については、「[Reusable EffectBehavior](~/xamarin-forms/app-fundamentals/behaviors/reusable/effect-behavior.md)」(再利用可能な EffectBehavior) を参照してください。 アタッチされるプロパティを使用してコントロールに効果をアタッチする方法の詳細については、「[Passing Parameters to an Effect](~/xamarin-forms/app-fundamentals/effects/passing-parameters/index.md)」(効果にパラメーターを渡す) を参照してください。

## <a name="consuming-the-effect-in-cnum"></a>C&num; での効果の使用

C# での同等の [`Entry`](xref:Xamarin.Forms.Entry) を次のコード例に示します。

```csharp
var entry = new Entry {
  Text = "Effect attached to an Entry",
  ...
};
```

次のコード例に示すように、効果をコントロールの [`Effects`](xref:Xamarin.Forms.Element.Effects) コレクションに追加することで、`FocusEffect` が `Entry` インスタンスにアタッチされます。

```csharp
public HomePageCS ()
{
  ...
  entry.Effects.Add (Effect.Resolve ($"MyCompany.{nameof(FocusEffect)}"));
  ...
}
```

[`Effect.Resolve`](xref:Xamarin.Forms.Effect.Resolve(System.String)) によって、指定された名前用の [`Effect`](xref:Xamarin.Forms.Effect) が返されます。これは解決グループ名 (効果クラスの [`ResolutionGroupName`](xref:Xamarin.Forms.ResolutionGroupNameAttribute) 属性を使用して指定されます) と効果クラスの [`ExportEffect`](xref:Xamarin.Forms.ExportEffectAttribute) 属性を使用して指定された一意の ID の連結を構成するパラメーターです。 プラットフォームで効果が提供されない場合、`Effect.Resolve` メソッドでは、`null` 以外の値が返されます。

## <a name="summary"></a>まとめ

この記事では、[`Entry`](xref:Xamarin.Forms.Entry) コントロールがフォーカスを取得したときにコントロールの背景色を変更する効果の作成方法を示しました。


## <a name="related-links"></a>関連リンク

- [カスタム レンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [効果](xref:Xamarin.Forms.Effect)
- [PlatformEffect](xref:Xamarin.Forms.PlatformEffect`2)
- [背景色効果 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-backgroundcoloreffect)
- [フォーカス効果 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-focuseffect)
