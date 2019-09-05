---
title: Cocoアポストロフィ Ds を使用した実際の例
description: このドキュメントでは、目標マジックペンを使用してC# 、Cocoアポストロフィ d からバインド定義を自動的に生成する方法を示します。
ms.prod: xamarin
ms.assetid: 233B781D-5841-4250-9F63-0585231D2112
author: conceptdev
ms.author: crdun
ms.date: 03/28/2018
ms.openlocfilehash: 0f730b1c0a0deacdb84c198cfe4af47308a268cc
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290029"
---
# <a name="real-world-example-using-cocoapods"></a>Cocoアポストロフィ Ds を使用した実際の例

> [!NOTE]
> この例では、 [Afnetworking Cocoアポストロフィ d](https://cocoapods.org/pods/AFNetworking)を使用します。

バージョン3.0 の新機能である目標マジックペンは、cocoアポストロフィのバインドをサポートして`sharpie pod`います。また、コマンド () を使用して、開発、構成、および開発を非常に簡単に行うことができます。 この機能を使用する前に、一般的に[Cocoアポストロフィに](https://cocoapods.org)ついて理解しておく必要があります。

## <a name="creating-a-binding-for-a-cocoapod"></a>Cocoアポストロフィ d のバインドの作成

この`sharpie pod`コマンドには、1つのグローバルオプションと2つのサブコマンドがあります。

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

サブ`init`コマンドには、次のような役に立つヘルプもあります。

```bash
$ sharpie pod init -help
usage: sharpie pod init [INIT_OPTIONS] TARGET_SDK POD_SPEC_NAMES

Init Options:
  -f, -force       Initialize a new Podfile and run actions against
                   it even if one already exists
```

には、複数の Cocoアポストロフィ d 名とサブ仕様名`init`を指定できます。

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

Cocoアポストロフィがセットアップされたら、バインドを作成できるようになります。

```bash
$ sharpie pod bind
```

これにより、Cocoアポストロフィ d Xcode プロジェクトがビルドされ、目標マジックペンによって評価および解析されます。 多数のコンソール出力が生成されますが、最終的にバインド定義は次のようになります。

```bash
(... lots of build output ...)

Parsing 19 header files...

Binding...
  [write] ApiDefinitions.cs
  [write] StructsAndEnums.cs

Done.
```

## <a name="next-steps"></a>次の手順

**ApiDefinitions.cs**ファイルと**StructsAndEnums.cs**ファイルを生成したら、次のドキュメントを参照して、アプリで使用するアセンブリを生成します。

- [バインディングの目的-C の概要](~/cross-platform/macios/binding/overview.md)
- [バインディングの目的 C ライブラリ](~/cross-platform/macios/binding/objective-c-libraries.md)
- [チュートリアル: IOS の目的 C ライブラリのバインド](~/ios/platform/binding-objective-c/walkthrough.md)
