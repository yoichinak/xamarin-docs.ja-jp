---
title: IOS クラッシュ ログを symbolicate .dSYM ファイルはどこで入手できますか。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: CB8607B9-FFDA-4617-8210-8E43EC512588
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/09/2018
ms.openlocfilehash: 60d897be8739ff5b78a322bc4ea3f43011785bb5
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/09/2018
---
# <a name="where-can-i-find-the-dsym-file-to-symbolicate-ios-crash-logs"></a>IOS クラッシュ ログを symbolicate .dSYM ファイルはどこで入手できますか。

Mac または Visual Studio 2017 の Visual Studio で iOS アプリを構築するときにクラッシュ レポートを symbolicate に必要な .dSYM ファイルは、アプリのプロジェクト ファイル (.csproj) と同じディレクトリ階層に配置されます。 正確な場所は、プロジェクトのビルド設定によって異なります。

- デバイスに固有のビルドを有効にした場合、次のディレクトリに、.dSYM が見つかりません。

    **&lt;プロジェクト ディレクトリ&gt;/bin/&lt;プラットフォーム&gt;/&lt;構成&gt;/device-builds/&lt;デバイス&gt;- &lt;os バージョン&gt;/**

    例えば:
  
    **TestApp/bin/iPhone/Release/device-builds/iphone8.4-11.3.1/**

- デバイスに固有のビルドを有効にできません、.dSYM 部分は、次のディレクトリで見つかります。

    **&lt;プロジェクト ディレクトリ&gt;/bin/&lt;プラットフォーム&gt;/&lt;構成&gt;/**

    例えば:

    **TestApp、bin、iPhone/リリース/**

> [!NOTE]
> ビルド プロセスの一環としては、Visual Studio 2017 は、Windows、Mac ビルド ホストから .dSYM ファイルをコピーします。 Windows 上の .dSYM ファイルが表示されない場合、アプリのビルド設定を構成したことを確認する[.ipa ファイルを作成する](~/ios/deploy-test/app-distribution/ipa-support.md)です。

## <a name="see-also"></a>関連項目

- [IOS クラッシュ ファイル (Xamarin.iOS) を symbolicating](http://jmillerdev.net/symbolicating-ios-crash-files-xamarin-ios/)
- [IOS アプリケーションのクラッシュ ログを demystifying](https://www.raywenderlich.com/23704/demystifying-ios-application-crash-logs)

