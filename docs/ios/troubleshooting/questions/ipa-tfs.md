---
title: IPA 出力ファイルを TFS のドロップ フォルダーにコピーする方法は?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: B0F1E09E-7315-45BA-B7FF-44D2063EE19C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 087a20ea3b573595e6cbd2b40d77de649676391e
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/26/2018
---
# <a name="how-can-i-copy-ipa-output-files-to-the-tfs-drop-folder"></a>IPA 出力ファイルを TFS のドロップ フォルダーにコピーする方法は?

開く、`.csproj`をテキスト エディターで、iOS アプリのプロジェクトのファイルし、最後に次の行を追加 (終了する直前に`</Project>`タグ)。

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

-   この同じ一般的な手法で説明した[IPA ファイルの出力パスを変更できますか。](~/ios/troubleshooting/questions/ipa-output-path.md)です。 設定する、2 つの重要な点は、`$(TF_BUILD_BINARIESDIRECTORY)`ため追加の条件を追加して保存先フォルダーとして`CopyIpa`TFS ビルド向けにのみ実行されます。

-   詳細については`TF_BUILD_BINARIESDIRECTORY`を参照してください[ https://msdn.microsoft.com/library/hh850448.aspx](https://msdn.microsoft.com/library/hh850448.aspx)です。

## <a name="additional-references"></a>その他のリファレンス

- [Xamarin を使用する TFS のインストールに関するドキュメント](https://docs.microsoft.com/vsts/tfvc/overview)
- [TFS のビルド タスク: Xamarin.Android](https://docs.microsoft.com/vsts/build-release/tasks/build/xamarin-android)
- [TFS のビルド タスク: Xamarin.iOS](https://docs.microsoft.com/vsts/build-release/tasks/build/xamarin-ios)

### <a name="next-steps"></a>次の手順
このドキュメントは、Visual Studio の Xamarin 3.11.666 時点で現在の動作をについて説明し、Xamarin.iOS 8.10.3 Mac 上では、ホストをビルドします。 詳細については、問い合わせ先、または、上記の情報を使用した後もこの問題が残っている場合を参照してください[どのようなサポート オプションは、Xamarin を使用しますか?](~/cross-platform/troubleshooting/support-options.md)推奨事項は、の連絡先のオプションについて方法だけでなく必要な場合は、新しいバグをファイルです。 



