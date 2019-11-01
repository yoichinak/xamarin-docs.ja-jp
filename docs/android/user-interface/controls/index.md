---
title: Android コントロール (ウィジェット)
description: Xamarin Android ユーザーインターフェイスを作成するための構成要素
ms.prod: xamarin
ms.assetid: B7A82166-B920-4672-B7A2-20DD5E0B5AEF
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/29/2018
ms.openlocfilehash: 8c0a2dffbe312cb25258cd2738b661ded2df8d7d
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029269"
---
# <a name="xamarinandroid-controls-widgets"></a>Xamarin Android コントロール (ウィジェット)

Android に用意されているすべてのネイティブユーザーインターフェイスコントロール (ウィジェット) は、Xamarin Android によって公開されます。 これらのコントロールは、Android Designer またはプログラムによって XML レイアウトファイルを使用して Xamarin Android アプリに簡単に追加できます。 選択する方法に関係なく、Xamarin Android では、のすべてのユーザーインターフェイスオブジェクトのプロパティとC#メソッドが公開されます。 以下のセクションでは、最も一般的な Android ユーザーインターフェイスコントロールについて紹介し、それらを Xamarin Android アプリに組み込む方法について説明します。

## <a name="action-barandroiduser-interfacecontrolsaction-barmd"></a>[操作バー](~/android/user-interface/controls/action-bar.md) 

`ActionBar` は、アクティビティタイトル、ナビゲーションインターフェイス、およびその他の対話型項目を表示するツールバーです。 通常、アクションバーは、アクティビティのウィンドウの上部に表示されます。

![ActionBar の例](images/action-bar.png)

## <a name="auto-completeandroiduser-interfacecontrolsauto-completemd"></a>[オートコンプリート](~/android/user-interface/controls/auto-complete.md)

`AutoCompleteTextView` は、ユーザーが入力している間に自動的に入力候補を表示する、編集可能なテキストビュー要素です。 修正候補の一覧がドロップダウンメニューに表示されます。このメニューから、編集ボックスの内容を置き換える項目をユーザーが選択できます。

![オートコンプリートの例](images/auto-complete.png)

## <a name="buttonsandroiduser-interfacecontrolsbuttonsindexmd"></a>[ボタン](~/android/user-interface/controls/buttons/index.md)

ボタンは、ユーザーが操作を実行するためにタップする UI 要素です。

![ボタンの例](images/buttons.png)

## <a name="calendarandroiduser-interfacecontrolscalendarmd"></a>[カレンダー](~/android/user-interface/controls/calendar.md)

`Calendar` クラスは、特定のインスタンスを時間内に変換するために使用されます (エポックからのミリ秒の値)。この値は、年、月、時、日、および次の週の日付などの値に変換されます。
`Calendar` では、イベント、出席者、アラームの読み取りや書き込みなどの機能を含む、カレンダーデータを使用した豊富な対話オプションがサポートされています。 アプリケーションで calendar プロバイダーを使用すると、API を通じて追加したデータが、Android に付属する組み込みの予定表アプリに表示されます。

![カレンダーの例](images/calendar.png)

## <a name="cardviewandroiduser-interfacecontrolscard-viewmd"></a>[CardView](~/android/user-interface/controls/card-view.md)

`CardView` は、カードに似たビューでテキストとイメージの内容を表示する UI コンポーネントです。 `CardView` は、角が丸く、影が `FrameLayout` ウィジェットとして実装されます。 通常、`CardView` は、`ListView` または `GridView` ビューグループに単一の行項目を表示するために使用されます。

![カードビューの例](images/cardview.png)

## <a name="edit-textandroiduser-interfacecontrolsedit-textmd"></a>[テキストの編集](~/android/user-interface/controls/edit-text.md)

`EditText` は、テキストの入力と変更に使用される UI 要素です。

![テキストの編集の例](images/edit-text.png)

