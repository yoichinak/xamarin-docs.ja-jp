---
title: Xamarin.Forms のカスタム レンダラー
description: カスタム レンダラーでは、開発者は Xamarin.Forms コントロールの動作と外観をカスタマイズする各プラットフォームのネイティブ コントロールのレンダリングをオーバーライドすることができます。
ms.prod: xamarin
ms.assetid: BF1CF23A-3BC9-4226-92E6-DAEEB91422F1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: c7ae25688b2f8635a9a89318e0b307e58add7a5a
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998745"
---
# <a name="xamarinforms-custom-renderers"></a>Xamarin.Forms のカスタム レンダラー

_Xamarin.Forms のユーザー インターフェイスは、Xamarin.Forms アプリケーションは各プラットフォームの適切なルック アンド フィールを保持する、ターゲット プラットフォームのネイティブ コントロールを使用してレンダリングされます。カスタム レンダラーでは、開発者の各プラットフォームで Xamarin.Forms コントロールの動作と外観をカスタマイズするには、このプロセスをオーバーライドすることができます。_

## <a name="introduction-to-custom-renderersintroductionmd"></a>[カスタム レンダラーの概要](introduction.md)

カスタム レンダラーは、Xamarin.Forms コントロールの動作と外観をカスタマイズするための強力なアプローチを提供します。 小規模なスタイルの変更または高度なプラットフォーム固有のレイアウトと動作のカスタマイズが使用できます。 この記事では、カスタム レンダラーでは、概要を示し、カスタム レンダラーを作成するためのプロセスの概要を説明します。

## <a name="renderer-base-classes-and-native-controlsrenderersmd"></a>[レンダラーの基本クラスおよびネイティブ コントロール](renderers.md)

すべての Xamarin.Forms コントロールには、ネイティブ コントロールのインスタンスを作成する各プラットフォームの付属のレンダラーがあります。 この記事では、レンダラー、および各 Xamarin.Forms ページ、レイアウト、ビュー、およびセルを実装するネイティブ コントロール クラスを示します。

## <a name="customizing-an-entryentrymd"></a>[エントリのカスタマイズ](entry.md)

Xamarin.Forms [ `Entry` ](xref:Xamarin.Forms.Entry)コントロールは、1 行の編集対象のテキストを使用できます。 この記事では、用のカスタム レンダラーを作成する方法を示します、`Entry`コントロール、開発者は独自のプラットフォームに固有のカスタマイズを使用した既定のネイティブ レンダリングをオーバーライドします。

## <a name="customizing-a-contentpagecontentpagemd"></a>[コンテンツ ページのカスタマイズ](contentpage.md)

A [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)は 1 つのビューを表示し、画面の大部分を占める視覚的要素。 この記事では、用のカスタム レンダラーを作成する方法を示します、 `ContentPage`  ページで、開発者は独自のプラットフォームに固有のカスタマイズを使用した既定のネイティブ レンダリングをオーバーライドします。

## <a name="customizing-a-mapmapindexmd"></a>[マップのカスタマイズ](map/index.md)

Xamarin.Forms.Maps を高速で使い慣れたマップを提供する各プラットフォームの Api が発生するネイティブのマップを使用して、ユーザーのマップを表示するため、クロス プラットフォームの抽象化を提供します。 このトピックのカスタム レンダラーを作成する方法を示します、`Map`コントロール、開発者は独自のプラットフォームに固有のカスタマイズを使用した既定のネイティブ レンダリングをオーバーライドします。

## <a name="customizing-a-listviewlistviewmd"></a>[ListView のカスタマイズ](listview.md)

Xamarin.Forms を[ `ListView` ](xref:Xamarin.Forms.ListView)は垂直方向の一覧としてデータのコレクションを表示するビューです。 この記事では、カスタム レンダラーをプラットフォーム固有のリスト コントロールとネイティブのセルのレイアウトをカプセル化するネイティブ リスト コントロールのパフォーマンスをさらに制御できるように作成する方法を示します。

## <a name="customizing-a-viewcellviewcellmd"></a>[ViewCell のカスタマイズ](viewcell.md)

Xamarin.Forms を[ `ViewCell` ](xref:Xamarin.Forms.ViewCell)セルに追加できるは、 [ `ListView` ](xref:Xamarin.Forms.ListView)または[ `TableView`](xref:Xamarin.Forms.TableView)開発者が定義したビューを含みます。 この記事では、用のカスタム レンダラーを作成する方法を示します、 `ViewCell` 、Xamarin.Forms 内でホストされている`ListView`コントロール。 これが停止中に繰り返し呼び出されるから Xamarin.Forms のレイアウトの計算`ListView`スクロールします。

## <a name="implementing-a-viewviewmd"></a>[ページの実装](view.md)

Xamarin.Forms のカスタム ユーザー インターフェイス コントロールがから派生する必要があります、 [ `View` ](xref:Xamarin.Forms.View)クラスは、レイアウトと画面上のコントロールを配置するために使用します。 この記事では、デバイスのカメラからのプレビュー ビデオ ストリームを表示するため、Xamarin.Forms カスタム コントロール用のカスタム レンダラーを作成する方法を示します。

## <a name="implementing-a-hybridwebviewhybridwebviewmd"></a>[HybridWebView の実装](hybridwebview.md)

この記事では、用のカスタム レンダラーを作成する方法を示します、`HybridWebView`カスタム コントロールは、JavaScript から呼び出される c# コードを許可するプラットフォームに固有の web コントロールを強化する方法を示します。

## <a name="implementing-a-video-playervideo-playerindexmd"></a>[ビデオ プレーヤーを実装します。](video-player/index.md)

この記事では、レンダラーがカスタム実装を記述する方法を示しています。 `VideoPlayer` web、アプリケーションのリソースとして埋め込まれたビデオまたはユーザーのデバイス上のビデオ ライブラリに格納されているビデオからビデオを再生できるコントロールです。 メソッドと読み取り専用のバインド可能なプロパティを実装するなど、いくつかの手法を説明します。


## <a name="related-links"></a>関連リンク

- [エフェクト](~/xamarin-forms/app-fundamentals/effects/index.md)
- [カスタム レンダラー (Xamarin University のビデオ)](https://developer.xamarin.com/videos/cross-platform/xamarinforms-custom-renderers/)
- [サンプルのカスタム レンダラー (Xamarin University のビデオ)](http://bit.ly/xf-customrenderer)
