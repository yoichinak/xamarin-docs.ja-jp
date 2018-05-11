---
title: CocoaPods を使用して実際の例
description: このドキュメントでは、目標ペンを使わずを使用して、CocoaPod から c# バインディングの定義を自動的に生成する方法を示します。
ms.prod: xamarin
ms.assetid: 233B781D-5841-4250-9F63-0585231D2112
author: asb3993
ms.author: amburns
ms.date: 03/28/2018
ms.openlocfilehash: 026b2c46f7c294d4ac4a110376131ec83c7c112e
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/10/2018
---
# <a name="real-world-example-using-cocoapods"></a>CocoaPods を使用して実際の例

> [!NOTE]
> この例では、 [AFNetworking CocoaPod](https://cocoapods.org/pods/AFNetworking)です。

バージョン 3.0 の新目標ペンを使わずをサポートします。 バインド CocoaPods、さらにコマンドが含まれます (`sharpie pod`) をダウンロード、構成、および CocoaPods 非常に簡単に構築します。 行う必要があります[CocoaPods に慣れ親しみ](https://cocoapods.org)一般にこの機能を使用する前にします。

## <a name="creating-a-binding-for-a-cocoapod"></a>A CocoaPod のバインドを作成します。

`sharpie pod`コマンドが 1 つのグローバル オプションと次の 2 つのサブコマンドには。

```bash
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

```bash
$ sharpie pod init -help
usage: sharpie pod init [INIT_OPTIONS] TARGET_SDK POD_SPEC_NAMES

Init Options:
  -f, -force       Initialize a new Podfile and run actions against
                   it even if one already exists
```

複数の CocoaPod 名および subspec 名に提供できる`init`です。

```bash
$ sharpie pod init ios AFNetworking
** Setting up CocoaPods master repo ...
   (this may take a while the first time)
** Searching for requested CocoaPods ...
** Working directory:
**   - Writing Podfile ...
**   - Installing CocoaPods ...
**     (running `pod install --no-integrate --no-repo-update`)
Analyzing dependencies
Downloading dependencies
Installing AFNetworking (2.6.0)
Generating Pods project
Sending stats
** 🍻 Success! You can now use other `sharpie podn`  commands.
```

いったん、CocoaPod がセットアップされるようになりましたバインディングを作成できます。

```bash
$ sharpie pod bind
```

これにより、CocoaPod Xcode プロジェクトされているビルドを評価し、目標ペンを使わずによって解析が発生します。 多数のコンソール出力は生成されますが、最後に、バインディングの定義が発生する必要があります。

```bash
(... lots of build output ...)

Parsing 19 header files...

Binding...
  [write] ApiDefinitions.cs
  [write] StructsAndEnums.cs

Done.
```

## <a name="next-steps"></a>次の手順

生成した後、 **ApiDefinitions.cs**と**StructsAndEnums.cs**ファイルを見て、次のドキュメントでは、アプリを使用するアセンブリを生成します。

- [バインディング Objective C の概要](~/cross-platform/macios/binding/overview.md)
- [Objective C ライブラリのバインド](~/cross-platform/macios/binding/objective-c-libraries.md)
- [チュートリアル: バインド iOS Objective C ライブラリ](~/ios/platform/binding-objective-c/walkthrough.md)

