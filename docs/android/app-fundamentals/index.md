---
title: Xamarin Android アプリケーションの基礎
description: コアアプリケーションの概念
ms.prod: xamarin
ms.assetid: 935B8BFE-23B7-4239-5C87-F4A503B889CB
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: eb581d68f3b7e57975b6979fe1005b1fac411ec8
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73019218"
---
# <a name="xamarinandroid-application-fundamentals"></a>Xamarin Android アプリケーションの基礎

このセクションでは、開発者が Android アプリケーションを開発する際に注意する必要がある、一般的なタスクまたは概念のいくつかについて説明します。

## <a name="accessibilityandroidapp-fundamentalsaccessibilitymd"></a>[ユーザー補助](~/android/app-fundamentals/accessibility.md)

このページでは、[ユーザー補助のチェックリスト](~/cross-platform/app-fundamentals/accessibility.md)に従って、Android アクセシビリティ api を使用してアプリを構築する方法について説明します。

## <a name="understanding-android-api-levelsandroidapp-fundamentalsandroid-api-levelsmd"></a>[Android API レベルについて](~/android/app-fundamentals/android-api-levels.md)

このガイドでは、Android が API レベルを使用してさまざまなバージョンの Android 間のアプリの互換性を管理する方法について説明します。また、これらの API レベルをアプリにデプロイするように Xamarin Android プロジェクト設定を構成する方法についても説明します。 また、このガイドでは、さまざまな API レベルを扱うランタイムコードを記述する方法について説明します。また、すべての Android API レベルのリファレンスリスト、バージョン番号 (Android 8.0 など)、Android コード名 (Oreo など)、ビルドバージョンコードを提供します。

## <a name="resources-in-androidandroidapp-fundamentalsresources-in-androidindexmd"></a>[Android のリソース](~/android/app-fundamentals/resources-in-android/index.md)

この記事では、Xamarin. Android の Android リソースの概念と、それらの使用方法について説明します。 Android アプリケーションのリソースを使用してアプリケーションのローカリゼーションをサポートする方法と、さまざまな画面サイズや密度を含む複数のデバイスを使用する方法について説明します。

## <a name="activity-lifecycleandroidapp-fundamentalsactivity-lifecycleindexmd"></a>[アクティビティのライフサイクル](~/android/app-fundamentals/activity-lifecycle/index.md)

アクティビティは Android アプリケーションの基本的な構成要素であり、さまざまな状態に存在できます。 アクティビティのライフサイクルは、インスタンス化で始まり、破棄で終了し、間に多くの状態が含まれます。 アクティビティが状態を変更すると、適切なライフサイクルイベントメソッドが呼び出され、発生した状態変更のアクティビティが通知され、その変更に合わせてコードを実行できるようになります。 この記事では、アクティビティのライフサイクルについて説明し、これらの各状態の変化におけるアクティビティが、適切に動作し、信頼性の高いアプリケーションの一部となる責任について説明します。

## <a name="localizationandroidapp-fundamentalslocalizationmd"></a>[ローカリゼーション](~/android/app-fundamentals/localization.md)

この記事では、文字列を翻訳し、代替イメージを提供することで、Xamarin を他の言語にローカライズする方法について説明します。

## <a name="servicesandroidapp-fundamentalsservicesindexmd"></a>[サービス](~/android/app-fundamentals/services/index.md)

この記事では、バックグラウンドで作業を行うことができる Android コンポーネントである Android サービスについて説明します。 サービスがに適しているさまざまなシナリオについて説明し、実行時間の長いバックグラウンドタスクを実行したり、リモートプロシージャコールのインターフェイスを提供したりするために、サービスを実装する方法を示します。

## <a name="broadcast-receiversandroidapp-fundamentalsbroadcast-receiversmd"></a>[ブロードキャスト レシーバー](~/android/app-fundamentals/broadcast-receivers.md)

このガイドでは、Xamarin Android のシステム全体のブロードキャストに応答する Android コンポーネントである、ブロードキャストレシーバーを作成して使用する方法について説明します。

