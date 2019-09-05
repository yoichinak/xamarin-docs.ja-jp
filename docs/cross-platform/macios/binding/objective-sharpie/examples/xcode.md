---
title: Xcode プロジェクトを使用した実際の例
description: このドキュメントでは、Xcode プロジェクトを目的のマジックペンへの直接入力として使用し、目的C#の C コードへのバインド作成プロセスを簡略化する方法について説明します。
ms.prod: xamarin
ms.assetid: 168AA64C-E181-4937-A1F2-AD095B9A36F2
author: conceptdev
ms.author: crdun
ms.date: 01/15/2016
ms.openlocfilehash: e460994994c1383f29028be7b13cec216f2d12f7
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290649"
---
# <a name="real-world-example-using-an-xcode-project"></a>Xcode プロジェクトを使用した実際の例

**この例では、 [Facebook の POP ライブラリ](https://github.com/facebook/pop)を使用します。**

バージョン3.0 の新機能である目標マジックペンは、入力として Xcode プロジェクトをサポートしています。 これらのプロジェクトでは、ネイティブライブラリをコンパイルするために必要な正しいヘッダーファイルとコンパイラフラグが指定されているため、それをバインドする必要があります。 目標マジックペンは、プロジェクトの最初の_ターゲット_と既定の構成を選択します (それ以外の場合)。

目標マジックペンは、プロジェクトファイルとヘッダーファイルを解析しようとする前にビルドする必要があります。 多くの場合、プロジェクトには、外部の消費と統合のためにヘッダーファイルを正しく構造するビルドフェーズがあります。そのため、プロジェクトをバインドする前に、常に完全なプロジェクトをビルドすることをお勧めします。

```
$ git clone https://github.com/facebook/pop.git
Cloning into 'pop'...
   (more git clone output)

$ cd pop
$ sharpie bind pop.xcodeproj -sdk iphoneos9.0
```
