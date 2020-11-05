---
title: Xamarin.Forms キー
description: この記事では、Entry クラスを使用して、 Xamarin.Forms アプリケーションで単一行のテキストまたはパスワード入力を受け入れる方法について説明します。
ms.prod: xamarin
ms.assetid: 9923C541-3C10-4D14-BAB5-C4D6C514FB1E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/21/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 625dd57d1f84b95cef1c6513ae832af805bf565a
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93375032"
---
# <a name="no-locxamarinforms-entry"></a>Xamarin.Forms キー

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/userinterface-text)

は、 Xamarin.Forms [`Entry`](xref:Xamarin.Forms.Entry) 単一行のテキスト入力に使用されます。 は、 `Entry` ビューのように、 [`Editor`](xref:Xamarin.Forms.Editor) 複数のキーボードの種類をサポートしています。 また、は `Entry` パスワードフィールドとして使用できます。

## <a name="set-and-read-text"></a>テキストの設定と読み取り

は、 `Entry` 他のテキスト表示ビューと同様に、プロパティを公開し [`Text`](xref:Xamarin.Forms.InputView.Text) ます。 このプロパティを使用して、によって表示されるテキストを設定および読み取ることができ `Entry` ます。 次の例は、XAML でプロパティを設定する方法を示してい `Text` ます。

```xaml
<Entry Text="I am an Entry" />
```

C# の場合:

```csharp
var MyEntry = new Entry { Text = "I am an Entry" };
```

テキストを読み取るには、 `Text` C# のプロパティにアクセスします。

```csharp
var text = MyEntry.Text;
```

## <a name="set-placeholder-text"></a>プレースホルダーテキストの設定

は、 [`Entry`](xref:Xamarin.Forms.Entry) ユーザー入力を格納していない場合にプレースホルダーテキストを表示するように設定できます。 これは、プロパティをに設定することによって実現され、多くの場合 [`Placeholder`](xref:Xamarin.Forms.InputView.Placeholder) `string` 、に適したコンテンツの種類を示すために使用され `Entry` ます。 また、プロパティをに設定することによって、プレースホルダーテキストの色を制御できます [`PlaceholderColor`](xref:Xamarin.Forms.InputView.PlaceholderColor) [`Color`](xref:Xamarin.Forms.Color) 。

```xaml
<Entry Placeholder="Username" PlaceholderColor="Olive" />
```

```csharp
var entry = new Entry { Placeholder = "Username", PlaceholderColor = Color.Olive };
```

> [!NOTE]
> の幅は、 `Entry` そのプロパティを設定することによって定義でき `WidthRequest` ます。 定義されているの幅は、 `Entry` プロパティの値によって異なり `Text` ます。

## <a name="prevent-text-entry"></a>テキスト入力を禁止する

[`Entry`](xref:Xamarin.Forms.Entry) `IsReadOnly` の既定値であるプロパティを次のように設定することによって、のテキストを変更できないようにすることができます `false` `true` 。

```xaml
<Entry Text="This is a read-only Entry"
       IsReadOnly="true" />
```

```csharp
var entry = new Entry { Text = "This is a read-only Entry", IsReadOnly = true });
```

> [!NOTE]
> プロパティは、の `IsReadonly` [`Entry`](xref:Xamarin.Forms.Entry) 視覚的な外観を `IsEnabled` 灰色に変更するプロパティとは異なり、の外観を変更しません `Entry` 。

## <a name="transform-text"></a>テキストの変換

は、プロパティを [`Entry`](xref:Xamarin.Forms.Entry) `Text` `TextTransform` 列挙体の値に設定することによって、プロパティに格納されているテキストの大文字と小文字を変換できます `TextTransform` 。 この列挙体には、次の4つの値があります。

- `None` テキストが変換されないことを示します。
- `Default` プラットフォームの既定の動作が使用されることを示します。 これは、`TextTransform` プロパティの既定値です。
- `Lowercase` テキストが小文字に変換されることを示します。
- `Uppercase` テキストが大文字に変換されることを示します。

次の例では、テキストを大文字に変換しています。

```xaml
<Entry Text="This text will be displayed in uppercase."
       TextTransform="Uppercase" />
```

これに相当する C# コードを次に示します。

```csharp
Entry entry = new Entry
{
    Text = "This text will be displayed in uppercase.",
    TextTransform = TextTransform.Uppercase
};
```

## <a name="limit-input-length"></a>入力の長さを制限する

プロパティは、で許可されて [`MaxLength`](xref:Xamarin.Forms.InputView.MaxLength) いる入力の長さを制限するために使用でき [`Entry`](xref:Xamarin.Forms.Entry) ます。 このプロパティは、正の整数に設定する必要があります。

