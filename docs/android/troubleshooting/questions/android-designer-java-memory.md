---
title: Android Designer の Java メモリ パラメーターの調整
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 62FAF21C-8090-4AF3-9D88-05A4CFCAFFDC
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/02/2018
ms.openlocfilehash: 5a692a931bfcdc1e8eee534de3adfff0de688891
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91457901"
---
# <a name="adjusting-java-memory-parameters-for-the-android-designer"></a>Android Designer の Java メモリ パラメーターの調整

Android Designer 用に `java` プロセスを開始するときに使用される既定のメモリ パラメーターは、一部のシステム構成と互換性がない場合があります。

Xamarin Studio 5.7.2.7 (以降、Visual Studio for Mac) および Xamarin 3.9.344 用の Visual Studio Tools 以降では、これらの設定をプロジェクトごとにカスタマイズできます。

## <a name="new-android-designer-properties-and-corresponding-java-options"></a>Android Designer の新しいプロパティと対応する Java オプション

次のプロパティ名は、指定されている Java [コマンドライン オプション](https://docs.oracle.com/javase/7/docs/technotes/tools/windows/java.html)に対応しています

- **AndroidDesignerJavaRendererMinMemory** -Xms

- **AndroidDesignerJavaRendererMaxMemory** -Xmx

- **AndroidDesignerJavaRendererPermSize** -XX:MaxPermSize

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. Visual Studio でソリューションを開きます。

2. ソリューション エクスプローラーで各 Android プロジェクトを 1 つずつ選択し、各プロジェクトで [[すべてのファイルの表示]](/previous-versions/visualstudio/visual-studio-2008/4afxey9h(v=vs.90)) を 2 回クリックします。 `.axml` レイアウト ファイルが含まれていないプロジェクトはスキップできます。 このステップにより、各プロジェクト ディレクトリに `.csproj.user` ファイルが含まれていることを確認します。

3. Visual Studio を終了します。

4. ステップ 2 で各プロジェクトの `.csproj.user` ファイルを見つけます。

5. テキスト エディターで各 `.csproj.user` ファイルを編集します。

6. 新しい Android Designer のメモリ プロパティの一部または全部を、`<PropertyGroup>` 要素内に追加します。 既存の `<PropertyGroup>` を使用するか、新しく作成することができます。 3 つの属性すべてが既定値に設定された `.csproj.user` ファイルの完全な例を次に示します。

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

7. 更新したすべての `.csproj.user` ファイルを保存して閉じます。

8. Visual Studio を再起動し、ソリューションを再度開きます。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

1. ソリューションを Visual Studio for Mac で開き、ソリューション ディレクトリに `.userprefs` ファイルが含まれていることを確認します。

2. Visual Studio for Mac を終了します。

3. ソリューション ディレクトリで `.userprefs` ファイルを見つけます。

4. テキスト エディターで `.userprefs` ファイルを編集します。

5. 次の形式の既存の XML 要素を見つけます。 この要素名の最後の部分は、プロジェクトの名前と一致します: この例では "AndroidApplication1"。

    ```xml
    <MonoDevelop.Ide.ItemProperties.AndroidApplication1 ... >
    ```

6. `<MonoDevelop.Ide.ItemProperties.AndroidApplication1 ... >` 要素が存在しない場合は、囲む `<Properties>` 要素内の任意の場所にそれを作成します。 必ず "AndroidApplication1" を自分のプロジェクトの名前に置き換えてください。

7. 新しい Android Designer のメモリ プロパティの一部または全部を、要素の属性として追加します。 3 つの属性すべてが既定値に設定された `.userprefs` ファイルの完全な例を次に示します。

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

8. `.axml` レイアウト ファイルが含まれるソリューション内の各 Android プロジェクトに対し、ステップ 5 から 7 を繰り返します。 (つまり、各プロジェクトに 1 つの `<MonoDevelop.Ide.ItemProperties.ProjectName>` 要素を追加します)。

9. `.userprefs` ファイルを保存して閉じます。

10. Visual Studio for Mac を再起動し、ソリューションを再度開きます。

-----