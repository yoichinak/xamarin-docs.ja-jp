---
title: Xamarin.Forms のカスタム レンダラー
description: カスタム レンダラーにより、開発者は各プラットフォーム上のネイティブ コントロールのレンダリングをオーバーライドして、Xamarin.Forms コントロールの外観とビヘイビアーをカスタマイズできるようになります。
ms.prod: xamarin
ms.assetid: BF1CF23A-3BC9-4226-92E6-DAEEB91422F1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
---

# <a name="xamarinforms-custom-renderers"></a>Xamarin.Forms のカスタム レンダラー

_Xamarin.Forms のユーザー インターフェイスは、ターゲット プラットフォームのネイティブ コントロールを使用してレンダリングされるため、Xamarin.Forms アプリケーションでは各プラットフォームの適切な外観を維持できます。カスタム レンダラーにより、開発者はこのプロセスをオーバーライドして、各プラットフォーム上で Xamarin.Forms コントロールの外観とビヘイビアーをカスタマイズできるようになります。_

## <a name="introduction-to-custom-renderersintroductionmd"></a>[カスタム レンダラーの概要](introduction.md)

カスタム レンダラーにより、Xamarin.Forms コントロールの外観とビヘイビアーをカスタマイズするための強力な方法が提供されます。 それらは、スタイルに関する小さな変更や、洗練されたプラットフォーム固有のレイアウトおよびビヘイビアーのカスタマイズのために使用できます。 この記事では、カスタム レンダラーの概要を示し、カスタム レンダラーを作成するプロセスについて説明します。

## <a name="renderer-base-classes-and-native-controlsrenderersmd"></a>[レンダラーの基本クラスおよびネイティブ コントロール](renderers.md)

すべての Xamarin.Forms コントロールには、ネイティブ コントロールのインスタンスを作成する各プラットフォーム用のレンダラーが付属しています。 この記事では、Xamarin.Forms の各ページ、レイアウト、ビュー、およびセルを実装する、レンダラーおよびネイティブ コントロールのクラスの一覧を示します。

## <a name="customizing-an-entryentrymd"></a>[エントリのカスタマイズ](entry.md)

Xamarin.Forms の [`Entry`](xref:Xamarin.Forms.Entry) コントロールによって、テキストの 1 行を編集対象にできます。 この記事では、`Entry` コントロール用のカスタム レンダラーを作成する方法を示します。これにより、開発者は既定のネイティブ レンダリングを、各自のプラットフォームに固有のカスタマイズでオーバーライドできるようになります。

## <a name="customizing-a-contentpagecontentpagemd"></a>[コンテンツ ページのカスタマイズ](contentpage.md)

[`ContentPage`](xref:Xamarin.Forms.ContentPage) は、単一ビューを表示し、画面の大部分を占めるビジュアル要素です。 この記事では、`ContentPage` ページ用のカスタム レンダラーを作成する方法を示します。これにより、開発者は既定のネイティブ レンダリングを、各自のプラットフォームに固有のカスタマイズでオーバーライドできるようになります。

## <a name="customizing-a-mapmapindexmd"></a>[マップのカスタマイズ](map/index.md)

Xamarin.Forms.Maps には、プラットフォームごとのネイティブ マップ API を使ったマップ表示用の抽象化がクロスプラットフォームで用意されていて、高速で使い慣れたマップのユーザー エクスペリエンスが提供されます。 このトピックでは、`Map` コントロール用のカスタム レンダラーを作成する方法を示します。これにより、開発者は既定のネイティブ レンダリングを、各自のプラットフォームに固有のカスタマイズでオーバーライドできるようになります。

## <a name="customizing-a-listviewlistviewmd"></a>[ListView のカスタマイズ](listview.md)

Xamarin.Forms の [`ListView`](xref:Xamarin.Forms.ListView) は、データのコレクションを垂直方向の一覧として表示するビューです。 この記事では、プラットフォーム固有のリスト コントロールとネイティブのセルのレイアウトをカプセル化するカスタム レンダラーを作成し、ネイティブ リスト コントロールのパフォーマンスをより厳密に制御する方法を示します。

## <a name="customizing-a-viewcellviewcellmd"></a>[ViewCell のカスタマイズ](viewcell.md)

Xamarin.Forms の [`ViewCell`](xref:Xamarin.Forms.ViewCell) は、[`ListView`](xref:Xamarin.Forms.ListView) または [`TableView`](xref:Xamarin.Forms.TableView) に追加できるセルで、これには開発者が定義したビューが含まれます。 この記事では、Xamarin.Forms の `ListView` コントロール内でホストされる、`ViewCell` 用のカスタム レンダラーを作成する方法を示します。 これにより、`ListView` のスクロール中に Xamarin.Forms のレイアウトの計算が繰り返し呼び出されることが回避されます。

## <a name="implementing-a-viewviewmd"></a>[ページの実装](view.md)

Xamarin.Forms のカスタム ユーザー インターフェイス コントロールは、[`View`](xref:Xamarin.Forms.View) クラスから派生させる必要があります。これは画面上にレイアウトとコントロールを配置するために使われます。 この記事では、デバイスのカメラからビデオ ストリームのプレビューを表示するために使う、Xamarin.Forms のカスタム コントロール用のカスタム レンダラーを作成する方法を示します。

## <a name="implementing-a-hybridwebviewhybridwebviewmd"></a>[HybridWebView の実装](hybridwebview.md)

この記事では、`HybridWebView` カスタム コントロール用のカスタム レンダラーを作成する方法を示します。これにより、プラットフォームに固有の Web コントロールを強化して、JavaScript からの C# コードの呼び出しを実現する方法が示されます。

## <a name="implementing-a-video-playervideo-playerindexmd"></a>[ビデオ プレーヤーの実装](video-player/index.md)

この記事では、レンダラーを記述して、Web のビデオ、アプリケーションのリソースとして埋め込まれたビデオ、またはユーザーのデバイス上のビデオ ライブラリに格納されているビデオを再生できる `VideoPlayer` のカスタム コントロールを実装する方法を示します。 メソッドの実装や、バインド可能な読み取り専用プロパティなど、いくつかの手法を示します。


## <a name="related-links"></a>関連リンク

- [エフェクト](~/xamarin-forms/app-fundamentals/effects/index.md)
- [カスタム レンダラー (Xamarin University のビデオ)](https://developer.xamarin.com/videos/cross-platform/xamarinforms-custom-renderers/)
