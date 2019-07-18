---
title: プラットフォーム仕様
description: プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。 この記事では、使用し、プラットフォーム固有設定を作成する方法について説明します。
ms.prod: xamarin
ms.assetid: 4729DB9C-8800-4E29-9D66-3BE13C5F8C94
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/01/2018
ms.openlocfilehash: 04cbdaac50b0ea77659d7c495dcd1a9e6d43335c
ms.sourcegitcommit: b23a107b0fe3d2f814ae35b52a5855b6ce2a3513
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/20/2019
ms.locfileid: "65926992"
---
# <a name="platform-specifics"></a>プラットフォーム仕様

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)

_プラットフォーム仕様は、 カスタム レンダラーや特殊効果を実装することなく、特定のプラットフォーム上でのみ利用できる機能の使用を可能にします。_

XAML、またはコードの fluent API を通じてプラットフォームに固有の使用するためのプロセスは次のとおりです。

1. [`Xamarin.Forms.PlatformConfiguration`](xref:Xamarin.Forms.PlatformConfiguration)名前空間に`xmlns`宣言または`using`ディレクティブを追加します。
1. プラットフォーム仕様の機能が含まれる名前空間に`xmlns`宣言または`using`ディレクティブを追加します
    1. iOS の場合、[`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)名前空間です。
    1. Android の場合は、[`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)名前空間です。 Android AppCompat の場合は、 [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)名前空間です。
    1. ユニバーサル Windows プラットフォームの場合は、[`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)名前空間です。
1. XAML から、または `On<T>` fluent API を含むコードからプラットフォーム仕様を適用します。 `T`の値には、[`Xamarin.Forms.PlatformConfiguration`](xref:Xamarin.Forms.PlatformConfiguration)名前空間から [`iOS`](xref:Xamarin.Forms.PlatformConfiguration.iOS)、[`Android`](xref:Xamarin.Forms.PlatformConfiguration.Android)、または [`Windows`](xref:Xamarin.Forms.PlatformConfiguration.Windows)の型を指定できます。

> [!NOTE]
> プラットフォーム仕様を利用できないプラットフォーム上で、プラットフォーム仕様の使おうとしてもエラーにはならないことに注意してください。 そのかわり、そのコードはプラットフォーム仕様の適用なしで実行されます。

プラットフォーム仕様は、[`IPlatformElementConfiguration`](xref:Xamarin.Forms.IPlatformElementConfiguration`2)オブジェクトを返すコードの `On<T>` fluent API を通じて使用します。 これにより、メソッドのカスケードを同じオブジェクトで呼び出される複数のプラットフォーム固有です。

Xamarin.Forms によって提供されるプラットフォーム固有の詳細については、次を参照してください[iOS プラットフォーム固有](~/xamarin-forms/platform/ios/index.md)、 [Android プラットフォーム固有](~/xamarin-forms/platform/android/index.md)、および[Windows プラットフォーム-詳細](~/xamarin-forms/platform/windows/index.md)。

## <a name="creating-platform-specifics"></a>プラットフォーム固有設定の作成

ベンダーは Effect を使って、独自の platform-specific を作成することができます。 特定の機能を Effect で提供し、それを platform-specific を通して公開します。 その結果、 Effect は、 XAML や C# の fluent API を通して、より簡単に使えるようになります。

Platform-specific を作成する手順は次のとおりです。

1. 特定の機能を Effect として実装します。 詳細については、[Effect の作成](~/xamarin-forms/app-fundamentals/effects/creating.md) を参照してください。
1. 作成した Effect を公開する platform-specific クラスを作成します。 詳細については、[Platform-Specific クラスの作成](#creating-a-platform-specific-class)を参照してください。
1. Platform-Specific クラスに XAML から使用できるようにするために、添付プロパティを実装します。 詳細については、[添付プロパティの追加](#adding-an-attached-property)を参照してください。
1. Platform-Specific クラスに fluent API から使用できるようにするために、拡張メソッドを実装します。 詳細については、[拡張メソッドの追加](#adding-extension-methods)を参照してください。
1. Platform-specific が Effect と同じプラットフォーム上で呼び出された場合にのみ、その Effect が適用されるように Effect の実装を修正します。 詳細については、[Effect の作成](#creating-the-effect)を参照してください。

Platform-specific として Effect を公開すると、 fluent API や XAML を通して、より簡単に Effect を使えるようになります。

> [!NOTE]
> ベンダーがユーザーの負担軽減のために、この技術を使って、独自の platform-specific を作成することを想定しています。 ユーザーが独自の platform-specific の作成を選択しようとする場合は、 Effect を作成して使用する場合より多くのコードが要求されるということに注意すべきです。

[サンプル アプリケーション](https://developer.xamarin.com/samples/xamarin-forms/userinterface/shadowplatformspecific/)示します、`Shadow`によって表示されるテキストに影をプラットフォームに固有の[ `Label` ](xref:Xamarin.Forms.Label)コントロール。

![](images/screenshots.png "Shadow Platform-Specific")

[サンプル アプリケーション](https://developer.xamarin.com/samples/xamarin-forms/userinterface/shadowplatformspecific/)実装、`Shadow`について理解しやすくするため、各プラットフォームでプラットフォームに固有です。 しかし、それぞれの platform-specific の Effect 実装以外の Shadow クラスの実装は 各プラットフォームで大部分は同じです。 したがって、このガイドでは Shadow クラスと 1つのプラットフォームに関連付けられる Effect の実装に焦点を合わせます。

Effect についての詳細は、[Effect を使った コントロールのカスタマイズ](~/xamarin-forms/app-fundamentals/effects/index.md)を参照してください。

### <a name="creating-a-platform-specific-class"></a>プラットフォーム固有のクラスを作成します。

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

#### <a name="adding-an-attached-property"></a>添付プロパティを追加します。

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

#### <a name="adding-extension-methods"></a>拡張メソッドを追加

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

#### <a name="creating-the-effect"></a>効果を作成します。

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

### <a name="consuming-the-platform-specific"></a>プラットフォーム固有の使用

`Shadow` platform-specific は XAML で `Shadow.IsShadowed` 添付プロパティに `boolean` 値を設定して使用します。

```xaml
<ContentPage xmlns:ios="clr-namespace:MyCompany.Forms.PlatformConfiguration.iOS" ...>
  ...
  <Label Text="Label Shadow Effect" ios:Shadow.IsShadowed="true" ... />
  ...
</ContentPage>
```

代わりに、fluent API を使用して C# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using MyCompany.Forms.PlatformConfiguration.iOS;

...

shadowLabel.On<iOS>().SetIsShadowed(true);
```

## <a name="related-links"></a>関連リンク

- [プラットフォーム仕様 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)
- [ShadowPlatformSpecific (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/shadowplatformspecific/)
- [iOS プラットフォーム固有設定](~/xamarin-forms/platform/ios/index.md)
- [Android のプラットフォーム固有設定](~/xamarin-forms/platform/android/index.md)
- [Windows プラットフォーム固有](~/xamarin-forms/platform/windows/index.md)
- [Effect を使ったコントロールのカスタマイズ](~/xamarin-forms/app-fundamentals/effects/index.md)
- [添付プロパティ](~/xamarin-forms/xaml/attached-properties.md)
- [PlatformConfiguration API](xref:Xamarin.Forms.PlatformConfiguration)
