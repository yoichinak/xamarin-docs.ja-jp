---
title: "Xcode プロジェクトを使用して実際の例"
ms.topic: article
ms.prod: xamarin
ms.assetid: 168AA64C-E181-4937-A1F2-AD095B9A36F2
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 01/15/2016
ms.openlocfilehash: 34efeb2b505f6076623f22f36c2a48a52d6f399f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
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

