---
title: IOS ライブラリのバインド
description: このドキュメントを作成する方法を説明しますC#Xamarin.iOS アプリケーションでネイティブ ライブラリと CocoaPods を使用すること、OBJECTIVE-C コードへのバインド。
ms.prod: xamarin
ms.assetid: EBDC50DC-B44B-4003-AB2B-1EEB868A5E01
ms.technology: xamarin-ios
ms.custom: xamu-video
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: e599d4f99877e24e06de2c26ed2cafe48526f6f5
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50122999"
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