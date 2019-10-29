---
title: IPA ファイルの出力パスを変更できますか。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F5E5DCC6-F7CC-48E2-89E8-709E9C269502
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 6df481688bfaa1e23cc56e6e34586f23d8ab9da6
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031036"
---
# <a name="can-i-change-the-output-path-of-the-ipa-file"></a>IPA ファイルの出力パスを変更できますか。

## <a name="for-cycle-7-and-higher"></a>サイクル7以降
はい。これを実現するには、カスタマイズされた MSBuild ターゲットを使用できます。 最も簡単な方法は、ビルド後に `.ipa` ファイルをコピーすることです。

これらの手順は、Mac または Windows で MSBuild ビルドエンジンを使用するすべての iOS プロジェクトに対して機能します。 (注: すべての Unified API プロジェクトは MSBuild ビルドエンジンを使用します)。

1. IOS アプリプロジェクトの `.csproj` ファイルをテキストエディターで開き、末尾に次の行を追加します (終了 `</Project>` タグの直前)。

    ```xml
    <PropertyGroup>
        <CreateIpaDependsOn>
        $(CreateIpaDependsOn);
        CopyIpa
        </CreateIpaDependsOn>
    </PropertyGroup>
    
    <Target Name="CopyIpa"
            Condition="'$(OutputType)' == 'Exe'
            And '$(ComputedPlatform)' == 'iPhone'
            And '$(BuildIpa)' == 'true'">
        <Copy
            SourceFiles="$(IpaPackagePath)"
            DestinationFolder="$(OutputPath)"
        />
    </Target>
    ```

2. DestinationFolder を目的の出力フォルダーに設定します。 通常どおり、必要に応じて、この引数の中で MSBuild プロパティ ($ (OutputPath) など) を使用できます。

## <a name="notes"></a>ノート

- `CreateIpaDependsOn` プロパティは、Xamarin. iOS の一部である `Xamarin.iOS.Common.targets` ファイルで定義されます。 これは、「[方法: Visual Studio のビルドプロセスを拡張する](https://docs.microsoft.com/visualstudio/msbuild/how-to-extend-the-visual-studio-build-process)」の「[定義済みのターゲットをオーバーライド](https://docs.microsoft.com/visualstudio/msbuild/how-to-extend-the-visual-studio-build-process#overriding-predefined-targets)する」セクションで説明されているように動作します。

- 必要に応じて、**コピー**タスクではなく**移動**タスクを使用できます。 このオプションを選択し、Windows でビルドする場合は、XamarinVS ビルドタスクのあいまいさを避けるために、完全修飾タスク名 `<Microsoft.Build.Tasks.Move>` を使用する必要があります。

## <a name="for-versions-before-xamarin-studio-6005174--xamarin-for-visual-studio-410530"></a>Xamarin Studio 前のバージョンの場合は、6.0.0.5174 |Xamarin for Visual Studio 4.1.0.530

はい。これを実現するには、カスタマイズされた MSBuild ターゲットを使用できます。 最も簡単な方法は、ビルド後に `.ipa` ファイルをコピーすることです。

これらの手順は、Mac または Windows で MSBuild ビルドエンジンを使用するすべての iOS プロジェクトに対して機能します。 (注: すべての Unified API プロジェクトは MSBuild ビルドエンジンを使用します)。

1. IOS アプリプロジェクトの `.csproj` ファイルをテキストエディターで開き、末尾に次の行を追加します (終了 `</Project>` タグの直前)。

    ```xml
    <PropertyGroup>
        <CreateIpaDependsOn>
            $(CreateIpaDependsOn);
            CopyIpa
        </CreateIpaDependsOn>
    </PropertyGroup>

    <Target Name="CopyIpa"
            Condition="'$(OutputType)' == 'Exe'
            And '$(ComputedPlatform)' == 'iPhone'
            And '$(BuildIpa)' == 'true'">
        <Copy
            SourceFiles="$(OutputPath)$(IpaPackageName)"
            DestinationFolder="/Users/macuser/Desktop/"
        />
    </Target>
    ```

2. `DestinationFolder` を目的の出力フォルダーに設定します。 通常どおり、必要に応じて、この引数の中で MSBuild のプロパティ (`$(OutputPath)`など) を使用できます。

## <a name="notes"></a>ノート

- `CreateIpaDependsOn` プロパティは、Xamarin. iOS の一部である `Xamarin.iOS.Common.targets` ファイルで定義されます。 t は、「[方法: Visual Studio のビルドプロセスを拡張する](https://docs.microsoft.com/visualstudio/msbuild/how-to-extend-the-visual-studio-build-process)」の「[定義済みのターゲットをオーバーライド](https://docs.microsoft.com/visualstudio/msbuild/how-to-extend-the-visual-studio-build-process#overriding-predefined-targets)する」セクションで説明されているように動作します。

- 必要に応じて、**コピー**タスクではなく**移動**タスクを使用できます。 このオプションを選択し、Windows でビルドする場合は、XamarinVS ビルドタスクのあいまいさを避けるために、完全修飾タスク名 `<Microsoft.Build.Tasks.Move>` を使用する必要があります。
