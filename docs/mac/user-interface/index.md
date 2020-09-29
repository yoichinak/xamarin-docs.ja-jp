---
title: Xamarin. Mac の macOS ユーザーインターフェイスコントロール
description: このドキュメントは、Xamarin の開発者が使用できるさまざまなユーザーインターフェイスコントロールについて説明しているガイドにリンクしています。 リンクされたコンテンツは、windows、ダイアログ、アラート、メニュー、ツールバー、テーブルビュー、アウトラインビューなどを確認できます。
ms.prod: xamarin
ms.assetid: 876B6EC2-E158-43F2-B9C9-03F54F3D2A49
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/27/2018
ms.openlocfilehash: 7ae33883b83a4080903b524ea78e0da009ad0a3a
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91431531"
---
# <a name="macos-user-interface-controls-in-xamarinmac"></a>Xamarin. Mac の macOS ユーザーインターフェイスコントロール

_この記事では、さまざまな macOS UI コントロールについて説明しているガイドにリンクしています。_

Xamarin. Mac アプリケーションで C# と .NET を使用する場合、 *Xcode および*で作業している開発者が行う*のと同じ*ユーザーインターフェイスコントロールにアクセスできます。 Xcode は直接統合されているため、Xcode の _Interface Builder_ を使用してユーザーインターフェイスを作成および管理できます (または、必要に応じて C# コードで直接作成することもできます)。

以下のガイドでは、Xamarin. Mac アプリケーションで macOS UI 要素を操作する方法について詳しく説明しています。 まず、 [Hello, Mac](~/mac/get-started/hello-mac.md) に関する記事を使用することをお勧めします。具体的には、各記事で使用する主要な概念と手法について説明している、「 [Xcode と Interface Builder の概要](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) 」と「 [アウトレットとアクション](~/mac/get-started/hello-mac.md#outlets-and-actions) 」セクションをご覧ください。

C# クラスを目的の C オブジェクトと UI 要素に接続するために使用される属性と属性については、「 [Xamarin. Mac の内部](~/mac/internals/how-it-works.md)ドキュメント」の「 [c# のクラス/メソッドを目的](~/mac/internals/how-it-works.md#exposing-c-classes--methods-to-objective-c)として公開する」セクションを参照して `Register` `Export` ください。

## <a name="windows"></a>[Windows](~/mac/user-interface/window.md)

この記事では、Xamarin. Mac アプリケーションでのウィンドウとパネルの使用について説明します。 Xcode と Interface Builder でのウィンドウとパネルの作成と保守、ストーリーボードまたは xib ファイルからのウィンドウとパネルの読み込み、windows の使用、C# コードでの windows への応答について説明します。

## <a name="dialogs"></a>[ダイアログ](~/mac/user-interface/dialog.md)

この記事では、Xamarin. Mac アプリケーションでのダイアログとモーダルウィンドウの使用について説明します。 Xcode と Interface Builder でのモーダルウィンドウの作成と保守、標準ダイアログの操作、C# コードでのウィンドウの表示と応答について説明します。

## <a name="alerts"></a>[警告](~/mac/user-interface/alert.md)

この記事では、Xamarin. Mac アプリケーションでのアラートの使用について説明します。 ここでは、C# コードからのアラートの作成と表示、およびアラートへの対応について説明します。

## <a name="menus"></a>[メニュー](~/mac/user-interface/menu.md)

メニューは、Mac アプリケーションのユーザーインターフェイスのさまざまな部分で使用されます。画面の上部にあるアプリケーションのメインメニューから、ポップアップメニューと、ウィンドウ内の任意の場所に表示されるコンテキストメニュー。 メニューは、Mac アプリケーションのユーザー エクスペリエンスにとって不可欠な部分です。 この記事では、Xamarin. Mac アプリケーションでの Cocoa メニューの使用について説明します。

## <a name="standard-controls"></a>[標準コントロール](~/mac/user-interface/standard-controls.md)

Xamarin. Mac アプリケーションで、ボタン、ラベル、テキストフィールド、チェックボックス、セグメント化されたコントロールなど、標準的な AppKit コントロールを操作します。 このガイドでは、Xcode の Interface Builder のユーザーインターフェイス設計に追加し、アウトレットとアクションを使用してコードに公開し、C# コードで AppKit コントロールを使用する方法について説明します。

## <a name="toolbars"></a>[[ツール バー]](~/mac/user-interface/toolbar.md)

この記事では、Xamarin. Mac アプリケーションでのツールバーの使用について説明します。 Xcode と Interface Builder でのツールバーの作成と保守、アウトレットとアクションを使用してツールバー項目をコードに公開する方法、ツールバー項目を有効または無効にする方法、最後に C# コードのツールバー項目に応答する方法について説明します。

## <a name="table-views"></a>[テーブルビュー](~/mac/user-interface/table-view.md)

この記事では、Xamarin. Mac アプリケーションでのテーブルビューの使用について説明します。 Xcode と Interface Builder でのテーブルビューの作成と管理、アウトレットとアクションを使用してテーブルビューアイテムをコードに公開する方法、テーブルビューの設定、C# コードでのテーブルビューアイテムへの応答について説明します。

## <a name="outline-views"></a>[アウトラインビュー](~/mac/user-interface/outline-view.md)

この記事では、Xamarin. Mac アプリケーションでアウトラインビューを使用する方法について説明します。 Xcode と Interface Builder でのアウトライン表示の作成と維持、アウトレットとアクションを使用してアウトラインビュー項目をコードに公開する方法、アウトラインビューの設定、C# コードのアウトラインビュー項目への応答について説明します。

## <a name="source-lists"></a>[ソースリスト](~/mac/user-interface/source-list.md)

この記事では、Xamarin. Mac アプリケーションでのソースリストの使用について説明します。 Xcode と Interface Builder でのソースリストの作成と管理、アウトレットとアクションを使用してソースリスト項目をコードに公開する方法、ソースリストの設定、C# コードでのソースリスト項目への応答について説明します。

## <a name="collection-views"></a>[コレクションビュー](~/mac/user-interface/collection-view.md)

この記事では、Xamarin. Mac アプリケーションでのコレクションビューの使用について説明します。 Xcode と Interface Builder でのコレクションビューの作成と管理、アウトレットとアクションを使用してコレクションビューアイテムをコードに公開する方法、コレクションビューの設定、C# コードでのコレクションビューへの応答について説明します。

## <a name="creating-custom-controls"></a>[カスタムコントロールの作成](~/mac/user-interface/custom-controls.md)

この記事では、カスタムユーザーインターフェイスコントロール (から継承) を作成する方法、 `NSControl` コントロールのカスタムインターフェイスを描画する方法、および Xcode の Interface Builder で使用できるカスタムアクションを作成する方法について説明します。

## <a name="mac-samples-gallery"></a>Mac サンプルギャラリー

また、 [Mac サンプルギャラリー](/samples/browse/?products=xamarin&term=Xamarin.Mac)を参照することもお勧めします。 これには、Xamarin. Mac プロジェクトをすぐに利用できるようにするための、使いやすいコードが豊富に用意されています。

## <a name="related-links"></a>関連リンク

- [macOS ヒューマン インターフェイス ガイドライン](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)