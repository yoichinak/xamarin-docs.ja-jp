---
title: Javadoc を使用したパラメーターの名前付け
description: この記事では、java プロジェクトから生成された Javadoc を使用して Java バインドプロジェクトのパラメーター名を回復する方法について説明します。
ms.prod: xamarin
ms.assetid: 59E8EF16-1322-486A-BB16-353804B77356
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/20/2017
ms.openlocfilehash: 77cd5ac86a24769947eefb6941229c56efc3297b
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2019
ms.locfileid: "69524563"
---
# <a name="naming-parameters-with-javadoc"></a>Javadoc を使用したパラメーターの名前付け

_この記事では、java プロジェクトから生成された Javadoc を使用して Java バインドプロジェクトのパラメーター名を回復する方法について説明します。_


## <a name="overview"></a>概要

既存の Java ライブラリをバインドする場合、バインドされた API に関するメタデータの一部は失われます。 具体的には、メソッドのパラメーターの名前です。 パラメーター名は、 `p0` `p1`、などとして表示されます。これは、java `.class`ファイルが java ソースコードで使用されたパラメーター名を保持していないためです。 

元のライブラリから Javadoc HTML にアクセスできる場合は、Xamarin Android Java バインドプロジェクトでパラメーター名を指定できます。 

## <a name="integrating-javadoc-html-into-a-java-binding-project"></a>Java バインドプロジェクトへの Javadoc HTML の統合

Javadoc HTML を Java バインドプロジェクトに統合するには、次の手順で構成される手動のプロセスを実行します。 

1. ライブラリ用の Javadoc をダウンロードする
2. ファイルを編集し、プロパティ`<JavaDocPaths>`を追加します。 `.csproj`
3. プロジェクトのクリーンとリビルド

この処理が完了したら、Java バインドプロジェクトによってバインドされた Api に元の Java パラメーター名が存在する必要があります。 


> [!NOTE]
> JavaDoc の出力には、多くの分散があります。 、.JAR バインディングツールチェーンは、考えられるすべての順列をサポートしているわけではないため、一部のパラメーターに適切な名前を付けることはできません。


## <a name="summary"></a>Summary

この記事では、Java バインドプロジェクトで Javadoc を使用して、バインドされた Api に対して意味のあるパラメーター名を指定する方法について説明しました。 