```xaml
<Entry ... MaxLength="10" />
```

```csharp
var entry = new Entry { ... MaxLength = 10 };
```

[`MaxLength`](xref:Xamarin.Forms.InputView.MaxLength)プロパティ値が0の場合、入力が許可されないことを示します。の値 (の既定値) は、 `int.MaxValue` [`Entry`](xref:Xamarin.Forms.Entry) 入力可能な文字数に有効な制限がないことを示します。

## <a name="character-spacing"></a>文字間隔

[`Entry`](xref:Xamarin.Forms.Entry)プロパティを値に設定することによって、文字間隔をに適用でき `Entry.CharacterSpacing` `double` ます。

```xaml
<Entry ...
       CharacterSpacing="10" />
```

これに相当する C# コードを次に示します。

```csharp
Entry entry = new Entry { CharacterSpacing = 10 };
```

結果として、によって表示されるテキスト内の文字 [`Entry`](xref:Xamarin.Forms.Entry) は、 `CharacterSpacing` デバイスに依存しない単位になります。

> [!NOTE]
> `CharacterSpacing`プロパティ値は、プロパティおよびプロパティによって表示されるテキストに適用され `Text` `Placeholder` ます。

## <a name="password-fields"></a>パスワードフィールド

`Entry` プロパティを提供 `IsPassword` します。 がの場合 `IsPassword` `true` 、フィールドの内容は黒の円として表示されます。

XAML の場合:

```xaml
<Entry IsPassword="true" />
```

C# の場合:

```csharp
var MyEntry = new Entry { IsPassword = true };
```

![Entry IsPassword の例](entry-images/password.png)

プレースホルダーは、 `Entry` パスワードフィールドとして構成されているのインスタンスで使用できます。

XAML の場合:

```xaml
<Entry IsPassword="true" Placeholder="Password" />
```

C# の場合:

```csharp
var MyEntry = new Entry { IsPassword = true, Placeholder = "Password" };
```

![Entry IsPassword と Placeholder の例](entry-images/passwordplaceholder.png)

## <a name="set-the-cursor-position-and-text-selection-length"></a>カーソルの位置とテキスト選択の長さを設定する

プロパティは、 [`CursorPosition`](xref:Xamarin.Forms.Entry.CursorPosition) プロパティに格納されている文字列に次の文字が挿入される位置を取得または設定するために使用でき [`Text`](xref:Xamarin.Forms.InputView.Text) ます。

```xaml
<Entry Text="Cursor position set" CursorPosition="5" />
```

```csharp
var entry = new Entry { Text = "Cursor position set", CursorPosition = 5 };
```

プロパティの既定値 [`CursorPosition`](xref:Xamarin.Forms.Entry.CursorPosition) は0です。これは、テキストがの先頭に挿入されることを示し `Entry` ます。

また、プロパティを [`SelectionLength`](xref:Xamarin.Forms.Entry.SelectionLength) 使用して、内のテキスト選択の長さを取得または設定することもでき `Entry` ます。

```xaml
<Entry Text="Cursor position and selection length set" CursorPosition="2" SelectionLength="10" />
```

```csharp
var entry = new Entry { Text = "Cursor position and selection length set", CursorPosition = 2, SelectionLength = 10 };
```

プロパティの既定値 [`SelectionLength`](xref:Xamarin.Forms.Entry.SelectionLength) は0です。これは、テキストが選択されていないことを示します。

## <a name="display-a-clear-button"></a>クリアボタンを表示する

プロパティを使用して、 `ClearButtonVisibility` がクリアボタンを表示するかどうかを制御し、 [`Entry`](xref:Xamarin.Forms.Entry) ユーザーがテキストをクリアできるようにすることができます。 このプロパティは、列挙体のメンバーに設定する必要があり `ClearButtonVisibility` ます。

- `Never` クリアボタンが表示されないことを示します。 これは、`Entry.ClearButtonVisibility` プロパティの既定値です。
- `WhileEditing` にフォーカスとテキストがあるときに、にクリアボタンが表示されることを示し [`Entry`](xref:Xamarin.Forms.Entry) ます。

次の例は、XAML でプロパティを設定する方法を示しています。

```xaml
<Entry Text="Xamarin.Forms"
       ClearButtonVisibility="WhileEditing" />
```

これに相当する C# コードを次に示します。

```csharp
var entry = new Entry { Text = "Xamarin.Forms", ClearButtonVisibility = ClearButtonVisibility.WhileEditing };
```

次のスクリーンショットでは、[ [`Entry`](xref:Xamarin.Forms.Entry) クリア] ボタンが有効になっているを示しています。

