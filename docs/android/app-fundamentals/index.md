---
title: Xamarin.Android アプリケーションの基礎
description: アプリケーションの中心概念
ms.prod: xamarin
ms.assetid: 935B8BFE-23B7-4239-5C87-F4A503B889CB
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 1d3bd071eeffb77f94a1b5f35f1df59f2d8c7a8a
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61022199"
---
# <a name="xamarinandroid-application-fundamentals"></a>Xamarin.Android アプリケーションの基礎

このセクションでは、いくつかの一般的なモ ノのタスクまたは開発者が Android アプリケーションを開発する際に注意する必要がある概念にガイドを提供します。

## <a name="accessibilityandroidapp-fundamentalsaccessibilitymd"></a>[ユーザー補助](~/android/app-fundamentals/accessibility.md)

このページは、Android のユーザー補助の Api を使用して、に従ってアプリを構築する方法をについて説明します、[アクセシビリティ チェックリスト](~/cross-platform/app-fundamentals/accessibility.md)します。

##  <a name="understanding-android-api-levelsandroidapp-fundamentalsandroid-api-levelsmd"></a>[Android API レベルを理解します。](~/android/app-fundamentals/android-api-levels.md)

このガイドでは、Android で API レベルを使用して、Android の異なるバージョン間でアプリケーションの互換性を管理する方法について説明し、アプリでこれらの API レベルを展開する Xamarin.Android プロジェクトの設定を構成する方法について説明します。 さらに、別の API レベルを処理するランタイム コードを記述する方法について説明し、Android API レベル、バージョン番号 (Android 8.0) など、Android のコード名 (など Oreo)、およびビルドのバージョン コードの参照リストを提供します。



##  <a name="resources-in-androidandroidapp-fundamentalsresources-in-androidindexmd"></a>[Android のリソース](~/android/app-fundamentals/resources-in-android/index.md)

この記事には、その使用方法 Xamarin.Android とドキュメントの Android のリソースの概念が導入されています。 アプリケーションのローカライズ、およびさまざまな画面サイズおよび密度を含む複数のデバイスをサポートするために、Android アプリケーションでリソースを使用する方法を説明します。




##  <a name="activity-lifecycleandroidapp-fundamentalsactivity-lifecycleindexmd"></a>[アクティビティのライフサイクル](~/android/app-fundamentals/activity-lifecycle/index.md)

アクティビティは、Android アプリケーションの基本的なビルディング ブロックと、さまざまな種類の状態で存在することができます。 アクティビティのライフ サイクルはインスタンス化で始まる破棄、終わるし、間に多くの状態が含まれています。 アクティビティの状態が変更されたとき適切なライフ サイクル イベントのメソッドは、アクティビティの兆候の状態の変更を通知して、その変更に合わせてコードを実行することができますに呼び出されます。 この記事では、責任について説明します調べアクティビティのライフ サイクル中に適切に動作、信頼性の高いアプリケーションの一部としてこれらの状態変更の各アクティビティがあります。

##  <a name="localizationandroidapp-fundamentalslocalizationmd"></a>[ローカリゼーション](~/android/app-fundamentals/localization.md)

この記事では、文字列を翻訳およびイメージの代替を提供する他の言語に Xamarin.Android をローカライズする方法について説明します。

## <a name="servicesandroidapp-fundamentalsservicesindexmd"></a>[Services](~/android/app-fundamentals/services/index.md)

この記事では、Android のサービスは、バック グラウンドで実行する作業を許可する Android のコンポーネントについて説明します。 サービスに適したさまざまなシナリオについて説明し、リモート プロシージャ コールにインターフェイスを提供するための同様の実行時間の長いバック グラウンド タスクを実行するための両方に実装する方法を示しています。

## <a name="broadcast-receiversandroidapp-fundamentalsbroadcast-receiversmd"></a>[ブロードキャスト レシーバー](~/android/app-fundamentals/broadcast-receivers.md)

このガイドでは、Xamarin.Android でのシステム全体のブロードキャストに応答する Android のコンポーネントを作成し、ブロードキャスト レシーバーを使用する方法について説明します。