## <a name="permissionsandroidapp-fundamentalspermissionsmd"></a>[アクセス許可](~/android/app-fundamentals/permissions.md)

Visual Studio for Mac または Visual Studio に組み込まれているツールのサポートを使用して、Android マニフェストにアクセス許可を作成し、追加することができます。 このドキュメントでは、Visual Studio と Xamarin Studio にアクセス許可を追加する方法について説明します。

## <a name="graphics-and-animationandroidapp-fundamentalsgraphics-and-animationmd"></a>[グラフィックスとアニメーション](~/android/app-fundamentals/graphics-and-animation.md)

Android には、2D グラフィックスとアニメーションをサポートするための豊富で多様なフレームワークが用意されています。 このドキュメントでは、これらのフレームワークについて説明し、カスタムグラフィックスとアニメーションを作成して Xamarin Android アプリケーションで使用する方法について説明します。

## <a name="cpu-architecturesandroidapp-fundamentalscpu-architecturesmd"></a>[CPU アーキテクチャ](~/android/app-fundamentals/cpu-architectures.md)

Xamarin Android では、32ビットと64ビットのデバイスを含む複数の CPU アーキテクチャがサポートされています。 この記事では、1つまたは複数の Android でサポートされている CPU アーキテクチャにアプリを対象にする方法について説明します。

## <a name="handling-rotationandroidapp-fundamentalshandling-rotationmd"></a>[回転の処理](~/android/app-fundamentals/handling-rotation.md)

この記事では、Xamarin Android でのデバイスの向きの変更を処理する方法について説明します。 Android リソースシステムを使用して、特定のデバイスの向きに関するリソースを自動的に読み込み、方向の変更をプログラムで処理する方法についても説明します。 次に、デバイスがローテーションされたときの状態を維持するための手法について説明します。

## <a name="android-audioandroidapp-fundamentalsandroid-audiomd"></a>[Android オーディオ](~/android/app-fundamentals/android-audio.md)

Android OS では、オーディオとビデオの両方を含むマルチメディアを広範囲にサポートしています。 このガイドでは、Android のオーディオに焦点を当て、組み込みのオーディオプレーヤーとレコーダークラス、および低レベルのオーディオ API を使用したオーディオの再生と録音について説明します。 また、開発者が適切に動作するアプリケーションを構築できるように、他のアプリケーションによってブロードキャストされるオーディオイベントの操作についても説明します。

## <a name="notificationsandroidapp-fundamentalsnotificationsindexmd"></a>[通知](~/android/app-fundamentals/notifications/index.md)

このセクションでは、Xamarin Android でローカル通知とリモート通知を実装する方法について説明します。 Android の通知のさまざまな UI 要素について説明し、通知の作成と表示に関連する API について説明します。 リモート通知については、Google Cloud Messaging と Firebase クラウドメッセージングの両方について説明します。 ステップバイステップのチュートリアルとコードサンプルが含まれています。

## <a name="touchandroidapp-fundamentalstouchindexmd"></a>[タッチ](~/android/app-fundamentals/touch/index.md)

このセクションでは、Android でのタッチジェスチャの実装の概念と詳細について説明します。 タッチ Api が導入され、その後にジェスチャレコグナイザーを探索できるようになりました。

## <a name="httpclient-stack-and-ssltlsandroidapp-fundamentalshttp-stackmd"></a>[HttpClient スタックと SSL/TLS](~/android/app-fundamentals/http-stack.md)

このセクションでは、Android 用の HttpClient スタックと SSL/TLS の実装セレクターについて説明します。 これらの設定によって、Xamarin Android アプリで使用される HttpClient と SSL/TLS の実装が決まります。

## <a name="writing-responsive-applicationswriting-responsive-appsmd"></a>[応答性の高いアプリケーションの作成](writing-responsive-apps.md)

この記事では、実行時間の長いタスクをバックグラウンドスレッドに移動して、Xamarin Android アプリケーションの応答性を維持するために、スレッド処理を使用する方法について説明します。
