---
title: CocoaPods を使用して実際の例
description: このドキュメントが自動的に生成する目的油性を使用する方法を示します、 C# 、CocoaPod から定義をバインドします。
ms.prod: xamarin
ms.assetid: 233B781D-5841-4250-9F63-0585231D2112
author: asb3993
ms.author: amburns
ms.date: 03/28/2018
ms.openlocfilehash: bac34f662e24c6b08a67cd8da1f41b37b43b3faf
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61200316"
---
# <a name="real-world-example-using-cocoapods"></a>CocoaPods を使用して実際の例

> [!NOTE]
> この例では、 [AFNetworking CocoaPod](https://cocoapods.org/pods/AFNetworking)します。

バージョン 3.0 の新機能目標油性が、CocoaPods をバインドをサポートし、さらにコマンドが含まれます (`sharpie pod`) をダウンロードし、構成、および、CocoaPods を非常に簡単に構築します。 必要があります[CocoaPods 慣れる](https://cocoapods.org)一般にこの機能を使用する前にします。

## <a name="creating-a-binding-for-a-cocoapod"></a>CocoaPod のバインディングの作成

`sharpie pod`コマンドは、1 つのグローバル オプションと 2 つのサブコマンドには。

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

`init`サブコマンドがいくつかの便利なヘルプにあります。

```bash
$ sharpie pod init -help
usage: sharpie pod init [INIT_OPTIONS] TARGET_SDK POD_SPEC_NAMES

Init Options:
  -f, -force       Initialize a new Podfile and run actions against
                   it even if one already exists
```

複数の CocoaPod 名および subspec 名に提供できる`init`します。

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

CocoaPod は設定されていると、バインドを作成できます。

```bash
$ sharpie pod bind
```

これにより、ビルドしし、評価および目標油性によって解析 CocoaPod Xcode プロジェクトが発生します。 多くのコンソール出力は生成されますが、最後に、バインド定義の結果します。

```bash
(... lots of build output ...)

Parsing 19 header files...

Binding...
  [write] ApiDefinitions.cs
  [write] StructsAndEnums.cs

Done.
```

## <a name="next-steps"></a>次の手順

生成した後、 **ApiDefinitions.cs**と**StructsAndEnums.cs**ファイルをアプリで使用するアセンブリを生成する、次のドキュメントを参照してください。

- [OBJECTIVE-C のバインディングの概要](~/cross-platform/macios/binding/overview.md)
- [OBJECTIVE-C ライブラリのバインド](~/cross-platform/macios/binding/objective-c-libraries.md)
- [チュートリアル: IOS の Objective C ライブラリのバインド](~/ios/platform/binding-objective-c/walkthrough.md)
- [Xamarin University のコース:OBJECTIVE-C バインディング ライブラリをビルド](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University のコース:目標油性で、OBJECTIVE-C のバインド ライブラリをビルドします。](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)
