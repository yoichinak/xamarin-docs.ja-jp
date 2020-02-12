---
title: 第 24 章の概要です。 ページのナビゲーション
description: 'Xamarin.Forms によるモバイル アプリの作成: 第 24 章の概要。 ページのナビゲーション'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: DDCDB49C-6008-4F72-B095-463EE21D7C23
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: fd8e4fc77917fcba9bc61e59ced714ac1cd6fbe9
ms.sourcegitcommit: ccbf914615c0ce6b3f308d930f7a77418aeb4dbc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/11/2020
ms.locfileid: "77130839"
---
# <a name="summary-of-chapter-24-page-navigation"></a>第 24 章の概要です。 ページのナビゲーション

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24)

多くのアプリケーションは、ユーザーが移動する複数のページで構成されます。 アプリケーションには、常に*メイン*ページまたは*ホーム*ページがあり、ユーザーは他のページに移動します。他のページに移動するには、スタックに保持されています。 その他のナビゲーションオプションについては、25章で説明されてい[**ます。ページの種類**](chapter25.md)。

## <a name="modal-pages-and-modeless-pages"></a>ページのモーダルとモードレスのページ

`VisualElement` は[`INavigation`](xref:Xamarin.Forms.INavigation)型の[`Navigation`](xref:Xamarin.Forms.NavigableElement.Navigation)プロパティを定義します。このプロパティには、新しいページに移動するための次の2つのメソッドが含まれています。

- [`PushAsync`](xref:Xamarin.Forms.INavigation.PushAsync(Xamarin.Forms.Page))
- [`PushModalAsync`](xref:Xamarin.Forms.INavigation.PushModalAsync(Xamarin.Forms.Page))

どちらのメソッドも、引数として `Page` インスタンスを受け取り、`Task` オブジェクトを返します。 次の 2 つのメソッドは、前のページに移動します。

- [`PopAsync`](xref:Xamarin.Forms.INavigation.PopAsync)
- [`PopModalAsync`](xref:Xamarin.Forms.INavigation.PopModalAsync)

ユーザーインターフェイスに独自の **[戻る]** ボタン (Android と Windows phone の場合) がある場合は、アプリケーションでこれらのメソッドを呼び出す必要はありません。

これらのメソッドはどの `VisualElement`からでも使用できますが、通常は、現在の `Page` インスタンスの `Navigation` プロパティから呼び出されます。

前のページに戻る前に、ページにいくつかの情報を提供する、ユーザーが必要な場合に、アプリケーションは一般にモーダル ページを使用します。 モーダルでないページは、モードレスまたは*階層*と呼ばれることもあります。 モーダルまたはモードレス; として区別ページ自体ではありません。これは、そこに移動するために使用するメソッドによって、代わりに制御されます。 すべてのプラットフォームで作業を行う、モーダル ページは、前のページに戻るの独自のユーザー インターフェイスを提供する必要があります。

[**Modeとモーダル**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ModelessAndModal)のサンプルを使用すると、モードレスページとモーダルページの違いを調べることができます。 ページナビゲーションを使用するアプリケーションは、そのホームページを[`NavigationPage`](xref:Xamarin.Forms.NavigationPage)コンストラクター (通常はプログラムの `App` クラス) に渡す必要があります。 1つの利点は、iOS のページで `Padding` を設定する必要がなくなったことです。

モードレスページでは、ページの[`Title`](xref:Xamarin.Forms.Page.Title)プロパティが表示されていることがわかります。 IOS、Android、および Windows タブレットとデスクトップ プラットフォームは、前のページに戻るユーザー インターフェイス要素を提供します。 もちろん、Android デバイスと Windows phone デバイスには、元に戻すための標準の **[戻る]** ボタンがあります。

モーダルページの場合、ページ `Title` は表示されず、前のページに戻るためのユーザーインターフェイス要素は提供されません。 前のページに戻るには、Android および Windows phone の標準の **[戻る]** ボタンを使用できますが、他のプラットフォームのモーダルページでは、元に戻すための独自のメカニズムが用意されている必要があります。

### <a name="animated-page-transitions"></a>アニメーション化されたページの切り替え効果

さまざまなナビゲーションメソッドの代替バージョンは、ページ切り替えにアニメーションを含める場合に `true` に設定する2番目のブール値引数を使用して提供されます。

- [PushAsync](xref:Xamarin.Forms.INavigation.PushAsync(Xamarin.Forms.Page,System.Boolean))
- [PushModalAsync](xref:Xamarin.Forms.INavigation.PushModalAsync(Xamarin.Forms.Page,System.Boolean))
- [PopAsync](xref:Xamarin.Forms.INavigation.PopAsync(System.Boolean))
- [PopModalAsync](xref:Xamarin.Forms.INavigation.PopModalAsync(System.Boolean))

