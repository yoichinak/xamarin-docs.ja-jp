---
title: Xamarin.iOS の色
description: このドキュメントでは、色と Xamarin.iOS または Xamarin.Mac アプリでの使用方法について説明します。 また、多くの重要な色に関連する概念のカラー スペース、チャネル、およびプライマリなどの高度な概要を提供します。
ms.prod: xamarin
ms.assetid: 576E978A-F182-489A-83E4-D8CDC6890B24
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: f139bcceda12752e43a3a8330fa0a0e038e539f9
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50121309"
---
# <a name="wide-color-in-xamarinios"></a>Xamarin.iOS の色

_この記事では、色と Xamarin.iOS または Xamarin.Mac アプリでの使用方法について説明します。_

iOS 10 デバイスと macOS Sierra 拡張範囲のピクセル形式とコア グラフィックス、Core のイメージ、金属製および AVFoundation などのフレームワークを含めて、システム全体で全体の色域スペースのサポートが強化されます。 ワイド カラー ディスプレイを使用したデバイスのサポートはさらに、全体のグラフィックス スタック全体でこの動作を提供することで緩和されました。

## <a name="about-wide-color"></a>色について

前述のように、iOS 10 デバイスと macOS Sierra 拡張範囲のピクセル形式とコア グラフィックス、Core のイメージ、金属製および AVFoundation などのフレームワークを含む、システム全体で全体の色域スペースのサポートが強化されます。 ワイド カラー ディスプレイを使用したデバイスのサポートはさらに、全体のグラフィックス スタック全体でこの動作を提供することで緩和されました。

年代 Apple が mac で処理する色を処理するために ColorSync を作成しました。 作成し、コンピューターのハードウェアでの色を処理するための標準のセットを昇格する International Color Consortium (ICC) の検出もされました。 ColorSync には ICC で Apple の作業に含まれており、(macOS と呼ばれるようになりました)、OS X の中核に組み込まれていることができます。

Apple は、Retina ディスプレイ新しい P3 の表示と、表示 P3 色領域 (2015 リリースで、iMac) などのハードウェアで表示テクノロジの最前線にもされたため、iPad、iPhone 7 と iPhone 7 で表示されます、TrueTone とします。

MacOS Sierra の色と iOS 10 では、Apple は iOS と macOS の両方がこれらの新しい表示テクノロジを活用するために色を扱う方法を変更します。

## <a name="core-color-concepts"></a>色の中心概念

次のコアの色の概念は、macOS、および iOS での色について詳しく説明を取得する前に説明する必要があります。

### <a name="color-space"></a>色空間

色空間とは、環境の色を表すし、比較します。 色成分の強度によって定義されている、1 ~ 4 次元空間であることができます。 

