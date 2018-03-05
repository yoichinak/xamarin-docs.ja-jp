---
title: "ポップアップを表示します。"
description: "Xamarin.Forms では、次の 2 つの pop 増のようなユーザー インターフェイス要素 –、アラートとアクション シートを提供します。 この記事では、単純な質問をユーザーに確認してを使用してタスクを使用してユーザーにアラートとアクション シート Api を使用してを示します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 46AB0D5E-0025-4A8A-9D00-3E66C3D0BA2E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 957db750b852b40daf1556e8dc8f7ba18e022dba
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2018
---
# <a name="displaying-pop-ups"></a>ポップアップを表示します。

_Xamarin.Forms では、次の 2 つの pop 増のようなユーザー インターフェイス要素 –、アラートとアクション シートを提供します。この記事では、単純な質問をユーザーに確認してを使用してタスクを使用してユーザーにアラートとアクション シート Api を使用してを示します。_

通知を表示するか、選択することをユーザーは、一般的な UI タスクです。 Xamarin.Forms 2 つのメソッドを持つ、 [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)ポップアップを使用して、ユーザーと対話するためのクラス: [ `DisplayAlert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)/)と[ `DisplayActionSheet`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayActionSheet(System.String,System.String,System.String,System.String[])/)です。 各プラットフォームの適切なネイティブ コントロールにレンダリングされます。

## <a name="displaying-an-alert"></a>通知を表示します。

Xamarin.Forms でサポートされているすべてのプラットフォームでは、モーダル ポップアップのユーザーに警告や、それらの単純な質問があります。 Xamarin.Forms では、これらのアラートを表示するには、使用、 [ `DisplayAlert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)/)いずれかのメソッド[ `Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)です。 次のコード行は、ユーザーに、単純なメッセージを示しています。

```csharp
DisplayAlert ("Alert", "You have been alerted", "OK");
```

![](pop-ups-images/alert.png "1 つのボタンと警告のダイアログ")

この例では、ユーザーの情報は収集しません。 このアラートをモーダルとして表示され、ユーザーが破棄された後、アプリケーションとの対話が続行されます。

[ `DisplayAlert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)/)メソッドは、2 つのボタンを表示し、返すことで、ユーザーの応答をキャプチャするも使用できます、`boolean`です。 アラートからの応答を取得するには、2 つのボタンのテキストを指定し、`await`メソッドです。 ユーザーが、オプションのいずれかを選択した後は、コードに、応答が返されます。 注、`async`と`await`次のサンプル コードに含まれるキーワード。

```csharp
async void OnAlertYesNoClicked (object sender, EventArgs e)
{
  var answer = await DisplayAlert ("Question?", "Would you like to play a game", "Yes", "No");
  Debug.WriteLine ("Answer: " + answer);
}
```

[ ![DisplayAlert](pop-ups-images/alert2-sml.png "アラートの 2 つのボタンを持つダイアログ")](pop-ups-images/alert2.png "2 つのボタンを持つダイアログのアラート")

## <a name="guiding-users-through-tasks"></a>タスクを使用してユーザー ガイド

[UIActionSheet](https://developer.apple.com/library/ios/documentation/uikit/reference/uiactionsheet_class/Reference/Reference.html) iOS での一般的な UI 要素です。 Xamarin.Forms [ `DisplayActionSheet` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayActionSheet(System.String,System.String,System.String,System.String[])/)メソッドを使用して、このコントロールは、クロス プラットフォーム アプリでは、Android および Windows Phone でネイティブの選択肢を表示できます。

アクション、シートを表示する`await` [ `DisplayActionSheet` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayActionSheet(System.String,System.String,System.String,System.String[])/)いずれかで[ `Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)メッセージを渡すこと、および文字列としてのラベルのボタンをクリックします。 このメソッドは、ユーザーがクリックしてされたボタンの文字列のラベルを返します。 単純な例を次に示します。

```csharp
async void OnActionSheetSimpleClicked (object sender, EventArgs e)
{
  var action = await DisplayActionSheet ("ActionSheet: Send to?", "Cancel", null, "Email", "Twitter", "Facebook");
  Debug.WriteLine ("Action: " + action);
}
```

![](pop-ups-images/action.png "ActionSheet ダイアログ")

`destroy`ボタンは、他とは異なるが表示され、ままでかまいません`null`または 3 番目の文字列パラメーターとして指定します。 次の例では、`destroy`ボタンをクリックします。

```csharp
async void OnActionSheetCancelDeleteClicked (object sender, EventArgs e)
{
  var action = await DisplayActionSheet ("ActionSheet: SavePhoto?", "Cancel", "Delete", "Photo Roll", "Email");
  Debug.WriteLine ("Action: " + action);
}
```

[ ![DisplayActionSheet](pop-ups-images/action2-sml.png "アクション シート ダイアログ破棄 ボタンを")](pop-ups-images/action2.png "破棄 ボタンとアクション シート ダイアログ ボックス")

## <a name="summary"></a>まとめ

この記事では、単純な質問をユーザーに確認してを使用してタスクを使用してユーザーにアラートとアクション シート Api を使用して示されています。 Xamarin.Forms 2 つのメソッドを持つ、 [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)ポップアップを使用して、ユーザーと対話するためのクラス: [ `DisplayAlert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)/)と[ `DisplayActionSheet` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayActionSheet(System.String,System.String,System.String,System.String[])/)、され、両方のレンダリングに各プラットフォームでネイティブ コントロールの適切です。



## <a name="related-links"></a>関連リンク

- [PopupsSample](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Pop-ups/)
