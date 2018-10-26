---
title: Xamarin.iOS アプリでのタッチを処理します。
description: このドキュメントは、タッチ、マルチタッチ、ジェスチャ、および Xamarin.iOS アプリで 3D Touch を使用する方法を説明するガイドにリンクしています。
ms.prod: xamarin
ms.assetid: E3904713-6018-4755-A315-EB045DFB3500
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 01/23/2017
ms.openlocfilehash: 5aabc3a3c2ffbcffc0e12379989f7eb43b03a902
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50116629"
---
# <a name="handling-touch-in-xamarinios-apps"></a>Xamarin.iOS アプリでのタッチを処理します。

他のモバイル プラットフォームのようには、iOS は、さまざまなタッチを処理する方法が。 マルチタッチをサポートできます-画面で連絡先の多くのポイント: および複雑なジェスチャ。 このガイドでは、いくつかの概念、および iOS でのタッチおよびジェスチャの実装の詳細について説明します。

iOS でのタッチ データをカプセル化、`UITouch`クラスは、一連のアプリケーションに利用可能になって`UIResponder`メソッド。 アプリケーションのサブクラスでこれらのメソッドをオーバーライドできます`UIView`と`UIViewController`、どちらも継承`UIResponder`します。

タッチ データのキャプチャ、に加えて iOS はタッチ ジェスチャのパターンを解釈するための手段を提供します。 これらのジェスチャ レコグナイザーは、イメージの回転やページの有効にするなど、アプリケーション固有のコマンドを解釈するさらに使用できます。 iOS では、最小値に追加されたコードでの一般的なジェスチャを処理するクラスの豊富なコレクションを提供します。

タッチとジェスチャ レコグナイザーの間で choice では、混乱の 1 つを指定できます。 このガイドでは、一般に、基本設定を許可できることジェスチャ レコグナイザーをお勧めします。 ジェスチャ レコグナイザーは、懸念事項と優れたカプセル化の分離を提供する個別のクラスとして実装されます。 これにより、簡単に記述されたコードの量を最小限に抑え、個別のビューの間のロジックを共有できます。

ただし、低レベルのタッチ処理を使用しても finger-paint プログラムを作成する複数の指を追跡する必要がある場合もあります。

## <a name="sections"></a>セクション

-  [iOS でのタッチ](touch-in-ios.md)
-  [チュートリアル: iOS でのタッチの使用](ios-touch-walkthrough.md)
-  [マルチタッチ追跡](touch-tracking.md)

このガイドは、タッチで iOS の概要として機能します。 IOS での 3D タッチと Haptic フィードバックの使用の詳細については、これが導入されました。 ios 9 と 10 はそれぞれ、以下の特定のガイドを参照してください。

* [3D Touch](~/ios/platform/3d-touch.md)
* [Haptic フィードバックの提供](~/ios/user-interface/ios-ui/haptic-feedback.md)

## <a name="related-links"></a>関連リンク

- [iOS (サンプル) を開始するタッチ](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_start)
- [iOS の最終タッチ (サンプル)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_final)
- [FingerPaint (サンプル)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/FingerPaint)
