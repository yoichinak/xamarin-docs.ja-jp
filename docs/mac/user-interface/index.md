---
title: Xamarin.Mac の macOS ユーザー インターフェイス コントロール
description: このドキュメントは、Xamarin.Mac 開発者が利用できる各種のユーザー インターフェイス コントロールについて説明するガイドにリンクしています。 リンクされたコンテンツは、windows、ダイアログ、アラート、メニューのツールバー、テーブルのビュー、アウトライン ビュー、および詳細の説明です。
ms.prod: xamarin
ms.assetid: 876B6EC2-E158-43F2-B9C9-03F54F3D2A49
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/27/2018
ms.openlocfilehash: b2392f05a03015f903918f15013919be14b99292
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/13/2018
ms.locfileid: "40250988"
---
# <a name="macos-user-interface-controls-in-xamarinmac"></a>Xamarin.Mac の macOS ユーザー インターフェイス コントロール

_この記事では、さまざまな macOS UI コントロールについて説明するガイドを使用するページにリンクしています。_

アクセス権がある、Xamarin.Mac アプリケーションで c# と .NET を使用する場合に、同じユーザー インターフェイス コントロールで作業する開発者*Objective C*と*Xcode*は。 Xamarin.Mac は直接 Xcode と統合、ためには、Xcode を使用して_Interface Builder_を作成し、ユーザー インターフェイスを維持 (またはに応じて c# コードで直接作成する)。

以下のガイドでは、Xamarin.Mac アプリケーションでの macOS UI 要素の使用に関する詳細情報を提供します。 作業することを強くお勧め、[こんにちは, Mac](~/mac/get-started/hello-mac.md)具体的には、最初の記事、 [Xcode と Interface Builder の概要](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder)と[Outlet と Action](~/mac/get-started/hello-mac.md#outlets-and-actions)ほどのセクションでは、主要な概念とすべての記事で使用する方法について説明します。

見てしたい場合があります、 [c# を公開するクラス/メソッドを OBJECTIVE-C](~/mac/internals/how-it-works.md#exposing-c-classes--methods-to-objective-c)のセクション、 [Xamarin.Mac の内部](~/mac/internals/how-it-works.md)、について説明しますと同様に、ドキュメント、`Register`と`Export`Objective C のオブジェクトと UI 要素を c# クラスをワイヤ アップに使用する属性。

## <a name="windowsmacuser-interfacewindowmd"></a>[Windows](~/mac/user-interface/window.md)

この記事では、ウィンドウとパネルは、Xamarin.Mac アプリケーションでの操作について説明します。 作成する、windows および Xcode と Interface Builder、読み込みのウィンドウ内のパネルとパネル .storyboard または .xib ファイルからのメンテナンスし windows を使用して、c# コードでウィンドウへの応答がについて説明します。

## <a name="dialogsmacuser-interfacedialogmd"></a>[ダイアログ](~/mac/user-interface/dialog.md)

この記事では、ダイアログとモーダル ウィンドウ、Xamarin.Mac アプリケーションでの操作について説明します。 作成および保守 Xcode と Interface Builder、標準のダイアログを使用し、表示と c# コードでの windows 応答でモーダル ウィンドウについて説明します。

## <a name="alertsmacuser-interfacealertmd"></a>[アラート](~/mac/user-interface/alert.md)

この記事では、Xamarin.Mac アプリケーションでのアラートの操作について説明します。 作成、c# コードからのアラートの表示とアラートへの対応について説明します。

## <a name="menusmacuser-interfacemenumd"></a>[メニュー](~/mac/user-interface/menu.md)

メニューを使用して、Mac アプリケーションのユーザー インターフェイスのさまざまな部分でアプリケーションのメイン メニューのポップアップ メニューとコンテキスト メニュー ウィンドウをどこにでも表示できる画面の上部にあります。 メニューは、Mac アプリケーションのユーザー エクスペリエンスの不可欠な部分です。 この記事では、Xamarin.Mac アプリケーションでの Cocoa メニューの操作について説明します。

## <a name="standard-controlsmacuser-interfacestandard-controlsmd"></a>[標準コントロール](~/mac/user-interface/standard-controls.md)

ボタン、ラベル、テキスト フィールド、チェック ボックス、および、Xamarin.Mac アプリケーションでのセグメント付きコントロールなどの標準の AppKit コントロールを使用します。 このガイドでは、Xcode の Interface Builder でのユーザー インターフェイスのデザインに追加すること、outlet と action を使用してコードに公開すること、および c# コードでの AppKit コントロールの操作について説明します。

## <a name="toolbarsmacuser-interfacetoolbarmd"></a>[ツールバー](~/mac/user-interface/toolbar.md)

この記事では、Xamarin.Mac アプリケーションでツールバーの使用について説明します。 作成すると、Xcode と Interface Builder でツールバーを維持管理コード outlet と action を使用して、有効にすると、ツールバーの項目を無効にして、最後に c# コードでのツールバー項目への応答にツールバー項目を公開する方法。

## <a name="table-viewsmacuser-interfacetable-viewmd"></a>[テーブル ビュー](~/mac/user-interface/table-view.md)

この記事では、Xamarin.Mac アプリケーションでのテーブル ビューの使用について説明します。 作成すると、Xcode と Interface Builder では、テーブルのビューを維持管理方法、テーブル ビューを公開する outlet と action を使用してコードに項目をテーブルのビューを設定し、c# コードでのテーブル ビュー項目への応答します。

## <a name="outline-viewsmacuser-interfaceoutline-viewmd"></a>[アウトライン ビュー](~/mac/user-interface/outline-view.md)

この記事では、Xamarin.Mac アプリケーションでのアウトライン ビューの使用について説明します。 作成すると、Xcode と Interface Builder でのアウトライン ビューを維持管理アウトライン ビューを公開する outlet と action を使用してコードに項目を方法、アウトライン ビューを設定し、応答を c# コードでアイテムの表示を説明します。

## <a name="source-listsmacuser-interfacesource-listmd"></a>[ソース リスト](~/mac/user-interface/source-list.md)

この記事では、Xamarin.Mac アプリケーションでのソース リストを使用した作業について説明します。 作成すると、Xcode と Interface Builder では、ソース リストを維持管理 outlet と action を使用し、ソース リストを設定して、c# コードでのソース リスト項目への応答コードをソース リスト項目を公開する方法。

## <a name="collection-viewsmacuser-interfacecollection-viewmd"></a>[コレクション ビュー](~/mac/user-interface/collection-view.md)

この記事では、Xamarin.Mac アプリケーションでのコレクション ビューの使用について説明します。 作成すると、Xcode と Interface Builder では、コレクション ビューを維持管理、コレクション ビューを公開する outlet と action を使用してコードに項目をコレクションのビューの設定とコレクション ビューは、c# コードで応答します。

## <a name="creating-custom-controlsmacuser-interfacecustom-controlsmd"></a>[カスタム コントロールの作成](~/mac/user-interface/custom-controls.md)

この記事では、カスタム ユーザー インターフェイス コントロールの作成では (から継承することによって`NSControl`)、コントロールのカスタム インターフェイスを描画し、Xcode の Interface Builder で使用できるカスタム アクションを作成します。

## <a name="mac-samples-gallery"></a>Mac サンプル ギャラリー

見ることもお勧め、 [Mac サンプル ギャラリー](https://developer.xamarin.com/samples/mac/all/)します。 これには、すぐに使用できるコードを上手 Xamarin.Mac プロジェクトを迅速に取得するのに役立つ豊富なが含まれます。

## <a name="related-links"></a>関連リンク

- [macOS ヒューマン インターフェイス ガイドライン](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