![IOS と Android でのクリアボタンを使用したエントリのスクリーンショット](entry-images/entry-clear-button.png)

## <a name="customize-the-keyboard"></a>キーボードをカスタマイズする

ユーザーがを操作するときに表示されるキーボードは、プロパティを使用して、 [`Entry`](xref:Xamarin.Forms.Entry) [`Keyboard`](xref:Xamarin.Forms.InputView.Keyboard) クラスの次のいずれかのプロパティにプログラムで設定でき [`Keyboard`](xref:Xamarin.Forms.Keyboard) ます。

- [`Chat`](xref:Xamarin.Forms.Keyboard.Chat) - 絵文字が使えるテキスト メッセージや場所に使います。
- [`Default`](xref:Xamarin.Forms.Keyboard.Default) - 既定のキーボード。
- [`Email`](xref:Xamarin.Forms.Keyboard.Email) - 電子メール アドレスを入力するときに使用します。
- [`Numeric`](xref:Xamarin.Forms.Keyboard.Numeric) - 数値を入力するときに使用します。
- [`Plain`](xref:Xamarin.Forms.Keyboard.Plain) - [`KeyboardFlags`](xref:Xamarin.Forms.KeyboardFlags) を指定しないで、テキストを入力するときに使用します。
- [`Telephone`](xref:Xamarin.Forms.Keyboard.Telephone) - 電話番号を入力するときに使用します。
- [`Text`](xref:Xamarin.Forms.Keyboard.Text) - テキストを入力するときに使用します。
- [`Url`](xref:Xamarin.Forms.Keyboard.Url) - ファイル パスおよび Web アドレスを入力するために使用します。

XAML では次のようにしてこれを実現できます。

```xaml
<Entry Keyboard="Chat" />
```

これに相当する C# コードを次に示します。

```csharp
var entry = new Entry { Keyboard = Keyboard.Chat };
```

各キーボードの例については、「 [レシピ](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/Controls/choose-keyboard-for-entry) リポジトリ」を参照してください。

[`Keyboard`](xref:Xamarin.Forms.Keyboard) クラスには、大文字の設定、スペルチェック、および単語補完候補の動作を指定することで、キーボードをカスタマイズするために使用できる [`Create`](xref:Xamarin.Forms.Keyboard.Create*) ファクトリ メソッドもあります。 [`KeyboardFlags`](xref:Xamarin.Forms.KeyboardFlags) 列挙値がメソッドへの引数として指定され、カスタマイズされた `Keyboard` が返されます。 `KeyboardFlags` 列挙体には次の値が含まれます。

- [`None`](xref:Xamarin.Forms.KeyboardFlags.None) - キーボードに機能は追加されません。
- [`CapitalizeSentence`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeSentence) - 入力された各文の最初の単語の最初の文字が自動的に大文字になることを示します。
- [`Spellcheck`](xref:Xamarin.Forms.KeyboardFlags.Spellcheck) - 入力したテキストに対してスペル チェックが実行されることを示します。
- [`Suggestions`](xref:Xamarin.Forms.KeyboardFlags.Suggestions) - 入力したテキストに対して単語補完が提供されることを示します。
- [`CapitalizeWord`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeWord) - 各単語の最初の文字が自動的に大文字になることを示します。
- [`CapitalizeCharacter`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeCharacter) - すべての文字が自動的に大文字になることを示します。
- [`CapitalizeNone`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeNone) - 大文字の自動設定を行わないことを示します。
- [`All`](xref:Xamarin.Forms.KeyboardFlags.All) - 入力したテキストに対して、スペルチェック、単語補完、および文への大文字の設定が行われることを示します。

次の XAML コード例は、既定の [`Keyboard`](xref:Xamarin.Forms.Keyboard) をカスタマイズして、単語補完を提供し、入力したすべての文字を大文字に設定する方法を示しています。

```xaml
<Entry Placeholder="Enter text here">
    <Entry.Keyboard>
        <Keyboard x:FactoryMethod="Create">
            <x:Arguments>
                <KeyboardFlags>Suggestions,CapitalizeCharacter</KeyboardFlags>
            </x:Arguments>
        </Keyboard>
    </Entry.Keyboard>
</Entry>
```

これに相当する C# コードを次に示します。

```csharp
var entry = new Entry { Placeholder = "Enter text here" };
entry.Keyboard = Keyboard.Create(KeyboardFlags.Suggestions | KeyboardFlags.CapitalizeCharacter);
```

### <a name="customize-the-return-key"></a>リターンキーをカスタマイズする

