---
title: .NET の埋め込みをインストールします。
description: このドキュメントでは、.NET の埋め込みをインストールする方法について説明します。 一方で、ツールを実行する方法について説明しますバインドを生成する方法、カスタム MSBuild のターゲットとビルド後の必要な手順を使用する方法、自動的にします。
ms.prod: xamarin
ms.assetid: 47106AF3-AC6E-4A0E-B30B-9F73C116DDB3
author: chamons
ms.author: chhamo
ms.date: 04/18/2018
ms.openlocfilehash: 2a572748c21d2a640add3346d1162f4b6bdc8e99
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57668440"
---
# <a name="installing-net-embedding"></a>.NET の埋め込みをインストールします。

## <a name="installing-net-embedding-from-nuget"></a>.NET の埋め込み NuGet からインストールします。

選択**追加 > NuGet パッケージを追加しています.** インストールと**Embeddinator 4000** NuGet パッケージ マネージャーから。

![NuGet パッケージ マネージャー](images/visualstudionuget.png)

これがインストールされます**Embeddinator 4000.exe**と**objcgen**に、**パッケージ/Embeddinator-4000/ツール**ディレクトリ。

一般に、ダウンロードの Embeddinator 4000 の最新リリースを選択してください。 Objective C のサポートには、0.4 が必要がありますまたはそれ以降。

## <a name="running-manually"></a>手動で実行されています。

できたので、NuGet がインストールされている場合、手動でツールを実行できます。

- (MacOS) ターミナルまたはコマンド プロンプト (Windows) を開く
- ソリューションのルートにディレクトリを変更
- ツールにインストールされます。
    - **。/パッケージ/Embeddinator-4000 です。[バージョン]/tools/objcgen** (OBJECTIVE-C)
    - **。/パッケージ/Embeddinator-4000 です。[VERSION]/tools/Embeddinator-4000.exe** (Java/C)
- Macos で**objcgen**直接実行できます。
- Windows で**Embeddinator 4000.exe**直接実行できます。
- Macos で**Embeddinator 4000.exe**併用して実行する必要がある**mono**:
    - `mono ./packages/Embeddinator-4000.[VERSION]/tools/Embeddinator-4000.exe`

各コマンドの呼び出しには、さまざまなプラットフォーム固有のドキュメントに記載のパラメーターが必要です。

## <a name="automatic-binding-generation"></a>自動バインドの生成

ビルド プロセスの一部の .NET の埋め込みを自動的に実行するための 2 つの方法はあります。

- カスタム MSBuild のターゲット
- ビルド後の手順

このドキュメントでは、両方について説明しますが、中に、カスタム MSBuild ターゲットを使用するが適しています。 後のビルド手順は必ずしも、コマンドラインからのビルド時に実行されません。

### <a name="custom-msbuild-targets"></a>カスタム MSBuild のターゲット

MSbuild ターゲットを持つビルドのカスタマイズをまず作成、 **Embeddinator 4000.targets** csproj に次のような横にあるファイル。

```xml
<Project DefaultTargets="Build" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
    <Target Name="RunEmbeddinator" AfterTargets="AfterBuild" Inputs="$(OutputPath)/$(AssemblyName).dll" Outputs="$(IntermediateOutputPath)/Embeddinator/$(AssemblyName).framework/$(AssemblyName)">
        <Exec Command="" />
    </Target>
</Project>
```

ここでは、.NET の埋め込み呼び出しがプラットフォーム固有のドキュメントに記載のいずれかを使用してコマンドのテキストを入力する必要があります。

このターゲットを使用します。

- Mac を Visual Studio 2017 または Visual Studio でプロジェクトを閉じる
- ライブラリの csproj をテキスト エディターで開く
- 最後の上、一番下にこの行を追加`</Project>`行。

```xml
 <Import Project="Embeddinator-4000.targets" />
```

- プロジェクトを再度開きます。

### <a name="post-build-steps"></a>ビルド後の手順

.NET の埋め込みを実行するビルド後のステップを追加する手順は、IDE によって異なります。

#### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

Visual Studio for Mac に移動**プロジェクト オプション > ビルド > カスタム コマンド**を追加し、**後構築**手順。

プラットフォーム固有のドキュメントの一覧にコマンドを設定します。

> [!NOTE]
> NuGet からインストールされているバージョン番号を使用することを確認します。

進行中の開発を行うしようとしているかどうか、C#プロジェクトをクリーンアップするカスタム コマンドも追加可能性があります、**出力**.NET の埋め込みを実行する前にディレクトリ。

```shell
rm -Rf '${SolutionDir}/output/'
```

![カスタムのビルド アクション](images/visualstudiocustombuild.png)

> [!NOTE]
> 指定されるディレクトリで、生成されたバインディングが配置される、`--outdir`または`--out`パラメーター。 一部のプラットフォームではパッケージ名の制限事項と、生成されたバインディング名が異なる場合があります。

#### <a name="visual-studio-2017"></a>Visual Studio 2017

同じことを設定し、基本的には、Visual Studio 2017 でのメニューはわずかに異なります。 シェル コマンドも若干異なります。

移動して**プロジェクト オプション > ビルド イベント**にプラットフォーム固有のドキュメントに記載のコマンドを入力し、**ビルド後に実行するコマンドライン**ボックス。 例:

![.NET Windows 上の埋め込み](images/visualstudiowindows.png)
