---
title: CardView
description: Cardview ウィジェットは、カードに似たビューでテキストとイメージの内容を表示する UI コンポーネントです。 このガイドでは、以前のバージョンの Android との下位互換性を維持しながら、Xamarin Android アプリケーションで CardView を使用およびカスタマイズする方法について説明します。
ms.prod: xamarin
ms.assetid: CF12FE85-D03A-4E64-95D2-D7115061A500
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: 2684866d04f2b085c70c70a04ff2c828205f0f32
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/09/2020
ms.locfileid: "84571893"
---
# <a name="xamarinandroid-cardview"></a>Xamarin Android CardView

_Cardview ウィジェットは、カードに似たビューでテキストとイメージの内容を表示する UI コンポーネントです。このガイドでは、以前のバージョンの Android との下位互換性を維持しながら、Xamarin Android アプリケーションで CardView を使用およびカスタマイズする方法について説明します。_

## <a name="overview"></a>概要

`Cardview`Android 5.0 (ロリポップ) で導入されたウィジェットは、カードに似たビューでテキストとイメージの内容を表示する UI コンポーネントです。 `CardView`は、角が `FrameLayout` 丸く、影が付いたウィジェットとして実装されます。 通常、 `CardView` またはビューグループに単一の行項目を表示するには、を使用し `ListView` `GridView` ます。 たとえば、次のスクリーンショットは、 `CardView` スクロール可能なでベースの旅行先カードを実装する旅行予約アプリの例です `ListView` 。

![旅行先ごとに CardView を使用するサンプルアプリ](card-view-images/01-cardview-example.png)

このガイドでは、 `CardView` Xamarin Android プロジェクトにパッケージを追加する方法、レイアウトに追加する方法、 `CardView` アプリでの外観をカスタマイズする方法について説明します `CardView` 。 さらに、このガイドでは、変更できる属性の詳細な一覧を示し `CardView` ます。これは、 `CardView` Android 5.0 ロリポップより前のバージョンの android での使用に役立つ属性も含まれています。

<a name="requirements"></a>

## <a name="requirements"></a>必要条件

Xamarin ベースのアプリで新しい Android 5.0 以降の機能 (を含む) を使用するには、次のものが必要です `CardView` 。

- **Xamarin android** &ndash; 4.20 以降をインストールして、Visual Studio または Visual Studio for Mac で構成する必要があります。

- **Android SDK** &ndash;Android SDK Manager を使用して、Android 5.0 (API 21) 以降をインストールする必要があります。

- **JAVA JDK 1.8** &ndash;API レベル23以前を対象としている場合は、JDK 1.7 を使用できます。 JDK 1.8 は、 [Oracle](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)から入手できます。

アプリにもパッケージが含まれている必要があり `Xamarin.Android.Support.v7.CardView` ます。 Visual Studio for Mac にパッケージを追加するには、次のようにし `Xamarin.Android.Support.v7.CardView` ます。

1. プロジェクトを開き、[**パッケージ**] を右クリックして [**パッケージの追加**] を選択します。

2. [**パッケージの追加**] ダイアログボックスで、 **CardView**を検索します。

3. [ **Xamarin Support Library V7 CardView**] を選択し、[**パッケージの追加**] をクリックします。

Visual Studio でパッケージを追加するには、次のようにし `Xamarin.Android.Support.v7.CardView` ます。

1. プロジェクトを開き、[**参照**] ノード ([**ソリューションエクスプローラー** ] ウィンドウ) を右クリックして、[ **NuGet パッケージの管理...**] を選択します。

2. [ **NuGet パッケージの管理**] ダイアログボックスが表示されたら、検索ボックスに「 **CardView** 」と入力します。

3. **Xamarin Support Library V7 CardView**が表示されたら、[**インストール**] をクリックします。

Android 5.0 アプリケーションプロジェクトを構成する方法については、「 [android 5.0 プロジェクトの設定](~/android/platform/lollipop.md)」を参照してください。
NuGet パッケージのインストールの詳細については、「[チュートリアル: プロジェクトに nuget を含める](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)」を参照してください。

