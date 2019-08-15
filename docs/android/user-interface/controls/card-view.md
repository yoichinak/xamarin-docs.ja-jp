---
title: CardView
description: Cardview ウィジェットは、カードに似たビューでテキストとイメージの内容を表示する UI コンポーネントです。 このガイドでは、以前のバージョンの Android との下位互換性を維持しながら、Xamarin Android アプリケーションで CardView を使用およびカスタマイズする方法について説明します。
ms.prod: xamarin
ms.assetid: CF12FE85-D03A-4E64-95D2-D7115061A500
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: 74d626fb1028c630b67888f84153adeb33ae32b9
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68644691"
---
# <a name="xamarinandroid-cardview"></a>Xamarin Android CardView

_Cardview ウィジェットは、カードに似たビューでテキストとイメージの内容を表示する UI コンポーネントです。このガイドでは、以前のバージョンの Android との下位互換性を維持しながら、Xamarin Android アプリケーションで CardView を使用およびカスタマイズする方法について説明します。_

## <a name="overview"></a>概要

Android 5.0 (ロリポップ) で導入されたウィジェットは、カードに似たビューでテキストとイメージの内容を表示するUIコンポーネントです。`Cardview` `CardView`は、角が`FrameLayout`丸く、影が付いたウィジェットとして実装されます。 通常、 `CardView` `ListView`または`GridView`ビューグループに単一の行項目を表示するには、を使用します。 たとえば、次のスクリーンショットは、スクロール`CardView` `ListView`可能なでベースの旅行先カードを実装する旅行予約アプリの例です。

![旅行先ごとに CardView を使用するサンプルアプリ](card-view-images/01-cardview-example.png)

このガイドでは`CardView` 、Xamarin Android プロジェクトにパッケージを追加する方法、レイアウトに追加`CardView`する方法、アプリでの`CardView`外観をカスタマイズする方法について説明します。 さらに、このガイドでは、変更できる`CardView`属性の詳細な一覧を示します。これは、 `CardView` android 5.0 ロリポップより前のバージョンの android での使用に役立つ属性も含まれています。

<a name="requirements" />

## <a name="requirements"></a>必要条件

Xamarin ベースのアプリで新しい Android 5.0 以降の機能 (を含む`CardView`) を使用するには、次のものが必要です。

-  **Xamarin.Android** &ndash; Xamarin.Android 4.20 or later must be installed and configured with either Visual Studio or Visual Studio for Mac.

-  **Android SDK**&ndash; Android SDK Manager を使用して、Android 5.0 (API 21) 以降をインストールする必要があります。

-  **JAVA JDK 1.8**&ndash;明示的に API レベル23以前をターゲットにする場合は、JDK 1.7 を使用できます。 JDK 1.8 は、 [Oracle](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)から入手できます。

アプリにも`Xamarin.Android.Support.v7.CardView`パッケージが含まれている必要があります。 Visual Studio for Mac にパッケージ`Xamarin.Android.Support.v7.CardView`を追加するには、次のようにします。

1. プロジェクトを開き、 **[パッケージ]** を右クリックして **[パッケージの追加]** を選択します。

2. **[パッケージの追加]** ダイアログボックスで、 **CardView**を検索します。

3. **[Xamarin Support Library V7 CardView]** を選択し、 **[パッケージの追加]** をクリックします。

Visual Studio で`Xamarin.Android.Support.v7.CardView`パッケージを追加するには、次のようにします。

1. プロジェクトを開き、 **[参照]** ノード ( **[ソリューションエクスプローラー]** ウィンドウ) を右クリックして、 **[NuGet パッケージの管理...]** を選択します。

2. **[NuGet パッケージの管理]** ダイアログボックスが表示されたら、検索ボックスに「 **CardView** 」と入力します。

3. **Xamarin Support Library V7 CardView**が表示されたら、 **[インストール]** をクリックします。

Android 5.0 アプリケーションプロジェクトを構成する方法については、「 [android 5.0 プロジェクトの設定](~/android/platform/lollipop.md)」を参照してください。
NuGet パッケージのインストールの詳細について[は、「チュートリアル:プロジェクト](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)に NuGet を含めます。


## <a name="introducing-cardview"></a>CardView の概要

既定値`CardView`は、最小の角が付いた白いカードと、小さな影を持つ白いカードに似ています。 次の例では、を`TextView`含む単一`CardView`のウィジェットが表示され**ます。**

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

このレイアウト例では、 `CardView`次のスクリーンショットに示すように、1行のテキストで既定のを作成します。

