---
title: Platform Specific の作成
description: この記事では platform-specific を通して Effect を公開する方法を説明します。
ms.prod: xamarin
ms.assetid: 0D0E6274-6EF2-4D40-BB77-3D8E53BCD24B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/23/2016
ms.openlocfilehash: 1d9f07a089eabedf07bef49c9815fe7e93128f09
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997257"
---
# <a name="creating-platform-specifics"></a>Platform Specific の作成

_ベンダーは Effects を使って、独自の platform-specifics を作成することができます。特定の機能を Effect で提供し、それを platform-specific を通して公開します。その結果、 Effect は、 XAML や C# の fluent API を通して、より簡単に使えるようになります。この記事では platform-specific を通して Effect を公開する方法を説明します。_

## <a name="overview"></a>概要

Platform-specific を作成する手順は次のとおりです。

1. 特定の機能を Effect として実装します。 詳細については、[Effect の作成](~/xamarin-forms/app-fundamentals/effects/creating.md) を参照してください。
1. 作成した Effect を公開する platform-specific クラスを作成します。 詳細については、[Platform-Specific クラスの作成](#creating)を参照してください。
1. Platform-Specific クラスに XAML から使用できるようにするために、添付プロパティを実装します。 詳細については、[添付プロパティの追加](#attached_property)を参照してください。
1. Platform-Specific クラスに fluent API から使用できるようにするために、拡張メソッドを実装します。 詳細については、[拡張メソッドの追加](#extension_methods)を参照してください。
1. Platform-specific が Effect と同じプラットフォーム上で呼び出された場合にのみ、その Effect が適用されるように Effect の実装を修正します。 詳細については、[Effect の作成](#creating_the_effect)を参照してください。

Platform-specific として Effect を公開すると、 fluent API や XAML を通して、より簡単に Effect を使えるようになります。

> [!NOTE]
> ベンダーがユーザーの負担軽減のために、この技術を使って、独自の platform-specific を作成することを想定しています。 ユーザーが独自の platform-specific の作成を選択しようとする場合は、 Effect を作成して使用する場合より多くのコードが要求されるということに注意すべきです。

サンプルアプリケーションでは、 [`Label`](xref:Xamarin.Forms.Label) コントロールによって表示されるテキストに影を加える `Shadow` platform-specific について説明します。

![](creating-images/screenshots.png "Shadow Platform-Specific")

サンプルアプリケーションでは、理解しやすいように、各プラットフォームで `Shadow` platform-specific を実装します。 しかし、それぞれの platform-specific の Effect 実装以外の Shadow クラスの実装は 各プラットフォームで大部分は同じです。 したがって、このガイドでは Shadow クラスと 1つのプラットフォームに関連付けられる Effect の実装に焦点を合わせます。

Effect についての詳細は、[Effect を使った コントロールのカスタマイズ](~/xamarin-forms/app-fundamentals/effects/index.md)を参照してください。

<a name="creating" />

## <a name="creating-a-platform-specific-class"></a>Platform-Specific クラスの作成

Platform-specific を `public static` クラスとして作成します。

```csharp
namespace MyCompany.Forms.PlatformConfiguration.iOS
{
  public static Shadow
  {
    ...
  }
}
```

次のセクションでは、`Shadow` platform-specific の実装と 関連づける Effect について説明します。

<a name="attached_property" />

### <a name="adding-an-attached-property"></a>添付プロパティの追加

XAML から使用可能にするために `Shadow` platform-specific に添付プロパティを追加します。

```csharp
namespace MyCompany.Forms.PlatformConfiguration.iOS
{
    using System.Linq;
    using Xamarin.Forms;
    using Xamarin.Forms.PlatformConfiguration;
    using FormsElement = Xamarin.Forms.Label;

    public static class Shadow
    {
        const string EffectName = "MyCompany.LabelShadowEffect";

        public static readonly BindableProperty IsShadowedProperty =
            BindableProperty.CreateAttached("IsShadowed",
                                            typeof(bool),
                                            typeof(Shadow),
                                            false,
                                            propertyChanged: OnIsShadowedPropertyChanged);

        public static bool GetIsShadowed(BindableObject element)
        {
            return (bool)element.GetValue(IsShadowedProperty);
        }

        public static void SetIsShadowed(BindableObject element, bool value)
        {
            element.SetValue(IsShadowedProperty, value);
        }

        ...

        static void OnIsShadowedPropertyChanged(BindableObject element, object oldValue, object newValue)
        {
            if ((bool)newValue)
            {
                AttachEffect(element as FormsElement);
            }
            else
            {
                DetachEffect(element as FormsElement);
            }
        }

        static void AttachEffect(FormsElement element)
        {
            IElementController controller = element;
            if (controller == null || controller.EffectIsAttached(EffectName))
            {
                return;
            }
            element.Effects.Add(Effect.Resolve(EffectName));
        }

        static void DetachEffect(FormsElement element)
        {
            IElementController controller = element;
            if (controller == null || !controller.EffectIsAttached(EffectName))
            {
                return;
            }

            var toRemove = element.Effects.FirstOrDefault(e => e.ResolveId == Effect.Resolve(EffectName).ResolveId);
            if (toRemove != null)
            {
                element.Effects.Remove(toRemove);
            }
        }
    }
}
```

`IsShadowed` 添付プロパティは、`MyCompany.LabelShadowEffect` クラスが添付されたコントロールに `Shadow` の Effect を追加したり取り除いたりするために使われます。 この添付プロパティに、プロパティの値が変更された時に実行される `OnIsShadowedPropertyChanged` メソッドを登録します。 次に、このメソッドは `IsShadowed` 添付プロパティの値に基づいて Effect を追加したり取り除いたりするための `AttachEffect` または `DetachEffect` メソッドを呼び出します。 コントロールの [`Effects`](xref:Xamarin.Forms.Element.Effects) コレクションを変更することによって、 その Effect をコントロールに追加したり取り除いたりします。

> [!NOTE]
> Effect は resolution group 名と Effect の実装を特定するユニークな識別子を連結した値によって決定されることに注意してください。 詳細については、[Effect の作成](~/xamarin-forms/app-fundamentals/effects/creating.md)を参照してください。

添付プロパティの詳細については、[添付プロパティ](~/xamarin-forms/xaml/attached-properties.md)を参照してください。

<a name="extension_methods" />

### <a name="adding-extension-methods"></a>拡張メソッドを追加

fluent API を通じて使用できるようにするためには、 `Shadow` platform-specific に拡張メソッドを追加する必要があります。

```csharp
namespace MyCompany.Forms.PlatformConfiguration.iOS
{
    using System.Linq;
    using Xamarin.Forms;
    using Xamarin.Forms.PlatformConfiguration;
    using FormsElement = Xamarin.Forms.Label;

    public static class Shadow
    {
        ...
        public static bool IsShadowed(this IPlatformElementConfiguration<iOS, FormsElement> config)
        {
            return GetIsShadowed(config.Element);
        }

        public static IPlatformElementConfiguration<iOS, FormsElement> SetIsShadowed(this IPlatformElementConfiguration<iOS, FormsElement> config, bool value)
        {
            SetIsShadowed(config.Element, value);
            return config;
        }
        ...
    }
}
```

`IsShadowed` と `SetIsShadowed` 拡張メソッドは、それぞれ `IsShadowed` 添付プロパティのための get / set アクセサーを呼び出します。 各拡張メソッドは、 `IPlatformElementConfiguration<iOS, FormsElement>` 型で操作します。これによって platform-specific を iOS から [`Label`](xref:Xamarin.Forms.Label) のインスタンスを使って呼び出すことができるようになります。

<a name="creating_the_effect" />

### <a name="creating-the-effect"></a>Effect の作成

`Shadow` platform-specific は、 `MyCompany.LabelShadowEffect` を [`Label`](xref:Xamarin.Forms.Label) に追加したり削除したりします。 次のコードサンプルでは、 iOS プロジェクトの `LabelShadowEffect` 実装を示します。

```csharp
[assembly: ResolutionGroupName("MyCompany")]
[assembly: ExportEffect(typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace ShadowPlatformSpecific.iOS
{
    public class LabelShadowEffect : PlatformEffect
    {
        protected override void OnAttached()
        {
            UpdateShadow();
        }

        protected override void OnDetached()
        {
        }

        protected override void OnElementPropertyChanged(PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged(args);

            if (args.PropertyName == Shadow.IsShadowedProperty.PropertyName)
            {
                UpdateShadow();
            }
        }

        void UpdateShadow()
        {
            try
            {
                if (((Label)Element).OnThisPlatform().IsShadowed())
                {
                    Control.Layer.CornerRadius = 5;
                    Control.Layer.ShadowColor = UIColor.Black.CGColor;
                    Control.Layer.ShadowOffset = new CGSize(5, 5);
                    Control.Layer.ShadowOpacity = 1.0f;
                }
                else if (!((Label)Element).OnThisPlatform().IsShadowed())
                {
                    Control.Layer.ShadowOpacity = 0;
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

`UpdateShadow` メソッドは、 `IsShadowed` 添付プロパティに `true` がセットされ、 `Shadow` platform-specific が Effectの実装と同一のプラットフォームで実行されると、影を作るために `Control.Layer` プロパティを設定します このチェックは `OnThisPlatform` メソッドを使って実行されます。

`Shadow.IsShadowed` 添付プロパティの値が実行時に変更された場合、 Effect は影を削除することで応答する必要があります。 そのため、 `OnElementPropertyChanged` メソッド のオーバーライドを使って、 `UpdateShadow` メソッドを呼び出すことによって、 バインド可能なプロパティの変更に対応します。

Effect の作成の詳細については、 [Effect の作成](~/xamarin-forms/app-fundamentals/effects/creating.md)と [添付プロパティを使って Effect のパラメーターを渡す](~/xamarin-forms/app-fundamentals/effects/passing-parameters/attached-properties.md)を参照してください。

## <a name="consuming-a-platform-specific"></a>Platform-Specific の使用

`Shadow` platform-specific は XAML で `Shadow.IsShadowed` 添付プロパティに `boolean` 値を設定して使用します。

```xaml
<ContentPage xmlns:ios="clr-namespace:MyCompany.Forms.PlatformConfiguration.iOS" ...>
  ...
  <Label Text="Label Shadow Effect" ios:Shadow.IsShadowed="true" ... />
  ...
</ContentPage>
```

代わりに、fluent API を使用して c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using MyCompany.Forms.PlatformConfiguration.iOS;

...

shadowLabel.On<iOS>().SetIsShadowed(true);
```

Platform-Specific の使用の詳細については、 [Platform-Specific の使用](~/xamarin-forms/platform/platform-specifics/consuming/index.md)を参照してください。

## <a name="summary"></a>まとめ

この記事では、 platform-specific を通じて Effect を公開する方法を説明しました。 その結果、 Effect は、 XAML や C# の fluent API を通して、より簡単に使えるようになります。


## <a name="related-links"></a>関連リンク

- [ShadowPlatformSpecific (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/shadowplatformspecific/)
- [Effect を使ったコントロールのカスタマイズ](~/xamarin-forms/app-fundamentals/effects/index.md)
- [添付プロパティ](~/xamarin-forms/xaml/attached-properties.md)
