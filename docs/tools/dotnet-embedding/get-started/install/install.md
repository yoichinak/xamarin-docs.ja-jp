---
title: インストール (.NET 埋め込みを)
description: このドキュメントでは、.NET 埋め込みをインストールする方法について説明します。 ツールを手動で実行する方法、バインディングを自動的に生成する方法、カスタム MSBuild ターゲットの使用方法、および必要なビルド後の手順について説明します。
ms.prod: xamarin
ms.assetid: 47106AF3-AC6E-4A0E-B30B-9F73C116DDB3
author: chamons
ms.author: chhamo
ms.date: 04/18/2018
ms.openlocfilehash: 47efbaa12475f627b5963cb6613c3441a1d96aac
ms.sourcegitcommit: c9651cad80c2865bc628349d30e82721c01ddb4a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/03/2019
ms.locfileid: "70227843"
---
# <a name="installing-net-embedding"></a>インストール (.NET 埋め込みを)

## <a name="installing-net-embedding-from-nuget"></a>NuGet からの .NET 埋め込みのインストール

[**追加 > nuget**パッケージの追加] を選択し、nuget パッケージマネージャーから**Embeddinator-4000**をインストールします。

![NuGet パッケージ マネージャー](images/visualstudionuget.png)

これにより、 **Embeddinator-4000**と**objcgen**が**packages/Embeddinator-4000/tools**ディレクトリにインストールされます。

一般に、ダウンロードするには、Embeddinator-4000 の最新リリースを選択する必要があります。 目標 C のサポートには0.4 以降が必要です。

## <a name="running-manually"></a>手動での実行

NuGet がインストールされたので、ツールを手動で実行できます。

- ターミナル (macOS) またはコマンドプロンプトを開く (Windows)
- ソリューションルートにディレクトリを変更する
- ツールは次のものにインストールされます。
  - **./packages/Embeddinator-4000.[バージョン]/ツール/objcgen** (目標-C)
  - **./packages/Embeddinator-4000.[バージョン]/tools/Embeddinator-4000.exe** (Java/C)
- MacOS では、 **objcgen**を直接実行できます。
- Windows では、 **Embeddinator-4000**を直接実行できます。
- MacOS では、 **Embeddinator-4000**を**mono**で実行する必要があります。
  - `mono ./packages/Embeddinator-4000.[VERSION]/tools/Embeddinator-4000.exe`

各コマンドの呼び出しには、プラットフォーム固有のドキュメントに記載されているいくつかのパラメーターが必要です。

## <a name="automatic-binding-generation"></a>自動バインド生成

ビルドプロセスの .NET 埋め込み部分を自動的に実行するには、2つの方法があります。

- カスタム MSBuild ターゲット
- ビルド後の手順

このドキュメントでは両方について説明しますが、カスタム MSBuild ターゲットを使用することをお勧めします。 ビルド後のステップは、コマンドラインからビルドするときに必ずしも実行されません。

### <a name="custom-msbuild-targets"></a>カスタム MSBuild ターゲット

MSbuild ターゲットを使用してビルドをカスタマイズするには、まず、次のような .csproj の横に**Embeddinator**ファイルを作成します。

```xml
<Project DefaultTargets="Build" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
    <Target Name="RunEmbeddinator" AfterTargets="AfterBuild" Inputs="$(OutputPath)/$(AssemblyName).dll" Outputs="$(IntermediateOutputPath)/Embeddinator/$(AssemblyName).framework/$(AssemblyName)">
        <Exec Command="" />
    </Target>
</Project>
```

ここでは、プラットフォーム固有のドキュメントに記載されている .NET 埋め込み呼び出しのいずれかを使用して、コマンドのテキストを入力する必要があります。

このターゲットを使用するには:

- Visual Studio 2017 または Visual Studio for Mac でプロジェクトを閉じます。
- ライブラリ .csproj をテキストエディターで開きます。
- 最後`</Project>`の行の上に次の行を追加します。

```xml
 <Import Project="Embeddinator-4000.targets" />
```

- プロジェクトを再度開く

### <a name="post-build-steps"></a>ビルド後の手順

.NET 埋め込みを実行するためのビルド後の手順を追加する手順は、IDE によって異なります。

#### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

Visual Studio for Mac で、[**プロジェクトオプション] を選択し > カスタムコマンドをビルド**して、**ビルド後**の手順を追加 > ます。

プラットフォーム固有のドキュメントに記載されているコマンドをセットアップします。

> [!NOTE]
> NuGet からインストールしたバージョン番号を使用してください。

C#プロジェクトで継続的な開発を行う場合は、.Net 埋め込みを実行する前に、カスタムコマンドを追加して**出力**ディレクトリをクリーンアップすることもできます。

```shell
rm -Rf '${SolutionDir}/output/'
```

![カスタムビルドアクション](images/visualstudiocustombuild.png)

> [!NOTE]
> 生成されたバインディングは、パラメーター `--outdir`または`--out`パラメーターによって示されるディレクトリに配置されます。 生成されるバインド名は、プラットフォームによってはパッケージ名に制限があるため、異なる場合があります。

#### <a name="visual-studio-2017"></a>Visual Studio 2017

基本的には同じことを設定しますが、Visual Studio 2017 のメニューは少し異なります。 シェルコマンドも少し異なります。

[**プロジェクトオプション] > [ビルドイベント**] の順に選択し、プラットフォーム固有のドキュメントに記載されているコマンドを ビルド後に実行する **[コマンドライン]** ボックスに入力します。 例えば:

![Windows での .NET の埋め込み](images/visualstudiowindows.png)
 