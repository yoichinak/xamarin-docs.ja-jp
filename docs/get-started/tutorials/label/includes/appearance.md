---
ms.openlocfilehash: 2b524c657a95035773d18fb17bb86308a78ce37c
ms.sourcegitcommit: 89b3e383a37db5b940f0c63bbfe9cb806dc7d5d1
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/15/2020
ms.locfileid: "81389935"
---
# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

1. **[MainPage.xaml]** で、[`Label`](xref:Xamarin.Forms.Label) 宣言を変更してその外観を変更します。

    ```xaml
    <Label Text="Welcome to Xamarin.Forms!"
           TextColor="Blue"
           FontAttributes="Italic"
           FontSize="22"
           TextDecorations="Underline"
           HorizontalOptions="Center" />
    ```

    このコードでは、[`Label`](xref:Xamarin.Forms.Label) の外観を変更するプロパティを設定します。 [`TextColor`](xref:Xamarin.Forms.Label.TextColor) プロパティでは、`Label` テキストの色を設定します。 [`FontAttributes`](xref:Xamarin.Forms.Label.FontAttributes) プロパティではラベルのフォントをイタリックに設定し、[`FontSize`](xref:Xamarin.Forms.Label.FontSize) プロパティではフォント サイズを設定します。 さらに、下線の文字修飾が、[`TextDecorations`](xref:Xamarin.Forms.Label.TextDecorations) プロパティを設定することで、その `Label` に適用され、[`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) プロパティを [`Center`](xref:Xamarin.Forms.LayoutOptions.Center) に設定することで、水平方向の中央に配置されます。

1. Visual Studio ツール バーで、 **[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択したリモート iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。 [`Label`](xref:Xamarin.Forms.Label) の外観が変更されたことを確認します。

    [![iOS および Android での、外観が変更された Label のスクリーンショット](../images/change-label-appearance.png "外観が変更された Label")](../images/change-label-appearance-large.png#lightbox "外観が変更された Label")

    [`Label`](xref:Xamarin.Forms.Label) の外観の設定について詳しくは、「[Xamarin.Forms Label](~/xamarin-forms/user-interface/text/label.md)」 (Xamarin.Forms のラベル) ガイドを参照してください。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **[MainPage.xaml]** で、[`Label`](xref:Xamarin.Forms.Label) 宣言を変更してその外観を変更します。

    ```xaml
    <Label Text="Welcome to Xamarin.Forms!"
           TextColor="Blue"
           FontAttributes="Italic"
           FontSize="22"
           TextDecorations="Underline"
           HorizontalOptions="Center" />
    ```

    このコードでは、[`Label`](xref:Xamarin.Forms.Label) の外観を変更するプロパティを設定します。 [`TextColor`](xref:Xamarin.Forms.Label.TextColor) プロパティでは、`Label` テキストの色を設定します。 [`FontAttributes`](xref:Xamarin.Forms.Label.FontAttributes) プロパティではラベルのフォントをイタリックに設定し、[`FontSize`](xref:Xamarin.Forms.Label.FontSize) プロパティではフォント サイズを設定します。 さらに、下線の文字修飾が、[`TextDecorations`](xref:Xamarin.Forms.Label.TextDecorations) プロパティを設定することで、その `Label` に適用され、[`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) プロパティを [`Center`](xref:Xamarin.Forms.LayoutOptions.Center) に設定することで、水平方向の中央に配置されます。

1. Visual Studio for Mac ツール バーで、 **[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択した iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。 [`Label`](xref:Xamarin.Forms.Label) の外観が変更されたことを確認します。

    [![iOS および Android での、外観が変更された Label のスクリーンショット](../images/change-label-appearance.png "外観が変更された Label")](../images/change-label-appearance-large.png#lightbox "外観が変更された Label")

    [`Label`](xref:Xamarin.Forms.Label) の外観の設定について詳しくは、「[Xamarin.Forms Label](~/xamarin-forms/user-interface/text/label.md)」 (Xamarin.Forms のラベル) ガイドを参照してください。
