---
title: IOS で Backgrounding の概要
description: このドキュメントでは、iOS で backgrounding について説明します。 アプリケーションの状態、アプリケーション ライフ サイクル メソッド、およびバック グラウンド アプリケーション更新します。
ms.prod: xamarin
ms.assetid: E214F2C7-E74E-46C7-B5BA-080B30D61250
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: c8084d8e218ba8e3468529795aaa5fd4eae30947
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783640"
---
# <a name="introduction-to-backgrounding-in-ios"></a>IOS で Backgrounding の概要

iOS では、バック グラウンド処理が非常に制御およびそれを実装する 3 つの方法を提供しています。

-  **バック グラウンド タスクを登録**-アプリケーションがバック グラウンドに移動したときにタスクを中断しないように iOS を尋ねることができますをアプリケーションが重要なタスクを完了する必要がある場合。 たとえば、アプリケーションは、ユーザー、ログインを完了またはサイズの大きなファイルのダウンロードを完了する必要があります。
-  **必要なバック グラウンド アプリケーションとして登録**-アプリがなどのアプリケーションが知られている特定の backgrounding 要件の特定の種類として登録できます*オーディオ*、 *VoIP* 、 *外部アクセサリ*、 *Newsstand* 、および*場所*です。 これらのアプリケーションでは、継続的なバック グラウンドの登録済みのアプリケーションの種類のパラメーターの範囲内のあるタスクを実行している限り、特権を処理が許可されます。
-  **バック グラウンド更新を有効にする**-アプリケーションがバック グラウンド更新プログラムをトリガーできます*領域監視*またはリッスンして*場所の大幅な変更*です。 IOS 7、時点でのアプリケーションも登録できますを使用して、バック グラウンドでコンテンツを更新する*バック グラウンドでフェッチ*または*リモート通知*です。


## <a name="application-states-and-application-delegate-methods"></a>アプリケーションの状態とアプリケーション デリゲート メソッド

バック グラウンド処理の iOS でのコードに進む前に iOS アプリケーションのライフ サイクル backgrounding 方法に影響を理解する必要があります。

