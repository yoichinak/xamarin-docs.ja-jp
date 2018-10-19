---
title: Android のコントロール (ウィジェット)
description: Xamarin.Android のユーザー インターフェイスを作成するための構成要素
ms.prod: xamarin
ms.assetid: B7A82166-B920-4672-B7A2-20DD5E0B5AEF
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/29/2018
ms.openlocfilehash: 842fb1df2c9cc1aaf1a106687179a3730c2503bd
ms.sourcegitcommit: 7e4070bc104d612b6754ea35dd5a49c5c3d45f4a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/18/2018
ms.locfileid: "32436714"
---
# <a name="android-controls-widgets"></a>Android のコントロール (ウィジェット)

Xamarin.Android では、Android によって提供されるネイティブ ユーザー インターフェイス コントロール (ウィジェット) のすべてを公開します。 これらのコントロールは、Android Designer を使用して Xamarin.Android アプリへ、または XML レイアウト ファイルを使用してプログラムで簡単に追加できます。 Xamarin.Android を選択する方法に関係なく、すべてのユーザー インターフェイス オブジェクトのプロパティと c# でメソッドを公開します。 次のセクションでは、最も一般的な Android のユーザー インターフェイス コントロールを紹介し、Xamarin.Android アプリに組み込む方法を説明します。

## <a name="action-barandroiduser-interfacecontrolsaction-barmd"></a>[操作バー](~/android/user-interface/controls/action-bar.md) 

`ActionBar` 活動のタイトル、ナビゲーション インターフェイス、およびその他の対話型の項目を表示するツールバーです。 通常、アクティビティのウィンドウの上部にある操作バーが表示されます。

![ActionBar の例](images/action-bar.png)


## <a name="auto-completeandroiduser-interfacecontrolsauto-completemd"></a>[オートコンプリート](~/android/user-interface/controls/auto-complete.md)

`AutoCompleteTextView` ユーザーの入力中に自動的に入力候補を表示する編集可能なテキスト ビュー要素です。 入力候補の一覧は、ユーザーが編集ボックスのコンテンツを置換する項目を選択できるメニュー ドロップダウンで表示されます。

![オート コンプリートの使用例](images/auto-complete.png)


## <a name="buttonsandroiduser-interfacecontrolsbuttonsindexmd"></a>[ボタン](~/android/user-interface/controls/buttons/index.md)

ボタンは、ユーザーがタップ操作を実行する UI 要素です。

![ボタンの例](images/buttons.png)


## <a name="calendarandroiduser-interfacecontrolscalendarmd"></a>[Calendar](~/android/user-interface/controls/calendar.md)

