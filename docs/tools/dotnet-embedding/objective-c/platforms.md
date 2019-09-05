---
title: 目的 C プラットフォーム
description: このドキュメントでは、.NET 埋め込みが目標 C コードを操作するときにターゲットにできるさまざまなプラットフォームについて説明します。 MacOS、iOS、tvOS、watchOS について説明します。
ms.prod: xamarin
ms.assetid: 43253BE4-A03A-4646-9A14-32C05174E672
author: conceptdev
ms.author: crdun
ms.date: 11/14/2017
ms.openlocfilehash: f97b595f129cb1ad1ea56e3ae43b0f0a477fef5a
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70282724"
---
# <a name="objective-c-platforms"></a>目的 C プラットフォーム

.NET 埋め込みは、目標 C コードを生成するときにさまざまなプラットフォームを対象にすることができます。

* macOS
* iOS
* tvOS
* watchOS [まだ実装されていません]

プラットフォームは、 `--platform=<platform>`コマンドライン引数を .net 埋め込みに渡すことによって選択されます。

IOS、tvOS、および watchOS プラットフォーム用にビルドする場合、.NET 埋め込みは、xamarin. iOS を埋め込むフレームワークを常に作成します。これは、Xamarin には、これらのプラットフォームで必要なランタイムサポートコードが多数含まれているためです。

ただし、macOS プラットフォーム用にビルドする場合は、生成されたフレームワークに Xamarin. Mac を埋め込むかどうかを選択できます。 バインドされたアセンブリが (直接または間接的に) xamarin. .dll を参照しない場合は、xamarin. mac を埋め込むことはできませ`--platform=macOS`ん。これは、.net 埋め込みツールに渡すことで選択します。

バインドされたアセンブリに embeddinator への参照が含まれている場合は、Xamarin. Mac を埋め込む必要があります。また、必要に応じて、使用するターゲットフレームワークを認識する必要があります。

使用可能な xamarin. `modern` mac ターゲットフレームワークは3つあります。 (以前はと呼ば`mobile`れていました)、 `full`および`system` (それぞれの違いは Xamarin. mac の[ターゲットフレームワーク][1]ドキュメントで説明されています)は、 `--platform=macOS-modern` `--platform=macOS-full`または`--platform=macOS-system` .net 埋め込みツールに渡すことによって選択されます。

[1]: ~/mac/platform/target-framework.md
