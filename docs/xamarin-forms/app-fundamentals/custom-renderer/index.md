---
title: "Xamarin.Forms のカスタム レンダラー" の説明:"カスタム レンダラーにより、開発者は各プラットフォーム上のネイティブ コントロールのレンダリングをオーバーライドして、Xamarin.Forms コントロールの外観とビヘイビアーをカスタマイズできるようになります。"
ms.prod: xamarin ms.assetid:BF1CF23A-3BC9-4226-92E6-DAEEB91422F1 ms.technology: xamarin-forms author: davidbritch ms.author: dabritch ms.date:12/03/2019 no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# <a name="xamarinforms-custom-renderers"></a>Xamarin.Forms のカスタム レンダラー

_Xamarin.Forms ユーザー インターフェイスは、ターゲット プラットフォームのネイティブ コントロールを使用してレンダリングされるため、Xamarin.Forms アプリケーションでは各プラットフォームの外観を維持できます。カスタム レンダラーにより、開発者はこのプロセスをオーバーライドして、各プラットフォーム上で Xamarin.Forms コントロールの外観とビヘイビアーをカスタマイズできるようになります。_

## <a name="introduction-to-custom-renderers"></a>[カスタム レンダラーの概要](introduction.md)

カスタム レンダラーにより、Xamarin.Forms コントロールの外観とビヘイビアーをカスタマイズするための強力な方法が提供されます。 それらは、スタイルに関する小さな変更や、洗練されたプラットフォーム固有のレイアウトおよびビヘイビアーのカスタマイズのために使用できます。 この記事では、カスタム レンダラーの概要を示し、カスタム レンダラーを作成するプロセスについて説明します。

## <a name="renderer-base-classes-and-native-controls"></a>[レンダラーの基本クラスおよびネイティブ コントロール](renderers.md)

すべての Xamarin.Forms コントロールには、ネイティブ コントロールのインスタンスを作成する各プラットフォーム用のレンダラーが付属しています。 この記事では、Xamarin.Forms のページ、レイアウト、ビュー、およびセルのそれぞれを実装するレンダラーとネイティブ コントロールのクラスの一覧を示します。

## <a name="customizing-an-entry"></a>[エントリのカスタマイズ](entry.md)

Xamarin.Forms の [`Entry`](xref:Xamarin.Forms.Entry) コントロールによって、1 行のテキストを編集対象にできます。 この記事では、`Entry` コントロール用のカスタム レンダラーを作成する方法を示します。これにより、開発者は既定のネイティブ レンダリングを、各自のプラットフォームに固有のカスタマイズでオーバーライドできるようになります。

## <a name="customizing-a-contentpage"></a>[コンテンツ ページのカスタマイズ](contentpage.md)

[`ContentPage`](xref:Xamarin.Forms.ContentPage) は、単一ビューを表示し、画面の大部分を占めるビジュアル要素です。 この記事では、`ContentPage` ページ用のカスタム レンダラーを作成する方法を示します。これにより、開発者は既定のネイティブ レンダリングを、各自のプラットフォームに固有のカスタマイズでオーバーライドできるようになります。

## <a name="customizing-a-map-pin"></a>[マップ ピンのカスタマイズ](map-pin.md)

Xamarin.Forms.Maps には、プラットフォームごとのネイティブ マップ API を使ったマップ表示用の抽象化がクロスプラットフォームで用意されていて、高速で使い慣れたマップのユーザー エクスペリエンスが提供されます。 このトピックでは、`Map` コントロール用のカスタム レンダラーを作成する方法を示します。これにより、開発者は既定のネイティブ レンダリングを、各自のプラットフォームに固有のカスタマイズでオーバーライドできるようになります。

## <a name="customizing-a-listview"></a>[ListView のカスタマイズ](listview.md)

Xamarin.Forms の [`ListView`](xref:Xamarin.Forms.ListView) は、データのコレクションを縦方向の一覧として表示するビューです。 この記事では、プラットフォーム固有のリスト コントロールとネイティブのセルのレイアウトをカプセル化するカスタム レンダラーを作成し、ネイティブ リスト コントロールのパフォーマンスをより厳密に制御する方法を示します。

## <a name="customizing-a-viewcell"></a>[ViewCell のカスタマイズ](viewcell.md)

Xamarin.Forms の [`ViewCell`](xref:Xamarin.Forms.ViewCell) は、[`ListView`](xref:Xamarin.Forms.ListView) または [`TableView`](xref:Xamarin.Forms.TableView) に追加できるセルで、これには開発者が定義したビューが含まれます。 この記事では、Xamarin.Forms の `ListView` コントロール内でホストされる、`ViewCell` 用のカスタム レンダラーを作成する方法を示します。 これにより、`ListView` のスクロール中に Xamarin.Forms のレイアウトの計算が繰り返し呼び出されることが回避されます。

## <a name="customizing-a-webview"></a>[WebView のカスタマイズ](hybridwebview.md)

Xamarin.Forms の [`WebView`](xref:Xamarin.Forms.WebView) は、アプリに Web コンテンツと HTML コンテンツを表示するビューです。 この記事では、JavaScript から C# コードを呼び出せるように `WebView` を拡張するカスタム レンダラーを作成する方法について説明します。

## <a name="implementing-a-view"></a>[ページの実装](view.md)

Xamarin.Forms のカスタム ユーザー インターフェイス コントロールは、[`View`](xref:Xamarin.Forms.View) クラスから派生させる必要があります。これは画面上にレイアウトとコントロールを配置するために使われます。 この記事では、デバイスのカメラからビデオ ストリームのプレビューを表示するために使う、Xamarin.Forms のカスタム コントロール用のカスタム レンダラーを作成する方法を示します。

## <a name="implementing-a-video-player"></a>[ビデオ プレーヤーの実装](video-player/index.md)

この記事では、レンダラーを記述して、Web のビデオ、アプリケーションのリソースとして埋め込まれたビデオ、またはユーザーのデバイス上のビデオ ライブラリに格納されているビデオを再生できる `VideoPlayer` のカスタム コントロールを実装する方法を示します。 メソッドの実装や、バインド可能な読み取り専用プロパティなど、いくつかの手法を示します。
