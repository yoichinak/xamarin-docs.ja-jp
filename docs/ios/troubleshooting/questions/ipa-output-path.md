---
title: IPA ファイルの出力パスを変更できますか。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F5E5DCC6-F7CC-48E2-89E8-709E9C269502
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 303d7d58cc0274b8d9f82c33c9f153b9fd00269f
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91437125"
---
# <a name="can-i-change-the-output-path-of-the-ipa-file"></a>IPA ファイルの出力パスを変更できますか。

## <a name="for-cycle-7-and-higher"></a>サイクル7以降
はい。これを実現するには、カスタマイズされた MSBuild ターゲットを使用できます。 最も簡単な方法は、ビルド後にファイルをコピーする `.ipa` ことです。

これらの手順は、Mac または Windows で MSBuild ビルドエンジンを使用するすべての iOS プロジェクトに対して機能します。 (注: すべての Unified API プロジェクトは MSBuild ビルドエンジンを使用します)。

1. `.csproj`IOS アプリプロジェクトのファイルをテキストエディターで開き、末尾に次の行を追加します (終了タグの直前 `</Project>` )。

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

## <a name="notes"></a>メモ

- `CreateIpaDependsOn`プロパティは、 `Xamarin.iOS.Common.targets` Xamarin. iOS の一部であるファイルで定義されます。 これは、「[方法: Visual Studio のビルドプロセスを拡張する](/visualstudio/msbuild/how-to-extend-the-visual-studio-build-process)」の「[定義済みのターゲットをオーバーライド](/visualstudio/msbuild/how-to-extend-the-visual-studio-build-process#overriding-predefined-targets)する」セクションで説明されているように動作します。

- 必要に応じて、**コピー**タスクではなく**移動**タスクを使用できます。 このオプションを選択し、Windows でビルドする場合は、 `<Microsoft.Build.Tasks.Move>` XamarinVS ビルドタスクのあいまいさを避けるために、完全修飾されたタスク名を使用する必要があります。

## <a name="for-versions-before-xamarin-studio-6005174--xamarin-for-visual-studio-410530"></a>Xamarin Studio 前のバージョンの場合は、6.0.0.5174 |Xamarin for Visual Studio 4.1.0.530

はい。これを実現するには、カスタマイズされた MSBuild ターゲットを使用できます。 最も簡単な方法は、ビルド後にファイルをコピーする `.ipa` ことです。

これらの手順は、Mac または Windows で MSBuild ビルドエンジンを使用するすべての iOS プロジェクトに対して機能します。 (注: すべての Unified API プロジェクトは MSBuild ビルドエンジンを使用します)。

1. `.csproj`IOS アプリプロジェクトのファイルをテキストエディターで開き、末尾に次の行を追加します (終了タグの直前 `</Project>` )。

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

2. を `DestinationFolder` 目的の出力フォルダーに設定します。 通常どおり、必要に応じて、 `$(OutputPath)` この引数の中で MSBuild のプロパティ (など) を使用できます。

## <a name="notes"></a>メモ

- `CreateIpaDependsOn`プロパティは、 `Xamarin.iOS.Common.targets` Xamarin. iOS の一部であるファイルで定義されます。 t は、「[方法: Visual Studio のビルドプロセスを拡張する](/visualstudio/msbuild/how-to-extend-the-visual-studio-build-process)」の「[定義済みのターゲットをオーバーライド](/visualstudio/msbuild/how-to-extend-the-visual-studio-build-process#overriding-predefined-targets)する」セクションで説明されているように動作します。

- 必要に応じて、**コピー**タスクではなく**移動**タスクを使用できます。 このオプションを選択し、Windows でビルドする場合は、 `<Microsoft.Build.Tasks.Move>` XamarinVS ビルドタスクのあいまいさを避けるために、完全修飾されたタスク名を使用する必要があります。