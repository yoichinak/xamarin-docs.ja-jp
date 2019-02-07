---
title: Xamarin.Forms のエントリ
description: この記事では、Xamarin.Forms エントリ クラスを使用して、1 行のテキストまたはアプリケーションでパスワードの入力をそのまま使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 9923C541-3C10-4D14-BAB5-C4D6C514FB1E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/27/2018
ms.openlocfilehash: 303cca48defdadd69449edbd6c4c3f5e4410bbbb
ms.sourcegitcommit: 93c9fe61eb2cdfa530960b4253eb85161894c882
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/07/2019
ms.locfileid: "55831964"
---
# <a name="xamarinforms-entry"></a>Xamarin.Forms のエントリ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)

_単一行のテキストまたはパスワードを入力_

Xamarin.Forms [ `Entry` ](xref:Xamarin.Forms.Entry)を単一行のテキスト入力に使用します。 `Entry`と同様に、 [ `Editor` ](xref:Xamarin.Forms.Editor)ビューで、複数のキーボードの種類をサポートしています。 さらに、`Entry`パスワード フィールドとして使用できます。

## <a name="display-customization"></a>表示のカスタマイズ

### <a name="setting-and-reading-text"></a>設定やテキストの読み取り

`Entry`などの他のテキストを表すビューを公開、 [ `Text` ](xref:Xamarin.Forms.Entry.Text)プロパティ。 このプロパティは、設定し、によって提示されるテキストの読み取りに使用できます、`Entry`します。 次の例では、設定、 `Text` XAML のプロパティ。

```xaml
<Entry Text="I am an Entry" />
```

C# の場合:

```csharp
var MyEntry = new Entry { Text = "I am an Entry" };
```

