---
title: IPA ファイルの出力パスを変更することができますか。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F5E5DCC6-F7CC-48E2-89E8-709E9C269502
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 8b0686a91f18b41aa8e2e7db071123c0d96723a0
ms.sourcegitcommit: 32c7cf8b0d00464779e4b0ea43e2fd996632ebe0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/17/2019
ms.locfileid: "68290103"
---
# <a name="can-i-change-the-output-path-of-the-ipa-file"></a>IPA ファイルの出力パスを変更することができますか。

## <a name="for-cycle-7-and-higher"></a>サイクル 7 以降
はい、これを実現するためにカスタマイズされた MSBuild のターゲットを使用することができます。 最も簡単なオプションはコピーする可能性がありますが、`.ipa`ファイルがビルドされています。

次の手順については、Mac または Windows のいずれかの MSBuild ビルド エンジンを使用するすべての iOS プロジェクトに対して機能します。 (注: すべての Unified API プロジェクトが MSBuild ビルド エンジンを使用します)。

1. 開く、`.csproj`テキスト エディターでの iOS アプリ プロジェクトのファイルを開き、最後に、次の行を追加 (終了する直前に`</Project>`タグ)。
    
    ```
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

2. 目的の出力フォルダーに、DestinationFolder を設定します。 通常どおり (する場合は、この引数内 $(OutputPath)) などの MSBuild プロパティを使用することがあります。

## <a name="notes"></a>メモ
- `CreateIpaDependsOn`でプロパティが定義されている、`Xamarin.iOS.Common.targets`されている Xamarin.iOS の一部のファイルします。 動作」の説明に従って、[定義済みのターゲットをオーバーライドする](https://docs.microsoft.com/visualstudio/msbuild/how-to-extend-the-visual-studio-build-process#overriding-predefined-targets)、記事の「[方法。Visual Studio ビルド処理を拡張](https://docs.microsoft.com/visualstudio/msbuild/how-to-extend-the-visual-studio-build-process)します。

- 使って、**移動**タスクではなく**コピー**タスクの優先する場合は。 Windows のオプションをビルドする場合は、タスクの完全修飾名を使用する必要があります。 `<Microsoft.Build.Tasks.Move>` 、XamarinVS であいまいさを回避するためには、ビルドのタスク。

## <a name="for-versions-before-xamarin-studio-6005174--xamarin-for-visual-studio-410530"></a>Xamarin Studio 6.0.0.5174 より前に、のバージョン |Xamarin for Visual Studio 4.1.0.530

はい、これを実現するためにカスタマイズされた MSBuild のターゲットを使用することができます。 最も簡単なオプションはコピーする可能性がありますが、`.ipa`ファイルがビルドされています。

次の手順については、Mac または Windows のいずれかの MSBuild ビルド エンジンを使用するすべての iOS プロジェクトに対して機能します。 (注: すべての Unified API プロジェクトが MSBuild ビルド エンジンを使用します)。

1. 開く、`.csproj`テキスト エディターでの iOS アプリ プロジェクトのファイルを開き、最後に、次の行を追加 (終了する直前に`</Project>`タグ)。

    ```csharp
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

2. 設定、`DestinationFolder`目的の出力フォルダーにします。 通常どおりに MSBuild プロパティを使用することがあります (など`$(OutputPath)`) する場合は、この引数にします。

## <a name="notes"></a>メモ
- `CreateIpaDependsOn`でプロパティが定義されている、`Xamarin.iOS.Common.targets`されている Xamarin.iOS の一部のファイルします。 t の動作」の説明に従って、[定義済みのターゲットをオーバーライドする](https://docs.microsoft.com/visualstudio/msbuild/how-to-extend-the-visual-studio-build-process#overriding-predefined-targets)、記事の「[方法。Visual Studio ビルド処理を拡張](https://docs.microsoft.com/visualstudio/msbuild/how-to-extend-the-visual-studio-build-process)します。

- 使って、**移動**タスクではなく**コピー**タスクの優先する場合は。 Windows のオプションをビルドする場合は、タスクの完全修飾名を使用する必要があります。 `<Microsoft.Build.Tasks.Move>` 、XamarinVS であいまいさを回避するためには、ビルドのタスク。
