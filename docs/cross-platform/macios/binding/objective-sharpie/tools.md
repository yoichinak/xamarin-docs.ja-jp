---
title: 目標マジックペンツール & コマンド
description: このドキュメントでは、マジックペンに含まれるツールの概要と、それらで使用するコマンドライン引数について説明します。
ms.prod: xamarin
ms.assetid: A84E209B-8932-4CC1-BAD1-7FD51F798A97
author: davidortinau
ms.author: daortin
ms.date: 10/05/2015
ms.openlocfilehash: 2179154aa6dc78a8b0b6b418d780e7996f8e557d
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73015921"
---
# <a name="objective-sharpie-tools--commands"></a>目標マジックペンツール & コマンド

_目標マジックペンに含まれるツールの概要と、それらを使用するためのコマンドライン引数。_

目標マジックペンが正常に[インストールさ](~/cross-platform/macios/binding/objective-sharpie/get-started.md)れたら、ターミナルを開き、マジックペンが提供する必要がある*コマンド*の目標を理解します。

```
$ sharpie -help
usage: sharpie [OPTIONS] TOOL [TOOL_OPTIONS]

Options:
  -h, -help                Show detailed help
  -v, -version             Show version information

Telemetry Options:
  -tlm-about               Show a detailed overview of what usage and binding
                             information will be submitted to Xamarin by
                             default when using Objective Sharpie.
  -tlm-do-not-submit       Do not submit any usage or binding information to
                             Xamarin. Run 'sharpie -tml-about' for more
                             information.
  -tlm-do-not-identify     Do not submit Xamarin account information when
                             submitting usage or binding information to Xamarin
                             for analysis. Binding attempts and usage data will
                             be submitted anonymously if this option is
                             specified.

Available Tools:
  xcode              Get information about Xcode installations and available SDKs.
  pod                Create a Xamarin C# binding to Objective-C CocoaPods
  bind               Create a Xamarin C# binding to Objective-C APIs
  update             Update to the latest release of Objective Sharpie
  verify-docs        Show cross reference documentation for [Verify] attributes
  docs               Open the Objective Sharpie online documentation
```

目標マジックペンには、次のツールが用意されています。

|ツール|説明|
|--- |--- |
|**xcode**|現在の Xcode のインストールと、使用可能な iOS および Mac Sdk のバージョンに関する情報を提供します。 この情報は、後でバインドを生成するときに使用します。|
|**ポッド**|(ローカルディレクトリ内の) を検索し、構成し、インストールします。また、マスター仕様リポジトリから使用できる目的の C [Cocoa](https://cocoapods.org/)ライブラリをバインドします。 このツールは、インストールされているを評価して、次の `bind` ツールに渡す正しい入力を自動的に推測します。 3\.0 の新。|
|**bind**|ApiDefinition.cs ライブラリ内のヘッダーファイル (`*.h`) を、初期の[ファイルと StructsAndEnums.cs](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md)ファイルに解析します。|
|**update**|新しいバージョンの目標マジックペンを確認し、インストーラーが使用可能な場合はダウンロードして起動します。|
|**verify-docs**|`[Verify]` の属性に関する詳細情報を表示します。|
|**docs**|既定の web ブラウザーでこのドキュメントに移動します。|

特定の目標マジックペンツールに関するヘルプを表示するには、ツールの名前と `-help` オプションを入力します。 たとえば、`sharpie xcode -help` は次の出力を返します。

```
$ sharpie xcode -help
usage: sharpie xcode [OPTIONS]

Options:
  -h, -help        Show detailed help
  -v, -verbose     Be verbose with output

Xcode Options:
  -sdks            List all available Xcode SDKs. Pass -verbose for more details.
```

バインドプロセスを開始する前に、次のコマンドをターミナル `sharpie xcode -sdks`に入力して、現在インストールされている Sdk に関する情報を取得する必要があります。 インストールされている Xcode のバージョンによっては、出力が異なる場合があります。 目標マジックペンは、`/Applications` ディレクトリの `Xcode*.app` にインストールされている Sdk を検索します。

```
$ sharpie xcode -sdks
sdk: appletvos9.0    arch: arm64
sdk: iphoneos9.1     arch: arm64   armv7
sdk: iphoneos9.0     arch: arm64   armv7
sdk: iphoneos8.4     arch: arm64   armv7
sdk: macosx10.11     arch: x86_64  i386
sdk: macosx10.10     arch: x86_64  i386
sdk: watchos2.0      arch: armv7
```

上記の手順では、`iphoneos9.1` SDK がコンピューターにインストールされており、`arm64` アーキテクチャのサポートがあることがわかります。 このセクションのすべてのサンプルに対して、この値を使用します。 この情報が記載されたので、目的の C ライブラリのヘッダーファイルを、バインドプロジェクトの初期 `ApiDefinition.cs` と `StructsAndEnums.cs` に解析する準備ができました。
