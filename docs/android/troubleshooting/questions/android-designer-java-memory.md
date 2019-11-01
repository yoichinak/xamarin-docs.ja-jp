---
title: Android Designer の Java メモリ パラメーターの調整
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 62FAF21C-8090-4AF3-9D88-05A4CFCAFFDC
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/02/2018
ms.openlocfilehash: 9c9b9f5a205a2eef7db9f27e8d09b10ce65a4318
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027054"
---
# <a name="adjusting-java-memory-parameters-for-the-android-designer"></a>Android Designer の Java メモリ パラメーターの調整

Android designer の `java` プロセスを開始するときに使用される既定のメモリパラメーターは、一部のシステム構成と互換性がない場合があります。

Xamarin Studio 5.7.2.7 (以降、Visual Studio for Mac) および Xamarin 3.9.344 の Visual Studio Tools 以降では、これらの設定はプロジェクトごとにカスタマイズできます。

## <a name="new-android-designer-properties-and-corresponding-java-options"></a>新しい Android designer のプロパティと対応する Java オプション

次のプロパティ名は、指定された java[コマンドラインオプション](https://docs.oracle.com/javase/7/docs/technotes/tools/windows/java.html)に対応しています。

- **AndroidDesignerJavaRendererMinMemory** -Xms

- **AndroidDesignerJavaRendererMaxMemory** -xmx

- **AndroidDesignerJavaRendererPermSize** -XX: MaxPermSize

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Visual Studio でソリューションを開きます。

2. ソリューションエクスプローラーで各 Android プロジェクトを1つずつ選択し、各プロジェクトの [[すべてのファイルを](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2008/4afxey9h(v=vs.90))2 回表示] をクリックします。 `.axml` レイアウトファイルが含まれていないプロジェクトはスキップできます。 この手順では、各プロジェクトディレクトリに `.csproj.user` ファイルが含まれていることを確認します。

3. Visual Studio を終了します。

4. 手順 2. の各プロジェクトの `.csproj.user` ファイルを見つけます。

5. 各 `.csproj.user` ファイルをテキストエディターで編集します。

6. `<PropertyGroup>` の要素内に新しい Android デザイナーのメモリプロパティを追加します。 既存の `<PropertyGroup>` を使用することも、新しいものを作成することもできます。 次の例では、3つの属性すべてが既定値に設定された `.csproj.user` ファイルの完全な例を示します。

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

7. 更新されたすべての `.csproj.user` ファイルを保存して閉じます。

8. Visual Studio を再起動し、ソリューションを再度開きます。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. ソリューションを Visual Studio for Mac で開き、ソリューションディレクトリに `.userprefs` ファイルが含まれていることを確認します。

2. Visual Studio for Mac を終了します。

3. ソリューションディレクトリ内の `.userprefs` ファイルを見つけます。

4. テキストエディターで `.userprefs` ファイルを編集します。

5. 次の形式で既存の XML 要素を見つけます。 この要素名の最後の部分は、プロジェクトの名前 (この例では "AndroidApplication1") と一致します。

    ```xml
    <MonoDevelop.Ide.ItemProperties.AndroidApplication1 ... >
    ```

6. `<MonoDevelop.Ide.ItemProperties.AndroidApplication1 ... >` 要素が存在しない場合は、それを囲む `<Properties>` 要素内の任意の場所に作成します。 必ず "AndroidApplication1" をプロジェクトの名前に置き換えてください。

7. 新しい Android デザイナーメモリプロパティのいずれかまたはすべてを、要素の属性として追加します。 次の例では、3つの属性すべてが既定値に設定された `.userprefs` ファイルの完全な例を示します。

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

8. `.axml` レイアウトファイルが含まれているソリューション内の各 Android プロジェクトに対して、手順5-7 を繰り返します。 (つまり、プロジェクトごとに1つの `<MonoDevelop.Ide.ItemProperties.ProjectName>` 要素を追加します)。

9. `.userprefs` ファイルを保存して閉じます。

10. Visual Studio for Mac を再起動し、ソリューションを再度開きます。

-----
