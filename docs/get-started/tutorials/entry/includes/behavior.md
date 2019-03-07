# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

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

1. Visual Studio ツール バーで、**[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択したリモート iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。 テキストを [`Entry`](xref:Xamarin.Forms.Entry) に入力し、各文字がパスワード マスク文字で置き換えられており、入力できる文字の最大数が 15 であることを確認します。

    [![iOS および Android での、パスワード文字によってテキストがマスクされた Entry のスクリーンショット](../images/customize-behavior.png "パスワード文字がマスクされた Entry")](../images/customize-behavior-large.png#lightbox "パスワード文字がマスクされた Entry")

    [`Entry`](xref:Xamarin.Forms.Entry) 動作のカスタマイズの詳細については、「[Xamarin.Forms Entry](~/xamarin-forms/user-interface/text/entry.md)」 (Xamarin.Forms のエントリ) ガイドを参照してください。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

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

1. Visual Studio for Mac ツール バーで、**[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択した iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。 テキストを [`Entry`](xref:Xamarin.Forms.Entry) に入力し、各文字がパスワード マスク文字で置き換えられており、入力できる文字の最大数が 15 であることを確認します。

    [![iOS および Android での、パスワード文字によってテキストがマスクされた Entry のスクリーンショット](../images/customize-behavior.png "パスワード文字がマスクされた Entry")](../images/customize-behavior-large.png#lightbox "パスワード文字がマスクされた Entry")

    [`Entry`](xref:Xamarin.Forms.Entry) 動作のカスタマイズの詳細については、「[Xamarin.Forms Entry](~/xamarin-forms/user-interface/text/entry.md)」 (Xamarin.Forms のエントリ) ガイドを参照してください。
