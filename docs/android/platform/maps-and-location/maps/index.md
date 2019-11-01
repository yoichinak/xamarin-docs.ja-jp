---
title: Xamarin Android で Google Maps と場所を使用する方法
description: この記事では、maps と location を Xamarin Android で使用する方法について説明します。 組み込みの maps アプリケーションを利用して、Google Maps Android API V2 を直接使用する方法のすべてについて説明します。
ms.prod: xamarin
ms.assetid: 425E0ED2-5380-6EBE-7059-256B6E9128B8
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/25/2018
ms.openlocfilehash: d877f415bb96024bb41edc2be9aec108ae248e88
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73020035"
---
# <a name="how-to-use-google-maps-and-location-with-xamarinandroid"></a>Xamarin Android で Google Maps と場所を使用する方法

_この記事では、maps と location を Xamarin Android で使用する方法について説明します。組み込みの maps アプリケーションを利用して、Google Maps Android API V2 を直接使用する方法のすべてについて説明します。_

## <a name="maps-overview"></a>マップの概要

マッピングテクノロジは、広く普及しているモバイルデバイスです。 デスクトップコンピューターやラップトップには、位置認識が組み込まれていない傾向があります。 一方、モバイルデバイスは、このようなアプリケーションを使用してデバイスを特定し、変更された位置情報を表示します。 Android には、デバイスで利用可能な場所のハードウェアを使用してマップ上の場所データを表示する、強力な組み込みテクノロジがあります。 この記事では、Xamarin での maps アプリケーションの機能について説明します。 Android には次のものが含まれます。 

- 組み込みの maps アプリケーションを使用して、マッピング機能をすばやく追加します。
- Maps API を使用してマップの表示を制御する。
- さまざまな手法を使用して、グラフィカルなオーバーレイを追加します。

このセクションのトピックでは、幅広いマッピング機能について説明します。
まず、Android の組み込みマップアプリケーションを活用する方法と、場所のパノラマビューを表示する方法について説明します。 次に、Maps API を使用して、マッピング機能をアプリケーション内に直接組み込む方法について説明します。また、マップの位置と表示を制御する方法と、グラフィカルなオーバーレイを追加する方法についても説明します。

## <a name="related-links"></a>関連リンク

- [MapsAndLocationDemo_v3 (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/mapsandlocationdemo-v3)
- [アクティビティのライフサイクル](~/android/app-fundamentals/activity-lifecycle/index.md)
- [Google マップ API キーを取得する](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)
- [インテントリスト: Android デバイスでの Google アプリケーションの呼び出し](https://developer.android.com/guide/appendix/g-app-intents.html)
- [場所とマップ](https://developer.android.com/guide/topics/location/index.html)
