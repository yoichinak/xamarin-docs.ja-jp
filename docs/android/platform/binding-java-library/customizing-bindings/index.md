---
title: バインドのカスタマイズ
description: バインドプロセスを制御するメタデータを編集することで、Xamarin の Android バインドをカスタマイズできます。 これらの手動による変更は、ビルドエラーを解決し、/.NET. とC#の一貫性を高めるために生成される API を整形するために必要になることがよくあります。 これらのガイドでは、このメタデータの構造、メタデータの変更方法、および JavaDoc を使用してメソッドパラメーターの名前を回復する方法について説明します。
ms.prod: xamarin
ms.assetid: 63C5078D-9E42-4F70-AF8C-8CEEA84FB6AF
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 09/25/2017
ms.openlocfilehash: e29432504f3b8554c387d277004d3cc779aade95
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2019
ms.locfileid: "69524644"
---
# <a name="customizing-bindings"></a>バインドのカスタマイズ

_バインドプロセスを制御するメタデータを編集することで、Xamarin の Android バインドをカスタマイズできます。これらの手動による変更は、ビルドエラーを解決し、/.NET. とC#の一貫性を高めるために生成される API を整形するために必要になることがよくあります。これらのガイドでは、このメタデータの構造、メタデータの変更方法、および JavaDoc を使用してメソッドパラメーターの名前を回復する方法について説明します。_


## <a name="overview"></a>概要
 
Xamarin. Android では、バインドプロセスの多くが自動化されます。ただし、場合によっては、次の問題に対処するために手動で変更を行う必要があります。

- 不足している型、難読化された型、重複する名前、クラスの可視性の問題、および Xamarin Android ツールで解決できないその他の状況によって発生するビルドエラーを解決しています。 

- Android API をC#のさまざまな型にバインドするために使用されるマッピングを変更する (たとえば、多くの開発者が`int` Java 定数C# `enum`を定数にマップすることを好む)。

- バインドする必要のない未使用の型を削除しています。 

- 基になる Java API に対応するものがない型を追加しています。 

バインドプロセスを制御するメタデータを変更することで、これらの変更の一部またはすべてを行うことができます。


## <a name="guides"></a>ガイド

次のガイドでは、バインドプロセスを制御するメタデータについて説明し、これらの問題に対処するためにこのメタデータを変更する方法を説明します。

- Java バインディング[メタデータ](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md)は、java バインディングに入るメタデータの概要を示します。
    ここでは、Java バインドライブラリを完成させるために必要となる可能性があるさまざまな手動手順について説明します。また、.NET の設計ガイドラインに従うために、バインディングによって公開される API を整形する方法についても説明します。

- [Javadoc を使用したパラメーターの名前付け](~/android/platform/binding-java-library/customizing-bindings/naming-parameters-with-javadoc.md)では、バインドされた java プロジェクトから生成された Javadoc を使用して Java バインドプロジェクトのパラメーター名を回復する方法について説明します。


 

