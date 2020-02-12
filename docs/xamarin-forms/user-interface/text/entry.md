---
title: Xamarin.Forms のエントリ
description: この記事では、Xamarin.Forms エントリ クラスを使用して、1 行のテキストまたはアプリケーションでパスワードの入力をそのまま使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 9923C541-3C10-4D14-BAB5-C4D6C514FB1E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/25/2019
ms.openlocfilehash: c7daad8898d3048f10c9489ee4e2c78f147c6e52
ms.sourcegitcommit: ccbf914615c0ce6b3f308d930f7a77418aeb4dbc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/11/2020
ms.locfileid: "77131148"
---
# <a name="xamarinforms-entry"></a>Xamarin.Forms のエントリ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)

_単一行のテキストまたはパスワードの入力_

単一行のテキスト入力には、Xamarin. Forms [`Entry`](xref:Xamarin.Forms.Entry)が使用されます。 `Entry`( [`Editor`](xref:Xamarin.Forms.Editor)ビューなど) では、複数のキーボードの種類がサポートされています。 また、`Entry` をパスワードフィールドとして使用することもできます。

## <a name="display-customization"></a>表示のカスタマイズ

### <a name="setting-and-reading-text"></a>設定やテキストの読み取り

`Entry`は、他のテキスト表示ビューと同様に、 [`Text`](xref:Xamarin.Forms.InputView.Text)プロパティを公開します。 このプロパティを使用して、`Entry`によって表示されるテキストを設定および読み取ることができます。 次の例は、XAML で `Text` プロパティを設定する方法を示しています。

```xaml
<Entry Text="I am an Entry" />
```

C# の場合:

```csharp
var MyEntry = new Entry { Text = "I am an Entry" };
```

テキストを読み取るには、のC#`Text` プロパティにアクセスします。

```csharp
var text = MyEntry.Text;
```

### <a name="setting-placeholder-text"></a>プレース ホルダー テキストを設定

[`Entry`](xref:Xamarin.Forms.Entry)は、ユーザー入力を格納していない場合にプレースホルダーテキストを表示するように設定できます。 これを実現するには、 [`Placeholder`](xref:Xamarin.Forms.InputView.Placeholder)プロパティを `string`に設定します。これは、多くの場合、`Entry`に適したコンテンツの種類を示すために使用されます。 また、 [`PlaceholderColor`](xref:Xamarin.Forms.InputView.PlaceholderColor)プロパティを[`Color`](xref:Xamarin.Forms.Color)に設定して、プレースホルダーテキストの色を制御できます。

```xaml
<Entry Placeholder="Username" PlaceholderColor="Olive" />
```

```csharp
var entry = new Entry { Placeholder = "Username", PlaceholderColor = Color.Olive };
```

> [!NOTE]
> `Entry` の幅は、`WidthRequest` プロパティを設定することによって定義できます。 `Text` プロパティの値に基づいて定義されている `Entry` の幅に依存しません。

### <a name="preventing-text-entry"></a>テキスト入力の防止

ユーザーは、既定値 `false`を持つ `IsReadOnly` プロパティを `true`に設定することによって、 [`Entry`](xref:Xamarin.Forms.Entry)のテキストを変更できないようにすることができます。

```xaml
<Entry Text="This is a read-only Entry"
       IsReadOnly="true" />
```

```csharp
var entry = new Entry { Text = "This is a read-only Entry", IsReadOnly = true });
```

> [!NOTE]
> `IsReadonly` プロパティは、`Entry` の視覚的な外観を灰色に変更する `IsEnabled` プロパティとは異なり、 [`Entry`](xref:Xamarin.Forms.Entry)の外観を変更しません。

### <a name="limiting-input-length"></a>入力の長さの制限

[`MaxLength`](xref:Xamarin.Forms.InputView.MaxLength)プロパティを使用して、 [`Entry`](xref:Xamarin.Forms.Entry)に許可されている入力の長さを制限できます。 このプロパティは、正の整数に設定する必要があります。

```xaml
<Entry ... MaxLength="10" />
```

```csharp
var entry = new Entry { ... MaxLength = 10 };
```

[`MaxLength`](xref:Xamarin.Forms.InputView.MaxLength)のプロパティ値が0の場合は、入力が許可されないことを示し、 [`Entry`](xref:Xamarin.Forms.Entry)の既定値である `int.MaxValue`の値は、入力できる文字数に有効な制限がないことを示します。

### <a name="character-spacing"></a>文字間隔

`Entry.CharacterSpacing` プロパティを `double` 値に設定することによって、文字間隔を[`Entry`](xref:Xamarin.Forms.Entry)に適用できます。

```xaml
<Entry ...
       CharacterSpacing="10" />
```

同等の C# コードを次に示します。

