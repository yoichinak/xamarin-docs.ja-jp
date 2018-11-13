---
title: 第 24 章の概要です。 ページのナビゲーション
description: 'Xamarin.Forms によるモバイル アプリの作成: 第 24 章の概要。 ページのナビゲーション'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: DDCDB49C-6008-4F72-B095-463EE21D7C23
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: d90bef4da589215247f412450905a5db541b0444
ms.sourcegitcommit: 03dfb4a2c20ad68515875b415e7d84ee9b0a8cb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2018
ms.locfileid: "51563161"
---
# <a name="summary-of-chapter-24-page-navigation"></a>第 24 章の概要です。 ページのナビゲーション

多くのアプリケーションは、ユーザーが移動する複数のページで構成されます。 アプリケーションは常には、*メイン*ページまたは*ホーム* ページで、し、そこから、ユーザーの移動に他のページに戻るためのスタックに保持されます。 追加のナビゲーション オプションは、「 [**第 25 章です。さまざまなページ**](chapter25.md)します。

## <a name="modal-pages-and-modeless-pages"></a>ページのモーダルとモードレスのページ

`VisualElement` 定義、 [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation)型のプロパティ[ `INavigation` ](xref:Xamarin.Forms.INavigation)、新しいページに移動する次の 2 つのメソッドが含まれます。

- [`PushAsync`](xref:Xamarin.Forms.INavigation.PushAsync(Xamarin.Forms.Page))
- [`PushModalAsync`](xref:Xamarin.Forms.INavigation.PushModalAsync(Xamarin.Forms.Page))

どちらのメソッド、`Page`インスタンスを引数と戻り値として、`Task`オブジェクト。 次の 2 つのメソッドは、前のページに移動します。

- [`PopAsync`](xref:Xamarin.Forms.INavigation.PopAsync)
- [`PopModalAsync`](xref:Xamarin.Forms.INavigation.PopModalAsync)

ユーザー インターフェイスには、独自場合**戻る**ボタン (Android および Windows phone での操作) と、アプリケーションをこれらのメソッドを呼び出す必要はありません。

これらのメソッドは、`VisualElement`から呼び出された一般的に、`Navigation`プロパティ、現在の`Page`インスタンス。

前のページに戻る前に、ページにいくつかの情報を提供する、ユーザーが必要な場合に、アプリケーションは一般にモーダル ページを使用します。 モーダルではないページとも呼ばれるモードレスまたは*階層*します。 モーダルまたはモードレス; として区別ページ自体ではありません。これは、そこに移動するために使用するメソッドによって、代わりに制御されます。 すべてのプラットフォームで作業を行う、モーダル ページは、前のページに戻るの独自のユーザー インターフェイスを提供する必要があります。

[ **ModelessAndModal** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ModelessAndModal)モードレスとモーダル ページ間の違いを調査することができます。 ページ ナビゲーションを使用するアプリケーションがそのホーム ページに渡す必要があります、 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage)コンス トラクター、プログラムの一般に`App`クラス。 1 つおまけが不要になったを設定する必要があること、 `Padding` iOS 用のページ。

モードレス ページ、ページのことを発見したは[ `Title` ](xref:Xamarin.Forms.Page.Title)プロパティが表示されます。 IOS、Android、および Windows タブレットとデスクトップ プラットフォームは、前のページに戻るユーザー インターフェイス要素を提供します。 コース、Android、および Windows Phone のデバイスがある標準**戻る**戻るボタンをクリックします。

モーダル ページ、ページの`Title`は表示されませんし、前のページに戻るユーザー インターフェイス要素が指定されていません。 標準の Android および Windows Phone を使用することもできます**戻る**他のプラットフォームでモーダル ページに戻るには、独自のメカニズムを提供する必要があります、前のページに戻るボタンをクリックします。

### <a name="animated-page-transitions"></a>アニメーション化されたページの切り替え効果

設定する 2 番目のブール型引数が付属してさまざまなナビゲーション方法の代替バージョン`true`ページ遷移アニメーションを含めることが必要な場合。

- [PushAsync](xref:Xamarin.Forms.INavigation.PushAsync(Xamarin.Forms.Page,System.Boolean))
- [PushModalAsync](xref:Xamarin.Forms.INavigation.PushModalAsync(Xamarin.Forms.Page,System.Boolean))
- [PopAsync](xref:Xamarin.Forms.INavigation.PopAsync(System.Boolean))
- [PopModalAsync](xref:Xamarin.Forms.INavigation.PopModalAsync(System.Boolean))

