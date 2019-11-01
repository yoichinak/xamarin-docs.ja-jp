---
title: さまざまな画面のリソース作成
ms.prod: xamarin
ms.assetid: 3D17DE45-115C-7192-5685-44F8EEE07DCC
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/28/2018
ms.openlocfilehash: cbd392dcae173eb3baf0fb8f0c3c4ec7c0da23a1
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73025116"
---
# <a name="creating-resources-for-varying-screens"></a>さまざまな画面用のリソースの作成

Android 自体はさまざまなデバイスで実行され、それぞれの解像度、画面サイズ、画面密度がさまざまです。 Android では、スケールとサイズ変更を行って、アプリケーションがこれらのデバイスで動作するようにしますが、これにより、ユーザーエクスペリエンスが最適化される可能性があります。 たとえば、画像がぼやけて表示されたり、表示に期待どおりに配置されたりする可能性があります。

## <a name="concepts"></a>概念

複数の画面をサポートするために、いくつかの用語と概念を理解しておくことが重要です。

- **画面サイズ**&ndash; アプリケーションを表示するための物理領域の量

- 画面の**密度**は、画面上の特定の領域のピクセル数 &ndash; ます。 一般的な測定単位は、ドット/インチ (dpi) です。

- **解像度**&ndash; 画面上の合計ピクセル数です。 アプリケーションを開発する場合、解像度は画面のサイズや密度ほど重要ではありません。

- **密度に依存しないピクセル (dp)** は、密度に依存しないレイアウトを作成できるようにする測定単位 &ndash; します。 この数式は、dp を画面ピクセルに変換するために使用されます。

    px &equals; dp &times; dpi &divide; 160

- 画面の向き &ndash; 高さよりも幅が広い場合は横**方向**と見なされます。 これに対して、縦向きは、画面の高さが幅よりも大きい場合にあります。 ユーザーがデバイスを回転させるときに、アプリケーションの有効期間中に向きが変化することがあります。

これらの概念のうち、最初の3つは相互に &ndash; 関連しています。密度を上げることなく解像度を上げると、画面のサイズが大きくなります。 ただし、密度と解像度の両方が増加しても、画面のサイズは変更されないままになります。 画面のサイズ、密度、解像度の関係により、画面のサポートが迅速になります。

この複雑さに対処するために、Android framework では画面レイアウトに*密度に依存しないピクセル (dp)* を使用することを推奨しています。 密度に依存しないピクセルを使用すると、ユーザーには、異なる密度を持つ画面で同じ物理サイズの UI 要素が表示されます。

## <a name="supporting-various-screen-sizes-and-densities"></a>さまざまな画面サイズと密度のサポート

Android では、ほとんどの作業を処理して、画面構成ごとにレイアウトを正しくレンダリングします。 ただし、システムの提供を支援するために実行できる操作がいくつかあります。

密度に依存しないピクセルの使用は、ほとんどの場合、レイアウトで実際のピクセルの代わりに使用するだけで十分です。
Android では、実行時に、適切なサイズになるように、実行可能な drawables スケーリングします。
ただし、拡大縮小によってビットマップがぼやけて表示される可能性があります。 この問題を回避するには、異なる密度の代替リソースを指定します。 複数の解像度と画面密度用にデバイスを設計する場合、解像度や密度の高いイメージで開始し、スケールダウンする方が簡単です。

### <a name="declare-the-supported-screen-size"></a>サポートされている画面サイズを宣言する

