---
title: macOS ユーザー インターフェイス
description: この記事には、さまざまな macOS UI コントロールについて説明するガイドへのリンクがします。
ms.prod: xamarin
ms.assetid: 876B6EC2-E158-43F2-B9C9-03F54F3D2A49
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/27/2018
ms.openlocfilehash: d40faa29f2fe278377bf4eae42a032f3dc9086ab
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="macos-user-interface"></a>macOS ユーザー インターフェイス

_この記事には、さまざまな macOS UI コントロールについて説明するガイドへのリンクがします。_

アクセス権がある Xamarin.Mac アプリケーションでは、c# と .NET で作業するときに、同じユーザー インターフェイス コントロールで作業する開発者*Objective C*と*Xcode*はします。 Xamarin.Mac は、Xcode と直接統合を使用すると Xcode の_インターフェイス ビルダー_を作成し、ユーザー インターフェイスを維持 (または必要に応じて c# コードで直接作成すること)。

以下に示すガイドでは、アプリケーションで Xamarin.Mac macOS UI 要素の操作に関する詳細情報を提供します。 作業することを強くお勧め、[こんにちは, Mac](~/mac/get-started/hello-mac.md)具体的には、最初の記事、 [Xcode とインターフェイスのビルダーの概要を](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder)と[コンセントとアクション](~/mac/get-started/hello-mac.md#Outlets_and_Actions)セクションでは、これとは、主な概念とすべての記事で使用する方法について説明します。

確認することも、 [c# を公開するクラス/OBJECTIVE-C メソッド](~/mac/internals/how-it-works.md#exposing-c-classes--methods-to-objective-c)のセクション、 [Xamarin.Mac 内部](~/mac/internals/how-it-works.md)について説明しますと同様に、ドキュメント、`Register`と`Export`Objective C のオブジェクトと UI 要素に、c# クラスをワイヤ アップに使用する属性。

## <a name="windowsmacuser-interfacewindowmd"></a>[Windows](~/mac/user-interface/window.md)

この記事では、windows と Xamarin.Mac アプリケーション内のパネルでの作業について説明します。 作成してウィンドウと Xcode とインターフェイスのビルダー、読み込みのウィンドウ内のパネルと .storyboard または .xib ファイルからのパネルを維持する、windows を使用して c# コードでの windows への応答方法を説明します。

## <a name="dialogsmacuser-interfacedialogmd"></a>[ダイアログ](~/mac/user-interface/dialog.md)

この記事では、ダイアログ ボックスとモーダル ウィンドウ Xamarin.Mac アプリケーションでの操作について説明します。 これは、作成し、Xcode とインターフェイスのビルダー、標準のダイアログを使用し、表示と c# コードでの windows への応答でモーダル ウィンドウの管理について説明します。

## <a name="alertsmacuser-interfacealertmd"></a>[アラート](~/mac/user-interface/alert.md)

この記事では、Xamarin.Mac アプリケーションでアラートの使用について説明します。 作成して c# コードからのアラートを表示してアラートへの対応方法を説明します。

## <a name="menusmacuser-interfacemenumd"></a>[メニュー](~/mac/user-interface/menu.md)

メニューは、Mac アプリケーションのユーザー インターフェイスのさまざまな部分で使用します。ポップアップ メニューおよびコンテキスト メニューを画面の上部にあるアプリケーションのメイン メニューから表示できる任意の場所のウィンドウでします。 メニューは、Mac アプリケーションのユーザー エクスペリエンスの不可欠な部分です。 この記事と Cocoa メニュー Xamarin.Mac アプリケーションでの作業について説明します。

## <a name="standard-controlsmacuser-interfacestandard-controlsmd"></a>[標準コントロール](~/mac/user-interface/standard-controls.md)

ボタン、ラベル、テキスト フィールド、チェック ボックス、および Xamarin.Mac アプリケーションでセグメント化されたコントロールなどの標準の AppKit コントロールを使用します。 このガイドでは、Xcode のインターフェイスのビルダーでのユーザー インターフェイスのデザインに追加すること、コンセントと操作を使用してコードには公開および c# コードで AppKit コントロールの操作について説明します。

## <a name="toolbarsmacuser-interfacetoolbarmd"></a>[ツールバー](~/mac/user-interface/toolbar.md)

この記事では、ツールバーを Xamarin.Mac アプリケーションでの作業について説明します。 作成して、Xcode とインターフェイスのビルダーのツールバーを維持する方法を説明するコード コンセントとアクションを使用して、有効化しツールバー項目を無効にするおよび c# コードでツールバー項目に最後に応答してツールバー項目を公開する方法です。

## <a name="table-viewsmacuser-interfacetable-viewmd"></a>[テーブルのビュー](~/mac/user-interface/table-view.md)

この記事では、表形式ビュー Xamarin.Mac アプリケーションでの操作について説明します。 作成して、Xcode とインターフェイスのビルダーでテーブルのビューを維持する方法を説明テーブル ビューを公開するには、コードをコンセントとアクションを使用する項目、テーブルのビューを設定して c# コードでのテーブル ビューの項目に応答します。

## <a name="outline-viewsmacuser-interfaceoutline-viewmd"></a>[アウトライン ビュー](~/mac/user-interface/outline-view.md)

この記事では、Xamarin.Mac アプリケーションでのアウトライン ビューでの作業について説明します。 作成して、Xcode とインターフェイスのビルダーのアウトライン表示を維持する方法を説明、アウトライン表示を公開するには、コードをコンセントとアクションを使用する項目、アウトライン モードを設定して c# コードでアイテムの表示を説明する応答します。

## <a name="source-listsmacuser-interfacesource-listmd"></a>[ソース リスト](~/mac/user-interface/source-list.md)

この記事では、ソース リスト Xamarin.Mac アプリケーションでの作業について説明します。 作成して、Xcode とインターフェイスのビルダーのソースの一覧を維持する方法を説明コンセントとアクションを使用し、ソース リストを設定し、c# コードでソース一覧の項目への応答コードをソース一覧の項目を公開する方法です。

## <a name="collection-viewsmacuser-interfacecollection-viewmd"></a>[コレクション ビュー](~/mac/user-interface/collection-view.md)

この記事では、アプリケーションで Xamarin.Mac コレクション ビューの使用について説明します。 作成して、Xcode とインターフェイスのビルダー コレクション ビューを維持する方法を説明コレクション ビューを公開するには、コードをコンセントとアクションを使用する項目、コレクション ビューを設定してコレクション ビューの c# コードで応答します。

## <a name="creating-custom-controlsmacuser-interfacecustom-controlsmd"></a>[カスタム コントロールの作成](~/mac/user-interface/custom-controls.md)

ここでは、カスタム ユーザー インターフェイス コントロールの作成について説明 (から継承することによって`NSControl`)、コントロールは、カスタムのインターフェイスを描画し、Xcode のインターフェイスのビルダーを使用できるカスタムの動作を作成します。

## <a name="mac-samples-gallery"></a>Mac サンプル ギャラリー

見ることもお勧め、 [Mac サンプル ギャラリー](https://developer.xamarin.com/samples/mac/all/)です。 これには、さまざまな地上から Xamarin.Mac プロジェクトを迅速に取得するのに役立つ、すぐに使用できるコードが含まれます。

## <a name="related-links"></a>関連リンク

- [macOS ヒューマン インターフェイス ガイドライン](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
