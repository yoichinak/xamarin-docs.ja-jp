---
title: プラットフォーム固有設定
description: プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。 この記事では、プラットフォーム固有のを使用して作成する方法について説明します。
ms.prod: xamarin
ms.assetid: 4729DB9C-8800-4E29-9D66-3BE13C5F8C94
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/01/2018
ms.openlocfilehash: f6190b9c0d29d57d6d509bdff25e2ce3572e3a3c
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/13/2020
ms.locfileid: "79305733"
---
# <a name="platform-specifics"></a>プラットフォーム固有設定

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

_プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。_

XAML、またはコードの fluent API を通じてプラットフォームに固有の使用するためのプロセスは次のとおりです。

1. [`Xamarin.Forms.PlatformConfiguration`](xref:Xamarin.Forms.PlatformConfiguration)名前空間の `xmlns` 宣言または `using` ディレクティブを追加します。
1. プラットフォーム固有の機能を含む名前空間の `xmlns` 宣言または `using` ディレクティブを追加します。
    1. IOS では、これは[`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)名前空間です。
    1. Android では、これは[`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)名前空間です。 Android AppCompat の場合、これは[`Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)名前空間です。
    1. ユニバーサル Windows プラットフォームでは、これが[`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)名前空間です。
1. XAML から、または `On<T>` fluent API を持つコードから、プラットフォーム固有のを適用します。 `T` の値は、 [`Xamarin.Forms.PlatformConfiguration`](xref:Xamarin.Forms.PlatformConfiguration)名前空間の[`iOS`](xref:Xamarin.Forms.PlatformConfiguration.iOS)、 [`Android`](xref:Xamarin.Forms.PlatformConfiguration.Android)、または[`Windows`](xref:Xamarin.Forms.PlatformConfiguration.Windows)型にすることができます。

> [!NOTE]
> プラットフォーム仕様を利用できないプラットフォーム上で、プラットフォーム仕様の使おうとしてもエラーにはならないことに注意してください。 そのかわり、そのコードはプラットフォーム仕様の適用なしで実行されます。