ただし、標準のページナビゲーションメソッドには既定でアニメーションが含まれているため、これらは起動時に特定のページに移動する場合 (この章の末尾について説明します)、または独自の開始アニメーションを提供する場合 (Chapter22 で説明します) にのみ役立ち[**ます。アニメーション**](chapter22.md))。

### <a name="visual-and-functional-variations"></a>外観および機能のバリエーション

`NavigationPage` には、`App` メソッドでクラスをインスタンス化するときに設定できる2つのプロパティが含まれています。

- [`BarBackgroundColor`](xref:Xamarin.Forms.NavigationPage.BarBackgroundColor)
- [`BarTextColor`](xref:Xamarin.Forms.NavigationPage.BarTextColor)

`NavigationPage` には、設定されている特定のページに影響を与える、アタッチ可能な4つのバインド可能なプロパティも含まれています。

- [`SetHasBackButton`](xref:Xamarin.Forms.NavigationPage.SetHasBackButton(Xamarin.Forms.Page,System.Boolean)) および [`GetHasBackButton`](xref:Xamarin.Forms.NavigationPage.GetHasBackButton(Xamarin.Forms.Page))
- [`SetHasNavigationBar`](xref:Xamarin.Forms.NavigationPage.SetHasNavigationBar(Xamarin.Forms.BindableObject,System.Boolean)) および [`GetHasNavigationBar`](xref:Xamarin.Forms.NavigationPage.GetHasNavigationBar(Xamarin.Forms.BindableObject))
- [`SetBackButtonTitle`](xref:Xamarin.Forms.NavigationPage.SetBackButtonTitle(Xamarin.Forms.BindableObject,System.String))と[`GetBackButtonTitle`](xref:Xamarin.Forms.NavigationPage.GetBackButtonTitle(Xamarin.Forms.BindableObject)) iOS での作業のみ
- [`SetTitleIcon`](xref:Xamarin.Forms.NavigationPage.SetTitleIcon(Xamarin.Forms.BindableObject,Xamarin.Forms.FileImageSource))と[`GetTitleIcon`](xref:Xamarin.Forms.NavigationPage.GetTitleIcon(Xamarin.Forms.BindableObject)) iOS と Android でのみ機能する

### <a name="exploring-the-mechanics"></a>しくみの調査

ページナビゲーションメソッドはすべて非同期であり、`await`で使用する必要があります。 完了には、ページ ナビゲーションが完了したことが、ページ ナビゲーション スタックを調べて安全であることを示されません。

