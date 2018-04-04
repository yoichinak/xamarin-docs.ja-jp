---
title: 24 章の概要です。 ページのナビゲーション
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: DDCDB49C-6008-4F72-B095-463EE21D7C23
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 9d1a226a4532b745fddee28e943562fb51d34e65
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-24-page-navigation"></a>24 章の概要です。 ページのナビゲーション

多くのアプリケーションは、ユーザーが移動する複数のページで構成されます。 常に、アプリケーションが、*メイン*ページまたは*ホーム* ページで、し、そこから、ユーザーが移動するその他のページでは、バックアップを移動するためのスタックで保持されます。 追加のナビゲーション オプションについては、「 [**第 25 章します。ページの種類**](chapter25.md)です。

## <a name="modal-pages-and-modeless-pages"></a>ページのモーダルとモードレスのページ

`VisualElement` 定義、 [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/)型のプロパティ[ `INavigation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.INavigation/)、新しいページに移動する次の 2 つのメソッドが含まれます。

- [`PushAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushAsync/p/Xamarin.Forms.Page/)
- [`PushModalAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushModalAsync/p/Xamarin.Forms.Page/)

どちらのメソッド、`Page`インスタンスの引数と戻り値として、`Task`オブジェクト。 次の 2 つのメソッドは、前のページに移動します。

- [`PopAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopAsync()/)
- [`PopModalAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopModalAsync()/)

ユーザー インターフェイスには、独自場合**戻る**ボタン (Android および Windows phone での操作) アプリケーションでこれらのメソッドを呼び出す必要はありません。

これらのメソッドは、 `VisualElement`、通常はから呼び出すことが、 `Navigation` 、現在のプロパティ`Page`インスタンス。

前のページに戻る前に ページでいくつかの情報を得るため、ユーザーが必要な場合、アプリケーションは通常、モーダル ページを使用します。 モーダルれていないページとも呼ばれるモードレスまたは*階層*です。 モーダルまたはモードレス; として区別ページ自体ではありません。移動に使用する方法により、代わりに管理されます。 すべてのプラットフォームで作業をするには、ときは、前のページに戻るモーダル ページが独自のユーザー インターフェイスを指定してください。

[ **ModelessAndModal** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ModelessAndModal)サンプルでは、モードレスおよびモーダル ページ間の違いを調査することができます。 ページ ナビゲーションを使用するアプリケーションがそのホーム ページに渡す必要があります、 [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)コンス トラクター、一般に、プログラムの`App`クラスです。 1 つのボーナスは不要になったを設定する必要があること、 `Padding` iOS のページです。

いることがわかりますモードレス ページのページでの[ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Title/)プロパティが表示されます。 IOS、Android、および Windows タブレットやデスクトップ プラットフォームは、前のページに戻るにユーザー インターフェイス要素を提供します。 コース、Android、および Windows phone デバイスがある、標準的な**戻る**戻るボタンをクリックします。

モーダル ページ、ページの`Title`は表示されませんし、前のページに戻るにはユーザー インターフェイス要素が指定されていません。 Android および Windows phone 標準を使用できますが、**戻る**モーダル ページの他のプラットフォームでは、戻るには、独自のメカニズムを提供する必要がありますが、前のページに戻るにはボタンをクリックします。

### <a name="animated-page-transitions"></a>アニメーション化されたページは、遷移

設定する 2 番目のブール型引数が付属してさまざまなナビゲーション メソッドの代替バージョン`true`ページ遷移アニメーションを含めることの場合。

- [PushAsync](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushAsync/p/Xamarin.Forms.Page/System.Boolean/)
- [PushModalAsync](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushModalAsync/p/Xamarin.Forms.Page/System.Boolean/)
- [PopAsync](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopAsync/p/System.Boolean/)
- [PopModalAsync](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopModalAsync/p/System.Boolean/)

ただし、標準のページ ナビゲーション メソッドなど、アニメーション既定では、(説明したようにこの章の末尾にかけて)、スタートアップ時に特定のページに移動するための貴重なはのみ (で説明するように、独自の入口アニメーションを提供するときに[**第 22 章します。アニメーション**](chapter22.md))。

### <a name="visual-and-functional-variations"></a>外観および機能のバリエーション

`NavigationPage` 内のクラスをインスタンス化するときに設定できる 2 つのプロパティが含まれています、`App`メソッド。

- [`BarBackgroundColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.NavigationPage.BarBackgroundColor/)
- [`BarTextColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.NavigationPage.BarTextColor/)

`NavigationPage` 設定されている特定のページに影響する 4 つの接続されているバインド可能なプロパティも含まれています。

- [`SetHasBackButton`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.SetHasBackButton/p/Xamarin.Forms.Page/System.Boolean/) そして [`GetHasBackButton`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.GetHasBackButton/p/Xamarin.Forms.Page/)
- [`SetHasNavigationBar`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.SetHasNavigationBar/p/Xamarin.Forms.BindableObject/System.Boolean/) そして [`GetHasNavigationBar`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.GetHasNavigationBar/p/Xamarin.Forms.BindableObject/)
- [`SetBackButtonTitle`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.SetBackButtonTitle/p/Xamarin.Forms.BindableObject/System.String/) および[ `GetBackButtonTitle` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.GetBackButtonTitle/p/Xamarin.Forms.BindableObject/)作業して iOS のみ
- [`SetTitleIcon`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.SetTitleIcon/p/Xamarin.Forms.BindableObject/Xamarin.Forms.FileImageSource/) および[ `GetTitleIcon` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.GetTitleIcon/p/Xamarin.Forms.BindableObject/)作業して iOS および Android のみ

### <a name="exploring-the-mechanics"></a>探索のしくみ

ページのナビゲーション メソッドはすべての非同期でありで使用する必要があります`await`です。 完了時にページ ナビゲーションが完了したことをだけをページ ナビゲーションのスタックを調べるには安全では示されません。

最初のページが一般にへの呼び出しを取得して移動すると 1 つのページ別、その[ `OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing()/)メソッド、および 2 番目のページへの呼び出しを取得します。 その[ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing()/)メソッドです。 同様に、1 ページに戻るとき別、最初のページを取得する呼び出し、`OnDisappearing`メソッド、および 2 番目のページへの呼び出しを取得、通常その`OnAppearing`メソッドです。 これらの呼び出し (およびナビゲーションを呼び出す非同期メソッドの完了) の順序は、プラットフォームに依存します。 上記の 2 つのステートメントに「一般」という単語の使用は、Android のモーダル ページ ナビゲーション、これらのメソッド呼び出しが発生しないためです。

