---
title: 目標マジックペンツール & コマンド
description: このドキュメントでは、マジックペンに含まれるツールの概要と、それらで使用するコマンドライン引数について説明します。
ms.prod: xamarin
ms.assetid: A84E209B-8932-4CC1-BAD1-7FD51F798A97
author: asb3993
ms.author: amburns
ms.date: 10/05/2015
ms.openlocfilehash: ddfe0f99991808214a6006c9504d267179adf2ab
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2019
ms.locfileid: "69521860"
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

|Tool|説明|
|--- |--- |
|**xcode**|現在の Xcode のインストールと、使用可能な iOS および Mac Sdk のバージョンに関する情報を提供します。 この情報は、後でバインドを生成するときに使用します。|
|**pod**|(ローカルディレクトリ内の) を検索し、構成し、インストールします。また、マスター仕様リポジトリから使用できる目的の C [Cocoa](https://cocoapods.org/)ライブラリをバインドします。 このツールは、インストールされているを評価して、次の`bind`ツールに渡す正しい入力を自動的に推測します。 3\.0 の新。|
|**bind**|目的の C ライブラリの`*.h`ヘッダーファイル () を解析し、初期の[ApiDefinition.cs ファイルと StructsAndEnums.cs](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md)ファイルに挿入します。|
|**update**|新しいバージョンの目標マジックペンを確認し、インストーラーが使用可能な場合はダウンロードして起動します。|
|**verify-docs**|属性に関する`[Verify]`詳細情報を表示します。|
|**docs**|既定の web ブラウザーでこのドキュメントに移動します。|

特定の目的のマジックペンツールに関するヘルプを表示するには、ツールの名前`-help`とオプションを入力します。 たとえば、は`sharpie xcode -help`次の出力を返します。

```
$ sharpie xcode -help
usage: sharpie xcode [OPTIONS]

Options:
  -h, -help        Show detailed help
  -v, -verbose     Be verbose with output

Xcode Options:
  -sdks            List all available Xcode SDKs. Pass -verbose for more details.
```

バインドプロセスを開始する前に、次のコマンドをターミナル`sharpie xcode -sdks`に入力して、現在インストールされている sdk に関する情報を取得する必要があります。 インストールされている Xcode のバージョンによっては、出力が異なる場合があります。 目標マジックペンは、次のディレクトリに`Xcode*.app`インストールさ`/Applications`れている sdk を検索します。

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

この記事では、 `iphoneos9.1` SDK がコンピューターにインストールされており、アーキテクチャが`arm64`サポートされていることがわかります。 このセクションのすべてのサンプルに対して、この値を使用します。 この情報が記載されたので、目的の C ライブラリヘッダーファイルを最初`ApiDefinition.cs`のおよび`StructsAndEnums.cs`バインドプロジェクトに解析する準備ができました。
