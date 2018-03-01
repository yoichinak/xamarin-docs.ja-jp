---
title: "コントロール"
description: "Xamarin.Android ユーザー インターフェイスを作成するためのビルド ブロック"
ms.topic: article
ms.prod: xamarin
ms.assetid: B7A82166-B920-4672-B7A2-20DD5E0B5AEF
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 25afd284fc88df4f23aaa3dfa1f47a3dc4fee551
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="controls"></a>コントロール


Xamarin.Android は、Android によって提供されるネイティブ ユーザー インターフェイス コントロール (のウィジェット) のすべてを公開します。 これらのコントロールは、Android デザイナーを使用して Xamarin.Android アプリにまたはレイアウトの XML ファイルを使用してプログラムで簡単に追加できます。 選択したどのメソッドに関係なく Xamarin.Android はすべてのユーザー インターフェイス オブジェクトのプロパティと c# のメソッドを公開します。 次のセクションでは、最も一般的な Android のユーザー インターフェイス コントロールを紹介し、Xamarin.Android アプリに組み込む方法について説明します。

## <a name="action-barandroiduser-interfacecontrolsaction-barmd"></a>[操作バー](~/android/user-interface/controls/action-bar.md) 

`ActionBar` 活動のタイトル、ナビゲーション インターフェイス、およびその他の対話型の項目を表示するツールバーです。 通常、アクティビティのウィンドウの上部にある操作バーが表示されます。

![例アクションバー](images/action-bar.png)


## <a name="auto-completeandroiduser-interfacecontrolsauto-completemd"></a>[オート コンプリート](~/android/user-interface/controls/auto-complete.md)

`AutoCompleteTextView` ユーザーの入力中に自動的に補完候補を表示する編集可能なテキスト ビュー要素です。 修正候補の一覧は、ユーザーがエディット ボックスのコンテンツを置換する項目を選択できるメニュー ドロップダウンに表示されます。

![オート コンプリートの例](images/auto-complete.png)


## <a name="buttonsandroiduser-interfacecontrolsbuttonsindexmd"></a>[ボタン](~/android/user-interface/controls/buttons/index.md)

ボタンは、操作を実行するユーザーはタップ操作する UI 要素を示します。

![ボタンの例](images/buttons.png)


## <a name="calendarandroiduser-interfacecontrolscalendarmd"></a>[Calendar](~/android/user-interface/controls/calendar.md)

`Calendar`クラスは、特定のインスタンスに変換するため使用時間など、年、月、1 時間、曜日、月、および次の週の日付の値に (エポックからのオフセット ミリ秒値)。
`Calendar` さまざまなイベント、出席者とリマインダーを読み書きする機能など、カレンダー データの相互作用オプションをサポートしています。 カレンダーのプロバイダーを使用すると、アプリケーションで、API を通じて追加したデータは、Android に付属する組み込みの予定表アプリに表示されます。

![予定表の例](images/calendar.png)


## <a name="cardviewandroiduser-interfacecontrolscard-viewmd"></a>[CardView](~/android/user-interface/controls/card-view.md)

`CardView` カードのようになりますをビュー内のテキストとイメージのコンテンツを表示する UI コンポーネントです。 `CardView` として実装された、`FrameLayout`角の丸いと影ウィジェット。 通常、`CardView`内の 1 つの行項目を表示するために、`ListView`または`GridView`ビュー グループ化します。

![例のカード ビュー](images/cardview.png)


## <a name="edit-textandroiduser-interfacecontrolsedit-textmd"></a>[テキストを編集します。](~/android/user-interface/controls/edit-text.md)

`EditText` 入力すると、テキストを変更するために使用する UI 要素です。

![例のテキストの編集](images/edit-text.png)


## <a name="galleryandroiduser-interfacecontrolsgallerymd"></a>[ギャラリー](~/android/user-interface/controls/gallery.md)

`Gallery` 水平方向にスクロール リスト内の項目を表示するために使用するレイアウト ウィジェットには現在の選択範囲をビューの中央に配置します。

![サンプル ギャラリー](images/gallery.png)


## <a name="navigation-barandroiduser-interfacecontrolsnavigation-barmd"></a>[ナビゲーション バー](~/android/user-interface/controls/navigation-bar.md)

*ナビゲーション バー*ハードウェア ボタンが含まれていないデバイス上のナビゲーション コントロールを提供**ホーム**、**戻る**、および**メニュー**です。

![ナビゲーション バーの例](images/navigation-bar.png)


## <a name="pickersandroiduser-interfacecontrolspickersindexmd"></a>[ピッカー](~/android/user-interface/controls/pickers/index.md)

*ピッカー* UI 要素はユーザーが Android によって提供されるダイアログを使用して、日付や時刻を選択できるようにします。

![例の選択](images/picker.png)


## <a name="popup-menuandroiduser-interfacecontrolspopup-menumd"></a>[ポップアップ メニュー](~/android/user-interface/controls/popup-menu.md)

`PopupMenu` 特定のビューに関連付けられているポップアップ メニューの表示に使用されます。

![例のポップアップ メニュー](images/popup-menu.png)


## <a name="spinnerandroiduser-interfacecontrolsspinnermd"></a>[スピン ボタン](~/android/user-interface/controls/spinner.md)

`Spinner` セットから 1 つの値を選択する簡単な方法を提供する UI 要素です。 ドロップダウン リストに simmilar することをお勧めします。 

![スピン ボタンの例](images/spinner.png)


## <a name="switchandroiduser-interfacecontrolsswitchmd"></a>[スイッチ](~/android/user-interface/controls/switch.md)

`Switch` ユーザーなど、2 つの状態間を切り替える] または [オフにできる UI 要素です。 `Switch`既定値は OFF です。

![例スイッチ](images/switch.png)


## <a name="textureviewandroiduser-interfacecontrolstexture-viewmd"></a>[TextureView](~/android/user-interface/controls/texture-view.md)

`TextureView` ビデオまたは表示する OpenGL のコンテンツ ストリームを有効にするハードウェア アクセラレータの 2D レンダリングを使用するビューです。

![例のテクスチャ ビュー](images/texture-view.png)


## <a name="toolbarandroiduser-interfacecontrolstool-barindexmd"></a>[ToolBar](~/android/user-interface/controls/tool-bar/index.md)

`Toolbar` (Android 5.0 ロリポップで導入) ウィジェットはようなもののアクション バー インターフェイスの汎化&ndash;操作バーを置き換えるためのものにします。 `Toolbar`レイアウトでは、アプリ、どこでも使用できますであり、アクション バーよりもはるかにカスタマイズ可能な。

![例のツールバー](images/toolbar.png)


## <a name="viewpagerandroiduser-interfacecontrolsview-pagerindexmd"></a>[ViewPager](~/android/user-interface/controls/view-pager/index.md) 

`ViewPager`はレイアウト マネージャーで、データのページ間の左右を反転することができます。

![例 ViewPager](images/viewpager.png)


## <a name="webviewandroiduser-interfacecontrolsweb-viewmd"></a>[WebView](~/android/user-interface/controls/web-view.md)

`WebView` UI 要素を使用すると、web ページを表示するための独自のウィンドウを作成する (またはであっても、完全なブラウザーの開発) です。

![例の Web ビュー](images/web-view.png)

