---
title: Xamarin.Forms エディター
description: この記事では、エディターコントロールを使用して、 Xamarin.Forms アプリケーションで複数行テキスト入力を許可する方法について説明します。
ms.prod: xamarin
ms.assetid: 7074DB3A-30D2-4A6B-9A89-B029EEF20B07
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/26/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 02749c9f8f55427bb1742e78464bbc003f1f7358
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84136176"
---
# <a name="xamarinforms-editor"></a>Xamarin.Forms エディター

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)

_複数行テキスト入力_

コントロールは、 [`Editor`](xref:Xamarin.Forms.Editor) 複数行の入力を受け入れるために使用されます。 この記事には、次の内容が含まれます。

- **[カスタマイズ](#customization)** &ndash;キーボードと色のオプション。
- **[対話機能](#interactivity)** &ndash;対話機能を提供するためにをリッスンできるイベント。

## <a name="customization"></a>カスタマイズ

### <a name="setting-and-reading-text"></a>設定とテキストの読み取り

は、 [`Editor`](xref:Xamarin.Forms.Editor) 他のテキスト表示ビューと同様に、プロパティを公開し `Text` ます。 このプロパティを使用して、によって表示されるテキストを設定および読み取ることができ `Editor` ます。 次の例は、XAML でプロパティを設定する方法を示してい `Text` ます。

```xaml
<Editor Text="I am an Editor" />
```

C# の場合:

```csharp
var MyEditor = new Editor { Text = "I am an Editor" };
```

テキストを読み取るには、 `Text` C# のプロパティにアクセスします。

```csharp
var text = MyEditor.Text;
```

### <a name="setting-placeholder-text"></a>プレースホルダーテキストの設定

は、 [`Editor`](xref:Xamarin.Forms.Editor) ユーザー入力を格納していない場合にプレースホルダーテキストを表示するように設定できます。 これは、プロパティをに設定することによって実現され、多くの場合 [`Placeholder`](xref:Xamarin.Forms.InputView.Placeholder) `string` 、に適したコンテンツの種類を示すために使用され `Editor` ます。 また、プロパティをに設定することによって、プレースホルダーテキストの色を制御できます [`PlaceholderColor`](xref:Xamarin.Forms.InputView.PlaceholderColor) [`Color`](xref:Xamarin.Forms.Color) 。

```xaml
<Editor Placeholder="Enter text here" PlaceholderColor="Olive" />
```

```csharp
var editor = new Editor { Placeholder = "Enter text here", PlaceholderColor = Color.Olive };
```

### <a name="preventing-text-entry"></a>テキスト入力の防止

[`Editor`](xref:Xamarin.Forms.Editor) `IsReadOnly` の既定値であるプロパティを次のように設定することによって、のテキストを変更できないようにすることができます `false` `true` 。

```xaml
<Editor Text="This is a read-only Editor"
        IsReadOnly="true" />
```

```csharp
var editor = new Editor { Text = "This is a read-only Editor", IsReadOnly = true });
```

> [!NOTE]
> プロパティは、の `IsReadonly` [`Editor`](xref:Xamarin.Forms.Editor) 視覚的な外観を `IsEnabled` 灰色に変更するプロパティとは異なり、の外観を変更しません `Editor` 。

### <a name="limiting-input-length"></a>制限 (入力の長さを)

プロパティは、で許可されて [`MaxLength`](xref:Xamarin.Forms.InputView.MaxLength) いる入力の長さを制限するために使用でき [`Editor`](xref:Xamarin.Forms.Editor) ます。 このプロパティは、正の整数に設定する必要があります。

```xaml
<Editor ... MaxLength="10" />
```

```csharp
var editor = new Editor { ... MaxLength = 10 };
```

[`MaxLength`](xref:Xamarin.Forms.InputView.MaxLength)プロパティ値が0の場合、入力が許可されないことを示します。の値 (の既定値) は、 `int.MaxValue` [`Editor`](xref:Xamarin.Forms.Editor) 入力可能な文字数に有効な制限がないことを示します。

### <a name="character-spacing"></a>文字間隔

[`Editor`](xref:Xamarin.Forms.Editor)プロパティを値に設定することによって、文字間隔をに適用でき `Editor.CharacterSpacing` `double` ます。

```xaml
<Editor ...
        CharacterSpacing="10" />
```

これに相当する C# コードを次に示します。

```csharp
Editor editor = new editor { CharacterSpacing = 10 };
```

結果として、によって表示されるテキスト内の文字 [`Editor`](xref:Xamarin.Forms.Editor) は、 `CharacterSpacing` デバイスに依存しない単位になります。

> [!NOTE]
> `CharacterSpacing`プロパティ値は、プロパティおよびプロパティによって表示されるテキストに適用され `Text` `Placeholder` ます。

### <a name="auto-sizing-an-editor"></a>エディターの自動サイズ変更

は、 [`Editor`](xref:Xamarin.Forms.Editor) [`Editor.AutoSize`](xref:Xamarin.Forms.Editor.AutoSize) プロパティを [`TextChanges`](xref:Xamarin.Forms.EditorAutoSizeOption.TextChanges) 列挙体の値であるに設定することにより、コンテンツへの自動サイズ変更を行うことができます [`EditoAutoSizeOption`](xref:Xamarin.Forms.EditorAutoSizeOption) 。 この列挙体には、次の2つの値があります。

- [`Disabled`](xref:Xamarin.Forms.EditorAutoSizeOption.Disabled)自動サイズ変更が無効になっていることを示します。これは既定値です。
- [`TextChanges`](xref:Xamarin.Forms.EditorAutoSizeOption.TextChanges)自動サイズ変更が有効になっていることを示します。

これは、次のようにコードで行うことができます。

```xaml
<Editor Text="Enter text here" AutoSize="TextChanges" />
```

```csharp
var editor = new Editor { Text = "Enter text here", AutoSize = EditorAutoSizeOption.TextChanges };
```

自動サイズ変更を有効にすると、ユーザーがテキストを入力したときにの高さが増加し、 [`Editor`](xref:Xamarin.Forms.Editor) ユーザーがテキストを削除すると高さが減少します。

> [!NOTE]
> [`Editor`](xref:Xamarin.Forms.Editor) [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) プロパティが設定されている場合、は自動的にサイズ変更されません。

### <a name="customizing-the-keyboard"></a>キーボードのカスタマイズ

ユーザーがを操作するときに表示されるキーボードは、プロパティを使用して、 [`Editor`](xref:Xamarin.Forms.Editor) [`Keyboard`](xref:Xamarin.Forms.InputView.Keyboard) クラスの次のいずれかのプロパティにプログラムで設定でき [`Keyboard`](xref:Xamarin.Forms.Keyboard) ます。

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

プロパティは、 [`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) スペルチェックを有効にするかどうかを制御します。 既定では、プロパティはに設定されて `true` います。 ユーザーがテキストを入力すると、スペルミスが示されます。

ただし、一部のテキスト入力シナリオ (ユーザー名の入力など) では、スペルチェックで否定的なエクスペリエンスが提供されるため、プロパティをに設定することによって無効にする必要があり [`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) `false` ます。

```xaml
<Editor ... IsSpellCheckEnabled="false" />
```

```csharp
var editor = new Editor { ... IsSpellCheckEnabled = false };
```

> [!NOTE]
> [`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled)プロパティがに設定され `false` ていて、カスタムキーボードが使用されていない場合、ネイティブスペルチェックは無効になります。 ただし、など、 [`Keyboard`](xref:Xamarin.Forms.Keyboard) スペルチェックを無効にするが設定されている場合、 [`Keyboard.Chat`](xref:Xamarin.Forms.Keyboard.Chat) `IsSpellCheckEnabled` プロパティは無視されます。 このため、プロパティを使用して、明示的に無効にするのスペルチェックを有効にすることはできません `Keyboard` 。

### <a name="enabling-and-disabling-text-prediction"></a>テキスト予測の有効化と無効化

プロパティは、 `IsTextPredictionEnabled` テキストの予測と自動テキスト修正が有効かどうかを制御します。 既定では、プロパティはに設定されて `true` います。 ユーザーがテキストを入力すると、ワード予測が表示されます。

ただし、テキスト入力のシナリオによっては、ユーザー名の入力、テキスト予測、自動テキスト修正などがありますが、プロパティを次のように設定することで無効にする必要があり `IsTextPredictionEnabled` `false` ます。

```xaml
<Editor ... IsTextPredictionEnabled="false" />
```

```csharp
var editor = new Editor { ... IsTextPredictionEnabled = false };
```

> [!NOTE]
> `IsTextPredictionEnabled`プロパティがに設定され `false` ていて、カスタムキーボードが使用されていない場合、テキスト予測と自動テキスト修正は無効になります。 ただし、 [`Keyboard`](xref:Xamarin.Forms.Keyboard) テキスト予測を無効にするが設定されている場合、 `IsTextPredictionEnabled` プロパティは無視されます。 このため、プロパティを使用して、明示的に無効にするのテキスト予測を有効にすることはできません `Keyboard` 。

### <a name="colors"></a>色

`Editor`プロパティを使用してカスタムの背景色を使用するように設定でき `BackgroundColor` ます。 各プラットフォームで色が使用できるようにするには、特別な注意が必要です。 各プラットフォームにはテキストの色について異なる既定値があるため、プラットフォームごとにカスタムの背景色を設定する必要がある場合があります。 各プラットフォームの UI の最適化の詳細については、「[プラットフォームの微](~/xamarin-forms/platform/device.md)調整の操作」を参照してください。

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

`Editor`2つのイベントを公開します。

- [TextChanged](xref:Xamarin.Forms.InputView.TextChanged) &ndash;エディターのテキストが変更されたときに発生します。 変更前と変更後のテキストを提供します。
- [完了](xref:Xamarin.Forms.Editor.Completed) &ndash;ユーザーがキーボードの return キーを押して入力を終了したときに発生します。

> [!NOTE]
> [`VisualElement`](xref:Xamarin.Forms.VisualElement)継承元のクラスは、 [`Entry`](xref:Xamarin.Forms.Entry) とのイベントも持ってい [`Focused`](xref:Xamarin.Forms.VisualElement.Focused) [`Unfocused`](xref:Xamarin.Forms.VisualElement.Unfocused) ます。

### <a name="completed"></a>完了

`Completed`イベントは、との相互作用の完了に応答するために使用され `Editor` ます。 `Completed`は、ユーザーがキーボードに戻りキーを入力するか、UWP の Tab キーを押すことによって、入力をフィールドで終了したときに発生します。 イベントのハンドラーは、送信者とを取得する汎用イベントハンドラーです `EventArgs` 。

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

`TextChanged`イベントは、フィールドの内容の変更に対処するために使用されます。

`TextChanged`のが変更されるたびに、が発生し `Text` `Editor` ます。 イベントのハンドラーは、のインスタンスを受け取り `TextChangedEventArgs` ます。 `TextChangedEventArgs`プロパティとプロパティを使用して、の古い値と新しい値へのアクセスを提供し `Editor` `Text` `OldTextValue` `NewTextValue` ます。

```csharp
void EditorTextChanged (object sender, TextChangedEventArgs e)
{
    var oldText = e.OldTextValue;
    var newText = e.NewTextValue;
}
```

完成したイベントは、コードと XAML でサブスクライブできます。

コード内で以下のように指定します。

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
