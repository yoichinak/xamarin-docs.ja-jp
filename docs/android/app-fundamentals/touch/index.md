---
title: タッチ
description: 現在のデバイスの多くのタッチ スクリーンでは、迅速かつ効率的に、自然で直感的な方法でデバイスと対話するようにします。 この連携では簡単なタッチの検出にのみ限定されません - もジェスチャを使用することはできます。 たとえば、ピンチ、ズーム ジェスチャは、はさむ、ユーザーは拡大または縮小する 2 本の指で画面の一部でこの非常に一般的な例を示します。このガイドでは、タッチを調べて、Android のジェスチャします。
ms.prod: xamarin
ms.assetid: 61874769-978A-4562-9B2A-7FFD45F58B38
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: d6c6d7fd02511ede84b7cfa75575d755f11bab99
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="touch"></a>タッチ

_現在のデバイスの多くのタッチ スクリーンでは、迅速かつ効率的に、自然で直感的な方法でデバイスと対話するようにします。この連携では簡単なタッチの検出にのみ限定されません - もジェスチャを使用することはできます。たとえば、ピンチ、ズーム ジェスチャは、はさむ、ユーザーは拡大または縮小する 2 本の指で画面の一部でこの非常に一般的な例を示します。このガイドでは、タッチを調べて、Android のジェスチャします。_

## <a name="touch-overview"></a>タッチの概要

iOS および Android タッチを処理する方法と類似しています。 マルチタッチ - 画面上の連絡先の多数のポイント、および複雑なジェスチャどちらもサポートできます。 このガイドでは、いくつかの概念の類似点とタッチの実装の詳細を紹介し、両方のプラットフォームでジェスチャします。

Android を使用して、`MotionEvent`タッチ データ、およびタッチをリッスンするビュー オブジェクトのメソッドをカプセル化するオブジェクト。

タッチ データのキャプチャ、に加えて iOS および Android の両方はタッチ ジェスチャのパターンを解釈する際の手段を提供します。 これらのジェスチャ レコグナイザーは、イメージの回転をまたはページの有効にするなど、アプリケーション固有のコマンドを解釈するさらに使用できます。 Android では、いくつかのサポートされているジェスチャをできるだけ簡単の複雑なカスタム ジェスチャを追加するためにリソースを提供します。

Android または iOS を使用しているかどうかタッチとジェスチャ レコグナイザーの選択肢は複雑になる場合の 1 つを指定できます。 このガイドでは、一般に、基本設定したするようにジェスチャ レコグナイザーをお勧めします。 ジェスチャ レコグナイザーは、上の問題および効率的にカプセル化の分離を提供する個別のクラスとして実装されます。 これにより、簡単に記述されたコードの量を最小限に抑え、さまざまなビューの間のロジックを共有します。

このガイドは次の各オペレーティング システムのような形式: 最初に、プラットフォームのタッチ Api が導入され、説明、どのタッチの相互作用を構築する基礎とします。 次に進むジェスチャ レコグナイザー – の世界最初でいくつかの一般的なジェスチャを活用し、を終了したアプリケーションのカスタム ジェスチャを作成します。 最後に、finger-paint プログラムを作成するタッチの低レベルの追跡を使用して個々 の指を追跡する方法が表示されます。

## <a name="sections"></a>セクション

-  [Android でのタッチ](~/android/app-fundamentals/touch/android-touch-walkthrough.md)
-  [チュートリアル: Android でのタッチの使用](~/android/app-fundamentals/touch/android-touch-walkthrough.md)
-  [マルチタッチ追跡](touch-tracking.md)

## <a name="summary"></a>まとめ

このガイドでは、Android でのタッチ調べられます。 両方のオペレーティング システムのタッチを有効にする方法とタッチ イベントに応答する方法を学習します。 次に、学習ジェスチャ、およびいくつかのジェスチャ レコグナイザー Android と iOS の両方より一般的なシナリオの一部が処理を提供します。 ここには、カスタム ジェスチャを作成し、それらをアプリケーションに実装する方法が調べられます。 なチュートリアルについては、アクション内の各オペレーティング システムの概念と Api を実演し、個々 の本の指を追跡する方法も説明しました。



## <a name="related-links"></a>関連リンク

- [Android タッチ (サンプル) を開始](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_start)
- [Android タッチ最終 (サンプル)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_final)
- [フィンガー ペイント (サンプル)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint)
