---
title: Xamarin.Forms のエントリ
description: この記事では、Xamarin.Forms エントリ クラスを使用して、1 行のテキストまたはアプリケーションでパスワードの入力をそのまま使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 9923C541-3C10-4D14-BAB5-C4D6C514FB1E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/31/2018
ms.openlocfilehash: 95afdfde878759d4a598e200d16fe6fb1fa2005e
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998244"
---
# <a name="xamarinforms-entry"></a>Xamarin.Forms のエントリ

_単一行のテキストまたはパスワードを入力_

Xamarin.Forms`Entry`を単一行のテキスト入力に使用します。 `Entry`と同様に、`Editor`ビューで、複数のキーボードの種類をサポートしています。 さらに、`Entry`パスワード フィールドとして使用できます。

## <a name="display-customization"></a>表示のカスタマイズ

### <a name="setting-and-reading-text"></a>設定やテキストの読み取り

`Entry`などの他のテキストを表すビューを公開、`Text`プロパティ。 このプロパティは、設定し、によって提示されるテキストの読み取りに使用できます、`Entry`します。 次の例では、設定、 `Text` XAML のプロパティ。

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

### <a name="keyboards"></a>キーボード

ユーザーが対話する際に表示されるキーボード、`Entry`経由でプログラムによって設定できる、`Keyboard`プロパティ。

キーボードの種類のオプションがあります。

- **既定**&ndash;既定のキーボード
- **チャット**&ndash;のためのテキストとプレース絵文字が便利です
- **電子メール**&ndash;電子メール アドレスを入力するときに使用
- **数値**&ndash;数値を入力するときに使用
- **電話**&ndash;電話番号を入力するときに使用
- **Url** &ndash;ファイル パスと web アドレスを入力するために使用

[各キーボードの使用例](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/choose-keyboard-for-entry/)レシピ セクションでします。

### <a name="enabling-and-disabling-spell-checking"></a>有効にして、スペル チェックを無効化

[ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled)プロパティ コントロールかどうかスペル チェックを有効にします。 既定では、プロパティに設定が`true`します。 テキストを入力すると、スペル ミスが示されます。

ただし、ユーザー名を入力するなど、いくつかのテキスト エントリ シナリオのスペル チェックを提供、負のエクスペリエンスとこれを設定して無効にする必要があります、 [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled)プロパティを`false`:

```xaml
<Entry ... IsSpellCheckEnabled="false" />
```

```csharp
var entry = new Entry { ... IsSpellCheckEnabled = false };
```

> [!NOTE]
> ときに、 [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled)プロパティに設定されて`false`、およびカスタムのキーボードが使用されていない、ネイティブのスペル チェックが無効になります。 ただし場合、 [ `Keyboard` ](xref:Xamarin.Forms.Keyboard)がされているセットをスペル チェックを無効にするチェックを行うなど[ `Keyboard.Chat` ](xref:Xamarin.Forms.Keyboard.Chat)、`IsSpellCheckEnabled`プロパティは無視されます。 そのため、スペル チェックを有効にするプロパティを使用できません、`Keyboard`を明示的に無効にします。

### <a name="placeholders"></a>プレース ホルダー

`Entry` ユーザー入力を格納するがない場合は、プレース ホルダー テキストを表示するのには設定できます。 実際には、これは多くの場合、見フォームでは、特定のフィールドの適切なコンテンツを明確にします。 プレース ホルダー テキストの色のカスタマイズすることはできず、関係なく同じになります、`TextColor`設定します。 カスタム プレース ホルダーの色を呼び出すと、設計場合に、フォールバックする必要があります、[カスタム レンダラー]()します。 次を作成、 `Entry` "Username"XAML 内のプレース ホルダーとして使用します。

```xaml
<Entry Placeholder="Username" />
```

C# の場合:

```csharp
var MyEntry = new Entry { Placeholder = "Username" };
```

![](entry-images/placeholder.png "エントリのプレース ホルダーの例")

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

- [TextChanged](xref:Xamarin.Forms.Entry.TextChanged) &ndash;エントリのテキストが変更されたときに発生します。 変更の前後にテキストを提供します。
- [完了した](xref:Xamarin.Forms.Entry.Completed)&ndash;ユーザーには、キーボードの戻り値のキーを押して、入力が終了したときに発生します。

### <a name="completed"></a>完了

`Completed`エントリとのやり取りの完了に反応するイベントを使用します。 `Completed` キーボードの戻り値のキーを入力して、ユーザーが終了する入力フィールドを持つ場合に発生します。 イベントのハンドラーは、送信者を取得、汎用イベント ハンドラーと`EventArgs`:

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
