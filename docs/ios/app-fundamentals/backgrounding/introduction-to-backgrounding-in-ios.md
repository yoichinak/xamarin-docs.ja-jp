---
title: iOS におけるバックグラウンド処理の概要
description: このドキュメントでは、iOS でのバックグラウンド処理について説明します。アプリケーションの状態、アプリケーションのライフサイクル方法、およびバックグラウンドアプリの更新です。
ms.prod: xamarin
ms.assetid: E214F2C7-E74E-46C7-B5BA-080B30D61250
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 07/24/2018
ms.openlocfilehash: 78751f53808ffa62589fdc57fe4cf59912849e00
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73010841"
---
# <a name="introduction-to-backgrounding-in-ios"></a>iOS におけるバックグラウンド処理の概要

iOS は、バックグラウンド処理を厳密に制御し、3つのアプローチを実装する方法を提供します。

- **バックグラウンドタスクの登録**-アプリケーションが重要なタスクを完了する必要がある場合は、アプリケーションがバックグラウンドに移行するときにタスクを中断しないように iOS に要求できます。 たとえば、アプリケーションでユーザーのログインを完了したり、大きなファイルのダウンロードを完了したりする必要がある場合があります。
- **バックグラウンドで必要なアプリケーションとして登録**する-アプリは、*オーディオ*、 *VoIP* 、*外部アクセサリ*、 *Newsstand*など、既知の特定のバックグラウンド処理要件を持つ特定の種類のアプリケーションとして登録できます。との*場所*。 これらのアプリケーションは、登録されているアプリケーションの種類のパラメーター内にあるタスクを実行している限り、連続したバックグラウンド処理特権を許可されます。
- **バックグラウンド更新を有効にする**-アプリケーションは、*領域監視*を使用して、または*重要な場所の変更*をリッスンすることにより、バックグラウンド更新をトリガーできます。 IOS 7 以降では、アプリケーションはバックグラウンドでの*フェッチ*または*リモート通知*を使用して、バックグラウンドでコンテンツを更新するように登録することもできます。

## <a name="application-states-and-application-delegate-methods"></a>アプリケーションの状態とアプリケーションのデリゲートメソッド

IOS でのバックグラウンド処理のコードについて説明する前に、バックグラウンド処理が iOS アプリケーションのライフサイクルに与える影響について理解しておく必要があります。

