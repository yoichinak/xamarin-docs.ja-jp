---
title: Xamarin.Forms のエディター
description: この記事では、Xamarin.Forms エディター コントロールを使用して、アプリケーションで複数行テキスト入力をそのまま使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 7074DB3A-30D2-4A6B-9A89-B029EEF20B07
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/31/2018
ms.openlocfilehash: 4879ff88d5bbdab5aa92024bee7f50239a141e3b
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995867"
---
# <a name="xamarinforms-editor"></a>Xamarin.Forms のエディター

_複数行テキスト入力_

`Editor`コントロールを使用すると、複数行の入力をそのまま使用します。 この記事では説明します。

- **[カスタマイズ](#customization)** &ndash;キーボードと色のオプション。
- **[対話機能](#interactivity)** &ndash;対話機能を提供する待機可能イベント。

## <a name="customization"></a>カスタマイズ

### <a name="setting-and-reading-text"></a>設定やテキストの読み取り

`Editor`などの他のテキストを表すビューを公開、`Text`プロパティ。 このプロパティは、設定し、によって提示されるテキストの読み取りに使用できます、`Editor`します。 次の例では、設定、 `Text` XAML のプロパティ。

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

### <a name="limiting-input-length"></a>入力の長さの制限

[ `MaxLength` ](xref:Xamarin.Forms.InputView.MaxLength)で使用できるように入力文字列の長さを制限するプロパティを使用できます、 [ `Editor`](xref:Xamarin.Forms.Editor)します。 このプロパティは、正の整数に設定する必要があります。

```xaml
<Editor ... MaxLength="10" />
```

```csharp
var editor = new Editor { ... MaxLength = 10 };
```

A [ `MaxLength` ](xref:Xamarin.Forms.InputView.MaxLength)プロパティ値が 0 のでは、ある入力は許可されません、ことを示しますの値と`int.MaxValue`の既定値は、 [ `Editor` ](xref:Xamarin.Forms.Editor)、ことを示しますなし有効な入力可能性がありますの文字数制限。

### <a name="keyboards"></a>キーボード

ユーザーが対話する際に表示されるキーボード、`Editor`経由でプログラムによって設定できる、 [ ``Keyboard`` ](xref:Xamarin.Forms.Keyboard)プロパティ。

キーボードの種類のオプションがあります。

- **既定**&ndash;既定のキーボード
- **チャット**&ndash;のためのテキストとプレース絵文字が便利です
- **電子メール**&ndash;電子メール アドレスを入力するときに使用
- **数値**&ndash;数値を入力するときに使用
- **電話**&ndash;電話番号を入力するときに使用
- **Url** &ndash; web アドレス (&)、ファイルのパスを入力するために使用

[各キーボードの使用例](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/choose-keyboard-for-entry/)レシピ セクションでします。

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

### <a name="completed"></a>完了

`Completed`とのやり取りの完了に反応するイベントを使用する`Editor`します。 `Completed` キーボードの戻り値のキーを入力して、ユーザーが終了する入力フィールドを持つ場合に発生します。 イベントのハンドラーは、送信者を取得、汎用イベント ハンドラーと`EventArgs`:

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
