---
title: Xamarin iOS アプリでのタッチ処理
description: このドキュメントでは、Xamarin iOS アプリでタッチ、マルチタッチ、ジェスチャ、3D タッチを操作する方法について説明しているガイドにリンクしています。
ms.prod: xamarin
ms.assetid: E3904713-6018-4755-A315-EB045DFB3500
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 01/23/2017
ms.openlocfilehash: db3e66920beb355e0b05df2118cd2645c602f0d5
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91433912"
---
# <a name="handling-touch-in-xamarinios-apps"></a>Xamarin iOS アプリでのタッチ処理

他のモバイルプラットフォームと同様に、iOS にはタッチを処理するさまざまな方法が用意されています。 マルチタッチ (画面上の多数の接点) と複雑なジェスチャをサポートできます。 このガイドでは、iOS でのタッチとジェスチャの実装の particularities に加えて、いくつかの概念について説明します。

iOS では、クラスのタッチデータをカプセル化し `UITouch` ています。これは、一連のメソッドを通じてアプリケーションで使用でき `UIResponder` ます。 アプリケーションは、およびのサブクラスでこれらのメソッドをオーバーライドでき `UIView` `UIViewController` ます。どちらもから継承さ `UIResponder` れます。

タッチデータをキャプチャするだけでなく、iOS はジェスチャへのタッチパターンを解釈するための手段を提供します。 これらのジェスチャレコグナイザーは、イメージの回転やページのめくりなど、アプリケーション固有のコマンドを解釈するために使用できます。 iOS には、最小限のコードを追加した一般的なジェスチャを処理するクラスの豊富なコレクションが用意されています。

タッチ認識とジェスチャレコグナイザーのどちらを選択するかは、混乱を招く可能性があります。 このガイドでは、一般にジェスチャレコグナイザーに設定することをお勧めします。 ジェスチャレコグナイザーは不連続クラスとして実装されます。これにより、関心の分離とカプセル化の向上が実現します。 これにより、さまざまなビュー間でロジックを共有することが簡単になり、記述されたコードの量を最小限に抑えることができます。

ただし、低いレベルのタッチ処理を使用する必要があり、複数の指を追跡する場合もあります。たとえば、指ペイントプログラムを作成する場合などです。

## <a name="sections"></a>セクション

- [iOS でのタッチ](touch-in-ios.md)
- [チュートリアル: iOS でのタッチの使用](ios-touch-walkthrough.md)
- [マルチタッチ追跡](touch-tracking.md)

このガイドは、iOS でのタッチの概要として機能します。 Ios での3D タッチと Haptic フィードバックの使用の詳細については、ios 9 と10で導入されました。以下の特定のガイドを参照してください。

- [3D Touch](~/ios/platform/3d-touch.md)
- [Haptic フィードバックの提供](~/ios/user-interface/ios-ui/haptic-feedback.md)

## <a name="related-links"></a>関連リンク

- [iOS タッチの最終版 (サンプル)](/samples/xamarin/ios-samples/applicationfundamentals-touch-final)
- [FingerPaint (サンプル)](/samples/xamarin/ios-samples/applicationfundamentals-fingerpaint)