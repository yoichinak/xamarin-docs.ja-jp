---
title: "ライブラリの iOS のバインド"
description: "IOS のネイティブ ライブラリ (および CocoaPods) を作成する方法の Xamarin アプリにアクセスします。"
ms.topic: article
ms.prod: xamarin
ms.assetid: DBBAA086-BB0F-8161-DF44-632F4F5DFE5D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 3afe1a03299e600502d49b1db039af4c6642e131
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="binding-ios-libraries"></a>ライブラリの iOS のバインド

_IOS のネイティブ ライブラリ (および CocoaPods) を作成する方法の Xamarin アプリにアクセスします。_

これらのリンクについては、Xamarin.iOS および Xamarin.Mac のバインディングを Objective C のライブラリおよび CocoaPods に従います。

- [**概要**](~/cross-platform/macios/binding/overview.md) -
  バインディングのしくみについて説明します。
- [**Objective C のライブラリをバインド**](~/cross-platform/macios/binding/objective-c-libraries.md) -
  Objective C ライブラリ Xamarin プロジェクトで使用するためにバインドする方法についてはします。
- [**型定義リファレンス ガイド**](~/cross-platform/macios/binding/binding-types-reference.md) -
  すべてのバインドの生成処理をドライブにするバインディングの作成者に使用できる属性について説明します。

## <a name="objective-sharpiecross-platformmaciosbindingobjective-sharpieindexmd"></a>[Objective Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md)

目標ペンを使わずは、ブートス トラップ バインディングの最初のパスを支援するコマンド ライン ツールです。
パブリック API をマップするネイティブ ライブラリのヘッダー ファイルを解析して、それと、[バインディング定義](~/cross-platform/macios/binding/objective-c-libraries.md)(それ以外の場合は手動でプロセス)。 目標ペンを使わずが単独で、バインドを作成できませんが、開始するためにことができます。

目標のペンを使わず 3.0 には、Cocoapods を直接バインドする機能が導入されました!

## <a name="walkthrough---binding-an-ios-objective-c-librarywalkthroughmd"></a>[チュートリアル - iOS Objective C ライブラリのバインド](walkthrough.md)

このページでは、オープン ソースを使用して iOS バインド プロジェクトの作成の詳細なチュートリアル[ **InfColorPicker** ](https://github.com/InfinitApps/InfColorPicker)例として Objective C のプロジェクトです。 **InfColorPicker**ライブラリには、ユーザーが、HSB 表現をよりわかりやすい色の選択を行ったに基づいて色を選択できる再利用可能なビュー コント ローラーが用意されています。
目標ペンを使わずは、バインディング プロセスを支援するために使用されます。



## <a name="related-links"></a>関連リンク

- [Objective-C のバインド](~/cross-platform/macios/binding/index.md)
- [Mac のバインド](~/mac/platform/binding.md)