```csharp
Entry entry = new Entry { CharacterSpacing = 10 };
```

結果として、 [`Entry`](xref:Xamarin.Forms.Entry)によって表示されるテキスト内の文字は、デバイスに依存しない単位 `CharacterSpacing` 間隔が区別されます。

> [!NOTE]
> `CharacterSpacing` プロパティの値は、`Text` プロパティと `Placeholder` プロパティによって表示されるテキストに適用されます。

### <a name="password-fields"></a>パスワード フィールド

`Entry` は `IsPassword` プロパティを提供します。 `IsPassword` が `true`すると、フィールドの内容が黒い円として表示されます。

で XAML:

```xaml
<Entry IsPassword="true" />
```

C# の場合:

```csharp
var MyEntry = new Entry { IsPassword = true };
```

![Entry IsPassword の例](entry-images/password.png)

プレースホルダーは、パスワードフィールドとして構成されている `Entry` のインスタンスで使用できます。

で XAML:

```xaml
<Entry IsPassword="true" Placeholder="Password" />
```

C# の場合:

```csharp
var MyEntry = new Entry { IsPassword = true, Placeholder = "Password" };
```

![Entry IsPassword と Placeholder の例](entry-images/passwordplaceholder.png)

### <a name="setting-the-cursor-position-and-text-selection-length"></a>カーソルの位置とテキスト選択範囲の長さの設定

[`CursorPosition`](xref:Xamarin.Forms.Entry.CursorPosition)プロパティを使用すると、 [`Text`](xref:Xamarin.Forms.InputView.Text)プロパティに格納されている文字列に次の文字が挿入される位置を取得または設定できます。

```xaml
<Entry Text="Cursor position set" CursorPosition="5" />
```

```csharp
var entry = new Entry { Text = "Cursor position set", CursorPosition = 5 };
```

[`CursorPosition`](xref:Xamarin.Forms.Entry.CursorPosition)プロパティの既定値は0です。これは、テキストが `Entry`の先頭に挿入されることを示します。

また、 [`SelectionLength`](xref:Xamarin.Forms.Entry.SelectionLength)プロパティを使用して、`Entry`内のテキスト選択の長さを取得または設定することもできます。

```xaml
<Entry Text="Cursor position and selection length set" CursorPosition="2" SelectionLength="10" />
```

```csharp
var entry = new Entry { Text = "Cursor position and selection length set", CursorPosition = 2, SelectionLength = 10 };
```

[`SelectionLength`](xref:Xamarin.Forms.Entry.SelectionLength)プロパティの既定値は0です。これは、テキストが選択されていないことを示します。

### <a name="displaying-a-clear-button"></a>クリアボタンを表示する

`ClearButtonVisibility` プロパティを使用して、 [`Entry`](xref:Xamarin.Forms.Entry)がクリアボタンを表示するかどうかを制御し、ユーザーがテキストをクリアできるようにすることができます。 このプロパティは、`ClearButtonVisibility` 列挙型のメンバーに設定する必要があります。

- `Never` は、クリアボタンが表示されないことを示します。 これは、`Entry.ClearButtonVisibility` プロパティの既定値です。
- `WhileEditing` は、 [`Entry`](xref:Xamarin.Forms.Entry)にフォーカスとテキストが表示されている間、クリアボタンが表示されることを示します。

次の例は、XAML でプロパティを設定する方法を示しています。

```xaml
<Entry Text="Xamarin.Forms"
       ClearButtonVisibility="WhileEditing" />
```

同等の C# コードを次に示します。

```csharp
var entry = new Entry { Text = "Xamarin.Forms", ClearButtonVisibility = ClearButtonVisibility.WhileEditing };
```

次のスクリーンショットは、[クリア] ボタンが有効になっている[`Entry`](xref:Xamarin.Forms.Entry)を示しています。

![IOS と Android でのクリアボタンを使用したエントリのスクリーンショット](entry-images/entry-clear-button.png)

### <a name="customizing-the-keyboard"></a>キーボードのカスタマイズ

ユーザーが[`Entry`](xref:Xamarin.Forms.Entry)と対話するときに表示されるキーボードは、 [`Keyboard`](xref:Xamarin.Forms.InputView.Keyboard)プロパティを使用して、 [`Keyboard`](xref:Xamarin.Forms.Keyboard)クラスの次のいずれかのプロパティにプログラムで設定できます。

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

同等の C# コードを次に示します。

```csharp
var entry = new Entry { Keyboard = Keyboard.Chat };
```

各キーボードの例については、「[レシピ](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/Controls/choose-keyboard-for-entry)リポジトリ」を参照してください。

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

同等の C# コードを次に示します。

