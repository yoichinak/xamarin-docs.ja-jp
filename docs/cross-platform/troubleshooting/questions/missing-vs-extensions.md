---
title: インストール後に見つからない Visual Studio の拡張機能がある
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 066d36a3-e553-48d6-8769-c972274d7641
author: asb3993
ms.author: amburns
ms.date: 03/20/2017
ms.openlocfilehash: 3e3d426e7b00725eafeba139de5bc46d416c368a
ms.sourcegitcommit: 650458de1d362cd7de174cacef7838f0e74426f3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2019
ms.locfileid: "58070892"
---
# <a name="missing-visual-studio-extensions-after-installation"></a>インストール後に見つからない Visual Studio の拡張機能がある

## <a name="error-message-this-project-is-incompatible-with-the-current-edition-of-visual-studio"></a>エラー メッセージ:このプロジェクトは Visual Studio の現在のエディションと互換性がありません。

以降をインストールまたは Visual Studio 2017 (Community、Professional または Enterprise) を確認します。

参照してください、 [Windows 要件](~/cross-platform/get-started/requirements.md#windows-requirements)します。

## <a name="possible-fix-1-change-the-installation-to-make-sure-the-visual-studio-extensions-are-installed"></a>考えられる修正 1:Visual Studio 拡張機能がインストールされているかどうかを確認するインストールを変更します。

特定の状況で Xamarin インストーラーが自動的に - チェック ボックスをオフ Visual Studio 拡張機能のインストール オプションです。 問題の原因がある場合は、インストーラーを使用して、不足している Visual Studio の拡張機能をインストール**変更**コマンド。 たとえば、Visual Studio 2013 の拡張機能をインストールします。

1. Windows を開く**プログラムと機能**コントロール パネル。

2. 右クリックして、 **Xamarin**エントリ、および選択**変更**します。

3. クリックして**次**、し**変更**します。

4. 確認、 **Xamarin for Visual Studio 2013**をインストールするオプションを設定します。

    ![](missing-vs-extensions-images/installer.png "Xamarin を Visual Studio 2013 のインストール オプションを有効にします。")

5. インストーラー ウィザードの残りの部分を実行します。

## <a name="possible-fix-2-ask-visual-studio-to-set-up-the-extensions-again"></a>考えられる修正 2:Visual Studio をもう一度設定し、拡張機能の確認します。

1. Xamarin 拡張機能を Visual Studio の拡張機能フォルダーにコピーされているかどうかを確認します。

    `C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\Extensions\Xamarin\Xamarin\3.1.228.0`

    拡張機能は、(バージョン 3.1.228) のインストールが正しく場合があります 60 項目フォルダー内です。


    ![](missing-vs-extensions-images/folder.png "エクスプ ローラーで 'Xamarin\3.1.228.0' フォルダーの内容の一覧")

2. このフォルダーが正しいことを確認した後は、Visual Studio で、拡張機能の設定をもう一度お試しくださいを指示します。

    `"C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\devenv.exe" /setup`

## <a name="possible-fix-3-try-a-fresh-reinstall-of-xamarin"></a>可能な限りでは、3 を修正します。Xamarin の最新の再インストールを再試行してください。

1.  Windows コントロール パネルで、表示されている次のいずれかをアンインストールします。

    *   Xamarin

    *   Xamarin for Windows

    *   Xamarin.Android

    *   Xamarin.iOS

    *   Xamarin for Visual Studio

2.  エクスプ ローラーで、Visual Studio の Xamarin 拡張機能フォルダーから残りのファイルを削除します (すべてのバージョンでは、両方を含む**Program Files**と**Program Files (x86)**)。

    `C:\Program Files*\Microsoft Visual Studio 1*.0\Common7\IDE\Extensions\Xamarin`

3.  "VirtualStore"ディレクトリから拡張機能ディレクトリのいずれかの「オーバーレイ」コピーがある可能性があるかどうかでも確認します。

    `%LOCALAPPDATA%\VirtualStore`

4.  レジストリ エディター (regedit) を開きます。

5.  このキーを探します。

    _HKEY\_LOCAL\_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\SharedDlls_

6.  次のパターンに一致するエントリがあれば、削除します。

    _C:\Program Files\*\Microsoft Visual Studio 1\*.0\Common7\IDE\Extensions\Xamarin_

7.  このキーを探します。

    `HKEY\_CURRENT\_USER\Software\Microsoft\VisualStudio\1\*.0\ExtensionManager\PendingDeletions`

8.  Xamarin に関連すると考えられるすべてのエントリを削除します。 たとえば、ここでの 1 つを使用 Xamarin の以前のバージョンで問題が発生します。

    _Mono.VisualStudio.Shell,1.0_

9.  再起動します。

10.  現在の安定したバージョンから Xamarin を再インストール[visualstudio.com](https://visualstudio.com/xamarin)します。

## <a name="possible-fix-4-repair-visual-studio-installation"></a>考えられる修正 4:Visual Studio のインストールを修復します。

1.  Windows を開く**プログラムと機能**コントロール パネル。

2.  関連の Microsoft Visual Studio エントリを右クリックし、選択**変更**

3.  をクリックして、**修復**Visual Studio のダイアログ ボックスが開きますがボタンをクリックします。
