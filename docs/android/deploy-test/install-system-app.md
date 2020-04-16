---
title: システム アプリケーションとして Xamarin.Android をインストールする
description: このガイドでは、システム アプリケーションとユーザー アプリケーションの違いと、Xamarin.Android アプリケーションをシステム アプリケーションとしてインストールする方法について説明します。 このガイドは、カスタム Android ROM イメージの作成者を対象としています。 カスタム ROM を作成する方法については説明しません。
ms.prod: xamarin
ms.assetid: 0113143B-7D8D-4C4C-B2F5-B966A2E7CE1F
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/15/2018
ms.openlocfilehash: 72cddde86708b5573dc578165354d137c4dc35b6
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "76723901"
---
# <a name="installing-xamarinandroid-as-a-system-app"></a>システム アプリケーションとして Xamarin.Android をインストールする

_このガイドでは、システム アプリケーションとユーザー アプリケーションの違いと、Xamarin.Android アプリケーションをシステム アプリケーションとしてインストールする方法について説明します。このガイドは、カスタム Android ROM イメージの作成者を対象としています。カスタム ROM を作成する方法については説明しません。_

## <a name="system-app"></a>システム アプリケーション

カスタム Android ROM イメージの作成者または Android デバイスの製造元の場合、ROM またはデバイスを配布するときに Xamarin.Android アプリケーションを _システム アプリケーション_ として含めたい場合があります。 システム アプリケーションとは、デバイスの機能にとって重要であると考えられるアプリケーション、またはカスタム ROM 作成者が常に使用したい機能を提供するアプリケーションです。

システム アプリケーションは、フォルダー **/system/app/** (ファイル システム上で読み取り専用ディレクトリ) にインストールされます。ユーザーがルート アクセス権を持っている場合を除き、ユーザーがシステム アプリケーションを削除または移動することはできません。 対照的に、ユーザーがインストールしたアプリケーション (通常は Google Play を使用するかアプリのサイドロードによってインストールします) は、_ユーザー アプリケーション_ と呼ばれます。 ユーザー アプリケーションはユーザーが削除できます。また多くの場合、デバイスの別の場所 (外部ストレージなど) に移動することができます。

システム アプリケーションはユーザー アプリケーションとまったく同じように動作しますが、主に次のような例外があります。

- システム アプリケーションは、通常の _ユーザー アプリケーション_ と同様にアップグレード可能です。 ただし、アプリケーションのコピーは常に **/system/app/** に存在するので、アプリケーションを元のバージョンにロールバックすることは常に可能です。

- システム アプリケーションには、ユーザー アプリケーションでは利用できないシステムのみのアクセス許可が付与されている場合があります。 システムのみのアクセス許可の例として、[`BLUETOOTH_PRIVILEGED`](https://developer.android.com/reference/android/Manifest.permission.html#BLUETOOTH_PRIVILEGED) があります。アプリケーションはこのアクセス許可を使用し、ユーザーの介入なしで Bluetooth デバイスとのペアリングを実行できます。

Xamarin.Android アプリケーションをシステム アプリケーションとして配布することができます。 カスタム ROM に APK が提供されるだけでなく、APK から ROM イメージのファイルシステムに手動でコピーする必要がある **libmonodroid.so** と **libmonosgen-2.0.so** という 2 つの共有ライブラリがあります。 このガイドでは、関連する手順について説明します。

## <a name="restrictions"></a>制約

このガイドは、カスタム Android ROM イメージの作成者を対象としています。 カスタム ROM を作成する方法については説明しません。

このガイドは、[Xamarin.Android のリリース APK をパッケージ化する方法](~/android/deploy-test/publishing/index.md)と [Android アプリケーションの CPU アーキテクチャ](~/android/app-fundamentals/cpu-architectures.md)を理解していることを前提としています。

## <a name="install-a-xamarinandroid-app-as-a-system-app"></a>システム アプリケーションとして Xamarin.Android アプリをインストールする

次の手順では、Xamarin.Android アプリケーションをシステム アプリケーションとしてインストールする方法について説明します。

1. **Xamarin.Android アプリケーションのリリース APK をパッケージ化する** &ndash; 詳細については、「[アプリケーションの発行](~/android/deploy-test/publishing/index.md)」ガイドを参照してください。

2. **共有ライブラリを APK から抽出する** &ndash; ZIP ユーティリティ プログラムを使用して APK ファイルを開き、 **/lib/** フォルダーの内容を確認します。 このフォルダーには、アプリケーションがサポートする _アプリケーション バイナリ インターフェイス_ (ABI) のサブディレクトリがあります。このフォルダー内には、その ABI でアプリケーションに必要なすべての共有ライブラリがあります。

    ![taskypro.zip の armeabi-v7a フォルダーにある .so ファイルのスクリーン ショット](install-system-app-images/install-system-app-01.png)

   前のスクリーン ショットでは、アプリケーションに必要な 2 つの **.so** ファイルを保持するサポート対象の ABI は 1 つのみです (**armeabi-v7a**)。 抽出する必要があるのは、デバイスまたはデバイス ROM のターゲット アーキテクチャに適した ABI ファイルのみである点に注意してください。たとえば、**x86** フォルダーの **.so** ファイルを **armeabi-v7a** デバイスまたは ROM にコピーしないでください。

3. **.so ファイルを /system/lib にコピーする**  &ndash; 前の手順で APK から抽出した **.so** ファイルをカスタム ROM 上の **/system/lib/** フォルダーにコピーします。

4. **APK ファイルを /system/app にコピーする** &ndash; 最後の手順は、APK ファイルを ROM 上の **/system/app** フォルダーにコピーすることです。

## <a name="summary"></a>まとめ

このガイドでは、_システム アプリケーション_ と _ユーザー アプリケーション_ の違いと、Xamarin.Android アプリケーションをシステム アプリケーションとしてインストールする方法について説明しました。

## <a name="related-links"></a>関連リンク

- [アプリケーションの発行](~/android/deploy-test/publishing/index.md)
- [CPU アーキテクチャ](~/android/app-fundamentals/cpu-architectures.md)
- [BLUETOOTH_PRIVILEGED](https://developer.android.com/reference/android/Manifest.permission.html#BLUETOOTH_PRIVILEGED)
- [ABI 管理](https://developer.android.com/ndk/guides/abis)
