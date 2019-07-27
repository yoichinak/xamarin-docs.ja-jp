---
title: Android コントロール (ウィジェット)
description: Xamarin Android ユーザーインターフェイスを作成するための構成要素
ms.prod: xamarin
ms.assetid: B7A82166-B920-4672-B7A2-20DD5E0B5AEF
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/29/2018
ms.openlocfilehash: 31f6c0dd0d4f5452ebc2cbde0cc44cd9c47eeb9a
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510315"
---
# <a name="xamarinandroid-controls-widgets"></a>Xamarin Android コントロール (ウィジェット)

Android に用意されているすべてのネイティブユーザーインターフェイスコントロール (ウィジェット) は、Xamarin Android によって公開されます。 これらのコントロールは、Android Designer またはプログラムによって XML レイアウトファイルを使用して Xamarin Android アプリに簡単に追加できます。 選択する方法に関係なく、Xamarin Android では、のすべてのユーザーインターフェイスオブジェクトのプロパティとC#メソッドが公開されます。 以下のセクションでは、最も一般的な Android ユーザーインターフェイスコントロールについて紹介し、それらを Xamarin Android アプリに組み込む方法について説明します。

## <a name="action-barandroiduser-interfacecontrolsaction-barmd"></a>[操作バー](~/android/user-interface/controls/action-bar.md) 

`ActionBar`は、アクティビティタイトル、ナビゲーションインターフェイス、およびその他の対話型項目を表示するツールバーです。 通常、アクションバーは、アクティビティのウィンドウの上部に表示されます。

![ActionBar の例](images/action-bar.png)


## <a name="auto-completeandroiduser-interfacecontrolsauto-completemd"></a>[オートコンプリート](~/android/user-interface/controls/auto-complete.md)

`AutoCompleteTextView`は編集可能なテキストビュー要素で、ユーザーの入力中に自動的に入力候補を表示します。 修正候補の一覧がドロップダウンメニューに表示されます。このメニューから、編集ボックスの内容を置き換える項目をユーザーが選択できます。

![オートコンプリートの例](images/auto-complete.png)


## <a name="buttonsandroiduser-interfacecontrolsbuttonsindexmd"></a>[ボタン](~/android/user-interface/controls/buttons/index.md)

ボタンは、ユーザーが操作を実行するためにタップする UI 要素です。

![ボタンの例](images/buttons.png)


## <a name="calendarandroiduser-interfacecontrolscalendarmd"></a>[Calendar](~/android/user-interface/controls/calendar.md)

`Calendar`クラスは、時間内の特定のインスタンス (エポックからのオフセットであるミリ秒の値) を、年、月、時、月の日、次の週の日付などの値に変換するために使用されます。
`Calendar`では、イベント、出席者、リマインダーの読み取りや書き込みなど、カレンダーデータを使用した豊富な相互作用オプションがサポートされています。 アプリケーションで calendar プロバイダーを使用すると、API を通じて追加したデータが、Android に付属する組み込みの予定表アプリに表示されます。

![カレンダーの例](images/calendar.png)


## <a name="cardviewandroiduser-interfacecontrolscard-viewmd"></a>[CardView](~/android/user-interface/controls/card-view.md)

`CardView`は、カードに似たビューにテキストとイメージの内容を表示する UI コンポーネントです。 `CardView`は、角が`FrameLayout`丸く、影が付いたウィジェットとして実装されます。 通常、 `CardView` `ListView`または`GridView`ビューグループに単一の行項目を表示するには、を使用します。

![カードビューの例](images/cardview.png)


## <a name="edit-textandroiduser-interfacecontrolsedit-textmd"></a>[テキストの編集](~/android/user-interface/controls/edit-text.md)

`EditText`は、テキストの入力と変更に使用される UI 要素です。

![テキストの編集の例](images/edit-text.png)


