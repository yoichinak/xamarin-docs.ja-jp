---
title: Xamarin.Forms エディター
description: この記事では、アプリケーションで複数行テキスト入力を受け付ける Xamarin.Forms エディター コントロールを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 7074DB3A-30D2-4A6B-9A89-B029EEF20B07
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/31/2018
ms.openlocfilehash: 7ad8c8aa77e23c5a8fb7649736ecb271f835d1a7
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245525"
---
# <a name="xamarinforms-editor"></a>Xamarin.Forms エディター

_複数行テキスト入力_

`Editor`を複数行の入力を受け付けるようにコントロールを使用します。 この記事を取り上げます。

- **[カスタマイズ](#customization)** &ndash;キーボードと色のオプションです。
- **[対話機能](#interactivity)** &ndash;対話機能を提供する待機可能イベント。

## <a name="customization"></a>カスタマイズ

### <a name="setting-and-reading-text"></a>設定やテキストの読み取り

`Editor`、他のテキストを表すビューと同様に公開、`Text`プロパティです。 このプロパティは設定で表示されるテキストを参照して、使用することができます、`Editor`です。 次の例では、設定、 `Text` XAML でのプロパティ。

```xaml
<Editor Text="I am an Editor" />
```

C# の場合:

```csharp
var MyEditor = new Editor { Text = "I am an Editor" };
```

テキストを読み取り、アクセス、`Text`プロパティ (C#)。

```csharp
var text = MyEditor.Text;
```

### <a name="limiting-input-length"></a>入力の長さの制限

[ `MaxLength` ](xref:Xamarin.Forms.InputView.MaxLength)で使用できる入力文字列の長さを制限するプロパティを使用することができます、 [ `Editor`](xref:Xamarin.Forms.Editor)です。 このプロパティは、正の整数に設定する必要があります。

```xaml
<Editor ... MaxLength="10" />
```

```csharp
var editor = new Editor { ... MaxLength = 10 };
```

A [ `MaxLength` ](xref:Xamarin.Forms.InputView.MaxLength)を入力することはできません、0 のプロパティの値を示します、値を`int.MaxValue`の既定値は、 [ `Editor` ](xref:Xamarin.Forms.Editor)、あることを示してありません入力できる文字数の有効な制限です。

### <a name="keyboards"></a>キーボード

ユーザーが対話するときに表示されるキーボード、`Editor`経由でプログラムで設定することができます、 [ ``Keyboard`` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Keyboard/)プロパティです。

キーボードの種類のオプションは次のとおりです。

- **既定**&ndash;既定のキーボード
- **チャット**&ndash;のためのテキストとプレース emoji は役立ちます
- **電子メール**&ndash;電子メール アドレスを入力するときに使用
- **数値**&ndash;番号を入力するときに使用
- **電話**&ndash;電話番号を入力するときに使用
- **Url** &ndash; web アドレス (&)、ファイルのパスを入力するために使用

[各キーボードの使用例](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/choose-keyboard-for-entry/)のレシピのセクションでします。

### <a name="enabling-and-disabling-spell-checking"></a>有効にして、スペル チェックを無効にします。

[ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled)プロパティ コントロールかどうかスペル チェックを有効にします。 既定では、プロパティに設定が`true`です。 テキストを入力すると、スペル ミスが示されます。

ただし、ユーザー名を入力するなど、一部のテキスト エントリ シナリオ スペル チェックは、負経験しているため無効に設定して、 [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled)プロパティを`false`:

```xaml
<Editor ... IsSpellCheckEnabled="false" />
```

```csharp
var editor = new Editor { ... IsSpellCheckEnabled = false };
```

> [!NOTE]
> ときに、 [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled)プロパティに設定されている`false`、カスタム キーボードが使用されていないと、ネイティブのスペル チェック機能は無効になります。 ただし場合、 [ `Keyboard` ](xref:Xamarin.Forms.Keyboard)がされてスペルを無効にする設定の確認など[ `Keyboard.Chat` ](xref:Xamarin.Forms.Keyboard.Chat)、`IsSpellCheckEnabled`プロパティは無視されます。 そのため、スペル チェックを有効にするプロパティを使用できません、`Keyboard`を明示的に無効にします。

### <a name="colors"></a>色

`Editor` 使用してカスタムの背景色を使用する設定することができます、`BackgroundColor`プロパティです。 特別な注意は、色を各プラットフォームで使用可能になることを確認する必要があります。 各プラットフォームには、テキストの色の別の既定値があるために、プラットフォームごとにカスタムの背景色を設定する必要があります。 参照してください[プラットフォームので使用](~/xamarin-forms/platform/device.md)詳細については、各プラットフォーム用の UI を最適化します。

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

XAML:

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

![](editor-images/textbackgroundcolor.png "BackgroundColor 例エディター")

確認して、背景色とテキスト色を選択する各プラットフォームで使用可能な任意のプレース ホルダー テキストが不明瞭ください。

## <a name="interactivity"></a>対話機能

`Editor` 2 つのイベントを公開します。

- [TextChanged](http://developer.xamarin.com/api/event/Xamarin.Forms.Editor.TextChanged/) &ndash;エディターでテキストが変更されたときに発生します。 前に、と変更後のテキストを提供します。
- [完了](http://developer.xamarin.com/api/event/Xamarin.Forms.Editor.Completed/)&ndash;ユーザーには、キーボードの戻り値のキーを押して、入力が終了したときに発生します。

### <a name="completed"></a>完了

`Completed`イベントとのやり取りの完了に対応するために使用する`Editor`です。 `Completed` キーボードの戻り値のキーを入力して、ユーザーがフィールドに入力を終了するときに発生します。 イベントのハンドラーは、ジェネリック イベント ハンドラーは、送信者を取得し、 `EventArgs`:

```csharp
void EditorCompleted (object sender, EventArgs e)
{
    var text = ((Editor)sender).Text; // sender is cast to an Editor to enable reading the `Text` property of the view.
}
```

完了のイベントは、コードと XAML をサブスクライブすることができます。

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

XAML:

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

`TextChanged` 発生するたびに、`Text`の`Editor`変更します。 インスタンスを受け取り、イベントのハンドラー`TextChangedEventArgs`です。 `TextChangedEventArgs` 新旧の値にアクセスできるように、 `Editor` `Text`を介して、`OldTextValue`と`NewTextValue`プロパティ。

```csharp
void EditorTextChanged (object sender, TextChangedEventArgs e)
{
    var oldText = e.OldTextValue;
    var newText = e.NewTextValue;
}
```

完了のイベントは、コードと XAML をサブスクライブすることができます。

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

XAML:

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
- [エディター API](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/)
