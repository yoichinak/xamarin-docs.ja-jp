---
title: "フィンガー プリントを登録します。"
description: "設定する方法は、画面のロックをセットアップし、Android デバイスまたはエミュレーターに指紋を登録します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 52092F63-00EE-4F8B-A49F-65C9CCBA7EF2
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 494c808c8c65e0f3cf0a34b26d87069dd456718c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="enrolling-a-fingerprint"></a>フィンガー プリントを登録します。

## <a name="enrolling-a-fingerprint-overview"></a>指紋の概要を登録します。

デバイスは、指紋認証で既に構成されている場合は、指紋認証を利用する Android アプリケーションのみです。 このガイドでは、Android デバイスまたはエミュレーターにフィンガー プリントを登録する方法を説明します。 エミュレーターはありません指紋スキャンを実行する実際のハードウェアが (後述) Android Debug Bridge を利用して指紋スキャンをシミュレートすることができます。  このガイドでは、Android デバイスで画面のロックを有効にして、認証のフィンガー プリントを登録する方法を説明します。

## <a name="requirements"></a>必要条件

フィンガー プリントを登録するには、Android デバイスまたはエミュレーターを API level 23 (Android 6.0) を実行しているが必要です。

Android のデバッグ Bridge (ADB) を使用するには、コマンド プロンプトでの知識が必要です。 および**adb**実行可能ファイルが、Bash、PowerShell でのパスにするか、コマンド プロンプト環境必要があります。

## <a name="configuring-a-screen-lock-and-enrolling-a-fingerprint"></a>画面のロックを構成して、フィンガー プリントを登録します。 

画面のロックを設定するには、次の手順を実行します。

1. 移動して**設定 > セキュリティ**を選択して**画面のロック**:

    ![[セキュリティ] 画面で選択内容を画面ロックの場所](enrolling-fingerprint-images/testing-01.png)

2. 次の画面に表示されるを選択して、画面のロックのセキュリティ メソッドのいずれかを構成するは、できます。 

    ![スワイプして、パターン、PIN、またはパスワードを選択します。](enrolling-fingerprint-images/testing-02.png)

   選択し、使用可能な画面のロック方法のいずれかを実行します。

3. Screenlock を構成した後に戻り、**設定 > セキュリティ**ページし、選択**指紋**:

    ![[セキュリティ] 画面で指紋選択範囲の位置](enrolling-fingerprint-images/testing-03.png)

4. そこから、次のデバイスに指紋を追加するシーケンスを実行します。

    [![一連のデバイスに指紋を追加するためのスクリーン ショット](enrolling-fingerprint-images/testing-04-sml.png)](enrolling-fingerprint-images/testing-04.png)

5. 最終画面で指紋スキャナーの上に指を配置するように求められます。 

    ![センサーに指を配置するように求める画面](enrolling-fingerprint-images/testing-05.png)

    Android デバイスを使用している場合は、スキャナーに指を変更することによって、プロセスを完了します。 
    
    
### <a name="simulating-a-fingerprint-scan-on-the-emulator"></a>エミュレーターで指紋のスキャンをシミュレートします。

Android エミュレーター、Android Debug Bridge を使用して、指紋スキャンをシミュレートすることです。 OS X の開始時に、ターミナル セッション中には、Windows コマンド プロンプトまたは Powershell セッションを開始および実行`adb`:

```shell
$ adb -e emu finger touch 1
```

値**1**は、_指\_id_ 「スキャン」された本指のです。 これは、各仮想指紋に割り当てる一意の整数です。 アプリが実行するときに、将来できますこの同じ ADB を実行コマンドごとに、エミュレーターが表示されたら、フィンガー プリントを行うことができます、`adb`コマンドを渡すこと、_指\_id_指紋スキャンをシミュレートするために.

指紋スキャンが完了したら、Android 通知されます指紋が追加されています。  

![追加の指紋を表示する画面です。](enrolling-fingerprint-images/testing-06.png)

## <a name="summary"></a>まとめ 

このガイドでは、画面のロックを設定して、Android デバイスまたは Android エミュレーターでのフィンガー プリントを登録する方法について説明します。 

