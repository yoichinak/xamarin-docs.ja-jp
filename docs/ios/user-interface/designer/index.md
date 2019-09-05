---
title: IOS Designer を使用したユーザーインターフェイスの構築
description: このドキュメントでは、Xamarin Designer for iOS を使用して、ストーリーボードと xib ファイルを含むアプリのユーザーインターフェイスを構築する方法について説明します。 ツールの可用性、その基本的な機能、デザイン可能なコントロールについて説明するドキュメントにリンクし、その使用方法についてのチュートリアルを提供します。
ms.prod: xamarin
ms.assetid: E35EFB69-EBBA-40E3-ADBE-CB8016F17127
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 05/31/2018
ms.openlocfilehash: 577c5602c1cbc331564c3034b3f0c11a4b97bc0c
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70279843"
---
# <a name="building-user-interfaces-with-the-ios-designer"></a>IOS Designer を使用したユーザーインターフェイスの構築

_Xamarin Designer for iOS は、Visual Studio for Mac および Visual Studio と完全に統合された iOS ストーリーボードと Interface Builder 形式のビジュアルデザイナーです。IOS デザイナーは、ストーリーボードと xib 形式との完全な互換性を維持し、Xcode の Interface Builder に加えて、Visual Studio for Mac または Visual Studio でファイルを編集できるようにします。さらに、Xamarin Designer for iOS は、エディターでデザイン時にレンダリングするカスタムコントロールなどの高度な機能をサポートしています。_

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![Visual Studio for Mac の IOS デザイナー](images/designer-vsmac-sml.png "IOS デザイナー")](images/designer-vsmac.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Visual Studio の IOS デザイナー](images/designer-vs.png "IOS デザイナー")](images/designer-vs.png#lightbox)

-----

## <a name="availability"></a>対象

Xamarin Designer for iOS は、Windows の Visual Studio for Mac と Visual Studio 2017 で使用できます。

これらのガイドでは、 [Xamarin の iOS はじめにガイド](~/ios/get-started/index.md)で説明されている内容を理解していることを前提としています。

## <a name="ios-designer-basicsintroductionmd"></a>[iOS Designer の基本](introduction.md)

このガイドでは、Xamarin iOS designer の機能について説明します。 デザイナーの基本について説明し、デザイナーを使用してコントロールを視覚的にレイアウトする方法と、プロパティを編集する方法を示します。

## <a name="designable-controls-overviewios-designable-controls-overviewmd"></a>[デザイン可能コントロールの概要](ios-designable-controls-overview.md)

このガイドでは、カスタムコントロールの詳細、作成方法、デザインサーフェイスにレンダリングするために満たす必要がある要件について詳しく説明します。 また、デザイン可能なコントロールを使用するときに発生する可能性のある一般的な問題をデバッグする方法も示します。

## <a name="walkthrough---using-custom-controls-with-ios-designerios-designable-controls-walkthroughmd"></a>[チュートリアル-iOS Designer でのカスタムコントロールの使用](ios-designable-controls-walkthrough.md)

この記事では、カスタムコントロールを作成し、iOS デザイナーで使用する方法を示す詳細なチュートリアルを提供します。 デザイナーのツールボックスでコントロールを使用できるようにして、ビューにドラッグアンドドロップできるようにする方法を示します。 また、デザイン時および実行時に適切にレンダリングされるようにコントロールを実装する方法と、デザイン時に設定できるプロパティを作成する方法についても説明します。

## <a name="auto-layout-with-the-xamarin-ios-designerdesigner-auto-layoutmd"></a>[Xamarin iOS Designer を使用した自動レイアウト](designer-auto-layout.md)

このガイドでは、ios の自動レイアウトと、iOS デザイナーで使用できる新しい制約のワークフローについて説明します。
