---
title: Xcode プロジェクトを使用して実際の例
description: このドキュメントでは、Xcode プロジェクトを c# OBJECTIVE-C コードへのバインドを作成するプロセスを簡略化の目的油性を直接入力として使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 168AA64C-E181-4937-A1F2-AD095B9A36F2
author: asb3993
ms.author: amburns
ms.date: 01/15/2016
ms.openlocfilehash: 05c55dc7cd20de2d216d1f267ea5a73631748a0a
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855248"
---
# <a name="real-world-example-using-an-xcode-project"></a>Xcode プロジェクトを使用して実際の例

**この例では、 [Facebook から POP ライブラリ](https://github.com/facebook/pop)します。**

新しいバージョン 3.0 では、目的の油性 Xcode プロジェクトとしてサポート入力します。 これらのプロジェクトでは、適切なヘッダー ファイルとコンパイラ フラグは、ネイティブのライブラリをコンパイルするために必要なおよびすぎるバインドするために必要なを指定します。 目標油性が最初に選択されます_ターゲット_し、それ以外の場合に実行するように指示されていない場合は、プロジェクトの既定の構成。

目標油性がプロジェクトとヘッダー ファイルを解析しようとすると、前に、ビルドする必要があります。 プロジェクトには、多くの場合、ため常にバインドする前に完全なプロジェクトをビルドすることをお勧め外部消費との統合、ヘッダー ファイルの構造が正しく構築フェーズがあります。

<pre>$ <b>git clone https://github.com/facebook/pop.git</b>
Cloning into 'pop'...
   <em>(more git clone output)</em>

$ <b>cd pop</b>
$ <b>sharpie bind pop.xcodeproj -sdk iphoneos9.0</b></pre>

## <a name="related-links"></a>関連リンク

- [Xamarin University のコース: OBJECTIVE-C のバインド ライブラリをビルドします。](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University のコース: 目標油性、OBJECTIVE-C のバインド ライブラリをビルドします。](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)