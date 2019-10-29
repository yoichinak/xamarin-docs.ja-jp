---
title: 目的 C プラットフォーム
description: このドキュメントでは、.NET 埋め込みが目標 C コードを操作するときにターゲットにできるさまざまなプラットフォームについて説明します。 MacOS、iOS、tvOS、watchOS について説明します。
ms.prod: xamarin
ms.assetid: 43253BE4-A03A-4646-9A14-32C05174E672
author: davidortinau
ms.author: daortin
ms.date: 11/14/2017
ms.openlocfilehash: dbc2c40e06f14ec636b4aa4cc11fc4f2d230ee6d
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73006421"
---
# <a name="objective-c-platforms"></a>目的 C プラットフォーム

.NET 埋め込みは、目標 C コードを生成するときにさまざまなプラットフォームを対象にすることができます。

* macOS
* iOS
* tvOS
* watchOS [まだ実装されていません]

プラットフォームは、`--platform=<platform>` コマンドライン引数を .NET 埋め込みに渡すことによって選択されます。

IOS、tvOS、および watchOS プラットフォーム用にビルドする場合、.NET 埋め込みは、xamarin. iOS を埋め込むフレームワークを常に作成します。これは、Xamarin には、これらのプラットフォームで必要なランタイムサポートコードが多数含まれているためです。

ただし、macOS プラットフォーム用にビルドする場合は、生成されたフレームワークに Xamarin. Mac を埋め込むかどうかを選択できます。 バインドされたアセンブリが (直接または間接的に) Xamarin を参照しない場合は、Xamarin. Mac を埋め込むことはできません。これは、`--platform=macOS` を .NET 埋め込みツールに渡すことによって選択します。

バインドされたアセンブリに embeddinator への参照が含まれている場合は、Xamarin. Mac を埋め込む必要があります。また、必要に応じて、使用するターゲットフレームワークを認識する必要があります。

使用可能な Xamarin. Mac ターゲットフレームワークには、`modern` (旧称 `mobile`)、`full`、および `system` (それぞれの違いは Xamarin. Mac の[ターゲットフレームワーク][1]ドキュメントで説明されています) と、それぞれを渡すことによって選択されます.NET 埋め込みツールを `--platform=macOS-modern`、`--platform=macOS-full` または `--platform=macOS-system` します。

[1]: ~/mac/platform/target-framework.md
