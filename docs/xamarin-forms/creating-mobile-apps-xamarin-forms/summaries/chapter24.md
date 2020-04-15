---
title: '第 24 章の概要: ページのナビゲーション'
description: 'Xamarin.Forms でモバイル アプリを作成する:第 24 章の概要: ページのナビゲーション'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: DDCDB49C-6008-4F72-B095-463EE21D7C23
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: fd8e4fc77917fcba9bc61e59ced714ac1cd6fbe9
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "77130839"
---
# <a name="summary-of-chapter-24-page-navigation"></a>第 24 章の概要: ページのナビゲーション

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24)

多くのアプリケーションは、ユーザーが移動する複数のページで構成されています。 アプリケーションには、必ず *main* ページまたは *home* ページがあり、元の場所に戻るためにスタックに保持されている他のページに、ユーザーはそのページから移動します。 その他のナビゲーション オプションについては、「[**第 25 章:ページの変数**](chapter25.md)」を参照してください。

## <a name="modal-pages-and-modeless-pages"></a>モーダル ページとモードレス ページ

`VisualElement` には、[`INavigation`](xref:Xamarin.Forms.INavigation) 型の [`Navigation`](xref:Xamarin.Forms.NavigableElement.Navigation) プロパティが定義されています。これには、新しいページに移動するための以下の 2 つのメソッドが含まれます。

- [`PushAsync`](xref:Xamarin.Forms.INavigation.PushAsync(Xamarin.Forms.Page))
- [`PushModalAsync`](xref:Xamarin.Forms.INavigation.PushModalAsync(Xamarin.Forms.Page))

いずれのメソッドも、引数として `Page` インスタンスを取り、`Task` オブジェクトを返します。 以下の 2 つのメソッドでは、前のページに戻ります。

- [`PopAsync`](xref:Xamarin.Forms.INavigation.PopAsync)
- [`PopModalAsync`](xref:Xamarin.Forms.INavigation.PopModalAsync)

(Android と Windows Phone の場合のように) ユーザー インターフェイスに独自の **[戻る]** ボタンがある場合、アプリケーションがこれらのメソッドを呼び出す必要はありません。

これらのメソッドはすべての `VisualElement` から使用できますが、通常は、現在の `Page` インスタンスの `Navigation` プロパティから呼び出します。

ユーザーが前のページに戻る前にページに情報を指定する必要がある場合、アプリケーションでは通常モーダル ページを使用します。 モーダルではないページは、モードレスまたは "*階層*" と呼ばれます。 ページ自体には、それがモーダルであるか、モードレスであるかを区別するものは含まれません。代わりに、それへの移動に使用されるメソッドによって管理されています。 モーダル ページがすべてのプラットフォームで動作するには、前のページに戻る独自のユーザー インターフェイスを備えている必要があります。

[**ModelessAndModal**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ModelessAndModal) サンプルでは、モードレス ページとモーダル ページの違いを確認できます。 ページ ナビゲーションを使用するすべてのアプリケーションでは、プログラムの `App` クラスに一般的に含まれる [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) コンストラクターにそのホーム ページを渡す必要があります。 これの利点の 1 つとして、iOS のページでは `Padding` を設定する必要がありません。

これはモードレス ページで、ページの [`Title`](xref:Xamarin.Forms.Page.Title) プロパティが表示されることからわかります。 iOS、Android、Windows タブレットおよびデスクトップのすべてのプラットフォームに、前のページに戻るユーザー インターフェイス要素が用意されています。 Android デバイスと Windows Phone デバイスにももちろん、元に戻るための **[戻る]** ボタンがあります。

モーダル ページでは、ページ `Title` が表示されず、前のページに戻るユーザー インターフェイス要素がありません。 Android と Windows Phone では標準の **[戻る]** ボタンを使用して前のページに戻れますが、他のプラットフォームではモーダル ページに、元に戻るための独自のメカニズムを用意する必要があります。

### <a name="animated-page-transitions"></a>アニメーションでのページ切り替え

ページの切り替えにアニメーションを含めたい場合に、さまざまなナビゲーション メソッドの別のバージョンを使用するために使用する 2 番目のブール値引数を `true` に設定する必要があります。

- [PushAsync](xref:Xamarin.Forms.INavigation.PushAsync(Xamarin.Forms.Page,System.Boolean))
- [PushModalAsync](xref:Xamarin.Forms.INavigation.PushModalAsync(Xamarin.Forms.Page,System.Boolean))
- [PopAsync](xref:Xamarin.Forms.INavigation.PopAsync(System.Boolean))
- [PopModalAsync](xref:Xamarin.Forms.INavigation.PopModalAsync(System.Boolean))

