---
title: インストール後に不足している Visual Studio の拡張機能
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 066d36a3-e553-48d6-8769-c972274d7641
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/20/2017
ms.openlocfilehash: 72870b9bf6ff6c3068ee037e6405e4ec03546cd6
ms.sourcegitcommit: 6f7033a598407b3e77914a85a3f650544a4b6339
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
# <a name="missing-visual-studio-extensions-after-installation"></a>インストール後に不足している Visual Studio の拡張機能

## <a name="error-message-this-project-is-incompatible-with-the-current-edition-of-visual-studio"></a>エラー メッセージ: このプロジェクトは Visual Studio の現在のエディションと互換性がありません。

互換性のあるバージョンの Visual Studio がインストールされていることを確認します。

-   Visual Studio 2017 (Community、Professional、または Enterprise)
-   Visual Studio 2015 (Community、Professional、または Enterprise)

参照、 [Windows 要件](~/cross-platform/get-started/requirements.md#windows)です。

## <a name="possible-fix-1-change-the-installation-to-make-sure-the-visual-studio-extensions-are-installed"></a>考えられる修正 1: Visual Studio 拡張機能がインストールされているかどうかを確認するインストールを変更します。

特定の状況で Xamarin インストーラーが自動的にチェック ボックスをオフ Visual Studio 拡張機能のインストール オプションです。 問題の原因がある場合、インストーラーを使用して、不足している Visual Studio の拡張機能をインストール**変更**コマンド。 たとえば、Visual Studio 2013 用拡張機能をインストールするには。

1. ウィンドウを開く**プログラムと機能**コントロール パネルの します。

2. 右クリックして、 **Xamarin**エントリ、および選択**変更**です。

3. をクリックして**次**、し**変更**です。

4. 確認、 **Xamarin for Visual Studio 2013**をインストールするオプションを設定します。

    ![](missing-vs-extensions-images/installer.png "Visual Studio 2013 のインストール オプションの Xamarin を有効にします。")

5. インストーラー ウィザードの残りの部分を実行します。

## <a name="possible-fix-2-ask-visual-studio-to-set-up-the-extensions-again"></a>考えられる修正 2: Visual Studio 拡張機能を再設定を依頼します。

1. Xamarin 拡張機能を Visual Studio 拡張機能フォルダーにコピーされているかどうかを確認してください。

    `C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\Extensions\Xamarin\Xamarin\3.1.228.0`

    (バージョン 3.1.228) については、拡張機能が正しくインストールされて場合があります 60 アイテム フォルダー内。


    ![](missing-vs-extensions-images/folder.png "エクスプ ローラーで 'Xamarin\3.1.228.0' フォルダーの内容の一覧")

2. このフォルダーが正しいことを確認した後は、Visual Studio 拡張機能をもう一度設定するを伝えます。

    `"C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\devenv.exe" /setup`

## <a name="possible-fix-3-try-a-fresh-reinstall-of-xamarin"></a>考えられる修正 3: は、Xamarin の最新の再インストールを再試行してください。

1.  Windows コントロール パネルから存在する次のいずれかをアンインストールします。

    *   Xamarin

    *   Xamarin for Windows

    *   Xamarin.Android

    *   Xamarin.iOS

    *   Xamarin for Visual Studio

2.  エクスプ ローラーで、Visual Studio の Xamarin 拡張機能フォルダーから残りのファイルを削除します (両方を含む、すべてのバージョン**Program Files**と**%program Files (x86)**)。

    `C:\Program Files*\Microsoft Visual Studio 1*.0\Common7\IDE\Extensions\Xamarin`

3.  "VirtualStore"ディレクトリを調べて、任意の拡張機能ディレクトリの任意の「オーバーレイ」コピーがある可能性があるかどうかでも確認してください。

    `%LOCALAPPDATA%\VirtualStore`

4.  レジストリ エディター (regedit) を開きます。

5.  このキーを探します。

    _HKEY\_LOCAL\_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\SharedDlls_

6.  検索し、このパターンに一致するエントリを削除します。

    _C:\Program Files\*\Microsoft Visual Studio 1\*.0\Common7\IDE\Extensions\Xamarin_

7.  このキーを探します。

    `HKEY\_CURRENT\_USER\Software\Microsoft\VisualStudio\1\*.0\ExtensionManager\PendingDeletions`

8.  Xamarin に関連するいると同じようにすべてのエントリを削除します。 たとえば、次のいずれかを使用して、Xamarin の以前のバージョンで問題が発生します。

    _Mono.VisualStudio.Shell,1.0_

9.  再起動します。

10.  現在の安定したバージョンの Xamarin の再インストール[visualstudio.com](https://visualstudio.com/xamarin)です。

## <a name="possible-fix-4-repair-visual-studio-installation"></a>考えられる修正 4: 修復の Visual Studio のインストール

1.  ウィンドウを開く**プログラムと機能**コントロール パネルの します。

2.  関連する Microsoft Visual Studio エントリを右クリックし、選択**変更**

3.  クリックして、**修復**Visual Studio ダイアログ ボックスが開きますがボタンをクリックします。
