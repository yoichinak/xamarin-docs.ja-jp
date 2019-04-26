---
title: IPA 出力ファイルを TFS 格納フォルダーにコピーする方法はありますか
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: B0F1E09E-7315-45BA-B7FF-44D2063EE19C
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 74e2f2219dcb0908edce7f109844932639038b25
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61421124"
---
# <a name="how-can-i-copy-ipa-output-files-to-the-tfs-drop-folder"></a>IPA 出力ファイルを TFS 格納フォルダーにコピーする方法はありますか

開く、`.csproj`テキスト エディターでの iOS アプリ プロジェクトのファイルを開き、最後に、次の行を追加 (終了する直前に`</Project>`タグ)。

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

- これに記載されている同じ一般的な手法[IPA ファイルの出力パスを変更できますか?](~/ios/troubleshooting/questions/ipa-output-path.md)します。 2 つの重要なポイントが設定するのには`$(TF_BUILD_BINARIESDIRECTORY)`ため追加の条件を追加して移動先フォルダーとして`CopyIpa`は TFS のビルドの場合にのみ実行します。

- 説明については`TF_BUILD_BINARIESDIRECTORY`を参照してください[ビルド変数の定義済み](https://docs.microsoft.com/azure/devops/pipelines/build/variables)します。

## <a name="additional-references"></a>その他の参照

- [Xamarin で使用するための TFS のインストールに関するドキュメント](https://docs.microsoft.com/azure/devops/repos/tfvc/overview)
- [Azure DevOps のビルド タスク。Xamarin.Android](https://docs.microsoft.com/azure/devops/pipelines/tasks/build/xamarin-android)
- [Azure DevOps のビルド タスク。Xamarin.iOS](https://docs.microsoft.com/azure/devops/pipelines/tasks/build/xamarin-ios)

### <a name="next-steps"></a>次の手順

このドキュメントでは、Visual Studio の Xamarin 3.11.666 時点では、現在の動作がについて説明し、ビルド ホストの Mac で Xamarin.iOS 8.10.3 します。 問い合わせ、または上記の情報を使用した後でもこの問題が残っている場合を参照してください、詳細については[Xamarin のどのようなサポート オプションを使用しますか?](~/cross-platform/troubleshooting/support-options.md)連絡先オプション、推奨事項は、についてする方法についても必要な場合は、新しいバグをファイルします。