また、呼び出し、`OnAppearing`と`OnDisappearing`メソッドを示さないページ ナビゲーションとは限りません。

`INavigation`インターフェイスには、ナビゲーションのスタックを調べるには、2 つのコレクションのプロパティが含まれています。

- [`NavigationStack`](https://developer.xamarin.com/api/property/Xamarin.Forms.INavigation.NavigationStack/) 型の`IReadOnlyList<Page>`モードレス スタック
- [`ModalStack`](https://developer.xamarin.com/api/property/Xamarin.Forms.INavigation.ModalStack/) 型の`IReadOnlyList<Page>`モーダル スタック

これらの呼び出し履歴にアクセスする最も安全な方法は、`Navigation`のプロパティ、 `NavigationPage` (必要があります、`App`クラスの[ `MainPage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.MainPage/)プロパティ)。 安全、ページ ナビゲーションの非同期メソッドが完了した後でこれらの呼び出し履歴を調べるにはのみです。 [ `CurrentPage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.NavigationPage.CurrentPage/)のプロパティ、`NavigationPage`現在場合は、現在のページがモーダルページを示すページが代わりにモードレスの最後のページを示していません。

[ **SinglePageNavigation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/SinglePageNavigation)サンプルでは、ページ ナビゲーションと、スタック、およびページ ナビゲーションの有効な型を調査できます。

- モードレスのページは、モードレスの別のページまたはモーダル ページに移動できます。
- モーダル ページがモーダルの別のページにのみ移動できます。

### <a name="enforcing-modality"></a>モーダルかどうかを適用します。

アプリケーションは、ユーザーからいくつかの情報を取得する必要がある場合、モーダル ページを使用します。 ユーザーは、その情報が提供されるまで、前のページに返すことを禁止する必要があります。 Ios の場合は、簡単に提供する**戻る**ボタンをクリックし、ページで、ユーザーが完了した場合にのみ有効にします。 Android および Windows phone デバイスでは、アプリケーションを上書きする必要がありますが、 [ `OnBackButtonPressed` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnBackButtonPressed()/)メソッドと戻り値`true`プログラムが処理される場合、**戻る**で示したように、それ自体のボタンをクリックして[ **ModalEnforcement** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ModalEnforcement)サンプルです。

[ **MvvmEnforcement** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/MvvmEnforcement)サンプルでは、MVVM シナリオではこのしくみを示します。

## <a name="navigation-variations"></a>ナビゲーションのバリエーション

モーダルの特定のページは、複数回に移動することができる場合、は、ユーザーが入力する代わりに、情報を編集できるように情報を保持する必要がありますすべてでもう一度です。 モーダル ページの特定のインスタンスを保持してこれを処理することができますが、ビュー モデル内の情報を保持している (iOS) では特により適切な方法です。

### <a name="making-a-navigation-menu"></a>ナビゲーション メニューの作成

[ **ViewGalleryType** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ViewGalleryType)サンプルの使用例、`TableView`のリスト メニュー項目にします。 各項目に関連付けられている、`Type`特定のページのオブジェクト。 その項目を選択すると、プログラムは、ページをインスタンス化しに移動します。

[![ギャラリー ビューの種類のトリプル スクリーン ショット](images/ch24fg21-small.png "テーブルを一覧表示するメニュー項目")](images/ch24fg21-large.png#lightbox "テーブルを一覧表示するメニュー項目")

[ **ViewGalleryInst** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ViewGalleryInst)サンプルは、メニューには、型ではなく各ページのインスタンスが含まれているという点では少し異なります。 これにより、各ページの情報を保持しますが、プログラムの起動時に、すべてのページをインスタンス化する必要があります。

### <a name="manipulating-the-navigation-stack"></a>ナビゲーション スタックを操作します。

[**StackManipulation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/StackManipulation)によって定義されるいくつかの関数を示します`INavigation`構造化された方法で、ナビゲーション スタックを操作するための。

- [`RemovePage`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.RemovePage/p/Xamarin.Forms.Page/)
- [`InsertPageBefore`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.InsertPageBefore/p/Xamarin.Forms.Page/Xamarin.Forms.Page/)
- [`PopToRootAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopToRootAsync()/) および[ `PopToRootAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopToRootAsync/p/System.Boolean/)省略可能なアニメーションを使用して

### <a name="dynamic-page-generation"></a>動的なページの生成

[ **BuildAPage** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/BuildAPage)サンプルではユーザー入力に基づいて実行時にページを構築します。

## <a name="patterns-of-data-transfer"></a>データ転送のパターン

ページ間でデータを共有する必要があります&mdash;とは、起動したページにデータを返すページを移動 ページでは、データを転送します。 これを行うためのいくつかの手法があります。

### <a name="constructor-arguments"></a>コンス トラクターの引数

新しいページに移動するときに、それ自体を初期化するためにページをできるようにするコンス トラクターの引数を持つページ クラスのインスタンスを作成することです。 [ **SchoolAndStudents** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/SchoolAndStudents)サンプルを示します。 移動 ページのこともその`BindingContext`に移動するページで設定します。

### <a name="properties-and-method-calls"></a>プロパティとメソッドの呼び出し

その他のデータ転送の例では、1 つのページが別のページに移動するときに、ページとの間で情報を渡す場合の問題について説明します。 これらのディスカッション内で、*ホーム*のページに移動、*情報*ページ、および初期化情報を転送する必要があります、*情報*ページ。 *情報*ページがユーザーから追加情報を取得しに情報を転送する、*ホーム*ページ。

*ホーム*するパブリック メソッドとプロパティに、ページが簡単にアクセスする、*情報*ページとしてそのページがインスタンス化します。 *情報*ページもアクセスできるパブリック メソッドとプロパティで、*ホーム* ページで、これは複雑になることができますに絶好のタイミングを選択します。 [ **DateTransfer1** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer1)このサンプルではその`OnDisappearing`をオーバーライドします。 1 つの欠点は、*情報*ページがの型を認識する必要があります、*ホーム*ページ。

### <a name="messagingcenter"></a>MessagingCenter

Xamarin.Forms [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/)クラスには、互いに通信するために 2 つのページの別の方法が用意されています。 メッセージは、テキスト文字列で識別され、任意のオブジェクトが存在することができます。

特定の種類からメッセージを受信しようとしているプログラムがサブスクライブする必要がありますを使用して[ `MessagingCenter.Subscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Subscribe%7BTSender,TArgs%7D/p/System.Object/System.String/System.Action%7BTSender,TArgs%7D/TSender/)し、コールバック関数を指定します。 後で呼び出すことによってアンサブスク ライブできます[ `MessagingCenter.Unsubscribe`](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Unsubscribe%7BTSender,TArgs%7D/p/System.Object/System.String/)です。 コールバック関数はを介して送信される指定の名前を持つ指定した型から送信されたメッセージを受信、 [ `Send` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Send%7BTSender,TArgs%7D/p/TSender/System.String/TArgs/)メソッドです。

[ **DateTransfer2** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer2)プログラムは、メッセージング センターを使用してデータを転送する方法を示しますが、もう一度このことが必要、*情報*ページには、の種類がわかっている*ホーム*ページ。

### <a name="events"></a>イベント

イベントは、別のクラスにそのクラスの型を知らなくても情報を送信する 1 つのクラスになじみ深いアプローチです。 [ **DateTransfer3** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer3)サンプル、*情報*クラスについては、準備ができたら、発生するイベントを定義します。 ただしの便利な場所がありません、*ホーム*イベント ハンドラーをデタッチするページ。

### <a name="the-app-class-intermediary"></a>App クラスの中継ぎ局

[ **DateTransfer4** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer4)サンプルで定義されているプロパティにアクセスする方法を示します、`App`両方によってクラス、*ホーム*ページおよび*情報*ページ。 これは優れたソリューションですが、次のセクションでは、優れたソリューションについて説明します。

### <a name="switching-to-a-viewmodel"></a>ViewModel への切り替え

により、情報、ViewModel を使用して、*ホーム*ページおよび*情報*情報クラスのインスタンスを共有するページ。 これに示されている、 [ **DateTransfer5** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer5)サンプルです。

### <a name="saving-and-restoring-page-state"></a>保存とページの状態の復元

`App`クラス中継ぎ局または ViewModel 方法が最も適しているプログラムがスリープ状態になった場合、アプリケーションで情報を保存する必要があります中に、*情報*ページがアクティブです。 [ **DateTransfer6** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer6)サンプルを示します。

## <a name="saving-and-restoring-the-navigation-stack"></a>保存と復元のナビゲーション スタック

[全般] の場合、スリープ状態になっている複数ページのプログラムは必要がありますを復元するときに、同じページに移動します。 つまり、このようなプログラムがナビゲーション スタックの内容を保存する必要があります。 このセクションでは、この目的で設計されたクラスでは、このプロセスを自動化する方法を示します。 このクラスは、それを保存し、そのページの状態の復元を許可する個々 のページも呼び出します。

[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリという名前のインターフェイスを定義する[ `IPersistantPage` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/IPersistentPage.cs)を保存し、でアイテムを復元するクラスに実装できること`Properties`ディクショナリ。

[ `MultiPageRestorableApp` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MultiPageRestorableApp.cs)クラス内で、 **Xamarin.FormsBook.Toolkit**から派生したライブラリ`Application`です。 派生することができます、`App`クラス`MultiPageRestorableApp`を行い、一部のハウスキーピングです。

[ **StackRestoreDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/StackRestoreDemo)の使用例を示します`MultiPageRestorableApp`です。

### <a name="something-like-a-real-life-app"></a>以下のような実際のアプリ

[ **NoteTaker** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/NoteTaker)サンプルでは、使用も`MultiPageRestorableApp`でき、入力と編集のノートに保存されていることができます、`Properties`ディクショナリ。



## <a name="related-links"></a>関連リンク

- [24 章フル テキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch24-Apr2016.pdf)
- [24 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24)
- [階層ナビゲーション](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)
- [モーダル ページ](~/xamarin-forms/app-fundamentals/navigation/modal.md)
