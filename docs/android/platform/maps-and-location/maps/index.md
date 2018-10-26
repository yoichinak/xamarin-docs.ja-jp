---
title: Xamarin.Android で Google マップと場所を使用する方法
description: この記事では、Xamarin.Android でマップと場所を使用する方法について説明します。 すべてから直接、Google マップ Android API V2 を使用する組み込みの地図アプリケーションを活用することを説明します。
ms.prod: xamarin
ms.assetid: 425E0ED2-5380-6EBE-7059-256B6E9128B8
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/25/2018
ms.openlocfilehash: fa7fff86e9a7e23bf332f2d62c3ec1a6ed3c54e1
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50117138"
---
# <a name="how-to-use-google-maps-and-location-with-xamarinandroid"></a>Xamarin.Android で Google マップと場所を使用する方法

_この記事では、Xamarin.Android でマップと場所を使用する方法について説明します。すべてから直接、Google マップ Android API V2 を使用する組み込みの地図アプリケーションを活用することを説明します。_

## <a name="maps-overview"></a>マップの概要

マッピング テクノロジは、モバイル デバイスを補完するユビキタスです。 デスクトップ コンピュータおよびラップトップはしないに組み込みの場所を認識している傾向があります。 その一方で、モバイル デバイスは、デバイスを検索して、変更の場所情報を表示する、このようなアプリケーションを使用します。 Android デバイスには、デバイスで使用可能な可能性のある場所のハードウェアを使用してマップ上の場所のデータを表示する組み込みの強力なテクノロジです。 この記事など、Xamarin.Android アプリケーション マップが、提供するさまざまなをについて説明します。 

-  組み込みの地図機能をすばやく追加するアプリケーションにマップします。
-  マップの表示を制御するマップ API を使用します。
-  グラフィカルなオーバーレイを追加するのににはさまざまな手法を使用します。

このセクションのトピックでは、さまざまなマッピングの機能について説明します。
最初に、これらは、Android の組み込みの地図アプリケーションで利用する方法と場所の番地パノラマ ビューを表示する方法について説明します。 それらは、コントロールの位置をマップの表示の両方の方法を説明する、アプリケーション内で直接マッピング機能を組み込む Maps API を使用する方法と、グラフィック オーバーレイを追加する方法について説明します。


## <a name="related-links"></a>関連リンク

- [MapsAndLocationDemo_v3 (sample)](https://developer.xamarin.com/samples/monodroid/MapsAndLocationDemo_v3/)
- [アクティビティのライフサイクル](~/android/app-fundamentals/activity-lifecycle/index.md)
- [Google マップ API キーを取得する](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)
- [インテント一覧: Android デバイスで Google のアプリケーションを起動します。](http://developer.android.com/guide/appendix/g-app-intents.html)
- [場所とマップ](http://developer.android.com/guide/topics/location/index.html)
