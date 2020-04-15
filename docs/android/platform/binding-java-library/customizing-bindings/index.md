---
title: バインドのカスタマイズ
description: バインド プロセスを制御するメタデータを編集することで、Xamarin.Android のバインドをカスタマイズできます。 これらの手動による変更は、ビルド エラーを解決したり、C#/.NET との整合性を高めるために結果の API を整形したりするために、必要になることがよくあります。 これらのガイドでは、このメタデータの構造、メタデータの変更方法、および JavaDoc を使用してメソッドのパラメーターの名前を回復する方法について説明します。
ms.prod: xamarin
ms.assetid: 63C5078D-9E42-4F70-AF8C-8CEEA84FB6AF
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 09/25/2017
ms.openlocfilehash: 04f3720d8684129476c955819390e91330a7800a
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "73020652"
---
# <a name="customizing-bindings"></a>バインドのカスタマイズ

_バインド プロセスを制御するメタデータを編集することで、Xamarin.Android のバインドをカスタマイズできます。これらの手動による変更は、ビルド エラーを解決したり、C#/.NET との整合性を高めるために結果の API を整形したりするために、必要になることがよくあります。これらのガイドでは、このメタデータの構造、メタデータの変更方法、および JavaDoc を使用してメソッドのパラメーターの名前を回復する方法について説明します。_

## <a name="overview"></a>概要

Xamarin.Android では、バインド プロセスの多くが自動化されます。ただし、場合によっては、次の問題に対処するため、手動での変更が必要になります。

- 不足している型、難読化された型、重複する名前、クラスの可視性の問題、および Xamarin.Android のツールで解決できないその他の状況によって発生するビルド エラーの解決。 

- Android API を C# の異なる型にバインドするための、Xamarin.Android で使用されるマッピングの変更 (たとえば、多くの開発者は Java の `int` 定数を C# の `enum` 定数にマップすることを好みます)。

- バインドする必要のない未使用の型の削除。 

- 基になる Java API に対応するものがない型の追加。 

これらの変更の一部または全部を、バインド プロセスを制御するメタデータを変更することによって行うことができます。

## <a name="guides"></a>ガイド

以下のガイドでは、バインド プロセスを制御するメタデータについて説明し、これらの問題に対処するためにこのメタデータを変更する方法を説明します。

- 「[Java バインド メタデータ](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md)」では、Java バインドで使用されるメタデータの概要を説明します。
    Java バインド ライブラリを完成させるために必要になることがあるさまざまな手動手順、および .NET の設計ガイドラインに従うためにバインドによって公開される API を整形する方法について説明します。

- 「[Javadoc でパラメーターに名前を付ける](~/android/platform/binding-java-library/customizing-bindings/naming-parameters-with-javadoc.md)」では、バインドされた Java プロジェクトから生成された Javadoc を使用して、Java バインド プロジェクト内のパラメーター名を回復する方法について説明します。
