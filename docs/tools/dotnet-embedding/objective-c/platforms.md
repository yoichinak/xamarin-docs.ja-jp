---
title: Objective C プラットフォーム
description: このドキュメントでは、OBJECTIVE-C コードを使用する場合に、.NET の埋め込みが対象とするさまざまなプラットフォームについて説明します。 これは、macOS、iOS、tvOS、watchOS、について説明します。
ms.prod: xamarin
ms.assetid: 43253BE4-A03A-4646-9A14-32C05174E672
author: lobrien
ms.author: laobri
ms.date: 11/14/2017
ms.openlocfilehash: 8091fb4e8328f61f1471d061b51b4735de3c089c
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50108883"
---
# <a name="objective-c-platforms"></a>Objective C プラットフォーム

.NET の埋め込みと、OBJECTIVE-C コードを生成するときにさまざまなプラットフォームをターゲットできます。

* macOS
* iOS
* tvOS
* watchOS [未実装]

渡すことによって、プラットフォームが選択されている、`--platform=<platform>`コマンドライン引数 .NET の埋め込みを指定します。

Ios のビルド時に .NET の埋め込み、tvOS、watchOS のプラットフォームは常に Xamarin.iOS には、これらのプラットフォームで必要なランタイムのサポート コードの多くが含まれているため、Xamarin.iOS を埋め込むためのフレームワークを作成します。

ただし、macOS プラットフォームを構築する場合、生成されたフレームワークに Xamarin.Mac を埋め込む必要があるかどうかどうかを選択することは。 バインドされているアセンブリが Xamarin.Mac.dll を参照していません (直接または間接的に)、およびを渡すことによってこれが選択されている場合に、Xamarin.Mac を組み込まないことは`--platform=macOS`.NET 埋め込みツールにします。

バインドされているアセンブリに Xamarin.Mac.dll への参照が含まれている場合は、Xamarin.Mac を埋め込む必要があるし、さらに、embeddinator が使用するには、どのターゲット フレームワークを知る必要があります。

次の 3 つの可能な Xamarin.Mac ターゲット フレームワークがあります: `modern` (旧称`mobile`)、`full`と`system`(それぞれの違いについては、「Xamarin.Mac の[ターゲット フレームワーク][ 1]ドキュメント)、それぞれが渡すことによって選択されていると`--platform=macOS-modern`、`--platform=macOS-full`または`--platform=macOS-system`.NET 埋め込みツールにします。

[1]: ~/mac/platform/target-framework.md
