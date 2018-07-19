---
title: IPA ファイルの出力パスを変更することができますか。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F5E5DCC6-F7CC-48E2-89E8-709E9C269502
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 9c80a209279a2f032eb6c9efcba1398ca0e267a5
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/26/2018
ms.locfileid: "31882803"
---
# <a name="can-i-change-the-output-path-of-the-ipa-file"></a>IPA ファイルの出力パスを変更することができますか。

## <a name="for-cycle-7-and-higher"></a>サイクル 7 以降
はい、これを実現するためにカスタマイズされた MSBuild ターゲットを使用することができます。 最も簡単なオプションはコピーする可能性がありますが、`.ipa`が構築された後のファイルです。

次の手順については、Mac または Windows のいずれかの MSBuild ビルド エンジンを使用するすべての iOS プロジェクトに対して機能します。 (注: Unified API のすべてのプロジェクトが MSBuild のビルド エンジンを使用します)。

1. 開く、`.csproj`をテキスト エディターで、iOS アプリのプロジェクトのファイルし、最後に次の行を追加 (終了する直前に`</Project>`タグ)。
    
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

2. 目的の出力フォルダーに、DestinationFolder を設定します。 通常どおり (する場合は、この引数で $(OutputPath)) のような MSBuild プロパティを使用することがあります。

## <a name="notes"></a>メモ
- `CreateIpaDependsOn`でプロパティが定義されている、 `Xamarin.iOS.Common.targets` Xamarin.iOS の一部であるファイルです。 下に説明どおりに動作する *'DependsOn' プロパティのオーバーライド*で[ https://msdn.microsoft.com/library/ms366724.aspx](https://msdn.microsoft.com/library/ms366724.aspx)です。

- 使用して、**移動**タスクではなく、**コピー**タスクの優先する場合。 Windows で、オプションをビルドすることを選択すると、タスクの完全修飾名を使用する必要があります`<Microsoft.Build.Tasks.Move>`XamarinVS であいまいさを回避するのにタスクを作成します。

## <a name="for-versions-before-xamarin-studio-6005174--xamarin-for-visual-studio-410530"></a>Xamarin Studio 6.0.0.5174 より前に、のバージョン |Xamarin for Visual Studio 4.1.0.530

はい、これを実現するためにカスタマイズされた MSBuild ターゲットを使用することができます。 最も簡単なオプションはコピーする可能性がありますが、`.ipa`が構築された後のファイルです。

次の手順については、Mac または Windows のいずれかの MSBuild ビルド エンジンを使用するすべての iOS プロジェクトに対して機能します。 (注: Unified API のすべてのプロジェクトが MSBuild のビルド エンジンを使用します)。

1. 開く、`.csproj`をテキスト エディターで、iOS アプリのプロジェクトのファイルし、最後に次の行を追加 (終了する直前に`</Project>`タグ)。

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

2. 設定、`DestinationFolder`目的の出力フォルダーにします。 MSBuild プロパティを使用することは通常どおり (と同様に`$(OutputPath)`) する場合は、この引数にします。

## <a name="notes"></a>メモ
- `CreateIpaDependsOn`でプロパティが定義されている、 `Xamarin.iOS.Common.targets` Xamarin.iOS の一部であるファイルです。 下に説明どおりに動作する *"DependsOn"プロパティのオーバーライド*で[ https://msdn.microsoft.com/library/ms366724.aspx](https://msdn.microsoft.com/library/ms366724.aspx)です。

- 使用して、**移動**タスクではなく、**コピー**タスクの優先する場合。 Windows で、オプションをビルドすることを選択すると、タスクの完全修飾名を使用する必要があります`<Microsoft.Build.Tasks.Move>`XamarinVS であいまいさを回避するのにタスクを作成します。
