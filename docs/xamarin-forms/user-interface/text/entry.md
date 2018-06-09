---
title: Xamarin.Forms エントリ
description: この記事では、Xamarin.Forms エントリ クラスを使用して、単一行のテキストまたはアプリケーションでパスワードの入力をそのまま使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 9923C541-3C10-4D14-BAB5-C4D6C514FB1E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/31/2018
ms.openlocfilehash: b6188b986589a56229ad2e092d4100ff3f75dbe4
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245554"
---
# <a name="xamarinforms-entry"></a>Xamarin.Forms エントリ

_単一行のテキストまたはパスワードの入力_

Xamarin.Forms`Entry`単一行のテキストの入力として使用されます。 `Entry`同様に、`Editor`ビューで、複数のキーボードの種類をサポートしています。 さらに、`Entry`パスワード フィールドとして使用できます。

## <a name="display-customization"></a>表示のカスタマイズ

### <a name="setting-and-reading-text"></a>設定やテキストの読み取り

`Entry`、他のテキストを表すビューと同様に公開、`Text`プロパティです。 このプロパティは設定で表示されるテキストを参照して、使用することができます、`Entry`です。 次の例では、設定、 `Text` XAML でのプロパティ。

```xaml
<Entry Text="I am an Entry" />
```

C# の場合:

```csharp
var MyEntry = new Entry { Text = "I am an Entry" };
```

