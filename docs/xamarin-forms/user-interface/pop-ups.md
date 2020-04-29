---
title: ポップアップを表示する
description: Xamarin. フォームには、警告、アクションシート、プロンプトという3つのポップアップのようなユーザーインターフェイス要素が用意されています。 この記事では、アラート、アクションシート、およびプロンプト Api を使用して、ユーザーに簡単な質問をするダイアログボックスを表示したり、ユーザーにタスクを案内したり、プロンプトを表示したりする方法について説明します。
ms.prod: xamarin
ms.assetid: 46AB0D5E-0025-4A8A-9D00-3E66C3D0BA2E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2020
ms.openlocfilehash: 87348d5821c2c9e2e46a777f212bd5f69d1a54d0
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/29/2020
ms.locfileid: "82517579"
---
# <a name="display-pop-ups"></a>ポップアップを表示する

[![](~/media/shared/download.png)サンプルをダウンロードするサンプルをダウンロードする](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-pop-ups)

警告を表示する、ユーザーに選択を求める、またはプロンプトを表示することは、一般的な UI タスクです。 この[`Page`](xref:Xamarin.Forms.Page)クラスには[`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*)、、 [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*)、およびと`DisplayPromptAsync`いうポップアップを使用してユーザーと対話するための3つのメソッドがあります。 それらは適切なネイティブ コントロールを使用して、各プラットフォーム上に表示されます。

## <a name="display-an-alert"></a>アラートを表示する

Xamarin.Forms に対応するすべてのプラットフォームには、ユーザーにアラートを表示または簡単な質問をする、モーダル ポップアップが用意されています。 これらのアラートを Xamarin. Forms で表示するに[`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*)は、任意[`Page`](xref:Xamarin.Forms.Page)のでメソッドを使用します。 次のコード行は、ユーザーに簡単なメッセージを表示します。

```csharp
await DisplayAlert ("Alert", "You have been alerted", "OK");
```

![](pop-ups-images/alert.png "Alert Dialog with One Button")

この例は、ユーザーから情報を収集しません。 アラートはモーダルとして表示され、一度無視するとユーザーは引き続きアプリケーションとやり取りできるようになります。

[`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*)メソッドを使用して、2つのボタンを表示し、を`boolean`返すことによって、ユーザーの応答をキャプチャすることもできます。 アラートから応答を取得するには、両方のボタンにテキストを提供し、メソッドを `await` します。 ユーザーがオプションのいずれかを選択すると、コードに応答が返されます。 以下のサンプル コード内の `async` キーワードと `await` キーワードに注意してください。

```csharp
async void OnAlertYesNoClicked (object sender, EventArgs e)
{
  bool answer = await DisplayAlert ("Question?", "Would you like to play a game", "Yes", "No");
  Debug.WriteLine ("Answer: " + answer);
}
```

[![DisplayAlert](pop-ups-images/alert2-sml.png "2つのボタンを含むアラートダイアログ")](pop-ups-images/alert2.png#lightbox "2つのボタンを含むアラートダイアログ")

## <a name="guide-users-through-tasks"></a>タスクを通じてユーザーをガイドする

[UIActionSheet](https://developer.apple.com/library/ios/documentation/uikit/reference/uiactionsheet_class/Reference/Reference.html) は、iOS の一般的な UI 要素です。 Xamarin. Forms [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*)メソッドを使用すると、クロスプラットフォームアプリにこのコントロールを追加し、ANDROID および UWP でネイティブの代替手段をレンダリングできます。

操作シートを`await` [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*)任意[`Page`](xref:Xamarin.Forms.Page)の形式で表示するには、メッセージとボタンのラベルを文字列として渡します。 そのメソッドは、ユーザーがクリックしたボタンの文字列ラベルを返します。 簡単な例を次に示します。

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

## <a name="display-a-prompt"></a>プロンプトの表示

プロンプトを表示するには、 `DisplayPromptAsync`任意[`Page`](xref:Xamarin.Forms.Page)のでを呼び出し、タイトルとメッセージ`string`を引数として渡します。

```csharp
string result = await DisplayPromptAsync("Question 1", "What's your name?");
```

プロンプトはモーダルで表示されます。

[![IOS と Android でのモーダルプロンプトのスクリーンショット](pop-ups-images/simple-prompt.png "モーダルプロンプト")](pop-ups-images/simple-prompt-large.png#lightbox "モーダルプロンプト")

[OK] ボタンをタップする`string`と、入力した応答がとして返されます。 [キャンセル] ボタンがタップさ`null`れると、が返されます。

`DisplayPromptAsync`メソッドの完全な引数リストは次のとおりです。

- `title`型`string`のは、プロンプトに表示するタイトルです。
- `message`型`string`のは、プロンプトに表示するメッセージです。
- `accept`型`string`のは、[accept] ボタンのテキストです。 これは省略可能な引数で、既定値は OK です。
- `cancel`型`string`のは、[キャンセル] ボタンのテキストです。 これは省略可能な引数で、既定値は Cancel です。
- `placeholder`型`string`のは、プロンプトに表示するプレースホルダーテキストです。 これは省略可能な引数で、既定値`null`はです。
- `maxLength`型`int`のは、ユーザー応答の最大長です。 これは省略可能な引数で、既定値は-1 です。
- `keyboard`型`Keyboard`のは、ユーザーの応答に使用するキーボードの種類です。 これは省略可能な引数で、既定値`Keyboard.Default`はです。
- `initialValue`型`string`のは、事前定義された応答であり、これを編集することができます。 これは省略可能な引数で、既定値は空`string`です。

次の例では、一部の省略可能な引数の設定を示します。

```csharp
string result = await DisplayPromptAsync("Question 2", "What's 5 + 5?", initialValue: "10", maxLength: 2, keyboard: Keyboard.Numeric);
```

このコードは、定義済みの10の応答を表示し、2に入力できる文字数を制限し、ユーザー入力用の数値キーボードを表示します。

[![IOS と Android でのモーダルプロンプトのスクリーンショット](pop-ups-images/keyboard-prompt.png "モーダルプロンプト")](pop-ups-images/keyboard-prompt-large.png#lightbox "モーダルプロンプト")

## <a name="related-links"></a>関連リンク

- [PopupsSample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-pop-ups)
