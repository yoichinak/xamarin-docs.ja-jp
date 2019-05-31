---
title: Xamarin.Forms シェル
description: このガイドでは、ほとんどのアプリケーションが必要とする基本的な機能を提供することで Xamarin.Forms アプリケーションの複雑さを軽減する Xamarin.Forms シェルの使用方法について説明します。
ms.prod: xamarin
ms.assetid: 85B322AA-808F-41B6-953A-5877264AE643
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: 7699b39a6df6c64ae9a481d9171f23dc6a8eba57
ms.sourcegitcommit: 9d90a26cbe13ebd106f55ba4a5445f28d9c18a1a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/06/2019
ms.locfileid: "65054192"
---
# <a name="xamarinforms-shell"></a>Xamarin.Forms シェル

![](~/media/shared/preview.png "この API は現在プレリリースです")

## <a name="introductionintroductionmd"></a>[はじめに](introduction.md)

Xamarin.Forms シェルでは、ほとんどのモバイル アプリケーションが必要としている基本機能を提供することで、モバイル アプリケーション開発の複雑さを軽減します。 これには、一般的なナビゲーション ユーザー エクスペリエンス、URI ベースのナビゲーション体系、および統合された検索ハンドラーが含まれます。

## <a name="flyoutflyoutmd"></a>[ポップアップ](flyout.md)

ポップアップは、シェル アプリケーションのルート メニューであり、アイコンから、または画面の横からスワイプして、アクセスすることが可能です。 ポップアップは、オプションのヘッダー、ポップアップ項目、およびオプションのメニュー項目から構成されています。

## <a name="tabstabsmd"></a>[タブ](tabs.md)

ポップアップ後は、シェル アプリケーション内の次のレベルのナビゲーションが下部のタブ バーに表示されます。 タブに複数のページが含まれる場合は、上部のタブからページをナビゲートできます。

## <a name="navigationnavigationmd"></a>[ナビゲーション](navigation.md)

シェル アプリケーションでは、設定されたナビゲーション階層に従わなくても、アプリケーション内の任意のページに移動するルートを使用する URI ベースのナビゲーション体系を活用できます。

## <a name="searchsearchmd"></a>[検索](search.md)

シェル アプリケーションでは、各ページの上部に追加できる検索ボックスとして提供される、統合された検索機能を利用できます。

## <a name="custom-rendererscustomrenderersmd"></a>[カスタム レンダラー](customrenderers.md)

シェル アプリケーションでは、さまざまなシェル クラスが公開しているプロパティとメソッドを利用して、高度なカスタマイズが可能です。 ただし、プラットフォーム固有のより詳細なカスタマイズが必要な場合は、シェルのカスタム レンダラーを作成することも可能です。
