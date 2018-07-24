---
title: さまざまな画面のリソースの作成
ms.prod: xamarin
ms.assetid: 3D17DE45-115C-7192-5685-44F8EEE07DCC
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: a98d65c6c04ae2400572a2405d487035ac466678
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
ms.locfileid: "30770964"
---
# <a name="creating-resources-for-varying-screens"></a>さまざまな画面のリソースの作成

Android 自体で実行されます多数の異なるデバイスでは、それぞれさまざまな解像度、画面サイズと画面ごとに密度が。 Android は実行のスケーリングと、アプリケーションがこれらのデバイスで作業するサイズを変更しますが、最適のユーザー エクスペリエンスの可能性があります。 たとえば、画像がぼやけて表示される可能性があります、イメージが発生するレイアウト内の UI 要素の位置が重複または離れすぎているが多すぎる (十分ではありません) 画面の領域を占有可能性があります。


## <a name="concepts"></a>概念

いくつかの用語と概念は、複数の画面をサポートするために重要です。

- **画面のサイズ**&ndash;アプリケーションを表示するための物理領域の量

- **画面の密度**&ndash;画面上の任意の特定領域のピクセル数です。 一般的な測定単位は、ドット/インチ (dpi) です。

- **解像度**&ndash;画面上のピクセルの合計数。 アプリケーションを開発するときの解像度は画面のサイズと密度と同じくらい重要がありません。

- **密度に依存しないピクセル (dp)** &ndash;これは、使用できるように設計するレイアウト密度の独立した仮想測定単位。 配布ポイントを画面のピクセルに変換するには、次の式を使用します。

    px &equals; dp &times; dpi &divide; 160

- **印刷の向き**&ndash;ランドス ケープが高さよりも長い場合に、画面の向きと見なされます。 これに対し、縦向きは、画面が縦長の幅です。 印刷の向きは、ユーザーがデバイスを回転、アプリケーションの有効期間中に変更できます。

これらの概念の最初の 3 つが相互に関連することに注意してください。&ndash;密度を増やすと、画面サイズが増加せずに、解像度を高くします。 ただし、密度と解像度の両方が増加している場合、画面のサイズを変更されません。 画面サイズ、密度と解像度の間のリレーションシップでは、画面のサポートは非常に迅速に複雑になります。

このような複雑さに対処するために Android framework 優先的に使用して*密度非依存ピクセル (dp)* 画面レイアウトの場合。 非依存のピクセルの密度を使用すると、UI 要素は、異なる密度の画面で、同じ物理サイズをユーザーに表示されます。


## <a name="supporting-various-screen-sizes-and-densities"></a>さまざまな画面サイズと密度のサポート

Android では、ほとんどの各画面構成用に正しくレイアウトを表示するために作業を処理します。 ただし、これには、システムの支援に実行できる一部の操作があります。

レイアウトで実際のピクセル密度に依存しないピクセルの使用は、密度の独立性を確認するほとんどの場合にも十分です。
Android には、実行時に、適切なサイズ ドロウアブルは拡大縮小します。
ただし、している可能性このスケーリングはビットマップがぼやけて見えるを勧めします。 これを回避するのには、代替のリソースを別の密度を指定する必要があります。 複数の解像度および画面の密度が簡単に証明するためにデバイスを設計するとき最初に、高解像度または密度のイメージおよびスケール ダウンし。 こうと、ぼかしやゆがみのサイズを変更する可能性があります。


### <a name="declare-the-screen-size-the-application-supports"></a>画面のサイズをサポートするアプリケーションを宣言します。

