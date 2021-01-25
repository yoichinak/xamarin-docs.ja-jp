---
ms.openlocfilehash: 4352ff38e785a0a222cf8d2d64511ed14b3ae192
ms.sourcegitcommit: a5a5c5de7d04f046a64e4875e180fc93227bf495
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/21/2021
ms.locfileid: "98690005"
---
# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

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

1. アプリケーションがまだ実行されている場合は、変更内容をファイルに保存すると、アプリケーションのユーザー インターフェイスがシミュレーターまたはエミュレーターで自動的に更新されます。 もしくは、Visual Studio ツール バーで、 **[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択したリモート iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。 [`Label`](xref:Xamarin.Forms.Label) の外観が変更されたことを確認します。

    [![iOS と Android で書式設定されたテキストを表示している Label のスクリーンショット](../images/label-formatted-text.png "書式設定されたテキストの Label")](../images/label-formatted-text-large.png#lightbox "書式設定されたテキストの Label")

    Visual Studio で、アプリケーションを停止します。

    [`Span`](xref:Xamarin.Forms.Span) の設定の詳細については、「[Xamarin.Forms Label](~/xamarin-forms/user-interface/text/label.md)」ガイドの「[Formatted text](~/xamarin-forms/user-interface/text/label.md#formatted-text)」 (書式設定されたテキスト) を参照してください。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/vsmac)

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

1. アプリケーションがまだ実行されている場合は、変更内容をファイルに保存すると、アプリケーションのユーザー インターフェイスがシミュレーターまたはエミュレーターで自動的に更新されます。 もしくは、Visual Studio for Mac ツール バーで、 **[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択した iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。 [`Label`](xref:Xamarin.Forms.Label) の外観が変更されたことを確認します。

    [![iOS と Android で書式設定されたテキストを表示している Label のスクリーンショット](../images/label-formatted-text.png "書式設定されたテキストの Label")](../images/label-formatted-text-large.png#lightbox "書式設定されたテキストの Label")

    Visual Studio for Mac で、アプリケーションを停止します。

    [`Span`](xref:Xamarin.Forms.Span) の設定の詳細については、「[Xamarin.Forms Label](~/xamarin-forms/user-interface/text/label.md)」ガイドの「[Formatted text](~/xamarin-forms/user-interface/text/label.md#formatted-text)」 (書式設定されたテキスト) を参照してください。