[![白の背景とテキスト行を含む CardView のスクリーンショット](card-view-images/02-basic-cardview-sml.png)](card-view-images/02-basic-cardview.png#lightbox)

この例では、アプリのスタイルを明るいマテリアルのテーマ (`Theme.Material.Light`) に設定して、 `CardView`影とエッジが見やすくなるようにしています。 Android 5.0 アプリのテーマの詳細については、「[マテリアルのテーマ](~/android/user-interface/material-theme.md)」を参照してください。 次のセクションでは、アプリケーションのをカスタマイズ`CardView`する方法について説明します。


## <a name="customizing-cardview"></a>CardView のカスタマイズ

基本`CardView`属性を変更して、 `CardView`アプリのの外観をカスタマイズできます。 たとえば、の標高`CardView`を上げて、より大きな影をキャストすることができます (これにより、カードが背景より上位になるように見えます)。 また、角の半径を大きくして、カードの角をさらに丸めるようにすることもできます。

次のレイアウト例では、カスタマイズ`CardView`されたを使用して、印刷写真 ("スナップショット") のシミュレーションを作成します。 はイメージ`CardView`を表示するためにに追加され、はイメージのタイトル`ImageView`を表示するためにの下に配置されます。 `TextView` `ImageView`
このレイアウト例では、 `CardView`に次のカスタマイズがあります。

-  は`cardElevation` 、大きな影をキャストするために、4dp に増加しています。
-  が`cardCornerRadius` 5dp に上がり、角が丸められるようになりました。

は`CardView` Android v7 サポートライブラリによって提供されるため、 `android:`名前空間から属性を使用することはできません。 したがって、独自の XML 名前空間を定義し、その名前空間`CardView`を属性プレフィックスとして使用する必要があります。 次のレイアウト例では、次の行を使用してという`cardview`名前空間を定義します。

```xml
    xmlns:cardview="http://schemas.android.com/apk/res-auto"
```

この名前空間`card_view`を呼び出すこと`myapp`も、選択した場合でも呼び出すことができます (このファイルのスコープ内でのみアクセス可能です)。 この名前空間を呼び出すことを選択した場合は、変更`CardView`する属性のプレフィックスとして使用する必要があります。 このレイアウト例`cardview`では、名前空間はと`cardCornerRadius`の`cardElevation`プレフィックスです。

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

このレイアウト例を使用して写真表示アプリで画像を表示すると、 `CardView`次のスクリーンショットに示すように、に写真スナップショットが表示されます。

[![イメージとキャプションを画像の下に CardView](card-view-images/03-photo-cardview-sml.png)](card-view-images/03-photo-cardview.png#lightbox)

このスクリーンショットは、 [RecyclerViewer](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-recyclerviewer)サンプルアプリから抜粋したもの`RecyclerView`です。このアプリでは、 `CardView`ウィジェットを使用して、写真を表示するためのイメージのスクロールリストを表示します。 の詳細`RecyclerView`については、 [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)ガイドを参照してください。

では、 `CardView`複数の子ビューをコンテンツ領域に表示できることに注意してください。 たとえば、上記の写真表示アプリの例では、コンテンツ領域は`ListView` `ImageView`とを`TextView`含むで構成されています。 多くの場合、`CardView` インスタンスは垂直方向に配置されますが、水平方向に配置することもできます(例のスクリーンショットについては、「[カスタムビュースタイルの作成](~/android/user-interface/material-theme.md#customview)」を参照してください)。


### <a name="cardview-layout-options"></a>CardView レイアウトオプション

`CardView`レイアウトをカスタマイズするには、埋め込み、標高、角の半径、および背景色に影響を与える1つ以上の属性を設定します。

[![CardView 属性の図](card-view-images/04-attributes-sml.png)](card-view-images/04-attributes.png#lightbox)

各属性は、対応する`CardView`メソッドを呼び出すことによって動的に変更することもできます ( `CardView`メソッドの詳細については、「 [CardView クラスのリファレンス](https://developer.android.com/reference/android/support/v7/widget/CardView.html)」を参照してください)。
これらの属性 (背景色を除く) では、ディメンション値 (10 進数の後に単位が続く) を受け取ることに注意してください。 たとえば、は`11.5dp` 11.5 の密度に依存しないピクセルを指定します。


#### <a name="padding"></a>[間隔]

`CardView`は、カード内にコンテンツを配置するための5つの埋め込み属性を提供します。 これらは、レイアウト XML で設定するか、コード内で類似のメソッドを呼び出すことができます。

[![CardView padding 属性の図](card-view-images/05-padding-sml.png)](card-view-images/05-padding.png#lightbox)

パディングの属性については、次のように説明します。

-  `contentPadding`の子ビューとカードのすべての端との間の内部余白。 `CardView` &ndash;

-  `contentPaddingBottom`の子ビューとカードの下端との間の内部余白。 `CardView` &ndash;

-  `contentPaddingLeft`の子ビューとカードの左端との間の内部余白。 `CardView` &ndash;

-  `contentPaddingRight`の子ビューとカードの右端との間の内部余白。 `CardView` &ndash;

-  `contentPaddingTop`の子ビューとカードの上端の間の内部余白。 `CardView` &ndash;

コンテンツの埋め込み属性は、コンテンツ領域内にある特定のウィジェットではなく、コンテンツ領域の境界を基準としています。
たとえば、フォト表示`contentPadding`アプリでが十分に増加した場合、 `CardView`は画像とカードに表示されているテキストの両方をトリミングします。



#### <a name="elevation"></a>高度

`CardView`は、昇格を制御する2つの昇格属性と、結果として、影のサイズを提供します。

[![CardView の昇格属性の図](card-view-images/06-elevation-sml.png)](card-view-images/06-elevation.png#lightbox)

昇格属性は次のように説明されています。

-  `cardElevation`&ndash; の標高(Z軸`CardView`を表します)。

-  `cardMaxElevation`&ndash;の昇格`CardView`の最大値。

の`cardElevation` 値`CardView`を大きくすると、影のサイズが大きくなり、背景の上の方が手前に見えるようになります。 また、重なり合ったビューの描画順序も決定されます。 `CardView`つまり、は、高いレベルの昇格が設定された別の重なり合うビューの下に描画されます。 `cardElevation`
この`cardMaxElevation`設定は、アプリが昇格を動的&ndash;に変更したときに、この設定で定義した制限を超えてシャドウが拡張されないようにする場合に便利です。


#### <a name="corner-radius-and-background-color"></a>角の半径と背景色

`CardView`には、コーナー半径と背景色を制御するために使用できる属性が用意されています。 この2つのプロパティを使用すると、 `CardView`の全体的なスタイルを変更できます。

[![CardView コーナーの図と背景色の属性](card-view-images/07-radius-bgcolor-sml.png)](card-view-images/07-radius-bgcolor.png#lightbox)

これらの属性について、次のように説明します。

-  `cardCornerRadius`のすべてのコーナーの角の半径。 `CardView` &ndash;

-  `cardBackgroundColor`&ndash; の`CardView`背景色。

この図では`cardCornerRadius` 、はより丸みのある10dp に`cardBackgroundColor`設定され`"#FFFFCC"` 、は (明るい黄色) に設定されています。


## <a name="compatibility"></a>互換性

Android 5.0 ロリポップ`CardView`より前のバージョンの android でを使用できます。 は`CardView` android v7 サポートライブラリの一部であるため、android 2.1 `CardView` (API レベル 7) 以上のを使用できます。
ただし、上記の[要件](#requirements)に`Xamarin.Android.Support.v7.CardView`従ってパッケージをインストールする必要があります。

`CardView`ロリポップの前のデバイスで動作が若干異なります (API レベル 21):

-  `CardView`プログラムによる影の実装を使用して、パディングを追加します。

-  `CardView`は、 `CardView`の丸い角と交差する子ビューをクリップしません。

これらの互換性の違いを管理する`CardView`ために、には、レイアウトで構成できるいくつかの追加属性が用意されています。

-   `cardPreventCornerOverlap`アプリが以前の`true`バージョンの Android で実行されている場合は、この属性をに設定してパディングを追加します (API レベル20以前)。 &ndash; この設定に`CardView`より、 `CardView`コンテンツがの丸い角と交差しないようにします。

-   `cardUseCompatPadding`アプリが API レベル`true` 21 以降のバージョンで実行されている場合は、この属性をに設定して、埋め込みを追加します。 &ndash; ロリポップ以前のデバイスで`CardView`を使用して、ロリポップ (またはそれ以降) で同じ外観にする場合は、この属性`true`をに設定します。 この属性が有効な場合`CardView` 、は、事前ロリポップデバイスで実行されるときに影を描画するための余白を追加します。 これは、ロリポップのプログラムシャドウ実装が有効になっている場合に導入されるパディングの違いを克服するのに役立ちます。

以前のバージョンの Android との互換性を維持する方法の詳細については、「[互換性の維持](https://developer.android.com/training/material/compatibility.html)」を参照してください。


## <a name="summary"></a>まとめ

このガイドでは、 `CardView` Android 5.0 (ロリポップ) に含まれる新しいウィジェットを導入しました。 既定`CardView`の外観を示し、その昇格、角`CardView`の丸み、コンテンツの埋め込み、および背景色を変更してカスタマイズする方法を説明しました。 `CardView`レイアウト属性 (参照図を含む) を一覧表示し、android 5.0 ロリポップ`CardView`より前の android デバイスでを使用する方法について説明しました。 の詳細`CardView`については、 [CardView クラスのリファレンス](https://developer.android.com/reference/android/support/v7/widget/CardView.html)を参照してください。


## <a name="related-links"></a>関連リンク

- [RecyclerView (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-recyclerviewer)
- [ロリポップの概要](~/android/platform/lollipop.md)
- [CardView クラスの参照](https://developer.android.com/reference/android/support/v7/widget/CardView.html)