`On<T>` fluent code API を介して使用されるプラットフォームの詳細は[`IPlatformElementConfiguration`](xref:Xamarin.Forms.IPlatformElementConfiguration`2)オブジェクトを返します。 これにより、メソッドのカスケードを同じオブジェクトで呼び出される複数のプラットフォーム固有です。

Xamarin によって提供されるプラットフォームの詳細については、「 [IOS プラットフォーム-](~/xamarin-forms/platform/ios/index.md)詳細」、「 [Android プラットフォーム](~/xamarin-forms/platform/android/index.md)の詳細」、および「 [Windows プラットフォームの](~/xamarin-forms/platform/windows/index.md)詳細」を参照してください。

## <a name="creating-platform-specifics"></a>プラットフォーム固有の作成

ベンダーは Effect を使って、独自の platform-specific を作成することができます。 特定の機能を Effect で提供し、それを platform-specific を通して公開します。 その結果、 Effect は、 XAML や C# の fluent API を通して、より簡単に使えるようになります。

Platform-specific を作成する手順は次のとおりです。

1. 特定の機能を Effect として実装します。 詳細については、「[効果の作成](~/xamarin-forms/app-fundamentals/effects/creating.md)」を参照してください。
1. 作成した Effect を公開する platform-specific クラスを作成します。 詳細については、「[プラットフォーム固有のクラスの作成](#creating-a-platform-specific-class)」を参照してください。
1. Platform-Specific クラスに XAML から使用できるようにするために、添付プロパティを実装します。 詳細については、「[添付プロパティの追加](#adding-an-attached-property)」を参照してください。
1. Platform-Specific クラスに fluent API から使用できるようにするために、拡張メソッドを実装します。 詳細については、「[拡張メソッドの追加](#adding-extension-methods)」を参照してください。
1. Platform-specific が Effect と同じプラットフォーム上で呼び出された場合にのみ、その Effect が適用されるように Effect の実装を修正します。 詳細については、「[効果の作成](#creating-the-effect)」を参照してください。

Platform-specific として Effect を公開すると、 fluent API や XAML を通して、より簡単に Effect を使えるようになります。

> [!NOTE]
> ベンダーがユーザーの負担軽減のために、この技術を使って、独自の platform-specific を作成することを想定しています。 ユーザーが独自の platform-specific の作成を選択しようとする場合は、 Effect を作成して使用する場合より多くのコードが要求されるということに注意すべきです。

この[サンプルアプリケーション](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shadowplatformspecific)は、 [`Label`](xref:Xamarin.Forms.Label)コントロールによって表示されるテキストに影を追加する `Shadow` プラットフォーム固有のコードを示しています。

![](images/screenshots.png "Shadow Platform-Specific")

この[サンプルアプリケーション](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shadowplatformspecific)は、理解しやすくするために、各プラットフォームでプラットフォーム固有の `Shadow` を実装しています。 しかし、それぞれの platform-specific の Effect 実装以外の Shadow クラスの実装は 各プラットフォームで大部分は同じです。 したがって、このガイドでは Shadow クラスと 1つのプラットフォームに関連付けられる Effect の実装に焦点を合わせます。

効果の詳細については、「[効果を持つコントロールのカスタマイズ](~/xamarin-forms/app-fundamentals/effects/index.md)」を参照してください。

### <a name="creating-a-platform-specific-class"></a>プラットフォーム固有のクラスの作成

プラットフォーム固有のは `public static` クラスとして作成されます。

```csharp
namespace MyCompany.Forms.PlatformConfiguration.iOS
{
  public static Shadow
  {
    ...
  }
}
```

以下のセクションでは、`Shadow` プラットフォーム固有および関連する効果の実装について説明します。

#### <a name="adding-an-attached-property"></a>添付プロパティの追加

XAML で使用できるようにするには、添付プロパティを `Shadow` プラットフォーム固有のに追加する必要があります。

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

`IsShadowed` 添付プロパティは、`Shadow` クラスがアタッチされているコントロールに `MyCompany.LabelShadowEffect` 効果を追加し、それを削除するために使用されます。 この添付プロパティにより、プロパティの値が変更されたときに実行される `OnIsShadowedPropertyChanged` デリゲートが登録されます。 次に、このメソッドは `AttachEffect` または `DetachEffect` メソッドを呼び出して、`IsShadowed` 添付プロパティの値に基づいて効果を追加または削除します。 コントロールの[`Effects`](xref:Xamarin.Forms.Element.Effects)コレクションを変更することによって、コントロールに対して効果が追加または削除されます。

> [!NOTE]
> Effect は resolution group 名と Effect の実装を特定するユニークな識別子を連結した値によって決定されることに注意してください。 詳細については、「[効果の作成](~/xamarin-forms/app-fundamentals/effects/creating.md)」を参照してください。

添付プロパティについて詳しくは、「[添付プロパティ](~/xamarin-forms/xaml/attached-properties.md)」をご覧ください。

#### <a name="adding-extension-methods"></a>拡張メソッドを追加

Fluent コード API を使用して使用できるようにするには、拡張メソッドを `Shadow` プラットフォーム固有のに追加する必要があります。

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

`IsShadowed` および `SetIsShadowed` の拡張メソッドは、それぞれ `IsShadowed` 添付プロパティの get アクセサーと set アクセサーを呼び出します。 各拡張メソッドは `IPlatformElementConfiguration<iOS, FormsElement>` 型で動作します。これは、iOS の[`Label`](xref:Xamarin.Forms.Label)インスタンスでプラットフォーム固有のを呼び出すことができることを指定します。

#### <a name="creating-the-effect"></a>効果を作成する

プラットフォーム固有の `Shadow` により、`MyCompany.LabelShadowEffect` が[`Label`](xref:Xamarin.Forms.Label)に追加され、削除されます。 iOS プロジェクト用の `LabelShadowEffect` の実装を次のコード例に示します。

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

`UpdateShadow` メソッドは、`Control.Layer` プロパティを設定してシャドウを作成します。これには、`IsShadowed` 添付プロパティが `true`に設定されていて、`Shadow` プラットフォーム固有のが、その効果が実装されているプラットフォームで呼び出されていることが必要です。 このチェックは、`OnThisPlatform` メソッドを使用して実行されます。

実行時に `Shadow.IsShadowed` 添付プロパティ値が変更された場合、その効果はシャドウを削除することによって応答する必要があります。 したがって、オーバーライドされた `OnElementPropertyChanged` メソッドのバージョンは、`UpdateShadow` メソッドを呼び出すことによって、バインド可能なプロパティの変更に応答するために使用されます。

効果を作成する方法の詳細については、「[効果の作成](~/xamarin-forms/app-fundamentals/effects/creating.md)」と「効果の[パラメーターを添付プロパティとして渡す](~/xamarin-forms/app-fundamentals/effects/passing-parameters/attached-properties.md)」を参照してください。

### <a name="consuming-the-platform-specific"></a>プラットフォーム固有の使用

`Shadow` プラットフォーム固有のは、`Shadow.IsShadowed` 添付プロパティを `boolean` 値に設定することによって XAML で使用されます。

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

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [ShadowPlatformSpecific (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shadowplatformspecific)
- [iOS プラットフォーム-詳細](~/xamarin-forms/platform/ios/index.md)
- [Android プラットフォーム-詳細](~/xamarin-forms/platform/android/index.md)
- [Windows プラットフォーム-詳細](~/xamarin-forms/platform/windows/index.md)
- [特殊効果を使用したコントロールのカスタマイズ](~/xamarin-forms/app-fundamentals/effects/index.md)
- [関連付けられたプロパティ](~/xamarin-forms/xaml/attached-properties.md)
- [PlatformConfiguration API](xref:Xamarin.Forms.PlatformConfiguration)
