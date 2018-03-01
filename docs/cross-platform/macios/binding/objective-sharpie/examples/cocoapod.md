---
title: "CocoaPods を使用して実際の例"
ms.topic: article
ms.prod: xamarin
ms.assetid: 233B781D-5841-4250-9F63-0585231D2112
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: ae92b491e6186371f1fc1ead835f918a94f18f86
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="real-world-example-using-cocoapods"></a>CocoaPods を使用して実際の例


**この例では、 [AFNetworking CocoaPod](https://cocoapods.org/pods/AFNetworking)です。**

新しいバージョンでは 3.0、目標ペンを使わず CocoaPods、バインドをサポートしているを使用してフロント エンド コマンド (`sharpie pod`) をダウンロード、構成、および CocoaPods 非常に簡単に構築します。 行う必要があります[faimilarize CocoaPods を自分で](https://cocoapods.org)一般にこの機能を使用する前にします。

`sharpie pod`コマンドが 1 つのグローバル オプションと次の 2 つのサブコマンドには。

```csharp
$ sharpie pod -help
usage: sharpie pod [OPTIONS] COMMAND [COMMAND_OPTIONS]

Pod Options:
  -d, -dir DIR     Use DIR as the CocoaPods binding directory,
                   defaulting to the current directory

Available Commands:
  init         Initialize a new Xamarin C# CocoaPods binding project
  bind         Bind an existing Xamarin C# CocoaPods project
```

`init`サブコマンドがあります助けに便利です。

```csharp
$ sharpie pod init -help
usage: sharpie pod init [INIT_OPTIONS] TARGET_SDK POD_SPEC_NAMES

Init Options:
  -f, -force       Initialize a new Podfile and run actions against
                   it even if one already exists
```

複数の CocoaPod 名および subspec 名に提供できる`init`です。

<pre>$ <b>sharpie pod init ios AFNetworking</b>
<span class="terminal-green">**</span> Setting up CocoaPods master repo ...
   (this may take a while the first time)
<span class="terminal-green">**</span> Searching for requested CocoaPods ...
<span class="terminal-green">**</span> Working directory:
<span class="terminal-green">**</span>   - Writing Podfile ...
<span class="terminal-green">**</span>   - Installing CocoaPods ...
<span class="terminal-green">**</span>     (running `<span class="terminal-blue">pod install --no-integrate --no-repo-update</span>`)
Analyzing dependencies
Downloading dependencies
Installing AFNetworking (2.6.0)
Generating Pods project
Sending stats
<span class="terminal-green">**</span> 🍻 Success! You can now use other `<span class="terminal-green">sharpie pod</span>`  commands.</pre>

いったん、CocoaPod がセットアップされるようになりましたバインディングを作成できます。

<pre>$ <b>sharpie pod bind</b></pre>

これにより、CocoaPod Xcode プロジェクトされているビルドを評価し、目標ペンを使わずによって解析が発生します。 多数のコンソール出力は生成されますが、最後に、バインディングの定義が発生する必要があります。

<pre><em>(... lots of build output ...)</em>

<span class="terminal-blue">Parsing 19 header files...</span>

<span class="terminal-magenta">Binding...</span>
  <span class="terminal-magenta">[write]</span> ApiDefinitions.cs
  <span class="terminal-magenta">[write]</span> StructsAndEnums.cs

<span class="terminal-green">Done.</span></pre>

