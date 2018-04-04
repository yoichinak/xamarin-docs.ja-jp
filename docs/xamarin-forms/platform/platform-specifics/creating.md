---
title: プラットフォーム固有の作成
description: ベンダーは、影響が、独自のプラットフォーム固有を作成できます。 効果は、プラットフォーム固有では公開し、特定の機能を提供します。 簡単に使用できる XAML、およびコードの fluent API を使用して有効になります。 この記事では、プラットフォーム固有で、特殊効果を公開する方法を示します。
ms.prod: xamarin
ms.assetid: 0D0E6274-6EF2-4D40-BB77-3D8E53BCD24B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/23/2016
ms.openlocfilehash: 6283e22d75d9e52ad3e2f300617818c98d887481
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="creating-platform-specifics"></a>プラットフォーム固有の作成

_ベンダーは、影響が、独自のプラットフォーム固有を作成できます。効果は、プラットフォーム固有では公開し、特定の機能を提供します。簡単に使用できる XAML、およびコードの fluent API を使用して有効になります。この記事では、プラットフォーム固有で、特殊効果を公開する方法を示します。_

## <a name="overview"></a>概要

プラットフォーム固有の仕様を作成するプロセスは次のとおりです。

1. 効果として特定の機能を実装します。 詳細については、次を参照してください。[効果を作成する](~/xamarin-forms/app-fundamentals/effects/creating.md)です。
1. 影響を公開するプラットフォームに固有のクラスを作成します。 詳細については、次を参照してください。[プラットフォームに固有のクラスを作成する](#creating)です。
1. プラットフォーム固有のクラスでは、XAML で使用されるプラットフォーム固有を許可する添付プロパティを実装します。 詳細については、次を参照してください。[添付プロパティを追加する](#attached_property)です。
1. プラットフォーム固有のクラスでは、コードの fluent API で使用するプラットフォーム固有のようにする拡張メソッドを実装します。 詳細については、次を参照してください。[拡張メソッドを追加する](#extension_methods)です。
1. 効果の実装を変更して、効果と同一のプラットフォームにプラットフォーム固有が呼び出されて場合にのみ効果が適用されます。 詳細については、次を参照してください。[効果を作成する](#creating_the_effect)です。

プラットフォーム固有の仕様と効果を公開するための結果は、こと効果より簡単に使用できる XAML およびコードの fluent API を使用してです。

> [!NOTE]
> ベンダーがこの手法を使用してのユーザーによる使用量を容易にするための独自のプラットフォームの仕様を作成することにした目標がします。 ユーザーは、独自のプラットフォーム固有の作成を選択中に、注意してください、特殊効果の作成とより多くのコードを必要とすることです。

サンプル アプリケーションは、`Shadow`によって表示されるテキストに影をプラットフォームに固有な[ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)コントロール。

![](creating-images/screenshots.png "プラットフォーム固有をシャドウします。")

サンプル アプリケーションを実装して、`Shadow`理解しやすいのため、各プラットフォームでは、プラットフォームに固有です。 ただし、各プラットフォームに固有の効果実装とは別にシャドウ クラスの実装は各プラットフォームのとほぼ同じです。 そのため、このガイドは、シャドウ クラスと関連付けられている 1 つのプラットフォームへの影響の実装での焦点を合わせます。

影響の詳細については、次を参照してください。[効果 Customizing Controls](~/xamarin-forms/app-fundamentals/effects/index.md)です。

<a name="creating" />

## <a name="creating-a-platform-specific-class"></a>プラットフォーム固有のクラスを作成します。

プラットフォーム固有の仕様として作成、`public static`クラス。

```csharp
namespace MyCompany.Forms.PlatformConfiguration.iOS
{
  public static Shadow
  {
    ...
  }
}
```

次のセクションでは、説明の実装、`Shadow`プラットフォーム固有および関連する効果。

<a name="attached_property" />

### <a name="adding-an-attached-property"></a>添付プロパティを追加します。

添付プロパティに追加する必要があります、`Shadow`プラットフォームに固有の XAML での使用を許可します。

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

`IsShadowed`添付プロパティを使用して、追加、`MyCompany.LabelShadowEffect`の効果をおよびコントロールから削除を`Shadow`にクラスが接続されています。 これは、添付プロパティのレジスタ、`OnIsShadowedPropertyChanged`プロパティの値が変更されたときに実行されるメソッド。 このメソッドを呼び出す、`AttachEffect`または`DetachEffect`の値に基づいてメソッドを追加または削除の影響、`IsShadowed`添付プロパティ。 効果を追加またはコントロールの変更することにより、コントロールから削除された[ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/)コレクション。

> [!NOTE]
> 効果は、グループ名の解決と効果の実装で指定されている一意識別子の連結である値を指定することによって解決されたことに注意してください。 詳細については、次を参照してください。[効果を作成する](~/xamarin-forms/app-fundamentals/effects/creating.md)です。

添付プロパティの詳細については、次を参照してください。[添付プロパティ](~/xamarin-forms/xaml/attached-properties.md)です。

<a name="extension_methods" />

### <a name="adding-extension-methods"></a>拡張メソッドを追加

拡張メソッドに追加する必要があります、`Shadow`プラットフォームに固有のコードの fluent API を介して使用を許可します。

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

`IsShadowed`と`SetIsShadowed`拡張メソッドは、get を呼び出すし、set アクセサーの`IsShadowed`添付プロパティをそれぞれします。 各拡張メソッドの動作で、`IPlatformElementConfiguration<iOS, FormsElement>`型、プラットフォーム固有で起動できることを指定する[ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) iOS からのインスタンス。

<a name="creating_the_effect" />

### <a name="creating-the-effect"></a>効果を作成します。

`Shadow`プラットフォーム固有の追加、`MyCompany.LabelShadowEffect`を[ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)、し、削除します。 次のコード例は、 `LabelShadowEffect` iOS プロジェクトの実装。

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

`UpdateShadow`メソッドのセット`Control.Layer`、シャドウを作成するプロパティが用意されている、`IsShadowed`に添付プロパティを設定`true`、`Shadow`同一のプラットフォームにプラットフォーム固有の仕様が呼び出されている、効果が実装されます。 このチェックは実行で、`OnThisPlatform`メソッドです。

場合、`Shadow.IsShadowed`プロパティ値の変更、実行時に影を削除することで応答する効果のニーズをアタッチします。 そのため、オーバーライドのバージョン、`OnElementPropertyChanged`メソッドを使用して呼び出すことによって、バインド可能なプロパティの変更に応答する、`UpdateShadow`メソッドです。

エフェクトの作成の詳細については、次を参照してください。[効果を作成する](~/xamarin-forms/app-fundamentals/effects/creating.md)と[添付プロパティとして有効パラメーターの引き渡し](~/xamarin-forms/app-fundamentals/effects/passing-parameters/attached-properties.md)です。

## <a name="consuming-a-platform-specific"></a>プラットフォーム固有の使用

`Shadow`を設定してプラットフォーム固有が XAML で使用される、`Shadow.IsShadowed`添付プロパティを`boolean`値。

```xaml
<ContentPage xmlns:ios="clr-namespace:MyCompany.Forms.PlatformConfiguration.iOS" ...>
  ...
  <Label Text="Label Shadow Effect" ios:Shadow.IsShadowed="true" ... />
  ...
</ContentPage>
```

または、fluent API を使用して、c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using MyCompany.Forms.PlatformConfiguration.iOS;

...

shadowLabel.On<iOS>().SetIsShadowed(true);
```

プラットフォーム固有の使用の詳細については、次を参照してください。[消費してプラットフォーム固有](~/xamarin-forms/platform/platform-specifics/consuming/index.md)です。

## <a name="summary"></a>まとめ

この記事では、プラットフォーム固有で、特殊効果を公開する方法を示しました。 簡単に使用できる XAML、およびコードの fluent API を使用して有効になります。


## <a name="related-links"></a>関連リンク

- [ShadowPlatformSpecific (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/shadowplatformspecific/)
- [効果を持つコントロールのカスタマイズ](~/xamarin-forms/app-fundamentals/effects/index.md)
- [関連付けられたプロパティ](~/xamarin-forms/xaml/attached-properties.md)
