---
title: ビデオ プレーヤーの実装
description: この記事では、Xamarin.Forms を使ってビデオ プレーヤー アプリケーションを実装する方法について説明します。
ms.prod: xamarin
ms.assetid: 0CE9BEE7-4F81-4A00-B9B3-5E2535CD3050
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 34c3e2bb09c6e285f38b3c17bab2e10debe1d890
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93367258"
---
# <a name="implementing-a-video-player"></a>ビデオ プレーヤーの実装

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos)

Xamarin.Forms アプリケーション内でビデオ ファイルを再生することが望ましい場合があります。 この一連の記事では、`VideoPlayer` という名前の Xamarin.Forms クラスに向けて、iOS、Android、およびユニバーサル Windows プラットフォーム (UWP) 用のカスタム レンダラーを記述する方法について説明します。

[**VideoPlayerDemos**](/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos) のサンプルでは、`VideoPlayer` を実装およびサポートするすべてのファイルが `FormsVideoLibrary` という名前のフォルダーに置かれ、`FormsVideoLibrary` という名前空間または `FormsVideoLibrary` で始まる名前空間を使って識別されました。 この機構と名前付けによって、ビデオ プレーヤー ファイルをご自身の Xamarin.Forms ソリューションにコピーしやすくなります。

`VideoPlayer` では、3 種類のソースからビデオ ファイルを再生できます。

- インターネット (URL を使用)
- プラットフォーム アプリケーションに埋め込まれたリソース
- デバイスのビデオ ライブラリ

ビデオ プレーヤーには、" *トランスポート コントロール* " (ビデオを再生および一時停止するためのボタン) と、ビデオの進行状況を表示したりユーザーが別の場所にすばやくスキップできたりする位置バーが必要です。 `VideoPlayer` では、プラットフォームによって提供されるトランスポート コントロールと位置バーのどちらかを使えます (以下を参照)。または、ご自身でカスタムのトランスポート コントロールと位置バーを提供できます。 iOS、Android、およびユニバーサル Windows プラットフォームで実行されているプログラムを次に示します。

[![Web ビデオを再生する](web-videos-images/playwebvideo-small.png "Web ビデオを再生する")](web-videos-images/playwebvideo-large.png#lightbox "Web ビデオを再生する")

もちろん、電話を横向きにして拡大表示させることが可能です。

より洗練されたビデオ プレーヤーには、ボリューム コントロールや、電話がかかってきたときにビデオを中断するしくみ、再生中に画面をアクティブ状態に保つ方法など、いくつかの追加機能が備わっています。

次の一連の記事では、プラットフォーム レンダラーとサポート クラスの構築方法を段階的に説明していきます。

## <a name="creating-the-platform-video-players"></a>[プラットフォーム ビデオ プレーヤーの作成](player-creation.md)

各プラットフォームには、プラットフォームによってサポートされているビデオ プレーヤーのコントロールを作成および管理する `VideoPlayerRenderer` クラスが必要です。 この記事では、レンダラー クラスの構造と、プレーヤーの作成方法について説明します。

## <a name="playing-a-web-video"></a>[Web ビデオの再生](web-videos.md)

おそらく、ビデオ プレーヤー用のビデオのソースとして最も一般的なものは、インターネットです。 この記事では、Web ビデオを参照し、ビデオ プレーヤーのソースとして使う方法について説明します。

## <a name="binding-video-sources-to-the-player"></a>[プレーヤーへのビデオ ソースのバインド](source-bindings.md)

この記事では `ListView` を使って、再生するビデオのコレクションを表現します。 1 つのプログラムでは分離コード ファイルを使ってビデオ プレーヤーのビデオ ソースを設定する方法が示されていますが、2 つ目のプログラムでは `ListView` とビデオ プレーヤーの間でデータ バインディングを使う方法が示されています。

## <a name="loading-application-resource-videos"></a>[アプリケーション リソース ビデオの読み込み](loading-resources.md)

ビデオはリソースとしてプラットフォーム プロジェクトに埋め込むことができます。 この記事では、それらのリソースを格納して、ビデオ プレーヤーで再生するために後でプログラムに読み込む方法を示します。

## <a name="accessing-the-devices-video-library"></a>[デバイスのビデオ ライブラリへのアクセス](accessing-library.md)

デバイスのカメラを使用してビデオを作成した場合、そのビデオ ファイルはデバイスのイメージ ライブラリに格納されます。 この記事では、デバイスのイメージ ピッカーを使ってビデオを選択し、それをビデオ プレイヤーを使って再生する方法について説明します。

## <a name="custom-video-transport-controls"></a>[カスタムのビデオ トランスポート コントロール](custom-transport.md)

各プラットフォーム上のビデオ プレーヤーには、 **[再生]** や **[一時停止]** ボタンという形でそれぞれのトランスポート コントロールが備わっていますが、そのボタンを表示させないようにしてご自身のものを指定できます。 この記事ではその方法を説明します。

## <a name="custom-video-positioning"></a>[ビデオのカスタム配置](custom-positioning.md)

各プラットフォームのビデオ プレーヤーには、ビデオの進行状況を示し、特定の位置まで前後にスキップできる位置バーが備わっています。 この記事では、その位置バーをカスタム コントロールで置き換える方法を示します。

## <a name="related-links"></a>関連リンク

- [ビデオ プレーヤーのデモ (サンプル)](/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos)