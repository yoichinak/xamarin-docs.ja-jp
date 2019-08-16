---
title: Xamarin. Android でのタッチとジェスチャ
description: 今日の多くのデバイスでタッチスクリーンを使用すると、ユーザーは、自然で直感的な方法でデバイスとすばやく効率的に対話できます。 この相互作用は、単純なタッチ検出だけに限定されません。ジェスチャを使用することもできます。 たとえば、ピンチとズームのジェスチャは、画面の一部を、ユーザーが拡大または縮小できる2本の指をピンチことによって、非常に一般的な例です。このガイドでは、Android でのタッチとジェスチャについて説明します。
ms.prod: xamarin
ms.assetid: 61874769-978A-4562-9B2A-7FFD45F58B38
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 0b11f5c290f07debb61d511bdd27ad1a7ad0b0b0
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2019
ms.locfileid: "69526086"
---
# <a name="touch-and-gestures-in-xamarinandroid"></a>Xamarin. Android でのタッチとジェスチャ

_今日の多くのデバイスでタッチスクリーンを使用すると、ユーザーは、自然で直感的な方法でデバイスとすばやく効率的に対話できます。この相互作用は、単純なタッチ検出だけに限定されません。ジェスチャを使用することもできます。たとえば、ピンチとズームのジェスチャは、画面の一部を、ユーザーが拡大または縮小できる2本の指をピンチことによって、非常に一般的な例です。このガイドでは、Android でのタッチとジェスチャについて説明します。_

## <a name="touch-overview"></a>タッチの概要

iOS と Android は、タッチ処理の方法に似ています。 どちらも、画面と複雑なジェスチャで、マルチタッチの多くの接点をサポートできます。 このガイドでは、概念のいくつかの類似点と、両方のプラットフォームでのタッチとジェスチャの実装の particularities について説明します。

Android では`MotionEvent` 、オブジェクトを使用してタッチデータをカプセル化し、ビューオブジェクトにメソッドを使用してタッチを待機します。

タッチデータをキャプチャするだけでなく、iOS と Android はどちらもジェスチャへのタッチパターンを解釈するための手段を提供します。 これらのジェスチャレコグナイザーは、イメージの回転やページのめくりなど、アプリケーション固有のコマンドを解釈するために使用できます。 Android では、サポートされるジェスチャがいくつか提供されています。また、複雑なカスタムジェスチャを簡単に追加するためのリソースも用意されています。

Android と iOS のどちらで作業している場合でも、タッチ認識とジェスチャレコグナイザーのどちらを選択するかは混乱を招く可能性があります。 このガイドでは、一般にジェスチャレコグナイザーに設定することをお勧めします。 ジェスチャレコグナイザーは不連続クラスとして実装されます。これにより、関心の分離とカプセル化の向上が実現します。 これにより、異なるビュー間でロジックを簡単に共有し、記述されたコードの量を最小限に抑えることができます。

このガイドは、オペレーティングシステムごとに同様の形式に従います。まず、タッチ操作が構築される基礎として、プラットフォームのタッチ Api が導入され、説明されています。 次に、いくつかの一般的なジェスチャを調べて、アプリケーションのカスタムジェスチャを作成することによって、ジェスチャレコグナイザーの世界について説明します。 最後に、低レベルのタッチ追跡を使用して個々の指を追跡し、フィンガーペイントプログラムを作成する方法について説明します。

## <a name="sections"></a>セクション

- [Android でのタッチ](~/android/app-fundamentals/touch/android-touch-walkthrough.md)
- [チュートリアル: Android でのタッチの使用](~/android/app-fundamentals/touch/android-touch-walkthrough.md)
- [マルチタッチトラッキング](touch-tracking.md)

## <a name="summary"></a>Summary

このガイドでは、Android でのタッチについて説明します。 どちらのオペレーティングシステムでも、タッチを有効にし、タッチイベントに応答する方法を学習しました。 次に、いくつかの一般的なシナリオを処理するために Android と iOS の両方で提供されるジェスチャとジェスチャ認識機能について学習しました。 カスタムジェスチャを作成し、アプリケーションに実装する方法について説明します。 チュートリアルでは、動作している各オペレーティングシステムの概念と Api について説明しました。また、個々の指を追跡する方法についても説明しました。



## <a name="related-links"></a>関連リンク

- [Android タッチスタート (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-touch-start)
- [Android のタッチ最終 (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-touch-final)
- [FingerPaint (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-fingerpaint)
