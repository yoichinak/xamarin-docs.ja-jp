---
title: ポップアップを表示する
description: Xamarin. フォームには、警告、アクションシート、プロンプトという3つのポップアップのようなユーザーインターフェイス要素が用意されています。 この記事では、アラート、アクションシート、およびプロンプト Api を使用して、ユーザーに簡単な質問をするダイアログボックスを表示したり、ユーザーにタスクを案内したり、プロンプトを表示したりする方法について説明します。
ms.prod: xamarin
ms.assetid: 46AB0D5E-0025-4A8A-9D00-3E66C3D0BA2E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/17/2020
ms.openlocfilehash: c71153cdaa94a7983b89968abc828011a648f2b1
ms.sourcegitcommit: 10b4d7952d78f20f753372c53af6feb16918555c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/26/2020
ms.locfileid: "77636100"
---
# <a name="display-pop-ups"></a>ポップアップを表示する

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-pop-ups)

警告を表示する、ユーザーに選択を求める、またはプロンプトを表示することは、一般的な UI タスクです。 [`Page`](xref:Xamarin.Forms.Page)クラスには、 [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*)、 [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*)、および `DisplayPromptAsync`のポップアップを使用してユーザーと対話するための3つのメソッドがあります。 それらは適切なネイティブ コントロールを使用して、各プラットフォーム上に表示されます。

## <a name="display-an-alert"></a>アラートを表示する

Xamarin.Forms に対応するすべてのプラットフォームには、ユーザーにアラートを表示または簡単な質問をする、モーダル ポップアップが用意されています。 Xamarin.Forms にこれらのアラートを表示するには、任意の [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) 上で [`Page`](xref:Xamarin.Forms.Page) メソッドを使用します。 次のコード行は、ユーザーに簡単なメッセージを表示します。

```csharp
await DisplayAlert ("Alert", "You have been alerted", "OK");
```

![](pop-ups-images/alert.png "Alert Dialog with One Button")

この例は、ユーザーから情報を収集しません。 アラートはモーダルとして表示され、一度無視するとユーザーは引き続きアプリケーションとやり取りできるようになります。

また、[`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) メソッドは、2 つのボタンを表示して `boolean` を返すことで、ユーザーの応答をキャプチャするためにも使用できます。 アラートから応答を取得するには、両方のボタンにテキストを提供し、メソッドを `await` します。 ユーザーがオプションのいずれかを選択すると、コードに応答が返されます。 以下のサンプル コード内の `async` キーワードと `await` キーワードに注意してください。

```csharp
async void OnAlertYesNoClicked (object sender, EventArgs e)
{
  bool answer = await DisplayAlert ("Question?", "Would you like to play a game", "Yes", "No");
  Debug.WriteLine ("Answer: " + answer);
}
```

[![DisplayAlert](pop-ups-images/alert2-sml.png "2つのボタンを含むアラートダイアログ")](pop-ups-images/alert2.png#lightbox "2つのボタンを含むアラートダイアログ")

## <a name="guide-users-through-tasks"></a>タスクを通じてユーザーをガイドする

[UIActionSheet](https://developer.apple.com/library/ios/documentation/uikit/reference/uiactionsheet_class/Reference/Reference.html) は、iOS の一般的な UI 要素です。 Xamarin.Forms の [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*)メソッドを使用すると、このコントロールをクロスプラットフォーム アプリにインクルードし、Android や UWP でネイティブの代替アイテムをレンダリングできます。

操作シートを表示するには、任意の[`Page`](xref:Xamarin.Forms.Page)に[`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*)を `await` して、メッセージとボタンのラベルを文字列として渡します。 そのメソッドは、ユーザーがクリックしたボタンの文字列ラベルを返します。 簡単な例を次に示します。

```csharp
async void OnActionSheetSimpleClicked (object sender, EventArgs e)
{
  string action = await DisplayActionSheet ("ActionSheet: Send to?", "Cancel", null, "Email", "Twitter", "Facebook");
  Debug.WriteLine ("Action: " + action);
}
```

![](pop-ups-images/action.png "ActionSheet Dialog")

`destroy` ボタンの表示は他のボタンとは異なり、`null` のままにするか、3 つ目の文字列パラメーターとして指定できます。 次の例では、`destroy` ボタンを使用します。

```csharp
async void OnActionSheetCancelDeleteClicked (object sender, EventArgs e)
{
  string action = await DisplayActionSheet ("ActionSheet: SavePhoto?", "Cancel", "Delete", "Photo Roll", "Email");
  Debug.WriteLine ("Action: " + action);
}
```

[![DisplayActionSheet](pop-ups-images/action2-sml.png "[破棄] ボタンがある操作シートダイアログ")](pop-ups-images/action2.png#lightbox "[破棄] ボタンがある操作シートダイアログ")

## <a name="display-a-prompt"></a>プロンプトを表示する

プロンプトを表示するには、任意の[`Page`](xref:Xamarin.Forms.Page)で `DisplayPromptAsync` を呼び出し、タイトルとメッセージを `string` 引数として渡します。

```csharp
string result = await DisplayPromptAsync("Question 1", "What's your name?");
```

プロンプトはモーダルで表示されます。

[![IOS と Android でのモーダルプロンプトのスクリーンショット](pop-ups-images/simple-prompt.png "モーダルプロンプト")](pop-ups-images/simple-prompt-large.png#lightbox "モーダルプロンプト")

[OK] ボタンをタップすると、入力した応答が `string`として返されます。 [キャンセル] ボタンがタップされると `null` が返されます。

`DisplayPromptAsync` メソッドの完全な引数リストは次のとおりです。

- `string`型の `title`は、プロンプトに表示するタイトルです。
- `string`型の `message`は、プロンプトに表示するメッセージです。
- `string`型の `accept`は、[accept] ボタンのテキストです。 これは省略可能な引数で、既定値は OK です。
- `string`型の `cancel`は、[キャンセル] ボタンのテキストです。 これは省略可能な引数で、既定値は Cancel です。
- `string`型の `placeholder`は、プロンプトに表示するプレースホルダーテキストです。 これは省略可能な引数で、既定値は `null`です。
- `int`型の `maxLength`は、ユーザー応答の最大長です。 これは省略可能な引数で、既定値は-1 です。
- `Keyboard`型の `keyboard`は、ユーザーの応答に使用するキーボードの種類です。 これは省略可能な引数で、既定値は `Keyboard.Default`です。
- `string`型の `initialValue`は、事前に定義された応答であり、これを編集できます。 これは省略可能な引数で、既定値は空の `string`です。

次の例では、一部の省略可能な引数の設定を示します。

```csharp
string result = await DisplayPromptAsync("Question 2", "What's 5 + 5?", initialValue: "10", maxLength: 2, keyboard: Keyboard.Numeric);
```

このコードは、定義済みの10の応答を表示し、2に入力できる文字数を制限し、ユーザー入力用の数値キーボードを表示します。

[![IOS と Android でのモーダルプロンプトのスクリーンショット](pop-ups-images/keyboard-prompt.png "モーダルプロンプト")](pop-ups-images/keyboard-prompt-large.png#lightbox "モーダルプロンプト")

> [!NOTE]
> `DisplayPromptAsync` メソッドは現在、iOS と Android にのみ実装されています。

## <a name="related-links"></a>関連リンク

- [PopupsSample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-pop-ups)
