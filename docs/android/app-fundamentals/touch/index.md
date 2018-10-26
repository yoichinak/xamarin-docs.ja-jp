---
title: タッチおよびジェスチャを Xamarin.Android で
description: 現在のデバイスの多くのタッチ スクリーンでは、迅速かつ効率的に、自然で直感的な方法でデバイスと対話するようにします。 この操作が簡単なタッチの検出だけに制限されています - もジェスチャを使用することは。 たとえば、ピンチ ズーム ジェスチャはこの非常に一般的な例、ユーザーは拡大または縮小する 2 本の指で画面の一部をピンチしています。このガイドは、タッチを調べ、Android でジェスチャします。
ms.prod: xamarin
ms.assetid: 61874769-978A-4562-9B2A-7FFD45F58B38
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 7f957c9ff5a0e7c3a0821978703860ed2f723a92
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50123324"
---
# <a name="touch-and-gestures-in-xamarinandroid"></a>タッチおよびジェスチャを Xamarin.Android で

_現在のデバイスの多くのタッチ スクリーンでは、迅速かつ効率的に、自然で直感的な方法でデバイスと対話するようにします。この操作が簡単なタッチの検出だけに制限されています - もジェスチャを使用することは。たとえば、ピンチ ズーム ジェスチャはこの非常に一般的な例、ユーザーは拡大または縮小する 2 本の指で画面の一部をピンチしています。このガイドは、タッチを調べ、Android でジェスチャします。_

## <a name="touch-overview"></a>タッチの概要

iOS と Android は、タッチを処理する点で似ています。 両方には、マルチタッチ画面で連絡先の多くポイント - と複雑なジェスチャをサポートできます。 このガイドでは、いくつかの概念の類似点とタッチの実装の詳細を紹介し、両方のプラットフォームでジェスチャします。

Android を使用して、`MotionEvent`タッチ データ、およびタッチをリッスンするビュー オブジェクトのメソッドをカプセル化するオブジェクト。

タッチ データのキャプチャ、に加えて iOS と Android の両方はタッチ ジェスチャのパターンを解釈するための手段を提供します。 これらのジェスチャ レコグナイザーは、イメージの回転やページの有効にするなど、アプリケーション固有のコマンドを解釈するさらに使用できます。 Android では、複雑なカスタム ジェスチャを簡単に追加のリソースと同様にサポートされているジェスチャのいくつか提供します。

Android や iOS 上を操作するかどうかタッチとジェスチャ レコグナイザーの間で choice 混乱を招くものを使用できます。 このガイドでは、一般に、基本設定を許可できることジェスチャ レコグナイザーをお勧めします。 ジェスチャ レコグナイザーは、懸念事項と優れたカプセル化の分離を提供する個別のクラスとして実装されます。 これにより、簡単に記述されたコードの量を最小限に抑え、個別のビューの間のロジックを共有できます。

このガイドは次の各オペレーティング システムのような形式: 最初に、プラットフォームのタッチ Api の導入し、はどのタッチの相互作用を構築する基礎について説明します。 次に、について詳細に説明ジェスチャ レコグナイザー – の世界最初のいくつかの一般的なジェスチャでは、調査とアプリケーションのカスタム ジェスチャの作成に関する最後の仕上げを。 最後に、タッチの低レベルの追跡を使用して finger-paint プログラムを作成する個々 の指を追跡する方法を確認します。

## <a name="sections"></a>セクション

-  [Android でのタッチ](~/android/app-fundamentals/touch/android-touch-walkthrough.md)
-  [チュートリアル: Android でタッチを使用します。](~/android/app-fundamentals/touch/android-touch-walkthrough.md)
-  [マルチタッチ追跡](touch-tracking.md)

## <a name="summary"></a>まとめ

このガイドでは、Android でタッチについて確認しました。 両方のオペレーティング システムでは、タッチを有効にする方法と、タッチ イベントに応答する方法を学びました。 次に、学習ジェスチャ、ジェスチャ レコグナイザーのいくつかの Android と iOS の両方がいくつかの一般的なシナリオを処理するために提供します。 私たちは、カスタム ジェスチャを作成し、それらをアプリケーションに実装する方法を調査します。 チュートリアルは、アクション内の各オペレーティング システムの概念と Api を実演し、個々 の本の指を追跡する方法も説明しました。



## <a name="related-links"></a>関連リンク

- [Android タッチ (サンプル) を開始](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_start)
- [Android タッチ最後 (サンプル)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_final)
- [FingerPaint (サンプル)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint)
