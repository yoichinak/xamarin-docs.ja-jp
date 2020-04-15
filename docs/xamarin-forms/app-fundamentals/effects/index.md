---
title: Xamarin.Forms のエフェクト
description: エフェクトを使うと、カスタム レンダラーの実装に頼ることなく、各プラットフォーム上のネイティブ コントロールをカスタマイズできます。
ms.prod: xamarin
ms.assetid: 8AF168A7-4CD9-4603-B961-15B8B1543784
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/01/2017
ms.openlocfilehash: 2de1d1dd065a01bb457ebf03acdc0c01529abf7b
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "73083831"
---
# <a name="xamarinforms-effects"></a>Xamarin.Forms のエフェクト

_Xamarin.Forms のユーザー インターフェイスは、ターゲット プラットフォームのネイティブ コントロールを使用してレンダリングされるため、Xamarin.Forms アプリケーションでは各プラットフォームの適切な外観を維持できます。エフェクトを使うと、カスタム レンダラーの実装に頼ることなく、各プラットフォーム上のネイティブ コントロールをカスタマイズできます。_

## <a name="introduction-to-effects"></a>[エフェクトの概要](introduction.md)

効果を使用すると、各プラットフォーム上のネイティブ コントロールをカスタマイズできます。通常は、小規模なスタイル変更のために使用します。 この記事では、エフェクトについて紹介し、エフェクトとカスタム レンダラーの境界の概要を説明し、`PlatformEffect` クラスについて説明します。

## <a name="creating-an-effect"></a>[エフェクトの作成](creating.md)

エフェクトにより、コントロールのカスタマイズが簡略化されます。 この記事では、[`Entry`](xref:Xamarin.Forms.Entry) コントロールにフォーカスしたときにコントロールの背景色を変更するエフェクトの作成方法を示します。

## <a name="passing-parameters-to-an-effect"></a>[エフェクトにパラメーターを渡す](passing-parameters/index.md)

パラメーターを使って構成されるエフェクトを作成することによって、そのエフェクトを再利用できるようになります。 これらの記事では、プロパティを使ってエフェクトにパラメーターを渡す方法と、実行時にパラメーターを変更する方法を示します。

## <a name="invoking-events-from-an-effect"></a>[エフェクトからのイベントの呼び出し](touch-tracking.md)

エフェクトでイベントを呼び出すことができます。 この記事では、指のマルチタッチ追跡を低レベルで実装し、タッチで押したり、動かしたり、離したりしたときにアプリケーションに通知するイベントの作成方法を示します。

## <a name="reusable-roundeffect"></a>[再利用可能な RoundEffect](reusable-roundeffect.md)

RoundEffect は再利用可能なエフェクトであり、VisualElement から派生したコントロールに適用して、コントロールを円としてレンダリングできます。 このエフェクトを使用して、円形の画像、円形のボタン、その他の円形のコントロールを作成できます。