画面サイズを宣言すると、サポートされているデバイスだけがアプリケーションをダウンロードできるようになります。 これを行うには、 **Androidmanifest .xml**ファイルで "[サポート-画面](https://developer.android.com/guide/topics/manifest/supports-screens-element.html)" 要素を設定します。 この要素は、アプリケーションでサポートされる画面サイズを指定するために使用されます。 指定された画面は、アプリケーションが画面に合わせてレイアウトを適切に配置できる場合、サポートされていると見なされます。 このマニフェスト要素を使用すると、画面の仕様を満たしていないデバイスの[*Google Play*](https://play.google.com/)にアプリケーションが表示されません。 ただし、アプリケーションは、サポートされていない画面を持つデバイスでも実行されますが、レイアウトがぼやけて見えないように見える場合があります。

サポートされているスクリーン sixes は、ソリューションの**caching/AndroidManifest .xml**ファイルで宣言されています。

<!-- markdownlint-disable MD001 -->

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Android マニフェスト](resources-for-varying-screens-images/01-android-manifest-sml.w1581.png)](resources-for-varying-screens-images/01-android-manifest.w1581.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![Android マニフェスト](resources-for-varying-screens-images/01-android-manifest-sml.m761.png)](resources-for-varying-screens-images/01-android-manifest.m761.png#lightbox)

-----

**Androidmanifest .xml**を編集して[、サポート画面](https://developer.android.com/guide/topics/manifest/supports-screens-element.html)を含めます。

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

### <a name="provide-alternate-layouts-for-different-screen-sizes"></a>さまざまな画面サイズに別のレイアウトを指定する

代替レイアウトを使用すると、特定の画面サイズのビューをカスタマイズし、コンポーネントの UI 要素の位置やサイズを変更することができます。

API レベル 13 (Android 3.2) 以降では、sw*N*dp 修飾子の使用を優先するため、画面サイズは非推奨となりました。 この新しい修飾子は、特定のレイアウトに必要な領域のサイズを宣言します。 Android 3.2 以降で実行するように設計されたアプリケーションでは、これらの新しい修飾子を使用することをお勧めします。

たとえば、レイアウトで画面幅の最低 700 dp が必要な場合、代替レイアウトは**sw700dp**フォルダーに配置されます。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![700 dp 画面の幅のレイアウトフォルダー](resources-for-varying-screens-images/03-layout-sw700dp-vs.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![700 dp 画面の幅のレイアウトフォルダー](resources-for-varying-screens-images/03-layout-sw700dp-xs.png)

-----

ガイドラインとして、さまざまなデバイスのいくつかの数値を次に示します。

- **一般的な電話**&ndash; 320 dp: 一般的な電話

- **5 つの "タブレット/" tweener "デバイス**&ndash; 480 Dp: Samsung Note など

- **7 "タブレット**&ndash; 600 Dp: Barnes &amp; 立派 nook

- **10 個の "タブレット**&ndash; 720 Dp: Motorola xoom など

API レベルを最大 12 (Android 3.1) にするアプリケーションでは、さまざまな画面の汎化として、**小さい**/**通常**/**大**/**特大**という修飾子を使用するディレクトリにレイアウトを設定する必要があります。ほとんどのデバイスで使用できるサイズ。 たとえば、次の図には、4つの異なる画面サイズの代替リソースがあります。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![4つの画面サイズの代替リソース](resources-for-varying-screens-images/04-layout-large-vs.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![4つの画面サイズの代替リソース](resources-for-varying-screens-images/04-layout-large-xs.png)

-----

次に示すのは、古い API レベル13より前の画面サイズ修飾子が密度に依存しないピクセルとどのように比較されるかを比較したものです。

- 426 dp x 320 dp が**小さい**

- 470 dp x 320 dp が**正常**である

- 640 dp x 480 dp が**大きく**なっています

- 960 dp x 720 dp が**特大**

API レベル13以上の新しい画面サイズ修飾子は、API レベル12以降の古い画面修飾子よりも優先順位が高くなります。
古い API レベルと新しい API レベルにまたがるアプリケーションでは、次のスクリーンショットに示すように、両方の修飾子を使用して代替リソースを作成する必要がある場合があります。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![両方の修飾子を持つ代替リソース](resources-for-varying-screens-images/05-both-qualifiers-vs.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![両方の修飾子を持つ代替リソース](resources-for-varying-screens-images/05-both-qualifiers-xs.png)

-----

### <a name="provide-different-bitmaps-for-different-screen-densities"></a>画面の密度に応じて異なるビットマップを指定する

Android では、必要に応じてデバイスのビットマップが拡張されますが、ビットマップ自体は洗練されていたり、ぼやけたりする場合があります。 画面の密度に適したビットマップを提供することで、この問題を軽減できます。

たとえば、次の図は、密度指定のリソースが提供されていない場合に発生する可能性があるレイアウトと外観の問題の例を示しています。

![密度リソースのないスクリーンショット](resources-for-varying-screens-images/06-density-not-provided.png)

密度固有のリソースを使用して設計されたレイアウトと比較します。

![密度固有のリソースを含むスクリーンショット](resources-for-varying-screens-images/07-density-specific-resources.png)

### <a name="create-varying-density-resources-with-android-asset-studio"></a>Android Asset Studio を使用してさまざまな密度リソースを作成する

さまざまな密度のビットマップの作成は、少し面倒になることがあります。 そのため、Google は、 [**Android Asset Studio**](https://romannurik.github.io/AndroidAssetStudio/)と呼ばれるこれらのビットマップの作成に関連する面倒な作業の一部を減らすことができるオンラインユーティリティを作成しました。

[Android Asset Studio の![](resources-for-varying-screens-images/08-android-asset-studio-sml.png)](resources-for-varying-screens-images/08-android-asset-studio.png#lightbox)

この web サイトは、1つのイメージを提供することで、4つの一般的な画面密度を対象とするビットマップの作成に役立ちます。 Android Asset Studio は、いくつかのカスタマイズでビットマップを作成し、zip ファイルとしてダウンロードできるようにします。

## <a name="tips-for-multiple-screens"></a>複数の画面に関するヒント

Android は困惑のデバイスで実行され、画面のサイズと画面密度の組み合わせには圧倒される可能性があります。 次のヒントは、さまざまなデバイスをサポートするために必要な作業を最小限に抑えるのに役立ちます。

- いくつかの異なるデバイスが存在する &ndash;、**必要なものに対してのみ設計と開発**を行いますが、の設計と開発にかなりの労力を要する可能性がある、まれなフォームファクターに存在することもあります。 [**画面のサイズと密度**](https://developer.android.com/resources/dashboard/screens.html)のダッシュボードは、Google によって提供されるページであり、画面サイズ/画面密度マトリックスの内訳に関するデータを提供します。 この内訳では、画面のサポートに関する開発作業についての洞察を提供します。

- **ピクセル単位ではなく、DPs を使用する**と、画面の密度が変化します。 ピクセル値をハードコーディングしないでください。 Dp (密度に依存しないピクセル) を優先するピクセルは避けてください。

- **可能な**限り、 [AbsoluteLayout](xref:Android.Widget.AbsoluteLayout)
  は使用し**ない**でください &ndash; API レベル 3 (Android 1.5) で非推奨とされ、レイアウトが不安定になります。 使用しないでください。 代わりに、 [**LinearLayout**](xref:Android.Widget.LinearLayout)、 [**RelativeLayout**](xref:Android.Widget.RelativeLayout)、new [**GridLayout**](xref:Android.Widget.GridLayout)などのより柔軟なレイアウトウィジェットを使用してみてください。

- **既定の &ndash; として1つのレイアウトの向きを選択し**ます。たとえば、代替リソース**レイアウト**と**レイアウトポート**を指定する代わりに、**レイアウト**に横長のリソースを配置し、縦長のリソースを**layout-port**。

- **Height と Width に LayoutParams を使用する**-XML レイアウトファイルで UI 要素を定義する場合、 **wrap_content**と**fill_parent**の値を使用する Android アプリケーションでは、ピクセルまたは密度に依存しない単位を使用します。 これらのディメンション値により、Android はビットマップリソースを必要に応じてスケーリングします。 これと同じ理由から、密度に依存しない単位は、UI 要素の余白と埋め込みを指定する場合に最適です。

## <a name="testing-multiple-screens"></a>複数の画面のテスト

Android アプリケーションは、サポートされるすべての構成に対してテストする必要があります。 理想的には、実際のデバイス自体でデバイスをテストすることをお勧めしますが、多くの場合、これは不可能であり、現実的ではありません。
この場合、デバイス構成ごとにエミュレーターと Android 仮想デバイスのセットアップを使用すると便利です。

Android SDK には、AVDs の作成に使用できるエミュレータースキンがいくつか用意されており、多くのデバイスのサイズ、密度、および解像度がレプリケートされます。
多くのハードウェアベンダーは、デバイスのスキンを提供しています。

別の方法として、サードパーティのテストサービスのサービスを使用することもできます。
これらのサービスは APK を使用し、さまざまなデバイスで実行してから、アプリケーションの動作に関するフィードバックを提供します。
