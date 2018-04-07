---
title: 実行する方法の詳細な for Xamarin for Visual Studio をアンインストールしますか?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: c1742239-05ea-449d-9c99-611e5e5a90e4
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 12/02/2016
ms.openlocfilehash: 49577961026d9895912d2848975e71a9f7eebbd8
ms.sourcegitcommit: 6f7033a598407b3e77914a85a3f650544a4b6339
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
# <a name="how-do-i-perform-a-thorough-uninstall-for-xamarin-for-visual-studio"></a>実行する方法の詳細な for Xamarin for Visual Studio をアンインストールしますか?


1.  Windows コントロール パネルから存在する次のいずれかをアンインストールします。

    -   Xamarin
    -   Xamarin for Windows
    -   Xamarin.Android
    -   Xamarin.iOS
    -   Xamarin for Visual Studio

2.  エクスプ ローラーで、Visual Studio の Xamarin 拡張機能フォルダーから残りのファイルを削除します (両方を含む、すべてのバージョン_Program Files_と_%program Files (x86)_)。

    _C:\\プログラム ファイル\*\\Microsoft Visual Studio 1\*.0\\Common7\\IDE\\拡張\\Xamarin_

3.  Visual Studio の MEF コンポーネント キャッシュ ディレクトリを削除します。

    _%LOCALAPPDATA%\\Microsoft\\VisualStudio\\1\*.0\\ComponentModelCache_

    実際には、それ自体では、この手順などのエラーを解決するための十分な多くの場合です。

    -   「'XamarinShellPackage' パッケージが正しく読み込まれませんでした」

    -   "プロジェクト ファイル、開くことができません。 不足しているプロジェクトのサブタイプがある"

    -   "オブジェクト参照がオブジェクトのインスタンスに設定されていません。  at Xamarin.VisualStudio.IOS.XamarinIOSPackage.Initialize()"

    -   「パッケージの SetSite に失敗しました」(Visual studio の_ActivityLog.xml_)

    -   「パッケージの LegacySitePackage に失敗しました」(Visual studio の_ActivityLog.xml_)

    (参照、 [MEF コンポーネントのキャッシュのクリア](https://visualstudiogallery.msdn.microsoft.com/22b94661-70c7-4a93-9ca3-8b6dd45f47cd)Visual Studio 拡張機能です。  参照してください、[バグ 40781、コメント 19](https://bugzilla.xamarin.com/show_bug.cgi?id=40781#c19)これらのエラーを引き起こす可能性のある Visual Studio で、上位の問題に関するもう少しコンテキストのです)。

4.  チェックインしても、 _VirtualStore_かどうか Windows が保存されているいずれかを表示するディレクトリのファイルのオーバーレイ、_拡張\\Xamarin_または_ComponentModelCache_ディレクトリがあります。

    _%LOCALAPPDATA%\\VirtualStore_

5.  レジストリ エディターを開きます (`regedit`)。

6.  次のキーを探します。

    _HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Wow6432Node\\Microsoft\\Windows\\CurrentVersion\\SharedDlls_

7.  検索し、このパターンに一致するエントリを削除します。

    _C:\\プログラム ファイル\*\\Microsoft Visual Studio 1\*.0\\Common7\\IDE\\拡張\\Xamarin_

8.  このキーを探します。

    _HKEY\_現在\_ユーザー\\ソフトウェア\\Microsoft\\VisualStudio\\1\*.0\\ExtensionManager\\PendingDeletions_

9.  Xamarin に関連するいると同じようにすべてのエントリを削除します。  たとえば、次のいずれかを使用して、Xamarin の以前のバージョンで問題が発生します。

    _Mono.VisualStudio.Shell,1.0_

10. 管理者を開く`cmd.exe`プロンプトのコマンドを実行し、`devenv /setup`と`devenv /updateconfiguration`インストールされている各バージョンの Visual Studio のコマンド。  たとえば、Visual Studio 2015。

    ```
    "%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\devenv.exe" /setup
    "%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\devenv.exe" /updateconfiguration
    ```

11. 再起動します。

12. 現在の安定したバージョンの Xamarin を使用しての再インストール[visualstudio.com](https://visualstudio.com/xamarin/)です。

## <a name="additional-troubleshooting-steps-for-package-did-not-load-correctly"></a>追加の「パッケージが正しく読み込まれませんでした」のトラブルシューティング手順

上記の手順と「パッケージが正しく読み込まれませんでした」エラーが解決しない場合、さらに、いくつか再試行する手順のとおりです。

1.  新しい Windows ユーザー アカウントを作成します。

2.  新しいユーザーのエラーが発生せず、Visual Studio の Xamarin 拡張機能を読み込むかどうかを確認してください。

3.  拡張機能が正常に読み込ま元のユーザーに対して、保存された設定によって、問題が原因ほとんどの場合。

    -   **エクスプ ローラーで**– _%localappdata%\\Microsoft\\VisualStudio\\1\*.0_
    -   **レジストリ エディターで**– _HKEY\_現在\_ユーザー\\ソフトウェア\\Microsoft\\VisualStudio\\1\*.0_
    -   **レジストリ エディターで**– _HKEY\_現在\_ユーザー\\ソフトウェア\\Microsoft\\VisualStudio\\1\*.0\_構成_

4.  表示する場合は、保存された設定は実際に問題である、バックアップを作成して、それらを削除しようとすることができます。