あるページが別のページに移動すると、最初のページは通常[`OnDisappearing`](xref:Xamarin.Forms.Page.OnDisappearing)メソッドへの呼び出しを取得し、2番目のページはその[`OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing)メソッドの呼び出しを取得します。 同様に、あるページが別のページに戻ったとき、最初のページはその `OnDisappearing` メソッドへの呼び出しを取得し、2番目のページは通常、`OnAppearing` メソッドの呼び出しを取得します。 これらの呼び出し (およびナビゲーションを呼び出す非同期メソッドの完了) の順序は、プラットフォームに依存します。 上記の 2 つのステートメントに「一般」という単語の使用は、Android のモーダル ページ ナビゲーションがこれらのメソッド呼び出しが発生しないためです。

また、`OnAppearing` および `OnDisappearing` メソッドの呼び出しは、必ずしもページナビゲーションを示すとは限りません。

`INavigation` インターフェイスには、ナビゲーションスタックを調べることができる2つのコレクションプロパティが含まれています。

- モードレススタックの `IReadOnlyList<Page>` 型の[`NavigationStack`](xref:Xamarin.Forms.INavigation.NavigationStack)
- モーダルスタックの型 `IReadOnlyList<Page>` の[`ModalStack`](xref:Xamarin.Forms.INavigation.ModalStack)

これらのスタックには、`NavigationPage` の `Navigation` プロパティ (`App` クラスの[`MainPage`](xref:Xamarin.Forms.Application.MainPage)プロパティ) からアクセスするのが安全です。 のみ、ページ ナビゲーションの非同期メソッドが完了した後、これらの呼び出し履歴を確認しても安全です。 現在のページがモーダルページである場合、`NavigationPage` の[`CurrentPage`](xref:Xamarin.Forms.NavigationPage.CurrentPage)プロパティは現在のページを示しませんが、代わりに最後のモードレスページであることを示します。

[**SinglePageNavigation**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/SinglePageNavigation)サンプルでは、ページナビゲーションとスタック、およびページナビゲーションの法的な種類を調べることができます。

- モードレスのページがモードレスの別のページまたはモーダル ページに移動できます。
- モーダル ページは別のモーダル ページにのみ移動できます。

### <a name="enforcing-modality"></a>モダリティを適用します。

アプリケーションは、ユーザーからのいくつかの情報を取得する必要がある場合に、モーダル ページを使用します。 ユーザーは、その情報が提供されるまで、前のページに返すことを禁止する必要があります。 IOS では、 **[戻る]** ボタンを簡単に提供し、ユーザーがページで終了した場合にのみ有効にすることができます。 ただし、Android および Windows phone デバイスの場合は、 [**ModalEnforcement**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ModalEnforcement)サンプルで説明されているように、アプリケーションが[`OnBackButtonPressed`](xref:Xamarin.Forms.Page.OnBackButtonPressed)メソッドをオーバーライドし、 **`true` を返す**必要があります。

[**MvvmEnforcement**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/MvvmEnforcement)サンプルでは、MVVM シナリオでのこの動作を示します。

## <a name="navigation-variations"></a>ナビゲーションのバリエーション

ユーザーが入力する代わりに、情報を編集できるように情報を保持する必要がありますが場合は、特定のモーダル ページを複数回にナビゲートするには、すべてでもう一度です。 これは、特定のインスタンス、モーダル ページを保持することで処理できますが、ビュー モデル内の情報を保持している (iOS) では特により優れたアプローチです。

### <a name="making-a-navigation-menu"></a>ナビゲーション メニューの作成

[**ViewGalleryType**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ViewGalleryType)サンプルでは、`TableView` を使用してメニュー項目を一覧表示する方法を示します。 各項目は、特定のページの `Type` オブジェクトに関連付けられています。 その項目が選択されているときに、プログラムは、ページをインスタンス化しに移動します。

[![ビューギャラリーの種類のトリプルスクリーンショット](images/ch24fg21-small.png "TableView リストのメニュー項目")](images/ch24fg21-large.png#lightbox "TableView リストのメニュー項目")

[**ViewGalleryInst**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ViewGalleryInst)サンプルは、メニューには型ではなく各ページのインスタンスが含まれているので、少し異なります。 これにより、各ページの情報を保持しますが、プログラムの起動時に、すべてのページをインスタンス化する必要があります。

### <a name="manipulating-the-navigation-stack"></a>ナビゲーション スタックを操作します。

[**Stackmanipulation**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/StackManipulation)は、`INavigation` によって定義されたいくつかの関数を示しています。これにより、ナビゲーションスタックを構造化された方法で操作できます。

- [`RemovePage`](xref:Xamarin.Forms.INavigation.RemovePage(Xamarin.Forms.Page))
- [`InsertPageBefore`](xref:Xamarin.Forms.INavigation.InsertPageBefore(Xamarin.Forms.Page,Xamarin.Forms.Page))
- オプションのアニメーションを使用した[`PopToRootAsync`](xref:Xamarin.Forms.INavigation.PopToRootAsync)と[`PopToRootAsync`](xref:Xamarin.Forms.INavigation.PopToRootAsync(System.Boolean))

### <a name="dynamic-page-generation"></a>動的なページの生成

[**Buildapage**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/BuildAPage)サンプルは、ユーザー入力に基づいて実行時にページを構築する方法を示しています。

## <a name="patterns-of-data-transfer"></a>データ転送のパターン

移動ページにデータを転送するために &mdash; ページ間でデータを共有し、ページを呼び出したページにデータを返すことが必要になることがよくあります。 これを行うためのいくつかの方法はあります。

### <a name="constructor-arguments"></a>コンス トラクターの引数

新しいページに移動するととき、自体を初期化するためにページを使用するコンス トラクター引数を使用して、ページ クラスのインスタンスを作成することができます。 [**SchoolAndStudents**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/SchoolAndStudents)サンプルでは、これを示します。 移動先のページでは、移動先のページで `BindingContext` 設定することもできます。

### <a name="properties-and-method-calls"></a>プロパティとメソッドの呼び出し

その他のデータ転送の例では、1 つのページが別のページに移動するときに、ページとバックエンド間で情報を渡すことの問題について説明します。 これらのディスカッションでは、*ホーム*ページが*情報*ページに移動し、初期化された情報を*情報*ページに転送する必要があります。 情報*ページでは、* ユーザーから追加情報を取得し、その情報を*ホーム*ページに転送します。

*ホーム*ページでは、そのページがインスタンス化されるとすぐに、[*情報*] ページのパブリックメソッドとプロパティに簡単にアクセスできます。 また、[*情報*] ページでは、*ホーム*ページのパブリックメソッドとプロパティにアクセスすることもできますが、そのためには適切な時間を選択するのが難しい場合があります。 [**DateTransfer1**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer1)サンプルでは、`OnDisappearing` オーバーライドでこれを行います。 欠点の1つは、*情報*ページが*ホーム*ページの種類を知る必要があることです。

### <a name="messagingcenter"></a>MessagingCenter

Xamarin [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter)クラスは、2つのページが互いに通信するための別の方法を提供します。 メッセージは、テキスト文字列で識別され、任意のオブジェクトを含めることができます。

特定の型からメッセージを受信するプログラムでは、 [`MessagingCenter.Subscribe`](xref:Xamarin.Forms.MessagingCenter.Subscribe*)を使用してメッセージをサブスクライブし、コールバック関数を指定する必要があります。 後で[`MessagingCenter.Unsubscribe`](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*)を呼び出すことによって、サブスクライブを解除できます。 コールバック関数は、 [`Send`](xref:Xamarin.Forms.MessagingCenter.Send*)メソッドを介して送信された指定された名前を使用して、指定された型から送信されるすべてのメッセージを受信します。

[**DateTransfer2**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer2)プログラムでは、メッセージングセンターを使用してデータを転送する方法を示していますが、この場合も、*ホーム*ページの種類が*情報*ページで認識されている必要があります。

### <a name="events"></a>events

イベントは、そのクラスの型を知らなくても、別のクラスに情報を送信する 1 つのクラスの伝統的なアプローチです。 [**DateTransfer3**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer3)サンプルでは、 *info*クラスは、情報の準備が整ったときに発生するイベントを定義します。 ただし、*ホーム*ページでイベントハンドラーをデタッチするのに便利な場所はありません。

### <a name="the-app-class-intermediary"></a>App クラスの中継ぎ局

[**DateTransfer4**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer4)サンプルでは、*ホーム*ページと*情報*ページの両方で `App` クラスで定義されているプロパティにアクセスする方法を示します。 これは優れたソリューションですが、次のセクションでは、もっと優れたについて説明します。

### <a name="switching-to-a-viewmodel"></a>ViewModel に切り替える

情報にビューモデルを使用すると、*ホーム*ページと*情報ページで*information クラスのインスタンスを共有できるようになります。 これは、 [**DateTransfer5**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer5)サンプルに示されています。

### <a name="saving-and-restoring-page-state"></a>保存とページの状態の復元

`App` クラスの中継点またはビューモデルのアプローチは、*情報*ページがアクティブになっている間にプログラムがスリープ状態になった場合に、アプリケーションで情報を保存する必要がある場合に最適です。 [**DateTransfer6**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer6)サンプルでは、これを示します。

## <a name="saving-and-restoring-the-navigation-stack"></a>保存と復元、ナビゲーション スタック

一般的なケースでは、スリープ状態になるマルチページ プログラムする必要がありますが復旧し次第、同じページに移動します。 つまり、このようなプログラムがナビゲーション スタックの内容を保存する必要があります。 このセクションでは、この目的で設計されたクラスでは、このプロセスを自動化する方法を示します。 このクラスは、個々 のページを保存し、そのページの状態を復元できるようにも呼び出します。

[**Xamarin. ツールキット**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリでは、クラスが `Properties` 辞書の項目を保存および復元するために実装できる[`IPersistantPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/IPersistentPage.cs)という名前のインターフェイスが定義されています。

[`MultiPageRestorableApp`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MultiPageRestorableApp.cs)クラスは、`Application`から派生してい**ます。** その後、`MultiPageRestorableApp` から `App` クラスを派生させ、いくつかのハウスキーピングを実行できます。

[**Stackrestoredemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/StackRestoreDemo)は、`MultiPageRestorableApp`の使用方法を示しています。

### <a name="something-like-a-real-life-app"></a>以下のような実際のアプリ

また[ **、この例で**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/NoteTaker)は、`MultiPageRestorableApp` を利用し、`Properties` 辞書に保存されているメモを入力および編集することもできます。

## <a name="related-links"></a>関連リンク

- [第24章フルテキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch24-Apr2016.pdf)
- [第24章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24)
- [階層ナビゲーション](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)
- [モーダル ページ](~/xamarin-forms/app-fundamentals/navigation/modal.md)
