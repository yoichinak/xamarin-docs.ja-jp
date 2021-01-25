---
ms.openlocfilehash: f4fccbfe7e00a353682b0dfe787c291b38e5cd76
ms.sourcegitcommit: a5a5c5de7d04f046a64e4875e180fc93227bf495
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/21/2021
ms.locfileid: "98690006"
---
# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

1. **[MainPage.xaml]** で、[`Entry`](xref:Xamarin.Forms.Entry) 宣言を変更して、その動作をカスタマイズします。

    ```xaml
    <Entry Placeholder="Enter password"
           MaxLength="15"
           IsSpellCheckEnabled="false"
           IsTextPredictionEnabled="false"
           IsPassword="true" />
    ```

    このコードでは、[`Entry`](xref:Xamarin.Forms.Entry) の動作をカスタマイズするプロパティを設定します。 [`MaxLength`](xref:Xamarin.Forms.InputView.MaxLength) プロパティでは、`Entry` で許可されている入力の長さが制限され、[`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) プロパティは、スペルチェックを無効にするために `false` に設定されます。 同様に、[`IsTextPredictionEnabled`](xref:Xamarin.Forms.Entry.IsTextPredictionEnabled) プロパティは、予測入力と自動予測入力を無効にするために `false` に設定されます。 さらに、[`IsPassword`](xref:Xamarin.Forms.Entry.IsPassword) プロパティでは、入力された文字が確実にパスワード文字 (黒丸) でマスクされるようにします。

    > [!NOTE]
    > パスワードの入力などの一部のテキスト入力シナリオの場合、スペルチェックと予測入力でネガティブなエクスペリエンスが提供されるため、これらは無効にする必要があります。

1. アプリケーションがまだ実行されている場合は、変更内容をファイルに保存すると、アプリケーションのユーザー インターフェイスがシミュレーターまたはエミュレーターで自動的に更新されます。 もしくは、Visual Studio ツール バーで、 **[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択したリモート iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。 テキストを [`Entry`](xref:Xamarin.Forms.Entry) に入力し、各文字がパスワード マスク文字で置き換えられており、入力できる文字の最大数が 15 であることを確認します。

    [![iOS および Android での、パスワード文字によってテキストがマスクされた Entry のスクリーンショット](../images/customize-behavior.png "マスクされたパスワード文字を使用した Entry")](../images/customize-behavior-large.png#lightbox "マスクされたパスワード文字を使用した Entry")

    Visual Studio で、アプリケーションを停止します。

    [`Entry`](xref:Xamarin.Forms.Entry) 動作のカスタマイズの詳細については、「[Xamarin.Forms Entry](~/xamarin-forms/user-interface/text/entry.md)」 (Xamarin.Forms のエントリ) ガイドを参照してください。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **[MainPage.xaml]** で、[`Entry`](xref:Xamarin.Forms.Entry) 宣言を変更して、その動作をカスタマイズします。

    ```xaml
    <Entry Placeholder="Enter password"
           MaxLength="15"
           IsSpellCheckEnabled="false"
           IsTextPredictionEnabled="false"
           IsPassword="true" />
    ```

    このコードでは、[`Entry`](xref:Xamarin.Forms.Entry) の動作をカスタマイズするプロパティを設定します。 [`MaxLength`](xref:Xamarin.Forms.InputView.MaxLength) プロパティでは、`Entry` で許可されている入力の長さが制限され、[`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) プロパティは、スペルチェックを無効にするために `false` に設定されます。 同様に、[`IsTextPredictionEnabled`](xref:Xamarin.Forms.Entry.IsTextPredictionEnabled) プロパティは、予測入力と自動予測入力を無効にするために `false` に設定されます。 さらに、[`IsPassword`](xref:Xamarin.Forms.Entry.IsPassword) プロパティでは、入力された文字が確実にパスワード文字 (黒丸) でマスクされるようにします。

    > [!NOTE]
    > パスワードの入力などの一部のテキスト入力シナリオの場合、スペルチェックと予測入力でネガティブなエクスペリエンスが提供されるため、これらは無効にする必要があります。

1. アプリケーションがまだ実行されている場合は、変更内容をファイルに保存すると、アプリケーションのユーザー インターフェイスがシミュレーターまたはエミュレーターで自動的に更新されます。 もしくは、Visual Studio for Mac ツール バーで、 **[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択した iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。 テキストを [`Entry`](xref:Xamarin.Forms.Entry) に入力し、各文字がパスワード マスク文字で置き換えられており、入力できる文字の最大数が 15 であることを確認します。

    [![iOS および Android での、パスワード文字によってテキストがマスクされた Entry のスクリーンショット](../images/customize-behavior.png "マスクされたパスワード文字を使用した Entry")](../images/customize-behavior-large.png#lightbox "マスクされたパスワード文字を使用した Entry")

    Visual Studio for Mac で、アプリケーションを停止します。

    [`Entry`](xref:Xamarin.Forms.Entry) 動作のカスタマイズの詳細については、「[Xamarin.Forms Entry](~/xamarin-forms/user-interface/text/entry.md)」 (Xamarin.Forms のエントリ) ガイドを参照してください。