## <a name="galleryandroiduser-interfacecontrolsgallerymd"></a>[ギャラリー](~/android/user-interface/controls/gallery.md)

`Gallery`は、水平スクロールリストに項目を表示するために使用されるレイアウトウィジェットです。現在の選択範囲をビューの中央に配置します。

![ギャラリーの例](images/gallery.png)


## <a name="navigation-barandroiduser-interfacecontrolsnavigation-barmd"></a>[ナビゲーション バー](~/android/user-interface/controls/navigation-bar.md)

*ナビゲーションバー*は、 **[ホーム]** 、 **[戻る]** 、および **[メニュー]** のハードウェアボタンを含まないデバイス上のナビゲーションコントロールを提供します。

![ナビゲーションバーの例](images/navigation-bar.png)


## <a name="pickersandroiduser-interfacecontrolspickersindexmd"></a>[選択](~/android/user-interface/controls/pickers/index.md)

*ピッカー*は、ユーザーが Android によって提供されるダイアログを使用して日付または時刻を選択できるようにする UI 要素です。

![ピッカーの例](images/picker.png)


## <a name="popup-menuandroiduser-interfacecontrolspopup-menumd"></a>[ポップアップ メニュー](~/android/user-interface/controls/popup-menu.md)

`PopupMenu`は、特定のビューにアタッチされているポップアップメニューを表示するために使用されます。

![ポップアップメニューの例](images/popup-menu.png)


## <a name="ratingbarandroiduser-interfacecontrolsratingbarmd"></a>[RatingBar](~/android/user-interface/controls/ratingbar.md)

は、星の評価を表示する UI 要素です。`RatingBar`

![RatingBar の例](ratingbar-images/01-ratingbar.png)


## <a name="spinnerandroiduser-interfacecontrolsspinnermd"></a>[スピン ボタン](~/android/user-interface/controls/spinner.md)

`Spinner`は、セットから1つの値を選択するための簡単な方法を提供する UI 要素です。 ドロップダウンリストが表示されます。 

![スピンボタンの例](images/spinner.png)


## <a name="switchandroiduser-interfacecontrolsswitchmd"></a>[スイッチ](~/android/user-interface/controls/switch.md)

`Switch`は、ユーザーがオンまたはオフなどの2つの状態を切り替えることができるようにする UI 要素です。 `Switch`既定値は OFF です。

![スイッチの例](images/switch.png)


## <a name="textureviewandroiduser-interfacecontrolstexture-viewmd"></a>[TextureView](~/android/user-interface/controls/texture-view.md)

`TextureView`は、ビデオまたは OpenGL コンテンツストリームを表示できるようにするための、ハードウェアアクセラの2D レンダリングを使用するビューです。

![テクスチャビューの例](images/texture-view.png)


## <a name="toolbarandroiduser-interfacecontrolstool-barindexmd"></a>[ToolBar](~/android/user-interface/controls/tool-bar/index.md)

( `Toolbar` Android 5.0 ロリポップで導入された) ウィジェットは、アクションバーインターフェイス&ndash;の一般化として考えることができます。これは、操作バーを置き換えることを目的としています。 は`Toolbar` 、アプリレイアウト内の任意の場所で使用でき、操作バーよりもはるかにカスタマイズできます。

![ツールバーの例](images/toolbar.png)


## <a name="viewpagerandroiduser-interfacecontrolsview-pagerindexmd"></a>[ViewPager](~/android/user-interface/controls/view-pager/index.md) 

は`ViewPager` 、ユーザーがデータのページを左右に反転できるレイアウトマネージャーです。

![ViewPager の例](images/viewpager.png)


## <a name="webviewandroiduser-interfacecontrolsweb-viewmd"></a>[WebView](~/android/user-interface/controls/web-view.md)

`WebView`は、web ページを表示するための独自のウィンドウを作成できる UI 要素です (または、完全なブラウザーを開発することもできます)。

![Web ビューの例](images/web-view.png)

