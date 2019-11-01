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
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027595"
---
# <a name="enrolling-a-fingerprint"></a>指紋の登録

## <a name="enrolling-a-fingerprint-overview"></a>指紋の登録の概要

デバイスが指紋認証を使用して既に構成されている場合は、Android アプリケーションで指紋認証を利用することはできません。 このガイドでは、Android デバイスまたはエミュレーターに指紋を登録する方法について説明します。 エミュレーターには指紋スキャンを実行するための実際のハードウェアがありませんが、Android Debug Bridge のヘルプを使用して指紋スキャンをシミュレートすることができます (以下で説明します)。  このガイドでは、Android デバイスで画面のロックを有効にし、認証用の指紋を登録する方法について説明します。

## <a name="requirements"></a>［要件］

指紋を登録するには、Android デバイス、または API レベル 23 (Android 6.0) を実行するエミュレーターが必要です。

Android Debug Bridge (ADB) を使用するには、コマンドプロンプトを使い慣れている必要があります。 **adb**実行可能ファイルは、Bash、PowerShell、またはコマンドプロンプト環境のパスになければなりません。

## <a name="configuring-a-screen-lock-and-enrolling-a-fingerprint"></a>画面ロックの構成と指紋の登録 

画面のロックを設定するには、次の手順を実行します。

1. **[> 設定]** セキュリティ にアクセスし、 **[画面のロック]** を選択します。

    ![セキュリティ画面での画面ロック選択の場所](enrolling-fingerprint-images/testing-01.png)

2. 次の画面が表示されるので、画面ロックのセキュリティメソッドのいずれかを選択して構成できます。 

    ![スワイプ、パターン、PIN、またはパスワードの選択](enrolling-fingerprint-images/testing-02.png)

   使用可能な画面ロック方法の1つを選択して完了します。

3. Screenlock が構成されたら、 **[Settings > Security]** ページに戻り、 **[フィンガープリント]** を選択します。

    ![[セキュリティ] 画面での指紋選択の場所](enrolling-fingerprint-images/testing-03.png)

4. そこから、シーケンスに従って、デバイスに指紋を追加します。

    [デバイスに指紋を追加するための一連のスクリーンショット![](enrolling-fingerprint-images/testing-04-sml.png)](enrolling-fingerprint-images/testing-04.png#lightbox)

5. 最後の画面では、指紋スキャナーに指を配置するように求められます。 

    ![指をセンサーに入れるように求める画面](enrolling-fingerprint-images/testing-05.png)

    Android デバイスを使用している場合は、スキャナーに指を触れることによってプロセスを完了します。 

### <a name="simulating-a-fingerprint-scan-on-the-emulator"></a>エミュレーターでの指紋スキャンのシミュレーション

Android エミュレーターでは、Android Debug Bridge を使用して指紋スキャンをシミュレートすることができます。 OS X で、Windows on Windows でターミナルセッションを開始するコマンドプロンプトまたは Powershell セッションを起動し、`adb`を実行します。

```shell
$ adb -e emu finger touch 1
```

値**1**は、"スキャン済み" という指の _\_id_です。 仮想指紋ごとに割り当てる一意の整数です。 アプリが実行されている場合は、エミュレーターが指紋の入力を求めるたびに、この同じ ADB コマンドを実行できます。 `adb` コマンドを実行し、それに_指\_id_を渡して、指紋スキャンをシミュレートすることができます。

指紋のスキャンが完了すると、次のように、指紋が追加されたことが Android から通知されます。  

![指紋を追加した画面が表示されます。](enrolling-fingerprint-images/testing-06.png)

## <a name="summary"></a>まとめ 

このガイドでは、画面ロックを設定し、Android デバイスまたは Android エミュレーターに指紋を登録する方法について説明しました。 
