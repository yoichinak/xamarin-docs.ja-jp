---
title: さまざまな画面のリソース作成
ms.prod: xamarin
ms.assetid: 3D17DE45-115C-7192-5685-44F8EEE07DCC
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/28/2018
ms.openlocfilehash: 63cbe556783ffe22512ff5312817d522120bd15e
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61013653"
---
# <a name="creating-resources-for-varying-screens"></a>さまざまな画面のリソースを作成します。

Android 自体で実行さまざまなデバイスでさまざまな解像度や画面サイズ、画面密度がそれぞれします。 Android は、スケーリングとサイズを変更すると、これらのデバイスでアプリケーションを実行しますが、最適なユーザー エクスペリエンスがあります。 たとえば、イメージがぼやけて表示されることや、ビューで期待どおりに配置することがあります。


## <a name="concepts"></a>概念

いくつかの用語と概念は、複数の画面をサポートするために重要です。

- **画面サイズ**&ndash;アプリケーションを表示するための物理領域の量

- **画面の密度**&ndash;画面上の任意の特定領域のピクセル数。 一般的な測定単位は、ドット/インチ (dpi です)。

- **解像度**&ndash;画面上のピクセルの合計数。 アプリケーションを開発する場合、解像度は画面のサイズおよび密度として重要ではないです。

- **密度に依存しないピクセル (dp)** &ndash;密度に依存しない設計するレイアウトを許可する仮想測定単位。 次の数式を使用して、ピクセルの配布ポイントに変換します。

    px &equals; dp &times; dpi &divide; 160

- **印刷の向き**&ndash;画面の向きが高さより幅がある場合は、ランドス ケープをすると見なされます。 これに対し、画面が縦長の筐場合に、縦向きです。 ユーザーがデバイスを回転すると、アプリケーションの有効期間中に、印刷の向きを変更できます。

これらの概念の最初の 3 つが相互に関連することに注意してください。&ndash;密度の向上と、画面サイズが増加せずに、解像度を高くします。 ただし、密度と解像度の向上、画面サイズはままです。 この画面のサイズ、密度、および解決の関係では、画面のサポートはすぐに複雑になります。

この複雑さを処理するには、Android のフレームワークを使用して希望*密度に依存しないピクセル (dp)* 画面のレイアウト。 密度に依存しないピクセルを使用すると、UI 要素は、異なる密度の画面で同じ物理サイズである場合にユーザーに表示されます。


## <a name="supporting-various-screen-sizes-and-densities"></a>さまざまな画面サイズおよび密度のサポート

Android では、各画面構成用に正しくレイアウトを表示するために作業のほとんどを処理します。 ただし、システムの支援に実行できる一部のアクションがあります。

レイアウトで実際のピクセル密度に依存しないピクセルの使用は、密度に依存しないことを確認するほとんどの場合は十分です。
Android では、適切なサイズに実行時に、ドローアブルを拡大縮小されます。
ただし、スケーリングはビットマップ不鮮明にすることが可能です。 この問題を回避するには、異なる密度の代替のリソースを指定します。 複数の解像度や画面解像度のデバイスを設計、それを証明する簡単で開始する以上の解像度または密度イメージし、は、スケール ダウン。


### <a name="declare-the-supported-screen-size"></a>サポートされている画面のサイズを宣言します。

