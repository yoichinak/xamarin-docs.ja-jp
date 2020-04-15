---
title: Android のリリース ビルドがインターネットに接続できないのはなぜですか
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: A6FE770B-A19A-4BF8-95E9-2CF880D4AFC5
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/09/2018
ms.openlocfilehash: 5996cfa3c0a18fc186ea862a2b3d7910594e1281
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "73027008"
---
# <a name="why-cant-my-android-release-build-connect-to-the-internet"></a>Android のリリース ビルドがインターネットに接続できないのはなぜですか

## <a name="cause"></a>原因

この問題の最も一般的な原因は、**INTERNET** アクセス許可が、デバッグ ビルドには自動的に含まれるのに対し、リリース ビルドでは手動で設定する必要があることです。 これは、デバッガーがプロセスにアタッチできるようにするために、インターネット アクセス許可が使用されるためです ([こちら](~/android/deploy-test/building-apps/build-process.md)の "DebugSymbols" を参照)。

## <a name="fix"></a>修正

この問題を解決するには、Android マニフェストでインターネット アクセス許可を要求することができます。 これは、マニフェスト エディターまたはマニフェストのソース コードのいずれかを使用して実行できます。

- エディターで修正する場合:Android プロジェクトで、 **[プロパティ] -> AndroidManifest.xml -> [必要なアクセス許可]** に移動し、 **[インターネット]** をオンにします。

- ソース コードで修正する場合:ソース エディターで AndroidManifest を開き、`<Manifest>` タグに permission タグを追加します。

    ```xml
    <Manifest>
    ...
    <uses-permission android:name="android.permission.INTERNET" />
    </Manifest>
    ```
