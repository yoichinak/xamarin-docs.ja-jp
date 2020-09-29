---
title: Xamarin のユーザーインターフェイスコントロール
description: このドキュメントでは、Xamarin の開発者が使用できるさまざまな iOS ユーザーインターフェイスコントロールについて説明しているガイドにリンクしています。 リンクされたコンテンツは、アラート、ボタン、コレクションビュー、画像、手動カメラコントロール、マップ、ラベル、ピッカー、日付のピッカーなどについて説明します。
ms.prod: xamarin
ms.assetid: C00EA232-ADCC-42AD-BF86-B526414A21C6
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 336e2468a3532b300697ed7fe596864e1bcf9622
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91433963"
---
# <a name="user-interface-controls-in-xamarinios"></a>Xamarin のユーザーインターフェイスコントロール

このドキュメントでは、iOS のユーザーインターフェイスの一般的なコントロールとその使用方法について説明します。

## <a name="alerts"></a>[警告](alerts.md)

IOS 8 以降では、UIAlertController は、Uialertcontroller と Uialertcontroller が両方とも非推奨とされるようになりました。

## <a name="buttons"></a>[ボタン](buttons.md)

UIButton クラスは、iOS の画面のさまざまな異なるスタイルのボタンを表すために使用されます。 このセクションでは、iOS でボタンを操作するためのさまざまなオプションについて説明します。

## <a name="collection-views"></a>[コレクション ビュー](uicollectionview.md)

クラスで使用できるコレクションビュー `UICollectionView` は、レイアウトを使用して複数の項目を画面に表示する、iOS 6 の新しい概念です。 にデータを提供 `UICollectionView` して項目を作成し、それらの項目と対話するパターンは、iOS 開発でよく使用されるものと同じ委任とデータソースパターンに従います。

## <a name="images"></a>[イメージ](image.md)

アプリにイメージを追加するには、次の2つの手順を実行する必要があります。まず、イメージをプロジェクトに追加します。次に、コントロールとコードを追加して、画面に表示します。 Xamarin. iOS のイメージ処理の詳細については、 [イメージの操作](~/ios/app-fundamentals/images-icons/index.md) に関する記事を参照してください。

## <a name="manual-camera-controls"></a>[手動カメラ コントロール](intro-to-manual-camera-controls.md)

IOS 8 のに用意されている手動カメラコントロールは、 `AVFoundation Framework` モバイルアプリケーションが ios デバイスのカメラを完全に制御できるようにします。 この細かい制御レベルを使用して、プロフェッショナルレベルのカメラアプリケーションを作成し、静止画像やビデオを撮影しながら、カメラのパラメーターを調整することでアーティストコンポジションを提供できます。

## <a name="maps"></a>[マップ](ios-maps/index.md)

Maps は、すべての最新のモバイルオペレーティングシステムで共通の機能です。 iOS では、マップキットフレームワークによってネイティブにマッピングがサポートされています。 マップキットを使用すると、アプリケーションは、リッチでインタラクティブなマップを簡単に追加できます。 これらのマップは、マップ上の場所をマークする注釈を追加したり、任意の図形のグラフィックをオーバーレイしたりするなど、さまざまな方法でカスタマイズできます。 マップキットには、デバイスの現在の場所を表示するためのサポートが組み込まれています。

## <a name="labels"></a>[ラベル](labels.md)

コントロールは、 `UILabel` 単一行および複数行の読み取り専用のテキストを表示するために使用されます。

## <a name="pickers-and-date-pickers"></a>[ピッカーと日付のピッカー](picker.md)

ピッカーコントロールは、選択した値が強調表示されている値のスクロール可能なリストを含む "ホイールのような" コントロールを表示します。 ユーザーは、ホイールを回転させて、必要なオプションを選択します。

の1つの特定のユーザーケースでは、日付/時刻を設定することができます。 この機能を提供するために、この Apple は UIDatePicker という UIPickerView クラスのカスタムサブクラスを作成しました。

## <a name="progress-and-activity-indicators"></a>[進行状況とアクティビティのインジケーター](progress-activity-indicator.md)

iOS には、アプリの進行状況を示す2つの主な方法が用意されています。アクティビティインジケーター (特定の _ネットワーク_ アクティビティインジケーターを含む) と進行状況バーです。

## <a name="search-bars"></a>[検索バー](searchbar.md)

UISearchBar は、値のリストを検索するために使用されます。 

## <a name="sliders-switches-and-segmented-controls"></a>[スライダー、スイッチ、およびセグメント付きコントロール](slider-switch-segmented-controls.md)

スライダーコントロールを使用すると、範囲内の数値を簡単に選択できます。 iOS では、 `UISwitch` 他のプラットフォームのラジオボタンで表すことができるブール型の入力としてを使用します。 セグメント化されたコントロールは、ユーザーが少数のオプションと対話できるようにするための整理された方法です。

## <a name="stack-view"></a>[スタック ビュー](uistackview.md)

スタックビューコントロール () は、 `UIStackView` 自動レイアウトクラスとサイズクラスの機能を活用して、マルチビューのスタックを水平方向または垂直方向に管理します。これは、iOS デバイスの向きと画面サイズに動的に反応します。

## <a name="tables-and-cells"></a>[テーブルとセル](tables/index.md)

ここでは、テーブルの作成と表示に使用されるクラスについて説明し、Xamarin での使用方法の例を示します。 テーブルの既定の外観を使用する方法、レイアウトをカスタマイズする方法、編集を実装する方法、Xamarin iOS Designer を使用してテーブルを視覚的にデザインする方法について説明します。 場合によっては、表示が明らかに行のリスト (ミュージックアプリなど) である場合や、テーブルコントロールを認識することが困難な場合があります (連絡先アプリでの編集、メッセージアプリのメッセージ交換など)。

## <a name="text-input"></a>[テキスト入力](text-input.md)

ユーザーテキスト入力の受け入れは、 `UITextField` 複数行の編集可能なテキスト用の単一行入力と UITextView のを使用して行われます。 これらのコントロールのいずれかを画面にドラッグしてダブルクリックすると、初期のテキストを設定できます。

## <a name="tab-bars-and-tab-bar-controllers"></a>[タブ バーとタブ バー コント ローラー](creating-tabbed-applications.md)

タブナビゲーション UI を使用する iOS アプリケーションは、UITabBarController クラスを使用して構築されます。 この記事では、複数のコントローラーとビューを含むタブ付きアプリケーションを設定する方法について説明します。 次に、ログイン画面の後など、ルートコントローラーではないときに UITabBarController を読み込む方法を確認します。

## <a name="web-views"></a>[Web ビュー](webview.md)

この記事では、Apple によって提供される web ビュー、その `WKWebview` `SFSafariViewController` 類似点と相違点、およびそれらの使用方法について説明します。

## <a name="related-links"></a>関連リンク

- [コントロール (サンプル)](/samples/xamarin/ios-samples/controls)