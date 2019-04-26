---
title: Android Designer の Java メモリ パラメーターの調整
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 62FAF21C-8090-4AF3-9D88-05A4CFCAFFDC
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/02/2018
ms.openlocfilehash: 9c564789f704180e9acc9f96dcba5e7d6eb20634
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "60946739"
---
# <a name="adjusting-java-memory-parameters-for-the-android-designer"></a>Android Designer の Java メモリ パラメーターの調整

開始するときに使用される既定のメモリ パラメーター、`java`プロセスでは、Android designer がいくつかのシステム構成と互換性がない可能性があります。

Xamarin Studio 5.7.2.7 (および以降では、Visual Studio for Mac) を開始し、Visual Studio Tools for Xamarin 3.9.344、これらの設定は、プロジェクトごとにカスタマイズできます。

## <a name="new-android-designer-properties-and-corresponding-java-options"></a>新しい Android デザイナーのプロパティと対応する Java オプション

次のプロパティ名が示された、java 対応[コマンド ライン オプション](http://docs.oracle.com/javase/7/docs/technotes/tools/windows/java.html)

- **AndroidDesignerJavaRendererMinMemory** -Xms

- **AndroidDesignerJavaRendererMaxMemory** -xmx

- **AndroidDesignerJavaRendererPermSize** -XX: MaxPermSize


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1.  Visual Studio でソリューションを開きます。

2.  ソリューション エクスプ ローラーで 1 つずつ Android プロジェクトを選択し、をクリックして[すべてのファイル](https://docs.microsoft.com/en-us/previous-versions/visualstudio/visual-studio-2008/4afxey9h(v=vs.90))プロジェクトごとに 2 回クリックします。 いずれか含まれていないプロジェクトをスキップする`.axml`レイアウト ファイルです。 この手順は、各プロジェクトのディレクトリが含まれていることを確認、`.csproj.user`ファイル。

3.  Visual Studio を終了します。

4.  検索、`.csproj.user`の各手順 2 からプロジェクト ファイル。

5.  各編集`.csproj.user`テキスト エディターでファイル。

6.  内の新しい Android デザイナーのメモリのプロパティの一部またはすべてを追加、`<PropertyGroup>`要素。 既存を使用する`<PropertyGroup>`か新規に作成します。 完全な例を次に示します`.csproj.user`3 つすべての属性を含むファイルが既定値に設定します。

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <Project ToolsVersion="12.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
       <PropertyGroup>
         <ProjectView>ProjectFiles</ProjectView>
       </PropertyGroup>
       <PropertyGroup>
         <AndroidDesignerJavaRendererMinMemory>128m</AndroidDesignerJavaRendererMinMemory>
         <AndroidDesignerJavaRendererMaxMemory>750m</AndroidDesignerJavaRendererMaxMemory>
         <AndroidDesignerJavaRendererPermSize>350m</AndroidDesignerJavaRendererPermSize>
       </PropertyGroup>
    </Project>
    ```

7.  保存して閉じますすべての更新された`.csproj.user`ファイル。

8.  Visual Studio を再起動し、ソリューションをもう一度です。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1.  開いている Visual studio for Mac にソリューション ディレクトリを確認します。 ソリューションに含まれる、`.userprefs`ファイル。

2.  Visual Studio を終了 for mac。

3.  検索、`.userprefs`ソリューション ディレクトリ内のファイル。

4.  編集、`.userprefs`テキスト エディターでファイル。

5.  次の形式で既存の XML 要素を探します。 この要素名の最後の部分には、プロジェクトの名前が一致します。"この例では"AndroidApplication1:

    ```xml
    <MonoDevelop.Ide.ItemProperties.AndroidApplication1 ... >
    ```

6.  場合、`<MonoDevelop.Ide.ItemProperties.AndroidApplication1 ... >`要素が存在しません。 内の任意の場所に作成して`<Properties>`要素。 必ず、"AndroidApplication1"をプロジェクトの名前に置き換えます。

7.  要素の属性として、新しい Android デザイナーのメモリのプロパティの一部またはすべてを追加します。 完全な例を次に示します`.userprefs`3 つすべての属性を含むファイルが既定値に設定します。

    ```xml
    <Properties StartupItem="AndroidApplication1\AndroidApplication1.csproj">
      <MonoDevelop.Ide.Workspace ActiveConfiguration="Debug" PreferredExecutionTarget="Android.SelectDevice" />
      <MonoDevelop.Ide.Workbench />
      <MonoDevelop.Ide.DebuggingService.Breakpoints>
        <BreakpointStore />
      </MonoDevelop.Ide.DebuggingService.Breakpoints>
      <MonoDevelop.Ide.DebuggingService.PinnedWatches />
      <MonoDevelop.Ide.ItemProperties.AndroidApplication1 AndroidDesignerJavaRendererMinMemory="128m" AndroidDesignerJavaRendererMaxMemory="750m" AndroidDesignerJavaRendererPermSize="350m" />
    </Properties>
    ```

8.  手順 5 ~ 7 を含むソリューション内の各 Android プロジェクトを`.axml`レイアウト ファイルです。 (つまり、1 つ追加`<MonoDevelop.Ide.ItemProperties.ProjectName>`の各プロジェクトの要素)。

9.  保存して閉じます、`.userprefs`ファイル。

10. Mac を Visual Studio を再起動し、ソリューションをもう一度します。

-----

