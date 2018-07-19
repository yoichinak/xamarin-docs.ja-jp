---
title: ポップアップを表示します
description: Xamarin.Forms は、2 つのポップアップ サインアップのようなユーザー インターフェイス要素 – アラートとアクション シートを提供します。 この記事では、アラートとアクション シート Api を使用して簡単な質問をユーザーに確認して、手順をユーザーにタスクを示します。
ms.prod: xamarin
ms.assetid: 46AB0D5E-0025-4A8A-9D00-3E66C3D0BA2E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 156c2f9dca47a7755d4f810d7921a05662388ded
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996715"
---
# <a name="displaying-pop-ups"></a>ポップアップを表示します

_Xamarin.Forms は、2 つのポップアップ サインアップのようなユーザー インターフェイス要素 – アラートとアクション シートを提供します。この記事では、アラートとアクション シート Api を使用して簡単な質問をユーザーに確認して、手順をユーザーにタスクを示します。_

アラートを表示または選択することを求める、一般的な UI タスクです。 Xamarin.Forms 2 つのメソッドを持つ、 [ `Page` ](xref:Xamarin.Forms.Page)ポップアップを使用するユーザーと対話するためのクラス: [ `DisplayAlert` ](xref:Xamarin.Forms.Page.DisplayAlert*)と[ `DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*)します。 各プラットフォームの適切なネイティブ コントロールでレンダリングされます。

## <a name="displaying-an-alert"></a>通知を表示します。

Xamarin.Forms でサポートされているすべてのプラットフォームでは、モーダル ポップアップのユーザーに警告またはそれらの単純な質問があります。 Xamarin.Forms では、これらのアラートを表示するには、使用、 [ `DisplayAlert` ](xref:Xamarin.Forms.Page.DisplayAlert*)いずれかのメソッド[ `Page`](xref:Xamarin.Forms.Page)します。 次のコード行には、ユーザーに、単純なメッセージが表示されます。

```csharp
DisplayAlert ("Alert", "You have been alerted", "OK");
```

![](pop-ups-images/alert.png "1 つのボタンと警告のダイアログ")

この例では、ユーザーからの情報は収集しません。 アラートをモーダルとして表示され、ユーザーを破棄すると、アプリケーションとの対話が続行されます。

[ `DisplayAlert` ](xref:Xamarin.Forms.Page.DisplayAlert*)メソッドが 2 つのボタンを表示し、返すことで、ユーザーの応答をキャプチャすることもでき、`boolean`します。 アラートからの応答を取得するには、2 つのボタンのテキストを指定し、`await`メソッド。 ユーザーが、オプションのいずれかを選択した後は、コードに、応答が返されます。 注、`async`と`await`次のサンプル コードに含まれるキーワード。

```csharp
async void OnAlertYesNoClicked (object sender, EventArgs e)
{
  var answer = await DisplayAlert ("Question?", "Would you like to play a game", "Yes", "No");
  Debug.WriteLine ("Answer: " + answer);
}
```

[![DisplayAlert](pop-ups-images/alert2-sml.png "アラートの 2 つのボタンを持つダイアログ")](pop-ups-images/alert2.png#lightbox "アラート ダイアログに 2 つのボタン")

## <a name="guiding-users-through-tasks"></a>タスクを使用してユーザー ガイド

[UIActionSheet](https://developer.apple.com/library/ios/documentation/uikit/reference/uiactionsheet_class/Reference/Reference.html) iOS での一般的な UI 要素です。 Xamarin.Forms [ `DisplayActionSheet` ](xref:Xamarin.Forms.Page.DisplayActionSheet*)メソッドを使用して、クロス プラットフォーム アプリ、Android、UWP にネイティブの代替レンダリングでこのコントロールを追加できます。

アクション シートを表示する`await` [ `DisplayActionSheet` ](xref:Xamarin.Forms.Page.DisplayActionSheet*)いずれかで[ `Page`](xref:Xamarin.Forms.Page)メッセージを渡すと、文字列としてのラベルのボタンをクリックします。 メソッドは、ユーザーがクリックしてされたボタンの文字列のラベルを返します。 簡単な例を次に示します。

```csharp
async void OnActionSheetSimpleClicked (object sender, EventArgs e)
{
  var action = await DisplayActionSheet ("ActionSheet: Send to?", "Cancel", null, "Email", "Twitter", "Facebook");
  Debug.WriteLine ("Action: " + action);
}
```

![](pop-ups-images/action.png "ActionSheet ダイアログ")

`destroy`ボタンが表示される、他とは異なるとおくことができます`null`または 3 番目の文字列パラメーターとして指定します。 次の例では、`destroy`ボタンをクリックします。

```csharp
async void OnActionSheetCancelDeleteClicked (object sender, EventArgs e)
{
  var action = await DisplayActionSheet ("ActionSheet: SavePhoto?", "Cancel", "Delete", "Photo Roll", "Email");
  Debug.WriteLine ("Action: " + action);
}
```

[![DisplayActionSheet](pop-ups-images/action2-sml.png "破棄 ボタンをアクション シート ダイアログ")](pop-ups-images/action2.png#lightbox "破棄 ボタンをアクション シートのダイアログ ボックス")

## <a name="summary"></a>まとめ

この記事では、簡単な質問をユーザーに確認して、タスクを使用してユーザーをガイドするアラートとアクション シート Api を使用して示されています。 Xamarin.Forms 2 つのメソッドを持つ、 [ `Page` ](xref:Xamarin.Forms.Page)ポップアップを使用するユーザーと対話するためのクラス: [ `DisplayAlert` ](xref:Xamarin.Forms.Page.DisplayAlert*)と[ `DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*)とは各プラットフォームの適切なネイティブ コントロールでレンダリング両方。



## <a name="related-links"></a>関連リンク

- [PopupsSample](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Pop-ups/)