## <a name="introducing-cardview"></a>CardView の概要

既定値は、 `CardView` 最小の角が付いた白いカードと、小さな影を持つ白いカードに似ています。 次の例では、を含む単一のウィジェットが表示され**ます。** `CardView` `TextView`

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:gravity="center_horizontal"
    android:padding="5dp">
    <android.support.v7.widget.CardView
        android:layout_width="fill_parent"
        android:layout_height="245dp"
        android:layout_gravity="center_horizontal">
        <TextView
            android:text="Basic CardView"
            android:layout_marginTop="0dp"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:gravity="center"
            android:layout_centerVertical="true"
            android:layout_alignParentRight="true"
            android:layout_alignParentEnd="true" />
    </android.support.v7.widget.CardView>
</LinearLayout>
```

この XML を使用して、 **Main**の既存の内容を置き換える場合は、前の xml のリソースを参照する**MainActivity.cs**内のコードを必ずコメントアウトしてください。

このレイアウト例では、 `CardView` 次のスクリーンショットに示すように、1行のテキストで既定のを作成します。

[![白の背景とテキスト行を含む CardView のスクリーンショット](card-view-images/02-basic-cardview-sml.png)](card-view-images/02-basic-cardview.png#lightbox)

この例では、アプリのスタイルを明るいマテリアルのテーマ () に設定し `Theme.Material.Light` `CardView` て、影とエッジが見やすくなるようにしています。 Android 5.0 アプリのテーマの詳細については、「[マテリアルのテーマ](~/android/user-interface/material-theme.md)」を参照してください。 次のセクションでは、アプリケーションのをカスタマイズする方法について説明し `CardView` ます。

## <a name="customizing-cardview"></a>CardView のカスタマイズ

基本属性を変更して `CardView` 、アプリのの外観をカスタマイズでき `CardView` ます。 たとえば、の標高を上げて、 `CardView` より大きな影をキャストすることができます (これにより、カードが背景より上位になるように見えます)。 また、角の半径を大きくして、カードの角をさらに丸めるようにすることもできます。

次のレイアウト例では、カスタマイズされたを使用して、 `CardView` 印刷写真 ("スナップショット") のシミュレーションを作成します。 はイメージを表示するためにに追加され、は `ImageView` `CardView` `TextView` イメージのタイトルを表示するためにの下に配置され `ImageView` ます。
このレイアウト例では、に `CardView` 次のカスタマイズがあります。

- は、 `cardElevation` 大きな影をキャストするために、4dp に増加しています。
- が `cardCornerRadius` 5dp に上がり、角が丸められるようになりました。

`CardView`は Android v7 サポートライブラリによって提供されるため、名前空間から属性を使用することはできません `android:` 。 したがって、独自の XML 名前空間を定義し、その名前空間を属性プレフィックスとして使用する必要があり `CardView` ます。 次のレイアウト例では、次の行を使用してという名前空間を定義し `cardview` ます。

```xml
    xmlns:cardview="http://schemas.android.com/apk/res-auto"
```

この名前空間を呼び出すことも、選択した場合でも呼び出すことができ `card_view` `myapp` ます (このファイルのスコープ内でのみアクセス可能です)。 この名前空間を呼び出すことを選択した場合は、変更する属性のプレフィックスとして使用する必要があり `CardView` ます。 このレイアウト例では、 `cardview` 名前空間はとのプレフィックスです `cardElevation` `cardCornerRadius` 。

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:cardview="http://schemas.android.com/apk/res-auto"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:gravity="center_horizontal"
    android:padding="5dp">
    <android.support.v7.widget.CardView
        android:layout_width="fill_parent"
        android:layout_height="245dp"
        android:layout_gravity="center_horizontal"
        cardview:cardElevation="4dp"
        cardview:cardCornerRadius="5dp">
        <LinearLayout
            android:layout_width="fill_parent"
            android:layout_height="240dp"
            android:orientation="vertical"
            android:padding="8dp">
            <ImageView
                android:layout_width="fill_parent"
                android:layout_height="190dp"
                android:id="@+id/imageView"
                android:scaleType="centerCrop" />
            <TextView
                android:layout_width="fill_parent"
                android:layout_height="wrap_content"
                android:textAppearance="?android:attr/textAppearanceMedium"
                android:textColor="#333333"
                android:text="Photo Title"
                android:id="@+id/textView"
                android:layout_gravity="center_horizontal"
                android:layout_marginLeft="5dp" />
        </LinearLayout>
    </android.support.v7.widget.CardView>
</LinearLayout>
```

