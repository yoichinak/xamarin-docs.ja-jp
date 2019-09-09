---
title: Android のリリース ビルドがインターネットに接続できないのはなぜですか
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: A6FE770B-A19A-4BF8-95E9-2CF880D4AFC5
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: 162378c00f3e20574d04dc373fcc492a9407b88d
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70761028"
---
# <a name="why-cant-my-android-release-build-connect-to-the-internet"></a>Android のリリース ビルドがインターネットに接続できないのはなぜですか

## <a name="cause"></a>原因

この問題の最も一般的な原因は、**インターネット**のアクセス許可がデバッグビルドに自動的に含まれ、リリースビルドに対して手動で設定する必要があることです。 これは、[ここで](~/android/deploy-test/building-apps/build-process.md)"debugsymbols" について説明されているように、デバッガーがプロセスにアタッチできるようにするために、インターネットアクセス許可が使用されるためです。

## <a name="fix"></a>Fix

この問題を解決するには、Android マニフェストでインターネットアクセス許可を要求することができます。 これは、マニフェストエディターまたはマニフェストの sourcecode のいずれかを使用して実行できます。

- エディターでの修正:Android プロジェクトで、[プロパティ] **-> AndroidManifest .xml-> 必要なアクセス許可**を指定し、 **[インターネット]** を確認します。

- Sourcecode での修正:ソースエディターで androidmanifest を開き、 `<Manifest>`タグ内に permission タグを追加します。

    ```xml
    <Manifest>
    ...
    <uses-permission android:name="android.permission.INTERNET" />
    </Manifest>
    ```
