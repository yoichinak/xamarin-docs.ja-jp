---
title: エディター
description: 複数行テキスト入力
ms.prod: xamarin
ms.assetid: 7074DB3A-30D2-4A6B-9A89-B029EEF20B07
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: 035365a22c487039ff811756d91ca0a8d392d628
ms.sourcegitcommit: c024f29ff730ae20c15e99bfe0268a0e1c9d41e5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/23/2018
---
# <a name="editor"></a>エディター

_複数行テキスト入力_

`Editor`を複数行の入力を受け付けるようにコントロールを使用します。 この記事を取り上げます。

- **[カスタマイズ](#customization)** &ndash;キーボードと色のオプションです。
- **[対話機能](#interactivity)** &ndash;対話機能を提供する待機可能イベント。

## <a name="customization"></a>カスタマイズ

### <a name="setting-and-reading-text"></a>設定やテキストの読み取り

その他のテキストを表すビューなどのエディターを公開、`Text`プロパティです。 `Text` 設定し、によって提示されるテキストの読み取りに使用できる、`Editor`です。 次の例では、XAML で設定のテキストを示します。

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
