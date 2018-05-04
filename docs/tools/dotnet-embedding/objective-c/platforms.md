---
title: Objective C プラットフォーム
ms.prod: xamarin
ms.assetid: 43253BE4-A03A-4646-9A14-32C05174E672
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 22652baa941debf7ded2d43cfda8c95ec474816f
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/03/2018
---
# <a name="objective-c-platforms"></a>Objective C プラットフォーム

Objective C コードを生成するときに、.NET の埋め込み対象のさまざまなプラットフォームことができます。

* macOS
* iOS
* tvOS
* [未実装] watchOS

渡すことによって、プラットフォームが選択されている、 `--platform=<platform>` .NET の埋め込みのコマンドライン引数。

ビルドする場合、ios、tvOS と watchOS プラットフォームでは、.NET の埋め込みは常に Xamarin.iOS に多くのこれらのプラットフォームで必要なランタイムのサポート コードが含まれているので、Xamarin.iOS が埋め込まれたフレームワークを作成します。

ただし、macOS プラットフォームを構築する場合、生成されたフレームワークに Xamarin.Mac を埋め込む必要があるかどうかどうかを選択することです。 バインドされているアセンブリが Xamarin.Mac.dll を参照していません (直接または間接的に) とを渡すことによってこれが選択されて、いない Xamarin.Mac を埋め込むことが可能である`--platform=macOS`.NET 埋め込みツールにします。

バインドされているアセンブリに Xamarin.Mac.dll への参照が含まれている場合、Xamarin.Mac を埋め込む必要があるし、さらに、embeddinator が使用するには、どのターゲット フレームワークを知る必要があります。

次の 3 つの可能な Xamarin.Mac ターゲット フレームワークがある: `modern` (旧称`mobile`)、`full`と`system`(それぞれの違いについては、「Xamarin.Mac の[ターゲット フレームワーク][ 1]ドキュメント)、渡すことによって各が選択されていると`--platform=macOS-modern`、`--platform=macOS-full`または`--platform=macOS-system`.NET 埋め込みツールにします。

[1]: ~/mac/platform/target-framework.md
