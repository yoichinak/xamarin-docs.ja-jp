---
title: "[ユーザー インターフェイス]"
description: "この記事には、さまざまな macOS UI コントロールについて説明するガイドへのリンクがします。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 876B6EC2-E158-43F2-B9C9-03F54F3D2A49
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/19/2016
ms.openlocfilehash: a8cb9488849dafc2cd720ecf59d654009a9ad781
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="user-interface"></a>[ユーザー インターフェイス]

_この記事には、さまざまな macOS UI コントロールについて説明するガイドへのリンクがします。_

同じアクセス権がある Xamarin.Mac アプリケーションでは、c# と .NET で作業するときのユーザー インターフェイス コントロールで作業する開発者*OBJECTIVE-C*と*Xcode*はします。 Xamarin.Mac は、Xcode と直接統合を使用すると Xcode の_インターフェイス ビルダー_を作成し、ユーザー インターフェイスを維持 (または必要に応じて c# コードで直接作成すること)。 

以下に示すガイドでは、アプリケーションで Xamarin.Mac macOS UI 要素の操作に関する詳細情報を提供します。 作業することを強くお勧め、[こんにちは, Mac](~/mac/get-started/hello-mac.md)具体的には、最初の記事、 [Xcode とインターフェイスのビルダーの概要を](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder)と[コンセントとアクション](~/mac/get-started/hello-mac.md#Outlets_and_Actions)セクションでは、これとは、主な概念とすべての記事で使用する方法について説明します。

確認することも、 [c# を公開するクラス/OBJECTIVE-C メソッド](~/mac/internals/how-it-works.md)のセクション、 [Xamarin.Mac 内部](~/mac/internals/how-it-works.md)が説明されても、ドキュメント、`Register`と`Export`コマンドObjective C のオブジェクトと UI 要素に、c# クラスをワイヤ アップするために使用します。

## <a name="windowsmacuser-interfacewindowmd"></a>[Windows](~/mac/user-interface/window.md)

この記事では、Windows とパネル Xamarin.Mac アプリケーションでの操作について説明します。 作成して、Windows およびからパネルを読み込んで、Xcode とインターフェイスのビルダーでの Windows とパネルを保持する方法を説明`.storyboard`または`.xib`Windows を使用して、c# コードで Windows を応答ファイル。

## <a name="dialogsmacuser-interfacedialogmd"></a>[ダイアログ](~/mac/user-interface/dialog.md)

この記事では、ダイアログ ボックスとモーダル ウィンドウ Xamarin.Mac アプリケーションでの操作について説明します。 Xcode とインターフェイス ビルダーでのモーダル ウィンドウの作成と維持管理、標準ダイアログの使用、C# コードでのウィンドウの表示とウィンドウへの応答に関する内容が含まれています。

## <a name="alertsmacuser-interfacealertmd"></a>[アラート](~/mac/user-interface/alert.md)

この記事では、アラートがある Xamarin.Mac アプリケーションでの作業について説明します。 C# コードからのアラートの作成と表示、アラートへの応答に関する内容が含まれています。

## <a name="menusmacuser-interfacemenumd"></a>[メニュー](~/mac/user-interface/menu.md)

メニューは、Mac アプリケーションのユーザー インターフェイスのさまざまな部分で使用します。ウィンドウで任意の場所に表示される可能性がポップアップ ウィンドウおよびコンテキスト メニューに、画面の上部にあるアプリケーションのメイン メニューです。 メニューは、Mac アプリケーションのユーザー エクスペリエンスの不可欠な部分です。 この記事では、Xamarin.Mac アプリケーションでの Cocoa メニューの使用について説明しています。

## <a name="standard-controlsmacuser-interfacestandard-controlsmd"></a>[標準コントロール](~/mac/user-interface/standard-controls.md)

Xamarin.Mac アプリケーションでのボタン、ラベル、テキスト フィールド、チェック ボックスをセグメント化されたコントロールなど、標準的な AppKit コントロールを操作します。 このガイドでは、Xcode のインターフェイスのビルダーでのユーザー インターフェイスのデザインに追加すること、コンセントとアクションを使用してコードには公開および c# コードで AppKit コントロールの操作について説明します。

 
## <a name="toolbarsmacuser-interfacetoolbarmd"></a>[ツールバー](~/mac/user-interface/toolbar.md)

この記事では、ツールバーと Xamarin.Mac アプリケーションでの作業について説明します。 Xcode とインターフェイス ビルダーでのツール バーの作成と維持管理、Outlet と Action を使用してツール バー項目をコードに公開する方法、ツール バー項目の有効化と無効化、最後に C# コードでのツール バー項目への応答に関する内容が含まれています。

## <a name="table-viewsmacuser-interfacetable-viewmd"></a>[テーブル ビュー](~/mac/user-interface/table-view.md)

この記事では、Xamarin.Mac アプリケーションでテーブルのビューでの作業について説明します。 作成して、Xcode とインターフェイスのビルダーでテーブルのビューを保持する方法を説明コード コンセントとアクションを使用し、テーブルのビューを設定して、テーブル項目を表示する c# コードで最後に応答する項目を表示するテーブルを公開する方法です。

## <a name="outline-viewsmacuser-interfaceoutline-viewmd"></a>[アウトライン ビュー](~/mac/user-interface/outline-view.md)

この記事では、Xamarin.Mac アプリケーションでのアウトライン ビューでの作業について説明します。 Xcode とインターフェイス ビルダーでのアウトライン ビューの作成と維持管理、Outlet と Action を使用してアウトライン ビュー項目をコードに公開する方法、アウトライン ビュー項目の設定、最後に C# コードでのアウトライン ビュー項目への応答に関する内容が含まれています。

## <a name="source-listsmacuser-interfacesource-listmd"></a>[ソース リスト](~/mac/user-interface/source-list.md)

この記事では、ソース リスト Xamarin.Mac アプリケーションでの作業について説明します。 Xcode とインターフェイス ビルダーでのソース リストの作成と維持管理、Outlet と Action を使用してソース リスト項目をコードに公開する方法、ソース リスト項目の設定、最後に C# コードでのソース リスト項目への応答に関する内容が含まれています。

## <a name="collection-viewsmacuser-interfacecollection-viewmd"></a>[コレクション ビュー](~/mac/user-interface/collection-view.md)

この記事では、コレクション ビューを Xamarin.Mac アプリケーションでの作業について説明します。 Xcode とインターフェイス ビルダーでのコレクション ビューの作成と維持管理、Outlet と Action を使用してコレクション ビュー項目をコードに公開する方法、コレクション ビューの設定、最後に C# コードでのコレクション ビューへの応答に関する内容が含まれています。

## <a name="creating-custom-user-controlsmacuser-interfacecustom-controlsmd"></a>[カスタム ユーザー コントロールを作成します。](~/mac/user-interface/custom-controls.md)

ここでは、カスタム ユーザー インターフェイス コントロールの作成について説明 (から継承することによって`NSControl`)、カスタム コントロールのインターフェイスを描画し、Xcode のインターフェイスのビルダーを使用できるカスタムの動作を作成します。

## <a name="mac-samples-gallery"></a>Mac サンプル ギャラリー

見ることもお勧め、 [Mac サンプル ギャラリー](http://developer.xamarin.com/samples/mac/all/)、豊富な地上から Xamarin.Mac プロジェクトを迅速に取得するのに役立つ、すぐに使用できるコードが含まれています。

## <a name="related-links"></a>関連リンク

- [macOS ヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
