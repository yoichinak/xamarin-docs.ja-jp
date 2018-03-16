---
title: "Xamarin のアンインストール"
description: "コンピューターから Xamarin 製品をアンインストールする"
ms.topic: article
ms.prod: xamarin
ms.assetid: b83a85ec-842a-444c-8f82-c2464eda099b
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 04/08/2017
ms.openlocfilehash: 9b7738736995835ebb6da68d32bdfbec868e73cc
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="uninstalling-xamarin"></a>Xamarin のアンインストール

> [!IMPORTANT]
> この記事では、Mac または Windows コンピューターから Xamarin Studio または他の Xamarin 製品をアンインストールする方法について説明します。 Visual Studio for Mac のアンインストール方法の詳細については、docs.microsoft.com の[アンインストール](https://docs.microsoft.com/visualstudio/mac/uninstall) ガイドを参照してください。

Xamarin Studio のようなスタンドアロン アプリ、および Visual Studio の Xamarin サポートのような他のアプリへの拡張機能など、クロスプラットフォーム アプリケーション開発が可能な Xamarin 製品がいくつかあります。

このガイドでは、macOS の Xamarin 機能、または Windows 上の Visual Studio から Xamarin 機能を削除する方法について説明します。

1.  [Xamarin Studio のアンインストール](#uninstallxamarinstudio)
  1.  [Mono のアンインストール](#uninstallmono)
  1.  [Xamarin.Android のアンインストール](#uninstallandroid)
  1.  [Xamarin.iOS のアンインストール](#uninstallios)
  1.  [Xamarin.Mac のアンインストール](#uninstallmac)
  2.  [Workbooks と Inspector をアンインストールする](#uninstallworkbooks)
1.  [Windows から Xamarin Studio をアンインストールする](#uninstallwindows)
  1.  [Visual Studio 2015 以前から Xamarin をアンインストールする](#uninstallvs2015)
  1.  [Visual Studio 2017 から Xamarin をアンインストールする](#uninstallvs2017)
1.  [Visual Studio for Mac のアンインストール](#uninstallvsmac)

ユニバーサル インストーラーを使用して Xamarin を再インストールする必要がある場合は、常に、最初にコンピューターを再起動することをお勧めします。

## <a name="uninstalling-xamarin-on-mac"></a>Mac での Xamarin のアンインストール

このガイドでは、関連するセクションに移動して各製品を個別にアンインストールする方法を説明します。 このガイドのすべての手順に従うことで、Xamarin ツールセット全体をアンインストールすることができます。

[アンインストール スクリプト](http://download.xamarin.com/developer/cross-platform/xamarin_uninstall.sh)の使用に関するヘルプについては、このガイドの下にある「[アンインストール スクリプトを使用する](#uninstallscript)」に進んでください。

<a name="uninstallxamarinstudio" />

### <a name="uninstall-xamarin-studio"></a>Xamarin Studio のアンインストール

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

<a name="uninstallmono" />

### <a name="uninstall-mono-sdk-mdk"></a>Mono SDK (MDK) をアンインストールする

Mono は Microsoft .NET Framework のオープン ソースの実装であり、すべての Xamarin 製品 (Xamarin.iOS、Xamarin.Android、Xamarin.Mac) によって、C# でこれらのプラットフォームの開発を可能にするために使われています。

> [!IMPORTANT]
> 注: Xamarin 以外にも Mono を使うアプリケーションがあります (Unity など)。 Mono をアンインストールする前に、Mono に依存するアプリケーションが他にないことを確認してください。

Mono Framework をコンピューターから削除するには、ターミナルで次のコマンドを実行します。

```bash
sudo rm -rf /Library/Frameworks/Mono.framework
sudo pkgutil --forget com.xamarin.mono-MDK.pkg
```

<a name="uninstallandroid" />

### <a name="uninstall-xamarinandroid"></a>Xamarin.Android をアンインストールする

Xamarin.Android をインストールして使うために必要なものがいくつかあります (Android SDK や Java SDK など)。 これらの必須コンポーネントの詳細については、[手動インストール](https://docs.microsoft.com/visualstudio/mac/installation/) ガイドを参照してください。

Xamarin.Android を削除するには、次のコマンドを使います。

```bash
sudo rm -rf /Developer/MonoDroid
rm -rf ~/Library/MonoAndroid
sudo pkgutil --forget com.xamarin.android.pkg
sudo rm -rf /Library/Frameworks/Xamarin.Android.framework
```

#### <a name="uninstall-android-sdk-and-java-sdk"></a>Android SDK と Java SDK をアンインストールする

Android アプリケーションの開発には Android SDK が必要です。 Android SDK のすべての部分を完全に削除するには、次の図に示すように、**~/Library/Developer/Xamarin/** でファイルを探して、**ごみ箱**に移動します。

 [![](uninstalling-xamarin-images/image2.png "Android SDK のすべての部分を完全に削除するには、この図に示すように、ファイルを探して、ごみ箱に移動します")](uninstalling-xamarin-images/image2.png#lightbox)

Java SDK (JDK) は、Mac OS X の一部として既にあらかじめパッケージ化されているので、アンインストールする必要はありません。

<a name="uninstallios" />

### <a name="uninstall-xamarinios"></a>Xamarin.iOS をアンインストールする

Xamarin.iOS を使用すると、Mac の Xamarin Studio で C# または F# を使って iOS アプリケーションを開発できます。
以前のバージョンの Xamarin.iOS では、Visual Studio での iOS 開発を可能にするために、Xamarin Build Host も自動的にインストールされました。 両方をコンピューターからアンインストールするには、次の手順のようにします。

ターミナルで次のコマンドを使って、ファイル システムからすべての Xamarin.iOS ファイルを削除します。

```bash
rm -rf ~/Library/MonoTouch
sudo rm -rf /Library/Frameworks/Xamarin.iOS.framework
sudo rm -rf /Developer/MonoTouch
sudo pkgutil --forget com.xamarin.monotouch.pkg
sudo pkgutil --forget com.xamarin.xamarin-ios-build-host.pkg
```

#### <a name="uninstall-the-mac-build-host"></a>Mac Build Host のアンインストール

注: 既に Xamarin 4 に更新している場合は、これは既に削除されている可能性があります。Build Host アプリケーションを削除するには、ターミナルで次のコマンドを実行します。

```bash
sudo rm -rf "/Applications/Xamarin.iOS Build Host.app"
```

Build Host プロセスまたは `launchd` ジョブは、継続して特定のポートで実行およびリッスンする場合があります。
ターミナルで `launchctl list | grep com.xamarin.mtvs.buildserver` を実行すると、その状態を確認できます。

```bash
sudo launchctl unload /Library/LaunchAgents/com.xamarin.mtvs.buildserver.plist
sudo rm -f /Library/LaunchAgents/com.xamarin.mtvs.buildserver.plist
```

<a name="uninstallmac" />

### <a name="uninstall-xamarinmac"></a>Xamarin.Mac をアンインストールする

Xamarin Studio が正常にアンインストールされたら、次の 2 つのコマンドを使って Mac から製品とライセンスを除去することにより、コンピューターから Xamarin.Mac を削除できます。

```bash
sudo rm -rf /Library/Frameworks/Xamarin.Mac.framework
rm -rf ~/Library/Xamarin.Mac
```

<a name="uninstallworkbooks" />

### <a name="uninstall-workbooks-and-inspector"></a>Workbooks と Inspector をアンインストールする

次の Bash コマンドにより、Xamarin Inspector と Workbooks バージョン 1.2.2 以降が削除されます。

```bash
sudo /Library/Frameworks/Xamarin.Interactive.framework/Versions/Current/uninstall
```

それ以前のバージョンについては、[Workbooks](~/tools/workbooks/install.md#uninstall-macos) のアンインストールガイドをご覧ください。

### <a name="uninstall-the-xamarin-installer"></a>Xamarin インストーラーのアンインストール

次のコマンドを使って、Xamarin Universal Installer のすべてのトレースを削除します。

```bash
rm -rf ~/Library/Caches/XamarinInstaller/
rm -rf ~/Library/Logs/XamarinInstaller/
rm -rf ~/Library/Preferences/Xamarin/
```

<a name="uninstallscript" />

### <a name="using-the-uninstall-script"></a>アンインストール スクリプトを使用する

[アンインストール スクリプト](http://download.xamarin.com/developer/cross-platform/xamarin_uninstall.sh)は、コンピューターから Xamarin を削除します。 アンインストール スクリプトを使用するには

1.  アンインストール スクリプトをダウンロードして、ダウンロード場所をメモします。 既定では、この場所は **/Downloads** ディレクトリです。
1.  **ターミナル**を開き、作業ディレクトリをスクリプトをダウンロードした場所に変更します。

        $ cd /location/of/file

1. スクリプトを実行可能にして、**sudo** で実行します。

        $ chmod +x ./xamarin_uninstall.sh
        $ sudo ./xamarin_uninstall.sh

1. 最後に、アンインストール スクリプトを削除します。

この時点で、Xamarin は、コンピューターからアンインストールされています。

<a name="uninstallwindows" />

## <a name="uninstalling-xamarin-on-windows"></a>Windows での Xamarin のアンインストール

<a name="uninstallvs2015" />

### <a name="visual-studio-2015-and-earlier"></a>Visual Studio 2015 以前

Xamarin は、**コントロール パネル**を使って Windows コンピューターからアンインストールできます。 次の図に示すように、**[プログラムと機能]** または **[プログラム] > [プログラムのアンインストール]** に移動します。

 [![](uninstalling-xamarin-images/image3.png "この図に示すように、[プログラムと機能] または [プログラム] の [プログラムのアンインストール] に移動します")](uninstalling-xamarin-images/image3.png#lightbox) [ ![](uninstalling-xamarin-images/image3.png "この図に示すように、[プログラムと機能] または [プログラム] の [プログラムのアンインストール] に移動します")](uninstalling-xamarin-images/image3.png#lightbox)

Xamarin Studio をアンインストールするには、プログラムの一覧で **[Xamarin Studio 5.x.x]** を見つけて、**[アンインストール]** ボタンをクリックします。 Visual Studio 用の Xamarin 拡張子、および SDK を削除するには、プログラムの一覧で **[Xamarin]** を見つけて、**[アンインストール]** をクリックします。 次のスクリーン ショットに示すとおりです。

 [![](uninstalling-xamarin-images/image4a.png "このスクリーン ショットに示すとおりです")](uninstalling-xamarin-images/image4a.png#lightbox)

すべての Xamarin コンポーネントを完全に削除するために、次のプログラムも削除される場合があります。

-  Android SDK


  [![](uninstalling-xamarin-images/image5.png "すべての Xamarin コンポーネントを完全に削除するために、次のプログラムも削除される場合があります")](uninstalling-xamarin-images/image5.png#lightbox)
-  GTK#


  [![](uninstalling-xamarin-images/image6.png "すべての Xamarin コンポーネントを完全に削除するために、次のプログラムも削除される場合があります")](uninstalling-xamarin-images/image6.png#lightbox)
-  Xamarin ユニバーサル インストーラー


 [![](uninstalling-xamarin-images/image7.png "すべての Xamarin コンポーネントを完全に削除するために、次のプログラムも削除される場合があります")](uninstalling-xamarin-images/image7.png#lightbox)
-  Java SDK (他の依存関係がある可能性があるため、これを削除するときは注意してください)


 [![](uninstalling-xamarin-images/image8.png "他の依存関係がある可能性があるため、Java SDK を削除するときは注意してください")](uninstalling-xamarin-images/image8.png#lightbox)

Visual Studio を完全にアンインストールするには、[Microsoft の手順](https://msdn.microsoft.com/library/mt720585.aspx)に従います。


<a name="uninstallvs2017" />

## <a name="visual-studio-2017"></a>Visual Studio 2017

Xamarin は、インストーラー アプリを使用して、Visual Studio 2017 からアンインストールできます。

1. **[スタート メニュー]** を使用して、**Visual Studio インストーラー**を開きます。

  [![](uninstalling-xamarin-images/vs2017-01-sml.png "Visual Studio インストーラーを起動します")](uninstalling-xamarin-images/vs2017-01.png#lightbox)

1. 変更するインスタンスの **[変更]** ボタンを押します。

  [![](uninstalling-xamarin-images/vs2017-02-sml.png "[変更] ボタンを押します")](uninstalling-xamarin-images/vs2017-02.png#lightbox)

1. **[ワークロード]** タブで、(**[モバイルとゲーム]** セクションの) **[.NET によるモバイル開発]** オプションの選択を解除します。

  [![](uninstalling-xamarin-images/vs2017-03-sml.png "[モバイル開発] ワークロードをオフにします")](uninstalling-xamarin-images/vs2017-03.png#lightbox)

1. ウィンドウの右下にある **[変更]** ボタンをクリックします。
1. インストーラーは、選択を解除したコンポーネントを削除します (インストーラーで変更を加えるには、Visual Studio 2017 を閉じる必要があります)。

  [![](uninstalling-xamarin-images/vs2017-04-sml.png "[変更] ボタンを押します")](uninstalling-xamarin-images/vs2017-04.png#lightbox)

個別の Xamarin コンポーネント (Profiler、Workbooks など) は、手順 3 で **[個別のコンポーネント]** タブに切り替えて、特定のコンポーネントの選択を解除して、アンインストールすることができます。

[![](uninstalling-xamarin-images/vs2017-components-sml.png "個々のコンポーネントをアンインストールします")](uninstalling-xamarin-images/vs2017-components.png#lightbox)

Visual Studio 2017 を完全にアンインストールするには、**[起動]** ボタンの横にある 3 本線のメニューから **[アンインストール]** を選びます。

[![](uninstalling-xamarin-images/vs2017-uninstall-sml.png "Visual Studio インストーラーを完全にアンインストールします")](uninstalling-xamarin-images/vs2017-uninstall.png#lightbox)

> [!IMPORTANT]
> **警告:** Visual Studio の 2 つ (以上) のインスタンスを side-by-side (SxS) でインストールしている場合 (リリース バージョンとプレビュー バージョンなど)、1 つのインスタンスをアンインストールすると、次のような他の Visual Studio インスタンスから一部の Xamarin の機能が削除される可能性があります。
>
> - Xamarin Profiler
> - Xamarin Workbooks/インスペクター
> - Xamarin Remote iOS Simulator
> - Apple Bonjour SDK
>
> 特定の状況では、SxS インスタンスのいずれかをアンインストールすると、これらの機能が誤って削除されることがあります。 これにより、SxS インスタンスをアンインストールした後に、システムに残っている Visual Studio インスタンスの Xamarin プラットフォームのパフォーマンスを低下させる可能性があります。
>
>これは、Visual Studio インストーラーで **[修復]** オプションを実行すると、不足しているコンポーネントが再インストールされ、解決する場合があります。

<a name="uninstallvsmac" />

## <a name="uninstalling-visual-studio-for-mac"></a>Visual Studio for Mac のアンインストール

Visual Studio for Mac をアンインストールし、Xamarin Studio を使い続けるには、**/Applications** ディレクトリで **Visual Studio.app** を探して、それをごみ箱にドラッグします。 または、右クリックして **[ごみ箱に移動]** を選びます (下図参照)。

 [![](uninstalling-xamarin-images/image9.png "[Visual Studio] アイコンを右クリックして [ごみ箱に移動] を選択します")](uninstalling-xamarin-images/image9.png#lightbox)

自分のマシンから Xamarin を完全にアンインストールするには、最初に Visual Studio for Mac を削除して、「[Xamarin Studio のアンインストール](#uninstallxamarinstudio)」セクションに一覧された手順に従います。

## <a name="summary"></a>まとめ

この記事では、ターミナル コマンドを使用して Mac から完全に Xamarin にアンインストールする方法、(Visual Studio 2015 以前の場合) **[プログラムと機能]** オプションを使って Windows マシンから Xamarin をアンインストールする方法、Visual Studio 2017 の場合は **Visual Studio インストーラー**を使用する方法について確認しました。


## <a name="related-links"></a>関連リンク

- [アンインストール スクリプト (サンプル)](http://download.xamarin.com/developer/cross-platform/xamarin_uninstall.sh)
