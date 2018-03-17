---
title: "ツールとコマンド"
description: "それらを使用するには、目的のペンを使わずとコマンドライン引数に付属のツールの概要です。"
ms.topic: article
ms.prod: xamarin
ms.assetid: A84E209B-8932-4CC1-BAD1-7FD51F798A97
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 10/05/2015
ms.openlocfilehash: 4985d67f74d54644cbc033d6c76148f88d741ee1
ms.sourcegitcommit: 5fc1c4d17cd9c755604092cf7ff038a6358f8646
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/17/2018
---
# <a name="tools--commands"></a>ツールとコマンド

_それらを使用するには、目的のペンを使わずとコマンドライン引数に付属のツールの概要です。_

<style type="text/css"> .terminal-blue { color: rgb(10,96,254); } .terminal-green { color: rgb(12,156,26); } .terminal-magenta { color: rgb(152,12,103); } </style>


目標ペンを使わずが正常に[インストール](~/cross-platform/macios/binding/objective-sharpie/get-started.md)端末を開き、理解しておく、<em>コマンド</em>目標ペンを使わずが提供するには。

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

目標ペンを使わずには、次のツールが用意されています。

|ツール|説明|
|--- |--- |
|**xcode**|現在の Xcode インストールと iOS および Mac の Sdk を利用できますがのバージョンに関する情報を提供します。 使用するこの情報は後で、バインディングが生成されたとき。|
|**pod**|検索、構成、(ローカル ディレクトリ) にインストールすると、およびバインド Objective C [CocoaPod](https://cocoapods.org/)マスター Spec リポジトリから使用可能なライブラリです。 このツールに渡す正しい入力を自動的に推測にインストールされている CocoaPod の評価、`bind`以下のツールです。 3.0 の新機能!|
|**bind**|ヘッダー ファイルを解析して (`*.h`)、最初に Objective C ライブラリの[ApiDefinition.cs と StructsAndEnums.cs](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md)ファイル。|
|**update**|目標ペンを使わずの新しいバージョンを確認し、ダウンロードし、1 つが利用可能な場合は、インストーラーを起動します。|
|**verify-docs**|に関する詳細情報が表示`[Verify]`属性。|
|**docs**|既定の web ブラウザーでは、このドキュメントに移動します。|

特定の目標ペンを使わずツールのヘルプを表示するには、ツールの名前を入力し、`-help`オプション。 たとえば、`sharpie xcode -help`次の出力を返します。

<pre>$ <b>sharpie xcode -help</b>
usage: sharpie xcode [OPTIONS]

Options:
  -h, -help        Show detailed help
  -v, -verbose     Be verbose with output

Xcode Options:
  -sdks            List all available Xcode SDKs. Pass -verbose for more details.</pre>

バインディング プロセスを始めることができます、前に、ターミナルに次のコマンドを入力して、現在インストールされている Sdk に関する情報を取得する必要があります`sharpie xcode -sdks`です。 出力は、インストールした Xcode のバージョンによって異なる場合があります。 いずれかにインストールされている Sdk の次の目標ペンを使わず`Xcode*.app`下にある、`/Applications`ディレクトリ。

<pre>$ <b>sharpie xcode -sdks</b>
<span class="terminal-blue">sdk:</span> appletvos9.0    <span class="terminal-green">arch:</span> arm64
<span class="terminal-blue">sdk:</span> iphoneos9.1     <span class="terminal-green">arch:</span> arm64   armv7
<span class="terminal-blue">sdk:</span> iphoneos9.0     <span class="terminal-green">arch:</span> arm64   armv7
<span class="terminal-blue">sdk:</span> iphoneos8.4     <span class="terminal-green">arch:</span> arm64   armv7
<span class="terminal-blue">sdk:</span> macosx10.11     <span class="terminal-green">arch:</span> x86_64  i386
<span class="terminal-blue">sdk:</span> macosx10.10     <span class="terminal-green">arch:</span> x86_64  i386
<span class="terminal-blue">sdk:</span> watchos2.0      <span class="terminal-green">arch:</span> armv7</pre>

上記から、表示があること、 `iphoneos9.1` 、マシンの SDK がインストールされておりが`arm64`アーキテクチャのサポート。 私たちはこの値を使用このセクションのすべてのサンプルです。 配置でこの情報を使用する準備が整いました Objective C ライブラリ ヘッダー ファイルを最初に解析`ApiDefinition.cs`と`StructsAndEnums.cs`バインド プロジェクト。

