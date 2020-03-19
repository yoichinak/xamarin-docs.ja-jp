---
title: Xamarin をアンインストールする
description: このドキュメントでは、Windows で Visual Studio から Xamarin をアンインストールする方法について説明します。
ms.prod: xamarin
ms.assetid: b83a85ec-842a-444c-8f82-c2464eda099b
author: conceptdev
ms.author: crdun
ms.date: 01/22/2020
ms.openlocfilehash: 4c9096edddeb00070aaabc3e93b283f2d55c1bfa
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/13/2020
ms.locfileid: "79303643"
---
# <a name="uninstall-xamarin-from-visual-studio"></a>Visual Studio から Xamarin をアンインストールする

このガイドでは、Windows の Visual Studio から Xamarin を削除する方法について説明します。

<a name="uninstallvs2017" />

## <a name="visual-studio-2019-and-visual-studio-2017"></a>Visual Studio 2017 と Visual Studio 2019

Xamarin は、インストーラー アプリを使用して、Visual Studio 2019 および Visual Studio 2017 からアンインストールされます。

1. **[スタート メニュー]** を使用して、**Visual Studio インストーラー**を開きます。

2. 変更するインスタンスの **[変更]** ボタンを押します。

    [![](uninstalling-xamarin-images/vs2017-02-sml.png "Press the modify button")](uninstalling-xamarin-images/vs2017-02.png#lightbox)

3. **[ワークロード]** タブで、( **[モバイルとゲーム]** セクションの) **[.NET によるモバイル開発]** オプションの選択を解除します。

    [![](uninstalling-xamarin-images/vs2017-03-sml.png "Uncheck the Mobile Development workload")](uninstalling-xamarin-images/vs2017-03.png#lightbox)

4. ウィンドウの右下にある **[変更]** ボタンをクリックします。

5. インストーラーは、選択を解除したコンポーネントを削除します (インストーラーで変更を加えるには、Visual Studio 2017 を閉じる必要があります)。

    [![](uninstalling-xamarin-images/vs2017-04-sml.png "Press the Modify button")](uninstalling-xamarin-images/vs2017-04.png#lightbox)

個別の Xamarin コンポーネント (Profiler、Workbooks など) は、手順 3 で **[個別のコンポーネント]** タブに切り替えて、特定のコンポーネントの選択を解除して、アンインストールすることができます。

[![](uninstalling-xamarin-images/vs2017-components-sml.png "Uninstall individual components")](uninstalling-xamarin-images/vs2017-components.png#lightbox)

Visual Studio 2017 を完全にアンインストールするには、 **[起動]** ボタンの横にある 3 本線のメニューから **[アンインストール]** を選びます。

[![](uninstalling-xamarin-images/vs2017-uninstall-sml.png "Uninstall Visual Studio completely")](uninstalling-xamarin-images/vs2017-uninstall.png#lightbox)

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

<a name="uninstallvs2015"></a>

## <a name="visual-studio-2015-and-earlier"></a>Visual Studio 2015 以前

Visual Studio 2015 を完全にアンインストールするには、[visualstudio.com のサポート回答](https://visualstudio.microsoft.com/vs/support/vs2015/uninstall-visual-studio-2015/)を使用してください。

Xamarin は、**コントロール パネル**を使って Windows コンピューターからアンインストールできます。 次の図に示すように、 **[プログラムと機能]** または **[プログラム] > [プログラムのアンインストール]** に移動します。

 [![](uninstalling-xamarin-images/image3.png "Navigate to Programs and Features or Programs  Uninstall a Program as illustrated here")](uninstalling-xamarin-images/image3.png#lightbox)

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
