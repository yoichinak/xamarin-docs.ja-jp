---
title: Android のリリース ビルドがインターネットに接続できないのはなぜですか
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: A6FE770B-A19A-4BF8-95E9-2CF880D4AFC5
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: cd27d5c884086cd0fade4364851039fd0cd915a0
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "60945462"
---
# <a name="why-cant-my-android-release-build-connect-to-the-internet"></a>Android のリリース ビルドがインターネットに接続できないのはなぜですか

## <a name="cause"></a>原因

この問題の最も一般的な原因は、**インターネット**アクセス許可は、デバッグ ビルドでは自動的に追加しますが、リリース ビルドを手動で設定する必要があります。 これは、"DebugSymbols"の説明に従って、プロセスにアタッチするデバッガーを許可するインターネット アクセス許可が使用されるため[ここ](~/android/deploy-test/building-apps/build-process.md)します。


## <a name="fix"></a>修正

この問題を解決するには、Android マニフェストでインターネット アクセス許可を要求できます。 これは、いずれかを使用してマニフェスト エディターまたはマニフェストのソースコードに実行できます。

-   エディターで修正します。Android プロジェクトに移動**プロパティ AndroidManifest.xml]-> [必要なアクセス許可]-> [** チェックと**インターネット**

-   Sourcecode で修復します。ソース エディターで、AndroidManifest を開き、内部アクセス許可のタグを追加、`<Manifest>`タグ。

    ```xml
    <Manifest>
    ...
    <uses-permission android:name="android.permission.INTERNET" />
    </Manifest>
    ```