テキストを読み取るには、アクセス、`Text`プロパティ (C#)。

```csharp
var text = MyEntry.Text;
```

### <a name="setting-placeholder-text"></a>プレース ホルダー テキストを設定

[ `Entry` ](xref:Xamarin.Forms.Entry)プレース ホルダー テキストを表示するユーザー入力を格納するがない場合に設定することができます。 これは、設定によって実現されます、 [ `Placeholder` ](xref:Xamarin.Forms.Entry.Placeholder)プロパティを`string`は適切なコンテンツの種類を示すためによく使用して、 `Entry`。 さらに、プレース ホルダー テキストの色を設定して制御できます、 [ `PlaceholderColor` ](xref:Xamarin.Forms.Entry.PlaceholderColor)プロパティを[ `Color` ](xref:Xamarin.Forms.Color):

```xaml
<Entry Placeholder="Username" PlaceholderColor="Olive" />
```

```csharp
var entry = new Entry { Placeholder = "Username", PlaceholderColor = Color.Olive };
```

> [!NOTE]
> 幅、`Entry`設定によってはその`WidthRequest`プロパティ。 幅に依存しない、`Entry`の値に基づいて定義されているその`Text`プロパティ。

### <a name="limiting-input-length"></a>入力の長さの制限

[ `MaxLength` ](xref:Xamarin.Forms.InputView.MaxLength)で使用できるように入力文字列の長さを制限するプロパティを使用できます、 [ `Entry`](xref:Xamarin.Forms.Entry)します。 このプロパティは、正の整数に設定する必要があります。

```xaml
<Entry ... MaxLength="10" />
```

```csharp
var entry = new Entry { ... MaxLength = 10 };
```

A [ `MaxLength` ](xref:Xamarin.Forms.InputView.MaxLength)プロパティ値が 0 のでは、ある入力は許可されません、ことを示しますの値と`int.MaxValue`の既定値は、 [ `Entry` ](xref:Xamarin.Forms.Entry)、ことを示しますなし有効な入力可能性がありますの文字数制限。

### <a name="password-fields"></a>パスワード フィールド

`Entry` 提供、`IsPassword`プロパティ。 ときに`IsPassword`は`true`フィールドの内容が黒の円として表示されます。

で XAML:

```xaml
<Entry IsPassword="true" />
```

C# の場合:

```csharp
var MyEntry = new Entry { IsPassword = true };
```

![](entry-images/password.png "エントリ IsPassword 例")

インスタンスで使用できるプレース ホルダー`Entry`パスワード フィールドとして構成されています。

で XAML:

```xaml
<Entry IsPassword="true" Placeholder="Password" />
```

C# の場合:

```csharp
var MyEntry = new Entry { IsPassword = true, Placeholder = "Password" };
```

![](entry-images/passwordplaceholder.png "エントリ IsPassword およびプレース ホルダーの例")

### <a name="setting-the-cursor-position-and-text-selection-length"></a>カーソルの位置とテキスト選択範囲の長さの設定

[ `CursorPosition` ](xref:Xamarin.Forms.Entry.CursorPosition)に格納された文字列に、次の文字を挿入先である位置を設定するプロパティを使用できます、 [ `Text` ](xref:Xamarin.Forms.Entry.Text)プロパティ。

```xaml
<Entry Text="Cursor position set" CursorPosition="5" />
```

```csharp
var entry = new Entry { Text = "Cursor position set", CursorPosition = 5 };
```

既定値、 [ `CursorPosition` ](xref:Xamarin.Forms.Entry.CursorPosition)プロパティが 0 で、テキストの先頭に挿入することを示します、`Entry`します。

さらに、 [ `SelectionLength` ](xref:Xamarin.Forms.Entry.SelectionLength)内のテキスト選択範囲の長さを設定するプロパティを使用できます、 `Entry`:

```xaml
<Entry Text="Cursor position and selection length set" CursorPosition="2" SelectionLength="10" />
```

```csharp
var entry = new Entry { Text = "Cursor position and selection length set", CursorPosition = 2, SelectionLength = 10 };
```

既定値、 [ `SelectionLength` ](xref:Xamarin.Forms.Entry.SelectionLength)プロパティが 0 で、テキストが選択されていないことを示します。

### <a name="customizing-the-keyboard"></a>キーボードのカスタマイズ

ユーザーが対話する際に表示されるキーボード、 [ `Entry` ](xref:Xamarin.Forms.Entry)経由でプログラムによって設定できる、 [ `Keyboard` ](xref:Xamarin.Forms.InputView.Keyboard) から次のプロパティのいずれかのプロパティ[ `Keyboard` ](xref:Xamarin.Forms.Keyboard)クラス。

- [`Chat`](xref:Xamarin.Forms.Keyboard.Chat) – テキストの使用と絵文字が便利な場所。
- [`Default`](xref:Xamarin.Forms.Keyboard.Default) – 既定のキーボード。
- [`Email`](xref:Xamarin.Forms.Keyboard.Email) – 電子メール アドレスを入力するときに使用します。
- [`Numeric`](xref:Xamarin.Forms.Keyboard.Numeric) – 数値を入力するときに使用します。
- [`Plain`](xref:Xamarin.Forms.Keyboard.Plain) – なしのテキストを入力するときに使用される[ `KeyboardFlags` ](xref:Xamarin.Forms.KeyboardFlags)指定します。
- [`Telephone`](xref:Xamarin.Forms.Keyboard.Telephone) – 電話番号を入力するときに使用します。
- [`Text`](xref:Xamarin.Forms.Keyboard.Text) – テキストを入力するときに使用します。
- [`Url`](xref:Xamarin.Forms.Keyboard.Url) – web アドレス (&)、ファイルのパスを入力するために使用します。

これで実行できます XAML には、次のようにします。

```xaml
<Entry Keyboard="Chat" />
```

同等の c# コードに示します。

```csharp
var entry = new Entry { Keyboard = Keyboard.Chat };
```

各キーボードの例が記載されて、[レシピ](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/Controls/choose-keyboard-for-entry)リポジトリ。

[ `Keyboard` ](xref:Xamarin.Forms.Keyboard)クラスがあります、 [ `Create` ](xref:Xamarin.Forms.Keyboard.Create*)大文字と小文字、スペル チェック、および修正候補の動作を指定することで、キーボードをカスタマイズするために使用するファクトリ メソッド。 [`KeyboardFlags`](xref:Xamarin.Forms.KeyboardFlags) 列挙値は、カスタマイズされたメソッドに引数として指定`Keyboard`返されます。 `KeyboardFlags`列挙には、次の値が含まれています。

- [`None`](xref:Xamarin.Forms.KeyboardFlags.None) – キーボード機能は追加されません。
- [`CapitalizeSentence`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeSentence) – を各入力文の最初の単語の最初の文字は自動的に大文字で入力することを示します。
- [`Spellcheck`](xref:Xamarin.Forms.KeyboardFlags.Spellcheck) – 入力したテキストでそのスペル チェックは実行を示します。
- [`Suggestions`](xref:Xamarin.Forms.KeyboardFlags.Suggestions) – 入力したテキストの入力候補が提供されるその単語を示します。
- [`CapitalizeWord`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeWord) – 各単語の最初の文字が自動的に大文字にすることを示します。
- [`CapitalizeCharacter`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeCharacter) – すべての文字が自動的に大文字にすることを示します。
- [`CapitalizeNone`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeNone) – 自動大文字と小文字が発生しないことを示します。
- [`All`](xref:Xamarin.Forms.KeyboardFlags.All) – スペル チェック、単語の入力候補、および大文字が入力したテキストで発生することを示します。

次の XAML コード例は、既定値をカスタマイズする方法を示しています。 [ `Keyboard` ](xref:Xamarin.Forms.Keyboard)をの単語候補を提供し、すべての入力した文字を大文字に変換します。

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

同等の c# コードに示します。

```csharp
var entry = new Entry { Placeholder = "Enter text here" };
entry.Keyboard = Keyboard.Create(KeyboardFlags.Suggestions | KeyboardFlags.CapitalizeCharacter);
```

#### <a name="customizing-the-return-key"></a>戻り値のキーのカスタマイズ

これは、ソフト キーボードの戻り値のキーの外観ときに表示される、 [ `Entry` ](xref:Xamarin.Forms.Entry)フォーカスを設定してカスタマイズすることができます、 [ `ReturnType` ](xref:Xamarin.Forms.Entry.ReturnType)プロパティ値をの[ `ReturnType` ](xref:Xamarin.Forms.ReturnType)列挙体。

- [`Default`](xref:Xamarin.Forms.ReturnType.Default) – 特定の戻り値のキーが不要であると、プラットフォームの既定値が使用されることを示します。
- [`Done`](xref:Xamarin.Forms.ReturnType.Done) –"Done"戻り値のキーを示します。
- [`Go`](xref:Xamarin.Forms.ReturnType.Go) –"Go"戻り値のキーを示します。
- [`Next`](xref:Xamarin.Forms.ReturnType.Next) -[次へ] の戻り値のキーを示します。
- [`Search`](xref:Xamarin.Forms.ReturnType.Search) –「検索」の戻り値のキーを示します。
- [`Send`](xref:Xamarin.Forms.ReturnType.Send) –「送信」の戻り値のキーを示します。

次の XAML の例では、戻り値のキーを設定する方法を示します。

```xaml
<Entry ReturnType="Send" />
```

同等の c# コードに示します。

```csharp
var entry = new Entry { ReturnType = ReturnType.Send };
```

> [!NOTE]
> 戻り値のキーの正確な外観は、プラットフォームによって異なります。 Ios では、戻り値のキーは、テキスト ベースのボタンです。 ただし、Android およびユニバーサル Windows プラットフォームでは、戻り値のキーは、アイコンに基づくボタンです。

戻り値のキーが押されたときに、 [ `Completed` ](xref:Xamarin.Forms.Entry.Completed)イベントが発生して、`ICommand`で指定された、 [ `ReturnCommand` ](xref:Xamarin.Forms.Entry.ReturnCommand)プロパティを実行します。 さらに、すべて`object`で指定された、 [ `ReturnCommandParameter` ](xref:Xamarin.Forms.Entry.ReturnCommandParameter)プロパティに渡される、`ICommand`をパラメーターとして。 コマンドの詳細については、次を参照してください。 [、のコマンド インターフェイス](~/xamarin-forms/app-fundamentals/data-binding/commanding.md)します。

### <a name="enabling-and-disabling-spell-checking"></a>有効にして、スペル チェックを無効化

[ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled)プロパティ コントロールかどうかスペル チェックを有効にします。 既定では、プロパティに設定が`true`します。 テキストを入力すると、スペル ミスが示されます。

ただし、ユーザー名を入力するなど、いくつかのテキスト エントリ シナリオのスペル チェック、負のエクスペリエンスを提供します、設定して無効にする必要があります、 [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled)プロパティを`false`:

```xaml
<Entry ... IsSpellCheckEnabled="false" />
```

```csharp
var entry = new Entry { ... IsSpellCheckEnabled = false };
```

> [!NOTE]
> ときに、 [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled)プロパティに設定されて`false`、およびカスタムのキーボードが使用されていない、ネイティブのスペル チェックが無効になります。 ただし場合、 [ `Keyboard` ](xref:Xamarin.Forms.Keyboard)がされているセットをスペル チェックを無効にするチェックを行うなど[ `Keyboard.Chat` ](xref:Xamarin.Forms.Keyboard.Chat)、`IsSpellCheckEnabled`プロパティは無視されます。 そのため、スペル チェックを有効にするプロパティを使用できません、`Keyboard`を明示的に無効にします。

### <a name="enabling-and-disabling-text-prediction"></a>有効にして、予測入力を無効化

[ `IsTextPredictionEnabled` ](xref:Xamarin.Forms.Entry.IsTextPredictionEnabled)プロパティ コントロールかどうか予測入力と自動テキストの修正を有効にします。 既定では、プロパティに設定が`true`します。 テキストを入力すると、word の予測が表示されます。

ただし、テキスト エントリ シナリオによっては、ユーザー名、予測入力とテキストの自動入力など修正負のエクスペリエンスを提供しますを設定して無効にする必要があります、 [ `IsTextPredictionEnabled` ](xref:Xamarin.Forms.Entry.IsTextPredictionEnabled)プロパティを`false`:。

```xaml
<Entry ... IsTextPredictionEnabled="false" />
```

```csharp
var entry = new Entry { ... IsTextPredictionEnabled = false };
```

> [!NOTE]
> ときに、 [ `IsTextPredictionEnabled` ](xref:Xamarin.Forms.Entry.IsTextPredictionEnabled)プロパティに設定されて`false`、カスタムのキーボードがされていないと予測入力と自動的に使用されるテキストの修正が無効になっています。 ただし場合、 [ `Keyboard` ](xref:Xamarin.Forms.Keyboard)を無効にします。 テキストの予測が設定されている、`IsTextPredictionEnabled`プロパティは無視されます。 そのための予測の入力を有効にするプロパティを使用できません、`Keyboard`を明示的に無効にします。

### <a name="colors"></a>色

カスタムの背景と、次のバインド可能なプロパティを使用してテキストの色を使用するエントリを設定できます。

- **TextColor** &ndash;テキストの色を設定します。
- **BackgroundColor** &ndash;テキストの背景の色を設定します。

特別な注意は、色は各プラットフォームで使用できることを確認する必要があります。 各プラットフォームのテキストと背景色に異なる既定値には、ため多くの場合、いずれかを設定した場合は、両方を設定する必要あります。

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

![](entry-images/textcolor.png "エントリ TextColor 例")

プレース ホルダーはありませんが、指定した影響を受ける`TextColor`します。

XAML では、背景色を設定します。

```xaml
<Entry BackgroundColor="#2c3e50" />
```

C# の場合:

```csharp
var entry = new Entry();
entry.BackgroundColor = Color.FromHex("#2c3e50");
```

![](entry-images/textbackgroundcolor.png "エントリの BackgroundColor 例")

選択した色の背景とテキストの色が各プラットフォームで使用可能なし、任意のプレース ホルダー テキストが不明瞭かどうかを確認するように注意します。

## <a name="events-and-interactivity"></a>イベントと対話

エントリは、2 つのイベントを公開します。

- [`TextChanged`](xref:Xamarin.Forms.Entry.TextChanged) &ndash; エントリのテキストが変更されたときに発生します。 変更の前後にテキストを提供します。
- [`Completed`](xref:Xamarin.Forms.Entry.Completed) &ndash; ユーザーには、キーボードの戻り値のキーを押して、入力が終了したときに発生します。

### <a name="completed"></a>完了

`Completed`エントリとのやり取りの完了に反応するイベントを使用します。 `Completed` ユーザー、キーボードの戻り値のキーを押して、フィールドを使用して入力が終了したときに発生します。 イベントのハンドラーは、送信者を取得、汎用イベント ハンドラーと`EventArgs`:

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

後に、 [ `Completed` ](xref:Xamarin.Forms.Entry.Completed)どのイベントが発生`ICommand`で指定された、 [ `ReturnCommand` ](xref:Xamarin.Forms.Entry.ReturnCommand)プロパティを実行するで、`object`で指定された、 [ `ReturnCommandParameter`](xref:Xamarin.Forms.Entry.ReturnCommandParameter)プロパティに渡される、`ICommand`します。

### <a name="textchanged"></a>TextChanged

`TextChanged`イベントは、フィールドの内容の変更に対応するために使用します。

`TextChanged` ときに発生しますが、`Text`の`Entry`変更します。 インスタンスを受け取り、イベントのハンドラーは`TextChangedEventArgs`します。 `TextChangedEventArgs` 新旧の値にアクセスできるように、 `Entry` `Text`を使用して、`OldTextValue`と`NewTextValue`プロパティ。

```csharp
void Entry_TextChanged (object sender, TextChangedEventArgs e)
{
    var oldText = e.OldTextValue;
    var newText = e.NewTextValue;
}
```

`TextChanged` XAML イベントにサブスクライブできます。

```xaml
<Entry TextChanged="Entry_TextChanged" />
```

c#:

```csharp
var entry = new Entry ();
entry.TextChanged += Entry_TextChanged;
```


## <a name="related-links"></a>関連リンク

- [テキスト (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [入力 API](xref:Xamarin.Forms.Entry)
