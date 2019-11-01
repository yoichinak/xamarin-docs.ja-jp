---
title: iOS クラッシュ ログにシンボル名を付加するための .dSYM ファイルはどこで入手できますか。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: CB8607B9-FFDA-4617-8210-8E43EC512588
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 05/09/2018
ms.openlocfilehash: 418a0196849099da03983085aca9ceed2077207b
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030914"
---
# <a name="where-can-i-find-the-dsym-file-to-symbolicate-ios-crash-logs"></a>iOS クラッシュ ログにシンボル名を付加するための .dSYM ファイルはどこで入手できますか。

Visual Studio for Mac または Visual Studio 2017 を使用して iOS アプリをビルドする場合、クラッシュレポートをもするために必要な dSYM ファイルは、アプリケーションのプロジェクトファイル (.csproj) と同じディレクトリ階層に配置されます。 正確な場所は、プロジェクトのビルド設定によって異なります。

- デバイス固有のビルドを有効にした場合、dSYM は次のディレクトリにあります。

    **&lt;プロジェクトディレクトリ&gt;/bin/&lt;プラットフォーム&gt;/&lt;構成&gt;/device-builds/&lt;デバイス&gt;-&lt;os バージョン&gt;/**

    (例:
  
    **TestApp/bin/iPhone/Release/デバイス-ビルド/iphone 8.4-11.3.1/**

- デバイス固有のビルドを有効にしていない場合、dSYM は次のディレクトリにあります。

    **&lt;プロジェクトディレクトリ&gt;/bin/&lt;プラットフォーム&gt;/&lt;構成&gt;/**

    (例:

    **TestApp/bin/iPhone/Release/**

> [!NOTE]
> ビルド処理の一部として、Visual Studio 2017 では、Mac ビルドホストから Windows に dSYM ファイルがコピーされます。 Windows に dSYM ファイルが表示されない場合は、 [ipa ファイルを作成](~/ios/deploy-test/app-distribution/ipa-support.md)するようにアプリのビルド設定が構成されていることを確認してください。

## <a name="see-also"></a>関連項目

- [Symbolicating iOS クラッシュファイル (Xamarin)](https://www.jmillerdev.net/symbolicating-ios-crash-files-xamarin-ios/)
- [Demystifying iOS アプリケーションのクラッシュログ](https://www.raywenderlich.com/23704/demystifying-ios-application-crash-logs)
