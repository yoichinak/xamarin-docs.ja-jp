---
title: "Javadoc のパラメーターの名前を付ける"
description: "この記事では、Java プロジェクトから生成される Javadoc を使用して、Java のバインドのプロジェクト内のパラメーター名を復元する方法について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 59E8EF16-1322-486A-BB16-353804B77356
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/20/2017
ms.openlocfilehash: d83135aa9c101e06a680b458cce8c12dcdddd947
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2018
---
# <a name="naming-parameters-with-javadoc"></a>Javadoc のパラメーターの名前を付ける

_この記事では、Java プロジェクトから生成される Javadoc を使用して、Java のバインドのプロジェクト内のパラメーター名を復元する方法について説明します。_


## <a name="overview"></a>概要

既存の Java ライブラリをバインドするときに、バインドされている API に関する一部のメタデータは失われます。 具体的にはメソッドにパラメーターの名前。 パラメーター名として表示されます`p0`、 `p1`, などです。これは、Java`.class`ファイルは Java ソース コードで使用されたパラメーター名を保持しません。 

Xamarin.Android プロジェクトのバインドは、元のライブラリから Javadoc HTML へのアクセスがある場合、パラメーター名を提供できます。 

## <a name="integrating-javadoc-html-into-a-java-binding-project"></a>プロジェクトのバインド Java Javadoc HTML に統合します。

次の手順から成る手動のプロセスは、Java バインド プロジェクトへの Javadoc HTML の統合します。 

1.  ライブラリの Javadoc をダウンロードします。
2.  編集、`.csproj`ファイルを追加、`<JavaDocPaths>`プロパティ。
3.  消去し、プロジェクトをリビルドします

これを完了すると、元の Java パラメーター名はバインドの Java プロジェクトによってバインドされる Api 内に存在する必要があります。 


> [!NOTE]
> JavaDoc の出力に差異が大幅に向上があります。 します。JAR バインド ツール チェーンはすべて 1 つの可能な順列をサポートしていませんし、したがっていくつかのパラメーターが正しくという名前は使用します。


## <a name="summary"></a>まとめ

この記事の方法について説明では、Java バインド プロジェクトで Javadoc を使用して、意味パラメーターがバインドされている Api の名前を指定します。 

