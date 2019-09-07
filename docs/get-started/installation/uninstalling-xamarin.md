---
title: Xamarin のアンインストール
description: このドキュメントでは、Mac と Windows の両方で Xamarin をアンインストールする方法について説明します。 Mono、Xamarin.Android、Xamarin.iOS、およびその他のツールのアンインストールに関する特定の手順を示します。
ms.prod: xamarin
ms.assetid: b83a85ec-842a-444c-8f82-c2464eda099b
author: conceptdev
ms.author: crdun
ms.date: 04/08/2017
ms.openlocfilehash: a5a9ddfe92bd8f5b743da2c535a93c282542c860
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70756781"
---
# <a name="uninstalling-xamarin"></a>Xamarin のアンインストール

このガイドでは、mac OS または Windows の Visual Studio から Xamarin を削除する方法について説明します。

Universal Installer を使用して Xamarin を再インストールする必要がある場合は、必ず最初にコンピューターを再起動することをお勧めします。

## <a name="uninstalling-xamarin-on-macos"></a>macOS での Xamarin のアンインストール

このガイドでは、関連するセクションに移動して各製品を個別にアンインストールする方法を説明します。 このガイドのすべての手順に従うことで、以下の製品を含めた Xamarin ツールセット全体をアンインストールすることができます。

