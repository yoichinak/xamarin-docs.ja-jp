---
title: Xamarin.Forms の効果
description: 効果は、カスタム レンダラーの実装に頼ることがなくカスタマイズするには、各プラットフォームのネイティブ コントロールを許可します。
ms.prod: xamarin
ms.assetid: 8AF168A7-4CD9-4603-B961-15B8B1543784
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/01/2017
ms.openlocfilehash: 9d859377c40c6fca07e140c50da46d8f30aaae04
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994459"
---
# <a name="xamarinforms-effects"></a>Xamarin.Forms の効果

_Xamarin.Forms のユーザー インターフェイスは、Xamarin.Forms アプリケーションは各プラットフォームの適切なルック アンド フィールを保持する、ターゲット プラットフォームのネイティブ コントロールを使用してレンダリングされます。効果は、カスタム レンダラーの実装に頼ることがなくカスタマイズするには、各プラットフォームのネイティブ コントロールを許可します。_

## <a name="introduction-to-effectsintroductionmd"></a>[効果の概要](introduction.md)

カスタマイズするには、各プラットフォームのネイティブ コントロールを許可する効果は通常小さなスタイルの変更の使用とします。 この記事で効果の概要については、効果とカスタム レンダラーでは、境界の概要を説明します、説明、`PlatformEffect`クラス。

## <a name="creating-an-effectcreatingmd"></a>[効果を作成します。](creating.md)

効果は、コントロールのカスタマイズを簡略化します。 この記事では、の背景色を変更する効果を作成する方法を示します、 [ `Entry` ](xref:Xamarin.Forms.Entry)コントロールがフォーカスを取得するときを制御します。

## <a name="passing-parameters-to-an-effectpassing-parametersindexmd"></a>[効果にパラメーターの引き渡し](passing-parameters/index.md)

パラメーターで構成される効果を作成することによって、再利用する効果。 これらの記事では、特殊効果にパラメーターを渡すプロパティの使用方法について説明し、実行時にパラメーターを変更します。

## <a name="invoking-events-from-an-effecttouch-trackingmd"></a>[効果からのイベントの呼び出し](touch-tracking.md)

効果は、イベントを呼び出すことができます。 この記事では、低レベルのマルチタッチ指の追跡を実装し、タッチ操作、移動、およびリリース用のアプリケーションに通知するイベントを作成する方法を示します。
