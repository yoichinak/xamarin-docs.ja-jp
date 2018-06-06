---
title: Xamarin.iOS のユーザー インターフェイス コントロール
description: このドキュメントには、さまざまな iOS ユーザー インターフェイス コントロール Xamarin.iOS 開発者が利用できるを説明するガイドへのリンクがします。 リンクされたコンテンツは、アラート、ボタン、コレクション ビュー、イメージ、カメラの手動のコントロール、マップ、ラベル、ピッカー、日付の選択、および詳細について説明します。
ms.prod: xamarin
ms.assetid: C00EA232-ADCC-42AD-BF86-B526414A21C6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 477cb4e479224b9e795f3460e0f59e0aa2038630
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790043"
---
# <a name="user-interface-controls-in-xamarinios"></a>Xamarin.iOS のユーザー インターフェイス コントロール

このドキュメントでは、いくつかの最も一般的な iOS のユーザー インターフェイス コントロールとその使用方法を紹介します。

## <a name="alertsalertsmd"></a>[アラート](alerts.md)

IOS 8 以降、UIAlertController が完了した交換 UIActionSheet とうち UIAlertView 両方は使用されなくなりました。

## <a name="buttonsbuttonsmd"></a>[ボタン](buttons.md)

UIButton クラスは、iOS の画面でボタンのさまざまな種類のスタイルを表すために使用します。 このセクションでは、iOS でのボタンを使用するためのさまざまなオプションについて説明します。

## <a name="collection-viewsuicollectionviewmd"></a>[コレクション ビュー](uicollectionview.md)

利用可能なコレクション ビュー、`UICollectionView`クラス、レイアウトを使用して、画面上の複数の項目の表示を紹介する iOS 6 での新しい概念です。 データを提供するためのパターン、`UICollectionView`項目を作成し、iOS 開発で使用される一般的なパターンを同じ委任とデータ ソース アイテムに従ってこれらと対話します。

## <a name="imagesimagemd"></a>[イメージ](image.md)

2 つの手順をアプリにイメージを追加することが必要です最初に、イメージをプロジェクトに追加。次に、コントロールと、画面に表示するコードを追加します。 参照してください、[イメージを操作](~/ios/app-fundamentals/images-icons/index.md)カバレッジ イメージ Xamarin.iOS で処理の詳細な記事。

## <a name="manual-camera-controlsintro-to-manual-camera-controlsmd"></a>[手動カメラ コントロール](intro-to-manual-camera-controls.md)

提供されるマニュアル カメラ コントロール、 `AVFoundation Framework` 8、iOS でモバイル アプリケーションを iOS デバイスのカメラを完全に制御を許可します。 プロフェッショナルなレベルのカメラ アプリケーションを作成し、静止画像またはビデオを作成中に、カメラのパラメーターを調整してアーティスト コンポジションを指定するのには、この制御の粒度の細かいレベルを使用できます。

## <a name="mapsios-mapsindexmd"></a>[マップ](ios-maps/index.md)

マップは、すべての最新のモバイル オペレーティング システムで一般的な機能です。 iOS では、マップ キット フレームワークを通じてネイティブ サポートがマッピングされています。 マップ キットを使用して、アプリケーションは、豊富な対話型のマップを簡単に追加できます。 これらのマップは、さまざまな、地図上に場所を示すために注釈を追加して、任意図形のグラフィックスのオーバーレイなどの方法でカスタマイズできます。 マップ キットでは、デバイスの現在の場所を表示するための組み込みサポートもあります。

## <a name="labelslabelsmd"></a>[ラベル](labels.md)

`UILabel`コントロールが単一および複数の行を表示するための読み取り専用のテキスト。

## <a name="pickers-and-date-pickerspickermd"></a>[ピッカーおよび日付の選択](picker.md)

ピッカー コントロールは、スクロール可能な値を強調表示されている選択した値の一覧を含む 'ホイールに似た' コントロールを表示します。 ユーザーは、必要なオプションを選択するホイールを回転します。

1 つの特定のユーザー場合ピッカーを日付または時刻を設定することです。 この Apple の提供には、カスタム UIDatePicker と呼ばれる UIPickerView クラスのサブクラスを作成しました。

## <a name="progress-and-activity-indicatorsprogress-activity-indicatormd"></a>[進行状況とアクティビティのインジケーター](progress-activity-indicator.md)

iOS には、アプリでの進行状況を示すために 2 つの主な方法が用意されています: アクティビティのインジケーター (など、特定_ネットワーク_アクティビティのインジケーター) と進行状況バーです。

## <a name="search-barssearchbarmd"></a>[検索バー](searchbar.md)

UISearchBar を使用すると、値のリストを検索します。 

## <a name="sliders-steppers-and-segmented-controlsslider-switch-segmented-controlsmd"></a>[スライダー、ステッパ、およびセグメント化されたコントロール](slider-switch-segmented-controls.md)

スライダー コントロールでは、数値の範囲内の単純な選択ができます。 iOS を使用して、`UISwitch`ブール値の入力によって他のプラットフォームでラジオ ボタンを表すことができます。 セグメント化されたコントロールは、オプションの数が少ない対話をユーザーに使用できるように整理された方法です。

## <a name="stack-viewuistackviewmd"></a>[スタック ビュー](uistackview.md)

スタック ビュー コントロール (`UIStackView`)、iOS デバイスの向きや画面のサイズを動的に応答する水平方向または垂直方向にサブビューのスタックを管理するには、自動レイアウトおよびサイズ クラスの機能を利用しています。

## <a name="tables-and-cellstablesindexmd"></a>[テーブルとセル](tables/index.md)

彼のセクションでは、作成し、テーブルの表示に使用するクラスが導入されていますし、Xamarin.iOS で使用する方法の例について説明します。 既定の外観を使用して、テーブルの編集、および Xamarin iOS デザイナーを使用してテーブルを視覚的にデザインするを実装する、レイアウトをカスタマイズする場合に適用されます。 場合があります、表示、明らかに (、音楽アプリなど) の行と、テーブルなどのコントロール (連絡先アプリの場合、またはメッセージ アプリにあるメッセージ交換での編集) を認識するが困難である他の時間の一覧です。

## <a name="text-inputtext-inputmd"></a>[テキスト入力](text-input.md)

実現は、ユーザーのテキスト入力を受け入れ、`UITextField`単一行の入力と複数行の編集可能なテキストの UITextView します。 これらのコントロールのいずれかの画面にドラッグし、最初のテキストを設定する をダブルクリックできます。

## <a name="tab-bars-and-tab-bar-controllerscreating-tabbed-applicationsmd"></a>[タブ バーとタブ バー コント ローラー](creating-tabbed-applications.md)

タブ ナビゲーション UI を使用して iOS アプリケーションは、UITabBarController クラスを使用して構築されます。 この記事の内容をいくつかのコント ローラーとビューを含むタブ付きアプリケーションを設定する方法を説明します。 できない場合に、ルートのコント ローラーなど、ログイン画面の後に、UITabBarController を読み込む方法について確認し、します。

## <a name="web-viewsuiwebviewmd"></a>[Web ビュー](uiwebview.md)

この記事の内容について学びます Apple によって提供される 3 つの Web ビューの各: `UIWebView`、 `WKWebview`、および`SFSafariViewController`、それらの類似点と相違点、および使用方法です。

## <a name="related-links"></a>関連リンク

- [コントロール (サンプル)](https://developer.xamarin.com/samples/Controls/)