ただし、標準のページナビゲーション メソッドにはアニメーションが既定で含まれているため、起動時に特定のページに移動する場合 (この章の終わり近くを参照) または独自の開始アニメーションを提供する場合 (「[**第 22 章:アニメーション**](chapter22.md)」を参照) のみ、これらは必要です。

### <a name="visual-and-functional-variations"></a>ビジュアルと機能のバリエーション

`NavigationPage` には、お使いの `App` メソッドでクラスをインスタンス化するときに設定できる以下の 2 つのプロパティがあります。

- [`BarBackgroundColor`](xref:Xamarin.Forms.NavigationPage.BarBackgroundColor)
- [`BarTextColor`](xref:Xamarin.Forms.NavigationPage.BarTextColor)

`NavigationPage` には、それらが設定されている特定のページに影響する、以下の 4 つの接続されている連結可能なプロパティもあります。

- [`SetHasBackButton`](xref:Xamarin.Forms.NavigationPage.SetHasBackButton(Xamarin.Forms.Page,System.Boolean)) および [`GetHasBackButton`](xref:Xamarin.Forms.NavigationPage.GetHasBackButton(Xamarin.Forms.Page))
- [`SetHasNavigationBar`](xref:Xamarin.Forms.NavigationPage.SetHasNavigationBar(Xamarin.Forms.BindableObject,System.Boolean)) および [`GetHasNavigationBar`](xref:Xamarin.Forms.NavigationPage.GetHasNavigationBar(Xamarin.Forms.BindableObject))
- [`SetBackButtonTitle`](xref:Xamarin.Forms.NavigationPage.SetBackButtonTitle(Xamarin.Forms.BindableObject,System.String)) および [`GetBackButtonTitle`](xref:Xamarin.Forms.NavigationPage.GetBackButtonTitle(Xamarin.Forms.BindableObject)) は iOS のみで動作
- [`SetTitleIcon`](xref:Xamarin.Forms.NavigationPage.SetTitleIcon(Xamarin.Forms.BindableObject,Xamarin.Forms.FileImageSource)) および [`GetTitleIcon`](xref:Xamarin.Forms.NavigationPage.GetTitleIcon(Xamarin.Forms.BindableObject)) は iOS と Android のみで動作

### <a name="exploring-the-mechanics"></a>機構を調べる

ページ ナビゲーション メソッドはすべて非同期であり、`await` と共に使用される必要があります。 完了はページ ナビゲーションの完了ではなく、ページ ナビゲーション スタックを確認するのが安全であるということのみを意味します。