テキストを読み取り、アクセス、`Text`プロパティ (C#)。

```csharp
var text = MyEntry.Text;
```

> [!NOTE]
> 幅、`Entry`設定で定義されていることができます、`WidthRequest`プロパティです。 幅に依存しない、`Entry`の値に基づいて定義されているその`Text`プロパティです。

### <a name="limiting-input-length"></a>入力の長さの制限

[ `MaxLength` ](xref:Xamarin.Forms.InputView.MaxLength)で使用できる入力文字列の長さを制限するプロパティを使用することができます、 [ `Entry`](xref:Xamarin.Forms.Entry)です。 このプロパティは、正の整数に設定する必要があります。

```xaml
<Entry ... MaxLength="10" />
```

```csharp
var entry = new Entry { ... MaxLength = 10 };
```

A [ `MaxLength` ](xref:Xamarin.Forms.InputView.MaxLength)を入力することはできません、0 のプロパティの値を示します、値を`int.MaxValue`の既定値は、 [ `Entry` ](xref:Xamarin.Forms.Entry)、あることを示してありません入力できる文字数の有効な制限です。

### <a name="keyboards"></a>キーボード

ユーザーが対話するときに表示されるキーボード、`Entry`経由でプログラムで設定することができます、`Keyboard`プロパティです。

キーボードの種類のオプションは次のとおりです。

- **既定**&ndash;既定のキーボード
- **チャット**&ndash;のためのテキストとプレース emoji は役立ちます
- **電子メール**&ndash;電子メール アドレスを入力するときに使用
- **数値**&ndash;番号を入力するときに使用
- **電話**&ndash;電話番号を入力するときに使用
- **Url** &ndash;ファイルのパスと web アドレスを入力するために使用

[各キーボードの使用例](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/choose-keyboard-for-entry/)のレシピのセクションでします。

### <a name="enabling-and-disabling-spell-checking"></a>有効にして、スペル チェックを無効にします。

[ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled)プロパティ コントロールかどうかスペル チェックを有効にします。 既定では、プロパティに設定が`true`です。 テキストを入力すると、スペル ミスが示されます。

ただし、ユーザー名を入力するなど、一部のテキスト エントリ シナリオ スペル チェックは、負経験しているため無効に設定して、 [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled)プロパティを`false`:

```xaml
<Entry ... IsSpellCheckEnabled="false" />
```

```csharp
var entry = new Entry { ... IsSpellCheckEnabled = false };
```

> [!NOTE]
> ときに、 [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled)プロパティに設定されている`false`、カスタム キーボードが使用されていないと、ネイティブのスペル チェック機能は無効になります。 ただし場合、 [ `Keyboard` ](xref:Xamarin.Forms.Keyboard)がされてスペルを無効にする設定の確認など[ `Keyboard.Chat` ](xref:Xamarin.Forms.Keyboard.Chat)、`IsSpellCheckEnabled`プロパティは無視されます。 そのため、スペル チェックを有効にするプロパティを使用できません、`Keyboard`を明示的に無効にします。

### <a name="placeholders"></a>プレース ホルダー

`Entry` ユーザー入力を格納するがない場合は、プレース ホルダー テキストを表示するのには設定できます。 実際には、このよく見られます。 フォームを指定したフィールドを適切なコンテンツを明確にします。 プレース ホルダー テキストの色はカスタマイズできず、、同じであるかに関係なく、`TextColor`設定します。 カスタム プレース ホルダーの色を呼び出すと、デザイン場合に、フォールバックする必要があります、[カスタム レンダラー]()です。 次を作成、 `Entry` "Username"XAML 内のプレース ホルダーとして使用します。

```xaml
<Entry Placeholder="Username" />
```

C# の場合:

```csharp
var MyEntry = new Entry { Placeholder = "Username" };
```

![](entry-images/placeholder.png "エントリのプレース ホルダーの例")

### <a name="password-fields"></a>パスワード フィールド

`Entry` 提供、`IsPassword`プロパティです。 ときに`IsPassword`は`true`フィールドの内容は、黒の円として表示されます。

XAML:

```xaml
<Entry IsPassword="true" />
```

C# の場合:

```csharp
var MyEntry = new Entry { IsPassword = true };
```

![](entry-images/password.png "エントリ IsPassword 例")

インスタンスで使用できるプレース ホルダー`Entry`パスワード フィールドとして構成されています。

XAML:

```xaml
<Entry IsPassword="true" Placeholder="Password" />
```

C# の場合:

```csharp
var MyEntry = new Entry { IsPassword = true, Placeholder = "Password" };
```

![](entry-images/passwordplaceholder.png "エントリ IsPassword とプレース ホルダーの例")


### <a name="colors"></a>色

エントリを設定して、カスタムの背景と、次のバインド可能なプロパティを使用してテキストの色を使用できます。

- **TextColor** &ndash;テキストの色を設定します。
- **BackgroundColor** &ndash;表示テキストの背景色を設定します。

特別な注意は、色を各プラットフォームで使用可能になることを確認する必要があります。 各プラットフォームは、テキスト色と背景色の既定値は異なるが、多くの場合、いずれかに設定した場合は、両方を設定する必要があります。

エントリのテキストの色を設定するのにには、次のコードを使用します。

XAML:

```xaml
<Entry TextColor="Green" />
```

C# の場合:

```csharp
var entry = new Entry();
entry.TextColor = Color.Green;
```

![](entry-images/textcolor.png "エントリ TextColor 例")

指定されたプレース ホルダーはありませんが影響を受ける`TextColor`です。

XAML では、背景色を設定します。

```xaml
<Entry BackgroundColor="#2c3e50" />
```

C# の場合:

```csharp
var entry = new Entry();
entry.BackgroundColor = Color.FromHex("#2c3e50");
```

![](entry-images/textbackgroundcolor.png "エントリ BackgroundColor 例")

確認して、背景色とテキスト色を選択する各プラットフォームで使用可能な任意のプレース ホルダー テキストが不明瞭になるように注意します。

## <a name="events-and-interactivity"></a>イベントと対話機能

エントリは、2 つのイベントを公開します。

- [TextChanged](http://developer.xamarin.com/api/event/Xamarin.Forms.Entry.TextChanged/) &ndash;エントリのテキストが変更されたときに発生します。 前に、と変更後のテキストを提供します。
- [完了](http://developer.xamarin.com/api/event/Xamarin.Forms.Entry.Completed/)&ndash;ユーザーには、キーボードの戻り値のキーを押して、入力が終了したときに発生します。

### <a name="completed"></a>完了

`Completed`イベントは、エントリとのやり取りの完了に対応するために使用します。 `Completed` キーボードの戻り値のキーを入力して、ユーザーがフィールドに入力を終了するときに発生します。 イベントのハンドラーは、ジェネリック イベント ハンドラーは、送信者を取得し、 `EventArgs`:

```csharp
void Entry_Completed (object sender, EventArgs e)
{
    var text = ((Entry)sender).Text; //cast sender to access the properties of the Entry
}
```

XAML を完了イベントをサブスクライブすることができます。

```xaml
<Entry Completed="Entry_Completed" />
```

および C# の場合:

```csharp
var entry = new Entry ();
entry.Completed += Entry_Completed;
```

### <a name="textchanged"></a>TextChanged

`TextChanged`イベントは、フィールドの内容の変更に対応するために使用します。

`TextChanged` 発生するたびに、`Text`の`Entry`変更します。 インスタンスを受け取り、イベントのハンドラー`TextChangedEventArgs`です。 `TextChangedEventArgs` 新旧の値にアクセスできるように、 `Entry` `Text`を介して、`OldTextValue`と`NewTextValue`プロパティ。

```csharp
void Entry_TextChanged (object sender, TextChangedEventArgs e)
{
    var oldText = e.OldTextValue;
    var newText = e.NewTextValue;
}
```

`TextChanged` XAML でイベントをサブスクライブすることができます。

```xaml
<Entry TextChanged="Entry_TextChanged" />
```

および C# の場合:

```csharp
var entry = new Entry ();
entry.TextChanged += Entry_TextChanged;
```


## <a name="related-links"></a>関連リンク

- [テキスト (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [API のエントリ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)
