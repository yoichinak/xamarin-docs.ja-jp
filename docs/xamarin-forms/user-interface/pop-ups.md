---
title: ポップアップを表示します。
description: Xamarin.Forms には、ポップアップに似た 2 つのユーザー インターフェイス要素、アラートとアクション シートが用意されています。 アラートとアクション シート Api を使用して、簡単な質問をユーザーに依頼し、手順をユーザーにタスク ダイアログ ボックスを表示するかを説明します。
ms.prod: xamarin
ms.assetid: 46AB0D5E-0025-4A8A-9D00-3E66C3D0BA2E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 58c98aefdf87bcd1ca819de96f67c66646c1723d
ms.sourcegitcommit: 6ad272c2c7b0c3c30e375ad17ce6296ac1ce72b2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/23/2019
ms.locfileid: "66182304"
---
# <a name="display-pop-ups"></a>ポップアップを表示します。

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Pop-ups/)

_Xamarin.Forms には、ポップアップに似た 2 つのユーザー インターフェイス要素、アラートとアクション シートが用意されています。アラートとアクション シート Api を使用して、簡単な質問をユーザーに依頼し、手順をユーザーにタスク ダイアログ ボックスを表示するかを説明します。_

UI の一般的なタスクは、アラートを表示するまたはユーザーに選択を求めることです。 Xamarin.Forms には、[`Page`](xref:Xamarin.Forms.Page) クラス上に、ポップアップを介してユーザーとやり取りをするための、[`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) と [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*) の 2 つのメソッドがあります。 それらは適切なネイティブ コントロールを使用して、各プラットフォーム上に表示されます。

## <a name="display-an-alert"></a>アラートを表示する

Xamarin.Forms に対応するすべてのプラットフォームには、ユーザーにアラートを表示または簡単な質問をする、モーダル ポップアップが用意されています。 Xamarin.Forms にこれらのアラートを表示するには、任意の [`Page`](xref:Xamarin.Forms.Page) 上で [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) メソッドを使用します。 次のコード行は、ユーザーに簡単なメッセージを表示します。

```csharp
DisplayAlert ("Alert", "You have been alerted", "OK");
```

![](pop-ups-images/alert.png "1 つのボタン付きのアラート ダイアログ")

この例は、ユーザーから情報を収集しません。 アラートはモーダルとして表示され、一度無視するとユーザーは引き続きアプリケーションとやり取りできるようになります。

また、[`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) メソッドは、2 つのボタンを表示して `boolean` を返すことで、ユーザーの応答をキャプチャするためにも使用できます。 アラートから応答を取得するには、両方のボタンにテキストを提供し、メソッドを `await` します。 ユーザーがオプションのいずれかを選択すると、コードに応答が返されます。 以下のサンプル コード内の `async` キーワードと `await` キーワードに注意してください。

```csharp
async void OnAlertYesNoClicked (object sender, EventArgs e)
{
  bool answer = await DisplayAlert ("Question?", "Would you like to play a game", "Yes", "No");
  Debug.WriteLine ("Answer: " + answer);
}
```

[![DisplayAlert](pop-ups-images/alert2-sml.png "2 つのボタン付きのアラート ダイアログ")](pop-ups-images/alert2.png#lightbox "2 つのボタン付きのアラート ダイアログ")

## <a name="guide-users-through-tasks"></a>タスクをユーザーに案内

[UIActionSheet](https://developer.apple.com/library/ios/documentation/uikit/reference/uiactionsheet_class/Reference/Reference.html) は、iOS の一般的な UI 要素です。 Xamarin.Forms の [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*)メソッドを使用すると、このコントロールをクロスプラットフォーム アプリにインクルードし、Android や UWP でネイティブの代替アイテムをレンダリングできます。

アクション シートを表示するには、任意の [`Page`](xref:Xamarin.Forms.Page) 内の [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*) を `await` し、メッセージとボタンのラベルを文字列として渡します。 そのメソッドは、ユーザーがクリックしたボタンの文字列ラベルを返します。 簡単な例を次に示します。

```csharp
async void OnActionSheetSimpleClicked (object sender, EventArgs e)
{
  string action = await DisplayActionSheet ("ActionSheet: Send to?", "Cancel", null, "Email", "Twitter", "Facebook");
  Debug.WriteLine ("Action: " + action);
}
```

![](pop-ups-images/action.png "ActionSheet ダイアログ")

`destroy` ボタンの表示は他のボタンとは異なり、`null` のままにするか、3 つ目の文字列パラメーターとして指定できます。 次の例では、`destroy` ボタンを使用します。

```csharp
async void OnActionSheetCancelDeleteClicked (object sender, EventArgs e)
{
  string action = await DisplayActionSheet ("ActionSheet: SavePhoto?", "Cancel", "Delete", "Photo Roll", "Email");
  Debug.WriteLine ("Action: " + action);
}
```

[![DisplayActionSheet](pop-ups-images/action2-sml.png "destroy ボタン付きのアクション シート ダイアログ")](pop-ups-images/action2.png#lightbox "destroy ボタン付きのアクション シート ダイアログ")

## <a name="related-links"></a>関連リンク

- [PopupsSample](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Pop-ups/)
