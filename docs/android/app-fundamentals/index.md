---
title: Xamarin.Android アプリケーションの基礎
description: 中核となるアプリケーションの概念
ms.prod: xamarin
ms.assetid: 935B8BFE-23B7-4239-5C87-F4A503B889CB
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: caa51fa0a70da1b799d56c706e6de974f61a14d9
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/03/2018
---
# <a name="xamarinandroid-application-fundamentals"></a>Xamarin.Android アプリケーションの基礎

このセクションでは、いくつかの一般的な処理タスクまたは Android アプリケーションを開発するときに注意する必要がある開発者の概念のガイドを提供します。

## <a name="accessibilityandroidapp-fundamentalsaccessibilitymd"></a>[ユーザー補助](~/android/app-fundamentals/accessibility.md)

Api を使用して、Android ユーザー補助機能によるとアプリをビルドする方法の説明、[アクセシビリティ チェックリスト](~/cross-platform/app-fundamentals/accessibility.md)です。

##  <a name="understanding-android-api-levelsandroidapp-fundamentalsandroid-api-levelsmd"></a>[Android API レベルを理解します。](~/android/app-fundamentals/android-api-levels.md)

このガイドでは、Android で API レベルを使用して、Android の異なるバージョン間でアプリケーションの互換性を管理する方法について説明し、アプリでこれらの API レベルを展開する Xamarin.Android プロジェクトの設定を構成する方法についても説明します。 さらに、このガイドの異なる API レベルを処理するランタイム コードを記述する方法について説明し、すべての Android API レベル、バージョン番号 (Android 8.0) など、Android のコード名 (など Oreo)、およびビルドのバージョンのコードの参照一覧を提供します。



##  <a name="resources-in-androidandroidapp-fundamentalsresources-in-androidindexmd"></a>[Android でのリソース](~/android/app-fundamentals/resources-in-android/index.md)

この記事には、その使用方法 Xamarin.Android とドキュメントの Android のリソースの概念が導入されています。 アプリケーションのローカライズ、およびさまざまな画面サイズと密度を含む複数のデバイスをサポートするために Android アプリのリソースを使用する方法を説明します。




##  <a name="activity-lifecycleandroidapp-fundamentalsactivity-lifecycleindexmd"></a>[アクティビティのライフサイクル](~/android/app-fundamentals/activity-lifecycle/index.md)

アクティビティは、Android アプリケーションの基本的なビルディング ブロックと、さまざまな異なる状態で存在することができます。 アクティビティのライフ サイクルは、インスタンス化で始まり、破棄で終わると間に多くの状態が含まれています。 アクティビティの状態が変わるときに、適切なライフ サイクル イベント メソッドは起こる状態のアクティビティに通知され、その変更に合わせて調整するコードを実行するように呼び出されます。 この記事は、アクティビティのライフ サイクルを検査し、責任について説明します、適切に動作で信頼性の高いアプリケーションの一部としてこれらの状態変更の各アクティビティがあります。

##  <a name="localizationandroidapp-fundamentalslocalizationmd"></a>[ローカリゼーション](~/android/app-fundamentals/localization.md)

この記事では、他の言語に文字列を変換し、代替のイメージを提供することによって、Xamarin.Android をローカライズする方法について説明します。

## <a name="servicesandroidapp-fundamentalsservicesindexmd"></a>[サービス](~/android/app-fundamentals/services/index.md)

この記事では、Android のサービス、バック グラウンドで実行する作業を許可する Android のコンポーネントについて説明します。 サービスが適しているさまざまなシナリオについて説明し、両方のタスクを実行する実行時間の長い背景もリモート プロシージャ コールにインターフェイスを提供するために実装する方法を示します。

## <a name="broadcast-receiversandroidapp-fundamentalsbroadcast-receiversmd"></a>[ブロードキャスト レシーバー](~/android/app-fundamentals/broadcast-receivers.md)

このガイドでは、システム全体のブロードキャストを Xamarin.Android 内に応答するために Android コンポーネント作成および放送受信機を使用する方法について説明します。



