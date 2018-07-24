---
title: IOS ライブラリのバインド
description: このドキュメントでは、c# へのバインド Objective C コードでは、Xamarin.iOS アプリケーションでネイティブ ライブラリと CocoaPods を使用することを作成する方法について説明します。
ms.prod: xamarin
ms.assetid: EBDC50DC-B44B-4003-AB2B-1EEB868A5E01
ms.technology: xamarin-ios
ms.custom: xamu-video
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 75c8f9a2eea080c3da031366b314d94929080811
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855222"
---
# <a name="binding-ios-libraries"></a>IOS ライブラリのバインド

Objective C ライブラリと CocoaPods を Xamarin.iOS および Xamarin.Mac のバインドの詳細については、これらのリンクに従います。

- [**概要**](~/cross-platform/macios/binding/overview.md) -
  バインドのしくみについて説明します。
- [**OBJECTIVE-C ライブラリのバインド**](~/cross-platform/macios/binding/objective-c-libraries.md) -
  Xamarin プロジェクトで使用するための Objective C ライブラリにバインドする方法について。
- [**入力定義リファレンス ガイド**](~/cross-platform/macios/binding/binding-types-reference.md) -
  のすべてのバインドの生成プロセスを促進するバインディングの作成者に使用可能な属性について説明します。

## <a name="objective-sharpiecross-platformmaciosbindingobjective-sharpieindexmd"></a>[Objective Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md)

油性の目標は、バインディングの最初のパスをブートス トラップするコマンド ライン ツールです。
パブリック API をマップするネイティブ ライブラリのヘッダー ファイルを解析することによって動作、[バインディング定義](~/cross-platform/macios/binding/objective-c-libraries.md)(手動で行うそれ以外の場合は、プロセス)。 目標油性が単独でバインディングを作成できませんが、作業を開始するに役立つことができます。

目標油性 3.0 には、Cocoapods を直接バインドする機能が導入されました。

## <a name="walkthrough---binding-an-ios-objective-c-librarywalkthroughmd"></a>[チュートリアル - iOS OBJECTIVE-C ライブラリのバインド](walkthrough.md)

このページは、オープン ソースを使用して iOS バインド プロジェクトの作成のステップ バイ ステップ チュートリアルを提供します。 [ **InfColorPicker** ](https://github.com/InfinitApps/InfColorPicker)例として Objective C のプロジェクト。 **InfColorPicker**ライブラリには、ユーザーよりユーザー フレンドリな色の選択を行う、HSB 形式に基づいて色を選択できる再利用可能なビュー コント ローラーが用意されています。
油性の目標は、バインディング プロセスを支援するために使用されます。

## <a name="xamarin-university-lightning-lecture"></a>Xamarin University Lightning Lecture

> [!VIDEO https://youtube.com/embed/ZUoPLcmnf1o]

**iOS C および C++ でのバインドによって[Xamarin University](https://university.xamarin.com/)**

## <a name="related-links"></a>関連リンク

- [Objective-C のバインド](~/cross-platform/macios/binding/index.md)
- [Mac のバインド](~/mac/platform/binding.md)
- [Xamarin University のコース: OBJECTIVE-C のバインド ライブラリをビルドします。](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University のコース: 目標油性、OBJECTIVE-C のバインド ライブラリをビルドします。](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)