---
title: "プラットフォーム仕様" 説明: "プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、プラットフォーム固有のを使用および作成する方法について説明します。 "
ms. 製品: xamarin ms. assetid: 4729DB9C-8800-4E29-9D66-3BE13C5F8C94: xamarin-forms author: davidbritch ms. author: dabritch ms. date: 10/01/2018 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="platform-specifics"></a>プラットフォーム固有設定

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

_プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。_

XAML または fluent コード API を使用してプラットフォーム固有のを使用するプロセスは、次のとおりです。

1. `xmlns`名前空間の宣言またはディレクティブを追加 `using` [`Xamarin.Forms.PlatformConfiguration`](xref:Xamarin.Forms.PlatformConfiguration) します。
1. `xmlns` `using` プラットフォーム固有の機能を含む名前空間の宣言またはディレクティブを追加します。
    1. IOS では、これは [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 名前空間です。
    1. Android では、これは [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) 名前空間です。 Android AppCompat の場合、これは [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat) 名前空間です。
    1. ユニバーサル Windows プラットフォームでは、これは [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) 名前空間です。
1. XAML から、または fluent API を持つコードから、プラットフォーム固有のを適用し `On<T>` ます。 の値には `T` [`iOS`](xref:Xamarin.Forms.PlatformConfiguration.iOS) 、 [`Android`](xref:Xamarin.Forms.PlatformConfiguration.Android) 名前空間の型、型、または型を指定でき [`Windows`](xref:Xamarin.Forms.PlatformConfiguration.Windows) [`Xamarin.Forms.PlatformConfiguration`](xref:Xamarin.Forms.PlatformConfiguration) ます。

> [!NOTE]
> 使用できないプラットフォームでプラットフォーム固有のを使用しようとしても、エラーは発生しません。 代わりに、プラットフォーム固有の適用を行わずにコードが実行されます。

Fluent コード API を介して使用されるプラットフォームの詳細は、 `On<T>` オブジェクトを返し [`IPlatformElementConfiguration`](xref:Xamarin.Forms.IPlatformElementConfiguration`2) ます。 これにより、メソッドがカスケードされている同じオブジェクトで複数のプラットフォーム固有の情報を呼び出すことができます。

によって提供されるプラットフォームの詳細については Xamarin.Forms 、「 [iOS プラットフォーム-](~/xamarin-forms/platform/ios/index.md)詳細」、「 [Android プラットフォーム](~/xamarin-forms/platform/android/index.md)-詳細」、および「 [Windows プラットフォームの](~/xamarin-forms/platform/windows/index.md)詳細」を参照してください。

## <a name="creating-platform-specifics"></a>プラットフォーム固有の作成

ベンダーは、独自のプラットフォーム固有の情報を効果付きで作成できます。 効果は、特定の機能を提供します。この機能は、プラットフォーム固有のを通じて公開されます。 結果は、XAML や fluent コード API を通じてより簡単に使用できるようになる効果です。

プラットフォーム固有のを作成するプロセスは次のとおりです。

1. 特定の機能を効果として実装します。 詳細については、「[効果の作成](~/xamarin-forms/app-fundamentals/effects/creating.md)」を参照してください。
1. 効果を公開するプラットフォーム固有のクラスを作成します。 詳細については、「[プラットフォーム固有のクラスの作成](#creating-a-platform-specific-class)」を参照してください。
1. プラットフォーム固有のクラスでは、アタッチされたプロパティを実装して、XAML を通じてプラットフォーム固有のを使用できるようにします。 詳細については、「[添付プロパティの追加](#adding-an-attached-property)」を参照してください。
1. プラットフォーム固有のクラスでは、拡張メソッドを実装して、fluent コード API を使用してプラットフォーム固有のを使用できるようにします。 詳細については、「[拡張メソッドの追加](#adding-extension-methods)」を参照してください。
1. 効果が適用されるのは、その効果と同じプラットフォームでプラットフォーム固有のが呼び出されている場合にのみ効果が適用されるように、効果の実装を変更します。 詳細については、「[効果の作成](#creating-the-effect)」を参照してください。

プラットフォーム固有として効果を公開した結果、XAML および fluent コード API を使用して効果をより簡単に使用できるようになります。

> [!NOTE]
> ベンダーは、この手法を使用して独自のプラットフォーム仕様を作成し、ユーザーによる使いやすさを予定ています。 ユーザーは独自のプラットフォーム仕様を作成することもできますが、効果を作成して使用するよりも多くのコードが必要であることに注意してください。

この[サンプルアプリケーション](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shadowplatformspecific)は、 `Shadow` コントロールによって表示されるテキストに影を追加するプラットフォーム固有の例を示してい [`Label`](xref:Xamarin.Forms.Label) ます。

![](images/screenshots.png "Shadow Platform-Specific")

この[サンプルアプリケーション](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shadowplatformspecific)では、理解しやすいように、プラットフォームごとにプラットフォーム固有のを実装して `Shadow` います。 ただし、プラットフォーム固有の各効果の実装とは別に、Shadow クラスの実装はプラットフォームごとにほぼ同じです。 このガイドでは、Shadow クラスの実装と、それに関連する1つのプラットフォームへの影響について焦点します。

効果の詳細については、「[効果を持つコントロールのカスタマイズ](~/xamarin-forms/app-fundamentals/effects/index.md)」を参照してください。

### <a name="creating-a-platform-specific-class"></a>プラットフォーム固有のクラスの作成

プラットフォーム固有のは、クラスとして作成され `public static` ます。

```csharp
namespace MyCompany.Forms.PlatformConfiguration.iOS
{
  public static Shadow
  {
    ...
  }
}
```

次のセクションでは、プラットフォーム固有および関連する効果の実装について説明し `Shadow` ます。

#### <a name="adding-an-attached-property"></a>添付プロパティの追加

XAML で使用できるようにするには、添付プロパティをプラットフォーム固有に追加する必要があり `Shadow` ます。

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

添付プロパティは、クラスがアタッチされて `IsShadowed` `MyCompany.LabelShadowEffect` いるコントロールに対して、その効果を追加したり削除したりするために使用され `Shadow` ます。 この添付プロパティにより、プロパティの値が変更されたときに実行される `OnIsShadowedPropertyChanged` デリゲートが登録されます。 次に、このメソッド `AttachEffect` はメソッドまたはメソッドを呼び出して、 `DetachEffect` 添付プロパティの値に基づいて効果を追加または削除し `IsShadowed` ます。 コントロールのコレクションを変更することによって、コントロールに対して効果が追加または削除され [`Effects`](xref:Xamarin.Forms.Element.Effects) ます。

> [!NOTE]
> 効果は、効果の実装で指定された解決グループ名と一意識別子を連結した値を指定することによって解決されることに注意してください。 詳細については、「[効果の作成](~/xamarin-forms/app-fundamentals/effects/creating.md)」を参照してください。

添付プロパティについて詳しくは、「[添付プロパティ](~/xamarin-forms/xaml/attached-properties.md)」をご覧ください。

#### <a name="adding-extension-methods"></a>拡張メソッドの追加

`Shadow`Fluent コード API を使用して使用できるようにするには、拡張メソッドをプラットフォーム固有に追加する必要があります。

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

`IsShadowed`および `SetIsShadowed` 拡張メソッドは、添付プロパティの get アクセサーと set アクセサーを `IsShadowed` それぞれ呼び出します。 各拡張メソッドは、型に対して動作し `IPlatformElementConfiguration<iOS, FormsElement>` ます。これは、iOS のインスタンスでプラットフォーム固有のを呼び出すことができることを指定し [`Label`](xref:Xamarin.Forms.Label) ます。

#### <a name="creating-the-effect"></a>効果を作成する

プラットフォーム固有のは、を `Shadow` に追加し、 `MyCompany.LabelShadowEffect` [`Label`](xref:Xamarin.Forms.Label) 削除します。 iOS プロジェクト用の `LabelShadowEffect` の実装を次のコード例に示します。

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

`UpdateShadow`メソッドは、 `Control.Layer` 添付プロパティがに設定されている場合に、シャドウを作成するためのプロパティを設定し `IsShadowed` `true` ます。また、プラットフォーム固有のが、その `Shadow` 効果が実装されているのと同じプラットフォーム上で呼び出されていることを示します。 このチェックは、メソッドを使用して実行され `OnThisPlatform` ます。

`Shadow.IsShadowed`アタッチされたプロパティ値が実行時に変更された場合、その効果はシャドウを削除することによって応答する必要があります。 したがって、メソッドを `OnElementPropertyChanged` 呼び出して、バインド可能なプロパティの変更に応答するには、オーバーライドされたメソッドのバージョンを使用し `UpdateShadow` ます。

効果を作成する方法の詳細については、「[効果の作成](~/xamarin-forms/app-fundamentals/effects/creating.md)」と「効果の[パラメーターを添付プロパティとして渡す](~/xamarin-forms/app-fundamentals/effects/passing-parameters/attached-properties.md)」を参照してください。

### <a name="consuming-the-platform-specific"></a>プラットフォーム固有の使用

プラットフォーム固有のは、 `Shadow` 添付プロパティを値に設定することによって XAML で使用され `Shadow.IsShadowed` `boolean` ます。

```xaml
<ContentPage xmlns:ios="clr-namespace:MyCompany.Forms.PlatformConfiguration.iOS" ...>
  ...
  <Label Text="Label Shadow Effect" ios:Shadow.IsShadowed="true" ... />
  ...
</ContentPage>
```

または、fluent API を使用して C# から使用することもできます。

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
