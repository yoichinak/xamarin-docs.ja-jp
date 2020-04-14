---
ms.openlocfilehash: bd3f37082443e93f10f60d9466fe22aae8571b14
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/09/2020
ms.locfileid: "61373439"
---
# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

1. **[MainPage.xaml]** で、[`Editor`](xref:Xamarin.Forms.Editor) 宣言を変更して、その動作をカスタマイズします。

    ```xaml
    <Editor Placeholder="Enter multi-line text here"
            AutoSize="TextChanges"
            MaxLength="200"
            IsSpellCheckEnabled="false"
            IsTextPredictionEnabled="false" />
    ```

    このコードでは、[`Editor`](xref:Xamarin.Forms.Editor) の動作をカスタマイズするプロパティを設定します。 [`AutoSize`](xref:Xamarin.Forms.Editor.AutoSize) プロパティは [`TextChanges`](xref:Xamarin.Forms.EditorAutoSizeOption.TextChanges) に設定されます。これは、`Editor` の高さが、テキストが取り込まれたときに増え、テキストが削除されたときに減ることを示します。 [`MaxLength`](xref:Xamarin.Forms.InputView.MaxLength) プロパティでは、`Editor` に許可されている入力の長さを制限します。 さらに、[`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) プロパティはスペルチェックを無効にするために `false` に設定され、一方、`IsTextPredictionEnabled` プロパティは予測入力と自動予測入力を無効にするために `false` に設定されます。

1. Visual Studio ツール バーで、 **[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択したリモート iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。 テキストを [`Editor`](xref:Xamarin.Forms.Entry) に入力し、`Editor` の長さがテキストが取り込まれたときに増え、テキストが削除されたときに減り、また、入力できる文字の最大数が 200 であることを確認します。

    [![iOS および Android での、自動サイズ調整 Editor のスクリーンショット](../images/customize-behavior.png "自動サイズ調整 Editor")](../images/customize-behavior-large.png#lightbox "自動サイズ調整 Editor")

    [`Editor`](xref:Xamarin.Forms.Editor) 動作のカスタマイズの詳細については、「[Xamarin.Forms Editor](~/xamarin-forms/user-interface/text/editor.md)」 (Xamarin.Forms のエディター) ガイドを参照してください。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **[MainPage.xaml]** で、[`Editor`](xref:Xamarin.Forms.Editor) 宣言を変更して、その動作をカスタマイズします。

    ```xaml
    <Editor Placeholder="Enter multi-line text here"
            AutoSize="TextChanges"
            MaxLength="200"
            IsSpellCheckEnabled="false"
            IsTextPredictionEnabled="false" />
    ```

    このコードでは、[`Editor`](xref:Xamarin.Forms.Editor) の動作をカスタマイズするプロパティを設定します。 [`AutoSize`](xref:Xamarin.Forms.Editor.AutoSize) プロパティは [`TextChanges`](xref:Xamarin.Forms.EditorAutoSizeOption.TextChanges) に設定されます。これは、`Editor` の高さが、テキストが取り込まれたときに増え、テキストが削除されたときに減ることを示します。 [`MaxLength`](xref:Xamarin.Forms.InputView.MaxLength) プロパティでは、`Editor` に許可されている入力の長さを制限します。 さらに、[`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) プロパティはスペルチェックを無効にするために `false` に設定され、一方、`IsTextPredictionEnabled` プロパティは予測入力と自動予測入力を無効にするために `false` に設定されます。

1. Visual Studio for Mac ツール バーで、 **[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択した iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。 テキストを [`Editor`](xref:Xamarin.Forms.Entry) に入力し、`Editor` の長さがテキストが取り込まれたときに増え、テキストが削除されたときに減り、また、入力できる文字の最大数が 200 であることを確認します。

    [![iOS および Android での、自動サイズ調整 Editor のスクリーンショット](../images/customize-behavior.png "自動サイズ調整 Editor")](../images/customize-behavior-large.png#lightbox "自動サイズ調整 Editor")

    [`Editor`](xref:Xamarin.Forms.Editor) 動作のカスタマイズの詳細については、「[Xamarin.Forms Editor](~/xamarin-forms/user-interface/text/editor.md)」 (Xamarin.Forms のエディター) ガイドを参照してください。