画面のサイズを宣言することにより、サポートされているデバイスのみがアプリケーションをダウンロードできます。 これを実現するには、[サポート画面](http://developer.android.com/guide/topics/manifest/supports-screens-element.html)内の要素、 **AndroidManifest.xml**ファイル。 この要素を使用して、アプリケーションでサポートされるどのような画面サイズを指定します。 特定の画面は、アプリケーションが正常にできる場合にサポートすると見なされますのレイアウトに表示します。 このマニフェストの要素を使用すると、アプリケーションは表示されませんで[ *Google Play* ](https://play.google.com/)を画面の仕様を満たしていないデバイス用です。 ただし、アプリケーションが、サポートされていない画面でのデバイスで実行されますが、レイアウトがぼやけて表示される可能性がありますとピクセルです。

これを行うには、Xamarin.Android に最初に追加する必要が、 **AndroidManifest.xml**ファイルをプロジェクトが既に存在しない場合。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Android マニフェスト](resources-for-varying-screens-images/01-android-manifest-vs-sml.png)](resources-for-varying-screens-images/01-android-manifest-vs.png#lightbox)

**AndroidManifest.xml**に追加、**プロパティ**ディレクトリ。 含めるファイルを編集し、[サポート画面](http://developer.android.com/guide/topics/manifest/supports-screens-element.html):

[![サポートする画面を追加します。](resources-for-varying-screens-images/02-adding-supports-screens-vs-sml.png)](resources-for-varying-screens-images/02-adding-supports-screens-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Android マニフェスト](resources-for-varying-screens-images/01-android-manifest-xs-sml.png)](resources-for-varying-screens-images/01-android-manifest-xs.png#lightbox)

**AndroidManifest.xml**に追加、**プロパティ**ディレクトリ。 含めるファイルを編集し、[サポート画面](http://developer.android.com/guide/topics/manifest/supports-screens-element.html):

[![サポートする画面を追加します。](resources-for-varying-screens-images/02-adding-supports-screens-xs-sml.png)](resources-for-varying-screens-images/02-adding-supports-screens-xs.png#lightbox)

-----

### <a name="provide-alternate-layouts-for-different-screen-sizes"></a>代替レイアウトは、さまざまな画面サイズ

Android では、画面のサイズの設定に基づいてサイズを変更するは、この可能性がありますされませんを十分にあります。 より大きな画面の一部の UI 要素のサイズを大きくまたは小さく画面の UI 要素の位置を変更することが望ましい場合があります。

API レベル 13 (Android 3.2) から始めて、画面のサイズは、非推奨、ソフトウェアの使用を優先し*N*dp 修飾子です。 この新しい修飾子は、指定したレイアウト領域の容量が必要なを宣言します。 ある Android 3.2 以降を実行になっているアプリケーションを使用してくださいような新しい修飾子を強くお勧めします。

たとえば、レイアウトでは、画面の幅の最小 700dp 必要に応じて場合、代替のレイアウトがフォルダーに移動は**レイアウト sw700dp**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![700dp 画面の幅のレイアウト フォルダー](resources-for-varying-screens-images/03-layout-sw700dp-vs.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![700dp 画面の幅のレイアウト フォルダー](resources-for-varying-screens-images/03-layout-sw700dp-xs.png)

-----


指針としては、さまざまなデバイス用のいくつかの数字を次に示します。

- **一般的な電話** &ndash; 320dp: 一般的な電話

- **5"タブレット/"tweener"デバイス** &ndash; 480dp: Samsung メモなど

- **7"タブレット** &ndash; 600dp: Barnes など&amp;かなった場所

- **10"タブレット** &ndash; 720dp: Motorola Xoom など

アプリケーションのターゲット API レベル (Android 3.1) を最大 12 のこと、レイアウトべき修飾子を使用しているディレクトリで**小さな**/**通常**/**大きな** / **xlarge**としてほとんどのデバイスで使用できるさまざまな画面サイズの汎化します。 たとえば、次の図でリソースがある代替の 4 つの異なる画面サイズ。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![代替のリソースの 4 つの画面サイズ](resources-for-varying-screens-images/04-layout-large-vs.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![代替のリソースの 4 つの画面サイズ](resources-for-varying-screens-images/04-layout-large-xs.png)

-----

古い pre API レベル 13 画面サイズ修飾子には、密度依存しないピクセル単位の比較方法の比較を次に示します。

- 426dp x 320dp は**小さな**

- 470dp x 320dp は**通常**

- 640dp x 480dp は**大きな**

- 960dp x 720dp は**xlarge**

新しい画面 API レベル 13 の修飾子のサイズを変更して API レベル 12 と下限の古い画面修飾子よりも優先順位の高いします。、
古いと、新たな API レベルの範囲をアプリケーションでは、次のスクリーン ショットに示すように、修飾子の両方のセットを使用して代替のリソースを作成する必要があります。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![両方の修飾子を使用して代替のリソース](resources-for-varying-screens-images/05-both-qualifiers-vs.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![両方の修飾子を使用して代替のリソース](resources-for-varying-screens-images/05-both-qualifiers-xs.png)

-----



### <a name="provide-different-bitmaps-for-different-screen-densities"></a>さまざまな画面密度の異なるビットマップを指定します。

Android デバイスの必要に応じて、ビットマップのスケーラビリティが自身のビットマップ可能性がありますいない効率よくスケール アップまたはスケール ダウン: がぼやけていたり、またはあいまいになる可能性があります。 画面の密度の適切なビットマップを提供することで、この問題を軽減します。

たとえば、次の図には発生する可能性がある問題をレイアウトと外観の例と密度を指定のリソースが指定されていません。

![密度リソースなしのスクリーン ショット](resources-for-varying-screens-images/06-density-not-provided.png)

密度の特定のリソースでデザインしたレイアウトにこれを比較します。

![密度固有のリソースとスクリーン ショット](resources-for-varying-screens-images/07-density-specific-resources.png)


### <a name="create-varying-density-resources-with-android-asset-studio"></a>資産の Android Studio のさまざまな密度リソースを作成します。

さまざまな密度のこれらのビットマップの作成をビット面倒になることができます。 Google が減らすことができますと呼ばれるこれらのビットマップの作成に関連する面倒なオンラインのユーティリティを作成するような場合、 [ **Android 資産 Studio**](https://romannurik.github.io/AndroidAssetStudio/)です。

[![Android Asset Studio](resources-for-varying-screens-images/08-android-asset-studio-sml.png)](resources-for-varying-screens-images/08-android-asset-studio.png#lightbox)

この web サイトを 1 つのイメージを提供することで 4 つの一般的な画面の密度を対象とするビットマップの作成に役立ちます。 資産の android Studio はいくつかのカスタマイズを含む、ビットマップを作成し、zip ファイルとしてダウンロードすることを許可します。


## <a name="tips-for-multiple-screens"></a>複数の画面のヒント

まごつくかもしれません多くのデバイスで android を実行し、画面サイズと画面ごとに密度の組み合わせが手に負えないものに思えるかもしれません。 次のヒントには、さまざまなデバイスをサポートするために必要な作業量を最小限に抑えるのに役立ちます。

- **唯一の設計および開発する必要の**&ndash;がありますが、多くのさまざまなデバイスがありを設計およびの開発作業の多くがかかる場合がありますまれなフォーム ファクターでいくつか存在します。 [**画面サイズと密度**](http://developer.android.com/resources/dashboard/screens.html)ダッシュ ボードは、データの画面サイズと画面密度マトリックスの内訳を提供する Google から提供されるページです。 この内訳は、画面のサポートに開発する方法に関する洞察を提供します。

- **代わりに使用する Dp ピクセル**-ピクセルが画面密度の変化に応じて厄介なものになります。 ピクセル値をハードコーディングしないでください。 配布ポイント (密度に依存しないピクセル単位) を優先するためのピクセルを回避します。

- **回避** [AbsoluteLayout](https://developer.xamarin.com/api/type/Android.Widget.AbsoluteLayout/)
  **可能な任意の場所** &ndash; API レベル 3 (Android 1.5) では使用されなくなりましたし、不安定にレイアウトになります。 これは使用されません必要があります。 より柔軟なレイアウト ウィジェットを使用しようとして代わりに、 [ **LinearLayout**](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)、 [ **[相対レイアウト]**](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/)、または新しい[**GridLayout**](https://developer.xamarin.com/api/type/Android.Widget.GridLayout/)です。

- **既定値として 1 つのレイアウトの方向を選択**&ndash;代替のリソースを提供するのではなく、たとえば、**レイアウト土地**と**レイアウト ポート**、用のリソースの配置横**レイアウト**とに縦のリソース**レイアウト ポート**です。

- **高さと幅を LayoutParams を使用して**XML レイアウト ファイルの UI 要素を定義するときに Android アプリケーションを使用して、 **wrap_content**と**fill_parent**値によって複数の成功を行うピクセルまたは密度の独立した単位を使用するよりもさまざまなデバイスでは、適切な外観を確認してください。 これらのディメンション値では、適切なビットマップ リソースの規模を Android が発生します。 この同じ理由から、密度に依存しない単位は最適な予約されている場合の余白を指定して、UI 要素のパディングです。


## <a name="testing-multiple-screens"></a>複数の画面のテスト

Android アプリケーションは、サポートされるすべての構成に対してテストする必要があります。 理想的です実際のデバイス自体でデバイスをテストする必要がありますが、多くの場合にこれが不可能または実用的です。
ここでデバイスの構成ごとに、エミュレーターおよび Android 仮想デバイスのセットアップの使用が役に立ちます。

Android SDK は、いくつかのエミュレーターに create AVD のスキンを使用する可能性がありますが、サイズ、密度と多くのデバイスの解像度をレプリケートするを提供します。
同様に、ハードウェア ベンダーの多くは、自分のデバイスのスキンを提供します。

サービスをテスト サード パーティのサービスを使用することもできます。
これらのサービスは、APK がかかる、多数の異なるデバイスで実行し、アプリケーションの動作のフィードバックを提供します。
