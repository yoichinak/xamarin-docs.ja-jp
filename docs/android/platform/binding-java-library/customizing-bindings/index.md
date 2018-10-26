---
title: バインドのカスタマイズ
description: バインディング プロセスを制御するメタデータを編集することによって、Xamarin.Android バインドをカスタマイズできます。 手動で行った変更がビルド エラーを解決するためより一貫性のあるように、結果として得られる API を整えるために必要な多くの場合、 C#/.NET します。 これらのガイドでは、このメタデータの構造、メタデータを変更する方法、および JavaDoc を使用して、メソッドのパラメーターの名前を回復する方法について説明します。
ms.prod: xamarin
ms.assetid: 63C5078D-9E42-4F70-AF8C-8CEEA84FB6AF
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 09/25/2017
ms.openlocfilehash: 44bff372225ee1bf555eb3eeb34da918830980b4
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50102387"
---
# <a name="customizing-bindings"></a>バインドのカスタマイズ

_バインディング プロセスを制御するメタデータを編集することによって、Xamarin.Android バインドをカスタマイズできます。手動で行った変更がビルド エラーを解決するためより一貫性のあるように、結果として得られる API を整えるために必要な多くの場合、 C#/.NET します。これらのガイドでは、このメタデータの構造、メタデータを変更する方法、および JavaDoc を使用して、メソッドのパラメーターの名前を回復する方法について説明します。_


## <a name="overview"></a>概要
 
Xamarin.Android では、バインディング プロセスの大部分を自動化します。ただし、場合によっては、次の問題に対処を手動で変更が必要。

-   ビルド型、難読化された型、重複する名前、クラスの可視性の問題、およびその他の状況を解決することはできませんの不足の原因となったエラーの解決、Xamarin.Android ツールでします。 

-   Android API に別の型にバインドする Xamarin.Android を使用するマッピングを変更するC#(Java をマップする多くの開発者が好むなど`int`に定数をC#`enum`定数)。

-   バインドする必要のない未使用の型を削除しています。 

-   基になる Java API の対応のない種類を追加します。 

バインディング プロセスを制御するメタデータを変更することでは、これらの変更の一部またはすべてを行うことができます。


## <a name="guides"></a>ガイド

次のガイドでは、バインディング プロセスを制御するメタデータを記述し、これらの問題に対処するには、このメタデータを変更する方法について説明します。

-   [Java バインドメタ データ](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md)Java バインディングに送られるメタデータの概要を説明します。
    Java バインド ライブラリを完了するために必要な場合があります、さまざまな手動手順について説明しより厳密に次の .NET デザイン ガイドラインへのバインドによって公開された API の整形方法を説明します。

-   [Javadoc でパラメーターの名前を付け](~/android/platform/binding-java-library/customizing-bindings/naming-parameters-with-javadoc.md)にバインドされた Java プロジェクトから生成された Javadoc を使用して Java バインド プロジェクト内のパラメーター名を回復する方法について説明します。


 