`Calendar`クラスで特定のインスタンスに変換するために使用時間 (年、月、1 時間、曜日、月、および次の週の日付などの値を (エポックからのオフセット ミリ秒値)。
`Calendar` 豊富なイベント、出席者、およびアラームを読み書きする機能など、予定表のデータとの相互作用オプションをサポートしています。 予定表のプロバイダーを使用すると、アプリケーションで、API を通じて追加したデータは、Android に付属する組み込みの予定表アプリに表示されます。

![予定表の例](images/calendar.png)


## <a name="cardviewandroiduser-interfacecontrolscard-viewmd"></a>[CardView](~/android/user-interface/controls/card-view.md)

`CardView` カードのようにビューのテキストとイメージのコンテンツを表示する UI コンポーネントです。 `CardView` として実装されます、`FrameLayout`の角が丸いと影のウィジェット。 通常、`CardView`内の 1 つの行項目を表示するために、`ListView`または`GridView`ビュー グループ化します。

![カード ビューの例](images/cardview.png)


## <a name="edit-textandroiduser-interfacecontrolsedit-textmd"></a>[テキストを編集します。](~/android/user-interface/controls/edit-text.md)

`EditText` 入力すると、テキストを変更するために使用する UI 要素です。

![例のテキストの編集](images/edit-text.png)


## <a name="galleryandroiduser-interfacecontrolsgallerymd"></a>[ギャラリー](~/android/user-interface/controls/gallery.md)

`Gallery` 水平方向にスクロール リストに項目を表示するために使用するレイアウト ウィジェット現在の選択範囲をビューの中央に配置します。

![サンプル ギャラリー](images/gallery.png)


## <a name="navigation-barandroiduser-interfacecontrolsnavigation-barmd"></a>[ナビゲーション バー](~/android/user-interface/controls/navigation-bar.md)

*ナビゲーション バー*用のハードウェア ボタンが含まれていないデバイス上のナビゲーション コントロールを提供します。**ホーム**、**戻る**、および**メニュー**します。

![ナビゲーション バーの例](images/navigation-bar.png)


## <a name="pickersandroiduser-interfacecontrolspickersindexmd"></a>[選択](~/android/user-interface/controls/pickers/index.md)

*ピッカー*ユーザーが Android によって提供されるダイアログ ボックスを使用して、日付や時刻を選択できるようにする UI 要素を示します。

![例の選択](images/picker.png)


## <a name="popup-menuandroiduser-interfacecontrolspopup-menumd"></a>[ポップアップ メニュー](~/android/user-interface/controls/popup-menu.md)

`PopupMenu` 特定のビューにアタッチされているポップアップ メニューを表示するために使用されます。

![ポップアップ メニューの例](images/popup-menu.png)


## <a name="ratingbarandroiduser-interfacecontrolsratingbarmd"></a>[RatingBar](~/android/user-interface/controls/ratingbar.md)

A`RatingBar`は星で評価を表示する UI 要素です。

![RatingBar の例](ratingbar-images/01-ratingbar.png)


## <a name="spinnerandroiduser-interfacecontrolsspinnermd"></a>[スピン ボタン](~/android/user-interface/controls/spinner.md)

`Spinner` セットから 1 つの値を選択する簡単な方法を提供する UI 要素です。 ドロップダウン リストに simmilar になります。 

![スピン ボタンの例](images/spinner.png)


## <a name="switchandroiduser-interfacecontrolsswitchmd"></a>[スイッチ](~/android/user-interface/controls/switch.md)

`Switch` ユーザーなど、2 つの状態の間で切り替えるか、オフにできる UI 要素です。 `Switch`既定値は OFF です。

![例のスイッチ](images/switch.png)


## <a name="textureviewandroiduser-interfacecontrolstexture-viewmd"></a>[TextureView](~/android/user-interface/controls/texture-view.md)

`TextureView` ビデオまたは表示する OpenGL のコンテンツ ストリームを有効にするハードウェア アクセラレータによる 2D レンダリングを使用するビューです。

![テクスチャ ビューの例](images/texture-view.png)


## <a name="toolbarandroiduser-interfacecontrolstool-barindexmd"></a>[ToolBar](~/android/user-interface/controls/tool-bar/index.md)

`Toolbar`ウィジェット (Android 5.0 Lollipop で導入された) アクション バーのインターフェイスの汎化として考えることができます&ndash;には、操作バーを代替するものです。 `Toolbar`アプリのレイアウトでの任意の場所で使用できるし、は、操作バーよりもはるかにカスタマイズ可能な。

![ツールバーの例](images/toolbar.png)


## <a name="viewpagerandroiduser-interfacecontrolsview-pagerindexmd"></a>[ViewPager](~/android/user-interface/controls/view-pager/index.md) 

`ViewPager`はユーザーがデータのページ間の左右を反転できるレイアウト マネージャーです。

![ViewPager の例](images/viewpager.png)


## <a name="webviewandroiduser-interfacecontrolsweb-viewmd"></a>[WebView](~/android/user-interface/controls/web-view.md)

`WebView` UI 要素を使用すると、web ページを表示するための独自のウィンドウを作成します (または完全なブラウザーを開発しても) です。

![Web ビューの例](images/web-view.png)

