---
title: "効果"
description: "Xamarin.Forms ユーザー インターフェイスでは、Xamarin.Forms アプリケーション プラットフォームごとに適切なルック アンド フィールを維持できるように、ターゲット プラットフォームのネイティブ コントロールを使用して表示されます。 効果は、カスタム レンダラーの実装に頼ることがなくカスタマイズするには、各プラットフォームでネイティブ コントロールを許可します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 8AF168A7-4CD9-4603-B961-15B8B1543784
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/01/2017
ms.openlocfilehash: dd8b98982052e15744bf67ece6a25cd940a9875a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="effects"></a>効果

_Xamarin.Forms ユーザー インターフェイスでは、Xamarin.Forms アプリケーション プラットフォームごとに適切なルック アンド フィールを維持できるように、ターゲット プラットフォームのネイティブ コントロールを使用して表示されます。効果は、カスタム レンダラーの実装に頼ることがなくカスタマイズするには、各プラットフォームでネイティブ コントロールを許可します。_

## <a name="introduction-to-effectsintroductionmd"></a>[影響の概要](introduction.md)

エフェクトをカスタマイズするには、各プラットフォームでネイティブ コントロールは、小規模のスタイル設定の変更に通常使用されます。 この記事は、効果を紹介し、効果とカスタム レンダラーは、境界の概要を示しますについて説明します、`PlatformEffect`クラスです。

## <a name="creating-an-effectcreatingmd"></a>[特殊効果を作成します。](creating.md)

効果は、コントロールのカスタマイズを簡略化します。 この記事は、の背景色を変更する効果を作成する方法を示します、 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)コントロールがフォーカスを取得する場合を制御します。

## <a name="passing-parameters-to-an-effectpassing-parametersindexmd"></a>[効果にパラメーターの引き渡し](passing-parameters/index.md)

パラメーターを使用して構成されている効果を作成すると、再利用する効果ができます。 これらの記事をデモンストレーション、効果にパラメーターを渡すプロパティを使用して実行時にパラメーターを変更するとします。

## <a name="invoking-events-from-an-effecttouch-trackingmd"></a>[影響からのイベントの呼び出し](touch-tracking.md)

効果は、イベントを呼び出すことができます。 この記事では、タッチの押下、移動、およびリリースのアプリケーションに通知をマルチタッチ本指の低レベルの追跡を実装するイベントを作成する方法を示します。