---
title: Xamarin. フォームエディター
description: この記事では、Xamarin Editor コントロールを使用して、アプリケーションでの複数行テキスト入力を受け入れる方法について説明します。
ms.prod: xamarin
ms.assetid: 7074DB3A-30D2-4A6B-9A89-B029EEF20B07
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/26/2019
ms.openlocfilehash: 0c610d7bdecc5d3454079be38c7e6ede5f0596e1
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "72696802"
---
# <a name="xamarinforms-editor"></a>Xamarin. フォームエディター

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)

_複数行テキスト入力_

[@No__t_1](xref:Xamarin.Forms.Editor)コントロールは、複数行の入力を受け入れるために使用されます。 この記事では次の内容について説明します。

- **[カスタマイズ](#customization)** &ndash; キーボードと色のオプション。
- **[インタラクティビティ &ndash; 対話](#interactivity)** 機能を提供するためにをリッスンできるイベントです。

## <a name="customization"></a>カスタマイズ

### <a name="setting-and-reading-text"></a>設定とテキストの読み取り

[@No__t_1](xref:Xamarin.Forms.Editor)は、他のテキスト表示ビューと同様に、`Text` プロパティを公開します。 このプロパティを使用して、`Editor` によって表示されるテキストを設定および読み取ることができます。 次の例は、XAML で `Text` プロパティを設定する方法を示しています。

```xaml
<Editor Text="I am an Editor" />
```

C# の場合:

```csharp
var MyEditor = new Editor { Text = "I am an Editor" };
```

テキストを読み取るには、のC#`Text` プロパティにアクセスします。

```csharp
var text = MyEditor.Text;
```

### <a name="setting-placeholder-text"></a>プレースホルダーテキストの設定

[@No__t_1](xref:Xamarin.Forms.Editor)は、ユーザー入力を格納していない場合にプレースホルダーテキストを表示するように設定できます。 これを実現するには、 [`Placeholder`](xref:Xamarin.Forms.Editor.Placeholder)プロパティを `string` に設定します。これは、多くの場合、`Editor` に適したコンテンツの種類を示すために使用されます。 また、 [`PlaceholderColor`](xref:Xamarin.Forms.Editor.PlaceholderColor)プロパティを[`Color`](xref:Xamarin.Forms.Color)に設定して、プレースホルダーテキストの色を制御できます。

```xaml
<Editor Placeholder="Enter text here" PlaceholderColor="Olive" />
```

```csharp
var editor = new Editor { Placeholder = "Enter text here", PlaceholderColor = Color.Olive };
```

### <a name="preventing-text-entry"></a>テキスト入力の防止

ユーザーは、既定値 `false` を持つ `IsReadOnly` プロパティを `true` に設定することによって、 [`Editor`](xref:Xamarin.Forms.Editor)のテキストを変更できないようにすることができます。

```xaml
<Editor Text="This is a read-only Editor"
        IsReadOnly="true" />
```

```csharp
var editor = new Editor { Text = "This is a read-only Editor", IsReadOnly = true });
```

> [!NOTE]
> @No__t_0 プロパティは、`Editor` の視覚的な外観を灰色に変更する `IsEnabled` プロパティとは異なり、 [`Editor`](xref:Xamarin.Forms.Editor)の外観を変更しません。

### <a name="limiting-input-length"></a>制限 (入力の長さを)

[@No__t_1](xref:Xamarin.Forms.InputView.MaxLength)プロパティを使用して、 [`Editor`](xref:Xamarin.Forms.Editor)に許可されている入力の長さを制限できます。 このプロパティは、正の整数に設定する必要があります。

```xaml
<Editor ... MaxLength="10" />
```

```csharp
var editor = new Editor { ... MaxLength = 10 };
```

[@No__t_1](xref:Xamarin.Forms.InputView.MaxLength)のプロパティ値が0の場合は、入力が許可されないことを示し、 [`Editor`](xref:Xamarin.Forms.Editor)の既定値である `int.MaxValue` の値は、入力できる文字数に有効な制限がないことを示します。

### <a name="character-spacing"></a>文字間隔

@No__t_2 プロパティを `double` 値に設定することによって、文字間隔を[`Editor`](xref:Xamarin.Forms.Editor)に適用できます。

```xaml
<Editor ...
        CharacterSpacing="10" />
```

これに相当する C# コードを次に示します。

```csharp
Editor editor = new editor { CharacterSpacing = 10 };
```

結果として、 [`Editor`](xref:Xamarin.Forms.Editor)によって表示されるテキスト内の文字は、デバイスに依存しない単位 `CharacterSpacing` 間隔が区別されます。

> [!NOTE]
> @No__t_0 プロパティの値は、`Text` プロパティと `Placeholder` プロパティによって表示されるテキストに適用されます。

### <a name="auto-sizing-an-editor"></a>エディターの自動サイズ変更

[@No__t_3](xref:Xamarin.Forms.Editor.AutoSize)プロパティを[`TextChanges`](xref:Xamarin.Forms.EditorAutoSizeOption.TextChanges)に設定することにより、コンテンツへの自動サイズ変更を行う[`Editor`](xref:Xamarin.Forms.Editor)できます。これは、 [`EditoAutoSizeOption`](xref:Xamarin.Forms.EditorAutoSizeOption)列挙体の値です。 この列挙体には、次の2つの値があります。

- [`Disabled`](xref:Xamarin.Forms.EditorAutoSizeOption.Disabled)は、自動サイズ変更が無効になっていることを示します。これは既定値です。
- [`TextChanges`](xref:Xamarin.Forms.EditorAutoSizeOption.TextChanges)は、自動サイズ変更が有効になっていることを示します。

これは、次のようにコードで行うことができます。

```xaml
<Editor Text="Enter text here" AutoSize="TextChanges" />
```

```csharp
var editor = new Editor { Text = "Enter text here", AutoSize = EditorAutoSizeOption.TextChanges };
```

自動サイズ変更を有効にすると、ユーザーがテキストを入力したときに[`Editor`](xref:Xamarin.Forms.Editor)の高さが増加し、ユーザーがテキストを削除すると高さが減少します。

> [!NOTE]
> [@No__t_3](xref:Xamarin.Forms.VisualElement.HeightRequest)プロパティが設定されている場合、 [`Editor`](xref:Xamarin.Forms.Editor)は自動サイズ変更されません。

### <a name="customizing-the-keyboard"></a>キーボードのカスタマイズ

ユーザーが[`Editor`](xref:Xamarin.Forms.Editor)と対話するときに表示されるキーボードは、 [`Keyboard`](xref:Xamarin.Forms.InputView.Keyboard)プロパティを使用して、 [`Keyboard`](xref:Xamarin.Forms.Keyboard)クラスの次のいずれかのプロパティにプログラムで設定できます。

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
<Editor Keyboard="Chat" />
```

これに相当する C# コードを次に示します。

```csharp
var editor = new Editor { Keyboard = Keyboard.Chat };
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
<Editor>
    <Editor.Keyboard>
        <Keyboard x:FactoryMethod="Create">
            <x:Arguments>
                <KeyboardFlags>Suggestions,CapitalizeCharacter</KeyboardFlags>
            </x:Arguments>
        </Keyboard>
    </Editor.Keyboard>
</Editor>
```

これに相当する C# コードを次に示します。

```csharp
var editor = new Editor();
editor.Keyboard = Keyboard.Create(KeyboardFlags.Suggestions | KeyboardFlags.CapitalizeCharacter);
```

### <a name="enabling-and-disabling-spell-checking"></a>スペルチェックを有効または無効にする

[@No__t_1](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled)プロパティは、スペルチェックを有効にするかどうかを制御します。 既定では、プロパティは `true` に設定されています。 ユーザーがテキストを入力すると、スペルミスが示されます。

ただし、一部のテキスト入力シナリオ (ユーザー名の入力など) では、スペルチェックで否定的なエクスペリエンスが提供されるため、 [`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled)プロパティを `false` に設定して無効にする必要があります。

```xaml
<Editor ... IsSpellCheckEnabled="false" />
```

```csharp
var editor = new Editor { ... IsSpellCheckEnabled = false };
```

> [!NOTE]
> [@No__t_1](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled)プロパティが `false` に設定されていて、カスタムキーボードが使用されていない場合、ネイティブスペルチェックは無効になります。 ただし、 [`Keyboard.Chat`](xref:Xamarin.Forms.Keyboard.Chat)などのスペルチェックを無効にする[`Keyboard`](xref:Xamarin.Forms.Keyboard)が設定されている場合、`IsSpellCheckEnabled` プロパティは無視されます。 このため、プロパティを使用して、明示的に無効にした `Keyboard` のスペルチェックを有効にすることはできません。

### <a name="enabling-and-disabling-text-prediction"></a>テキスト予測の有効化と無効化

@No__t_0 プロパティは、テキストの予測と自動テキスト修正が有効かどうかを制御します。 既定では、プロパティは `true` に設定されています。 ユーザーがテキストを入力すると、ワード予測が表示されます。

ただし、テキスト入力のシナリオによっては、ユーザー名の入力、テキスト予測、自動テキスト修正などがあります。そのため、`IsTextPredictionEnabled` プロパティを `false` に設定して無効にする必要があります。

```xaml
<Editor ... IsTextPredictionEnabled="false" />
```

```csharp
var editor = new Editor { ... IsTextPredictionEnabled = false };
```

> [!NOTE]
> @No__t_0 プロパティが `false` に設定されていて、カスタムキーボードが使用されていない場合、テキスト予測と自動テキスト修正は無効になります。 ただし、テキストの予測を無効にする[`Keyboard`](xref:Xamarin.Forms.Keyboard)が設定されている場合、`IsTextPredictionEnabled` プロパティは無視されます。 このため、プロパティを使用して、明示的に無効にする `Keyboard` のテキスト予測を有効にすることはできません。

### <a name="colors"></a>色

`Editor` は、`BackgroundColor` プロパティを使用してカスタムの背景色を使用するように設定できます。 各プラットフォームで色が使用できるようにするには、特別な注意が必要です。 各プラットフォームにはテキストの色について異なる既定値があるため、プラットフォームごとにカスタムの背景色を設定する必要がある場合があります。 各プラットフォームの UI の最適化の詳細については、「[プラットフォームの微](~/xamarin-forms/platform/device.md)調整の操作」を参照してください。

C# の場合:

```csharp
public partial class EditorPage : ContentPage
{
    public EditorPage ()
    {
        InitializeComponent ();
        var layout = new StackLayout { Padding = new Thickness(5,10) };
        this.Content = layout;
        //dark blue on UWP & Android, light blue on iOS
        var editor = new Editor { BackgroundColor = Device.RuntimePlatform == Device.iOS ? Color.FromHex("#A4EAFF") : Color.FromHex("#2c3e50") };
        layout.Children.Add(editor);
    }
}
```

XAML の場合:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    x:Class="TextSample.EditorPage"
    Title="Editor Demo">
    <ContentPage.Content>
        <StackLayout Padding="5,10">
            <Editor>
                <Editor.BackgroundColor>
                    <OnPlatform x:TypeArguments="x:Color">
                        <On Platform="iOS" Value="#a4eaff" />
                        <On Platform="Android, UWP" Value="#2c3e50" />
                    </OnPlatform>
                </Editor.BackgroundColor>
            </Editor>
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

![](editor-images/textbackgroundcolor.png "Editor with BackgroundColor Example")

選択した背景とテキストの色が各プラットフォームで使用できること、およびプレースホルダーのテキストが不明瞭になっていないことを確認します。

## <a name="interactivity"></a>対話機能

`Editor` は、次の2つのイベントを公開します。

- [TextChanged](xref:Xamarin.Forms.Editor.TextChanged) &ndash;、エディターのテキストが変更されたときに発生します。 変更前と変更後のテキストを提供します。
- [完了](xref:Xamarin.Forms.Editor.Completed)&ndash;、ユーザーがキーボードの return キーを押して入力を終了したときに発生します。

> [!NOTE]
> [@No__t_3](xref:Xamarin.Forms.Entry)を継承する[`VisualElement`](xref:Xamarin.Forms.VisualElement)クラスにも[`Focused`](xref:Xamarin.Forms.VisualElement.Focused)および[`Unfocused`](xref:Xamarin.Forms.VisualElement.Unfocused)イベントがあります。

### <a name="completed"></a>完了

@No__t_0 イベントは、`Editor` との対話の完了に応答するために使用されます。 `Completed` は、キーボードに戻りキーを入力することによって (または UWP の Tab キーを押して)、ユーザーがフィールドで入力を終了したときに発生します。 イベントのハンドラーは、送信者と `EventArgs` を取得する汎用イベントハンドラーです。

```csharp
void EditorCompleted (object sender, EventArgs e)
{
    var text = ((Editor)sender).Text; // sender is cast to an Editor to enable reading the `Text` property of the view.
}
```

完成したイベントは、コードと XAML でサブスクライブできます。

C# の場合:

```csharp
public partial class EditorPage : ContentPage
{
    public EditorPage ()
    {
        InitializeComponent ();
        var layout = new StackLayout { Padding = new Thickness(5,10) };
        this.Content = layout;
        var editor = new Editor ();
        editor.Completed += EditorCompleted;
        layout.Children.Add(editor);
    }
}
```

XAML の場合:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TextSample.EditorPage"
Title="Editor Demo">
    <ContentPage.Content>
        <StackLayout Padding="5,10">
            <Editor Completed="EditorCompleted" />
        </StackLayout>
    </ContentPage.Content>
</Contentpage>
```

### <a name="textchanged"></a>TextChanged

@No__t_0 イベントは、フィールドの内容の変更に対処するために使用されます。

`Editor` の `Text` が変更されるたびに `TextChanged` が発生します。 イベントのハンドラーは、`TextChangedEventArgs` のインスタンスを受け取ります。 `TextChangedEventArgs` は、`OldTextValue` および `NewTextValue` のプロパティを使用して、`Editor` `Text` の新旧の値へのアクセスを提供します。

```csharp
void EditorTextChanged (object sender, TextChangedEventArgs e)
{
    var oldText = e.OldTextValue;
    var newText = e.NewTextValue;
}
```

完成したイベントは、コードと XAML でサブスクライブできます。

コードは次のとおりです。

```csharp
public partial class EditorPage : ContentPage
{
    public EditorPage ()
    {
        InitializeComponent ();
        var layout = new StackLayout { Padding = new Thickness(5,10) };
        this.Content = layout;
        var editor = new Editor ();
        editor.TextChanged += EditorTextChanged;
        layout.Children.Add(editor);
    }
}
```

XAML の場合:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TextSample.EditorPage"
Title="Editor Demo">
    <ContentPage.Content>
        <StackLayout Padding="5,10">
            <Editor TextChanged="EditorTextChanged" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

## <a name="related-links"></a>関連リンク

- [Text (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
- [Editor API](xref:Xamarin.Forms.Editor)
