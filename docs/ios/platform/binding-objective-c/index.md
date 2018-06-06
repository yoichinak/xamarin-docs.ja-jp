---
title: ライブラリの iOS のバインド
description: このドキュメントでは、バインディングを作成する C# の場合、OBJECTIVE-C コードに Xamarin.iOS アプリケーションでネイティブ ライブラリと CocoaPods を利用できるようにする方法について説明します。
ms.prod: xamarin
ms.assetid: EBDC50DC-B44B-4003-AB2B-1EEB868A5E01
ms.technology: xamarin-ios
ms.custom: xamu-video
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: b054595568a34616a01f2c3f3c7d85f968c3f1fa
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787170"
---
# <a name="binding-ios-libraries"></a>ライブラリの iOS のバインド

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

## <a name="xamarin-university-lightning-lecture"></a>Xamarin 大学稲妻講義

> [!VIDEO https://youtube.com/embed/ZUoPLcmnf1o]

**iOS C と C++ でのバインドによって[Xamarin 大学](https://university.xamarin.com/)**

## <a name="related-links"></a>関連リンク

- [Objective-C のバインド](~/cross-platform/macios/binding/index.md)
- [Mac のバインド](~/mac/platform/binding.md)