##  <a name="permissionsandroidapp-fundamentalspermissionsmd"></a>[アクセス許可](~/android/app-fundamentals/permissions.md)

Mac 用の Visual Studio または Visual Studio に組み込まれているツール サポートを使用して、作成し、Android マニフェストへのアクセス許可を追加することができます。 このドキュメントでは、Visual Studio と Xamarin Studio でのアクセス許可を追加する方法について説明します。



##  <a name="graphics-and-animationandroidapp-fundamentalsgraphics-and-animationmd"></a>[グラフィックスとアニメーション](~/android/app-fundamentals/graphics-and-animation.md)

Android では、2 次元グラフィックとアニメーションをサポートするため、非常に豊富なさまざまなフレームワークを提供します。 このドキュメントでは、これらのフレームワークを紹介し、カスタム グラフィックとアニメーションを作成および Xamarin.Android アプリケーションで使用する方法について説明します。


##  <a name="cpu-architecturesandroidapp-fundamentalscpu-architecturesmd"></a>[CPU アーキテクチャ](~/android/app-fundamentals/cpu-architectures.md)

Xamarin.Android には、32 ビットおよび 64 ビットのデバイスも含め、複数の CPU アーキテクチャがサポートされています。 この記事では、1 つまたは複数の Android でサポートされている CPU アーキテクチャにアプリを対象にする方法について説明します。




##  <a name="handling-rotationandroidapp-fundamentalshandling-rotationmd"></a>[回転の処理](~/android/app-fundamentals/handling-rotation.md)

この記事では、Xamarin.Android でデバイスの向きの変更を処理する方法について説明します。 自動的に変更をプログラムによって印刷の向きを処理する方法と同様、特定のデバイスの向きのリソースを読み込む Android リソース システムを操作する方法を説明します。 デバイスを回転したときに状態を維持するための手法について説明します。



##  <a name="android-audioandroidapp-fundamentalsandroid-audiomd"></a>[Android のオーディオ](~/android/app-fundamentals/android-audio.md)

Android OS は、オーディオとビデオの両方を含むマルチ メディア、広範なサポートを提供します。 このガイドでは、Android でのオーディオを重視し、再生組み込みオーディオ プレーヤーとレコーダー クラスだけでなく、低レベルのオーディオ API を使用してオーディオを録音したりについて説明します。 開発者が適切に動作のアプリケーションを構築できるように、他のアプリケーションによってブロードキャストされたオーディオ イベントの処理も取り上げています。




##  <a name="notificationsandroidapp-fundamentalsnotificationsindexmd"></a>[通知](~/android/app-fundamentals/notifications/index.md)

このセクションでは、Xamarin.Android にローカルとリモートの通知を実装する方法について説明します。 Android の通知のさまざまな UI 要素について説明し、API について説明します、通知が表示の作成と関係しているのです。 リモート通知を行うには、Google Cloud Messaging と Firebase Cloud Messaging の両方を説明します。 ステップ バイ ステップのチュートリアルとサンプル コードが含まれます。



##  <a name="touchandroidapp-fundamentalstouchindexmd"></a>[タッチ](~/android/app-fundamentals/touch/index.md)

このセクションでは、タッチ Android でのジェスチャの概念と実装の詳細について説明します。 タッチ Api が導入され、その後に、探索ジェスチャ レコグナイザーの説明。



##  <a name="httpclient-stack-and-ssltlsandroidapp-fundamentalshttp-stackmd"></a>[HttpClient スタックと SSL/TLS](~/android/app-fundamentals/http-stack.md)

このセクションには、Android 用の HttpClient スタックと SSL/TLS 実装セレクターがについて説明します。 これらの設定は、Xamarin.Android アプリで使用する HttpClient および SSL や TLS の実装を決定します。


##  <a name="writing-responsive-applicationswriting-responsive-appsmd"></a>[応答性の高いアプリケーションの作成](writing-responsive-apps.md)

この記事では、スレッド処理を使用して、バック グラウンド スレッドに実行時間の長いタスクを移動することによって、Xamarin.Android アプリケーションの応答性を維持する方法について説明します。