ただし、標準ページ ナビゲーション メソッドなどがあります、アニメーション、既定では (この章の最後の方で説明する) と、起動時に特定のページに移動するため貴重なのみ (で説明したように、独自の開始のアニメーションを提供するときに[**第 22 章です。アニメーション**](chapter22.md))。

### <a name="visual-and-functional-variations"></a>外観および機能のバリエーション

`NavigationPage` 内のクラスをインスタンス化するときに設定できる 2 つのプロパティが含まれています、`App`メソッド。

- [`BarBackgroundColor`](xref:Xamarin.Forms.NavigationPage.BarBackgroundColor)
- [`BarTextColor`](xref:Xamarin.Forms.NavigationPage.BarTextColor)

`NavigationPage` 4 つの接続されているバインド可能なプロパティが設定されている特定のページに影響を与えるとも含まれます。

- [`SetHasBackButton`](xref:Xamarin.Forms.NavigationPage.SetHasBackButton(Xamarin.Forms.Page,System.Boolean)) および [`GetHasBackButton`](xref:Xamarin.Forms.NavigationPage.GetHasBackButton(Xamarin.Forms.Page))
- [`SetHasNavigationBar`](xref:Xamarin.Forms.NavigationPage.SetHasNavigationBar(Xamarin.Forms.BindableObject,System.Boolean)) および [`GetHasNavigationBar`](xref:Xamarin.Forms.NavigationPage.GetHasNavigationBar(Xamarin.Forms.BindableObject))
- [`SetBackButtonTitle`](xref:Xamarin.Forms.NavigationPage.SetBackButtonTitle(Xamarin.Forms.BindableObject,System.String)) [ `GetBackButtonTitle` ](xref:Xamarin.Forms.NavigationPage.GetBackButtonTitle(Xamarin.Forms.BindableObject)) iOS のみで動作
- [`SetTitleIcon`](xref:Xamarin.Forms.NavigationPage.SetTitleIcon(Xamarin.Forms.BindableObject,Xamarin.Forms.FileImageSource)) [ `GetTitleIcon` ](xref:Xamarin.Forms.NavigationPage.GetTitleIcon(Xamarin.Forms.BindableObject))作業して iOS と Android のみ

### <a name="exploring-the-mechanics"></a>しくみの調査

ページのナビゲーション メソッドは、すべて非同期と併用する必要があります`await`します。 完了には、ページ ナビゲーションが完了したことが、ページ ナビゲーション スタックを調べて安全であることを示されません。

