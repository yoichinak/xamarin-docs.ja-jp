---
title: "Android のリリース ビルドが、インターネットに接続できない理由をしますか。"
ms.topic: article
ms.prod: xamarin
ms.assetid: A6FE770B-A19A-4BF8-95E9-2CF880D4AFC5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.openlocfilehash: 60da469001f8a9641aff7e014add5f58bdb389cb
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="why-cant-my-android-release-build-connect-to-the-internet"></a>Android のリリース ビルドが、インターネットに接続できない理由をしますか。

## <a name="cause"></a>原因

この問題の最も一般的な原因は、**インターネット**権限は、デバッグ ビルドで自動的に含まれるが、リリース ビルドを手動で設定する必要があります。 これは、インターネット アクセス許可を「DebugSymbols」の説明に従って、プロセスにアタッチするデバッガーを許可する使用ため[ここ](~/android/deploy-test/building-apps/build-process.md)です。


## <a name="fix"></a>修正

問題を解決するには、Android のマニフェストでインターネット アクセス許可を要求できます。 これは、マニフェスト エディターまたはマニフェストのソースコードから実行できます。

-   エディターで修正: Android のプロジェクトに移動**プロパティ AndroidManifest.xml]-> [必要なアクセス許可]-> [**チェックと**インターネット**

-   ソースコードで修正: ソース エディターで、AndroidManifest を開き、内のアクセス許可のタグを追加、`<Manifest>`タグ。

    ```xml
    <Manifest>
    ...
    <uses-permission android:name="android.permission.INTERNET" />
    </Manifest>
    ```
