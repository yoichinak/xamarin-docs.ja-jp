---
title: Xamarin.iOS アプリでのタッチの処理
description: このドキュメントには、タッチ、マルチタッチ、ジェスチャ、および 3 D Touch Xamarin.iOS アプリで使用する方法を説明するガイドへのリンクがします。
ms.prod: xamarin
ms.assetid: E3904713-6018-4755-A315-EB045DFB3500
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/23/2017
ms.openlocfilehash: eb8dce8b13345c13a6f95ae7784bd135e7d1f1f5
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784163"
---
# <a name="handling-touch-in-xamarinios-apps"></a>Xamarin.iOS アプリでのタッチの処理

その他のモバイル プラットフォームと同様には、iOS は、タッチを処理する方法の番号を持ちます。 マルチタッチをサポートして、画面上の連絡先の多数のポイント — や複雑なジェスチャ。 このガイドでは、いくつかの概念、および iOS でのタッチとジェスチャの実装の詳細について説明します。

iOS でのタッチ データをカプセル化、`UITouch`クラスは、一連のアプリケーションに提供されて`UIResponder`メソッドです。 アプリケーションのサブクラスでこれらのメソッドをオーバーライドできます`UIView`と`UIViewController`、どちらも継承`UIResponder`です。

タッチ データのキャプチャ、に加えては、iOS は、タッチ ジェスチャのパターンを解釈するための手段を提供します。 これらのジェスチャ レコグナイザーは、イメージの回転をまたはページの有効にするなど、アプリケーション固有のコマンドを解釈するさらに使用できます。 iOS では、最低限のコードを追加で共通のジェスチャを処理するクラスの機能豊富なコレクションを提供します。

タッチとジェスチャ レコグナイザーの選択肢には、複雑になる場合のどちらを指定できます。 このガイドでは、一般に、基本設定したするようにジェスチャ レコグナイザーをお勧めします。 ジェスチャ レコグナイザーは、上の問題および効率的にカプセル化の分離を提供する個別のクラスとして実装されます。 これにより、簡単に記述されたコードの量を最小限に抑え、さまざまなビューの間のロジックを共有します。

ただし、低レベルのタッチ処理を使用しても finger-paint プログラムを作成するなどの複数の指を追跡する必要がある場合があります。

## <a name="sections"></a>セクション

-  [iOS でのタッチ](touch-in-ios.md)
-  [チュートリアル: iOS でのタッチの使用](ios-touch-walkthrough.md)
-  [マルチタッチ追跡](touch-tracking.md)

このガイドは、iOS でのタッチ概要として機能します。 3D のタッチと Haptic フィードバックを使用して、iOS での詳細については、これで導入された iOS 9 と 10 をそれぞれ特定ガイドでは、次を参照してください。

* [3D Touch](~/ios/platform/3d-touch.md)
* [Haptic フィードバックの提供](~/ios/user-interface/ios-ui/haptic-feedback.md)

## <a name="related-links"></a>関連リンク

- [iOS (サンプル) を開始するタッチ](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_start)
- [iOS タッチ最終的な (サンプル)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_final)
- [フィンガー ペイント (サンプル)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/FingerPaint)