[![](wide-color-images/color00.png "色空間")](wide-color-images/color00.png#lightbox)

### <a name="color-channels"></a>カラー チャネル

色のコンポーネントは、カラー チャネルとしてにも参照できます。 使い慣れた表記の一部は、RGB スペース、灰色のスペース、CMYK スペースまたはデバイスの独立したスペースになります。 

[![](wide-color-images/color02.png "カラー チャネルとしての色要素を参照することができますも")](wide-color-images/color02.png#lightbox)

### <a name="color-primaries"></a>色の原色

色の原色を比較し、色を計算するために使用する座標系を指定します。 通常、色の原色には、カラー チャネル内で生成できる、指定された色の最も負荷の高いバージョンが分類されます。

[![](wide-color-images/color01.png "色の原色を比較し、色を計算するために使用する座標系の指定します。")](wide-color-images/color01.png#lightbox)

上で表される、RGB 色空間では、色の原色が where、`1.0`座標はアンカー (など`[1.0, 0.0, 0.0]`赤の場合)。

### <a name="color-gamut"></a>広色域

広色域は、特定の色空間内の個々 の色チャネルの組み合わせとして定義できる色のすべてを指します。

[![](wide-color-images/color03.png "色域の使用例")](wide-color-images/color03.png#lightbox)

## <a name="what-is-wide-color"></a>色とは

色のトピックをカバーする前に現在の業界標準の RGB 色空間 (sRGB) の色の標準に関するディスカッションを得られる必要があります。 現在のコンピューティングで最も広く使用されている色空間と、iOS および macOS の既定の色空間です。

SRGB 色空間では、次のプロパティがあります。

- ITU-R BT.709 標準に基づいています。
- おおよそのガンマ値 2.2 があります。
- これは、一般的なライティング条件 (D65) を表します。

SRGB は広く業界で使用すると、開発者できることをいくつかの前提条件であるために表示される任意のデバイスで指定した色を忠実に表されます。 ただし、これが常にできない場合。 また、特定の sRGB 色空間に適合しないし、そのため、これで表現できないいくつかの色が使用されます。

たとえば、多くのインクと以下の sRGB の外部にある染料を使用してスレッドを多数繊維が設計されています。 日常業務で人が発生した多くの製品は、sRGB 色空間の外部にある明るい鮮やかな色で作成されます。 いくつかの sRGB で表せない色の最も魅力的な例は、日没、秋のリーフ、風変わりな花 tropical 歩などの性質を持つもの。

RAW 形式でデジタル イメージのキャプチャされているユーザーは、イメージを下げてできません正しく画面に表示される、sRGB 色空間で正しく表現できない場合でも、この色データのすべてが含まれている自分のデバイスでがあります。

### <a name="the-display-p3-color-space"></a>Display P3 の色空間

2015 では、Apple は、sRGB 色空間によって作成された問題に対処する新しい Display P3 色空間を提供する新しい製品 (iMac および iPad Pro 9.7 インチ) をリリースします。

[![](wide-color-images/color04.png "新しい Display P3 色空間")](wide-color-images/color04.png#lightbox)

Display P3 の色空間では、次のプロパティがあります。

- 最新のハードウェア プラットフォームの色領域をサポートしています。
- SMPTE DCI-P3 標準に基づいています。 DCI P3 はデジタル プロジェクター用に設計されましたが、モニターをサポートするために Apple によって変更されました。
- おおよそのガンマ値 2.2 があります。
- これは、一般的なライティング条件 (D65) を表します。

Apple、に従ってときは、自分のモバイル プラットフォームにユーザーが、ワークフローを移動しています。 単に色の表示を含めて、必要なプロフェッショナルなモバイル デバイス (iPad プロフェッショナル) での sRGB によって提示される色プレゼンテーションの問題の解決。 各デバイスのデバイスから、色の表示が正確で一貫性のある工場出荷時のことを調整が完了するために、工場出荷時の調整をアップグレードすると、ソリューションのいずれかでした。

別のソリューションは、Apple は iOS 10 デバイスと macOS Sierra 組み込まれた、完全なシステム全体の色の管理を含めることです。 

### <a name="the-extended-range-srgb-color-space"></a>Extended Range sRGB 色空間

新しい iOS 10 システム全体の色の管理は、アカウントのすべての既存の iOS アプリをビルドして、sRGB、微調整する必要があります。 これらのアプリのいずれかの色形式またはアプリ パフォーマンスに影響していないことを確認することがあります。

このような状況を処理するには、Apple は、iOS 10 では、Extended Range sRGB 色空間 (および macOS Sierra も) に含まれます。

Extended Range sRGB 色空間では、次のプロパティがあります。

- すべての同じ sRGB プライマリがあります。
- おおよそのガンマ値 2.2 があります。
- これは、一般的なライティング条件 (D65) を表します。
- 負の値をサポートし、1 (1) よりも大きい値します。

0 より小さい値可能にして、1 より大きい Extended Range sRGB 色空間は、パフォーマンス ヒットまたは、歪みがない sRGB に存在する色を既存のアプリだけでなくで許可を表示内の任意の色を表す色空間スペクトルします。 このすべては、sRGB 色空間と同じアンカー ポイントを保持しながら実行されます。

### <a name="extended-range-srgb-in-action"></a>アクションの拡張の Range sRGB

0 と 1 つ外の値が、Extended Range sRGB 色空間でどのように機能するかを確認するための次の例は、Display P3 色空間では最も鮮やかの赤の。

[![](wide-color-images/color05.png "0 と 1 つ外の値が、Extended Range sRGB 色空間でどのように機能するか")](wide-color-images/color05.png#lightbox)

Display P3 でとしてこの色を表すは`[1.0, 0.0, 0.0]`となります Extended Range sRGB`[1.358, -0.074, -0.012]`します。 SRGB 値は、完全なので、Display P3 内に含まれていると Display P3 値のレイアウトを「外部」sRGB 範囲の。

極端な正の値から負の値を極端に移行するピクセルの値が許可される物理ハードウェアには、可視スペクトルで使用可能な任意の色を表示することし、これらの値は、Extended Range sRGB 色空間で表すことができます。

### <a name="device-pixel-formats"></a>デバイスのピクセル形式 

カラー チャネルあたり 8 ビットはほとんどの場合であるために、8 ビットのピクセル形式を使用して、sRGB 色空間が大きく標準化の sRGB 色を記述するのに十分な。 完璧なが十分でないこれし、イメージを表示するメモリおよびプロセッサの使用量とのトレードオフを提供します。

Display P3、sRGB 色空間の外部の色の座標を表すことのできるため、Extended Range sRGB 色空間での色を正しく表すカラー チャネルあたり 16 ビットが必要です。

## <a name="system-wide-wide-color-support"></a>システム全体の幅の色のサポート

色と iOS 10 の内部で作る macOS Sierra を完全にサポート、Apple が Display P3 と Extended Range sRGB 色空間をフルに活用する次のフレームワークを拡張します。

- (IOS のみ) の UIKit
- SceneKit
- コア グラフィックス
- ImageIO
- Core イメージ
- WebKit
- SpriteKit
- コア アニメーション
- AppKit で macOS のみ)

さらに、Extended Range sRGB 色空間のサポートが強化されている Retina ディスプレイと Display P3 を表示します。

ワイド色はサポートされているし、次のアプリケーション コンテンツの種類で使用できます。

- アプリ バンドルに含まれる静的イメージ リソース。
- ドキュメントとネットワーク ベースのイメージ リソースを実行します。
- ライブの写真や生の形式のイメージなどの高度なメディア。
- 3D グラフィックス シェーダーのテクスチャ画像。

## <a name="solving-the-color-problem"></a>色の問題を解決します。

アプリに表示されるコンテンツは、さまざまな色が豊富なソースから取得できます。 さらに、このコンテンツは、それぞれに独自の範囲の色の表示機能、デバイスの広範な範囲で表示できます。

IOS 10 アプリでは、新しい組み込み、システム全体の色の管理システムを使用して、これら 2 つの問題の違いを橋渡しします。 このシステムにより、イメージに、同じ外観でエンコードされたイメージの色領域に関係なく、すべての iOS デバイスようになります。

色の管理は、関連付けられている色空間 (またはカラー プロファイル) を持つすべてのイメージで開始します。 この情報が使用される、_色に一致するプロセス_出力デバイスで使用する色、ソース イメージの色が一致する場所。 イメージのすべてのピクセルは、色と一致する必要があります、ため時間がかかる場合およびデバイスの CPU に負担を配置できます。

性質により、_色に一致するプロセス_、可能性のある"損失を伴う場合、出力デバイスにソース イメージよりも小さい域この変換を指定できます。

移動して、計算さいわい、_色に一致するプロセス_できます簡単にハードウェア高速化 (か、CPU または GPU 上) と Apple により Quartz 2 D などの基本システムにサポートを組み込むことで自動的に動作するようになりますColorSync とコアのアニメーション。 正しくタグ付けされたコンテンツは、コーディングは必要ありませんこれらの機能を活用します。

色の管理は、各プラットフォームでサポートされています。

- **macOS** -macOS が誕生管理されている色をされています。
- **iOS** -iOS は iOS 9.3 (以降) から色の自動管理をサポートします。

### <a name="designing-for-wide-gamut"></a>作るの設計

Apple には、次の提案の設計と色ワイドを使用して、iOS および macOS のアプリにイメージ コンテンツを作る。

- 場所で作るコンテンツを使用してのみが使用できない場合が自動的にすべての場所で、アプリで検出します。
- のみ鮮明な色がユーザー エクスペリエンスを向上する場所を作るのコンテンツに使用します。
- する必要はありません P3 に既存のアプリのすべてのコンテンツを変更します。

Apple のツール チェーンは、オール_オア_ナッシングの状況でないアプリで色をサポートしているため、段階的なオプトイン作るイメージのコンテンツをサポートします。

### <a name="upgrading-existing-content-to-wide-color"></a>色に既存のコンテンツをアップグレードします。

Apple では、さまざまな色に既存のイメージのコンテンツをアップグレードするための次の推奨事項があります。

- だけ「割り当てないでください」P3 プロファイル画像編集アプリ内のコンテンツにします。 これにより、拡大でき、イメージの新しいスペースに収まるように色などの予期しない結果に新しい色領域へのコンテンツの既存の色は単に再マップします。
- イメージのコンテンツは、画像編集アプリを使用して、Display P3 プロファイルに「変換」する必要があります。

### <a name="file-formats-and-color-profiles"></a>ファイル形式とカラー プロファイル

Apple では、ファイル形式と、アプリの色の画像のコンテンツで使用されているカラー プロファイルの次の方法があります。

- RGB 作業スペース"Display P3"カラー プロファイルを使用します。
- カラー チャネルのモードごとに 16 ビットを使用します。
- 遅延 2015 iMac を使用して (またはそれ以降) を正確にイメージのコンテンツをプレビューします。
- 埋め込みの"Display P3"ICC プロファイルの 16 ビットの PNG ファイルとしてのイメージ アセットをエクスポートします。

> [!IMPORTANT]
> 使用して、 **for Web に保存**または**エクスポート資産**機能は、最も一般的な画像編集ソフトウェアで見つかった_されません_これらの機能がされていないために、ワイド カラー イメージに対して動作まだ必要なファイル形式の仕様をサポートするために更新されます。

### <a name="supporting-wide-color-with-asset-catalogs"></a>資産カタログの色をサポートしています。

Apple は iOS 10 と macOS Sierra で色をサポートするために資産カタログを含めるし、アプリのバンドルで静的なイメージのコンテンツを分類するために使用を拡大しました。

使用して資産カタログがアプリに次の利点を提供します。

- 静的イメージ資産の最適な展開オプションを提供します。
- 色の自動修正をサポートしています。
- 自動のピクセル形式の最適化をサポートしています。
- アプリのスライスとは関連する get のコンテンツのみが、エンドユーザーのデバイスに配信されるようにするアプリが簡略化をサポートしています。

Apple が行われて、次の拡張機能資産カタログに色のサポート。

- 16 ビット (色のチャネル) ごとのソース コンテンツをサポートしています。
- 表示の色域によってカタログのコンテンツをサポートしています。 コンテンツは、sRGB または Display P3 色域のいずれかのタグ付けすることができます。

開発者には、それぞれのアプリでワイド カラー コンテンツをサポートするための 3 つのオプションがあります。

1. **何もしない**-としてこの要件以外でもコンテンツを残しておく必要がある色コンテンツは、アプリが (場所、ユーザーのエクスペリエンスを向上するは) 鮮明な色を表示する必要がある状況でのみ使用する必要があります、ためには。 すべてのハードウェアの状況で正しくレンダリングされる続けます。
2. **Display P3 に既存のコンテンツをアップグレード**-P3 形式で、アップグレードされた新しいファイルを使用して、資産カタログで既存のイメージのコンテンツを交換し、資産エディターでタグを開発者が必要です。 ビルド時に、これらの資産から sRGB 派生イメージが生成されます。
3. **資産のコンテンツの最適化を提供**-8 ビット、sRGB と (正しく資産エディターでタグ付け) 資産カタログ内の各イメージの 16 ビット Display P3 ビジョンの両方でこのような状況では、開発者は、します。

### <a name="asset-catalog-deployment"></a>資産カタログの展開

次のことを開発者は、ワイド カラー イメージのコンテンツを含む資産カタログを使用してアプリ ストアにアプリを送信するとき。

- エンドユーザーにアプリが展開されると、アプリのスライスことによりユーザーのデバイスに適切なコンテンツのバリアントのみが配信されるようにします。
- 色をサポートしていないデバイスではありませんの色のコンテンツを含むペイロードのコストは、デバイスを出荷ことはありません。
- `NSImage` macos Sierra (以降) がハードウェアの表示に最適なコンテンツの表現を自動的に選択します。
- 表示されているコンテンツは、デバイスのハードウェア特性の変更を表示する場合、または自動的に更新されます。

### <a name="asset-catalog-storage"></a>資産カタログ ストレージ

資産カタログ ストレージには、次のプロパティと、アプリに影響があります。

- ビルド時に、Apple は、高品質なイメージの変換を使用して、イメージ コンテンツの記憶域を最適化しようとします。
- ワイド カラー コンテンツのカラー チャネルあたり 16 ビットが使用されます。
- コンテンツの依存するイメージの圧縮を使用して、成果物のコンテンツ サイズを削減します。 コンテンツのサイズをさらに最適化するために、新しい「非可逆」の圧縮が追加されました。

## <a name="colors-in-user-interfaces"></a>ユーザー インターフェイス内の色

純色で画面上のピクセルのほとんどは、ユーザー インターフェイスの色を使用する場合です。 さらに、これらのピクセルのほとんどはイメージの資産から発生しますが、アプリによって直接 (または、アプリの代わりに OS によって) に描画されます。

作る色は、UI レベルで作業する場合、いくつかの課題を提示できます。

- **色との通信**- 設計者と開発者は通常、色について説明するとき、_と見なされます_sRGB 色空間が関係します。 色として伝達が`rgb(128, 45, 56)`または`#FF0456`します。 作る設計でこれらの表現は正確に指定した色を表すための十分な情報を指定しない、作業色空間に含める必要があることもできます。 使用して Apple の提案`P3(128, 45, 56)`と`P3#FF0456`代わりにします。 
- **色のピッキング**- ほとんどの一般的なイメージの編集と設計 sRGB 色空間と同じ制限から低下するソフトウェアが、組み込みのカラー ピッカーを使用する場合。 デザイナーでは、色のスタイルを使用する場合、カラー ピッカーで"Display P3"色空間ではことを確認してください。
- **色のコーディング**両方 - `NSColor` (macOS) と`UIColor`P3 の色を直接生成するための新しい便利なメソッドがある (iOS と tvOS) と Extended Range sRGB 色空間を同様の色をサポートするために拡張されています。
- **色を格納する**-アプリのドキュメントでカラーを作るのファイルを格納するときに、注意を実行する必要があります。 適切に機能して、sRGB 色空間のカラー チャネルあたり 8 ビット、さまざまな色のカラー チャネルあたり 16 ビットを使用してください。 開発者もを監視する必要があります (すべてのものが、sRGB だけでは従来) してから、想定した色空間のインスタンス。

## <a name="colors-on-the-web"></a>Web 上の色

Web ページで、さまざまな色の表示をサポートするデバイスで色を使用する場合、十分に注意してください。 場合は、web サイトに含まれているイメージのコンテンツのすべてが適切にタグ付けされ、iOS および macOS は自動的に色と一致し、正しく表示します。

メディア クエリ P3、sRGB 対応デバイス間での資産を解決するためにも利用します。

```xml
<picture>
    <source srcset="monkey-p3.jpg" media="(color-gamut: p3)">
    <source srcset="monkey-rpg.jpg">
</picture>
```

Apple では、WebKit 提案により、想定した sRGB 領域だけでなく他の色空間で指定する CSS でもあります。

## <a name="rendering-off-screen-images-in-app"></a>アプリの画面イメージのレンダリング

作成されているアプリの種類に基づいて、インターネットから収集したか (アプリをたとえば描画ベクトル) など、アプリ内から直接イメージ コンテンツを作成、イメージ コンテンツを追加するユーザーを許可する可能性がありますに。

どちらのこのような場合、アプリをレンダリングできます必要な画像で iOS と macOS の両方に追加された拡張機能を使用して色をワイド画面。

### <a name="drawing-wide-color-in-ios"></a>IOS での色の描画

IOS 10 でワイドの色の画像を正しく描画する方法を説明する前に共通の iOS 描画コードは次を参照してください。

```csharp
public UIImage DrawWideColorImage ()
{
    var size = new CGSize (250, 250);
    UIGraphics.BeginImageContext (size);

    ...
    
    UIGraphics.EndImageContext ();
    return image = UIGraphics.GetImageFromCurrentImageContext ();
}
```

対処する必要がある標準のコードに問題がある_する前に_ワイド カラー イメージを描画するために使用できます。 `UIGraphics.BeginImageContext (size)` IOS イメージの描画を開始するために使用するメソッドは、次の制限。

- カラー チャネルあたり 8 ビット以上のイメージのコンテキストを作成できません。
- これは、Extended Range sRGB 色空間内の色を表すことはできません。
- バック グラウンドで呼び出される低レベル C ルーチンのため、非 sRGB 色空間でコンテキストを作成するインターフェイスを提供する機能はありません。

これらの制限を処理し、iOS 10 でワイドの色の画像を描画するには、代わりに次のコードを使用します。

```csharp
public UIImage DrawWideColorImage ()
{
    var size = new CGSize (250, 250);
    var render = new UIGraphicsImageRenderer (size);

    var image = render.CreateImage ((UIGraphicsImageRendererContext context) => {
        var bounds = context.Format.Bounds;
        CGRect slice;
        CGRect remainder;
        bounds.Divide (bounds.Width / 2, CGRectEdge.MinXEdge, out slice, out remainder);

        var redP3 = UIColor.FromDisplayP3 (1.0F, 0.0F, 0.0F, 1.0F);
        redP3.SetColor ();
        context.FillRect (slice);

        var redRGB = UIColor.Red;
        redRGB.SetColor ();
        context.FillRect (remainder);
    });

    // Return results
    return image;
}
```

新しい`UIGraphicsImageRenderer`クラスは、Extended Range sRGB 色空間の処理に対応している新しいイメージ コンテキストを作成し、次の機能があります。

- 既定で管理されている色では完全にします。
- 既定では、Extended Range sRGB 色空間をサポートします。
- また、sRGB、Extended Range sRGB 色空間ので、アプリが実行されている iOS デバイスの機能に基づくでレンダリングするかどうかは適切に決定されます。
- 完全かつ自動的に管理イメージ コンテキスト (`CGContext`) 呼び出しについて心配する必要はありません、開発者のための有効期間が開始およびコンテキスト コマンドを終了します。
- 互換性があります、`UIGraphics.GetCurrentContext()`メソッド。

`CreateImage`のメソッド、`UIGraphicsImageRenderer`クラスがワイド カラー イメージを作成すると呼ばれ、に描画するイメージのコンテキストに完了ハンドラーに渡されます。 次のようにこの完了ハンドラー内で実行すべての描画は。

- `UIColor.FromDisplayP3`メソッドが作る Display P3 色空間に新しい色の彩度赤を作成し、四角形の前半の描画に使用されます。 
- 2 番目の完全に通常の sRGB で描画する四角形の半分には、比較の赤のカラーが飽和状態。

### <a name="drawing-wide-color-in-macos"></a>MacOS での色の描画

`NSImage` Macos Sierra ワイド カラー イメージの描画をサポートするクラスが拡張されています。 例えば:

```csharp
var size = CGSize(250,250);
var wideColorImage = new NSImage(size, false, (drawRect) =>{
    var rects = drawRect.Divide(drawRect.Size.Width/2,CGRect.MinXEdge);
    
    var color = new NSColor(1.0, 0.0, 0.0, 1.0).Set();
    var path = new NSBezierPath(rects.Slice).Fill();
    
    color = new NSColor(1.0, 0.0, 0.0, 1.0).Set();
    path = new NSBezierPath(rects.Remainder).Fill();
    
    // Return modified
    return true;
});
```

## <a name="rendering-on-screen-images-in-app"></a>アプリでイメージを画面に表示

ワイド カラー イメージを画面に表示される表示するために、プロセスは、macOS、および iOS 上で示したワイド画面のカラー イメージの描画のような動作します。

### <a name="rendering-on-screen-in-ios"></a>IOS の画面に表示

アプリで iOS の画面に表示される色のイメージをレンダリングする場合、オーバーライド、`Draw`のメソッド、`UIView`通常どおり、問題の。 例えば:

```csharp
using System;
using UIKit;
using CoreGraphics;

namespace MonkeyTalk
{
    public class MonkeyView : UIView
    {
        public MonkeyView ()
        {
        }

        public override void Draw (CGRect rect)
        {
            base.Draw (rect);

            // Draw the image to screen
            ...
        }
    }
}
``` 

IOS 10 と同様、 `UIGraphicsImageRenderer` sRGB、Extended Range sRGB 色空間のときに、アプリが実行されている iOS デバイスの機能に基づくでレンダリングするかどうかに適切に決定されます、上記のようにクラス、`Draw`メソッドが呼び出されます。 さらに、`UIImageView`されて管理される iOS の 9.3 以降の色。

アプリがでレンダリングが行われる方法を把握する必要があるかどうか、`UIView`または`UIViewController`、新しいチェック`DisplayGamut`のプロパティ、`UITraitCollection`クラス。 この値になります、`UIDisplayGamut`次の列挙型。

- P3
- Srgb
- 指定されていません。

アプリは、イメージを描画するために使用される色空間コントロールする場合、使用できる新しい`ContentsFormat`のプロパティ、`CALayer`目的の色空間を指定します。 この値は、`CAContentsFormat`次の列挙型。

- Gray8Uint
- Rgba16Float
- Rgba8Uint

### <a name="rendering-on-screen-in-macos"></a>MacOS の画面に表示

アプリは、macOS の画面に表示される色でイメージをレンダリングする必要がある、オーバーライド、`DrawRect`のメソッド、`NSView`通常どおり問題。 例えば:

```csharp
using System;
using AppKit;
using CoreGraphics;
using CoreAnimation;

namespace MonkeyTalkMac
{
    public class MonkeyView : NSView
    {
        public MonkeyView ()
        {
        }

        public override void DrawRect (CGRect dirtyRect)
        {
            base.DrawRect (dirtyRect);

            // Draw graphics into the view
            ...
        }
    }
}
```

ここでも、それが適切に決定 sRGB、Extended Range sRGB 色空間の場合で、アプリが実行されている Mac のハードウェアの機能に基づくでレンダリングするかどうか、`DrawRect`メソッドが呼び出されます。

アプリは、イメージを描画するために使用される色空間コントロールする場合、使用できる新しい`DepthLimit`のプロパティ、`NSWindow`クラスが目的の色空間を指定します。 この値は、`NSWindowDepth`次の列挙型。

- OneHundredTwentyEightBitRgb
- SixtyfourBitRgb
- TwentyfourBitRgb

## <a name="summary"></a>まとめ

この記事では、色、および方法が実装、Xamarin.iOS または Xamarin.Mac アプリの内部で使用することについて説明しました。



## <a name="related-links"></a>関連リンク

- [iOS 10 のサンプル](https://developer.xamarin.com/samples/ios/iOS10/)