にフォーカスがあるときに表示される、ソフトキーボードの戻り値キーの外観は、 [`Entry`](xref:Xamarin.Forms.Entry) [`ReturnType`](xref:Xamarin.Forms.Entry.ReturnType) プロパティを列挙体の値に設定することによってカスタマイズでき [`ReturnType`](xref:Xamarin.Forms.ReturnType) ます。

- [`Default`](xref:Xamarin.Forms.ReturnType.Default) –特定の return キーが必要ないこと、およびプラットフォームの既定値が使用されることを示します。
- [`Done`](xref:Xamarin.Forms.ReturnType.Done) – "Done" 戻り値のキーを示します。
- [`Go`](xref:Xamarin.Forms.ReturnType.Go) – "進む" 戻り値のキーを示します。
- [`Next`](xref:Xamarin.Forms.ReturnType.Next) – "Next" 戻り値のキーを示します。
- [`Search`](xref:Xamarin.Forms.ReturnType.Search) – "Search" キーを返します。
- [`Send`](xref:Xamarin.Forms.ReturnType.Send) – "Send" キーを返します。

次の XAML の例は、return キーを設定する方法を示しています。

```xaml
<Entry ReturnType="Send" />
```

これに相当する C# コードを次に示します。

```csharp
var entry = new Entry { ReturnType = ReturnType.Send };
```

> [!NOTE]
> 戻り値キーの正確な外観は、プラットフォームによって異なります。 IOS では、リターンキーはテキストベースのボタンです。 ただし、Android とユニバーサル Windows プラットフォームでは、リターンキーはアイコンベースのボタンです。

Return キーが押されると、 [`Completed`](xref:Xamarin.Forms.Entry.Completed) イベントが発生し、 `ICommand` プロパティによって指定された [`ReturnCommand`](xref:Xamarin.Forms.Entry.ReturnCommand) が実行されます。 また、 `object` プロパティによって指定されたは [`ReturnCommandParameter`](xref:Xamarin.Forms.Entry.ReturnCommandParameter) 、 `ICommand` パラメーターとしてに渡されます。 コマンドについて詳しくは、[コマンド インターフェイス](~/xamarin-forms/app-fundamentals/data-binding/commanding.md)に関するページをご覧ください。

## <a name="enable-and-disable-spell-checking"></a>スペルチェックを有効または無効にする

プロパティは、 [`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) スペルチェックを有効にするかどうかを制御します。 既定では、プロパティはに設定されて `true` います。 ユーザーがテキストを入力すると、スペルミスが示されます。

ただし、一部のテキスト入力シナリオ (ユーザー名の入力など) では、スペルチェックで否定的なエクスペリエンスが提供されるため、プロパティをに設定することによって無効にする必要があり [`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) `false` ます。

```xaml
<Entry ... IsSpellCheckEnabled="false" />
```

```csharp
var entry = new Entry { ... IsSpellCheckEnabled = false };
```

