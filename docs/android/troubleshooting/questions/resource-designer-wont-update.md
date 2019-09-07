---
title: Android Resource.designer.cs ファイルが更新されない
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 3F7376E3-59CC-4722-AEED-BB50E4D952AA
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/19/2017
ms.openlocfilehash: c175c533e437736849eac6be1f4a90a783123812
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70757142"
---
# <a name="my-android-resourcedesignercs-file-will-not-update"></a>Android Resource.designer.cs ファイルが更新されない

> [!NOTE]
> この問題は Xamarin Studio 5.1.4 以降のバージョンで解決されました。 ただし、Visual Studio for Mac で問題が発生した場合は、完全なバージョン管理情報と完全ビルドログ出力を使用して[新しいバグを作成](~/cross-platform/troubleshooting/questions/howto-file-bug.md)してください。

以前には、.csproj ファイルの xml コードを部分的または完全に削除することで .csproj ファイルが破損しています5.1。 これにより、Android ビルドシステムの重要な部分 (Android Resource.designer.cs の更新など) が失敗します。 7月15日の5.1.4 安定リリースの時点で、このバグは修正されています。ただし、多くの場合、次に示すように、プロジェクトファイルを手動で修復する必要があります。

## <a name="two-possible-approaches-to-fixing-up-the-project-file"></a>プロジェクトファイルの修正方法として2つの方法があります。

**いずれも：**

1. 新しい Xamarin. Android アプリケーションプロジェクトを作成し、すべてのプロジェクトプロパティを古いプロジェクトに一致するように設定して、すべてのリソース、ソースファイルなどをプロジェクトに追加します。

   **または**

2. 元のプロジェクトの .csproj ファイルのバックアップコピーを作成し、テキストエディターで開き、不足している要素を、生成された .csproj ファイルからもう一度追加します。

### <a name="if-this-does-not-solve-the-problem"></a>これで問題が解決しない場合は、

これらの要素を試した後、要素を追加してプロジェクトをリビルドした後、Resource.designer.cs ファイルは更新されますが、コード補完を認識させるためには、ソリューションを閉じて再度開く必要がある場合があります。Resource.designer.cs に格納されている新しい型。 