```csharp
var entry = new Entry { Placeholder = "Enter text here" };
entry.Keyboard = Keyboard.Create(KeyboardFlags.Suggestions | KeyboardFlags.CapitalizeCharacter);
```

#### <a name="customizing-the-return-key"></a>戻り値のキーのカスタマイズ

[`Entry`](xref:Xamarin.Forms.Entry)にフォーカスがあるときに表示されるソフトキーボードの戻りキーの外観は、 [`ReturnType`](xref:Xamarin.Forms.Entry.ReturnType)プロパティを[`ReturnType`](xref:Xamarin.Forms.ReturnType)列挙体の値に設定することによってカスタマイズできます。

- [`Default`](xref:Xamarin.Forms.ReturnType.Default) –特定のリターンキーが必要ないこと、およびプラットフォームの既定値が使用されることを示します。
- [`Done`](xref:Xamarin.Forms.ReturnType.Done) – "Done" 戻り値のキーを示します。
- [`Go`](xref:Xamarin.Forms.ReturnType.Go) – "進む" 戻り値のキーを示します。
- [`Next`](xref:Xamarin.Forms.ReturnType.Next) – "Next" 戻り値のキーを示します。
- [`Search`](xref:Xamarin.Forms.ReturnType.Search) – "Search" return キーを示します。
- [`Send`](xref:Xamarin.Forms.ReturnType.Send) – "Send" キーを返します。

次の XAML の例では、戻り値のキーを設定する方法を示します。

```xaml
<Entry ReturnType="Send" />
```

同等の C# コードを次に示します。

```csharp
var entry = new Entry { ReturnType = ReturnType.Send };
```

> [!NOTE]
> 戻り値のキーの正確な外観は、プラットフォームによって異なります。 Ios では、戻り値のキーは、テキスト ベースのボタンです。 ただし、Android およびユニバーサル Windows プラットフォームでは、戻り値のキーは、アイコンに基づくボタンです。

Return キーが押されると、 [`Completed`](xref:Xamarin.Forms.Entry.Completed)イベントが発生し、 [`ReturnCommand`](xref:Xamarin.Forms.Entry.ReturnCommand)プロパティによって指定されたすべての `ICommand` が実行されます。 また、 [`ReturnCommandParameter`](xref:Xamarin.Forms.Entry.ReturnCommandParameter)プロパティによって指定された `object` は、パラメーターとして `ICommand` に渡されます。 コマンドについて詳しくは、[コマンド インターフェイス](~/xamarin-forms/app-fundamentals/data-binding/commanding.md)に関するページをご覧ください。

### <a name="enabling-and-disabling-spell-checking"></a>有効にして、スペル チェックを無効化

[`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled)プロパティは、スペルチェックを有効にするかどうかを制御します。 既定では、プロパティは `true`に設定されています。 テキストを入力すると、スペル ミスが示されます。

ただし、一部のテキスト入力シナリオ (ユーザー名の入力など) では、スペルチェックで否定的なエクスペリエンスが提供されるため、 [`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled)プロパティを `false`に設定して無効にする必要があります。

```xaml
<Entry ... IsSpellCheckEnabled="false" />
```

```csharp
var entry = new Entry { ... IsSpellCheckEnabled = false };
```

