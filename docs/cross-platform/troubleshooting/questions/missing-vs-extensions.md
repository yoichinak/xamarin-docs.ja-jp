---
title: インストール後に見つからない Visual Studio の拡張機能がある
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 066d36a3-e553-48d6-8769-c972274d7641
author: davidortinau
ms.author: daortin
ms.date: 03/20/2017
ms.openlocfilehash: 976d0882c5875c1d3e1c8f0ea1732de08df8e07f
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/23/2020
ms.locfileid: "86996176"
---
# <a name="missing-visual-studio-extensions-after-installation"></a>インストール後に見つからない Visual Studio の拡張機能がある

## <a name="error-message-this-project-is-incompatible-with-the-current-edition-of-visual-studio"></a>エラーメッセージ: このプロジェクトは、Visual Studio の現在のエディションと互換性がありません

Visual Studio 2017 (Community、Professional、または Enterprise) 以降がインストールされていることを確認します。

「Windows の[要件](~/cross-platform/get-started/requirements.md#windows-requirements)」も参照してください。

## <a name="possible-fix-1-change-the-installation-to-make-sure-the-visual-studio-extensions-are-installed"></a>考えられる修正プログラム 1: Visual Studio 拡張機能がインストールされていることを確認するために、インストールを変更します。

場合によっては、Xamarin インストーラーによって、Visual Studio 拡張機能のインストールオプションが自動的にオフにされることがあります。 これが問題の原因である場合は、インストーラーの [**変更**] コマンドを使用して、不足している Visual Studio 拡張機能をインストールします。 たとえば、Visual Studio 2013 の拡張機能をインストールするには、次のようにします。

1. コントロールパネルの [Windows の**プログラムと機能**] を開きます。

2. **Xamarin**エントリを右クリックし、[**変更**] を選択します。

3. [**次へ**]、[**変更**] の順にクリックします。

4. **Xamarin for Visual Studio 2013**オプションが [インストール] に設定されていることを確認します。

    ![Visual Studio 2013 インストールオプションでの Xamarin の有効化](missing-vs-extensions-images/installer.png)

5. インストーラーウィザードの残りの部分を続行します。

## <a name="possible-fix-2-ask-visual-studio-to-set-up-the-extensions-again"></a>考えられる修正プログラム 2: 拡張機能をもう一度設定するように Visual Studio に依頼する

1. Xamarin 拡張機能が Visual Studio extensions フォルダーにコピーされているかどうかを確認します。

    `C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\Extensions\Xamarin\Xamarin\3.1.228.0`

    拡張機能が正しくインストールされている場合 (バージョン3.1.228 の場合)、次のフォルダーに60項目が表示されます。

    ![エクスプローラーの Xamarin\3.1.228.0 フォルダーの内容の一覧](missing-vs-extensions-images/folder.png)

2. このフォルダーが正しく表示されていることを確認した後、Visual Studio に拡張機能のセットアップをもう一度試してください。

    `"C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\devenv.exe" /setup`

## <a name="possible-fix-3-try-a-fresh-reinstall-of-xamarin"></a>考えられる修正プログラム 3: Xamarin の新規再インストールを試す

1. Windows のコントロールパネルで、次のいずれかのものをアンインストールします。

    * Xamarin

    * Xamarin for Windows

    * Xamarin.Android

    * Xamarin.iOS

    * Xamarin for Visual Studio

2. エクスプローラーで、Xamarin Visual Studio 拡張機能フォルダー (**プログラムファイル**と**プログラムファイル (x86)** の両方を含むすべてのバージョン) から残りのファイルを削除します。

    `C:\Program Files*\Microsoft Visual Studio 1*.0\Common7\IDE\Extensions\Xamarin`

3. また、"VirtualStore" ディレクトリをチェックインして、いずれかの拡張ディレクトリの "オーバーレイ" コピーが存在する可能性があるかどうかを確認します。

    `%LOCALAPPDATA%\VirtualStore`

4. レジストリエディター (regedit) を開きます。

5. このキーを探します。

    _HKEY \_ LOCAL \_ MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\SharedDlls_

6. 次のパターンに一致するエントリがあれば、削除します。

    _C:\Program Files \* \Microsoft Visual Studio 1 \* .0 \ Common7\IDE\Extensions\Xamarin_

7. このキーを探します。

    `HKEY\_CURRENT\_USER\Software\Microsoft\VisualStudio\1\*.0\ExtensionManager\PendingDeletions`

8. Xamarin に関連すると考えられるすべてのエントリを削除します。 たとえば、以前のバージョンの Xamarin では、次のような問題が発生します。

    _VisualStudio、1.0_

9. 再起動します。

10. [Visualstudio.com](https://visualstudio.com/xamarin)から、Xamarin の現在の安定バージョンを再インストールします。

## <a name="possible-fix-4-repair-visual-studio-installation"></a>考えられる修正プログラム 4: Visual Studio のインストールを修復する

1. コントロールパネルの [Windows の**プログラムと機能**] を開きます。

2. 関連する Microsoft Visual Studio エントリを右クリックし、[**変更**] を選択します。

3. 表示された Visual Studio ダイアログで [**修復**] ボタンをクリックします。
