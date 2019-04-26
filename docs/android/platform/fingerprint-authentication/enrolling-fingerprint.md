---
title: 指紋の登録
description: 設定する方法の画面のロックをセットアップして、Android デバイスまたはエミュレーターにフィンガー プリントを登録します。
ms.prod: xamarin
ms.assetid: 52092F63-00EE-4F8B-A49F-65C9CCBA7EF2
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 18903a7d8f6c4033dc3ac7c4c0187a247d023bc1
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61027019"
---
# <a name="enrolling-a-fingerprint"></a>指紋の登録

## <a name="enrolling-a-fingerprint-overview"></a>指紋の概要を登録します。

のみ、デバイスが指紋認証を既に構成されている場合は、指紋認証を利用する Android アプリケーション用可能です。 このガイドは、Android デバイスまたはエミュレーターにフィンガー プリントを登録する方法について説明します。 エミュレーターは、指紋スキャンを実行する実際のハードウェアはありませんが、Android Debug Bridge (後述) の協力の指紋のスキャンをシミュレートすることができます。  このガイドは、Android デバイスで画面のロックを有効にして、認証用のフィンガー プリントを登録する方法について説明します。

## <a name="requirements"></a>必要条件

フィンガー プリントを登録するには、Android デバイスまたはエミュレーター API レベル 23 (Android 6.0) の実行が必要です。

Android Debug Bridge (ADB) の使用には、コマンド プロンプトで、知識が必要です、 **adb**実行可能ファイルが、Bash、PowerShell のパスにするか、コマンド プロンプト環境必要があります。

## <a name="configuring-a-screen-lock-and-enrolling-a-fingerprint"></a>画面のロックを構成して、指紋の登録 

画面のロックをセットアップするには、次の手順を実行します。

1. 移動して**設定 > セキュリティ**、選び**画面のロック**:

    ![セキュリティ画面上の画面ロック選択の場所](enrolling-fingerprint-images/testing-01.png)

2. 次の画面が選択して、画面のロックのセキュリティ メソッドのいずれかを構成を許可します。 

    ![スワイプ、パターン、PIN、またはパスワードを選択します。](enrolling-fingerprint-images/testing-02.png)

   選択し、使用可能な画面のロック方法のいずれかを実行します。

3. Screenlock を構成するを返す、**設定 > セキュリティ** ページ**指紋**:

    ![セキュリティ画面上の指紋の選択の場所](enrolling-fingerprint-images/testing-03.png)

4. そこから、指紋をデバイスに追加する順序に従います。

    [![一連の指紋をデバイスに追加するためのスクリーン ショット](enrolling-fingerprint-images/testing-04-sml.png)](enrolling-fingerprint-images/testing-04.png#lightbox)

5. 最後の画面に指を指紋スキャナーに配置する求められます。 

    ![センサーの上に指を配置するように求められます画面](enrolling-fingerprint-images/testing-05.png)

    Android デバイスを使用している場合、スキャナーに指が接触してプロセスを完了します。 
    
    
### <a name="simulating-a-fingerprint-scan-on-the-emulator"></a>エミュレーターでの指紋のスキャンをシミュレートします。

Android エミュレーターで Android Debug Bridge を使用して、指紋のスキャンをシミュレートすることができます。 OS X の開始時に、ターミナル セッション中には、Windows コマンド プロンプトまたは Powershell セッションを開始および実行`adb`:

```shell
$ adb -e emu finger touch 1
```

値**1**は、_指\_id_指「スキャン」されたのです。 割り当てる各仮想指紋一意の整数になります。 今後とするアプリが実行されていることができますこの同じ ADB を実行コマンドごとに、エミュレーターが求め、フィンガー プリントを実行することができます、`adb`コマンドを渡す、_指\_id_指紋のスキャンをシミュレートするには.

指紋のスキャンが完了したら、Android いたします指紋が追加されています。  

![追加の指紋を表示する画面です。](enrolling-fingerprint-images/testing-06.png)

## <a name="summary"></a>まとめ 

このガイドでは、画面のロックを設定して、または Android のエミュレーターで Android デバイスで指紋を登録する方法について説明します。 