IOS アプリケーションのライフ サイクルは、アプリケーションの状態とそれらの間を移動するためのメソッドのコレクションです。 アプリケーションは、ユーザーの動作と、アプリケーションの backgrounding 要件に基づいて状態の間で移行します。 移動は次の図で例を示します。

 [![](introduction-to-backgrounding-in-ios-images/applicationlifecycle-.png "アプリケーションの状態とアプリケーション デリゲート メソッドのダイアグラム")](introduction-to-backgrounding-in-ios-images/applicationlifecycle-.png#lightbox)

-  **実行されていない**-アプリケーションがデバイスに起動されていません。
-  **実行/アクティブ**-アプリケーションが、画面上にありフォア グラウンドでコードが実行されています。
-  **非アクティブな**-かかってきた電話、テキスト、またはその他の中断によって、アプリケーションが中断されました。
-  **Backgrounded** -アプリケーションがバック グラウンドに移動して、バック グラウンド コードの実行を続行します。
-  **Suspended** - 場合は、アプリケーションには、バック グラウンドで実行するためのコードはありません。 または、すべてのコードが完了したら、アプリとなる*Suspended* OS によってです。 中断されているアプリケーションのプロセスが維持が、アプリケーションはこの状態ですべてのコードを実行することはできません。
-  **ない実行/終了 (Rare) を返す**- 場合によっては、アプリケーションのプロセスが破棄され、アプリケーションに戻る、*が実行されていない*状態です。 これは、メモリ不足の状況で、ユーザーが手動でアプリケーションを終了した場合またはします。


マルチタスク サポートの概要については、以降 iOS がほとんどアイドル状態のアプリケーションを終了し、代わりに、プロセスを維持*Suspended*メモリにします。 アプリケーションのプロセスを履歴に保持するとは、アプリケーションは、次回ユーザーが開くことで起動簡単にすることを確認します。 アプリケーションを自由に移動できることも意味、 *Suspended*状態に戻す、 *Backgrounded*システム リソース上に描画せず状態です。 iOS 7 では、デバイスがスリープ状態、update からコンテンツに直接ユーザーの操作をバック グラウンドになったときに、バック グラウンド タスクを一時停止するアプリケーションを有効にする新しい Api を使用してこの機能を悪用します。 については、新しい Api を[iOS Backgrounding 手法](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/index.md)です。

## <a name="application-lifecycle-methods"></a>アプリケーション ライフ サイクル メソッド

アプリの状態が変わるときに iOS アプリケーションに通知しますイベント メソッドによって、`AppDelegate`クラス。

-  `OnActivated` -これはアプリケーションを起動すると、最初と呼ばれますが、アプリが前面に表示に戻るたびに、します。 これは、アプリが開かれるたびに実行する必要があるコードを配置する場所です。
-  `OnResignActivation` -ユーザーが受け取った場合、テキストや電話などの中断、このメソッドが呼び出され、アプリが一時的に非アクティブ化します。 ユーザーは電話メッセージを受け入れる必要があります、アプリは、バック グラウンドに送信されます。
-  `DidEnterBackground` -アプリ backgrounded 状態になったときに呼び出されると、このメソッドは、可能な終了の準備をするアプリケーション約 5 秒を与えます。 ユーザー データと、タスクを保存し、画面から機密情報を削除するには、この時間を使用します。
-  `WillEnterForeground` -ユーザー backgrounded または停止中のアプリケーションに返すし、フォア グラウンドに起動`WillEnterForeground`と呼ばれるを取得します。 これは、保存中にいずれかの状態を復元することによって、前景色を実行するアプリを準備する時間`DidEnterBackground`です。  `OnActivated` このメソッドが完了した直後に呼び出されます。
-  `WillTerminate` -アプリケーションのシャット ダウンし、そのプロセスは破棄されます。 このメソッドだけが呼び出されるマルチタスク使用できない場合は、デバイスまたは OS のバージョンでメモリが小さい場合、またはユーザーが手動で backgrounded アプリケーションを終了した場合。 中断されているアプリケーションが終了した取得を呼び出さないことに注意してください`WillTerminate`です。


次の図は、アプリケーションの状態し、ライフ サイクル メソッドが継ぎ合わさ。

 [![](introduction-to-backgrounding-in-ios-images/image2.png "この図では、アプリケーションの状態し、ライフ サイクル メソッドの連携")](introduction-to-backgrounding-in-ios-images/image2.png#lightbox)

## <a name="user-controls-for-backgrounding-in-ios"></a>IOS で Backgrounding 用のユーザー コントロール

iOS 7 では、ユーザーがアプリケーションの backgrounded 状態より詳細に制御できるようにするいくつかの機能が導入されました。 アプリのスイッチャーと背景アプリの更新設定の両方は、アプリケーション ライフ サイクルに影響します。

### <a name="app-switcher"></a>アプリのスイッチャー

アプリの切り替えは、iOS 7 で導入された重要な管理機能です。 ダブルタップによって起動された、**ホーム**ボタンをクリックし、保持されているプロセスのアプリケーションを示します。

 [![](introduction-to-backgrounding-in-ios-images/app-switcher-.png "アプリのスイッチャーを使用してアプリ間での移動")](introduction-to-backgrounding-in-ios-images/app-switcher-.png#lightbox)

ユーザーはアプリのスイッチャーを使用すると、すべて backgrounded および中断されているアプリケーションのスナップショットをスクロールできます。 アプリケーションの順にタップは、フォア グラウンドに起動します。 そのプロセスを終了して、バック グラウンドからアプリケーションを削除するをスワイプします。 詳しく見てでアプリのスイッチャーが取得、 [iOS アプリケーション ライフ サイクルのデモ](~/ios/app-fundamentals/backgrounding/application-lifecycle-demo.md)次のセクションでします。

> [!IMPORTANT]
> アプリのスイッチャーでは、backgrounded と中断されたアプリケーションの違いは表示されません。



### <a name="background-app-refresh-settings"></a>バック グラウンド アプリの更新の設定

iOS 7 は、ユーザーがアプリケーションの backgrounding から除外することでアプリケーションのライフ サイクルにわたってユーザー コントロールを増加[バック グラウンド処理用に登録](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/registering-applications-to-run-in-background.md)です。 *これはアプリケーションのバック グラウンド タスクの実行を妨げません*です。

ユーザーに移動してこの設定を変更できます<span class="uiitem">設定 > [全般] > 背景アプリの更新</span>と選択したアプリケーションの backgrounding 特権を編集します。 背景アプリの更新は、オフに設定されているアプリケーションが、バック グラウンドの入力時に即座に中断されし、バック グラウンド処理を実行できませんでした。

 [![](introduction-to-backgrounding-in-ios-images/settings-.png "バック グラウンド アプリの更新の設定")](introduction-to-backgrounding-in-ios-images/settings-.png#lightbox)

開発者は、バック グラウンド更新アプリケーションの状態を確認できます、 `BackgroundRefreshStatus` API です。 例についてを参照してください、[バック グラウンド更新設定の確認のレシピ](https://developer.xamarin.com/recipes/ios/multitasking/check_background_refresh_setting/)です。

IOS アプリケーションのライフ サイクル、およびアプリケーションのライフ サイクルを制御するための機能の基礎を説明してきました。 次に、アクションで iOS アプリケーションのライフ サイクルを見てみましょう。

