---
title: 指紋の登録
description: 画面ロックを設定し、Android デバイスまたはエミュレーターに指紋を登録する方法。
ms.prod: xamarin
ms.assetid: 52092F63-00EE-4F8B-A49F-65C9CCBA7EF2
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: c0290dfa3b4aa301a07a589f78577899e8282158
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "73027595"
---
# <a name="enrolling-a-fingerprint"></a>指紋の登録

## <a name="enrolling-a-fingerprint-overview"></a>指紋の登録の概要

デバイスが指紋認証を使用して既に構成されている場合は、Android アプリケーションのみで指紋認証を利用するができます。 このガイドでは、Android デバイスまたはエミュレーターに指紋を登録する方法について説明します。 エミュレーターに指紋スキャンを実行するための実際のハードウェアは備わっていませんが、Android Debug Bridge のヘルプを使用して指紋スキャンをシミュレートすることができます (以下で説明)。  このガイドでは、Android デバイスで画面ロックを有効にし、認証用の指紋を登録する方法について説明します。

## <a name="requirements"></a>必要条件

指紋を登録するには、Android デバイス、または API レベル 23 (Android 6.0) を実行しているエミュレーターが必要です。

Android Debug Bridge (ADB) を使用するには、コマンド プロンプトを使い慣れている必要があります。また、**adb** 実行可能ファイルは、Bash、PowerShell、またはコマンド プロンプト環境の PATH に存在する必要があります。

## <a name="configuring-a-screen-lock-and-enrolling-a-fingerprint"></a>画面ロックの構成と指紋の登録 

画面ロックを設定するには、次の手順を行います。

1. **[設定] > [セキュリティ]** にアクセスし、 **[画面ロック]** を選択します。

    ![[セキュリティ] 画面での画面ロック選択の場所](enrolling-fingerprint-images/testing-01.png)

2. 次の画面が表示されるので、画面ロックにおけるいずれかのセキュリティの方法を選択して構成できます。 

    ![スワイプ、パターン、PIN、またはパスワードの選択](enrolling-fingerprint-images/testing-02.png)

   使用可能な画面ロックの方法の 1 つを選択して完了します。

3. 画面ロックが構成されたら、 **[設定] > [セキュリティ]** ページに戻り、 **[指紋]** を選択します。

    ![[セキュリティ] 画面での指紋選択の場所](enrolling-fingerprint-images/testing-03.png)

4. そこからシーケンスに従って、デバイスに指紋を追加します。

    [![デバイスに指紋を追加するためのスクリーンショットのシーケンス](enrolling-fingerprint-images/testing-04-sml.png)](enrolling-fingerprint-images/testing-04.png#lightbox)

5. 最後の画面で、指紋スキャナーに指を配置するように求められます。 

    ![指をセンサーに置くように求められる画面](enrolling-fingerprint-images/testing-05.png)

    Android デバイスを使用している場合は、スキャナーに指をタッチすることによってプロセスを完了します。 

### <a name="simulating-a-fingerprint-scan-on-the-emulator"></a>エミュレーターでの指紋スキャンのシミュレーション

Android エミュレーターでは、Android Debug Bridge を使用して指紋スキャンをシミュレートすることができます。 OS X ではターミナル セッションが開始されますが、Windows ではコマンド プロンプトまたは PowerShell セッションが起動され、`adb` が実行されます。

```shell
$ adb -e emu finger touch 1
```

値 **1** は、"スキャン済み" となった指の _finger\_id_ です。 これは、仮想指紋ごとに割り当てる一意の整数です。 今後アプリが実行されている際、エミュレーターが指紋を要求するたびに、この同じ ADB コマンドを実行できます。また、`adb` コマンドを実行して _finger\_id_ に渡し、指紋スキャンをシミュレートすることができます。

指紋のスキャンが完了すると、次のように、Android から指紋が追加されたことが通知されます。  

![指紋が追加されたことを示す画面](enrolling-fingerprint-images/testing-06.png)

## <a name="summary"></a>まとめ 

このガイドでは、画面ロックを設定し、Android デバイスまたは Android Emulator に指紋を登録する方法について説明しました。 
