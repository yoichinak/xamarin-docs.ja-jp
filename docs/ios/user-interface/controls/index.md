---
title: Xamarin.iOS でのユーザー インターフェイス コントロール
description: このドキュメントは、さまざまな iOS ユーザー インターフェイス コントロール Xamarin.iOS の開発者が利用できるについて説明するガイドにリンクしています。 リンクされたコンテンツは、アラート、ボタン、コレクション ビュー、イメージ、手動カメラ コントロール、マップ、ラベル、ピッカー、日付の選択、および詳細について説明します。
ms.prod: xamarin
ms.assetid: C00EA232-ADCC-42AD-BF86-B526414A21C6
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: fab653eb24b504fa17e6f2eac32915fd88580817
ms.sourcegitcommit: 2eb8961dd7e2a3e06183923adab6e73ecb38a17f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/11/2019
ms.locfileid: "66827829"
---
# <a name="user-interface-controls-in-xamarinios"></a>Xamarin.iOS でのユーザー インターフェイス コントロール

このドキュメントでは、いくつかの最も一般的な iOS ユーザー インターフェイス コントロールとその使用方法を紹介します。

## <a name="alertsalertsmd"></a>[アラート](alerts.md)

IOS 8 以降、UIAlertController が完成した置換 UIActionSheet とうち UIAlertView 両方が非推奨となりました。

## <a name="buttonsbuttonsmd"></a>[ボタン](buttons.md)

UIButton クラスは、iOS 画面のボタンのさまざまな異なるスタイルを表すために使用されます。 このセクションでは、iOS のボタンを使用するためのさまざまなオプションについて説明します。

## <a name="collection-viewsuicollectionviewmd"></a>[コレクション ビュー](uicollectionview.md)

コレクション ビューで使用できる、`UICollectionView`クラスは、iOS 6 のレイアウトを使用して、画面で、プレゼンテーションを複数の項目を紹介する新しい概念です。 データを提供するためのパターンを`UICollectionView`を項目を作成し、同じ委任とデータ ソースの iOS 開発でよく使用されるパターンこれら項目に従って操作します。

## <a name="imagesimagemd"></a>[イメージ](image.md)

アプリにイメージを追加するには、2 つの手順が必要です: 最初に、プロジェクトにイメージを追加次に、コントロールと、画面に表示するコードを追加します。 参照してください、[イメージを操作](~/ios/app-fundamentals/images-icons/index.md)Xamarin.iOS での画像処理のカバレッジの詳細情報の記事。

## <a name="manual-camera-controlsintro-to-manual-camera-controlsmd"></a>[手動カメラ コントロール](intro-to-manual-camera-controls.md)

提供される、手動カメラ コントロール、`AVFoundation Framework`では、iOS 8、iOS デバイスのカメラを完全に制御を実行するモバイル アプリケーションを許可します。 この細かいレベルの制御は、プロフェッショナル レベルのカメラ アプリケーションを作成し、静止画像またはビデオを作成中に、カメラのパラメーターを調整することにより、アーティストのコンポジションを提供できます。

## <a name="mapsios-mapsindexmd"></a>[マップ](ios-maps/index.md)

マップは、すべての最新のモバイル オペレーティング システムで一般的な機能です。 iOS では、Map Kit フレームワークを通じてネイティブ マッピング サポートを提供します。 マップのキット、アプリケーションは、リッチで対話型のマップを簡単に追加できます。 これらのマップは、さまざまな、マップ上の場所をマークする注釈を追加して、任意図形のグラフィックスのオーバーレイなどの方法でカスタマイズできます。 Map Kit もデバイスの現在の場所を表示するための組み込みのサポートしています。

## <a name="labelslabelsmd"></a>[ラベル](labels.md)

`UILabel`コントロールが単一および複数の行を表示するための読み取り専用のテキスト。

## <a name="pickers-and-date-pickerspickermd"></a>[ピッカーおよび日付の選択](picker.md)

ピッカー コントロールは、強調表示されている選択した値を持つ値のスクロール可能な一覧を含む 'ホイールに似た' コントロールを表示します。 ユーザーは、必要なオプションを選択するホイールを回転します。

特定のユーザーが 1 つ場合ピッカーを日付または時刻を設定することです。 この Apple では、カスタム UIDatePicker と呼ばれる UIPickerView クラスのサブクラスを作成しました。

## <a name="progress-and-activity-indicatorsprogress-activity-indicatormd"></a>[進行状況とアクティビティのインジケーター](progress-activity-indicator.md)

iOS では、アプリでの進行状況を示す 2 つの主な方法が用意されています。アクティビティのインジケーター (など、特定_ネットワーク_アクティビティのインジケーター) と進行状況バー。

## <a name="search-barssearchbarmd"></a>[検索バー](searchbar.md)

値の一覧を検索して、UISearchBar です。 

## <a name="sliders-switches-and-segmented-controlsslider-switch-segmented-controlsmd"></a>[スライダー、スイッチ、およびセグメント付きコントロール](slider-switch-segmented-controls.md)

スライダー コントロールには、数値の範囲内の単純な選択ができるようにします。 iOS を使用して、`UISwitch`ブール値の入力で他のプラットフォームでラジオ ボタンを表すことができます。 セグメント化されたコントロールは、オプションの数が少ない対話をユーザーに許可する適切な手段です。

## <a name="stack-viewuistackviewmd"></a>[スタック ビュー](uistackview.md)

スタック ビュー コントロール (`UIStackView`) iOS デバイスの向きや画面サイズに動的に応答する水平方向または垂直方向に、サブビューのスタックを管理するには、自動レイアウトとサイズ クラスの機能を利用しています。

## <a name="tables-and-cellstablesindexmd"></a>[テーブルとセル](tables/index.md)

彼のセクションでは、作成し、テーブルの表示に使用されるクラスが導入されていますし、Xamarin.iOS で使用する方法の例を紹介します。 既定の外観を使用して、テーブルの場合、編集、および Xamarin iOS Designer を使用してテーブルを視覚的に設計を実装するレイアウトをカスタマイズするには説明します。 場合があります、表示、明らかに (ミュージック アプリ) などの行とそれ以外の時間 (連絡先アプリの場合、またはメッセージ交換でメッセージ アプリでの編集) などのテーブル コントロールを認識することは困難の一覧です。

## <a name="text-inputtext-inputmd"></a>[テキスト入力](text-input.md)

ユーザーのテキスト入力を受け取るためにでは、`UITextField`の単一行の入力と複数行の編集可能なテキストの UITextView します。 これらのコントロールのいずれかの画面にドラッグして、初期テキストを設定する をダブルクリックします。

## <a name="tab-bars-and-tab-bar-controllerscreating-tabbed-applicationsmd"></a>[タブ バーとタブ バー コント ローラー](creating-tabbed-applications.md)

タブ ナビゲーション UI を使用して iOS アプリケーションは、UITabBarController クラスを使用して構築されます。 この記事ではいくつかのコント ローラーとビューを含むタブ付きアプリケーションを設定する方法について説明します。 できない場合に、ルートのコント ローラーなど、ログイン画面の後に、UITabBarController を読み込む方法を説明します。

## <a name="web-viewsuiwebviewmd"></a>[Web ビュー](uiwebview.md)

この記事で紹介 Apple によって提供される次の 3 つの Web ビューの各: `UIWebView`、 `WKWebview`、および`SFSafariViewController`、これらの類似点と相違点、および使用方法。

## <a name="related-links"></a>関連リンク

- [コントロール (サンプル)](https://developer.xamarin.com/samples/monotouch/Controls/)
