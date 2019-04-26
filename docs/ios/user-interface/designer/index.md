---
title: IOS Designer でのユーザー インターフェイスの構築
description: このドキュメントでは、Xamarin iOS デザイナーを使用して、ストーリー ボードと .xib ファイルによるアプリのユーザー インターフェイスを構築する方法について説明します。 ツールの可用性、その基本的な機能、デザインのコントロールについて説明し、その使用方法のチュートリアルを提供するドキュメントにリンクします。
ms.prod: xamarin
ms.assetid: E35EFB69-EBBA-40E3-ADBE-CB8016F17127
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 05/31/2018
ms.openlocfilehash: 7c6529c539ab502fb6c13226acd18a57f8f6d58e
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61378688"
---
# <a name="building-user-interfaces-with-the-ios-designer"></a>IOS Designer でのユーザー インターフェイスの構築

_IOS 用の Xamarin のデザイナーは、Mac と Visual Studio の Visual Studio と完全に統合されている iOS ストーリー ボードと Interface Builder の形式のビジュアル デザイナーです。IOS Designer は、Visual Studio for Mac または Xcode の Interface Builder だけでなく、Visual Studio のいずれかでファイルを編集できるように、ストーリー ボードと .xib 形式の完全な互換性を維持します。さらに、iOS 用の Xamarin のデザイナーでは、エディターのデザイン時にレンダリングするカスタム コントロールなどの高度な機能をサポートしています。_

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![iOS Visual Studio for Mac で Designer](images/designer-vsmac-sml.png "iOS Designer")](images/designer-vsmac.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Visual Studio デザイナーでの iOS](images/designer-vs.png "iOS Designer")](images/designer-vs.png#lightbox)

-----

## <a name="availability"></a>可用性

IOS 用の Xamarin のデザイナーは、Windows で Visual Studio 2017 と Visual Studio for Mac で使用できます。

これらのガイドで説明した内容を熟知することを前提としています、[ガイド Xamarin.iOS Getting Started](~/ios/get-started/index.md)します。

## <a name="ios-designer-basicsintroductionmd"></a>[iOS Designer の基本](introduction.md)

このガイドでは、Xamarin iOS デザイナーの機能について説明します。 デザイナーを使用して、視覚的にコントロールをレイアウトする方法とプロパティを編集する方法を示す、designer の基本を説明します。

## <a name="designable-controls-overviewios-designable-controls-overviewmd"></a>[デザイン可能なコントロールの概要](ios-designable-controls-overview.md)

このガイドでは、カスタム コントロールの深さでの作成方法およびどのような要件をデザイン サーフェイスに表示を満たしている必要があります。 さらに、デザイン可能なコントロールを使用する場合に発生する可能性がある一般的な問題をデバッグする方法を示します。

## <a name="walkthrough---using-custom-controls-with-ios-designerios-designable-controls-walkthroughmd"></a>[チュートリアル - iOS Designer でカスタム コントロールの使用](ios-designable-controls-walkthrough.md)

この記事では、カスタム コントロールを作成し、iOS デザイナーで使用する方法を示すステップ バイ ステップ チュートリアルを提供します。 そのドラッグ/ドロップできますビューのためにコントロールをデザイナーのツールボックスで使用できるようにする方法を示します。 また、デザイン時およびランタイム、正しくレンダリングされるようにコントロールを実装する方法と、デザイン時に設定できるプロパティを作成する方法を示します。

## <a name="auto-layout-with-the-xamarin-ios-designerdesigner-auto-layoutmd"></a>[Xamarin iOS Designer での自動レイアウト](designer-auto-layout.md)

このガイドでは、自動レイアウトの iOS および iOS デザイナーで使用できる新しい制約のワークフローが導入されています。
