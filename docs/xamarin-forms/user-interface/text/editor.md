---
title: Xamarin.Forms のエディター
description: この記事では、Xamarin.Forms エディター コントロールを使用して、アプリケーションで複数行テキスト入力をそのまま使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 7074DB3A-30D2-4A6B-9A89-B029EEF20B07
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/26/2018
ms.openlocfilehash: 97bb5ec954f36e48d8ae115baf8738862e5a8358
ms.sourcegitcommit: c1d85b2c62ad84c22bdee37874ad30128581bca6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/08/2019
ms.locfileid: "67649547"
---
# <a name="xamarinforms-editor"></a>Xamarin.Forms のエディター

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)

_複数行テキスト入力_

[ `Editor` ](xref:Xamarin.Forms.Editor)コントロールを使用すると、複数行の入力をそのまま使用します。 この記事では次の内容について説明します。

- **[カスタマイズ](#customization)** &ndash;キーボードと色のオプション。
- **[対話機能](#interactivity)** &ndash;対話機能を提供する待機可能イベント。

## <a name="customization"></a>カスタマイズ

### <a name="setting-and-reading-text"></a>設定やテキストの読み取り

[ `Editor`](xref:Xamarin.Forms.Editor)などの他のテキストを表すビューを公開、`Text`プロパティ。 このプロパティは、設定し、によって提示されるテキストの読み取りに使用できます、`Editor`します。 次の例では、設定、 `Text` XAML のプロパティ。

```xaml
<Editor Text="I am an Editor" />
```

C# の場合:

```csharp
var MyEditor = new Editor { Text = "I am an Editor" };
```

テキストを読み取るには、アクセス、`Text`プロパティ (C#)。

```csharp
var text = MyEditor.Text;
```

### <a name="setting-placeholder-text"></a>プレース ホルダー テキストを設定

[ `Editor` ](xref:Xamarin.Forms.Editor)プレース ホルダー テキストを表示するユーザー入力を格納するがない場合に設定することができます。 これは、設定によって実現されます、 [ `Placeholder` ](xref:Xamarin.Forms.Editor.Placeholder)プロパティを`string`は適切なコンテンツの種類を示すためによく使用して、 `Editor`。 さらに、プレース ホルダー テキストの色を設定して制御できます、 [ `PlaceholderColor` ](xref:Xamarin.Forms.Editor.PlaceholderColor)プロパティを[ `Color` ](xref:Xamarin.Forms.Color):

```xaml
<Editor Placeholder="Enter text here" PlaceholderColor="Olive" />
```

```csharp
var editor = new Editor { Placeholder = "Enter text here", PlaceholderColor = Color.Olive };
```

### <a name="preventing-text-entry"></a>テキスト入力の防止

内のテキストを変更することを防止できるユーザー、 [ `Editor` ](xref:Xamarin.Forms.Editor)を設定して、`IsReadOnly`プロパティで、既定値を持つの`false`を`true`:

```xaml
<Editor Text="This is a read-only Editor"
        IsReadOnly="true" />
```

```csharp
var editor = new Editor { Text = "This is a read-only Editor", IsReadOnly = true });
```

> [!NOTE]
> `IsReadonly`プロパティの視覚的な外観は変更しません、 [ `Editor`](xref:Xamarin.Forms.Editor)とは異なり、`IsEnabled`もの視覚的な外観を変更するプロパティ、`Editor`灰色。

### <a name="limiting-input-length"></a>入力の長さの制限

[ `MaxLength` ](xref:Xamarin.Forms.InputView.MaxLength)で使用できるように入力文字列の長さを制限するプロパティを使用できます、 [ `Editor`](xref:Xamarin.Forms.Editor)します。 このプロパティは、正の整数に設定する必要があります。

```xaml
<Editor ... MaxLength="10" />
```

```csharp
var editor = new Editor { ... MaxLength = 10 };
```

A [ `MaxLength` ](xref:Xamarin.Forms.InputView.MaxLength)プロパティ値が 0 のでは、ある入力は許可されません、ことを示しますの値と`int.MaxValue`の既定値は、 [ `Editor` ](xref:Xamarin.Forms.Editor)、ことを示しますなし有効な入力可能性がありますの文字数制限。

### <a name="auto-sizing-an-editor"></a>エディターを自動サイズ調整

[ `Editor` ](xref:Xamarin.Forms.Editor)を設定してそのコンテンツへの自動-サイズに行んだことができます、 [ `Editor.AutoSize` ](xref:Xamarin.Forms.Editor.AutoSize)プロパティを[ `TextChanges` ](xref:Xamarin.Forms.EditorAutoSizeOption.TextChanges)、これは、値、の[ `EditoAutoSizeOption` ](xref:Xamarin.Forms.EditorAutoSizeOption)列挙体。 この列挙体では、2 つの値があります。

- [`Disabled`](xref:Xamarin.Forms.EditorAutoSizeOption.Disabled) 自動サイズ変更を無効にし、既定値を示します。
- [`TextChanges`](xref:Xamarin.Forms.EditorAutoSizeOption.TextChanges) 自動サイズ変更が有効になっていることを示します。

これは、ことができますように実行コードで。

```xaml
<Editor Text="Enter text here" AutoSize="TextChanges" />
```

```csharp
var editor = new Editor { Text = "Enter text here", AutoSize = EditorAutoSizeOption.TextChanges };
```

自動サイズ変更を有効にするの高さ、 [ `Editor` ](xref:Xamarin.Forms.Editor)テキストで、ユーザーの入力し、ユーザーは、テキストを削除します。 高さは減らすが増加します。

> [!NOTE]
> [ `Editor` ](xref:Xamarin.Forms.Editor)は場合に自動-サイズではなく、 [ `HeightRequest` ](xref:Xamarin.Forms.VisualElement.HeightRequest)プロパティが設定されています。

### <a name="customizing-the-keyboard"></a>キーボードのカスタマイズ

ユーザーが対話する際に表示されるキーボード、 [ `Editor` ](xref:Xamarin.Forms.Editor)経由でプログラムによって設定できる、 [ `Keyboard` ](xref:Xamarin.Forms.InputView.Keyboard) から次のプロパティのいずれかのプロパティ[ `Keyboard` ](xref:Xamarin.Forms.Keyboard)クラス。

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
<Editor Keyboard="Chat" />
```

同等の C# コードに示します。

```csharp
var editor = new Editor { Keyboard = Keyboard.Chat };
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

同等の C# コードに示します。

```csharp
var editor = new Editor();
editor.Keyboard = Keyboard.Create(KeyboardFlags.Suggestions | KeyboardFlags.CapitalizeCharacter);
```

### <a name="enabling-and-disabling-spell-checking"></a>有効にして、スペル チェックを無効化

[ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled)プロパティ コントロールかどうかスペル チェックを有効にします。 既定では、プロパティに設定が`true`します。 テキストを入力すると、スペル ミスが示されます。

ただし、ユーザー名を入力するなど、いくつかのテキスト エントリ シナリオのスペル チェックを提供、負のエクスペリエンスとこれを設定して無効にする必要があります、 [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled)プロパティを`false`:

```xaml
<Editor ... IsSpellCheckEnabled="false" />
```

```csharp
var editor = new Editor { ... IsSpellCheckEnabled = false };
```

> [!NOTE]
> ときに、 [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled)プロパティに設定されて`false`、およびカスタムのキーボードが使用されていない、ネイティブのスペル チェックが無効になります。 ただし場合、 [ `Keyboard` ](xref:Xamarin.Forms.Keyboard)がされているセットをスペル チェックを無効にするチェックを行うなど[ `Keyboard.Chat` ](xref:Xamarin.Forms.Keyboard.Chat)、`IsSpellCheckEnabled`プロパティは無視されます。 そのため、スペル チェックを有効にするプロパティを使用できません、`Keyboard`を明示的に無効にします。

### <a name="enabling-and-disabling-text-prediction"></a>有効にして、予測入力を無効化

`IsTextPredictionEnabled`プロパティ コントロールかどうか予測入力と自動テキストの修正を有効にします。 既定では、プロパティに設定が`true`します。 テキストを入力すると、word の予測が表示されます。

ただし、テキスト エントリ シナリオによっては、ユーザー名、予測入力とテキストの自動入力など修正負のエクスペリエンスを提供しますを設定して無効にする必要があります、`IsTextPredictionEnabled`プロパティを`false`:。

```xaml
<Editor ... IsTextPredictionEnabled="false" />
```

```csharp
var editor = new Editor { ... IsTextPredictionEnabled = false };
```

> [!NOTE]
> ときに、`IsTextPredictionEnabled`プロパティに設定されて`false`、カスタムのキーボードがされていないと予測入力と自動的に使用されるテキストの修正が無効になっています。 ただし場合、 [ `Keyboard` ](xref:Xamarin.Forms.Keyboard)を無効にします。 テキストの予測が設定されている、`IsTextPredictionEnabled`プロパティは無視されます。 そのための予測の入力を有効にするプロパティを使用できません、`Keyboard`を明示的に無効にします。

### <a name="colors"></a>色

`Editor` 使用してカスタムの背景色を使用して、設定することができます、`BackgroundColor`プロパティ。 特別な注意は、色は各プラットフォームで使用できることを確認する必要があります。 各プラットフォームには、テキストの色の異なる既定値があるために、各プラットフォーム用のカスタムの背景色を設定する必要があります。 参照してください[プラットフォームな調整の操作](~/xamarin-forms/platform/device.md)詳細については、各プラットフォームの UI を最適化します。

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

で XAML:

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

![](editor-images/textbackgroundcolor.png "エディターの BackgroundColor 例")

選択した色の背景とテキストの色は、各プラットフォームで使用し、プレース ホルダー テキストが不明瞭してください。

## <a name="interactivity"></a>対話機能

`Editor` 2 つのイベントを公開します。

- [TextChanged](xref:Xamarin.Forms.Editor.TextChanged) &ndash;エディターでテキストが変更されたときに発生します。 変更の前後にテキストを提供します。
- [完了した](xref:Xamarin.Forms.Editor.Completed)&ndash;ユーザーには、キーボードの戻り値のキーを押して、入力が終了したときに発生します。

> [!NOTE]
> [ `VisualElement` ](xref:Xamarin.Forms.VisualElement)元のクラス[ `Entry` ](xref:Xamarin.Forms.Entry)継承もが[ `Focused` ](xref:Xamarin.Forms.VisualElement.Focused)と[ `Unfocused` ](xref:Xamarin.Forms.VisualElement.Unfocused)イベント。

### <a name="completed"></a>完了

`Completed`とのやり取りの完了に反応するイベントを使用する`Editor`します。 `Completed` キーボードの戻り値のキーを入力して (または UWP の Tab キーを押して)、ユーザーがフィールドに入力を終了したときに発生します。 イベントのハンドラーは、送信者を取得、汎用イベント ハンドラーと`EventArgs`:

```csharp
void EditorCompleted (object sender, EventArgs e)
{
    var text = ((Editor)sender).Text; // sender is cast to an Editor to enable reading the `Text` property of the view.
}
```

完了イベントは、コードと XAML でサブスクライブできます。

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

で XAML:

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

`TextChanged`イベントは、フィールドの内容の変更に対応するために使用します。

`TextChanged` ときに発生しますが、`Text`の`Editor`変更します。 インスタンスを受け取り、イベントのハンドラーは`TextChangedEventArgs`します。 `TextChangedEventArgs` 新旧の値にアクセスできるように、 `Editor` `Text`を使用して、`OldTextValue`と`NewTextValue`プロパティ。

```csharp
void EditorTextChanged (object sender, TextChangedEventArgs e)
{
    var oldText = e.OldTextValue;
    var newText = e.NewTextValue;
}
```

完了イベントは、コードと XAML でサブスクライブできます。

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

で XAML:

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

- [テキスト (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [エディターの API](xref:Xamarin.Forms.Editor)
