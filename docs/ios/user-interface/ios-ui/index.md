---
title: iOS のユーザー インターフェイス
description: このドキュメントは、Xamarin.iOS アプリでユーザー インターフェイスを構築する方法を説明するガイドにリンクしています。 リンクのガイドでは、ユーザー インターフェイス オブジェクトやレイアウトのオプションを作成、外観 API について説明します。
ms.prod: xamarin
ms.assetid: 1BB46561-F503-491E-A27C-7878E7EBE00B
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/14/2017
ms.openlocfilehash: efb88ada8a4b4c36dd49de137eb64acd63552968
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61382337"
---
# <a name="user-interfaces-in-ios"></a>iOS のユーザー インターフェイス

## <a name="appearance-apiintroduction-to-the-appearance-apimd"></a>[外観 API](introduction-to-the-appearance-api.md)

iOS は、UIAppearance Api を使用して構成されるユーザー インターフェイス コントロールの多くの視覚属性を使用できます。

## <a name="creating-user-interface-objectsiosuser-interfaceios-uicreating-ui-objectsmd"></a>[ユーザー インターフェイス オブジェクトの作成](~/ios/user-interface/ios-ui/creating-ui-objects.md)

Apple のグループに関連の Xamarin.iOS の名前空間と同等の「フレームワーク」に機能します。 `UIKit` iOS 用のすべてのユーザー インターフェイス コントロールを含む名前空間です。

## <a name="layout-optionsiosuser-interfaceios-uilayout-optionsmd"></a>[レイアウト オプション](~/ios/user-interface/ios-ui/layout-options.md)

これには、ビューがサイズ変更、または回転したときに、レイアウトを制御するための 2 つの異なるメカニズムがあります。自動サイズ調整と自動レイアウト。

## <a name="providing-haptic-feedbackiosuser-interfaceios-uihaptic-feedbackmd"></a>[Haptic フィードバックの提供](~/ios/user-interface/ios-ui/haptic-feedback.md)

この記事では、iOS 10 と Xamarin.iOS でそれらを実装する方法で使用できる haptic フィードバックの新しい型について説明します。

## <a name="working-with-the-ui-threadiosuser-interfaceios-uiui-threadmd"></a>[UI スレッドの操作](~/ios/user-interface/ios-ui/ui-thread.md)

コード変更をユーザー インターフェイス コントロールのメイン (または UI) スレッドのみことをする必要があります。 (コールバックまたはバック グラウンドのスレッド) などの別のスレッドで発生する UI の更新は、画面にレンダリング取得されない可能性があります。 またはクラッシュを起こす可能性があります。




