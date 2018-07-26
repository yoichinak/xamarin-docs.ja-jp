---
title: IOS でのバック グラウンド処理の概要
description: このドキュメントでは、iOS でバック グラウンド処理について説明します。 アプリケーションの状態、アプリケーション ライフ サイクル メソッドをおよびバック グラウンド アプリで更新します。
ms.prod: xamarin
ms.assetid: E214F2C7-E74E-46C7-B5BA-080B30D61250
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 804a99817f664989bbac67a4c662357f4ee628c5
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242278"
---
# <a name="introduction-to-backgrounding-in-ios"></a>IOS でのバック グラウンド処理の概要

iOS では、バック グラウンド処理が非常に厳格に制御していますし、それを実装する 3 つのアプローチを提供しています。

-  **バック グラウンド タスクを登録**-アプリケーションがバック グラウンドに移動したときにタスクを中断が iOS を依頼できますをアプリケーションが、重要なタスクを完了する必要がある場合。 たとえば、アプリケーションは、ユーザー、ログインを完了したり、大きなファイルのダウンロードを完了する必要があります。
-  **必要なバック グラウンド アプリケーションとして登録**-アプリがなどのアプリケーションが知られている、特定のバック グラウンド処理の要件の特定の種類として登録できます*オーディオ*、 *VoIP* 、 *外部アクセサリ*、 *Newsstand* 、および*場所*します。 これらのアプリケーションでは、継続的なバック グラウンドの登録済みのアプリケーションの種類のパラメーター内にあるタスクを実行している限り、特権の処理が許可されます。
-  **バック グラウンドの更新を有効にする**-アプリケーションでバック グラウンド更新をトリガーできる*領域監視*またはリッスンして*場所の大幅な変更*。 使用して、バック グラウンドでコンテンツを更新する登録もアプリケーション時点で、iOS 7、*バック グラウンド フェッチ*または*リモート通知*します。


## <a name="application-states-and-application-delegate-methods"></a>アプリケーションの状態とアプリケーション デリゲート メソッド

説明のコードをバック グラウンドで iOS 処理に進む前に、iOS アプリケーションのライフ サイクルのバック グラウンド処理方法に影響を理解する必要があります。

