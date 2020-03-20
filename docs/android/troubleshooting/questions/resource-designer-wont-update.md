---
title: Android Resource.designer.cs ファイルが更新されない
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 3F7376E3-59CC-4722-AEED-BB50E4D952AA
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/19/2017
ms.openlocfilehash: 4683bbaa5aa48c7b5de5fb9a87a4cd3fbc0aeada
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/10/2020
ms.locfileid: "73019513"
---
# <a name="my-android-resourcedesignercs-file-will-not-update"></a>Android Resource.designer.cs ファイルが更新されない

> [!NOTE]
> この問題は Xamarin Studio 5.1.4 以降のバージョンでは解決されています。 ただし、Visual Studio for Mac で問題が発生した場合は、[新しいバグ](~/cross-platform/troubleshooting/questions/howto-file-bug.md)をバージョン管理情報全体とビルド ログ出力全体を含めて提出してください。

以前は Xamarin.Studio 5.1 のバグによって .csproj ファイル内の xml コードの一部が削除されるか完全に削除されることによって、.csproj ファイルが破損していました。 これにより、Android ビルド システムの重要な部分 (Android Resource.designer.cs の更新など) が失敗していました。 このバグは 7 月 15 日の 5.1.4 安定リリースの時点で修正されています。ただし、多くの場合、次に示すようにプロジェクト ファイルを手動で修復する必要があります。

## <a name="two-possible-approaches-to-fixing-up-the-project-file"></a>プロジェクト ファイルを修正するための 2 つの可能な方法

**次のいずれか:**

1. 新しい Xamarin.Android アプリケーション プロジェクトを作成し、プロジェクトのすべてのプロパティを古いプロジェクトと一致するように設定し、すべてのリソースやソース ファイルなどをプロジェクトに再度追加します。

   **または**

2. 元のプロジェクトの .csproj ファイルのバックアップ コピーを作成した後、それをテキスト エディターで開き、不足している要素をクリーンに生成された .csproj ファイルから追加します。

### <a name="if-this-does-not-solve-the-problem"></a>これで問題が解決しない場合

これらの要素で試した後、要素を再度追加してプロジェクトをリビルドすると、Resource.designer.cs ファイルが更新されていることに気付く場合がありますが、その場合でも、Resource.designer.cs に含まれている新しい型を認識するようにコードを完成させるには、ソリューションを閉じて再度開く必要があります。 
