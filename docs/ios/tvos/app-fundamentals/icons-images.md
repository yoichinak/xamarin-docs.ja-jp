---
title: アイコンとイメージを Xamarin で tvOS の操作
description: このドキュメントでは、アイコンと Xamarin でビルドされた tvOS アプリでイメージを操作する方法について説明します。 起動イメージ、階層化されたイメージ、アプリのアイコン、およびがについて説明します。
ms.prod: xamarin
ms.assetid: A2DA4347-0563-4C72-A8D7-5B9DE9E28712
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: dab1b0f7bf7aabb4dfcfbfdcb5e202baa48e664d
ms.sourcegitcommit: 7ccc7a9223cd1d3c42cd03ddfc28050a8ea776c2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/13/2019
ms.locfileid: "67865166"
---
# <a name="working-with-tvos-icons-and-images-in-xamarin"></a>アイコンとイメージを Xamarin で tvOS の操作

魅惑的な作成、および画像のアイコンは、Apple TV のアプリの没入型のユーザー エクスペリエンスの開発の重要な部分です。 このガイドを作成し、Xamarin.tvOS アプリに必要なグラフィックス アセットを含めるために必要な手順が含まれます。

- [起動イメージ](#Launch-Image)-アプリを初めて起動すると起動が完了したら、アプリの最初の画面に置換されます起動イメージを表示します。
- [イメージ レイヤー](#Layered-Images) - 選択された項目の 3D 効果を作成する視差効果と新しいイメージの階層化の作業を Apple の Apple TV に固有です。 いくつかの方法がある[階層型イメージの作成](#Creating-Layered-Images)です。
- [アプリ アイコン](#App-Icons)-だけでなく、Apple TV ホーム画面しますが、App store にアイコンが必要です。 それらは、階層型イメージとして指定する必要があります。
- [トップ シェルフのイメージ](#Top-Shelf-Image)-トップ シェルフのイメージをアプリの機能を強調表示する必要が場合は、アプリは、ホーム画面の上部の行に配置します。 必要に応じて、指定[上部棚の動的コンテンツ](#Dynamic-Top-Shelf-Content)をアプリのコンテンツを強調表示します。
- [ゲーム センター イメージ](#Game-Center-Images)の場合は、アプリは、ゲームし、Game Center を使用して、その他のいくつかのイメージが必要になります。
- [Xamarin.tvOS プロジェクトのイメージを設定](#Setting-Xamarin.tvOS-Project-Images)-Xamarin.tvOS アプリの起動イメージとアプリのアイコンを設定するために必要な手順について説明します。

> [!IMPORTANT]
> Apple TV のすべてのイメージは、1 倍解像度では (`@1x`) する必要がありますと_のみ_このサイズのイメージを使用します。 拡大を含む、以上の解像度のグラフィックスにダウンロードして追加のメモリとストレージを使用する時間がかかるだけでなくが、実行時に動的に再スケーリングする必要がある、描画のパフォーマンスが低下します。

<a name="Launch-Image" />

## <a name="launch-image"></a>イメージを起動します。

起動イメージは、まず、Xamarin.tvOS アプリは、Apple TV に最初に起動したときに表示され、そのため、すべての tvOS アプリが起動イメージを指定する必要があります。 

起動イメージでは、簡単に表示され、アプリが高速で応答性があるという印象を与えます。 Apple TV で置換されます起動イメージ、アプリの最初の画面すぐあります後。

起動画像がない広告または芸術の式の営業案件、アプリをすばやく起動されができているという印象を与えるためだけに存在するを使用します。

|起動イメージのサイズ|メモ|
|---|---|
|1920 x 1080 ピクセル|非階層の .png ファイルのみ|

Apple では、アプリの起動イメージをデザインするための次の推奨事項。

- **最初の画面にほぼ同じ**-Your 起動画面は、可能なアプリの最初の画面の近くにする必要があります。 別のグラフィック要素などにより、いたずら"flash"最初の画面が表示されたらします。
- **使用してテキストを避けるため**-起動イメージは静的と、そのため、表示される前にローカライズされません。
- **起動を済ませ**-アプリを頻繁に切り替えるための Apple TV のユーザー、アプリの起動プロセスに注目しないでください。
- **ない広告やブランド**-Your 起動イメージとして、[バージョン情報] 画面では使用できませんまたはアプリの最初の画面の静的部分である場合を除き、すべてのブランドが含まれます。 広告は固く禁止されています。

<a name="Setting-the-Launch-Image" />

### <a name="setting-the-launch-image"></a>起動イメージの設定

起動画像が tvOS プロジェクトを設定するには次を行ってください。

1. **ソリューション エクスプ ローラー**、ダブルクリックして`Assets.xcassets`編集用に開きます。 

    [![](icons-images-images/asset01.png "Assets.xcassets ファイル")](icons-images-images/asset01.png#lightbox)
2. **資産エディター**、 をクリックして、`LaunchImages`資産。 

    [![](icons-images-images/asset02.png "LaunchImages 資産")](icons-images-images/asset02.png#lightbox)
3. をクリックして、 **Apple TV x 1**エントリ起動イメージを選択するか、必要に応じて、ファイル システムから新しいイメージをドラッグします。 

    [![](icons-images-images/asset03.png "起動イメージを選択します。")](icons-images-images/asset03.png#lightbox)
4. 変更内容を保存します。

<a name="Layered-Images" />

## <a name="layered-images"></a>階層化されたイメージ

新しい Apple TV、部屋の頭の中、画面上のコンテンツに接続されている、ソファにユーザーを保存するときに役立つ 3 D 効果を生成するために、視差効果のイメージの階層型作業をします。

階層化されたイメージが含まれてから 2 つ (2 5 (5) に個別のイメージの完了が組み合わされたレイヤーです。 背景のレイヤーを除く各レイヤーは、奥行を作成するのにと透明度の Z オーダーを使用します。 階層型イメージを使用して操作すると、ユーザー、Z オーダーの上位のレイヤーが拡大縮小され、この効果を作成する重複しました。

[![](icons-images-images/layered01.png "階層化の画像の Z オーダーのダイアグラム")](icons-images-images/layered01.png#lightbox)

> [!IMPORTANT]
> 階層化されたイメージは、アプリのアイコンに必要なし、はその他の省略可能[フォーカスを設定できる項目](~/ios/tvos/app-fundamentals/navigation-focus.md#Focus-and-Selection)(トップ シェルフのイメージ) など。 ただし、Apple は、階層型イメージが、アプリでフォーカスを得られる任意のイメージを使用をお勧めします。




Apple では、階層型イメージをデザインするための次の推奨事項。

- **背景レイヤー不透明**-、背景レイヤー (レイヤー 1)**する必要があります**不透明または Apple TV の階層型イメージを使用しようとすると、エラーが表示されます。 その他のすべてのレイヤーは、3 D 効果を強化するために透過性の複数のレベルを含めることができます。
- **前景色、中央、および背景要素を分離**-フォア グラウンドで (ゲームの文字) など、著名な要素を配置、セカンダリの要素またはシャドウの中央を使用します。 最後に、ニュートラルの背景、上位層のステージを提供するが含まれます。
- **フォア グラウンドでテキストを保持**-、テキストをより高いレベルで表示する場合を除き、通常必要がある最上位のレイヤーにします。
- **単純な階層化を使用して、** -、視差効果がデザインされたときは、微妙なを少し目障りな感じ、非現実的な効果を防ぐために最小限にしておいて、レイヤー。
- **セーフ ゾーンを含む**-視差効果の中にトリミングされるは、上位層、ため、各レイヤーに安全な領域の枠線をビルドする必要があります。 近すぎるのレイヤー エッジで、コンテンツを取得する場合からトリミングになることができます。 上部レイヤーは、スケーリングの詳細と、下位層よりもトリミングが発生します。 参照してください、[サイジング イメージ レイヤー](#Sizing-Image-Layers)以下のセクション。
- **多くの場合、プレビュー** -イメージの階層化の目的の 3D 効果が発生して、トリミングされていますが、個々 のレイヤー上のコンテンツのいずれも確保するには、多くの場合、プレビューを表示する必要があります。 想定どおりにレンダリングするかどうかを確認する実際のハードウェアに Apple TV で、階層型イメージをプレビューする必要があります。

可能であれば、常に、組み込みを使用する必要があります`UIKit`フォーカスになると、視差効果を自動的に取得するが、階層型イメージを表示するコントロール。

<a name="Sizing-Image-Layers" />

### <a name="sizing-image-layers"></a>イメージ レイヤーのサイズ変更

必ず含めてすることが重要な_セーフ ゾーン_階層型イメージを構成する各層に境界線。 レイヤーのエッジに近すぎる場合、個々 のレイヤーを拡張し、視差効果の中にトリミング、ためからレイヤーのコンテンツがトリミングされることができます。

[![](icons-images-images/layered02.png "35 ピクセルの枠線")](icons-images-images/layered02.png#lightbox)

<a name="Creating-Layered-Images" />

### <a name="creating-layered-images"></a>階層型イメージを作成します。

tvOS では、次の形式でイメージのレイヤーで動作します。

- **自動車ファイル**-Apple が作成した独自資産カタログの形式になります。 車のファイルを直接作成しないでください、LSR ファイルからコンパイル時に作成され、アプリ バンドルに含まれます。
- **LSR イメージ**-これは、Apple が作成した独自のイメージ形式。 使用、[視差エクスポーター Adobe の Photoshop プラグイン](https://itunespartner.apple.com/assets/downloads/ParallaxExporter_Apps.zip)または[視差プレビューアー](http://itunespartner.apple.com/assets/downloads/Parallax%20Previewer.dmg) LSR 形式でのプレビュー イメージの階層を作成します。
- **Assets.xcassets** - から 2 つ (2 5 (5) に標準`.png`自動車または LSR にコンパイルされる資産カタログに含まれている書式設定されたイメージは、コンパイル時に階層型イメージを書式設定します。
- **LCR ファイル**-Apple が作成した独自のファイル形式になります。 LCR ファイルは、コンテンツ サーバーのいずれかからダウンロードしたその他のコンテンツとして使用するためのものです。 LCR ファイル、アプリ バンドルに含めることはありません。 LCR のファイルを使用して LSR または Photoshop のファイルから生成された、 `layerutil` Xcode に含まれているコマンド ライン ツール。

<a name="The-Parallax-Previewer" />

### <a name="the-parallax-previewer"></a>視差プレビューアー

作成された Apple、[視差プレビューアー](http://itunespartner.apple.com/assets/downloads/Parallax%20Previewer.dmg)プレビューを作成したアプリのアイコンとオプションのフォーカスを設定できる項目に必要な画像の階層化します。 プレビューアーには、完了した階層型イメージを形成するすべてのレイヤーが表示されます。

[![](icons-images-images/layered03.png "視差プレビューアー")](icons-images-images/layered03.png#lightbox)

階層化の画像をプレビューするには、中にイメージを回転して視差効果をプレビューするには、マウスを使用できます。 使用して、 **+** (正符号 +) と **-** (マイナス記号) ボタンを追加してレイヤーを削除します。

階層型の新しいイメージを作成するときに LSR 形式でエクスポートし、アプリのバンドルに含まれることができます。

作成して、階層型イメージのプレビューの詳細については、Apple を参照してください[視差のアートワークを作成する](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/CreatingParallaxArtwork.html#//apple_ref/doc/uid/TP40015241-CH19-SW1)のセクション、[の tvOS アプリのプログラミング ガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)します。

<a name="App-Icons" />

## <a name="app-icons"></a>アプリ アイコン

Xamarin.tvOS アプリには、Apple TV ホーム画面も、App Store のアイコンのだけでなくアプリのアイコンが必要です。 アプリ アイコンは、最初の潜在的なユーザーに優れた印象を与える変更し、ひとめでアプリの目的を伝える必要があります。

[![](icons-images-images/icon01.png "アプリ アイコン")](icons-images-images/icon01.png#lightbox)

すべてのアプリには、小さいと、アプリ アイコンの大きなバージョンの両方を指定する必要があります。 アプリがインストールされている場合、Apple TV ホーム画面の小さいアイコンが使用されます。 App Store では、大きなバージョンを使用します。 大きなアプリ アイコンには、小さいアイコンのバージョンのルック アンド フィールを模倣する必要があります。

|小さいアイコン||大きいアイコン||
|---|---|---|---|
|実際のサイズ|400x240px|サイズ|1280x768px|
|安全なゾーンのサイズ|370x222px|||
|フォーカスされていないサイズ|300x180px|||
|フォーカスのあるサイズ|370x222px|||

> [!IMPORTANT]
> アプリのアイコンとして指定する必要があります**階層型イメージ**します。 参照してください、[階層型イメージ](#Layered-Images)詳細については、前述の「します。




Apple では、アプリのアイコンを作成するための次の推奨事項を提供します。

- **1 つのフォーカス ポイントを提供**– デザイン、1 つのフォーカス ポイントと、アイコンがアイコンの中央に直接配置します。
- **単純な背景を使用する**– 上位層が目立つようにアイコンの背景を単純にしてください。単純な色または微妙なグラデーションを使用してください。
- **テキストの量を制限する**– のみ、アイコンの設計に必要なときにも、テキストが含めるアプリの名前は、アイコンの下に表示される、ユーザーが選択されている、ために必要があります。
- **スクリーン ショットを使用しない**– スクリーン ショットが複雑すぎるため、アイコンと、ひとめでアプリの目的を表示するユーザーを許可しません。
- **アイコンの正方形を維持する**– tvOS 微妙、アイコンの角を丸くマスクを自動的に適用されます。 自分でこの丸めを含めないでください。
- **分離のレイヤー注意深く**– ほとんどのレイヤーの中央とニュートラル バック グラウンドで、これを洗練するため、上位層でセカンダリ アイテム、テキストが上にする必要があります。
- **グラデーションと慎重に影を使用して、** – グラデーションや影が慎重に使用する必要がありますので視差効果と衝突ことができます。 単純な上から下へ、薄い-濃いグラデーション スタイルに最適です。 Shadows が鋭角、はっきりと最適に動作する通常します。
- **レイヤーの透明度の異なる**: 3 D 効果を向上させる、アプリ アイコンの上位レベルの透過性のレベルをさまざまな使用。 背景レイヤーは不透明である必要があります。 またはエラーになります。

<a name="Setting-the-App-Icons" />

### <a name="setting-the-app-icons"></a>アプリ アイコンの設定

TvOS プロジェクトに必要なアプリ アイコンを設定するには次を行ってください。

1. **ソリューション エクスプ ローラー**、ダブルクリックして`Assets.xcassets`編集用に開きます。 

    [![](icons-images-images/asset01.png "Assets.xcassets fileg")](icons-images-images/asset01.png#lightbox)
2. **資産エディター**、展開、`App Icon & Top Shelf Image`資産。 

    [![](icons-images-images/asset04.png "トップ シェルフのイメージ資産を展開します。")](icons-images-images/asset04.png#lightbox)
3. 次に、展開、`App Icon - Small`資産。 

    [![](icons-images-images/asset05.png "アプリ アイコン - 小さい資産を展開します。")](icons-images-images/asset05.png#lightbox)
4. 展開し、`Back`クリックして、資産、`Contents`エントリ。 

    [![](icons-images-images/asset06.png "背面の資産の順に展開します。")](icons-images-images/asset06.png#lightbox)
5. をクリックして、 **Apple TV エントリ x 1**イメージ ファイルを選択します。
6. 上記の手順を繰り返して、`Front`と`Middle`資産。
7. 定義する同じ手順を繰り返して、`App Icon - Large`資産。
8. 変更内容を保存します。

<a name="Top-Shelf-Image" />

## <a name="top-shelf-image"></a>トップ シェルフのイメージ

ユーザーは、Apple TV ホーム画面の上部の行に Xamarin.tvOS アプリを配置するが場合、は、アプリがユーザーによって選択されたときに大きなトップ シェルフのイメージが表示されます。 このイメージは、アプリの機能を強調表示したり、そのコンテンツへの直接リンクを提供しないでください。

[![](icons-images-images/topshelf01.png "トップ シェルフのイメージの例")](icons-images-images/topshelf01.png#lightbox)

トップ シェルフのイメージを単一の静的なを指定できますか`.png`または`.lsr`ファイル (を参照してください[階層型イメージの作成](#Creating-Layered-Images)) またはフォーカスを設定できる項目の 1 つの行として動的に実行時に作成されるできます (を参照してください[。トップ シェルフの動的なコンテンツ](#Dynamic-Top-Shelf-Content)以下)。

|トップ シェルフのイメージのサイズ|メモ|
|---|---|
|1920x720px|静的 .png またはレイヤード .lsr ファイル|

Apple は、上部シェルフのイメージを作成するための次の推奨事項を提供します。

- **豊富な静的な画像を使用して、** – アプリが動的なコンテンツを提供していない場合、トップ シェルフのイメージがのフォーカスを設定できるになります。 このイメージを使用して、アプリまたはブランドの機能を強調表示します。
- **アプリのコンテンツへのリンク**– 動的上部棚のレイアウトでは、ユーザーがアプリで最も重要な検出されたコンテンツへのクイック リンク。 この領域を使用すると、アプリを開始して、すぐに指定されたコンテンツに移動するためのクイック リンクを提供します。
- **最新のコンテンツを紹介**– トップ シェルフのリッチ コンテンツは、アプリにユーザーを描画したり、さらにこれを使用するようにします。 最高評価または最新コンテンツを紹介するのに領域として使用します。
- **コンテンツをパーソナライズされた**– ユーザーの場所が、最も頻繁に使用またはお気に入りのアプリのホーム画面の上部の行にします。 トップ シェルフを使用すると、最も関心があるコンテンツを表示します。
- **広告が許可されていません**– トップ シェルフに表示されない、広告が固く禁止されています。 最新購入可能なコンテンツを表示することがありますが、価格情報は表示されません。

### <a name="setting-the-top-shelf-image"></a>トップ シェルフのイメージを設定します。

TvOS プロジェクトに必要なトップ シェルフのイメージを設定するには次を行ってください。

1. **ソリューション エクスプ ローラー**、ダブルクリックして`Assets.xcassets`編集用に開きます。 

    [![](icons-images-images/asset01.png "Assets.xcassets ファイル")](icons-images-images/asset01.png#lightbox)
2. **資産エディター**、展開、`App Icon & Top Shelf Image`資産。 

    [![](icons-images-images/asset04.png "トップ シェルフのイメージ資産を展開します。")](icons-images-images/asset04.png#lightbox)
3. をクリックして、`Top Shelf Image`資産。 

    [![](icons-images-images/asset07.png "トップ シェルフのイメージ アセット")](icons-images-images/asset07.png#lightbox)
4. をクリックして、 **Apple TV エントリ x 1**イメージ ファイルを選択します。
5. 変更内容を保存します。

<a name="Dynamic-Top-Shelf-Content" />

### <a name="dynamic-top-shelf-content"></a>トップ シェルフの動的なコンテンツ

トップ シェルフ静的トップ シェルフのイメージを表示するには、代わりの動的な行を含めることができます[フォーカスを設定できる項目](~/ios/tvos/app-fundamentals/navigation-focus.md#Focus-and-Selection)またはバナーのスクロールの動的なセット。 これらの動的なスタイルの両方で、アプリや最も使用されているその機能へのジャンプが提供するコンテンツを強調表示できます。

<a name="Sectioned-Content-Row" />

#### <a name="sectioned-content-row"></a>それは区分けされたコンテンツの行

この動的トップ シェルフ コンテンツの種類は、1 行のスクロール、必要に応じてセクションに分かれてフォーカスを設定できる項目を表示します。 通常は、新しいお気に入りを強調表示に使用または、最近のアプリのコンテンツを表示します。

コンテンツは、(現在フォーカスがある) を選択したコンテンツの現在の下に表示されるラベルとコンテンツの 1 つの水平方向スクロール リストとして表示されます。 ユーザーは、特定のコンテンツを選択する場合は、アプリが起動され、その内容を直接撮影する必要があります。

次のコンテンツのサイズは、必要になります。

||ポスター (2:3)|正方形の (1:1)|HDTV (16:9)|
|---|---|---|---|
|実際のサイズ|404x608px|608x608px|908x512px|
|安全なゾーンのサイズ|380x570px|570x570px|852x479px|
|フォーカスされていないサイズ|333x500px|500x500px|782x440px|
|フォーカスのあるサイズ|380x570px|570x570px|852x479px|

Apple では、コンテンツの断面行の次の推奨事項を提供します。

- **行の完了**– 画面の幅全体にまたがるに十分なコンテンツを提供する必要があります。
- **混合イメージをスケーリング**– The 断面コンテンツの行は、イメージのサイズ (上記のリスト) からの組み合わせを保持するために設計されました。 ただし、イメージのサイズを混在させることは場合、は、コンテンツの表示を正規化する追加のスケーリングを適用、ことを認識します。

<a name="Scrolling-Inset-Banners" />

#### <a name="scrolling-inset-banners"></a>スクロールのマージン バナー

必要に応じて、Xamarin.tvOS アプリは自動的にスクロールおよび画面がほぼいっぱいバナーのコレクションをループとしてトップ シェルフでそのコンテンツを表示できます。 このスタイルは、新しいテレビ番組などの豊富な新しいコンテンツを紹介する通常使用されます。

自動スクロールだけでなくユーザーがバナーを制御し、Siri リモコンを使用していずれかの方向にスクロールできます。 バナーのフォーカスがあるときに Siri のリモートでの循環のジェスチャの小さなを行う、そのバナーの視差効果が有効になります。

**バナー イメージ (余分な全体)**

|   |   |
|---|---|
|実際のサイズ|1940x624px|
|安全なゾーンのサイズ|1740x620px|
|フォーカスされていないサイズ|1740x560px|
|フォーカスのあるサイズ|1740x620px|

スクロールのマージン バナーは、いずれかの静的なとして指定できます`.png`レイヤーまたは`.lsr`ファイル。

Apple では、スクロールのマージン バナーの次の推奨事項を提供します。

- **コンテンツの量**-自然にスクロールする 3 つ (3) バナーの最小値を提供する必要があります。 バナー個 8 (8) を含める必要がありますか、エンドユーザーのナビゲーションをハードにします。
- **テキストをコンテンツ**- 場合は、バナーは、内のテキストは、バナー イメージに含める必要がありますが必要です。 階層型イメージを使用している場合、テキストは、最上位層でなければなりません。

Apple を参照してください[TVServices フレームワーク参照](https://developer.apple.com/library/prerelease/tvos/documentation/TVServices/Reference/TVServices_Ref/index.html#//apple_ref/doc/uid/TP40016412)トップ シェルフの動的なコンテンツを提供する、アプリへの上位棚の拡張機能の追加の詳細についてはします。

<a name="Game-Center-Images" />

## <a name="game-center-images"></a>Game Center の画像

Xamarin.tvOS アプリが、ゲーム、Game Center のサポートが含まれている場合は、いくつかのイメージ アセットが必要になります。

- **アチーブメント アイコン**-非透過のイメージが自動的には円形にトリミングされる各アチーブメント必要です。 フォーカスを設定できる項目は。
- **ダッシュ ボードのアートワーク**-Game Center アプリのダッシュ ボードの上部に表示される指定されたオプションのイメージを指定できます。 これらのイメージは、フォーカスを設定します。
- **ランキング アートワーク**にする必要があります提供の間で 1 つ (1) に 3 つ (3) 16:9 の縦横比のイメージ、アプリをサポートする各ランキングの。 静的あります`.png`レイヤーまたは`.lsr`ファイル。 ランキング アートワークがフォーカスを設定できます。

||アチーブメントのアイコン|ダッシュ ボードのアートワーク|ランキングのアートワーク|
|---|---|---|---|
|表示サイズ|200x200px|923x150px|N/A|
|実際のサイズ|320x320px|N/A|659x371px|
|安全なゾーンのサイズ|N/A|N/A|618x348px|
|フォーカスされていないサイズ|N/A|N/A|548x309px|
|フォーカスのあるサイズ|N/A|N/A|618x348px|

Game Center の使用方法の詳細については、Apple を参照してください[ゲーム センターのプログラミング ガイド](https://developer.apple.com/library/prerelease/tvos/documentation/NetworkingInternet/Conceptual/GameKit_Guide/Introduction/Introduction.html)します。

<a name="Working-with-Images" />

## <a name="working-with-images"></a>イメージの処理

TvOS 9 は、iOS 9 のサブセットであるために、同じ手法を含めるし、Xamarin.iOS アプリにイメージを表示するために使用はも Xamarin.tvOS アプリの動作します。 参照してください、[イメージを表示する](~/ios/app-fundamentals/images-icons/displaying-an-image.md)詳細についてはドキュメントです。

<a name="Setting-Xamarin.tvOS-Project-Images" />

## <a name="setting-xamarintvos-project-images"></a>Xamarin.tvOS プロジェクト イメージの設定

前述のように、すべての tvOS アプリを必要とする[起動イメージ](#Launch-Image)と[アプリ アイコン](#App-Icons)。 このセクションでは、アプリ アイコンと起動イメージは、アセット カタログで設定した後、Xamarin.tvOS アプリ プロジェクトの選択について説明します。

次の手順で行います。

1. **ソリューション エクスプ ローラー**、ダブルクリックして、`Info.plist`編集用に開きます。 

    [![](icons-images-images/info01.png "Info.plist ファイル")](icons-images-images/info01.png#lightbox)
2. **Info.Plist エディター**、アセット カタログを選択します (上で構成された、[アプリ アイコン設定](#Setting-the-App-Icons)セクション) の**アプリ アイコン**: 

    [![](icons-images-images/info02.png "Info.Plist エディター")](icons-images-images/info02.png#lightbox)
3. 次に、アセット カタログを選択します (上で構成された、[起動イメージの設定](#Setting-the-Launch-Image)セクション) の**起動イメージ**。
4. 変更内容を保存します。

<a name="Summary" />

## <a name="summary"></a>Summary

この記事では、すべてのイメージの種類と Xamarin.tvOS アプリで使用されるサイズについて説明しました。 最初に、起動イメージ、イメージの階層化、アプリ アイコン、上部シェルフのイメージおよびゲーム センターのイメージについて説明します。 Xamarin.tvOS アプリでのイメージの操作を説明しました。

## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマン インターフェイス ガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS 用のアプリのプログラミング ガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