- [Mono](#uninstallmono)
- [Xamarin.Android](#uninstallandroid)
- [Xamarin.iOS](#uninstallios)
- [Xamarin.Mac](#uninstallmac)
- [Workbooks](#uninstallworkbooks)
- [Xamarin Profiler](#uninstallprofiler)
- [インストーラー](#uninstallinstaller)

> [!TIP]
> Xamarin を macOS コンピューターから削除するときに使用できる[アンインストール スクリプト](https://raw.githubusercontent.com/MicrosoftDocs/visualstudio-docs/master/mac/resources/uninstall-vsmac.sh)を提供しています。 スクリプトの使用については、このガイドの「[アンインストール スクリプトを使用する](#uninstallscript)」を参照してください。

### <a name="uninstalling-visual-studio-for-mac"></a>Visual Studio for Mac のアンインストール

Mac から Xamarin をアンインストールするときは最初に、 **/Applications** ディレクトリで **Visual Studio.app** を探して、それを**ごみ箱**にドラッグします。 または、次の図のように、右クリックして **[ごみ箱に移動]** を選びます。

![Visual Studio アプリケーションをごみ箱に移動する](uninstalling-xamarin-images/uninstall-image1.png)

このアプリ バンドルを削除すると Visual Studio for Mac は削除されますが、Xamarin 関連の他のファイルがファイル システムにまだ残っている可能性があります。

Visual Studio for Mac のすべてのトレースを削除するには、ターミナルで次のコマンドを実行します。

```bash
sudo rm -rf "/Applications/Visual Studio.app"
rm -rf ~/Library/Caches/VisualStudio
rm -rf ~/Library/Preferences/VisualStudio
rm -rf ~/Library/Preferences/Visual\ Studio
rm -rf ~/Library/Logs/VisualStudio
rm -rf ~/Library/VisualStudio
rm -rf ~/Library/Preferences/Xamarin/
rm -rf ~/Library/Developer/Xamarin
rm -rf ~/Library/Application\ Support/VisualStudio
rm -rf ~/Library/Application\ Support/VisualStudio/7.0/LocalInstall/Addins/
```

> [!NOTE]
> Visual Studio for Mac のアンインストール方法の詳細については、docs.microsoft.com の[アンインストール](https://docs.microsoft.com/visualstudio/mac/uninstall) ガイドを参照してください。

<a name="uninstallmono" />

### <a name="uninstall-mono-sdk-mdk"></a>Mono SDK (MDK) をアンインストールする

Mono は .NET Framework のオープン ソースの実装であり、すべての Xamarin 製品 (Xamarin.iOS、Xamarin.Android、Xamarin.Mac) によって、C# でこれらのプラットフォームの開発を可能にするために使われています。

> [!WARNING]
> Xamarin 以外にも Mono を使うアプリケーションがあります (Unity など)。 Mono をアンインストールする前に、Mono に依存するアプリケーションが他にないことを確認してください。

Mono Framework を削除するには、ターミナルで次のコマンドを実行します。

```bash
sudo rm -rf /Library/Frameworks/Mono.framework
sudo pkgutil --forget com.xamarin.mono-MDK.pkg
sudo rm /etc/paths.d/mono-commands
```

<a name="uninstallandroid" />

### <a name="uninstall-xamarinandroid"></a>Xamarin.Android をアンインストールする

Android SDK や Java SDK など、Xamarin.Android を使用するときに必要な項目はたくさんあります、これらを Xamarin.Android のアンインストール時に削除する必要があります。 このセクションでは、必要なすべての部分をアンインストールする方法を説明します。

Xamarin.Android を削除するには、ターミナルで次のコマンドを実行します。

```bash
sudo rm -rf /Developer/MonoDroid
rm -rf ~/Library/MonoAndroid
sudo pkgutil --forget com.xamarin.android.pkg
sudo rm -rf /Library/Frameworks/Xamarin.Android.framework
```

#### <a name="uninstall-android-sdk-and-java-sdk"></a>Android SDK と Java SDK をアンインストールする

Android アプリケーションの開発には Android SDK が必要です。 Android SDK のすべての部分を完全に削除するには、 **~/Library/Developer/Xamarin/** でファイルを探して、**ごみ箱**に移動します。

Java SDK (JDK) は、Mac OS X の一部として既にあらかじめパッケージ化されているので、アンインストールする必要はありません。

#### <a name="uninstall-android-avd"></a>Android AVD をアンインストールする

> [!WARNING]
> Android Studio など、Visual Studio for Mac 以外にも Android AVD とこれらの追加 Android コンポーネントを使うアプリケーションがあります。
> このディレクトリを削除すると、プロジェクトが Android Studio で壊れる可能性があります。 

Android AVD および追加 Android コンポーネントを削除するには、次のコマンドを使います。

```bash
rm -rf ~/.android
```

Android AVD のみを削除するには、次のコマンドを使います。

```bash
rm -rf ~/.android/avd
```

<a name="uninstallios" />

### <a name="uninstall-xamarinios"></a>Xamarin.iOS をアンインストールする

Xamarin.iOS では、C# または F# を使って iOS アプリケーションを開発できます。 Xamarin.iOS をコンピューターからアンインストールするには、次の手順のようにします。

すべての Xamarin.iOS ファイルを削除するには、ターミナルで次のコマンドを使います。

```bash
rm -rf ~/Library/MonoTouch
sudo rm -rf /Library/Frameworks/Xamarin.iOS.framework
sudo rm -rf /Developer/MonoTouch
sudo pkgutil --forget com.xamarin.monotouch.pkg
sudo pkgutil --forget com.xamarin.xamarin-ios-build-host.pkg
sudo pkgutil --forget com.xamarin.xamarin.ios.pkg
```

<a name="uninstallmac" />

### <a name="uninstall-xamarinmac"></a>Xamarin.Mac をアンインストールする

次の 2 つのコマンドを使って Mac から製品とライセンスを除去することにより、コンピューターから Xamarin.Mac を削除できます。

```bash
sudo rm -rf /Library/Frameworks/Xamarin.Mac.framework
rm -rf ~/Library/Xamarin.Mac
```

<a name="uninstallworkbooks" />

### <a name="uninstall-workbooks"></a>Workbooks のアンインストール

Xamarin Workbooks バージョン 1.2.2 以上を削除するには、ターミナルで次のコマンドを使用します。

```bash
sudo /Library/Frameworks/Xamarin.Interactive.framework/Versions/Current/uninstall
```

それ以前のバージョンについては、[Workbooks](~/tools/workbooks/install.md#uninstall-macos) のアンインストールガイドをご覧ください。

<a name="uninstallprofiler" />

### <a name="uninstall-the-xamarin-profiler"></a>Xamarin Profiler をアンインストールする

Xamarin Profiler を削除するには、ターミナルで次のコマンドを使います。

```bash
sudo rm -rf "/Applications/Xamarin Profiler.app"
```

<a name="uninstallinstaller" />

### <a name="uninstall-the-xamarin-installer"></a>Xamarin インストーラーのアンインストール

Xamarin Universal Installer のすべてのトレースを削除するには、次のコマンドを使います。

```bash
rm -rf ~/Library/Caches/XamarinInstaller/
rm -rf ~/Library/Caches/VisualStudioInstaller/
rm -rf ~/Library/Logs/XamarinInstaller/
rm -rf ~/Library/Logs/VisualStudioInstaller/
rm -rf ~/Library/Preferences/Xamarin/
rm -rf "~/Library/Preferences/Visual Studio/"
```

<a name="uninstallscript" />

### <a name="using-the-uninstall-script"></a>アンインストール スクリプトを使用する

このアンインストール スクリプトでは、Visual Studio for Mac とそれに関連する Xamarin コンポーネントを一度にアンインストールできます。

このスクリプトには、この記事にあるほとんどのコマンドが含まれます。 外部依存関係の可能性のためにスクリプトからは次の 2 つが除外されています。

- Mono のアンインストール
- Android AVD のアンインストール

スクリプトを実行するには、次の手順のようにします。

1. スクリプトを右クリックして [名前を付けて保存] を選び、 Mac にファイルを保存します。

2. **ターミナル**を開き、作業ディレクトリをスクリプトをダウンロードした場所に変更します。

    ```
    cd /location/of/file
    ```

3. スクリプトを実行可能にして、**sudo** で実行します。

    ```
    chmod +x ./xamarin_uninstall.sh
    sudo ./xamarin_uninstall.sh
    ```

4. 最後に、アンインストール スクリプトを削除します。

この時点で、Xamarin は、コンピューターからアンインストールされています。

<a name="uninstallwindows" />

## <a name="uninstalling-xamarin-on-windows"></a>Windows での Xamarin のアンインストール

Xamarin は次でサポートされています。

- [Visual Studio 2019 および Visual Studio 2017](#uninstallvs2017)
- [Visual Studio 2015](#uninstallvs2015)
- [Visual Studio 2013](#uninstallvs2015) **[サポートされていません]**
- [Xamarin Studio](#uninstallxamarinstudio) **[サポートされていません]**

<a name="uninstallvs2017" />

### <a name="visual-studio-2019-and-visual-studio-2017"></a>Visual Studio 2017 と Visual Studio 2019

Xamarin は、インストーラーアプリを使用して Visual Studio 2019 および Visual Studio 2017 からアンインストールされます。

1. **[スタート メニュー]** を使用して、**Visual Studio インストーラー**を開きます。

2. 変更するインスタンスの **[変更]** ボタンを押します。

    [![](uninstalling-xamarin-images/vs2017-02-sml.png "[変更] ボタンを押します")](uninstalling-xamarin-images/vs2017-02.png#lightbox)

3. **[ワークロード]** タブで、( **[モバイルとゲーム]** セクションの) **[.NET によるモバイル開発]** オプションの選択を解除します。

    [![](uninstalling-xamarin-images/vs2017-03-sml.png "[モバイル開発] ワークロードをオフにします")](uninstalling-xamarin-images/vs2017-03.png#lightbox)

4. ウィンドウの右下にある **[変更]** ボタンをクリックします。

5. インストーラーは、選択を解除したコンポーネントを削除します (インストーラーで変更を加えるには、Visual Studio 2017 を閉じる必要があります)。

    [![](uninstalling-xamarin-images/vs2017-04-sml.png "[変更] ボタンを押します")](uninstalling-xamarin-images/vs2017-04.png#lightbox)

個別の Xamarin コンポーネント (Profiler、Workbooks など) は、手順 3 で **[個別のコンポーネント]** タブに切り替えて、特定のコンポーネントの選択を解除して、アンインストールすることができます。

[![](uninstalling-xamarin-images/vs2017-components-sml.png "個々のコンポーネントをアンインストールします")](uninstalling-xamarin-images/vs2017-components.png#lightbox)

Visual Studio 2017 を完全にアンインストールするには、 **[起動]** ボタンの横にある 3 本線のメニューから **[アンインストール]** を選びます。

[![](uninstalling-xamarin-images/vs2017-uninstall-sml.png "Visual Studio インストーラーを完全にアンインストールします")](uninstalling-xamarin-images/vs2017-uninstall.png#lightbox)

> [!IMPORTANT]
> Visual Studio の 2 つ (以上) のインスタンスを side-by-side (SxS) でインストールしている場合 (リリース バージョンとプレビュー バージョンなど)、1 つのインスタンスをアンインストールすると、次のような他の Visual Studio インスタンスから一部の Xamarin の機能が削除される可能性があります。
>
> - Xamarin Profiler
> - Xamarin Workbooks/インスペクター
> - Xamarin Remote iOS Simulator
> - Apple Bonjour SDK
>
> 特定の状況では、SxS インスタンスのいずれかをアンインストールすると、これらの機能が誤って削除されることがあります。 これにより、SxS インスタンスをアンインストールした後に、システムに残っている Visual Studio インスタンスの Xamarin プラットフォームのパフォーマンスを低下させる可能性があります。
>
>これは、Visual Studio インストーラーで **[修復]** オプションを実行すると、不足しているコンポーネントが再インストールされ、解決します。

## <a name="uninstalling-older-and-unsupported-products"></a>古い製品とサポート非対象の製品のアンインストール

<a name="uninstallvs2015"></a>

### <a name="visual-studio-2015-and-earlier"></a>Visual Studio 2015 以前

Visual Studio 2015 を完全にアンインストールするには、[visualstudio.com のサポート回答](https://visualstudio.microsoft.com/vs/support/vs2015/uninstall-visual-studio-2015/)を使用してください。

Xamarin は、**コントロール パネル**を使って Windows コンピューターからアンインストールできます。 次の図に示すように、 **[プログラムと機能]** または **[プログラム] > [プログラムのアンインストール]** に移動します。

 [![](uninstalling-xamarin-images/image3.png "図に示すように、[プログラムと機能] または [プログラム] > [プログラムのアンインストール] に移動します。")](uninstalling-xamarin-images/image3.png#lightbox) 

コントロール パネルで、表示されている次のいずれかをアンインストールします。

- Xamarin
- Xamarin for Windows
- Xamarin.Android
- Xamarin.iOS
- Xamarin for Visual Studio

エクスプローラーで、Xamarin Visual Studio 拡張機能フォルダーから残りのすべてのファイルを削除します ([Program Files] と [Program Files (x86)] の両方を含む、すべてのバージョン)。

``` 
C:\Program Files*\Microsoft Visual Studio 1*.0\Common7\IDE\Extensions\Xamarin
```

次の場所にある、Visual Studio の MEF コンポーネント キャッシュ ディレクトリを削除します。

``` 
%LOCALAPPDATA%\Microsoft\VisualStudio\1*.0\ComponentModelCache
```

**Extensions\Xamarin** または **ComponentModelCache** ディレクトリにオーバーレイ ファイルが保存されているかどうかを確認するため、**VirtualStore** ディレクトリにチェックインします。

``` 
%LOCALAPPDATA%\VirtualStore
```

レジストリ エディター (regedit) を開き、次のキーを探します。

``` 
HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\SharedDlls
```

次のパターンに一致するエントリがあれば、削除します。

``` 
C:\Program Files*\Microsoft Visual Studio 1*.0\Common7\IDE\Extensions\Xamarin
```

このキーを探します。

``` 
HKEY_CURRENT_USER\Software\Microsoft\VisualStudio\1*.0\ExtensionManager\PendingDeletions
```

Xamarin に関連すると考えられるすべてのエントリを削除します。 たとえば、`mono` や `xamarin` のような用語を含むものすべてです。

管理者 cmd.exe コマンド プロンプトを開き、Visual Studio のインストールされている各バージョンに対して `devenv /setup` と `devenv /updateconfiguration` のコマンドを実行します。 たとえば、Visual Studio 2015 では次のようになります。

```cmd
"%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\devenv.exe" /setup
"%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\devenv.exe" /updateconfiguration
```

<a name="uninstallxamarinstudio"></a>

### <a name="uninstall-xamarin-studio-on-windows"></a>Windows の Xamarin Studio をアンインストールする

Xamarin Studio は、**コントロール パネル**を使って Windows コンピューターからアンインストールされます。 **[プログラムと機能]** または **[プログラム] > [プログラムのアンインストール]** に移動します。 

Xamarin Studio をアンインストールするには、プログラムの一覧で **[Xamarin Studio 5.x.x]** を見つけて、 **[アンインストール]** ボタンをクリックします。 

### <a name="uninstall-xamarin-studio-on-mac"></a>Mac の Xamarin Studio をアンインストールする

Mac から Xamarin Studio をアンインストールするときは、最初に **/Applications** ディレクトリで **Xamarin Studio.app** を探して、それを**ごみ箱**にドラッグします。 または、右クリックして **[ごみ箱に移動]** を選びます (下図参照)。

 [![](uninstalling-xamarin-images/image1.png "または、この図のように、右クリックして [ごみ箱に移動] を選びます")](uninstalling-xamarin-images/image1.png#lightbox)

このアプリ バンドルを削除すると、Xamarin Studio は削除されますが、Xamarin 関連の他のファイルがファイル システムにまだ残っています。

Xamarin Studio のすべてのトレースを削除するには、ターミナルで次のコマンドを実行する必要があります。

```bash
sudo rm -rf "/Applications/Xamarin Studio.app"
rm -rf ~/Library/Caches/XamarinStudio-*
rm -rf ~/Library/Preferences/XamarinStudio-*
rm -rf ~/Library/Logs/XamarinStudio-*
rm -rf ~/Library/XamarinStudio-*
```

## <a name="summary"></a>Summary

この記事では、ターミナル コマンドを使用して Mac から完全に Xamarin をアンインストールする方法について説明しました。 **[プログラムと機能]** オプション (Visual Studio 2015 以前の場合) を使用して Windows コンピューターから Xamarin をアンインストールする方法と、Visual Studio 2017 で **Visual Studio インストーラー**を使用する方法も説明しました。

## <a name="related-links"></a>関連リンク

- [アンインストール スクリプト (サンプル)](https://raw.githubusercontent.com/MicrosoftDocs/visualstudio-docs/master/mac/resources/uninstall-vsmac.sh)