> [!NOTE]
> [`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled)プロパティがに設定され `false` ていて、カスタムキーボードが使用されていない場合、ネイティブスペルチェックは無効になります。 ただし、など、 [`Keyboard`](xref:Xamarin.Forms.Keyboard) スペルチェックを無効にするが設定されている場合、 [`Keyboard.Chat`](xref:Xamarin.Forms.Keyboard.Chat) `IsSpellCheckEnabled` プロパティは無視されます。 このため、プロパティを使用して、明示的に無効にするのスペルチェックを有効にすることはできません `Keyboard` 。

## <a name="enable-and-disable-text-prediction"></a>テキスト予測を有効または無効にする

プロパティは、 [`IsTextPredictionEnabled`](xref:Xamarin.Forms.Entry.IsTextPredictionEnabled) テキストの予測と自動テキスト修正が有効かどうかを制御します。 既定では、プロパティはに設定されて `true` います。 ユーザーがテキストを入力すると、ワード予測が表示されます。

ただし、テキスト入力のシナリオによっては、ユーザー名の入力、テキスト予測、自動テキスト修正などがありますが、プロパティを次のように設定することで無効にする必要があり [`IsTextPredictionEnabled`](xref:Xamarin.Forms.Entry.IsTextPredictionEnabled) `false` ます。

```xaml
<Entry ... IsTextPredictionEnabled="false" />
```

```csharp
var entry = new Entry { ... IsTextPredictionEnabled = false };
```

> [!NOTE]
> [`IsTextPredictionEnabled`](xref:Xamarin.Forms.Entry.IsTextPredictionEnabled)プロパティがに設定され `false` ていて、カスタムキーボードが使用されていない場合、テキスト予測と自動テキスト修正は無効になります。 ただし、 [`Keyboard`](xref:Xamarin.Forms.Keyboard) テキスト予測を無効にするが設定されている場合、 `IsTextPredictionEnabled` プロパティは無視されます。 このため、プロパティを使用して、明示的に無効にするのテキスト予測を有効にすることはできません `Keyboard` 。

## <a name="colors"></a>色

次のバインド可能なプロパティを使用して、カスタムの背景色とテキスト色を使用するようにエントリを設定できます。

- **Textcolor** &ndash; テキストの色を設定します。
- **BackgroundColor** &ndash; テキストの背後に表示される色を設定します。

各プラットフォームで色が使用できるようにするには、特別な注意が必要です。 各プラットフォームにはテキストと背景色について異なる既定値があるため、1つを設定する場合は、両方を設定する必要があります。

エントリのテキストの色を設定するには、次のコードを使用します。

XAML の場合:

```xaml
<Entry TextColor="Green" />
```

C# の場合:

```csharp
var entry = new Entry();
entry.TextColor = Color.Green;
```

![エントリの TextColor の例](entry-images/textcolor.png)

プレースホルダーは、指定したの影響を受けないことに注意して `TextColor` ください。

XAML の背景色を設定するには:

```xaml
<Entry BackgroundColor="#2c3e50" />
```

C# の場合:

```csharp
var entry = new Entry();
entry.BackgroundColor = Color.FromHex("#2c3e50");
```

![エントリの BackgroundColor の例](entry-images/textbackgroundcolor.png)

選択した背景とテキストの色が各プラットフォームで使用できることを確認し、プレースホルダーテキストを不明瞭にしないように注意してください。

## <a name="events-and-interactivity"></a>イベントと対話機能

エントリは、次の2つのイベントを公開します。

- [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged)&ndash;エントリのテキストが変更されたときに発生します。 変更前と変更後のテキストを提供します。
- [`Completed`](xref:Xamarin.Forms.Entry.Completed)&ndash;ユーザーがキーボードの return キーを押して入力を終了したときに発生します。

> [!NOTE]
> [`VisualElement`](xref:Xamarin.Forms.VisualElement)継承元のクラスは、 [`Entry`](xref:Xamarin.Forms.Entry) とのイベントも持ってい [`Focused`](xref:Xamarin.Forms.VisualElement.Focused) [`Unfocused`](xref:Xamarin.Forms.VisualElement.Unfocused) ます。

### <a name="completed"></a>完了

`Completed`イベントは、エントリとの対話の完了に応答するために使用されます。 `Completed` は、ユーザーがキーボードの return キーを押すか、UWP の Tab キーを押すことによって、入力をフィールドで終了したときに発生します。 イベントのハンドラーは、送信者とを取得する汎用イベントハンドラーです `EventArgs` 。

```csharp
void Entry_Completed (object sender, EventArgs e)
{
    var text = ((Entry)sender).Text; //cast sender to access the properties of the Entry
}
```

完成したイベントは、XAML でサブスクライブできます。

```xaml
<Entry Completed="Entry_Completed" />
```

および C#:

```csharp
var entry = new Entry ();
entry.Completed += Entry_Completed;
```

イベントが [`Completed`](xref:Xamarin.Forms.Entry.Completed) 発生すると、 `ICommand` プロパティによって指定されたが実行され、 [`ReturnCommand`](xref:Xamarin.Forms.Entry.ReturnCommand) `object` に渡されるプロパティによって指定され [`ReturnCommandParameter`](xref:Xamarin.Forms.Entry.ReturnCommandParameter) `ICommand` ます。

### <a name="textchanged"></a>TextChanged

`TextChanged`イベントは、フィールドの内容の変更に対処するために使用されます。

`TextChanged` のが変更されるたびに、が発生し `Text` `Entry` ます。 イベントのハンドラーは、のインスタンスを受け取り `TextChangedEventArgs` ます。 `TextChangedEventArgs` プロパティとプロパティを使用して、の古い値と新しい値へのアクセスを提供し `Entry` `Text` `OldTextValue` `NewTextValue` ます。

```csharp
void Entry_TextChanged (object sender, TextChangedEventArgs e)
{
    var oldText = e.OldTextValue;
    var newText = e.NewTextValue;
}
```

イベントは、 `TextChanged` XAML でサブスクライブできます。

```xaml
<Entry TextChanged="Entry_TextChanged" />
```

および C#:

```csharp
var entry = new Entry ();
entry.TextChanged += Entry_TextChanged;
```

## <a name="related-links"></a>関連リンク

- [Text (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-text)
- [Entry API](xref:Xamarin.Forms.Entry)