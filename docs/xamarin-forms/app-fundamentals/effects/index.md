---
title: Xamarin.Forms のエフェクト
description: エフェクトを使うと、カスタム レンダラーの実装に頼ることなく、各プラットフォーム上のネイティブ コントロールをカスタマイズできます。
ms.prod: xamarin
ms.assetid: 8AF168A7-4CD9-4603-B961-15B8B1543784
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/01/2017
ms.openlocfilehash: 9d859377c40c6fca07e140c50da46d8f30aaae04
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994459"
---
# <a name="xamarinforms-effects"></a>Xamarin.Forms のエフェクト

_Xamarin.Forms のユーザー インターフェイスは、ターゲット プラットフォームのネイティブ コントロールを使用してレンダリングされるため、Xamarin.Forms アプリケーションでは各プラットフォームの適切な外観を維持できます。エフェクトを使うと、カスタム レンダラーの実装に頼ることなく、各プラットフォーム上のネイティブ コントロールをカスタマイズできます。_

## <a name="introduction-to-effectsintroductionmd"></a>[エフェクトの概要](introduction.md)

エフェクトを使うと、各プラットフォーム上のネイティブ コントロールをカスタマイズできます。通常は、スタイルに関する小さな変更のために使います。 この記事では、エフェクトについて紹介し、エフェクトとカスタム レンダラーの境界の概要を説明し、`PlatformEffect` クラスについて説明します。

## <a name="creating-an-effectcreatingmd"></a>[エフェクトの作成](creating.md)

エフェクトにより、コントロールのカスタマイズが簡略化されます。 この記事では、[`Entry`](xref:Xamarin.Forms.Entry) コントロールにフォーカスしたときにコントロールの背景色を変更するエフェクトの作成方法を示します。

## <a name="passing-parameters-to-an-effectpassing-parametersindexmd"></a>[エフェクトにパラメーターを渡す](passing-parameters/index.md)

パラメーターを使って構成されるエフェクトを作成することによって、そのエフェクトを再利用できるようになります。 これらの記事では、プロパティを使ってエフェクトにパラメーターを渡す方法と、実行時にパラメーターを変更する方法を示します。

## <a name="invoking-events-from-an-effecttouch-trackingmd"></a>[エフェクトからのイベントの呼び出し](touch-tracking.md)

エフェクトでイベントを呼び出すことができます。 この記事では、指のマルチタッチ追跡を低レベルで実装し、タッチで押したり、動かしたり、離したりしたときにアプリケーションに通知するイベントの作成方法を示します。