IOS アプリケーションのライフ サイクルは、アプリケーションの状態とそれらの間を移動するためのメソッドのコレクションです。 アプリケーションは、ユーザーの動作と要件に基づき、backgrounding アプリケーションの状態が遷移します。 移動は、次の図で示されています。

 [![](introduction-to-backgrounding-in-ios-images/applicationlifecycle-.png "アプリケーションの状態とアプリケーション デリゲート メソッドのダイアグラム")](introduction-to-backgrounding-in-ios-images/applicationlifecycle-.png#lightbox)

-  **実行されていない**-アプリケーションがデバイスで起動されていません。
-  **実行/アクティブ**-アプリケーションが画面があり、コードのフォア グラウンドで実行します。
-  **非アクティブな**-着信の電話、テキスト、またはその他の中断によってアプリケーションが中断されました。
-  **Backgrounded** -アプリケーションがバック グラウンドに移動し、バック グラウンドのコードの実行を続けます。
-  **Suspended** - 場合は、アプリケーションには、バック グラウンドで実行するためのコードはありません。 または、すべてのコードが完了したら、アプリとなる*Suspended* OS によって。 中断されているアプリケーションのプロセスが保持が、アプリケーションがこの状態ですべてのコードを実行することではありません。
-  **ない実行/終了 (Rare) を返す**- 場合によっては、アプリケーションのプロセスが破棄され、アプリケーションに返す、*実行されていない*状態。 これは、メモリ不足の状況で、ユーザーが手動でアプリケーションを終了した場合またはします。


以降のマルチタス キングのサポートの概要については、iOS ことはほとんどありませんアイドル状態のアプリケーションを終了し、代わりに、プロセスを維持*Suspended*メモリにします。 アプリケーションのプロセスを履歴に保持すると、アプリケーションは、次回ユーザーが開くことで起動すばやくようにします。 アプリケーションを自由に移動できることも意味、 *Suspended*状態に戻す、 *Backgrounded*システム リソース上に描画せず状態。 iOS 7 では、アプリケーション、デバイスがスリープ状態にし、ユーザーの介入なしのバック グラウンドから直接更新プログラムのコンテンツには、バック グラウンド タスクを一時停止を有効にする新しい Api で、この機能を悪用します。 新しい Api が取り上げる[iOS バック グラウンド処理手法](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/index.md)します。

## <a name="application-lifecycle-methods"></a>アプリケーション ライフ サイクル メソッド

イベントのメソッドをとおして iOS アプリケーションに通知、アプリの状態が変更されたとき、`AppDelegate`クラス。

-  `OnActivated` -これは、初めてアプリケーションを起動すると呼びますたびに、アプリがフォア グラウンドに戻ったとします。 これは、アプリが開かれるたびに実行する必要があるコードを配置する場所です。
-  `OnResignActivation` -ユーザーは、テキストまたは通話呼び出しなど、中断を受信する場合は、このメソッドが呼び出され、アプリが一時的に非アクティブ化します。 ユーザーは電話呼び出しを受け入れる必要があります、アプリがバック グラウンドに送信されます。
-  `DidEnterBackground` -アプリ backgrounded 状態になったときに呼び出されると、このメソッドを可能な終了の準備を約 5 秒アプリケーションに提供します。 ユーザー データと、タスクを保存し、画面から機密情報を削除するには、この時間を使用します。
-  `WillEnterForeground` ユーザーが backgrounded または停止中のアプリケーションに返すし、フォア グラウンドにそれを起動-`WillEnterForeground`呼び出されます。 これは、フォア グラウンドにリハイド レート中に保存された状態で実行されるアプリを準備する時間`DidEnterBackground`します。  `OnActivated` このメソッドが完了した直後に呼び出されます。
-  `WillTerminate` -アプリケーションを終了し、そのプロセスは破棄されます。 このメソッドは、マルチタスクはメモリが小さい場合、またはユーザーが手動で backgrounded アプリケーションを終了した場合、デバイスまたは OS のバージョンでご利用いただけません場合使用呼び出されますのみです。 終了する中断されているアプリケーションは呼び出さないことに注意してください。`WillTerminate`します。


次の図は、アプリケーションの状態し、ライフ サイクル メソッドをまとめます。

 [![](introduction-to-backgrounding-in-ios-images/image2.png "この図では、アプリケーションの状態し、ライフ サイクル メソッドをまとめる")](introduction-to-backgrounding-in-ios-images/image2.png#lightbox)

## <a name="user-controls-for-backgrounding-in-ios"></a>IOS でバック グラウンド処理のユーザー コントロール

iOS 7 では、アプリケーションの backgrounded 状態の詳細に制御できるようにするいくつかの機能が導入されました。 アプリ スイッチャーおよびバック グラウンド アプリの更新設定の両方のアプリケーション ライフ サイクルに影響します。

### <a name="app-switcher"></a>アプリケーションのスイッチャー

アプリの切り替えは、iOS 7 で導入された重要な管理機能です。 ダブル タップによって起動されますが、**ホーム**ボタンをクリックし、プロセスが生きているアプリケーションを示しています。

 [![](introduction-to-backgrounding-in-ios-images/app-switcher-.png "アプリケーションのスイッチャーを使用してアプリ間の移動")](introduction-to-backgrounding-in-ios-images/app-switcher-.png#lightbox)

ユーザーは、アプリケーションのスイッチャーを使用して、backgrounded および保留中のすべてのアプリケーションのスナップショットをスクロールできます。 アプリケーションをタップすると、フォア グラウンドに起動します。 上方向にスワイプは、バック グラウンドでのプロセスを終了してから、アプリケーションを削除します。 ここでアプリケーションのスイッチャーについて詳しく見てになります、 [iOS アプリケーションのライフ サイクルのデモ](~/ios/app-fundamentals/backgrounding/application-lifecycle-demo.md)次のセクションでします。

> [!IMPORTANT]
> アプリの切り替えでは、backgrounded および保留中のアプリケーションの違いは表示されません。



### <a name="background-app-refresh-settings"></a>バック グラウンド アプリの更新の設定

iOS 7 では、アプリケーション ライフ サイクルにわたってユーザー コントロールを向上できるため、アプリケーションのバック グラウンド処理を無効にするユーザー[バック グラウンド処理用に登録されている](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/registering-applications-to-run-in-background.md)します。 *アプリケーションのバック グラウンド タスクを実行することは*します。

ユーザーに移動してこの設定を変更できます<span class="uiitem">設定 > [全般] > バック グラウンド アプリの更新</span>と編集の選択したアプリケーションの backgrounding 権限。 バック グラウンド アプリの更新がオフに設定されて、アプリケーションがバック グラウンドでの入力時にすぐに中断され、バック グラウンド処理を実行できなくなります。

 [![](introduction-to-backgrounding-in-ios-images/settings-.png "バック グラウンド アプリの更新の設定")](introduction-to-backgrounding-in-ios-images/settings-.png#lightbox)

開発者は、の アプリケーションのバック グラウンド更新状態を確認できます、 `BackgroundRefreshStatus` API。 例についてを参照してください、[バック グラウンド更新設定の確認レシピ](https://github.com/xamarin/recipes/tree/master/Recipes/ios/multitasking/check_background_refresh_setting)します。

IOS アプリケーションのライフ サイクル、およびアプリケーションのライフ サイクルを制御するための機能の基礎を取り上げました。 次に、アクションで iOS アプリケーションのライフ サイクルを見てみましょう。