画面のサイズを宣言することにより、サポートされているデバイスのみが、アプリケーションをダウンロードできます。 これは、設定によって実現されます、[サポート画面](https://developer.android.com/guide/topics/manifest/supports-screens-element.html)内の要素、 **AndroidManifest.xml**ファイル。 この要素は、アプリケーションでどのような画面サイズがサポートされている指定に使用されます。 特定の画面は、アプリケーションが画面には、そのレイアウトを配置できます正しくサポートと見なされます。 このマニフェスト要素を使用すると、アプリケーションは表示されませんで[ *Google Play* ](https://play.google.com/)画面仕様を満たしていないデバイス。 ただし、アプリケーションが、サポートされていない画面を備えたデバイスで実行されますが、レイアウトがぼやけてとピクセルです。

サポートされている画面 sixes が宣言されている、 **Properites/AndroidManifest.xml**ソリューションのファイル。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Android マニフェスト](resources-for-varying-screens-images/01-android-manifest-sml.w1581.png)](resources-for-varying-screens-images/01-android-manifest.w1581.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![Android マニフェスト](resources-for-varying-screens-images/01-android-manifest-sml.m761.png)](resources-for-varying-screens-images/01-android-manifest.m761.png#lightbox)

-----

編集**AndroidManifest.xml**に含める[サポート画面](https://developer.android.com/guide/topics/manifest/supports-screens-element.html):

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
          android:versionCode="1"
          android:versionName="1.0"
          package="HelloWorld.HelloWorld">
      <uses-sdk android:minSdkVersion="21" android:targetSdkVersion="27" />
      <supports-screens android:resizable="true"
                        android:smallScreens="true"
                        android:normalScreens="true"
                        android:largeScreens="true" />
      <application android:allowBackup="true"
                   android:icon="@mipmap/ic_launcher"
                   android:label="@string/app_name"
                   android:roundIcon="@mipmap/ic_launcher_round"
                   android:supportsRtl="true" android:theme="@style/AppTheme">
  </application>
</manifest>
```

### <a name="provide-alternate-layouts-for-different-screen-sizes"></a>代替レイアウトは、さまざまな画面サイズ


代替レイアウトを使用すれば、特定の画面サイズ、位置を変更またはコンポーネントの UI 要素のサイズのビューをカスタマイズできます。

以降では、API レベル 13 (Android 3.2) は、画面サイズは非推奨のため、ソフトウェアを使用して*N*dp 修飾子。 指定したレイアウト領域の容量が必要な新しい修飾子を宣言します。 これらの新しい修飾子が Android 3.2 以降を実行するためのものがアプリケーションに使用することをお勧めします。

たとえば、必要な最小 700 のレイアウトを画面の幅の配布ポイントは、代替レイアウトは、フォルダーの移動は**レイアウト sw700dp**:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![700 dp 画面幅レイアウト フォルダー](resources-for-varying-screens-images/03-layout-sw700dp-vs.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![700 dp 画面幅レイアウト フォルダー](resources-for-varying-screens-images/03-layout-sw700dp-xs.png)

-----


なガイドラインとして、さまざまなデバイス用のいくつかの数字以下に示します。

- **一般的な電話** &ndash; 320 dp: 一般的な携帯電話

- **タブレット 5"/"tweener"デバイス** &ndash; 480 dp: Samsung 注など

- **7"tablet** &ndash; 600 dp: など、Barnes &amp; Noble 探る

- **10 インチのタブレット** &ndash; 720 dp: Motorola Xoom など

アプリケーションのターゲット API レベル 12 (Android 3.1) までのこと、レイアウトに入れる修飾子を使用して、ディレクトリ**小さな**/**通常**/**大きな** /**特大**としてほとんどのデバイスで利用できるさまざまな画面サイズの汎化します。 たとえば、次の図では、代替のリソースの 4 つの異なる画面サイズがあります。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![代替のリソースの 4 つの画面サイズ](resources-for-varying-screens-images/04-layout-large-vs.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![代替のリソースの 4 つの画面サイズ](resources-for-varying-screens-images/04-layout-large-xs.png)

-----

古い pre API レベル 13 画面サイズ修飾子と密度に依存しないピクセルとの比較の比較を次に示します。

- 426 dp dp は x 320**小さな**

- 470 dp dp は x 320**通常**

- 640 dp dp は x 480**大きな**

- 960 dp dp は x 720**特大**

新しい画面は、API レベル 13 で修飾子のサイズや、API レベル 12 と下限の古い画面修飾子よりも高い優先順位があるをします。
アプリケーションにまたがり、古いと、新しい API レベルでは、次のスクリーン ショットに示すように、修飾子の両方のセットを使用して別のリソースを作成するために必要な場合があります。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![代替のリソースを両方の修飾子](resources-for-varying-screens-images/05-both-qualifiers-vs.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![代替のリソースを両方の修飾子](resources-for-varying-screens-images/05-both-qualifiers-xs.png)

-----



### <a name="provide-different-bitmaps-for-different-screen-densities"></a>さまざまな画面密度の異なるビットマップを指定します。

Android デバイスの必要に応じて、ビットマップのスケーラビリティがビットマップ自体可能性がありますいない洗練された方法スケール アップまたはスケール ダウン: ぼやけて、またはあいまいになる可能性があります。 画面密度の適切なビットマップを提供することで、この問題を軽減します。

次の図に発生するレイアウトと外観の問題の例は、たとえば、密度の指定とリソースが指定されていません。

![密度リソースなしのスクリーン ショット](resources-for-varying-screens-images/06-density-not-provided.png)

密度に固有のリソースでデザインしたレイアウトと比較します。

![密度に固有のリソースのスクリーン ショット](resources-for-varying-screens-images/07-density-specific-resources.png)


### <a name="create-varying-density-resources-with-android-asset-studio"></a>Android Asset Studio のさまざまな密度のリソースを作成します。

さまざまな密度のこれらのビットマップの作成を少し面倒なことができます。 そのため、Google と呼ばれるこれらのビットマップの作成に伴う面倒な作業の一部を削減できるオンラインのユーティリティが作成、 [ **Android Asset Studio**](https://romannurik.github.io/AndroidAssetStudio/)します。

[![Android Asset Studio](resources-for-varying-screens-images/08-android-asset-studio-sml.png)](resources-for-varying-screens-images/08-android-asset-studio.png#lightbox)

この web サイトを 1 つのイメージを提供することで、次の 4 つの一般的な画面密度を対象とするビットマップの作成に役立ちます。 Android Asset Studio は一部のカスタマイズで、ビットマップを作成し、zip ファイルとしてダウンロードすることを許可します。


## <a name="tips-for-multiple-screens"></a>複数の画面のヒント

デバイスでは、さまざまなまごつくかもしれませんに android を実行し、画面サイズや画面解像度の組み合わせが多すぎると感じることができます。 さまざまなデバイスをサポートするために必要な労力を最小限に抑えるには、次のヒントが役立ちます。

- **のみの設計および必要な開発**&ndash;があり、多くのさまざまなデバイスがあるが、まれなフォーム ファクターの設計と開発のかなりの労力がかかる場合がありますをいくつか存在します。 [**画面サイズおよび密度**](https://developer.android.com/resources/dashboard/screens.html)ダッシュ ボードは、データ、画面サイズ/画面密度マトリックスの内訳を提供する Google から提供されるページ。 この内訳は、画面のサポートで開発作業をする方法の洞察を提供します。

- **ピクセル単位ではなく、DPs 使用**-ピクセルが画面密度の変化に応じて厄介なものになります。 ピクセル値をハードコーディングしないでください。 配布ポイント (密度に依存しないピクセル単位) を優先してピクセルを避けてください。

- **回避** [AbsoluteLayout](https://developer.xamarin.com/api/type/Android.Widget.AbsoluteLayout/)
  **可能な任意の場所** &ndash; API レベル 3 (Android 1.5) は非推奨し、レイアウトが不安定になります。 これを使用しない必要があります。 代わりより柔軟なレイアウト ウィジェットを使用しよう[ **LinearLayout**](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)、 [ **[相対レイアウト]**](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/)、または新しい[**GridLayout**](https://developer.xamarin.com/api/type/Android.Widget.GridLayout/)します。

- **既定値として 1 つのレイアウトの方向を選択**&ndash;代替のリソースを提供するのではなく、たとえば、**レイアウト land**と**レイアウト ポート**、用のリソース配置横向きで**レイアウト**、リソースと、縦に**レイアウト ポート**します。

- **LayoutParams を使用して、高さと幅を**- XML レイアウト ファイルでは、UI 要素を定義するときに、Android アプリケーションを使用して、 **wrap_content**と**fill_parent**さらなる成功の値が必要がありますさまざまなデバイス ピクセルまたは密度に依存しない単位を使用してより適切な外観を確認してください。 これらのディメンション値は、Android を適切なビットマップ リソースのスケールにあります。 場合に最適な予約は密度に依存しない単位この同じ理由で、余白を指定して、UI 要素の埋め込み。


## <a name="testing-multiple-screens"></a>複数の画面のテスト

Android アプリケーションは、サポートされるすべての構成に対してテストする必要があります。 理想的です実際のデバイス自体でデバイスをテストする必要がありますが、多くの場合にこれが不可能または実用的です。
ここでは、各デバイスの構成の Android 仮想デバイスのセットアップ、エミュレーターの使用が便利になります。

Android SDK では、いくつかのエミュレーター スキンは、Avd を作成するために使用可能性がありますが、サイズ、密度、および多くのデバイスの解像度はレプリケートを提供します。
同様に、ハードウェア ベンダーの多くは、自分のデバイスのスキンを提供します。

別のオプションでは、サービスをテストするサード パーティ製のサービスを使用します。
これらのサービスは APK を実行でさまざまなデバイスで実行し、アプリケーションがどのように動作するフィードバックを提供します。
