---
title: Xamarin for Visual Studio の完全なアンインストールを実行するにはどうすればいいですか
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: c1742239-05ea-449d-9c99-611e5e5a90e4
author: asb3993
ms.author: amburns
ms.date: 12/02/2016
ms.openlocfilehash: 99fde9330498ee62d3cf6b5910c2cbfae39cfdeb
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61159624"
---
# <a name="how-do-i-perform-a-thorough-uninstall-for-xamarin-for-visual-studio"></a>Xamarin for Visual Studio の完全なアンインストールを実行するにはどうすればいいですか


1.  Windows コントロール パネルで、表示されている次のいずれかをアンインストールします。

    -   Xamarin
    -   Xamarin for Windows
    -   Xamarin.Android
    -   Xamarin.iOS
    -   Xamarin for Visual Studio

2.  エクスプ ローラーで、Visual Studio の Xamarin 拡張機能フォルダーから残りのファイルを削除します (すべてのバージョンでは、両方を含む_Program Files_と_Program Files (x86)_)。

    _C:\\プログラム ファイル\*\\Microsoft Visual Studio 1\*.0\\Common7\\IDE\\拡張\\Xamarin_

3.  Visual Studio の MEF コンポーネント キャッシュ ディレクトリを削除します。

    _%LOCALAPPDATA%\\Microsoft\\VisualStudio\\1\*.0\\ComponentModelCache_

    実際には、単独では、この手順などのエラーを解決するための十分な多くの場合です。

    -   「'XamarinShellPackage' パッケージが正しく読み込まれませんでした」

    -   "プロジェクト ファイル…開くことができません。 不足しているプロジェクトのサブタイプがある"

    -   "オブジェクト参照がオブジェクトのインスタンスに設定されていません。  at Xamarin.VisualStudio.IOS.XamarinIOSPackage.Initialize()"

    -   「パッケージの SetSite に失敗しました」(Visual studio の_ActivityLog.xml_)

    -   「パッケージの LegacySitePackage が失敗しました」(Visual studio の_ActivityLog.xml_)

    (も参照してください、 [MEF コンポーネント キャッシュのクリア](https://visualstudiogallery.msdn.microsoft.com/22b94661-70c7-4a93-9ca3-8b6dd45f47cd)Visual Studio 拡張機能。  参照してくださいと[バグ 40781、コメント 19](https://bugzilla.xamarin.com/show_bug.cgi?id=40781#c19)これらのエラーを引き起こす可能性のある Visual Studio でのアップ ストリームの問題についてもう少しコンテキスト)。

4.  チェックインしても、 _VirtualStore_かどうか Windows 可能性がありますが保存されているかを確認するディレクトリにオーバーレイ ファイル、_拡張\\Xamarin_または_ComponentModelCache_ディレクトリがあります。

    _%LOCALAPPDATA%\\VirtualStore_

5.  レジストリ エディターを開きます (`regedit`)。

6.  次のキーを探します。

    _HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Wow6432Node\\Microsoft\\Windows\\CurrentVersion\\SharedDlls_

7.  次のパターンに一致するエントリがあれば、削除します。

    _C:\\プログラム ファイル\*\\Microsoft Visual Studio 1\*.0\\Common7\\IDE\\拡張\\Xamarin_

8.  このキーを探します。

    _HKEY\_現在\_ユーザー\\ソフトウェア\\Microsoft\\VisualStudio\\1\*.0\\ExtensionManager\\PendingDeletions_

9.  Xamarin に関連すると考えられるすべてのエントリを削除します。  たとえば、ここでの 1 つを使用 Xamarin の以前のバージョンで問題が発生します。

    _Mono.VisualStudio.Shell,1.0_

10. 管理者を開く`cmd.exe`コマンド プロンプトを実行し、`devenv /setup`と`devenv /updateconfiguration`Visual Studio のインストールされている各バージョン用のコマンド。  たとえば、Visual Studio 2015 では次のようになります。

    ```
    "%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\devenv.exe" /setup
    "%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\devenv.exe" /updateconfiguration
    ```

11. 再起動します。

12. Xamarin を使用しての現在の安定したバージョンを再インストール[visualstudio.com](https://visualstudio.com/xamarin/)します。

## <a name="additional-troubleshooting-steps-for-package-did-not-load-correctly"></a>追加の手順を「パッケージが正しく読み込まれませんでした」のトラブルシューティング

上記の手順と、「パッケージが正しく読み込まれませんでした」エラーが解決しない場合、お試しに、いくつかの手順を示します。

1.  新しい Windows ユーザー アカウントを作成します。

2.  新しいユーザーのエラーが発生せず、Visual Studio の Xamarin 拡張機能を読み込むかどうかを確認します。

3.  拡張機能が正常にロード元のユーザーの保存された設定の一部で問題が発生ほとんどの場合。

    -   **エクスプ ローラーで**– _%localappdata%\\Microsoft\\VisualStudio\\1\*.0_
    -   **Regedit で**– _HKEY\_現在\_ユーザー\\ソフトウェア\\Microsoft\\VisualStudio\\1\*.0_
    -   **Regedit で**– _HKEY\_現在\_ユーザー\\ソフトウェア\\Microsoft\\VisualStudio\\1\*.0\_構成_

4.  問題はこれら保存されている設定が表示されて実際に場合、は、バックアップを作成して、それらを削除を試すことができます。
