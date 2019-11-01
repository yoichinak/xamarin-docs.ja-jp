---
title: Xamarin for Visual Studio の完全なアンインストールを実行するにはどうすればいいですか
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: c1742239-05ea-449d-9c99-611e5e5a90e4
author: davidortinau
ms.author: daortin
ms.date: 12/02/2016
ms.openlocfilehash: ed0171c2b6bd98e5b29ec100d0235131d36acb05
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73013341"
---
# <a name="how-do-i-perform-a-thorough-uninstall-for-xamarin-for-visual-studio"></a>Xamarin for Visual Studio の完全なアンインストールを実行するにはどうすればいいですか

1. Windows のコントロールパネルで、次のいずれかのものをアンインストールします。

    - Xamarin
    - Xamarin for Windows
    - Xamarin.Android
    - Xamarin.iOS
    - Xamarin for Visual Studio

2. エクスプローラーで、Xamarin Visual Studio 拡張機能フォルダー (_プログラムファイル_と_プログラムファイル (x86)_ の両方を含むすべてのバージョン) から残りのファイルを削除します。

    _C:\\プログラムファイル\*\\Microsoft Visual Studio 1\*.0\\Common7\\IDE\\拡張機能\\Xamarin_

3. Visual Studio の MEF コンポーネントキャッシュディレクトリも削除します。

    _% LOCALAPPDATA%\\Microsoft\\VisualStudio\\1\*. 0\\ComponentModelCache_

    実際、この手順では、次のようなエラーを解決するのに十分な場合があります。

    - "XamarinShellPackage" パッケージが正しく読み込まれませんでした "

    - "プロジェクトファイル...を開くことができません。 プロジェクトのサブタイプがありません "

    - "オブジェクト参照がオブジェクトのインスタンスに設定されていません。  VisualStudio () XamarinIOSPackage にあります。

    - "パッケージの SetSite に失敗しました" (Visual Studio の_Activitylog .xml_)

    - "パッケージの LegacySitePackage に失敗しました" (Visual Studio の_Activitylog .xml_)

    (「 [CLEAR MEF Component Cache](https://visualstudiogallery.msdn.microsoft.com/22b94661-70c7-4a93-9ca3-8b6dd45f47cd) Visual Studio extension」も参照してください。  また、このようなエラーを引き起こす可能性のある Visual Studio のアップストリームの問題については[、バグ40781、コメント 19](https://bugzilla.xamarin.com/show_bug.cgi?id=40781#c19)に関する説明を参照してください。)

4. また、 _Virtualstore_ディレクトリをチェックインして、Windows に_拡張機能_のオーバーレイファイルが格納されているかどうかを確認します。 Xamarin または_componentmodelcache_ディレクトリ\\ます。

    _% LOCALAPPDATA%\\VirtualStore_

5. レジストリエディター (`regedit`) を開きます。

6. 次のキーを探します。

    _HKEY\_ローカル\_マシン\\ソフトウェア\\Wow6432Node\\Microsoft\\Windows\\CurrentVersion\\SharedDlls_

7. 次のパターンに一致するエントリがあれば、削除します。

    _C:\\プログラムファイル\*\\Microsoft Visual Studio 1\*.0\\Common7\\IDE\\拡張機能\\Xamarin_

8. このキーを探します。

    _HKEY\_現在\_ユーザー\\ソフトウェア\\Microsoft\\VisualStudio\\1\*.0\\ExtensionManager\\PendingDeletions_

9. Xamarin に関連すると考えられるすべてのエントリを削除します。  たとえば、以前のバージョンの Xamarin では、次のような問題が発生します。

    _VisualStudio、1.0_

10. 管理者 `cmd.exe` コマンドプロンプトを開き、インストールされている Visual Studio の各バージョンに対して `devenv /setup` と `devenv /updateconfiguration` のコマンドを実行します。  たとえば、Visual Studio 2015 では次のようになります。

    ```
    "%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\devenv.exe" /setup
    "%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\devenv.exe" /updateconfiguration
    ```

11. 再起動します。

12. [Visualstudio.com](https://visualstudio.com/xamarin/)から、を使用して、Xamarin の現在の安定バージョンを再インストールします。

## <a name="additional-troubleshooting-steps-for-package-did-not-load-correctly"></a>"パッケージは正しく読み込まれませんでした" の追加のトラブルシューティング手順

上記の手順で "パッケージが正しく読み込まれませんでした" というエラーが解決されない場合は、次の手順を実行してください。

1. 新しい Windows ユーザーアカウントを作成します。

2. 新しいユーザーに対して、Xamarin Visual Studio 拡張機能がエラーなしで読み込まれるかどうかを確認します。

3. 拡張機能が正しく読み込まれた場合、元のユーザーの格納されている設定のいくつかによって問題が発生する可能性が最も高くなります。

    - **エクスプローラ**: _% localappdata%\\Microsoft\\VisualStudio\\1\*.0_
    - **Regedit** – _HKEY\_現在\_ユーザー\\ソフトウェア\\Microsoft\\VisualStudio\\1\*.0_
    - **Regedit** – _HKEY\_現在\_ユーザー\\ソフトウェア\\Microsoft\\VisualStudio\\1\*.0\_構成_

4. これらの格納されている設定が問題であると思われる場合は、バックアップを実行してから削除することができます。
