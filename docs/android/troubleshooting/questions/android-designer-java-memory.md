---
title: Android デザイナーの Java メモリ パラメーターを調整します。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 62FAF21C-8090-4AF3-9D88-05A4CFCAFFDC
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 32d98efd644fb033785fbae0d9689494e42b2809
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="adjusting-java-memory-parameters-for-the-android-designer"></a>Android デザイナーの Java メモリ パラメーターを調整します。

開始するときに使用される既定のメモリのパラメーター、`java`プロセスでは、Android デザイナーが一部のシステム構成と互換性がない可能性があります。

Xamarin Studio 5.7.2.7 (およびそれ以降、Visual Studio for Mac) で始まると、Xamarin for Visual Studio 3.9.344 プロジェクトごとにこれらの設定をカスタマイズすることができます。

## <a name="new-android-designer-properties-and-corresponding-java-options"></a>新しい Android デザイナーのプロパティと対応する Java オプション

次のプロパティ名が示された、java に対応して[コマンド ライン オプション](http://docs.oracle.com/javase/7/docs/technotes/tools/windows/java.html)

- **AndroidDesignerJavaRendererMinMemory** -Xms

- **AndroidDesignerJavaRendererMaxMemory** -Xmx

- **AndroidDesignerJavaRendererPermSize** -XX:MaxPermSize


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Visual Studio でソリューションを開きます。

2.  ソリューション エクスプ ローラーで Android プロジェクトで 1 つを選択し、をクリックして[すべてのファイル](https://msdn.microsoft.com/en-us/library/4afxey9h.aspx)プロジェクトごとに 2 回クリックします。 いずれかが存在しないプロジェクトをスキップする`.axml`レイアウト ファイルです。 この手順は、各プロジェクトのディレクトリが含まれていることを確認、`.csproj.user`ファイル。

3.  Visual Studio を終了します。

4.  検索、`.csproj.user`列ごと手順 2. からプロジェクトのファイルです。

5.  各編集`.csproj.user`ファイルをテキスト エディターでします。

6.  内で新しい Android デザイナー メモリのプロパティの一部またはすべてを追加、`<PropertyGroup>`要素。 既存を使用することができます`<PropertyGroup>`か新規に作成します。 完全な例を次に示します`.csproj.user`を 3 つのすべての属性を含むファイルを既定値に設定します。

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

8.  Visual Studio を再起動し、ソリューションを閉じて再度開きます。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1.  開いているソリューションのディレクトリを確実に Mac を Visual Studio でソリューションに含まれる、`.userprefs`ファイル。

2.  Visual Studio を終了 for mac

3.  検索、`.userprefs`ソリューション ディレクトリにファイル。

4.  編集、`.userprefs`ファイルをテキスト エディターでします。

5.  次の形式の既存の XML 要素を探します。 この要素名の最後の部分は、プロジェクトの名前に一致します。 この例では"AndroidApplication1"。

    ```xml
    <MonoDevelop.Ide.ItemProperties.AndroidApplication1 ... >
    ```

6.  場合、`<MonoDevelop.Ide.ItemProperties.AndroidApplication1 ... >`要素が存在しないか、囲んでいる内の任意の場所に作成`<Properties>`要素。 必ず、"AndroidApplication1"をプロジェクトの名前に置き換えます。

7.  要素の属性として新しいの Android デザイナーのメモリのプロパティの一部またはすべてを追加します。 完全な例を次に示します`.userprefs`を 3 つのすべての属性を含むファイルを既定値に設定します。

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

8.  手順 5 ~ 7 を含むソリューションで Android プロジェクトごとに繰り返します`.axml`レイアウト ファイルです。 (つまり、1 つ追加`<MonoDevelop.Ide.ItemProperties.ProjectName>`の各プロジェクトの要素です)。

9.  保存して閉じます、`.userprefs`ファイル。

10. Mac 用 Visual Studio を再起動し、ソリューションを閉じて再度開きます。

-----