あるページから別のページに移動する場合、通常、最初のページではその [`OnDisappearing`](xref:Xamarin.Forms.Page.OnDisappearing) メソッドに対する呼び出しが取得され、2 番目のページでは [`OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing) メソッドに対する呼び出しが取得されます。 同様に、あるページから別のページに戻る場合、最初のページではその `OnDisappearing` メソッドに対する呼び出しが取得され、2 番目のページでは通常、`OnAppearing` メソッドに対する呼び出しが取得されます。 これらの呼び出しの順番 (およびナビゲーションを呼び出す非同期メソッドの完了) は、プラットフォームによって異なります。 前の 2 つの文で "通常" という語を使用しているのは、Android モーダルページ ナビゲーションではこれらのメソッド呼び出しが発生しないためです。

また、`OnAppearing` および `OnDisappearing` のメソッド呼び出しは、必ずしもページ ナビゲーションを意味しません。

`INavigation` インターフェイスには、ナビゲーション スタックを確認できる、以下の 2 つのコレクション プロパティがあります。

- モードレス スタック用の `IReadOnlyList<Page>` 型の [`NavigationStack`](xref:Xamarin.Forms.INavigation.NavigationStack)
- モダール スタック用の `IReadOnlyList<Page>` 型の [`ModalStack`](xref:Xamarin.Forms.INavigation.ModalStack)

これらのスタックには、`NavigationPage` (`App` クラスの [`MainPage`](xref:Xamarin.Forms.Application.MainPage) プロパティ) の `Navigation` プロパティからアクセスするのが最も安全です。 これらのスタックの確認は、非同期のページ ナビゲーション メソッドが完了した後のみ安全です。 現在のページがモーダル ページである場合、`NavigationPage` の [`CurrentPage`](xref:Xamarin.Forms.NavigationPage.CurrentPage) プロパティはそれが現在のページではなく、最後のモードレス ページであることを示します。

[**SinglePageNavigation**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/SinglePageNavigation) サンプルでは、ページ ナビゲーションとそのスタック、およびページ ナビゲーションの正当な種類を調べることができます。

- モードレス ページでは、別のモードレス ページまたはモーダル ページに移動できる
- モーダル ページでは、別のモーダル ページにのみ移動できる

### <a name="enforcing-modality"></a>モダリティの適用

アプリケーションでは、ユーザーからいくつかの情報を取得する必要がある場合に、モーダル ページが使用されます。 ユーザーは、その情報が提供されるまで、前のページに戻ることはできません。 iOS では、ユーザーがそのページを終了した場合にのみ **[戻る]** ボタンを用意して有効にすることが簡単にできます。 しかし、Android デバイスと Windows Phone デバイスでは、プログラムで **[戻る]** ボタン自体が処理された場合、アプリケーションが [`OnBackButtonPressed`](xref:Xamarin.Forms.Page.OnBackButtonPressed) メソッドをオーバーライドし、`true` を返す必要があります ([**ModalEnforcement**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ModalEnforcement) のサンプルを参照)。

[**MvvmEnforcement**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/MvvmEnforcement) サンプルでは、この動作を MVVM シナリオで示しています。

## <a name="navigation-variations"></a>ナビゲーションの種類

特定のモーダル ページに複数回移動できる場合、ユーザーがそこにすべての情報を再入力するのではなく、そこで情報を編集できるように、情報が保持されている必要があります。 これは、モーダル ページの特定のインスタンスを保持することで対応できますが、(特に iOS では) ビュー モデルで情報を保持した方がより便利です。

### <a name="making-a-navigation-menu"></a>ナビゲーション メニューの作成

[**ViewGalleryType**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ViewGalleryType) サンプルでは、`TableView` を使用してメニュー項目が一覧表示されています。 各項目には、特定のページの `Type` オブジェクトが関連付けられています。 その項目が選択されると、プログラムがそのページをインスタンス化し、それに移動できます。

[![View Gallery Type のトリプル スクリーンショット](images/ch24fg21-small.png "メニュー項目が一覧されている TableView")](images/ch24fg21-large.png#lightbox "メニュー項目が一覧されている TableView")

[**ViewGalleryInst**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ViewGalleryInst) サンプルは、型ではなくメニューに各ページのインスタンスが含まれている点で、少し異なります。 これは各ページの情報を保持するのに役立ちますが、すべてのページがプログラムの起動時にインスタンス化される必要があります。

### <a name="manipulating-the-navigation-stack"></a>ナビゲーション スタックの操作

[**StackManipulation**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/StackManipulation) では、`INavigation` で定義されたナビゲーション スタックを構造化して操作するためのいくつかの関数が示されています。

- [`RemovePage`](xref:Xamarin.Forms.INavigation.RemovePage(Xamarin.Forms.Page))
- [`InsertPageBefore`](xref:Xamarin.Forms.INavigation.InsertPageBefore(Xamarin.Forms.Page,Xamarin.Forms.Page))
- オプションでアニメーションを使用する [`PopToRootAsync`](xref:Xamarin.Forms.INavigation.PopToRootAsync) および [`PopToRootAsync`](xref:Xamarin.Forms.INavigation.PopToRootAsync(System.Boolean))

### <a name="dynamic-page-generation"></a>ページの動的生成

[**BuildAPage**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/BuildAPage) サンプルでは、ユーザー入力に基づいて実行時にページを構築する方法を示しています。

## <a name="patterns-of-data-transfer"></a>データ転送のパターン

移動先のページにデータを移動したり、そのページを呼び出したページにデータを戻すために、ページ間ではしばしばデータが共有される必要があります。 これを行うには、いくつかの方法があります。

### <a name="constructor-arguments"></a>コンストラクターの引数

新しいページに移動する場合、ページ自体の初期化を許可するコンストラクター引数を使用してページ クラスをインスタンス化できます。 この方法は、[**SchoolAndStudents**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/SchoolAndStudents) サンプルで示されています。 移動先のページの `BindingContext` をそれの移動元のページで設定することもできます。

### <a name="properties-and-method-calls"></a>プロパティとメソッド呼び出し

残りのデータ転送の例では、あるページから別のページに移動して戻るとき、ページ間で情報を渡す問題について説明します。 これらの説明では、*home* ページから *info* ページに移動し、*info* ページに初期化された情報が転送される必要があります。 *info* ページでは、ユーザーから追加情報を取得し、その情報を *home* ページに転送します。

*home* ページでは、そのページがインスタンス化されるとすぐに *info* ページのパブリック メソッドとプロパティに簡単にアクセスできるようになります。 *info* ページでも、*home* ページのパブリック メソッドとプロパティにアクセスできますが、それの適切なタイミングの選択が困難です。 [**DateTransfer1**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer1) サンプルでは、`OnDisappearing` のオーバーライドでこれを行います。 1 つの欠点として、*info* ページで *home* ページの型が認識されている必要があります。

### <a name="messagingcenter"></a>MessagingCenter

Xamarin.Forms [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) クラスでは、別の方法で 2 つのページ間で相互通信することが可能です。 メッセージはテキスト文字列で識別され、任意のオブジェクトと共に使用されます。

特定の型からメッセージを受け取るプログラムでは、[`MessagingCenter.Subscribe`](xref:Xamarin.Forms.MessagingCenter.Subscribe*) を使用してそれにサブスクライブし、コールバック関数を指定する必要があります。 サブスクライブを解除するには、後で [`MessagingCenter.Unsubscribe`](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*) を呼び出します。 コールバック関数は、[`Send`](xref:Xamarin.Forms.MessagingCenter.Send*) メソッドを介して送信された指定済みの名前を使用する、指定された型から送信されたすべてのメッセージを受信します。

[**DateTransfer2**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer2) プログラムでは、メッセージング センターを使用してデータを転送する方法を示していますが、この場合も、*info* ページが *home* ページの型を認識している必要があります。

### <a name="events"></a>イベント

イベントとは、あるクラスが型がわからない別のクラスに情報を送信するための、従来からの手法です。 [**DateTransfer3**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer3) のサンプルの *info* クラスでは、情報の準備が整ったときに呼び出されるイベントが定義されています。 ただし、*home* ページがイベント ハンドラーをデタッチするのに適切な場所がありません。

### <a name="the-app-class-intermediary"></a>App クラスでの中継

[**DateTransfer4**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer4) サンプルでは、*home* ページと *info* ページの両方で `App` クラスに定義されているプロパティにアクセスする方法を示しています。 これは適切な解決策ですが、以下のセクションではさらに適切なものを説明しています。

### <a name="switching-to-a-viewmodel"></a>ViewModel への切り替え

情報に ViewModel を使用すると、*home* ページと *info* ページで情報クラスのインスタンスを共有できます。 これは、[**DateTransfer5**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer5) サンプルに示されています。

### <a name="saving-and-restoring-page-state"></a>ページの状態の保存と復元

`App` クラスでの中継または ViewModel の手法は、*info* ページがアクティブな間にプログラムがスリープ状態になるアプリケーションで、情報を保存する必要がある場合に最適です。 これは、[**DateTransfer6**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer6) サンプルで示されています。

## <a name="saving-and-restoring-the-navigation-stack"></a>ナビゲーション スタックの保存と復元

スリープ状態になる複数ページのプログラムでは、一般的に復元時に同じページに移動する必要があります。 つまり、このようなプログラムでは、ナビゲーション スタックの内容が保存される必要があります。 このセクションでは、この目的のために設計されたクラスで、このプロセスを自動化する方法について説明します。 また、このクラスでは個々のページを呼び出して、それらのページの状態を保存および復元できるようにします。

[**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) ライブラリには、`Properties` ディクショナリに項目を保存および復元するためにクラスで実装できる [`IPersistantPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/IPersistentPage.cs) という名前のインターフェイスが定義されています。

**Xamarin.FormsBook.Toolkit** ライブラリの [`MultiPageRestorableApp`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MultiPageRestorableApp.cs) クラスは `Application` から派生しています。 その後、`MultiPageRestorableApp` から使用する `App` クラスを派生させ、いくつかのハウスキープ処理を実行できます。

[**StackRestoreDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/StackRestoreDemo) では、`MultiPageRestorableApp` の使用方法を示しています。

### <a name="something-like-a-real-life-app"></a>実在しているようなアプリ

[**NoteTaker**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/NoteTaker) サンプルでも `MultiPageRestorableApp` を使用して、`Properties` ディクショナリに保存されているメモを入力および編集できます。

## <a name="related-links"></a>関連リンク

- [第 24 章の全文 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch24-Apr2016.pdf)
- [第 24 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24)
- [階層ナビゲーション](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)
- [モーダル ページ](~/xamarin-forms/app-fundamentals/navigation/modal.md)
