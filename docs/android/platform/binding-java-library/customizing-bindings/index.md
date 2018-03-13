---
title: "バインドのカスタマイズ"
description: "Xamarin.Android バインディングをカスタマイズするには、バインディング プロセスを制御するメタデータを編集します。 これらを手動で変更がビルド エラーを解決するため、c# を使用してより一貫性のあるされるように、結果として得られる API を整えるために必要な多くの場合、または .NET です。 これらのガイドは、このメタデータの構造、メタデータを変更する方法、および JavaDoc を使用して、メソッドのパラメーターの名前を回復する方法について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 63C5078D-9E42-4F70-AF8C-8CEEA84FB6AF
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 09/25/2017
ms.openlocfilehash: 14372c3ca42d1ba4a8ade1248f3c5f3210cc7e46
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="customizing-bindings"></a>バインドのカスタマイズ

_Xamarin.Android バインディングをカスタマイズするには、バインディング プロセスを制御するメタデータを編集します。これらを手動で変更がビルド エラーを解決するため、c# を使用してより一貫性のあるされるように、結果として得られる API を整えるために必要な多くの場合、または .NET です。これらのガイドは、このメタデータの構造、メタデータを変更する方法、および JavaDoc を使用して、メソッドのパラメーターの名前を回復する方法について説明します。_


## <a name="overview"></a>概要
 
バインディング プロセスの大部分が自動化されて Xamarin.Androidただし、場合によっては、次の問題に対処を手動で変更が必要。

-   型、難読化された型、重複する名前、クラスの可視性の問題、およびその他の状況を解決できませんされていない場合に発生したエラーを解決するビルド Xamarin.Android ツールでします。 

-   Android API を c# でさまざまな種類にバインドする Xamarin.Android を使用するマッピングを変更する (など、多くの開発者が Java をマップたい`int`c# 定数`enum`定数)。

-   バインドする必要のない未使用の種類を削除しています。 

-   基になる Java API で対応するものが型を追加しません。 

バインディング プロセスを制御するメタデータを変更することにより、これらの変更の一部またはすべてを行うことができます。


## <a name="guides"></a>ガイド

次のガイドでは、バインディング プロセスを制御するメタデータおよびをこれらの問題に対処するには、このメタデータを変更する方法を説明します。

-   [Java バインディング メタデータ](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md)Java バインドになるメタデータの概要を説明します。
    さまざまな Java バインディング ライブラリを完了するために必要な場合があります手動手順を説明しより密接に .NET デザインのガイドラインに従うへのバインドによって公開される API の整形方法を説明します。

-   [Javadoc のパラメーターの名前付け](~/android/platform/binding-java-library/customizing-bindings/naming-parameters-with-javadoc.md)にバインドされた Java プロジェクトから生成される Javadoc を使用してバインドの Java プロジェクト内のパラメーター名を回復する方法について説明します。


 

