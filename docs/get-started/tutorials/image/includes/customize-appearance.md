---
ms.openlocfilehash: 42d1f79b22e25e46f81f7c5a77389fd578a4d894
ms.sourcegitcommit: a5a5c5de7d04f046a64e4875e180fc93227bf495
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/21/2021
ms.locfileid: "98689860"
---
# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

1. **[MainPage.xaml]** で、[`Image`](xref:Xamarin.Forms.Image) 宣言を変更してその外観をカスタマイズします。

    ```xaml
    <Image Source="https://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg"
           Aspect="Fill"
           HeightRequest="{OnPlatform iOS=300, Android=250}"
           WidthRequest="{OnPlatform iOS=300, Android=250}"
           HorizontalOptions="Center" />
    ```

    このコードでは、イメージのスケーリング モードを定義する、[`Aspect`](xref:Xamarin.Forms.Image.Aspect) プロパティを [`Fill`](xref:Xamarin.Forms.Aspect.Fill) に設定します。 `Fill` メンバーは、[`Aspect`](xref:Xamarin.Forms.Aspect) 列挙型で定義され、イメージがゆがんでいるかどうかに関係なく、ビューを完全に埋めるためにそのイメージを拡大します。 イメージのスケーリングの詳細については、「[Images in Xamarin.Forms](~/xamarin-forms/user-interface/images.md)」 (Xamarin.Forms のイメージ) ガイドの「[Displaying images](~/xamarin-forms/user-interface/images.md#display-images)」 (イメージの表示) を参照してください。

    `OnPlatform` マークアップ拡張では、プラットフォームごとに UI の外観をカスタマイズできます。 この例では、マークアップ拡張を使用して、[`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) および [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) プロパティを、iOS では 300 (デバイスに依存しない単位) に、Android では 250 (デバイスに依存しない単位) に設定します。 `OnPlatform` マークアップ拡張の詳細については、「[Consuming XAML Markup Extensions](~/xamarin-forms/xaml/markup-extensions/consuming.md)」 (XAM マークアップ拡張の使用) ガイドの「[OnPlatform markup extension](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform-markup-extension)」 (OnPlatform マークアップ拡張) を参照してください。

    また、[`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) プロパティでは、イメージが水平方向の中央に配置されるように指定します。

1. アプリケーションがまだ実行されている場合は、変更内容をファイルに保存すると、アプリケーションのユーザー インターフェイスがシミュレーターまたはエミュレーターで自動的に更新されます。 もしくは、Visual Studio ツール バーで、 **[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択したリモート iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。

    [![iOS 上と Android 上とでサイズが異なる画像のスクリーンショット](../images/customize-appearance.png "プラットフォームによってサイズが異なる画像")](../images/customize-appearance-large.png#lightbox "プラットフォームによってサイズが異なる画像")

    Visual Studio で、アプリケーションを停止します。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **[MainPage.xaml]** で、[`Image`](xref:Xamarin.Forms.Image) 宣言を変更してその外観をカスタマイズします。

    ```xaml
    <Image Source="https://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg"
           Aspect="Fill"
           HeightRequest="{OnPlatform iOS=300, Android=250}"
           WidthRequest="{OnPlatform iOS=300, Android=250}"
           HorizontalOptions="Center" />
    ```

    このコードでは、イメージのスケーリング モードを定義する、[`Aspect`](xref:Xamarin.Forms.Image.Aspect) プロパティを [`Fill`](xref:Xamarin.Forms.Aspect.Fill) に設定します。 `Fill` メンバーは、[`Aspect`](xref:Xamarin.Forms.Aspect) 列挙型で定義され、イメージがゆがんでいるかどうかに関係なく、ビューを完全に埋めるためにそのイメージを拡大します。 イメージのスケーリングの詳細については、「[Images in Xamarin.Forms](~/xamarin-forms/user-interface/images.md)」 (Xamarin.Forms のイメージ) ガイドの「[Displaying images](~/xamarin-forms/user-interface/images.md#display-images)」 (イメージの表示) を参照してください。

    `OnPlatform` マークアップ拡張では、プラットフォームごとに UI の外観をカスタマイズできます。 この例では、マークアップ拡張を使用して、[`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) および [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) プロパティを、iOS では 300 に、Android では 250 に設定します。 `OnPlatform` マークアップ拡張の詳細については、「[Consuming XAML Markup Extensions](~/xamarin-forms/xaml/markup-extensions/consuming.md)」 (XAM マークアップ拡張の使用) ガイドの「[OnPlatform markup extension](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform-markup-extension)」 (OnPlatform マークアップ拡張) を参照してください。

    また、[`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) プロパティでは、イメージが水平方向の中央に配置されるように指定します。

1. アプリケーションがまだ実行されている場合は、変更内容をファイルに保存すると、アプリケーションのユーザー インターフェイスがシミュレーターまたはエミュレーターで自動的に更新されます。 もしくは、Visual Studio for Mac ツール バーで、 **[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択した iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。

    [![iOS 上と Android 上とでサイズが異なる画像のスクリーンショット](../images/customize-appearance.png "プラットフォームによってサイズが異なる画像")](../images/customize-appearance-large.png#lightbox "プラットフォームによってサイズが異なる画像")

    Visual Studio for Mac で、アプリケーションを停止します。