##  <a name="permissionsandroidapp-fundamentalspermissionsmd"></a>[アクセス許可](~/android/app-fundamentals/permissions.md)

Visual Studio for Mac または Visual Studio に組み込まれているツールのサポートを使用して、作成し、Android マニフェストへのアクセス許可を追加することができます。 このドキュメントでは、Visual Studio と Xamarin Studio でのアクセス許可を追加する方法について説明します。



##  <a name="graphics-and-animationandroidapp-fundamentalsgraphics-and-animationmd"></a>[グラフィックスとアニメーション](~/android/app-fundamentals/graphics-and-animation.md)

Android では、2 D グラフィックスとアニメーションをサポートするため、非常に豊富で多様なフレームワークを提供します。 このドキュメントでは、これらのフレームワークを紹介し、カスタムのグラフィックスとアニメーションを作成して、Xamarin.Android アプリケーションで使用する方法について説明します。


##  <a name="cpu-architecturesandroidapp-fundamentalscpu-architecturesmd"></a>[CPU アーキテクチャ](~/android/app-fundamentals/cpu-architectures.md)

Xamarin.Android には、32 ビットおよび 64 ビットのデバイスを含め、いくつかの CPU アーキテクチャがサポートされています。 この記事では、1 つまたは複数の Android でサポートされている CPU アーキテクチャにアプリをターゲットにする方法について説明します。




##  <a name="handling-rotationandroidapp-fundamentalshandling-rotationmd"></a>[回転の処理](~/android/app-fundamentals/handling-rotation.md)

この記事では、Xamarin.Android でデバイスの向きの変更を処理する方法について説明します。 プログラムで印刷の向きを処理するために変更する方法も、特定のデバイスの向きのリソースを自動的に読み込む Android リソース システムを使用する方法を説明します。 デバイスを回転したときに、状態を維持するための手法について説明します。



##  <a name="android-audioandroidapp-fundamentalsandroid-audiomd"></a>[Android のオーディオ](~/android/app-fundamentals/android-audio.md)

Android OS は、オーディオとビデオの両方を含むマルチ メディア、広範なサポートを提供します。 このガイドでは、Android でのオーディオを重視し、再生および録音オーディオの低レベルの API だけでなく、組み込みのオーディオ プレーヤーとレコーダー クラス、使用について説明します。 開発者が適切に動作するアプリケーションを作成できるように、他のアプリケーションによってブロードキャストされたオーディオ イベントの処理も説明します。




##  <a name="notificationsandroidapp-fundamentalsnotificationsindexmd"></a>[通知](~/android/app-fundamentals/notifications/index.md)

このセクションでは、Xamarin.Android でのローカルとリモート通知を実装する方法について説明します。 Android の通知のさまざまな UI 要素について説明し、API について説明を作成して、通知を表示するのに関係するのです。 リモート通知を行うには、Google Cloud Messaging と Firebase Cloud Messaging の両方を説明します。 ステップ バイ ステップのチュートリアルとコード サンプルが含まれます。



##  <a name="touchandroidapp-fundamentalstouchindexmd"></a>[タッチ](~/android/app-fundamentals/touch/index.md)

このセクションでは、概念と実装の詳細については、Android でジェスチャをタッチについて説明します。 タッチ Api が導入され、その後でジェスチャ レコグナイザーの探索について説明します。



##  <a name="httpclient-stack-and-ssltlsandroidapp-fundamentalshttp-stackmd"></a>[HttpClient スタックと SSL/TLS](~/android/app-fundamentals/http-stack.md)

このセクションには、Android 用の HttpClient スタックと SSL/TLS の実装セレクターがについて説明します。 これらの設定は、Xamarin.Android アプリで使用する HttpClient と SSL/TLS の実装を決定します。


##  <a name="writing-responsive-applicationswriting-responsive-appsmd"></a>[応答性の高いアプリケーションの作成](writing-responsive-apps.md)

この記事では、スレッドを使用して、バック グラウンド スレッドに実行時間の長いタスクを移動することによって、Xamarin.Android アプリケーションの応答性を維持する方法について説明します。