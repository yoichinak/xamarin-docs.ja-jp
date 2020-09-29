---
title: IPA の出力ファイルを TFS の格納フォルダーにコピーする方法はありますか
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: B0F1E09E-7315-45BA-B7FF-44D2063EE19C
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 2542eace19cf08a994be99379566e9e621c27190
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91435585"
---
# <a name="how-can-i-copy-ipa-output-files-to-the-tfs-drop-folder"></a>IPA の出力ファイルを TFS の格納フォルダーにコピーする方法はありますか

`.csproj`IOS アプリプロジェクトのファイルをテキストエディターで開き、末尾に次の行を追加します (終了タグの直前 `</Project>` )。

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
        And '$(BuildIpa)' == 'true'
        And '$(TF_BUILD)' == 'true'">
    <Copy
        SourceFiles="$(OutputPath)$(IpaPackageName)"
        DestinationFolder="$(TF_BUILD_BINARIESDIRECTORY)"
    />
</Target>
```

## <a name="notes"></a>メモ

- これは、「 [IPA ファイルの出力パスを変更することはできますか](~/ios/troubleshooting/questions/ipa-output-path.md)」で説明したのと同じ一般的な手法です。 2つの重要な点は、 `$(TF_BUILD_BINARIESDIRECTORY)` 対象フォルダーとしてを設定し、条件を追加して `CopyIpa` TFS ビルドに対してのみを実行することです。

- の説明につい `TF_BUILD_BINARIESDIRECTORY` ては、「 [定義済みのビルド変数](/azure/devops/pipelines/build/variables)」を参照してください。

## <a name="additional-references"></a>その他の参照情報

- [Xamarin で使用するための TFS のインストールに関するドキュメント](/azure/devops/repos/tfvc/overview)
- [Azure DevOps ビルドタスク: Xamarin Android](/azure/devops/pipelines/tasks/build/xamarin-android)
- [Azure DevOps ビルドタスク: Xamarin. iOS](/azure/devops/pipelines/tasks/build/xamarin-ios)

### <a name="next-steps"></a>次の手順

このドキュメントでは、Mac ビルドホストでの Xamarin 3.11.666 for Visual Studio と Xamarin 8.10.3 iOS の現在の動作について説明します。 詳細については、こちらからお問い合わせください。または、上記の情報を利用した後もこの問題が残っている場合は、「 [Xamarin で使用できるサポートオプション](~/cross-platform/troubleshooting/support-options.md) 」を参照してください。連絡先オプション、提案、および必要に応じて新しいバグをファイルに登録する方法については、こちらをご覧ください。