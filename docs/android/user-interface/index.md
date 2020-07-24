---
title: Xamarin Android を使用したユーザーインターフェイスの作成
description: Xamarin Android アプリ用のユーザーインターフェイスの作成
ms.prod: xamarin
ms.assetid: F67B7C33-BC53-2BB6-CDA7-16E4AB4A9EFB
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/25/2018
ms.openlocfilehash: 08085877080c954142729e75e88235011c3a1324
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/23/2020
ms.locfileid: "86997138"
---
# <a name="user-interfaces-with-xamarinandroid"></a>Xamarin Android を使用したユーザーインターフェイス

以下のセクションでは、Xamarin Android アプリでユーザーインターフェイスを作成するために使用されるさまざまなツールと構成要素について説明します。

## <a name="android-designer"></a>[Android Designer](~/android/user-interface/android-designer/index.md)

このセクションでは、Android Designer を使用してコントロールを視覚的にレイアウトし、プロパティを編集する方法について説明します。 また、デザイナーを使用して、さまざまな構成 (テーマ、言語、デバイス構成など) でユーザーインターフェイスとリソースを操作する方法についても説明します。また、横や縦などの別のビューをデザインする方法についても説明します。

## <a name="material-theme"></a>[Material Theme](~/android/user-interface/material-theme.md)

*マテリアルテーマ*は、Android でのビューとアクティビティのルックアンドフィールを決定するユーザーインターフェイススタイルです。 マテリアルのテーマは Android に組み込まれているので、システム UI やアプリケーションによって使用されます。 このガイドでは、素材のデザインの原則と、組み込みの素材テーマまたはカスタムテーマを使用してアプリのテーマを設定する方法について説明します。

## <a name="user-profile"></a>[ユーザー プロファイル](~/android/user-interface/user-profile.md)

このガイドでは、デバイス所有者の名前や電話番号などの連絡先データを含む、デバイスの所有者の個人プロファイルにアクセスする方法について説明します。

## <a name="splash-screen"></a>[スプラッシュスクリーン](~/android/user-interface/splash-screen.md)

Android アプリの起動には時間がかかります。特に、アプリがデバイスで最初に起動されるときです。 スプラッシュスクリーンでは、ユーザーに開始の進行状況が表示される場合があります。 このガイドでは、アプリのスプラッシュスクリーンを作成する方法について説明します。

## <a name="layouts"></a>[レイアウト](~/android/user-interface/layouts/index.md)

レイアウトは、ユーザーインターフェイスのビジュアル構造を定義するために使用されます。
`ListView`やなどのレイアウト `RecyclerView` は、Android アプリケーションの最も基本的な構成要素です。 通常、レイアウトではを使用して、レイアウトの `Adapter` データ項目を設定するために使用される基になるデータへのレイアウトのブリッジとして機能します。 このセクションでは、、、、、などのレイアウトの使用方法について説明し `LinearLayout` `RelativeLayout` `TableLayout` `RecyclerView` `GridView` ます。

## <a name="controls"></a>[コントロール](~/android/user-interface/controls/index.md)

Android コントロール (*ウィジェット*とも呼ばれます) は、ユーザーインターフェイスの構築に使用する UI 要素です。 このセクションでは、ボタン、ツールバー、日付/時刻ピッカー、カレンダー、spinners、スイッチ、ポップアップメニュー、ポケットベルの表示、web ビューなどのコントロールの使用方法について説明します。
