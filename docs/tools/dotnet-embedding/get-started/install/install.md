---
title: .NET の埋め込みをインストールします。
description: このドキュメントでは、.NET の埋め込みをインストールする方法について説明します。 手の形で、ツールを実行する方法についても説明のバインドを生成する方法をカスタムの MSBuild ターゲット、および必要なビルド後の手順を使用する方法、自動的にします。
ms.prod: xamarin
ms.assetid: 47106AF3-AC6E-4A0E-B30B-9F73C116DDB3
author: chamons
ms.author: chhamo
ms.date: 4/18/2018
ms.openlocfilehash: 057a1f3f662b2dbe2f8aee277505e1d6e8798084
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34793796"
---
# <a name="installing-net-embedding"></a>.NET の埋め込みをインストールします。

## <a name="installing-net-embedding-from-nuget"></a>.NET は NuGet から埋め込みをインストールします。

選択**追加 > NuGet パッケージを追加しています.** およびインストール**Embeddinator 4000** NuGet パッケージ マネージャーから。

![NuGet パッケージ マネージャー](images/visualstudionuget.png)

これがインストールされます**Embeddinator 4000.exe**と**objcgen**に、**パッケージ/Embeddinator-4000/ツール**ディレクトリ。

一般をダウンロードする Embeddinator 4000 の最新のリリースを選択してください。 Objective C のサポートが必要です 0.4 またはそれ以降。

## <a name="running-manually"></a>手動で実行します。

NuGet をインストールすると、手動でツールを実行できます。

- ターミナル (macOS) または (Windows) のコマンド プロンプトを開く
- ソリューションは、ルート ディレクトリを変更します。
- ツールにインストールされます。
    - **。/パッケージ/Embeddinator-4000 です。[バージョン]/tools/objcgen** (Objective C)
    - **。/パッケージ/Embeddinator-4000 です。[VERSION]/tools/Embeddinator-4000.exe** (Java/C) 
- Macos、 **objcgen**直接実行することができます。 
- Windows では、 **Embeddinator 4000.exe**直接実行することができます。
- Macos、 **Embeddinator 4000.exe**と一緒に実行する必要がある**モノラル**: 
    - `mono ./packages/Embeddinator-4000.[VERSION]/tools/Embeddinator-4000.exe`

コマンドの呼び出しのたびには、プラットフォーム固有のドキュメントに一覧表示のパラメーターの数が必要です。

## <a name="automatic-binding-generation"></a>自動バインド生成

ビルド プロセスの一部の .NET の埋め込みを自動的に実行するための 2 つの方法はあります。

- カスタムの MSBuild ターゲット
- ビルド後の手順

このドキュメントでは、両方について説明、中に、カスタム MSBuild ターゲットを使用するが適しています。 ビルド ステップの post は必ずしも、コマンドラインからのビルド時に実行されません。

### <a name="custom-msbuild-targets"></a>カスタムの MSBuild ターゲット

MSbuild ターゲットを持つ、ビルドをカスタマイズするには、最初に作成、 **Embeddinator 4000.targets**の横にある次のような csproj ファイル。

```xml
<Project DefaultTargets="Build" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
    <Target Name="RunEmbeddinator" AfterTargets="AfterBuild" Inputs="$(OutputPath)/$(AssemblyName).dll" Outputs="$(IntermediateOutputPath)/Embeddinator/$(AssemblyName).framework/$(AssemblyName)">
        <Exec Command="" />
    </Target>
</Project>
```

ここでは、プラットフォーム固有のドキュメントに表示されている .NET の埋め込みの呼び出しのいずれかを使用して、コマンドのテキストを入力する必要があります。

このターゲットを使用します。

- Mac 用 Visual Studio 2017 または Visual Studio でプロジェクトを閉じる
- ライブラリ csproj をテキスト エディターで開く
- 最後の上の下部にあるこの行を追加`</Project>`行。

```xml
 <Import Project="Embeddinator-4000.targets" />
```

- プロジェクトを閉じて再度開きます。

### <a name="post-build-steps"></a>ビルド後の手順

.NET の埋め込みを実行するビルド後のステップを追加する手順は、IDE によって異なります。

#### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

Mac 用 Visual Studio でに移動**プロジェクトのオプション > ビルド > カスタム コマンド**を追加し、**後にビルド**手順です。

プラットフォーム固有のドキュメントで説明されているコマンドを設定します。

> [!NOTE]
> NuGet をインストールしたバージョン番号を使用することを確認します。

C# プロジェクトの開発を継続的な操作を実行する場合は、追加することも、カスタム コマンドをクリーンアップするのには、**出力**.NET の埋め込みを実行する前にディレクトリ。

```shell
rm -Rf '${SolutionDir}/output/'
```

![カスタム ビルド アクション](images/visualstudiocustombuild.png)

> [!NOTE]
> 生成されたバインディングで指定されたディレクトリに配置されます、`--outdir`または`--out`パラメーター。 一部のプラットフォームではパッケージ名の制限事項と、生成されたバインディングの名前が異なる場合があります。

#### <a name="visual-studio-2017"></a>Visual Studio 2017

同じを設定し、本質的にが、Visual Studio 2017 のメニューはわずかに異なります。 シェル コマンドも若干異なります。

移動して**プロジェクトのオプション > ビルド イベント**にプラットフォーム固有のドキュメントで説明されているコマンドを入力し、**ビルド後に実行するコマンドライン**ボックス。 例えば:

![.NET Windows 上の埋め込み](images/visualstudiowindows.png)
