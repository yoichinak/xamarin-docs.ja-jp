---
title: Xamarin での tvOS アイコンとイメージの操作
description: このドキュメントでは、Xamarin でビルドされた tvOS アプリでアイコンとイメージを操作する方法について説明します。 起動イメージ、レイヤー化されたイメージ、アプリアイコンなどについて説明します。
ms.prod: xamarin
ms.assetid: A2DA4347-0563-4C72-A8D7-5B9DE9E28712
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: 72c10d10e65194171479d66845d597e313281cdf
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/09/2020
ms.locfileid: "84573782"
---
# <a name="working-with-tvos-icons-and-images-in-xamarin"></a>Xamarin での tvOS アイコンとイメージの操作

魅了のアイコンと画像の作成は、Apple TV アプリのイマーシブユーザーエクスペリエンスを開発する上で重要な部分です。 このガイドでは、tvOS アプリに必要なグラフィック資産を作成して含めるために必要な手順について説明します。

- [[起動イメージ](#Launch-Image)]-起動イメージは、アプリの初回起動時に表示されます。起動が完了すると、アプリの最初の画面に置き換えられます。
- [レイヤー図](#Layered-Images): apple TV に固有の新しいレイヤー化されたイメージでは、視差効果を使用して、選択した項目の3d 効果を作成します。 複数の方法で[レイヤーイメージを作成](#Creating-Layered-Images)できます。
- [アプリアイコン](#App-Icons)-アイコンは、Apple TV ホーム画面だけでなく、アプリストアにも必要です。 これらは、レイヤー化されたイメージとして提供される必要があります。
- [[トップシェルフイメージ](#Top-Shelf-Image)]-アプリがホーム画面の一番上に配置されている場合は、アプリの機能を強調表示するために、一番上の棚のイメージが必要になります。 必要に応じて、アプリのコンテンツを強調表示するために、[動的な上位シェルフコンテンツ](#Dynamic-Top-Shelf-Content)を指定できます。
- [画像の Game Center](#Game-Center-Images) -アプリがゲームで、Game Center を使用する場合は、いくつかの追加のイメージが必要になります。
- [TvOS プロジェクトイメージの設定](#Setting-Xamarin.tvOS-Project-Images)-tvOS アプリの起動イメージとアプリアイコンを設定するために必要な手順について説明します。

> [!IMPORTANT]
> Apple TV 上のすべてのイメージは1倍の解像度 ( `@1x` ) であり、このサイズのイメージ_のみ_を使用する必要があります。 より大きな解像度のグラフィックスを含めて、より多くのメモリとストレージをダウンロードして使用するだけでなく、実行時に動的にスケールする必要があり、描画のパフォーマンスに悪影響を及ぼします。

<a name="Launch-Image"></a>

## <a name="launch-image"></a>起動イメージ

起動イメージは、tvOS アプリが Apple TV で最初に起動されたときに最初に表示されます。そのため、すべての tvOS アプリが起動イメージを提供する必要があります。 

起動イメージがすぐに表示され、アプリの速度と応答性が向上します。 Apple TV は、起動イメージを、後すぐにアプリの最初の画面に置き換えます。

起動イメージは広告や芸術表現の機会ではなく、アプリがすぐに起動して使用できるようになるという印象を与えるためだけに存在します。

|起動イメージのサイズ|メモ|
|---|---|
|1920x1080px|階層化していない .png ファイルのみ|

Apple では、アプリの起動イメージを設計するために次の提案を行います。

- **最初の画面とほぼ同じ**-起動画面は、アプリの最初の画面のできるだけ近くに配置する必要があります。 異なるグラフィックスまたは要素を追加すると、最初の画面が表示されたときに面倒な "flash" が発生する可能性があります。
- **テキスト**起動イメージの使用は避けてください。そのため、表示される前にはローカライズされません。
- **Downplay の起動**-Apple TV ユーザーはアプリを頻繁に切り替えるため、アプリの起動プロセスに注意する必要はありません。
- **広告やブランド**化がない-起動イメージは、アプリの最初の画面の静的な部分でない限り、About 画面として使用したり、ブランド化を含めたりすることはできません。 広告は、厳密には禁止されています。

<a name="Setting-the-Launch-Image"></a>

### <a name="setting-the-launch-image"></a>起動イメージの設定

TvOS プロジェクトの起動イメージを設定するには、次の手順を実行します。

1. **ソリューションエクスプローラー**で、ダブルクリックし `Assets.xcassets` て開き、編集します。 

    [![](icons-images-images/asset01.png "The Assets.xcassets file")](icons-images-images/asset01.png#lightbox)
2. **アセットエディター**で、資産をクリックし `LaunchImages` ます。 

    [![](icons-images-images/asset02.png "The LaunchImages asset")](icons-images-images/asset02.png#lightbox)
3. [ **1X APPLE TV** ] エントリをクリックし、起動イメージを選択するか、またはファイルシステムから新しいイメージをドラッグします。 

    [![](icons-images-images/asset03.png "Select a Launch Image")](icons-images-images/asset03.png#lightbox)
4. 変更内容を保存します。

<a name="Layered-Images"></a>

## <a name="layered-images"></a>レイヤー図

Apple TV の新機能であるレイヤー化されたイメージは、視差効果を使用して、部屋の中の画面のコンテンツに接続された、ソファの頭にいるユーザーを維持するための3D 効果を生み出します。

レイヤー化されたイメージには、完全なイメージを形成するために結合された 2 ~ 5 個の独立したレイヤーが含まれています。 背景層を除き、各レイヤーは、その Z オーダーと透明度を使用して、奥行を作成します。 ユーザーがレイヤー図を操作すると、より高い Z オーダーレイヤーが拡大縮小され、重なってこの効果が作成されます。

[![](icons-images-images/layered01.png "Layered Images Z-ordered diagram")](icons-images-images/layered01.png#lightbox)

> [!IMPORTANT]
> レイヤー化されたイメージはアプリのアイコンに必要であり、他のフォーカス可能な[項目](~/ios/tvos/app-fundamentals/navigation-focus.md#Focus-and-Selection)(上部の棚の画像など) ではオプションです。 しかし、Apple では、アプリにフォーカスを移すことができるすべてのイメージに対して、レイヤー化されたイメージを使用することが提案されて

Apple では、階層化されたイメージを設計するために次の提案を行います。

- **背景レイヤーを不透明**にする-背景レイヤー (レイヤー 1) を不透明にする**必要があり**ます。または、Apple TV でレイヤーイメージを使用しようとするとエラーが発生します。 他のすべてのレイヤーには、3D 効果を向上させるために複数の透明度レベルを含めることができます。
- **前景、中間、および背景の要素を分離**する (ゲーム文字など) 前景の要素は、2番目の要素または影に中央を使用します。 最後に、上位層のステージを提供するために、ニュートラルな背景を含めます。
- **テキストを前景に保持**する-テキストをより高いレベルで非表示にする場合は、通常は最上位のレイヤーにする必要があります。
- **単純なレイヤーを使用**する-視差効果は微妙になるように設計されているので、jarring や非現実的な効果を防ぐためにレイヤーを最小限に抑えることができます。
- **セーフゾーンを含める**-視差効果の際に上位レイヤーをトリミングできるため、各レイヤーに安全なゾーンの境界線を作成する必要があります。 コンテンツを取得したときにレイヤーの端を閉じると、トリミングされることがあります。 上位レイヤーでは、下位レイヤーよりも拡大縮小とトリミングが発生します。 下の「[イメージレイヤーのサイズ変更](#Sizing-Image-Layers)」セクションを参照してください。
- **プレビュー頻繁**: レイヤー図は、必要な3d 効果が発生し、個々のレイヤーのコンテンツがトリミングされていないことを確認するために、頻繁にプレビューする必要があります。 実際の Apple TV ハードウェアでレイヤー図をプレビューし、予想どおりにレンダリングされることを確認する必要があります。

可能な場合は常に、組み込みのコントロールを使用して、 `UIKit` レイヤー化されたイメージを表示する必要があります。これは、視差がフォーカスされたときに自動的に効果が得られるためです。

<a name="Sizing-Image-Layers"></a>

### <a name="sizing-image-layers"></a>イメージレイヤーのサイズ変更

階層化されたイメージを構成する各レイヤーには、_安全なゾーン_の境界線を含めることが重要です。 視差効果の際に個々のレイヤーをスケーリングおよびトリミングできるため、レイヤーの端に近すぎる場合はレイヤーのコンテンツをトリミングできます。

[![](icons-images-images/layered02.png "35 pixel border")](icons-images-images/layered02.png#lightbox)

<a name="Creating-Layered-Images"></a>

### <a name="creating-layered-images"></a>レイヤー図の作成

tvOS は、次の形式のレイヤーイメージで動作します。

- **車両ファイル**-Apple によって作成された独自のアセットカタログ形式です。 車両ファイルは直接作成されず、コンパイル時に任意の LSR ファイルから作成され、アプリバンドルに含まれます。
- **Lsr イメージ**-これは Apple によって作成された独自のイメージ形式です。 [視差エクスポーター Adobe Photoshop プラグイン](https://itunespartner.apple.com/assets/downloads/ParallaxExporter_Apps.zip)または[視差プレビューアー](https://itunespartner.apple.com/assets/downloads/Parallax%20Previewer.dmg)を使用して、Lsr 形式でレイヤー化されたイメージを作成およびプレビューします。
- **Assets** (2) から 5 (5) の標準書式設定され `.png` たイメージから、コンパイル時に CAR または lsr でフォーマットされたレイヤー化イメージにコンパイルされる、アセットカタログに含まれる2つの形式の画像。
- **LCR ファイル**-Apple によって作成された独自のファイル形式です。 LCR ファイルは、コンテンツサーバーの1つからダウンロードした追加のコンテンツとして使用することを目的としています。 LCR ファイルは、アプリバンドルに含めないでください。 LCR ファイルは、 `layerutil` Xcode に付属のコマンドラインツールを使用して、LSR または Photoshop ファイルから生成されます。

<a name="The-Parallax-Previewer"></a>

### <a name="the-parallax-previewer"></a>視差プレビューアー

Apple は、アプリアイコンおよびオプションのフォーカス可能な項目に必要なレイヤーイメージをプレビューおよび作成するために[視差プレビューアー](https://itunespartner.apple.com/assets/downloads/Parallax%20Previewer.dmg)を作成しました。 次のように、完成したレイヤードイメージを形成するすべてのレイヤーがプレビューアーによって表示されます。

[![](icons-images-images/layered03.png "The Parallax Previewer")](icons-images-images/layered03.png#lightbox)

レイヤー化されたイメージをプレビューするときに、マウスを使用してイメージを回転し、視差効果をプレビューすることができます。 **+** **-** レイヤーを追加および削除するには、(プラス) ボタンと (マイナス) ボタンを使用します。

新しいレイヤー図を作成するときには、LSR 形式でエクスポートし、アプリのバンドルに含めることができます。

階層化されたイメージの作成とプレビューの詳細については、 [tvOS のアプリプログラミングガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)の Apple の[視差アートワークの作成](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/CreatingParallaxArtwork.html#//apple_ref/doc/uid/TP40015241-CH19-SW1)に関するセクションを参照してください。

<a name="App-Icons"></a>

## <a name="app-icons"></a>アプリアイコン

TvOS アプリでは、Apple TV ホーム画面のアプリアイコンだけでなく、アプリストアのアイコンも必要になります。 アプリアイコンは、ユーザーにとって優れた印象を得られるようにするための最初の変更であり、アプリの目的を一目で伝えなければなりません。

[![](icons-images-images/icon01.png "The App Icon")](icons-images-images/icon01.png#lightbox)

すべてのアプリで、アプリアイコンの小さいバージョンと大きなバージョンの両方を提供する必要があります。 小さいアイコンは、アプリのインストール時に、Apple TV ホーム画面で使用されます。 大規模なバージョンは、App Store によって使用されます。 大きいアプリアイコンは、小さいアイコンバージョンのルックアンドフィールを模倣します。

|小さいアイコン||大きいアイコン||
|---|---|---|---|
|原寸大|400x240px|サイズ|1280x768px|
|セーフゾーンのサイズ|370x222px|||
|見るサイズ|300x180px|||
|フォーカスサイズ|370x222px|||

> [!IMPORTANT]
> アプリアイコンは、レイヤー化された**イメージ**として指定する必要があります。 詳細については、上の「[レイヤー図](#Layered-Images)」セクションを参照してください。

Apple では、アプリアイコンの作成に関して次の提案が提供されています。

- **1 つのフォーカスポイントを提供する**–アイコンの中央に直接配置された単一のフォーカスポイントを使用して、アイコンをデザインします。
- **単純な背景を使用**します。上位のレイヤーが目立つように、アイコンの背景を単純なままにしておきます。単純な色または微妙なグラデーションの使用を検討してください。
- **テキストの量を制限**する–アプリの名前は、ユーザーが選択したときにアイコンの下に表示されるため、アイコンのデザインに不可欠な場合にのみ、テキストを含める必要があります。
- **スクリーンショットを使用しないでください**。スクリーンショットはアイコンに対して複雑すぎるため、ユーザーはアプリの目的を一目で確認することができません。
- **アイコン正方形を保持**する– tvOS は、アイコンの角を微妙に丸めるマスクを自動的に適用します。 この丸め処理は含めないでください。
- **レイヤーを慎重に分割**する–最上位のレイヤー、中央の2番目の項目、および上位のレイヤーを表示できるようにする中間色の背景にテキストを配置します。
- **グラデーションと影を注意深く使用**する–グラデーションと影は視差効果と競合しているため、慎重に使用する必要があります。 単純な上から下、薄く、濃いグラデーションのスタイルが最適です。 影は、通常はシャープで、非常にエッジが鮮明な濃淡として機能します。
- **レイヤーの透明度を変更**する–アプリアイコンの上位レベルでさまざまなレベルの透明度を使用して、3d 効果を向上させます。 背景層は不透明である必要があります。そうしないと、エラーが発生します。

<a name="Setting-the-App-Icons"></a>

### <a name="setting-the-app-icons"></a>アプリアイコンの設定

TvOS プロジェクトに必要なアプリアイコンを設定するには、次の手順を実行します。

1. **ソリューションエクスプローラー**で、ダブルクリックし `Assets.xcassets` て開き、編集します。 

    [![](icons-images-images/asset01.png "The Assets.xcassets fileg")](icons-images-images/asset01.png#lightbox)
2. **アセットエディター**で、資産を展開し `App Icon & Top Shelf Image` ます。 

    [![](icons-images-images/asset04.png "Expand the Top Shelf Image asset")](icons-images-images/asset04.png#lightbox)
3. 次に、資産を展開し `App Icon - Small` ます。 

    [![](icons-images-images/asset05.png "Expand the App Icon - Small asset")](icons-images-images/asset05.png#lightbox)
4. 次に、 `Back` 資産を展開し、エントリをクリックし `Contents` ます。 

    [![](icons-images-images/asset06.png "Then expand the Back asset")](icons-images-images/asset06.png#lightbox)
5. [ **1X APPLE TV] エントリ**をクリックし、画像ファイルを選択します。
6. およびアセットに対して上記の手順を繰り返し `Front` `Middle` ます。
7. 次に、同じ手順を繰り返して資産を定義し `App Icon - Large` ます。
8. 変更内容を保存します。

<a name="Top-Shelf-Image"></a>

## <a name="top-shelf-image"></a>上部の棚の画像

ユーザーが Apple TV ホーム画面の一番上の行に tvOS アプリを配置した場合は、ユーザーがアプリを選択すると、大きな上部の棚の画像が表示されます。 このイメージは、アプリの機能を強調表示したり、コンテンツへの直接リンクを提供したりする必要があります。

[![](icons-images-images/topshelf01.png "Top Shelf Image example")](icons-images-images/topshelf01.png#lightbox)

一番上の棚のイメージは、1つの静的またはファイルとして提供するか `.png` `.lsr` (「[レイヤーイメージの作成](#Creating-Layered-Images)」を参照)、または実行時にフォーカス可能な項目の単一行として動的に作成することができます (以下の「[動的な上位シェルフコンテンツ](#Dynamic-Top-Shelf-Content)」を参照してください)。

|上部の棚の画像のサイズ|メモ|
|---|---|
|1920x720px|静的な .png またはレイヤードファイル|

Apple では、トップシェルフイメージを作成するための次の提案が提供されています。

- **リッチな静的な画像を使用する**–アプリが動的なコンテンツを提供しない場合、そのトップシェルフのイメージはフォーカスを設定できません。 このイメージを使用して、アプリまたはブランドの機能を強調表示します。
- **アプリコンテンツへのリンク**–動的な上位シェルフレイアウトは、ユーザーがアプリで最も重要なコンテンツへのクイックリンクを提供します。 この領域を使用して、アプリを起動し、指定されたコンテンツにすぐにジャンプするためのクイックリンクを提供します。
- **最新のコンテンツを**紹介します。リッチな上位シェルフコンテンツを使用すると、アプリにユーザーを描画して、より多くのものを使用することができます。 これを領域として使用して、評価されたコンテンツの最大値を示すことができます。
- **パーソナライズ**されたコンテンツ–ユーザーが最も頻繁に使用するアプリまたはお気に入りのアプリをホーム画面の一番上の行に配置します。 最も関心のあるコンテンツを表示するには、上部の棚を使用します。
- **広告は使用できません**。広告は、一番上の棚に表示されることを厳密に禁止されています。 最新の購入可能なコンテンツを表示することはできますが、価格情報は表示されません。

### <a name="setting-the-top-shelf-image"></a>上部の棚の画像の設定

TvOS プロジェクトに必要な最大のシェルフイメージを設定するには、次の手順を実行します。

1. **ソリューションエクスプローラー**で、ダブルクリックし `Assets.xcassets` て開き、編集します。 

    [![](icons-images-images/asset01.png "The Assets.xcassets file")](icons-images-images/asset01.png#lightbox)
2. **アセットエディター**で、資産を展開し `App Icon & Top Shelf Image` ます。 

    [![](icons-images-images/asset04.png "Expand the Top Shelf Image asset")](icons-images-images/asset04.png#lightbox)
3. 資産をクリックし `Top Shelf Image` ます。 

    [![](icons-images-images/asset07.png "The Top Shelf Image asset")](icons-images-images/asset07.png#lightbox)
4. [ **1X APPLE TV] エントリ**をクリックし、画像ファイルを選択します。
5. 変更内容を保存します。

<a name="Dynamic-Top-Shelf-Content"></a>

### <a name="dynamic-top-shelf-content"></a>動的なトップシェルフコンテンツ

最上位シェルフには、静的なトップシェルフイメージを表示する代わりに、[フォーカス](~/ios/tvos/app-fundamentals/navigation-focus.md#Focus-and-Selection)可能な項目の動的な行、またはスクロール用の動的な一連のバナーを含めることができます。 どちらの動的スタイルでも、アプリによって提供されるコンテンツを強調表示したり、最も使用されている機能にジャンプしたりすることができます。

<a name="Sectioned-Content-Row"></a>

#### <a name="sectioned-content-row"></a>区分けしたコンテンツ行

この動的なトップシェルフコンテンツタイプでは、スクロール可能な項目を1行ずつ表示し、オプションでセクションに分割することができます。 通常は、新規、お気に入り、または最近表示したアプリのコンテンツを強調表示するために使用されます。

コンテンツは、選択したコンテンツの現在の部分 (現在フォーカスがあるコンテンツ) の下にラベルを表示した、コンテンツの単一の水平スクロールリストとして表示されます。 ユーザーが特定のコンテンツを選択すると、アプリが起動され、そのコンテンツに直接移動する必要があります。

次のコンテンツサイズが必要になります。

||ポスター (2:3)|四角 (1:1)|HDTV (16:9)|
|---|---|---|---|
|原寸大|404x608px|608x608px|908x512px|
|セーフゾーンのサイズ|380x570px|570x570px|852x479 px|
|見るサイズ|333x500px|500x500px|782x440px|
|フォーカスサイズ|380x570px|570x570px|852x479 px|

Apple では、次の提案されたコンテンツ行に対して次の提案を提供しています。

- **行を完成**させます。画面全体の幅を広げるために十分なコンテンツを提供する必要があります。
- **混合画像の拡大/縮小**: 表示されるコンテンツ行は、イメージサイズの組み合わせを保持するように設計されています (上記のリストから)。 ただし、イメージのサイズを混合する場合は、コンテンツの表示を正規化するために追加のスケーリングが適用されることに注意してください。

<a name="Scrolling-Inset-Banners"></a>

#### <a name="scrolling-inset-banners"></a>スクロール (リボンを)

必要に応じて、tvOS アプリは、画面をほぼ塗りつぶすバナーのコレクションとして、自動的にスクロールとループを行うことで、そのコンテンツをトップシェルフに提示することができます。 このスタイルは、通常、新しいテレビ番組などのリッチな新しいコンテンツを示すために使用されます。

自動スクロールに加えて、ユーザーは、Siri リモートを使用して、バナーを制御し、いずれかの方向にスクロールすることができます。 バナーがフォーカスされているときに、Siri リモートで小さな円形のジェスチャを行うと、そのバナーの視差効果がアクティブになります。

**バナーイメージ (特大)**

|   |   |
|---|---|
|原寸大|1940x624px|
|セーフゾーンのサイズ|1740x620px|
|見るサイズ|1740x560px|
|フォーカスサイズ|1740x620px|

スクロール用のバナーは、静的なファイルまたは階層化されたファイルとして提供でき `.png` `.lsr` ます。

Apple では、スクロール用のバナーに関して次の提案が提供されています。

- **コンテンツの量**-自然な外観にするには、少なくとも3つのバナーを指定する必要があります。 最大8つのバナーを含めることも、エンドユーザーに対してナビゲーションを難しくすることもできます。
- **コンテンツテキスト**-バナーのテキストが必要な場合は、バナーイメージに含める必要があります。 階層化されたイメージを使用する場合、テキストは最上位のレイヤーに配置する必要があります。

アプリに上位棚拡張機能を追加して、動的な上位シェルフコンテンツを提供する方法の詳細については、Apple の[TVServices Framework リファレンス](https://developer.apple.com/library/prerelease/tvos/documentation/TVServices/Reference/TVServices_Ref/index.html#//apple_ref/doc/uid/TP40016412)を参照してください。

<a name="Game-Center-Images"></a>

## <a name="game-center-images"></a>Game Center イメージ

TvOS アプリがゲームであり、Game Center サポートが含まれている場合は、さらにいくつかのイメージ資産が必要になります。

- **アチーブメントアイコン**-円形に自動的にトリミングされる各アチーブメントに対し、不透明な画像が必要です。 アチーブメントはフォーカスできない項目です。
- **ダッシュボードのアートワーク**-Game Center 内のアプリのダッシュボードの上部に表示されるオプションのイメージを指定できます。 これらのイメージにはフォーカスがありません。
- **ランキングアートボード**: アプリでサポートされている各ランキングに対し、1 ~ 3 個の16:9 縦横比イメージを指定する必要があります。 これらは、静的なファイルでも、階層化されたファイルでもかまい `.png` `.lsr` ません。 スコアボードのアートワークはフォーカスを持つことができます。

||アチーブメントアイコン|ダッシュボードのアートワーク|ランキングアートワーク|
|---|---|---|---|
|表示サイズ|200x200px|923x150 px|該当なし|
|原寸大|320x320px|該当なし|659x371px|
|セーフゾーンのサイズ|該当なし|該当なし|618x348px|
|見るサイズ|該当なし|該当なし|548x309px|
|フォーカスサイズ|該当なし|該当なし|618x348px|

Game Center の操作の詳細については、「Apple の[Game Center プログラミングガイド](https://developer.apple.com/library/prerelease/tvos/documentation/NetworkingInternet/Conceptual/GameKit_Guide/Introduction/Introduction.html)」を参照してください。

<a name="Working-with-Images"></a>

## <a name="working-with-images"></a>イメージの処理

TvOS 9 は iOS 9 のサブセットであるため、Xamarin. iOS アプリでのイメージの追加と表示に使用されているものと同じ手法が tvOS アプリでも機能します。 詳細について[は、イメージの表示に](~/ios/app-fundamentals/images-icons/displaying-an-image.md)関するドキュメントを参照してください。

<a name="Setting-Xamarin.tvOS-Project-Images"></a>

## <a name="setting-xamarintvos-project-images"></a>TvOS プロジェクトイメージの設定

前述のように、すべての tvOS アプリには[起動イメージ](#Launch-Image)と[アプリアイコン](#App-Icons)が必要です。 このセクションでは、tvOS App プロジェクトがアセットカタログで設定された後に、そのプロジェクトの起動イメージとアプリアイコンを選択する方法について説明します。

次の操作を行います。

1. **ソリューションエクスプローラー**で、をダブルクリックし `Info.plist` て開き、編集します。 

    [![](icons-images-images/info01.png "The Info.plist file")](icons-images-images/info01.png#lightbox)
2. 次のように、**アプリのアイコン**について、**情報エディター**で、[[アプリアイコンの設定](#Setting-the-App-Icons)] セクションで構成した Assets カタログを選択します。 

    [![](icons-images-images/info02.png "The Info.Plist Editor")](icons-images-images/info02.png#lightbox)
3. 次に、**起動イメージ**のアセットカタログ ([[起動イメージの設定](#Setting-the-Launch-Image)] セクションで構成したもの) を選択します。
4. 変更内容を保存します。

<a name="Summary"></a>

## <a name="summary"></a>まとめ

この記事では、tvOS アプリで使用されるすべてのイメージの種類とサイズについて説明しました。 まず、起動イメージ、階層化されたイメージ、アプリアイコン、上部の棚イメージ、Game Center イメージについて説明します。 次に、tvOS アプリのイメージの操作について説明します。

## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマンインターフェイスガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS のアプリプログラミングガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