## <a name="galleryandroiduser-interfacecontrolsgallerymd"></a>[ギャラリー](~/android/user-interface/controls/gallery.md)

`Gallery` は、水平スクロールリストに項目を表示するために使用されるレイアウトウィジェットです。現在の選択範囲をビューの中央に配置します。

![ギャラリーの例](images/gallery.png)

## <a name="navigation-barandroiduser-interfacecontrolsnavigation-barmd"></a>[ナビゲーション バー](~/android/user-interface/controls/navigation-bar.md)

*ナビゲーションバー*は、 **[ホーム]** 、 **[戻る]** 、および **[メニュー]** のハードウェアボタンを含まないデバイス上のナビゲーションコントロールを提供します。

![ナビゲーションバーの例](images/navigation-bar.png)

## <a name="pickersandroiduser-interfacecontrolspickersindexmd"></a>[選択](~/android/user-interface/controls/pickers/index.md)

*ピッカー*は、ユーザーが Android によって提供されるダイアログを使用して日付または時刻を選択できるようにする UI 要素です。

![ピッカーの例](images/picker.png)

## <a name="popup-menuandroiduser-interfacecontrolspopup-menumd"></a>[ポップアップ メニュー](~/android/user-interface/controls/popup-menu.md)

`PopupMenu` は、特定のビューにアタッチされているポップアップメニューを表示するために使用されます。

![ポップアップメニューの例](images/popup-menu.png)

## <a name="ratingbarandroiduser-interfacecontrolsratingbarmd"></a>[RatingBar](~/android/user-interface/controls/ratingbar.md)

`RatingBar` は、星の評価を表示する UI 要素です。

![RatingBar の例](ratingbar-images/01-ratingbar.png)

## <a name="spinnerandroiduser-interfacecontrolsspinnermd"></a>[スピン ボタン](~/android/user-interface/controls/spinner.md)

`Spinner` は、セットから1つの値を選択するための簡単な方法を提供する UI 要素です。 ドロップダウンリストが表示されます。 

![スピンボタンの例](images/spinner.png)

## <a name="switchandroiduser-interfacecontrolsswitchmd"></a>[スイッチ](~/android/user-interface/controls/switch.md)

`Switch` は、ユーザーがオンまたはオフなどの2つの状態を切り替えることができるようにする UI 要素です。 `Switch` の既定値は OFF です。

![スイッチの例](images/switch.png)

## <a name="textureviewandroiduser-interfacecontrolstexture-viewmd"></a>[TextureView](~/android/user-interface/controls/texture-view.md)

`TextureView` は、ビデオまたは OpenGL コンテンツストリームを表示できるようにするための、ハードウェアアクセラの2D レンダリングを使用するビューです。

![テクスチャビューの例](images/texture-view.png)

## <a name="toolbarandroiduser-interfacecontrolstool-barindexmd"></a>[ToolBar](~/android/user-interface/controls/tool-bar/index.md)

(Android 5.0 ロリポップで導入された) `Toolbar` ウィジェットは、アクションバーのインターフェイスの一般化として考えることができ &ndash; 操作バーを置き換えることを意図しています。 `Toolbar` は、アプリレイアウト内の任意の場所で使用できます。また、操作バーよりもはるかにカスタマイズできます。

![ツールバーの例](images/toolbar.png)

## <a name="viewpagerandroiduser-interfacecontrolsview-pagerindexmd"></a>[ViewPager](~/android/user-interface/controls/view-pager/index.md) 

`ViewPager` は、ユーザーがデータページを左右に反転できるレイアウトマネージャーです。

![ViewPager の例](images/viewpager.png)

## <a name="webviewandroiduser-interfacecontrolsweb-viewmd"></a>[WebView](~/android/user-interface/controls/web-view.md)

`WebView` は、web ページを表示するための独自のウィンドウを作成できる UI 要素です (または、完全なブラウザーを開発することもできます)。

![Web ビューの例](images/web-view.png)
