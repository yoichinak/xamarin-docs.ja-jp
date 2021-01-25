---
ms.openlocfilehash: 703d99446d3a43411b681677bf9d3c9ab01ea83f
ms.sourcegitcommit: a5a5c5de7d04f046a64e4875e180fc93227bf495
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/21/2021
ms.locfileid: "98690073"
---
# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

1. **[MainPage.xaml]** で、[`Button`](xref:Xamarin.Forms.Button) 宣言を変更してその外観を変更します。

    ```xaml
    <Button Text="Click me"
            Clicked="OnButtonClicked"
            TextColor="Blue"
            BackgroundColor="Aqua"
            BorderColor="Red"
            BorderWidth="5"
            CornerRadius="5"
            WidthRequest="150"
            HeightRequest="75" />
    ```

    このコードでは、[`Button`](xref:Xamarin.Forms.Button) の外観を変更するプロパティを設定します。 [`TextColor`](xref:Xamarin.Forms.Button.TextColor) プロパティでは `Button` テキストの色を設定し、[`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) プロパティではテキストの背景色を設定します。 [`BorderColor`](xref:Xamarin.Forms.Button.BorderColor) プロパティでは、`Button` の周りの領域の色を設定し、[`BorderWidth`](xref:Xamarin.Forms.Button.BorderWidth) プロパティでは罫線の幅を設定します。 既定では、`Button` は四角形ですが、[`CornerRadius`](xref:Xamarin.Forms.Button.CornerRadius) プロパティを適切な値に設定することで、角を丸くすることができます。 また、`Button` のサイズは、その [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) および [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) プロパティを設定して変更します。

1. アプリケーションがまだ実行されている場合は、変更内容をファイルに保存すると、アプリケーションのユーザー インターフェイスがシミュレーターまたはエミュレーターで自動的に更新されます。 もしくは、Visual Studio ツール バーで、 **[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択したリモート iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。 [`Button`](xref:Xamarin.Forms.Button) の外観が変更されたことを確認します。

    [![iOS および Android での、外観が変更された Button のスクリーンショット](../images/change-button-appearance.png "外観が変更された Button")](../images/change-button-appearance-large.png#lightbox "外観が変更された Button")

    Visual Studio で、アプリケーションを停止します。

    [`Button`](xref:Xamarin.Forms.Button) の外観の設定について詳しくは、「[Xamarin.Forms Button](~/xamarin-forms/user-interface/button.md)」 (Xamarin.Forms のボタン) ガイドの「[Button appearance](~/xamarin-forms/user-interface/button.md#button-appearance)」 (ボタンの外観) を参照してください。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **[MainPage.xaml]** で、[`Button`](xref:Xamarin.Forms.Button) 宣言を変更してその外観を変更します。

    ```xaml
    <Button Text="Click me"
            Clicked="OnButtonClicked"
            TextColor="Blue"
            BackgroundColor="Aqua"
            BorderColor="Red"
            BorderWidth="5"
            CornerRadius="5"
            WidthRequest="150"
            HeightRequest="75" />
    ```

    このコードでは、[`Button`](xref:Xamarin.Forms.Button) の外観を変更するプロパティを設定します。 [`TextColor`](xref:Xamarin.Forms.Button.TextColor) プロパティでは `Button` テキストの色を設定し、[`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) プロパティではテキストの背景色を設定します。 [`BorderColor`](xref:Xamarin.Forms.Button.BorderColor) プロパティでは、`Button` の周りの領域の色を設定し、[`BorderWidth`](xref:Xamarin.Forms.Button.BorderWidth) プロパティでは罫線の幅を設定します。 既定では、`Button` は四角形ですが、[`CornerRadius`](xref:Xamarin.Forms.Button.CornerRadius) プロパティを適切な値に設定することで、角を丸くすることができます。 また、`Button` のサイズは、その [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) および [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) プロパティを設定して変更します。

1. アプリケーションがまだ実行されている場合は、変更内容をファイルに保存すると、アプリケーションのユーザー インターフェイスがシミュレーターまたはエミュレーターで自動的に更新されます。 もしくは、Visual Studio for Mac ツール バーで、 **[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択した iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。 [`Button`](xref:Xamarin.Forms.Button) の外観が変更されたことを確認します。

    [![iOS および Android での、外観が変更された Button のスクリーンショット](../images/change-button-appearance.png "外観が変更された Button")](../images/change-button-appearance-large.png#lightbox "外観が変更された Button")

    Visual Studio for Mac で、アプリケーションを停止します。

    [`Button`](xref:Xamarin.Forms.Button) の外観の設定について詳しくは、「[Xamarin.Forms Button](~/xamarin-forms/user-interface/button.md)」 (Xamarin.Forms のボタン) ガイドの「[Button appearance](~/xamarin-forms/user-interface/button.md#button-appearance)」 (ボタンの外観) を参照してください。
