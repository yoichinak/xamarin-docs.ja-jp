---
title: 目標油性ツールとコマンド
description: このドキュメントでは、それらで使用するには、目的の油性とコマンドライン引数に含まれているツールの概要を示します。
ms.prod: xamarin
ms.assetid: A84E209B-8932-4CC1-BAD1-7FD51F798A97
author: asb3993
ms.author: amburns
ms.date: 10/05/2015
ms.openlocfilehash: 718b5104ddc4593d080b88b062c42d371d9e8e2e
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61261169"
---
# <a name="objective-sharpie-tools--commands"></a>目標油性ツールとコマンド

_それらを使用するには、目的の油性とコマンドライン引数に含まれるツールの概要です。_

<style type="text/css"> .terminal 青 {色: rgb(10,96,254);} .terminal 緑 {color: rgb(12,156,26);} .terminal マゼンタ {色: rgb(152,12,103);} </style>


目標油性が正常に[インストール](~/cross-platform/macios/binding/objective-sharpie/get-started.md)ターミナルを開き、理解して、<em>コマンド</em>目標油性が提供する必要があります。

<pre>$ <b>sharpie -help</b>
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
  docs               Open the Objective Sharpie online documentation</pre>

目標油性には、次のツールが用意されています。

|ツール|説明|
|--- |--- |
|**xcode**|現在の Xcode インストールと iOS デバイスと使用可能な Mac Sdk のバージョンに関する情報を提供します。 使用するこの情報は後で、バインドが生成されたとき。|
|**pod**|検索、構成、(ローカル ディレクトリ) にインストールすると、および OBJECTIVE-C のバインド[CocoaPod](https://cocoapods.org/)マスター Spec リポジトリから使用可能なライブラリ。 このツールで評価を渡すための適切な入力を自動的に推測にインストールされている CocoaPod、`bind`以下のツール。 3.0 の新機能です。|
|**bind**|ヘッダー ファイルの解析 (`*.h`)、最初に、OBJECTIVE-C ライブラリ[ApiDefinition.cs と StructsAndEnums.cs](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md)ファイル。|
|**update**|目標油性の新しいバージョンをチェックし、ダウンロードがある場合、インストーラーを起動します。|
|**verify-docs**|に関する詳細情報が表示されます`[Verify]`属性。|
|**docs**|既定の web ブラウザーでは、このドキュメントに移動します。|

特定の目標油性ツールのヘルプを表示するには、ツールの名前を入力し、`-help`オプション。 たとえば、`sharpie xcode -help`次の出力を返します。

<pre>$ <b>sharpie xcode -help</b>
usage: sharpie xcode [OPTIONS]

Options:
  -h, -help        Show detailed help
  -v, -verbose     Be verbose with output

Xcode Options:
  -sdks            List all available Xcode SDKs. Pass -verbose for more details.</pre>

バインディング プロセスを始めることができます、前に、ターミナルに次のコマンドを入力して、現在インストールされている各種 Sdk についての情報を取得する必要があります`sharpie xcode -sdks`します。 出力は、インストールされている Xcode のバージョンによって異なる場合があります。 いずれかにインストールされている Sdk の次の目標油性`Xcode*.app`下、`/Applications`ディレクトリ。

<pre>$ <b>sharpie xcode -sdks</b>
<span class="terminal-blue">sdk:</span> appletvos9.0    <span class="terminal-green">arch:</span> arm64
<span class="terminal-blue">sdk:</span> iphoneos9.1     <span class="terminal-green">arch:</span> arm64   armv7
<span class="terminal-blue">sdk:</span> iphoneos9.0     <span class="terminal-green">arch:</span> arm64   armv7
<span class="terminal-blue">sdk:</span> iphoneos8.4     <span class="terminal-green">arch:</span> arm64   armv7
<span class="terminal-blue">sdk:</span> macosx10.11     <span class="terminal-green">arch:</span> x86_64  i386
<span class="terminal-blue">sdk:</span> macosx10.10     <span class="terminal-green">arch:</span> x86_64  i386
<span class="terminal-blue">sdk:</span> watchos2.0      <span class="terminal-green">arch:</span> armv7</pre>

上記からわかりますがあること、 `iphoneos9.1` SDK は、コンピューターにインストールされているしが`arm64`アーキテクチャ サポート。 このセクションでは、すべてのサンプルについてはこの値が使用されます。 この情報が、最初に Objective C ライブラリ ヘッダー ファイルを解析する準備ができて`ApiDefinition.cs`と`StructsAndEnums.cs`バインド プロジェクト。

## <a name="related-links"></a>関連リンク

- [Xamarin University のコース:OBJECTIVE-C バインディング ライブラリをビルド](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University のコース:目標油性で、OBJECTIVE-C のバインド ライブラリをビルドします。](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)