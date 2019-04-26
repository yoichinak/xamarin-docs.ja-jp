---
title: Android Resource.designer.cs ファイルが更新されない
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 3F7376E3-59CC-4722-AEED-BB50E4D952AA
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/19/2017
ms.openlocfilehash: ba3c2b07e7f35bf9fd84d10b74d034a02ca6a73d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61153289"
---
# <a name="my-android-resourcedesignercs-file-will-not-update"></a>Android Resource.designer.cs ファイルが更新されない

> [!NOTE]
> Xamarin Studio 5.1.4 と以降のバージョンでは、この問題を解決されています。 ただし、Visual studio for Mac の問題が発生する場合を提出してください、[新しいバグ](~/cross-platform/troubleshooting/questions/howto-file-bug.md)完全なバージョン管理情報と完全のビルド ログ出力します。

Xamarin.Studio 5.1 のバグでは、部分的または完全には、.csproj ファイルの xml コードを削除することによって、.csproj ファイルを以前と破損しています。 これにより、Android の部分 (Android Resource.designer.cs の更新) などのシステムを構築する重要なが失敗します。 5.1.4 安定した時点では、年 7 月 15 日にリリースでは、このバグが修正されました。多くの場合、プロジェクト ファイルが手動で、以下の説明に従って、修復します。


## <a name="two-possible-approaches-to-fixing-up-the-project-file"></a>プロジェクト ファイルを修正する 2 つのアプローチ

### <a name="either"></a>次のいずれかが原因です。

1) 新しい Xamarin.Android アプリケーション プロジェクトを作成、古いプロジェクトと一致するすべてのプロジェクトのプロパティを設定するすべてのリソースの追加、プロジェクトにソース ファイルなどのバックアップします。

### <a name="or"></a>OR

2) 元のプロジェクトの .csproj ファイルのバックアップ コピーを作成、テキスト エディターで開くと、正常に生成された .csproj ファイルから不足している要素に再度追加します。

### <a name="if-this-does-not-solve-the-problem"></a>問題が解決しない場合

後、これらの要素をみることがわかりますが、する必要がありますも閉じて、再度を認識するコード補完機能を取得するソリューションを開いて、要素を追加して、プロジェクトを再構築、Resource.designer.cs ファイルは更新は後、Resource.designer.cs に含まれる新しい型。 
