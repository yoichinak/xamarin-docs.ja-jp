---
title: Xamarin. Mac の macOS ユーザーインターフェイスコントロール
description: このドキュメントは、Xamarin の開発者が使用できるさまざまなユーザーインターフェイスコントロールについて説明しているガイドにリンクしています。 リンクされたコンテンツは、windows、ダイアログ、アラート、メニュー、ツールバー、テーブルビュー、アウトラインビューなどを確認できます。
ms.prod: xamarin
ms.assetid: 876B6EC2-E158-43F2-B9C9-03F54F3D2A49
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/27/2018
ms.openlocfilehash: 7f5303cd63c6ff1433b56b3f47b67d3925b1d1e1
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032777"
---
# <a name="macos-user-interface-controls-in-xamarinmac"></a>Xamarin. Mac の macOS ユーザーインターフェイスコントロール

_この記事では、さまざまな macOS UI コントロールについて説明しているガイドにリンクしています。_

Xamarin. Mac C#アプリケーションでと .net を使用する場合、 *Xcode および*で作業している開発者が行う*のと同じ*ユーザーインターフェイスコントロールにアクセスできます。 Xcode は直接統合されているため、Xcode の_Interface Builder_を使用してユーザーインターフェイスを作成および管理できます (また、必要C#に応じて、コード内で直接作成することもできます)。

以下のガイドでは、Xamarin. Mac アプリケーションで macOS UI 要素を操作する方法について詳しく説明しています。 最初に、 [Hello, Mac](~/mac/get-started/hello-mac.md)の記事を使用して作業することを強くお勧めします。具体的には、 [Xcode と Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder)および[アウトレットとアクション](~/mac/get-started/hello-mac.md#outlets-and-actions)に関するセクションで説明します。これは、で使用する主要な概念と手法に関するものです。すべての記事。

ここでは、 C#クラスをに接続するために使用する`Register`と`Export`の属性について説明しているため、Xamarin の内部ドキュメントの「[クラス/メソッドを目的の C に公開C# ](~/mac/internals/how-it-works.md#exposing-c-classes--methods-to-objective-c)する」のセクションも参照して[ください。](~/mac/internals/how-it-works.md)目的 C オブジェクトと UI 要素。

## <a name="windowsmacuser-interfacewindowmd"></a>[Windows](~/mac/user-interface/window.md)

この記事では、Xamarin. Mac アプリケーションでのウィンドウとパネルの使用について説明します。 Xcode と Interface Builder でのウィンドウとパネルの作成と保守、ストーリーボードまたは xib ファイルからのウィンドウとパネルの読み込み、windows の使用、およびコードC#での windows への応答について説明します。

## <a name="dialogsmacuser-interfacedialogmd"></a>[ダイアログ](~/mac/user-interface/dialog.md)

この記事では、Xamarin. Mac アプリケーションでのダイアログとモーダルウィンドウの使用について説明します。 Xcode と Interface Builder でのモーダルウィンドウの作成と保守、標準ダイアログの操作、およびコードでのC#ウィンドウの表示と応答について説明します。

## <a name="alertsmacuser-interfacealertmd"></a>[アラート](~/mac/user-interface/alert.md)

この記事では、Xamarin. Mac アプリケーションでのアラートの使用について説明します。 この記事では、コードからC#のアラートの作成と表示、およびアラートへの対応について説明します。

## <a name="menusmacuser-interfacemenumd"></a>[メニュー](~/mac/user-interface/menu.md)

メニューは、Mac アプリケーションのユーザーインターフェイスのさまざまな部分で使用されます。画面の上部にあるアプリケーションのメインメニューから、ポップアップメニューと、ウィンドウ内の任意の場所に表示されるコンテキストメニュー。 メニューは、Mac アプリケーションのユーザー エクスペリエンスにとって不可欠な部分です。 この記事では、Xamarin. Mac アプリケーションでの Cocoa メニューの使用について説明します。

## <a name="standard-controlsmacuser-interfacestandard-controlsmd"></a>[標準コントロール](~/mac/user-interface/standard-controls.md)

Xamarin. Mac アプリケーションで、ボタン、ラベル、テキストフィールド、チェックボックス、セグメント化されたコントロールなど、標準的な AppKit コントロールを操作します。 このガイドでは、Xcode の Interface Builder のユーザーインターフェイス設計に追加し、アウトレットとアクションを使用してコードに公開し、コードでC# appkit コントロールを操作する方法について説明します。

## <a name="toolbarsmacuser-interfacetoolbarmd"></a>[ツールバー](~/mac/user-interface/toolbar.md)

この記事では、Xamarin. Mac アプリケーションでのツールバーの使用について説明します。 Xcode と Interface Builder でのツールバーの作成と保守、アウトレットとアクションを使用してツールバー項目をコードに公開する方法、ツールバー項目を有効または無効C#にする方法、最後にコード内のツールバー項目に応答する方法について説明します。

## <a name="table-viewsmacuser-interfacetable-viewmd"></a>[テーブルビュー](~/mac/user-interface/table-view.md)

この記事では、Xamarin. Mac アプリケーションでのテーブルビューの使用について説明します。 Xcode および Interface Builder でのテーブルビューの作成と管理、アウトレットとアクションを使用してテーブルビューアイテムをコードに公開する方法、テーブルビューの設定、コード内のC#テーブルビューアイテムへの応答について説明します。

## <a name="outline-viewsmacuser-interfaceoutline-viewmd"></a>[アウトラインビュー](~/mac/user-interface/outline-view.md)

この記事では、Xamarin. Mac アプリケーションでアウトラインビューを使用する方法について説明します。 Xcode と Interface Builder でのアウトライン表示の作成と保守、アウトレットとアクションを使用してアウトラインビューアイテムをコードに公開する方法、アウトラインビューの設定、コードのC#アウトラインビューアイテムへの応答について説明します。

## <a name="source-listsmacuser-interfacesource-listmd"></a>[ソースリスト](~/mac/user-interface/source-list.md)

この記事では、Xamarin. Mac アプリケーションでのソースリストの使用について説明します。 Xcode と Interface Builder でのソースリストの作成と管理、アウトレットとアクションを使用してソースリスト項目をコードに公開する方法、ソースリストの設定、コード内C#のソースリスト項目への応答について説明します。

## <a name="collection-viewsmacuser-interfacecollection-viewmd"></a>[コレクションビュー](~/mac/user-interface/collection-view.md)

この記事では、Xamarin. Mac アプリケーションでのコレクションビューの使用について説明します。 Xcode と Interface Builder でのコレクションビューの作成と管理、アウトレットとアクションを使用してコレクションビューアイテムをコードに公開する方法、コレクションビューの設定、コード内C#のコレクションビューへの応答について説明します。

## <a name="creating-custom-controlsmacuser-interfacecustom-controlsmd"></a>[カスタムコントロールの作成](~/mac/user-interface/custom-controls.md)

この記事では、(`NSControl`から継承することによって) カスタムユーザーインターフェイスコントロールを作成し、コントロールのカスタムインターフェイスを描画し、Xcode の Interface Builder で使用できるカスタムアクションを作成する方法について説明します。

## <a name="mac-samples-gallery"></a>Mac サンプルギャラリー

また、 [Mac サンプルギャラリー](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Mac)を参照することもお勧めします。 これには、Xamarin. Mac プロジェクトをすぐに利用できるようにするための、使いやすいコードが豊富に用意されています。

## <a name="related-links"></a>関連リンク

- [macOS ヒューマン インターフェイス ガイドライン](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