> [!NOTE]
> [`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled)プロパティが `false`に設定されていて、カスタムキーボードが使用されていない場合、ネイティブスペルチェックは無効になります。 ただし、 [`Keyboard.Chat`](xref:Xamarin.Forms.Keyboard.Chat)などのスペルチェックを無効にする[`Keyboard`](xref:Xamarin.Forms.Keyboard)が設定されている場合、`IsSpellCheckEnabled` プロパティは無視されます。 このため、プロパティを使用して、明示的に無効にした `Keyboard` のスペルチェックを有効にすることはできません。

### <a name="enabling-and-disabling-text-prediction"></a>有効にして、予測入力を無効化

[`IsTextPredictionEnabled`](xref:Xamarin.Forms.Entry.IsTextPredictionEnabled)プロパティは、テキストの予測と自動テキスト修正が有効かどうかを制御します。 既定では、プロパティは `true`に設定されています。 テキストを入力すると、word の予測が表示されます。

ただし、テキスト入力のシナリオによっては、ユーザー名の入力、テキスト予測、自動テキスト修正などがあります。そのため、 [`IsTextPredictionEnabled`](xref:Xamarin.Forms.Entry.IsTextPredictionEnabled)プロパティを `false`に設定して無効にする必要があります。

```xaml
<Entry ... IsTextPredictionEnabled="false" />
```

```csharp
var entry = new Entry { ... IsTextPredictionEnabled = false };
```

> [!NOTE]
> [`IsTextPredictionEnabled`](xref:Xamarin.Forms.Entry.IsTextPredictionEnabled)プロパティが `false`に設定されていて、カスタムキーボードが使用されていない場合、テキスト予測と自動テキスト修正は無効になります。 ただし、テキストの予測を無効にする[`Keyboard`](xref:Xamarin.Forms.Keyboard)が設定されている場合、`IsTextPredictionEnabled` プロパティは無視されます。 このため、プロパティを使用して、明示的に無効にする `Keyboard` のテキスト予測を有効にすることはできません。

### <a name="colors"></a>色

カスタムの背景と、次のバインド可能なプロパティを使用してテキストの色を使用するエントリを設定できます。

- **Textcolor** &ndash; テキストの色を設定します。
- **BackgroundColor** &ndash; テキストの背後に表示される色を設定します。

各プラットフォームで色を使用するには特別な注意が必要です。 各プラットフォームのテキストと背景色に異なる既定値には、ため多くの場合、いずれかを設定した場合は、両方を設定する必要あります。

エントリのテキストの色を設定するのにには、次のコードを使用します。

で XAML:

```xaml
<Entry TextColor="Green" />
```

C# の場合:

```csharp
var entry = new Entry();
entry.TextColor = Color.Green;
```

![エントリの TextColor の例](entry-images/textcolor.png)

プレースホルダーは、指定された `TextColor`の影響を受けないことに注意してください。

XAML では、背景色を設定します。

```xaml
<Entry BackgroundColor="#2c3e50" />
```

C# の場合:

```csharp
var entry = new Entry();
entry.BackgroundColor = Color.FromHex("#2c3e50");
```

![エントリの BackgroundColor の例](entry-images/textbackgroundcolor.png)

選択した色の背景とテキストの色が各プラットフォームで使用可能なし、任意のプレース ホルダー テキストが不明瞭かどうかを確認するように注意します。

## <a name="events-and-interactivity"></a>イベントと対話

エントリは、2 つのイベントを公開します。

- [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) 、エントリのテキストが変更されたときに発生 &ndash; ます。 変更の前後にテキストを提供します。
- [`Completed`](xref:Xamarin.Forms.Entry.Completed) &ndash;、ユーザーがキーボードの return キーを押して入力を終了したときに発生します。

> [!NOTE]
> [`Entry`](xref:Xamarin.Forms.Entry)を継承する[`VisualElement`](xref:Xamarin.Forms.VisualElement)クラスにも[`Focused`](xref:Xamarin.Forms.VisualElement.Focused)および[`Unfocused`](xref:Xamarin.Forms.VisualElement.Unfocused)イベントがあります。

### <a name="completed"></a>[完了]

`Completed` イベントは、エントリとの対話の完了に応答するために使用されます。 `Completed` は、ユーザーがキーボードの return キーを押すか、UWP の Tab キーを押すことによって、入力をフィールドで終了したときに発生します。 イベントのハンドラーは、送信者と `EventArgs`を取得する汎用イベントハンドラーです。

```csharp
void Entry_Completed (object sender, EventArgs e)
{
    var text = ((Entry)sender).Text; //cast sender to access the properties of the Entry
}
```

XAML の完了イベントにサブスクライブできます。

```xaml
<Entry Completed="Entry_Completed" />
```

c#:

```csharp
var entry = new Entry ();
entry.Completed += Entry_Completed;
```

[`Completed`](xref:Xamarin.Forms.Entry.Completed)イベントが発生すると、 [`ReturnCommand`](xref:Xamarin.Forms.Entry.ReturnCommand)プロパティによって指定されたすべての `ICommand` が実行され、 [`ReturnCommandParameter`](xref:Xamarin.Forms.Entry.ReturnCommandParameter)プロパティによって指定された `object` が `ICommand`に渡されます。

### <a name="textchanged"></a>TextChanged

`TextChanged` イベントは、フィールドの内容の変更に対処するために使用されます。

`Entry` の `Text` が変更されるたびに `TextChanged` が発生します。 イベントのハンドラーは、`TextChangedEventArgs`のインスタンスを受け取ります。 `TextChangedEventArgs` は、`OldTextValue` および `NewTextValue` のプロパティを使用して、`Entry` `Text` の新旧の値へのアクセスを提供します。

```csharp
void Entry_TextChanged (object sender, TextChangedEventArgs e)
{
    var oldText = e.OldTextValue;
    var newText = e.NewTextValue;
}
```

`TextChanged` イベントは、XAML でサブスクライブできます。

```xaml
<Entry TextChanged="Entry_TextChanged" />
```

c#:

```csharp
var entry = new Entry ();
entry.TextChanged += Entry_TextChanged;
```

## <a name="related-links"></a>関連リンク

- [Text (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
- [Entry API](xref:Xamarin.Forms.Entry)