最初のページが一般にへの呼び出しを取得して別に移動する 1 つのページ、その[ `OnDisappearing` ](xref:Xamarin.Forms.Page.OnDisappearing)メソッド、および 2 番目のページへの呼び出しを取得します。 その[ `OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing)メソッド。 同様に、別に 1 つのページが戻るとき、最初のページは取得の呼び出しをその`OnDisappearing`メソッド、および 2 番目のページへの呼び出しを取得します通常その`OnAppearing`メソッド。 これらの呼び出し (およびナビゲーションを呼び出す非同期メソッドの完了) の順序は、プラットフォームに依存します。 上記の 2 つのステートメントに「一般」という単語の使用は、Android のモーダル ページ ナビゲーションがこれらのメソッド呼び出しが発生しないためです。

また、呼び出し、`OnAppearing`と`OnDisappearing`メソッドを示さないページ ナビゲーションとは限りません。

`INavigation`インターフェイスには、ナビゲーション スタックを調査するための 2 つのコレクション プロパティが含まれています。

- [`NavigationStack`](xref:Xamarin.Forms.INavigation.NavigationStack) 型の`IReadOnlyList<Page>`モードレス スタック
- [`ModalStack`](xref:Xamarin.Forms.INavigation.ModalStack) 型の`IReadOnlyList<Page>`モーダル スタック

これらの呼び出し履歴にアクセスする最も安全な方法は、`Navigation`のプロパティ、 `NavigationPage` (です、`App`クラスの[ `MainPage` ](xref:Xamarin.Forms.Application.MainPage)プロパティ)。 のみ、ページ ナビゲーションの非同期メソッドが完了した後、これらの呼び出し履歴を確認しても安全です。 [ `CurrentPage` ](xref:Xamarin.Forms.NavigationPage.CurrentPage)のプロパティ、`NavigationPage`現在場合は、現在のページがモーダルページを示すページが代わりにモードレスの最後のページを示していません。

[ **SinglePageNavigation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/SinglePageNavigation)サンプルでは、ページ ナビゲーションと、スタックでは、ページ ナビゲーションの有効な型を検証することができます。

- モードレスのページがモードレスの別のページまたはモーダル ページに移動できます。
- モーダル ページは別のモーダル ページにのみ移動できます。

### <a name="enforcing-modality"></a>モダリティを適用します。

アプリケーションは、ユーザーからのいくつかの情報を取得する必要がある場合に、モーダル ページを使用します。 ユーザーは、その情報が提供されるまで、前のページに返すことを禁止する必要があります。 Ios では、簡単に使用できますが、**戻る**ボタンをクリックし、ユーザーがページに完了したときにのみ有効にします。 Android および Windows Phone デバイスでは、アプリケーションをオーバーライドする必要がありますが、 [ `OnBackButtonPressed` ](xref:Xamarin.Forms.Page.OnBackButtonPressed)メソッドと戻り値`true`プログラムが処理される場合は、**戻る**で示した自体には、ボタン[ **ModalEnforcement** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ModalEnforcement)サンプル。

[ **MvvmEnforcement** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/MvvmEnforcement) MVVM シナリオではこのしくみを示します。

## <a name="navigation-variations"></a>ナビゲーションのバリエーション

ユーザーが入力する代わりに、情報を編集できるように情報を保持する必要がありますが場合は、特定のモーダル ページを複数回にナビゲートするには、すべてでもう一度です。 これは、特定のインスタンス、モーダル ページを保持することで処理できますが、ビュー モデル内の情報を保持している (iOS) では特により優れたアプローチです。

### <a name="making-a-navigation-menu"></a>ナビゲーション メニューの作成

[ **ViewGalleryType** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ViewGalleryType)サンプルの使用例、`TableView`リスト メニュー項目にします。 各項目に関連付けられた、`Type`特定ページのオブジェクト。 その項目が選択されているときに、プログラムは、ページをインスタンス化しに移動します。

[![ギャラリーの種類のビューのスクリーン ショットをトリプル](images/ch24fg21-small.png "テーブルを一覧表示するメニュー項目")](images/ch24fg21-large.png#lightbox "テーブルを一覧表示するメニュー項目")

[ **ViewGalleryInst** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ViewGalleryInst)メニューには、型ではなく各ページのインスタンスが含まれていることで、サンプルが若干異なります。 これにより、各ページの情報を保持しますが、プログラムの起動時に、すべてのページをインスタンス化する必要があります。

### <a name="manipulating-the-navigation-stack"></a>ナビゲーション スタックを操作します。

[**StackManipulation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/StackManipulation)によって定義されているいくつかの関数を示します`INavigation`ナビゲーション スタックの構造化された方法で操作することができます。

- [`RemovePage`](xref:Xamarin.Forms.INavigation.RemovePage(Xamarin.Forms.Page))
- [`InsertPageBefore`](xref:Xamarin.Forms.INavigation.InsertPageBefore(Xamarin.Forms.Page,Xamarin.Forms.Page))
- [`PopToRootAsync`](xref:Xamarin.Forms.INavigation.PopToRootAsync) [ `PopToRootAsync` ](xref:Xamarin.Forms.INavigation.PopToRootAsync(System.Boolean))省略可能なアニメーション

### <a name="dynamic-page-generation"></a>動的なページの生成

[ **BuildAPage** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/BuildAPage)ユーザー入力に基づいて実行時にページを作成することを示します。

## <a name="patterns-of-data-transfer"></a>データ転送のパターン

ページ間でデータを共有する必要があります&mdash;へ移動 ページでは、およびページを起動したページにデータを返すデータを転送します。 これを行うためのいくつかの方法はあります。

### <a name="constructor-arguments"></a>コンス トラクターの引数

新しいページに移動するととき、自体を初期化するためにページを使用するコンス トラクター引数を使用して、ページ クラスのインスタンスを作成することができます。 [ **SchoolAndStudents** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/SchoolAndStudents)のサンプルで例示します。 移動して、ページのこともその`BindingContext`ページに移動するで設定します。

### <a name="properties-and-method-calls"></a>プロパティとメソッドの呼び出し

その他のデータ転送の例では、1 つのページが別のページに移動するときに、ページとバックエンド間で情報を渡すことの問題について説明します。 これらの説明で、*ホーム*ページに移動、*情報*ページ、および初期化情報を転送する必要があります、*情報*ページ。 *情報*ページ、ユーザーから追加情報を取得およびに情報を転送する、*ホーム*ページ。

*ホーム*ページのパブリック メソッドとプロパティにアクセスできる簡単に、*情報*ページ、そのページをインスタンス化するようになります。 *情報*ページのパブリック メソッドとプロパティにアクセスできることも、*ホーム* ページが、これは複雑になるは、絶好のタイミングを選択します。 [ **DateTransfer1** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer1)このサンプルではその`OnDisappearing`をオーバーライドします。 1 つの欠点は、*情報*ページがの型を認識する必要があります、*ホーム*ページ。

### <a name="messagingcenter"></a>MessagingCenter

Xamarin.Forms [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter)クラスには、互いに通信する 2 つのページの別の方法が用意されています。 メッセージは、テキスト文字列で識別され、任意のオブジェクトを含めることができます。

特定の種類からメッセージを受信しようとしているプログラムがサブスクライブする必要がありますを使用して[ `MessagingCenter.Subscribe` ](xref:Xamarin.Forms.MessagingCenter.Subscribe*)コールバック関数を指定します。 後で呼び出すことによってアンサブスク ライブできます[ `MessagingCenter.Unsubscribe`](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*)します。 コールバック関数を経由して送信する指定の名前を持つ指定した型から送信されたメッセージの受信、 [ `Send` ](xref:Xamarin.Forms.MessagingCenter.Send*)メソッド。

[ **DateTransfer2** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer2)プログラムは、メッセージング center を使用してデータを転送する方法を示しますが、もう一度このことが必要、*情報*ページ、の型を認識する*ホーム*ページ。

### <a name="events"></a>イベント

イベントは、そのクラスの型を知らなくても、別のクラスに情報を送信する 1 つのクラスの伝統的なアプローチです。 [ **DateTransfer3** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer3)サンプル、*情報*クラスについては、準備ができたら、起動するイベントを定義します。 ただし、便利な場所はありません、*ホーム*イベント ハンドラーをデタッチするページ。

### <a name="the-app-class-intermediary"></a>App クラスの中継ぎ局

[ **DateTransfer4** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer4)サンプルで定義されたプロパティにアクセスする方法を示しています、`App`両方によってクラス、*ホーム*ページと*情報*ページ。 これは優れたソリューションですが、次のセクションでは、もっと優れたについて説明します。

### <a name="switching-to-a-viewmodel"></a>ViewModel に切り替える

情報を ViewModel を使用して、*ホーム*ページと*情報*情報クラスのインスタンスを共有するページ。 これは、方法については、 [ **DateTransfer5** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer5)サンプル。

### <a name="saving-and-restoring-page-state"></a>保存とページの状態の復元

`App`クラス中継ぎ局または ViewModel アプローチが最も適しているプログラムは、スリープ状態になる場合、アプリケーションで情報を保存する必要があります中に、*情報*ページがアクティブです。 [ **DateTransfer6** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer6)のサンプルで例示します。

## <a name="saving-and-restoring-the-navigation-stack"></a>保存と復元、ナビゲーション スタック

一般的なケースでは、スリープ状態になるマルチページ プログラムする必要がありますが復旧し次第、同じページに移動します。 つまり、このようなプログラムがナビゲーション スタックの内容を保存する必要があります。 このセクションでは、この目的で設計されたクラスでは、このプロセスを自動化する方法を示します。 このクラスは、個々 のページを保存し、そのページの状態を復元できるようにも呼び出します。

[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリという名前のインターフェイスを定義する[ `IPersistantPage` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/IPersistentPage.cs)クラスは、保存し、復元、内の項目を実装できます`Properties`ディクショナリ。

[ `MultiPageRestorableApp` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MultiPageRestorableApp.cs)クラス、 **Xamarin.FormsBook.Toolkit**から派生したライブラリ`Application`します。 導き、`App`クラス`MultiPageRestorableApp`一部ハウスキーピングを実行します。

[ **StackRestoreDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/StackRestoreDemo)の使用方法を示します`MultiPageRestorableApp`します。

### <a name="something-like-a-real-life-app"></a>以下のような実際のアプリ

[ **NoteTaker** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/NoteTaker)サンプルも使用`MultiPageRestorableApp`入力と編集のノートに保存されていることができ、`Properties`ディクショナリ。



## <a name="related-links"></a>関連リンク

- [第 24 章フル テキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch24-Apr2016.pdf)
- [第 24 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24)
- [階層ナビゲーション](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)
- [モーダル ページ](~/xamarin-forms/app-fundamentals/navigation/modal.md)
