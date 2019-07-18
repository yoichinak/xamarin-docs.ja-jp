---
title: Javadoc でパラメーターの名前を付ける
description: この記事では、Java プロジェクトから生成された Javadoc を使用して、Java バインド プロジェクト内のパラメーター名を回復する方法について説明します。
ms.prod: xamarin
ms.assetid: 59E8EF16-1322-486A-BB16-353804B77356
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/20/2017
ms.openlocfilehash: e394377043953a297afed36a3ce0747a3e6d1512
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "60955872"
---
# <a name="naming-parameters-with-javadoc"></a>Javadoc でパラメーターの名前を付ける

_この記事では、Java プロジェクトから生成された Javadoc を使用して、Java バインド プロジェクト内のパラメーター名を回復する方法について説明します。_


## <a name="overview"></a>概要

既存の Java ライブラリをバインドするときに、バインドされた API に関するいくつかのメタデータは失われます。 具体的にはメソッドにパラメーターの名前。 パラメーター名は、として表示されます`p0`、`p1`など。これは、Java`.class`ファイルには、Java ソース コードで使用されたパラメーター名は保持されません。 

Java の Xamarin.Android バインド プロジェクトは、元のライブラリの Javadoc HTML にアクセス権がある場合、パラメーター名を指定できます。 

## <a name="integrating-javadoc-html-into-a-java-binding-project"></a>Java プロジェクトのバインドへの HTML の Javadoc の統合

Java バインド プロジェクトに Javadoc HTML の統合は、次の手順から成る手動のプロセスです。 

1.  ライブラリの Javadoc をダウンロードします。
2.  編集、`.csproj`追加ファイルを開き、`<JavaDocPaths>`プロパティ。
3.  消去し、プロジェクトのリビルド

これが完了すると、元の Java パラメーター名は Java バインド プロジェクトがバインドされている Api に存在する必要があります。 


> [!NOTE]
> 大量の JavaDoc の出力に差異があります。 します。JAR のバインディング ツール チェーンがすべて 1 つの可能な順列をサポートしていませんし、そのいくつかのパラメーターが正しくという名前はします。


## <a name="summary"></a>まとめ

この記事で説明する方法は、バインドされた Api の意味パラメーターの名前を提供するのに Java バインド プロジェクトで Javadoc を使用します。 

