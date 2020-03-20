---
title: Xamarin.Android で Google Maps と位置情報を使用する方法
description: この記事では、Xamarin.Android でマップと位置情報を使用する方法について説明します。 組み込みのマップ アプリケーションの利用方法から Google Maps Android API V2 の直接的な使用方法に至るすべての事柄について説明します。
ms.prod: xamarin
ms.assetid: 425E0ED2-5380-6EBE-7059-256B6E9128B8
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/25/2018
ms.openlocfilehash: d877f415bb96024bb41edc2be9aec108ae248e88
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/10/2020
ms.locfileid: "73020035"
---
# <a name="how-to-use-google-maps-and-location-with-xamarinandroid"></a>Xamarin.Android で Google Maps と位置情報を使用する方法

"_この記事では、Xamarin.Android でマップと位置情報を使用する方法について説明します。組み込みのマップ アプリケーションの利用方法から Google Maps Android API V2 の直接的な使用方法に至るすべての事柄について説明します。_ "

## <a name="maps-overview"></a>マップの概要

マッピング テクノロジは、モバイル デバイスで広く使用されている技術です。 デスクトップ コンピューターやラップトップには、位置の認識は組み込まれない傾向があります。 一方、モバイル デバイスでは、このようなアプリケーションを使用してデバイスの位置が特定され、変化する位置情報が表示されます。 Android には、デバイスで利用可能な位置情報ハードウェアを使用して、位置情報データをマップ上に表示する強力なテクノロジが組み込まれています。 この記事では、Xamarin.Android のマップ アプリケーションで提供する必要がある機能範囲について説明します。以下が含まれます。 

- 組み込みのマップ アプリケーションを使用して、マッピング機能をすばやく追加します。
- Maps API を使用して、マップの表示を制御します。
- さまざまな手法を使用して、グラフィカル オーバーレイを追加します。

このセクションのトピックでは、幅広いマッピング機能について説明します。
最初に、Android の組み込みマップ アプリケーションを活用する方法と、場所のパノラマ ビューを表示する方法について説明します。 次に、Maps API を使用して、マッピング機能をアプリケーション内に直接組み込む方法について説明します。マップの配置と表示を制御する方法の説明とグラフィカル オーバーレイを追加する方法の説明の両方が含まれています。

## <a name="related-links"></a>関連リンク

- [MapsAndLocationDemo_v3 (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/mapsandlocationdemo-v3)
- [アクティビティのライフサイクル](~/android/app-fundamentals/activity-lifecycle/index.md)
- [Google マップ API キーを取得する](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)
- [インテントの一覧:Android デバイス上で Google アプリケーションを呼び出す](https://developer.android.com/guide/appendix/g-app-intents.html)
- [位置情報と地図](https://developer.android.com/guide/topics/location/index.html)