IOS アプリケーションライフサイクルは、アプリケーションの状態と、それらの間を移動するためのメソッドのコレクションです。 アプリケーションは、ユーザーの動作とアプリケーションのバックグラウンド処理要件に基づいて、状態を遷移させることができます。 この移動を次の図に示します。

 [![](introduction-to-backgrounding-in-ios-images/applicationlifecycle-.png "Application States and Application Delegate Methods diagram")](introduction-to-backgrounding-in-ios-images/applicationlifecycle-.png#lightbox)

- 実行されて**いません**。アプリケーションはデバイス上でまだ起動されていません。
- **Running/アクティブ**-アプリケーションは画面上にあり、フォアグラウンドでコードを実行しています。
- **非アクティブ**-着信通話、テキスト、またはその他の中断によってアプリケーションが中断されます。
- **Backgrounded** -アプリケーションがバックグラウンドに移動し、バックグラウンドコードの実行を継続します。
- **[中断]** -アプリケーションにバックグラウンドで実行するコードがない場合、またはすべてのコードが完了している場合、アプリは OS によって*中断*されます。 中断されたアプリケーションのプロセスは保持されますが、アプリケーションはこの状態でコードを実行できません。
- [**実行されていない/終了 (まれ)] に戻り**ます。アプリケーションのプロセスが破棄され、アプリケーションが*実行されていない*状態に戻ることがあります。 これは、メモリが不足している場合や、ユーザーがアプリケーションを手動で終了した場合に発生します。

マルチタスクサポートが導入されたため、iOS はアイドル状態のアプリケーションを終了することはめったになく、代わりにプロセスをメモリ内で*中断*させます。 アプリケーションのプロセスをアクティブにすると、ユーザーが次回そのアプリケーションを開いたときに、アプリケーションがすぐに起動するようになります。 また、アプリケーションは、システムリソースを描画せずに、*中断*状態から*Backgrounded*状態に自由に移動できます。 iOS 7 では、デバイスがスリープ状態になったときにバックグラウンドタスクを一時停止し、ユーザーの操作なしでコンテンツをバックグラウンドから直接更新できるようにする新しい Api を使用して、この機能を利用しています。 新しい Api については、「 [IOS バックグラウンド処理技法](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/index.md)」で説明します。

## <a name="application-lifecycle-methods"></a>アプリケーションライフサイクルの方法

アプリが状態を変更すると、iOS は `AppDelegate` クラスのイベントメソッドを使用してアプリケーションに通知します。

- `OnActivated`-これは、アプリケーションが初めて起動されたときと、アプリがフォアグラウンドに戻るたびに呼び出されます。 これは、アプリが開かれるたびに実行する必要があるコードを配置する場所です。
- `OnResignActivation`-ユーザーがテキストや電話などの割り込みを受信した場合、このメソッドが呼び出され、アプリは一時的に非アクティブになります。 ユーザーが電話を受け入れると、アプリがバックグラウンドに送信されます。
- `DidEnterBackground`-アプリが backgrounded 状態になると呼び出されます。このメソッドは、可能な終了の準備として5秒程度のアプリケーションを提供します。 この時間を使用して、ユーザーデータとタスクを保存し、画面から機密情報を削除します。
- `WillEnterForeground`-ユーザーが backgrounded または中断されたアプリケーションに戻ったときに、そのアプリケーションをフォアグラウンドに起動すると、`WillEnterForeground` が呼び出されます。 これは、`DidEnterBackground` 中に保存された状態をリハイドレートして、フォアグラウンドを撮影するようにアプリを準備するための時間です。  `OnActivated` は、このメソッドが完了した直後に呼び出されます。
- `WillTerminate`-アプリケーションがシャットダウンされ、プロセスが破棄されます。 このメソッドは、デバイスまたは OS バージョンでマルチタスキングが使用できない場合、メモリが不足している場合、またはユーザーが手動で backgrounded アプリケーションを終了した場合にのみ呼び出されます。 中断されたアプリケーションが終了した場合、`WillTerminate` は呼び出されません。

次の図は、アプリケーションの状態とライフサイクルの方法をまとめたものです。

 [![](introduction-to-backgrounding-in-ios-images/image2.png "This diagram illustrates how the application states and lifecycle methods fit together")](introduction-to-backgrounding-in-ios-images/image2.png#lightbox)

## <a name="user-controls-for-backgrounding-in-ios"></a>IOS でのバックグラウンド処理のユーザーコントロール

iOS 7 では、ユーザーがアプリケーションの backgrounded 状態をより細かく制御できるように、いくつかの機能が導入されました。 アプリスイッチャーとバックグラウンドアプリの更新の両方の設定がアプリケーションのライフサイクルに影響します。

### <a name="app-switcher"></a>アプリスイッチャー

アプリスイッチャーは、iOS 7 で導入された重要な制御機能です。 **[ホーム]** ボタンをダブルタップすると起動され、プロセスが生きているアプリケーションが表示されます。

 [![](introduction-to-backgrounding-in-ios-images/app-switcher-.png "Moving between apps using the App Switcher")](introduction-to-backgrounding-in-ios-images/app-switcher-.png#lightbox)

アプリスイッチャーを使用すると、ユーザーは、すべての backgrounded アプリケーションと中断されたアプリケーションのスナップショットをスクロールできます。 アプリケーションをタップすると、それがフォアグラウンドに起動します。 スワイプすると、バックグラウンドからアプリケーションが削除され、プロセスが終了します。 アプリスイッチャーの詳細については、次のセクションの[IOS アプリケーションライフサイクルデモ](~/ios/app-fundamentals/backgrounding/application-lifecycle-demo.md)をご覧ください。

> [!IMPORTANT]
> アプリスイッチャーは、backgrounded アプリケーションと中断されたアプリケーションの違いを示すものではありません。

### <a name="background-app-refresh-settings"></a>バックグラウンドアプリ更新設定

iOS 7 では、[バックグラウンド処理用に登録さ](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/registering-applications-to-run-in-background.md)れたアプリケーションのバックグラウンド処理をユーザーがオプトアウトできるようにすることで、アプリケーションライフサイクルをユーザーが制御できるようになります。 *これによって、アプリケーションがバックグラウンドタスクを実行できなくなることはありません*。

この設定を変更するには、[設定] に移動して **[> 全般 > バックグラウンドアプリの更新**] を選択し、選択したアプリケーションのバックグラウンド処理特権を編集します。 バックグラウンドアプリの更新が off に設定されている場合、アプリケーションはバックグラウンドで入力されるとすぐに中断され、バックグラウンド処理を実行できなくなります。

 [![](introduction-to-backgrounding-in-ios-images/settings-.png "Background App Refresh Settings")](introduction-to-backgrounding-in-ios-images/settings-.png#lightbox)

開発者は、`BackgroundRefreshStatus` API を使用して、バックグラウンド更新アプリケーションの状態を確認できます。 例については、「[バックグラウンド更新設定のチェック](https://github.com/xamarin/recipes/tree/master/Recipes/ios/multitasking/check_background_refresh_setting)」を参照してください。

ここでは、iOS アプリケーションのライフサイクルの基本と、アプリケーションのライフサイクルを制御するための機能について説明しました。 次に、iOS アプリケーションのライフサイクルを実際に見てみましょう。
