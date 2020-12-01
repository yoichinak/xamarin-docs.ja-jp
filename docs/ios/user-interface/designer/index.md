---
title: IOS Designer を使用したユーザーインターフェイスの構築
description: このドキュメントでは、Xamarin Designer for iOS を使用して、ストーリーボードと xib ファイルを含むアプリのユーザーインターフェイスを構築する方法について説明します。 ツールの可用性、その基本的な機能、デザイン可能なコントロールについて説明するドキュメントにリンクし、その使用方法についてのチュートリアルを提供します。
ms.prod: xamarin
ms.assetid: E35EFB69-EBBA-40E3-ADBE-CB8016F17127
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 05/31/2018
ms.openlocfilehash: 07226b22243f3d463ce2630e1f12a94f83ddd64a
ms.sourcegitcommit: d1f0e0a9100548cfe0960ed2225b979cc1d7c28f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/01/2020
ms.locfileid: "96439434"
---
# <a name="building-user-interfaces-with-the-ios-designer"></a>IOS Designer を使用したユーザーインターフェイスの構築

_Xamarin Designer for iOS は、Visual Studio for Mac および Visual Studio と完全に統合された iOS ストーリーボードと Interface Builder 形式のビジュアルデザイナーです。IOS デザイナーは、ストーリーボードと xib 形式との完全な互換性を維持し、Xcode の Interface Builder に加えて、Visual Studio for Mac または Visual Studio でファイルを編集できるようにします。さらに、Xamarin Designer for iOS は、エディターでデザイン時にレンダリングするカスタムコントロールなどの高度な機能をサポートしています。_

> [!WARNING]
> IOS Designer は、Visual Studio 2019 バージョン16.8 と Visual Studio 2019 for Mac バージョン8.8 で段階的に廃止される予定です。
> IOS ユーザーインターフェイスを構築する場合は、Xcode を実行している Mac 上で直接作成することをお勧めします。 詳細については、「 [Xcode を使用したユーザーインターフェイスの設計](../storyboards/index.md)」を参照してください。 

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

[![Visual Studio for Mac の iOS デザイナー](images/designer-vsmac-sml.png "iOS Designer")](images/designer-vsmac.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Visual Studio の iOS デザイナー](images/designer-vs.png "iOS Designer")](images/designer-vs.png#lightbox)

-----

## <a name="availability"></a>可用性

Xamarin Designer for iOS は、Windows の Visual Studio for Mac と Visual Studio 2017 で使用できます。

これらのガイドでは、 [Xamarin の iOS はじめにガイド](~/ios/get-started/index.md)で説明されている内容を理解していることを前提としています。

## <a name="ios-designer-basics"></a>[iOS Designer の基本](introduction.md)

このガイドでは、Xamarin iOS designer の機能について説明します。 デザイナーの基本について説明し、デザイナーを使用してコントロールを視覚的にレイアウトする方法と、プロパティを編集する方法を示します。

## <a name="designable-controls-overview"></a>[デザイン可能コントロールの概要](ios-designable-controls-overview.md)

このガイドでは、カスタムコントロールの詳細、作成方法、デザインサーフェイスにレンダリングするために満たす必要がある要件について詳しく説明します。 また、デザイン可能なコントロールを使用するときに発生する可能性のある一般的な問題をデバッグする方法も示します。

## <a name="walkthrough---using-custom-controls-with-ios-designer"></a>[チュートリアル-iOS Designer でのカスタムコントロールの使用](ios-designable-controls-walkthrough.md)

この記事では、カスタムコントロールを作成し、iOS デザイナーで使用する方法を示す詳細なチュートリアルを提供します。 デザイナーのツールボックスでコントロールを使用できるようにして、ビューにドラッグアンドドロップできるようにする方法を示します。 また、デザイン時および実行時に適切にレンダリングされるようにコントロールを実装する方法と、デザイン時に設定できるプロパティを作成する方法についても説明します。

## <a name="auto-layout-with-the-xamarin-ios-designer"></a>[Xamarin iOS Designer を使用した自動レイアウト](designer-auto-layout.md)

このガイドでは、ios の自動レイアウトと、iOS デザイナーで使用できる新しい制約のワークフローについて説明します。
