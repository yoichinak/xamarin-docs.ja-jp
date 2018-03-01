---
title: "ビデオ プレーヤーを実装します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: CE9E955D-A9AC-4019-A5D7-6390D80DECA1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: e818bc3fa9793f093c10ac2617c5a822d08213d4
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="implementing-a-video-player"></a>ビデオ プレーヤーを実装します。

Xamarin.Forms のアプリケーションでのビデオ ファイルを再生することが望ましい場合があります。 この一連の記事を iOS、Android、および Xamarin.Forms クラスという名前のユニバーサル Windows プラットフォーム (UWP) 用のカスタム レンダラーを記述する方法を説明する`VideoPlayer`です。

[ **VideoPlayerDemos** ](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)を実装し、サポートするすべてのファイルのサンプル`VideoPlayer`という名前のフォルダーには`FormsVideoLibrary`され、名前空間で識別`FormsVideoLibrary`または名前空間開始される`FormsVideoLibrary`です。 この組織と名前付けを簡単にファイルをコピーする、ビデオ プレーヤーを Xamarin.Forms ソリューションです。

`VideoPlayer` 3 種類のソースからのビデオ ファイルを再生できます。

- URL を使用して、インターネット
- プラットフォームのアプリケーションに埋め込まれたリソース
- デバイスのビデオ ライブラリ

ビデオ プレーヤーを必要と*トランスポート コントロール*を再生したり、ビデオを一時停止するためのボタンは、ビデオを使用して進行状況を示しています、バーを配置、および別の場所に迅速にスキップすることができます。 `VideoPlayer` トランスポート コントロールを使用し、カスタム トランスポート コントロールとバーを配置するか (以下の手順に従って、プラットフォームによって提供される位置のバーを提供します。 IOS、Android、およびユニバーサル Windows プラットフォームで実行されているプログラムを次に示します。

[![Web のビデオの再生](web-videos-images/playwebvideo-small.png "Web ビデオの再生")](web-videos-images/playwebvideo-large.png "Web ビデオの再生")

もちろん、拡大表示の電話を横向きにすることができます。

高度なビデオ プレーヤーは、ボリューム コントロールやは、電話の呼び出し時にビデオを中断するためのメカニズムの再生中にアクティブな画面を維持する方法など、いくつかの機能があります。

次の一連の記事の段階的に、プラットフォームのレンダラーとサポート クラスを構築する方法を示しています。

## <a name="creating-the-platform-video-playersplayer-creationmd"></a>[プラットフォームのビデオ プレーヤーを作成します。](player-creation.md)

各プラットフォームの要件、`VideoPlayerRenderer`作成し、プラットフォームでサポートされているビデオ プレーヤーの制御を維持するクラス。 この記事は、クラス、および、プレーヤーの作成方法のレンダラーの構造を示しています。

## <a name="playing-a-web-videoweb-videosmd"></a>[Web ビデオを再生します。](web-videos.md)

おそらく、ビデオ プレーヤー向けのビデオの最も一般的なソースは、インターネットです。 この記事では、Web ビデオを参照されているし、ビデオ プレーヤーのソースとして使用する方法について説明します。

## <a name="binding-video-sources-to-the-playersource-bindingsmd"></a>[プレーヤーへのビデオ ソースのバインド](source-bindings.md)

この記事では、`ListView`を再生するビデオのコレクションを表示します。 1 つのプログラムが、分離コード ファイルが、ビデオ プレーヤーのビデオ ソースを設定する方法を示していますが、別のプログラムは、データの間のバインディングを使用する方法を示しています、`ListView`と、ビデオ プレーヤー。

## <a name="loading-application-resource-videosloading-resourcesmd"></a>[アプリケーション リソースのビデオの読み込み](loading-resources.md)

ビデオは、プラットフォームのプロジェクトにリソースとして埋め込むことができます。 この記事では、それらのリソースを格納し、後で、ビデオ プレーヤーで再生できるように、プログラムに読み込むにする方法を示します。

## <a name="accessing-the-devices-video-libraryaccessing-librarymd"></a>[デバイスのビデオ ライブラリへのアクセス](accessing-library.md)

ビデオを作成すると、デバイスのカメラを使用して、ビデオ ファイルは、デバイスのイメージのライブラリに格納されます。 この記事では、ビデオを選択し、再生、ビデオ プレーヤーを使用して、デバイスのイメージの選択にアクセスする方法を示します。

## <a name="custom-video-transport-controlscustom-transportmd"></a>[カスタムのビデオ トランスポート コントロール](custom-transport.md)

各プラットフォームでのビデオ プレーヤー用のボタンの形式で、独自のトランスポート コントロールを提供します**再生**と**Pause**、これらのボタンの表示を抑制して、独自に提供します。 方法は、この記事で説明します。

## <a name="custom-video-positioningcustom-positioningmd"></a>[ビデオのカスタムの配置](custom-positioning.md)

ビデオの進行状況を表示し、特定の位置に進んでいるか遅れてをスキップすることができます位置バーを持つ各プラットフォームのビデオ プレーヤーのとします。 この記事では、カスタム コントロールと位置バーに置き換える方法を示します。





## <a name="related-links"></a>関連リンク

- [ビデオ プレーヤーのデモ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
