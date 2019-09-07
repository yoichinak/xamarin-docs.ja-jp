---
title: Xamarin for Visual Studio の完全なアンインストールを実行するにはどうすればいいですか
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: c1742239-05ea-449d-9c99-611e5e5a90e4
author: conceptdev
ms.author: crdun
ms.date: 12/02/2016
ms.openlocfilehash: 1596e7ed7b3f6d71e13f19a64d111873efb7445c
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70764946"
---
# <a name="how-do-i-perform-a-thorough-uninstall-for-xamarin-for-visual-studio"></a>Xamarin for Visual Studio の完全なアンインストールを実行するにはどうすればいいですか

1. Windows のコントロールパネルで、次のいずれかのものをアンインストールします。

    - Xamarin
    - Xamarin for Windows
    - Xamarin.Android
    - Xamarin.iOS
    - Xamarin for Visual Studio

2. エクスプローラーで、Xamarin Visual Studio 拡張機能フォルダー (_プログラムファイル_と_プログラムファイル (x86)_ の両方を含むすべてのバージョン) から残りのファイルを削除します。

    _C:\\Program Files\*MicrosoftVisualStudio1\*.0Common7\\IDE\\ExtensionsXamarin\\\\\\_

3. Visual Studio の MEF コンポーネントキャッシュディレクトリも削除します。

    _%LOCALAPPDATA%\\Microsoft\\VisualStudio\\1\*.0\\ComponentModelCache_

    実際、この手順では、次のようなエラーを解決するのに十分な場合があります。

    - "XamarinShellPackage" パッケージが正しく読み込まれませんでした "

    - "プロジェクトファイル...を開くことができません。 プロジェクトのサブタイプがありません "

    - "オブジェクト参照がオブジェクトのインスタンスに設定されていません。  at Xamarin.VisualStudio.IOS.XamarinIOSPackage.Initialize()"

    - "パッケージの SetSite に失敗しました" (Visual Studio の_Activitylog .xml_)

    - "パッケージの LegacySitePackage に失敗しました" (Visual Studio の_Activitylog .xml_)

    (「 [CLEAR MEF Component Cache](https://visualstudiogallery.msdn.microsoft.com/22b94661-70c7-4a93-9ca3-8b6dd45f47cd) Visual Studio extension」も参照してください。  また、このようなエラーを引き起こす可能性のある Visual Studio のアップストリームの問題については[、バグ40781、コメント 19](https://bugzilla.xamarin.com/show_bug.cgi?id=40781#c19)に関する説明を参照してください。)

4. また、 _virtualstore_ディレクトリで、_拡張機能\\Xamarin_または_componentmodelcache_ディレクトリのオーバーレイファイルが Windows に格納されているかどうかを確認します。

    _%LOCALAPPDATA%\\VirtualStore_

5. レジストリエディター (`regedit`) を開きます。

6. 次のキーを探します。

    _HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Wow6432Node\\Microsoft\\Windows\\CurrentVersion\\SharedDlls_

7. 次のパターンに一致するエントリがあれば、削除します。

    _C:\\Program Files\*MicrosoftVisualStudio1\*.0Common7\\IDE\\ExtensionsXamarin\\\\\\_

8. このキーを探します。

    _HKEY\_CURRENT\_USERSoftware\\Microsoft\\VisualStudio1\*.0ExtensionManagerpendingdeletions\\\\\\\\_

9. Xamarin に関連すると考えられるすべてのエントリを削除します。  たとえば、以前のバージョンの Xamarin では、次のような問題が発生します。

    _Mono.VisualStudio.Shell,1.0_

10. 管理者`cmd.exe`コマンドプロンプトを開き、インストールされて`devenv /updateconfiguration`いる Visual Studio の各バージョンに対して、コマンド`devenv /setup`とコマンドを実行します。  たとえば、Visual Studio 2015 では次のようになります。

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

    - **エクスプローラーで**- _% localappdata%\\Microsoft\\VisualStudio\\1\*.0_
    - **Regedit** – _\_HKEY CURRENT\_USERSoftwareMicrosoft\\VisualStudio1\\.0\\\\\*_
    - **Regedit** – _\_HKEY CURRENT\_USERSoftwareMicrosoft\\VisualStudio 1.0\*Config\\\\\\\__

4. これらの格納されている設定が問題であると思われる場合は、バックアップを実行してから削除することができます。
