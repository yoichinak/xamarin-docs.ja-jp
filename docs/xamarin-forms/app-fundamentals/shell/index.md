---
title: Xamarin.Forms シェル
description: このガイドでは、ほとんどのアプリケーションが必要とする基本的な機能を提供することで Xamarin.Forms アプリケーションの複雑さを軽減する Xamarin.Forms シェルの使用方法について説明します。
ms.prod: xamarin
ms.assetid: 85B322AA-808F-41B6-953A-5877264AE643
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/28/2019
ms.openlocfilehash: 20ac6ad748e7056f7f8037a73a95de66b9eae3b6
ms.sourcegitcommit: 5f972a757030a1f17f99177127b4b853816a1173
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/21/2019
ms.locfileid: "69888914"
---
# <a name="xamarinforms-shell"></a>Xamarin.Forms シェル

## <a name="introductionintroductionmd"></a>[はじめに](introduction.md)

Xamarin.Forms シェルでは、ほとんどのモバイル アプリケーションが必要としている基本機能を提供することで、モバイル アプリケーション開発の複雑さを軽減します。 これには、一般的なナビゲーション ユーザー エクスペリエンス、URI ベースのナビゲーション体系、および統合された検索ハンドラーが含まれます。

## <a name="create-a-xamarinforms-shell-applicationcreatemd"></a>[Xamarin.Forms シェル アプリケーションを作成する](create.md)

Xamarin.Forms シェル アプリケーションを作成するプロセスは、`Shell` クラスをサブクラス化し、アプリケーションの `App` クラスの `MainPage` プロパティにサブクラス化した `Shell` オブジェクトを設定した後に、サブクラス化した `Shell` クラスにアプリケーションのビジュアル階層を記述する XAML ファイルを作成することです。

## <a name="flyoutflyoutmd"></a>[ポップアップ](flyout.md)

ポップアップは、シェル アプリケーションのルート メニューであり、アイコンから、または画面の横からスワイプして、アクセスすることが可能です。 ポップアップは、オプションのヘッダー、ポップアップ項目、およびオプションのメニュー項目から構成されています。

## <a name="tabstabsmd"></a>[タブ](tabs.md)

シェル アプリケーションにおけるポップアップの次のレベルのナビゲーションは、下部のタブ バーです。 または、アプリケーションのナビゲーション パターンを下部のタブから始めて、ポップアップを使用しないようにすることができます。 どちらの場合も下部のタブに複数のページが含まれる場合は、上部のタブからページをナビゲートできます。

## <a name="page-configurationconfigurationmd"></a>[ページの構成](configuration.md)

`Shell` クラスは、Xamarin.Forms シェル アプリケーションでページの外観を構成するために使用できる添付プロパティを定義します。 これには、ページの色の設定、ナビゲーション バーの無効化、タブ バーの無効化、およびナビゲーション バーでのビューの表示が含まれます。

## <a name="navigationnavigationmd"></a>[ナビゲーション](navigation.md)

シェル アプリケーションでは、設定されたナビゲーション階層に従わなくても、アプリケーション内の任意のページに移動するルートを使用する URI ベースのナビゲーション体系を活用できます。

## <a name="searchsearchmd"></a>[検索](search.md)

シェル アプリケーションでは、各ページの上部に追加できる検索ボックスとして提供される、統合された検索機能を利用できます。

## <a name="lifecyclelifecyclemd"></a>[ライフサイクル](lifecycle.md)

シェル アプリケーションでは Xamarin.Forms のライフサイクルが尊重され、ページが画面に表示されるときに `Appearing` イベントが発生し、ページが画面から消えるときに `Disappearing` イベントが発生します。

## <a name="custom-rendererscustomrenderersmd"></a>[カスタム レンダラー](customrenderers.md)

シェル アプリケーションでは、さまざまなシェル クラスが公開しているプロパティとメソッドを利用して、高度なカスタマイズが可能です。 ただし、プラットフォーム固有のより詳細なカスタマイズが必要な場合は、シェルのカスタム レンダラーを作成することも可能です。
