---
title: Ios デザイナー ユーザー インターフェイスの構築
description: このドキュメントでは、デザイナーを使用して、Xamarin for iOS 用のストーリー ボードおよび .xib ファイルと、アプリのユーザー インターフェイスを構築する方法について説明します。 ツールの可用性、その基本的な機能、設計可能なコントロールは、について説明し、その使用方法のチュートリアルを提供するドキュメントにリンクします。
ms.prod: xamarin
ms.assetid: E35EFB69-EBBA-40E3-ADBE-CB8016F17127
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/31/2018
ms.openlocfilehash: eadc2147a44d6077436e394a4757d367ce42e5fa
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790004"
---
# <a name="building-user-interfaces-with-the-ios-designer"></a>Ios デザイナー ユーザー インターフェイスの構築

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

=======
# <a name="ios-designer"></a>iOS Designer

_IOS 用の Xamarin デザイナーは、Mac と Visual Studio の Visual Studio に完全に統合されている iOS ストーリー ボードとインターフェイスのビルダー形式をビジュアル デザイナーです。IOS デザイナーは、Visual Studio for Mac または Xcode のインターフェイスのビルダーに加えて、Visual Studio でファイルを編集できるように、ストーリー ボードと .xib 形式と完全な互換性を維持します。さらに、iOS 用の Xamarin デザイナーは、エディターでのデザイン時に表示するカスタム コントロールなどの高度な機能をサポートします。_
>>>>>>> master

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![Mac 用の Visual Studio デザイナーでの iOS](images/designer-vsmac-sml.png "iOS デザイナー")](images/designer-vsmac.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Visual Studio デザイナーでの iOS](images/designer-vs.png "iOS デザイナー")](images/designer-vs.png#lightbox)

-----

## <a name="availability"></a>可用性

IOS 用の Xamarin デザイナーは、Windows 上の Visual Studio 2017 と Mac を Visual Studio で使用できます。

これらのガイドで説明した内容に関する知識を前提と、 [Xamarin.iOS が概要ガイド](~/ios/get-started/index.md)です。

## <a name="ios-designer-basicsintroductionmd"></a>[iOS Designer の基本](introduction.md)

このガイドでは、Xamarin iOS デザイナーの機能について説明します。 デザイナーを使用してコントロールを視覚的にレイアウトする方法とプロパティを編集する方法を示す、デザイナーの基本を説明します。

## <a name="designable-controls-overviewios-designable-controls-overviewmd"></a>[コントロールのデザインの概要](ios-designable-controls-overview.md)

このガイドは、カスタム コントロールは、深さの検索の作成方法およびどのような要件は、デザイン画面に表示される満たしている必要があります。 また、設計可能なコントロールを使用するときに発生する一般的な問題をデバッグする方法を示します。

## <a name="walkthrough---using-custom-controls-with-ios-designerios-designable-controls-walkthroughmd"></a>[チュートリアル - iOS デザイナーでカスタム コントロールの使用](ios-designable-controls-walkthrough.md)

この記事では、カスタム コントロールを作成し、iOS デザイナーで使用する方法を示す詳細なチュートリアルを提供します。 ようにコントロール デザイナーのツールボックスで使用できるドラッグ/削除するビューにする方法を示します。 また、デザイン時および実行時、正しくレンダリングされるようにコントロールを実装する方法と、デザイン時に設定できるプロパティを作成する方法を示します。

## <a name="auto-layout-with-the-xamarin-ios-designerdesigner-auto-layoutmd"></a>[Xamarin iOS デザイナーで自動レイアウト](designer-auto-layout.md)

このガイドでは、自動レイアウトを iOS および iOS デザイナーで使用できる新しい制約のワークフローが導入されています。