このレイアウト例を使用して写真表示アプリで画像を表示すると、 `CardView` 次のスクリーンショットに示すように、に写真スナップショットが表示されます。

[![イメージとキャプションを画像の下に CardView](card-view-images/03-photo-cardview-sml.png)](card-view-images/03-photo-cardview.png#lightbox)

このスクリーンショットは、 [RecyclerViewer](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-recyclerviewer)サンプルアプリから抜粋したものです。このアプリでは、ウィジェットを使用し `RecyclerView` て、写真を表示するためのイメージのスクロールリストを表示し `CardView` ます。 の詳細については、 `RecyclerView` [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)ガイドを参照してください。

では、複数の `CardView` 子ビューをコンテンツ領域に表示できることに注意してください。 たとえば、上記の写真表示アプリの例では、コンテンツ領域はとを含むで構成されてい `ListView` `ImageView` `TextView` ます。 `CardView`多くの場合、インスタンスは垂直方向に配置されますが、水平方向に配置することもできます (例のスクリーンショットについては[、「カスタムビュースタイルの作成](~/android/user-interface/material-theme.md#customview)」を参照してください)。

### <a name="cardview-layout-options"></a>CardView レイアウトオプション

`CardView`レイアウトをカスタマイズするには、埋め込み、標高、角の半径、および背景色に影響を与える1つ以上の属性を設定します。

[![CardView 属性の図](card-view-images/04-attributes-sml.png)](card-view-images/04-attributes.png#lightbox)

各属性は、対応するメソッドを呼び出すことによって動的に変更することもでき `CardView` ます (メソッドの詳細については、 `CardView` 「 [CardView クラスのリファレンス](https://developer.android.com/reference/android/support/v7/widget/CardView.html)」を参照してください)。
これらの属性 (背景色を除く) では、ディメンション値 (10 進数の後に単位が続く) を受け取ることに注意してください。 たとえば、は `11.5dp` 11.5 の密度に依存しないピクセルを指定します。

#### <a name="padding"></a>余白

`CardView`は、カード内にコンテンツを配置するための5つの埋め込み属性を提供します。 これらは、レイアウト XML で設定するか、コード内で類似のメソッドを呼び出すことができます。

[![CardView padding 属性の図](card-view-images/05-padding-sml.png)](card-view-images/05-padding.png#lightbox)

パディングの属性については、次のように説明します。

- `contentPadding`&ndash;の子ビュー `CardView` とカードのすべての端との間の内部余白。

- `contentPaddingBottom`&ndash;の子ビュー `CardView` とカードの下端との間の内部余白。

- `contentPaddingLeft`&ndash;の子ビュー `CardView` とカードの左端との間の内部余白。

- `contentPaddingRight`&ndash;の子ビュー `CardView` とカードの右端との間の内部余白。

- `contentPaddingTop`&ndash;の子ビュー `CardView` とカードの上端の間の内部余白。

コンテンツの埋め込み属性は、コンテンツ領域内にある特定のウィジェットではなく、コンテンツ領域の境界を基準としています。
たとえば、 `contentPadding` フォト表示アプリでが十分に増加した場合、は `CardView` 画像とカードに表示されているテキストの両方をトリミングします。

#### <a name="elevation"></a>Elevation

`CardView`は、昇格を制御する2つの昇格属性と、結果として、影のサイズを提供します。

[![CardView の昇格属性の図](card-view-images/06-elevation-sml.png)](card-view-images/06-elevation.png#lightbox)

昇格属性は次のように説明されています。

- `cardElevation`&ndash;の標高 `CardView` (Z 軸を表します)。

- `cardMaxElevation`の &ndash; 昇格の最大値 `CardView` 。

の値を大きくすると `cardElevation` 、影のサイズが大きくなり、 `CardView` 背景の上の方が手前に見えるようになります。 `cardElevation`また、重なり合ったビューの描画順序も決定されます。つまり、は、高いレベルの昇格が設定さ `CardView` れた別の重なり合うビューの下に描画されます。
`cardMaxElevation`この設定は、アプリが昇格を動的に変更したときに、 &ndash; この設定で定義した制限を超えてシャドウが拡張されないようにする場合に便利です。

#### <a name="corner-radius-and-background-color"></a>角の半径と背景色

`CardView`には、コーナー半径と背景色を制御するために使用できる属性が用意されています。 この2つのプロパティを使用すると、の全体的なスタイルを変更でき `CardView` ます。

[![CardView コーナーの図と背景色の属性](card-view-images/07-radius-bgcolor-sml.png)](card-view-images/07-radius-bgcolor.png#lightbox)

これらの属性について、次のように説明します。

- `cardCornerRadius`&ndash;のすべてのコーナーの角の半径 `CardView` 。

- `cardBackgroundColor`&ndash;の背景色 `CardView` 。

この図で `cardCornerRadius` は、はより丸みのある10dp に設定され、 `cardBackgroundColor` は `"#FFFFCC"` (明るい黄色) に設定されています。

## <a name="compatibility"></a>互換性

Android `CardView` 5.0 ロリポップより前のバージョンの android でを使用できます。 `CardView`は android v7 サポートライブラリの一部であるため、 `CardView` android 2.1 (API レベル 7) 以上のを使用できます。
ただし、上記の要件に従ってパッケージをインストールする必要があり `Xamarin.Android.Support.v7.CardView` ます。 [Requirements](#requirements)

`CardView`ロリポップの前のデバイスで動作が若干異なります (API レベル 21):

- `CardView`プログラムによる影の実装を使用して、パディングを追加します。

- `CardView`は、の丸い角と交差する子ビューをクリップしません `CardView` 。

これらの互換性の違いを管理するために、には、 `CardView` レイアウトで構成できるいくつかの追加属性が用意されています。

- `cardPreventCornerOverlap`&ndash; `true` アプリが以前のバージョンの Android で実行されている場合は、この属性をに設定してパディングを追加します (API レベル20以前)。 この設定 `CardView` により、コンテンツがの丸い角と交差しないように `CardView` します。

- `cardUseCompatPadding`&ndash; `true` アプリが API レベル21以降のバージョンで実行されている場合は、この属性をに設定して、埋め込みを追加します。 `CardView`ロリポップ以前のデバイスでを使用して、ロリポップ (またはそれ以降) で同じ外観にする場合は、この属性をに設定し `true` ます。 この属性が有効な場合、は、 `CardView` 事前ロリポップデバイスで実行されるときに影を描画するための余白を追加します。 これは、ロリポップのプログラムシャドウ実装が有効になっている場合に導入されるパディングの違いを克服するのに役立ちます。

以前のバージョンの Android との互換性を維持する方法の詳細については、「[互換性の維持](https://developer.android.com/training/material/compatibility.html)」を参照してください。

## <a name="summary"></a>まとめ

このガイドでは、 `CardView` Android 5.0 (ロリポップ) に含まれる新しいウィジェットを導入しました。 既定の外観を `CardView` `CardView` 示し、その昇格、角の丸み、コンテンツの埋め込み、および背景色を変更してカスタマイズする方法を説明しました。 `CardView`レイアウト属性 (参照図を含む) を一覧表示し、 `CardView` Android 5.0 ロリポップより前の android デバイスでを使用する方法について説明しました。 の詳細については `CardView` 、 [CardView クラスのリファレンス](https://developer.android.com/reference/android/support/v7/widget/CardView.html)を参照してください。

## <a name="related-links"></a>関連リンク

- [RecyclerView (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-recyclerviewer)
- [ロリポップの概要](~/android/platform/lollipop.md)
- [CardView クラスの参照](https://developer.android.com/reference/android/support/v7/widget/CardView.html)
