---
title: IPA ファイルの出力パスを変更できますか。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F5E5DCC6-F7CC-48E2-89E8-709E9C269502
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/21/2017
ms.openlocfilehash: b8006b1ffe253ac57c1ab435690c5b378cc709fb
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70278667"
---
# <a name="can-i-change-the-output-path-of-the-ipa-file"></a>IPA ファイルの出力パスを変更できますか。

## <a name="for-cycle-7-and-higher"></a>サイクル7以降
はい。これを実現するには、カスタマイズされた MSBuild ターゲットを使用できます。 最も簡単な方法は、ビルド後`.ipa`にファイルをコピーすることです。

これらの手順は、Mac または Windows で MSBuild ビルドエンジンを使用するすべての iOS プロジェクトに対して機能します。 (注: すべての Unified API プロジェクトは MSBuild ビルドエンジンを使用します)。

1. IOS アプリプロジェクトの`</Project>` ファイルをテキストエディターで開き、末尾に次の行を追加します(終了タグの直前)。`.csproj`

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
- プロパティは、Xamarin. iOS `Xamarin.iOS.Common.targets`の一部であるファイルで定義されます。 `CreateIpaDependsOn` この動作は、記事[の「[定義済みのターゲットをオーバーライド](https://docs.microsoft.com/visualstudio/msbuild/how-to-extend-the-visual-studio-build-process#overriding-predefined-targets)する方法」で説明されているように動作します。Visual Studio のビルドプロセス](https://docs.microsoft.com/visualstudio/msbuild/how-to-extend-the-visual-studio-build-process)を拡張します。

- 必要に応じて、**コピー**タスクではなく**移動**タスクを使用できます。 このオプションを選択し、Windows でビルドする場合は、XamarinVS ビルドタスクのあいまいさを避けるために`<Microsoft.Build.Tasks.Move>` 、完全修飾されたタスク名を使用する必要があります。

## <a name="for-versions-before-xamarin-studio-6005174--xamarin-for-visual-studio-410530"></a>Xamarin Studio 前のバージョンの場合は、6.0.0.5174 |Xamarin for Visual Studio 4.1.0.530

はい。これを実現するには、カスタマイズされた MSBuild ターゲットを使用できます。 最も簡単な方法は、ビルド後`.ipa`にファイルをコピーすることです。

これらの手順は、Mac または Windows で MSBuild ビルドエンジンを使用するすべての iOS プロジェクトに対して機能します。 (注: すべての Unified API プロジェクトは MSBuild ビルドエンジンを使用します)。

1. IOS アプリプロジェクトの`</Project>` ファイルをテキストエディターで開き、末尾に次の行を追加します(終了タグの直前)。`.csproj`

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

2. を目的`DestinationFolder`の出力フォルダーに設定します。 通常どおり、必要に応じて、この`$(OutputPath)`引数の中で MSBuild のプロパティ (など) を使用できます。

## <a name="notes"></a>メモ
- プロパティは、Xamarin. iOS `Xamarin.iOS.Common.targets`の一部であるファイルで定義されます。 `CreateIpaDependsOn` t は、記事[の「[定義済みのターゲットをオーバーライド](https://docs.microsoft.com/visualstudio/msbuild/how-to-extend-the-visual-studio-build-process#overriding-predefined-targets)する方法」で説明されているように動作します。Visual Studio のビルドプロセス](https://docs.microsoft.com/visualstudio/msbuild/how-to-extend-the-visual-studio-build-process)を拡張します。

- 必要に応じて、**コピー**タスクではなく**移動**タスクを使用できます。 このオプションを選択し、Windows でビルドする場合は、XamarinVS ビルドタスクのあいまいさを避けるために`<Microsoft.Build.Tasks.Move>` 、完全修飾されたタスク名を使用する必要があります。
