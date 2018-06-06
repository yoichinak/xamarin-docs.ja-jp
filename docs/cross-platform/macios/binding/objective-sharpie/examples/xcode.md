---
title: Xcode プロジェクトを使用して実際の例
description: このドキュメントでは、Xcode プロジェクトを c# Objective C コードへのバインドを作成するプロセスを簡略化、目標ペンを使わずに直接入力として使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 168AA64C-E181-4937-A1F2-AD095B9A36F2
author: asb3993
ms.author: amburns
ms.date: 01/15/2016
ms.openlocfilehash: 850c351f91c9a09a6654c876167e035f751e9504
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781118"
---
# <a name="real-world-example-using-an-xcode-project"></a>Xcode プロジェクトを使用して実際の例


**この例では、[の Facebook からの POP ライブラリ](https://github.com/facebook/pop)です。**

新しいバージョン 3.0 に目標ペンを使わず Xcode プロジェクトとしてサポート入力します。 これらのプロジェクトは、適切なヘッダー ファイルと、ネイティブ ライブラリをコンパイルするために必要と非常にバインドするために必要なコンパイラ フラグを指定します。 目標ペンを使わずに、最初は選択_ターゲット_し、それ以外の場合に実行するように指示されていない場合、プロジェクトの既定の構成。

目標ペンを使わずが、プロジェクト ファイルとヘッダー ファイルを解析しようとすると、前はそれをビルドする必要があります。 プロジェクトには、多くの場合、ため、常にバインドする前に、完全なプロジェクトをビルドすることをお勧め外部の消費量との統合、ヘッダー ファイルが正しく構築したビルド フェーズがあります。

<pre>$ <b>git clone https://github.com/facebook/pop.git</b>
Cloning into 'pop'...
   <em>(more git clone output)</em>

$ <b>cd pop</b>
$ <b>sharpie bind pop.xcodeproj -sdk iphoneos9.0</b></pre>

