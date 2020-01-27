---
ms.openlocfilehash: 841dac9486097e27923ccfe582803b4ec50371cf
ms.sourcegitcommit: 3f0e4f10e5def19122588bb05f26ab2baa9df6eb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/23/2020
ms.locfileid: "60896725"
---
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. **MainPage.xaml** で、[`Label`](xref:Xamarin.Forms.Label) 宣言を変更して、複数の形式を使用するテキストを 1 つの `Label` で示します。

    ```xaml
    <Label TextColor="Gray"
           FontSize="Medium">
        <Label.FormattedText>
            <FormattedString>
                <Span Text="This sentence contains " />
                <Span Text="words that are emphasized, "
                      FontAttributes="Italic" />
                <Span Text="and underlined."
                      TextDecorations="Underline" />
            </FormattedString>
        </Label.FormattedText>
    </Label>
    ```

    このコードは、複数の形式を使用するテキストを 1 つの [`Label`](xref:Xamarin.Forms.Label) で表示します。 最初の [`Span`](xref:Xamarin.Forms.Span) 内のテキストは、`Label` で設定された形式を使用して表示されますが、2 番目と 3 番目の `Span` インスタンス内のテキストは、`Label` で設定された形式と各 `Span` で設定された追加の形式を使用して表示されます。

    > [!NOTE]
    > [`FormattedText`](xref:Xamarin.Forms.Label.FormattedText) プロパティは、1 つ以上の [`Span`](xref:Xamarin.Forms.Span)インスタンスで構成される [`FormattedString`](xref:Xamarin.Forms.FormattedString) の型です。

1. Visual Studio ツール バーで、 **[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択したリモート iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。 [`Label`](xref:Xamarin.Forms.Label) の外観が変更されたことを確認します。

    [![iOS と Android で書式設定されたテキストを表示している Label のスクリーンショット](../images/label-formatted-text.png "書式設定されたテキストの Label")](../images/label-formatted-text-large.png#lightbox "書式設定されたテキストの Label")

    [`Span`](xref:Xamarin.Forms.Span) の設定の詳細については、「[Xamarin.Forms Label](~/xamarin-forms/user-interface/text/label.md)」ガイドの「[Formatted text](~/xamarin-forms/user-interface/text/label.md#formatted-text)」 (書式設定されたテキスト) を参照してください。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **MainPage.xaml** で、[`Label`](xref:Xamarin.Forms.Label) 宣言を変更して、複数の形式を使用するテキストを 1 つの `Label` で示します。

    ```xaml
    <Label TextColor="Gray"
           FontSize="Medium">
        <Label.FormattedText>
            <FormattedString>
                <Span Text="This sentence contains " />
                <Span Text="words that are emphasized, "
                      FontAttributes="Italic" />
                <Span Text="and underlined."
                      TextDecorations="Underline" />
            </FormattedString>
        </Label.FormattedText>
    </Label>
    ```

    このコードは、複数の形式を使用するテキストを 1 つの [`Label`](xref:Xamarin.Forms.Label) で表示します。 最初の [`Span`](xref:Xamarin.Forms.Span) 内のテキストは、`Label` で設定された形式を使用して表示されますが、2 番目と 3 番目の `Span` インスタンス内のテキストは、`Label` で設定された形式と各 `Span` で設定された追加の形式を使用して表示されます。

    > [!NOTE]
    > [`FormattedText`](xref:Xamarin.Forms.Label.FormattedText) プロパティは、1 つ以上の [`Span`](xref:Xamarin.Forms.Span)インスタンスで構成される [`FormattedString`](xref:Xamarin.Forms.FormattedString) の型です。

1. Visual Studio for Mac ツール バーで、 **[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択した iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。 [`Label`](xref:Xamarin.Forms.Label) の外観が変更されたことを確認します。

    [![iOS と Android で書式設定されたテキストを表示している Label のスクリーンショット](../images/label-formatted-text.png "書式設定されたテキストの Label")](../images/label-formatted-text-large.png#lightbox "書式設定されたテキストの Label")

    [`Span`](xref:Xamarin.Forms.Span) の設定の詳細については、「[Xamarin.Forms Label](~/xamarin-forms/user-interface/text/label.md)」ガイドの「[Formatted text](~/xamarin-forms/user-interface/text/label.md#formatted-text)」 (書式設定されたテキスト) を参照してください。
