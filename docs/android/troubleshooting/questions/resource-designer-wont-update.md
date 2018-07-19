---
title: Android Resource.designer.cs ファイルは更新されません。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 3F7376E3-59CC-4722-AEED-BB50E4D952AA
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/19/2017
ms.openlocfilehash: 6d20c92fbb61ab11fcfeda3e9d2f63fdfc5a6d54
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
ms.locfileid: "30762621"
---
# <a name="my-android-resourcedesignercs-file-will-not-update"></a>Android Resource.designer.cs ファイルは更新されません。

> [!NOTE]
> この問題は、Xamarin Studio 5.1.4 およびそれ以降のバージョンでは解決されています。 ただし、Mac を Visual Studio で問題が発生する場合を送信してください、[新しいバグ](~/cross-platform/troubleshooting/questions/howto-file-bug.md)すべてのバージョン管理情報と完全のビルド ログ出力します。

Xamarin.Studio 5.1 のバグでは、部分的または完全には、.csproj ファイルの xml コードを削除することによって、.csproj ファイルを以前と破損しています。 これが原因で重要な部分、Android のビルド システム (Android Resource.designer.cs の更新) などが失敗します。 5.1.4 安定した時点では、年 7 月 15 日にリリースでは、このバグが修正されました。多くの場合、プロジェクト ファイルを手動で、以下の説明に従って、修復します。


## <a name="two-possible-approaches-to-fixing-up-the-project-file"></a>2 つの可能なアプローチをプロジェクト ファイルを修正するには

### <a name="either"></a>次のいずれかが原因です。

1) まったく新しい Xamarin.Android アプリケーション プロジェクトを作成、古いプロジェクトと一致するすべてのプロジェクト プロパティを設定すべてのリソースの追加、ソース ファイルなど、プロジェクトにバックアップします。

### <a name="or"></a>OR

2) 元のプロジェクトの .csproj ファイルのバックアップ コピーを作成をテキスト エディターで開くと、正常に生成された .csproj ファイルから要素が欠落で再度追加します。

### <a name="if-this-does-not-solve-the-problem"></a>問題が解決しない場合

後、これらの要素を試し、表示した後、要素を追加して、プロジェクトを再構築、Resource.designer.cs ファイルを更新、し、必要しますが、ありますまだ閉じてから再度開いてを認識するコード補完機能を取得するようにソリューションをResource.designer.cs に含まれる新しい型。 
