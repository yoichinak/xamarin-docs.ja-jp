---
title: CardView
description: Cardview ウィジェットは、カードに似たビューでテキストとイメージの内容を表示する UI コンポーネントです。 このガイドでは、以前のバージョンの Android との下位互換性を維持しながら、Xamarin Android アプリケーションで CardView を使用およびカスタマイズする方法について説明します。
ms.prod: xamarin
ms.assetid: CF12FE85-D03A-4E64-95D2-D7115061A500
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: 053847426d770408826297d9a80b6e38d7f6bc44
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029281"
---
# <a name="xamarinandroid-cardview"></a>Xamarin Android CardView

_Cardview ウィジェットは、カードに似たビューでテキストとイメージの内容を表示する UI コンポーネントです。このガイドでは、以前のバージョンの Android との下位互換性を維持しながら、Xamarin Android アプリケーションで CardView を使用およびカスタマイズする方法について説明します。_

## <a name="overview"></a>概要

Android 5.0 (ロリポップ) で導入された `Cardview` ウィジェットは、カードに似たビューでテキストとイメージの内容を表示する UI コンポーネントです。 `CardView` は、角が丸く、影が `FrameLayout` ウィジェットとして実装されます。 通常、`CardView` は、`ListView` または `GridView` ビューグループに単一の行項目を表示するために使用されます。 たとえば、次のスクリーンショットは、スクロール可能な `ListView`で `CardView`ベースの旅行先カードを実装する旅行予約アプリの例です。

![旅行先ごとに CardView を使用するサンプルアプリ](card-view-images/01-cardview-example.png)

このガイドでは、`CardView` パッケージを Xamarin Android プロジェクトに追加する方法、レイアウトに `CardView` を追加する方法、アプリの `CardView` の外観をカスタマイズする方法について説明します。 また、このガイドでは、Android 5.0 ロリポップより前のバージョンの Android で `CardView` を使用するために役立つ属性など、変更できる `CardView` 属性の詳細な一覧を示します。

<a name="requirements" />

## <a name="requirements"></a>［要件］

Xamarin ベースのアプリで新しい Android 5.0 以降の機能 (`CardView`を含む) を使用するには、次のものが必要です。

- Xamarin **android** 4.20 &ndash; Visual Studio または Visual Studio for Mac を使用してインストールおよび構成する必要があります。

- **Android SDK** &ndash; Android 5.0 (API 21) 以降を Android SDK Manager を使用してインストールする必要があります。

- API レベル23以前をターゲットにしている場合は、 **JAVA jdk 1.8** &ndash; jdk 1.7 を使用できます。 JDK 1.8 は、 [Oracle](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)から入手できます。

アプリには、`Xamarin.Android.Support.v7.CardView` パッケージも含める必要があります。 Visual Studio for Mac に `Xamarin.Android.Support.v7.CardView` パッケージを追加するには、次のようにします。

1. プロジェクトを開き、 **[パッケージ]** を右クリックして **[パッケージの追加]** を選択します。

2. **[パッケージの追加]** ダイアログボックスで、 **CardView**を検索します。

3. **[Xamarin Support Library V7 CardView]** を選択し、 **[パッケージの追加]** をクリックします。

Visual Studio で `Xamarin.Android.Support.v7.CardView` パッケージを追加するには、次のようにします。

1. プロジェクトを開き、 **[参照]** ノード ( **[ソリューションエクスプローラー]** ウィンドウ) を右クリックして、 **[NuGet パッケージの管理...]** を選択します。

2. **[NuGet パッケージの管理]** ダイアログボックスが表示されたら、検索ボックスに「 **CardView** 」と入力します。

3. **Xamarin Support Library V7 CardView**が表示されたら、 **[インストール]** をクリックします。

Android 5.0 アプリケーションプロジェクトを構成する方法については、「 [android 5.0 プロジェクトの設定](~/android/platform/lollipop.md)」を参照してください。
NuGet パッケージのインストールの詳細については、「[チュートリアル: プロジェクトに nuget を含める](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)」を参照してください。

## <a name="introducing-cardview"></a>CardView の概要

既定の `CardView` は、最小の角が付いた白いカードに似ており、影はわずかです。 次のサンプルの例では、`TextView`を含む単一の `CardView` ウィジェットを表示**しています。**

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

このレイアウト例では、次のスクリーンショットに示すように、1行のテキストを使用して既定の `CardView` を作成します。

[白い背景とテキスト行を含む CardView のスクリーンショットの![](card-view-images/02-basic-cardview-sml.png)](card-view-images/02-basic-cardview.png#lightbox)

この例では、アプリのスタイルを明るいマテリアルテーマ (`Theme.Material.Light`) に設定して、`CardView` 影とエッジが見やすくなるようにします。 Android 5.0 アプリのテーマの詳細については、「[マテリアルのテーマ](~/android/user-interface/material-theme.md)」を参照してください。 次のセクションでは、アプリケーションの `CardView` をカスタマイズする方法について説明します。

## <a name="customizing-cardview"></a>CardView のカスタマイズ

基本 `CardView` 属性を変更して、アプリの `CardView` の外観をカスタマイズできます。 たとえば、`CardView` の標高を上げて、より大きな影 (カードを背景の上にフロートするように見える) をキャストすることができます。 また、角の半径を大きくして、カードの角をさらに丸めるようにすることもできます。

次のレイアウト例では、カスタマイズされた `CardView` を使用して、印刷写真 ("スナップショット") のシミュレーションを作成します。 画像を表示するための `CardView` に `ImageView` が追加され、画像のタイトルを表示するために `TextView` が `ImageView` の下に配置されます。
このレイアウト例では、`CardView` に次のカスタマイズがあります。

- `cardElevation` が4dp に上がり、大きな影がキャストされます。
- `cardCornerRadius` が5dp に上がり、角が丸められるようになりました。

`CardView` は Android v7 サポートライブラリによって提供されるため、`android:` 名前空間から属性を使用することはできません。 したがって、独自の XML 名前空間を定義し、その名前空間を `CardView` 属性プレフィックスとして使用する必要があります。 次のレイアウト例では、次の行を使用して、`cardview`という名前空間を定義します。

```xml
    xmlns:cardview="http://schemas.android.com/apk/res-auto"
```

この名前空間 `card_view` を呼び出すことができます。または、(このファイルのスコープ内でのみアクセス可能) を選択して `myapp` することもできます。 この名前空間を呼び出すことを選択した場合は、変更する `CardView` 属性のプレフィックスとして使用する必要があります。 このレイアウト例では、`cardview` 名前空間が `cardElevation` と `cardCornerRadius`のプレフィックスになります。

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

このレイアウト例を使用して写真表示アプリに画像を表示すると、次のスクリーンショットに示すように、`CardView` の写真スナップショットが表示されます。

[イメージとキャプションを画像の下に![CardView](card-view-images/03-photo-cardview-sml.png)](card-view-images/03-photo-cardview.png#lightbox)

このスクリーンショットは、`RecyclerView` ウィジェットを使用して写真を表示するための `CardView` イメージのスクロールリストを表示する、 [RecyclerViewer](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-recyclerviewer)サンプルアプリから抜粋したものです。 `RecyclerView`の詳細については、 [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)ガイドを参照してください。

`CardView` では、複数の子ビューをコンテンツ領域に表示できることに注意してください。 たとえば、上記の写真表示アプリの例では、コンテンツ領域は、`ImageView` と `TextView`を含む `ListView` で構成されています。 多くの場合 `CardView` インスタンスは垂直方向に配置されますが、水平方向に配置することもできます (例のスクリーンショットについては[、「カスタムビュースタイルの作成](~/android/user-interface/material-theme.md#customview)」を参照してください)。

### <a name="cardview-layout-options"></a>CardView レイアウトオプション

`CardView` のレイアウトをカスタマイズするには、埋め込み、標高、角の半径、および背景色に影響を与える1つ以上の属性を設定します。

[CardView 属性の![ダイアグラム](card-view-images/04-attributes-sml.png)](card-view-images/04-attributes.png#lightbox)

各属性は、対応する `CardView` メソッドを呼び出すことによって動的に変更することもできます (`CardView` メソッドの詳細については、 [CardView クラスリファレンス](https://developer.android.com/reference/android/support/v7/widget/CardView.html)を参照してください)。
これらの属性 (背景色を除く) では、ディメンション値 (10 進数の後に単位が続く) を受け取ることに注意してください。 たとえば、`11.5dp` では11.5 密度に依存しないピクセルが指定します。

#### <a name="padding"></a>[間隔]

`CardView` には、カード内にコンテンツを配置するための5つの埋め込み属性が用意されています。 これらは、レイアウト XML で設定するか、コード内で類似のメソッドを呼び出すことができます。

[CardView padding 属性の![ダイアグラム](card-view-images/05-padding-sml.png)](card-view-images/05-padding.png#lightbox)

パディングの属性については、次のように説明します。

- `CardView` の子ビューとカードのすべての端の間に &ndash; 内部余白を `contentPadding` します。

- `CardView` の子ビューとカードの下端との間に &ndash; 内部余白を `contentPaddingBottom` します。

- `CardView` の子ビューとカードの左端の間の内部余白を `contentPaddingLeft` &ndash; ます。

- `CardView` の子ビューとカードの右端との間の &ndash; 内部余白を `contentPaddingRight` します。

- `CardView` の子ビューとカードの上端の間の内部余白を `contentPaddingTop` &ndash; ます。

コンテンツの埋め込み属性は、コンテンツ領域内にある特定のウィジェットではなく、コンテンツ領域の境界を基準としています。
たとえば、写真表示アプリで `contentPadding` が十分に増加していた場合、`CardView` は画像とカードに表示されているテキストの両方をトリミングします。

#### <a name="elevation"></a>高度

`CardView` には、昇格を制御するための昇格属性が2つ用意されています。結果として、影のサイズは次のようになります。

[CardView の昇格属性の図![](card-view-images/06-elevation-sml.png)](card-view-images/06-elevation.png#lightbox)

昇格属性は次のように説明されています。

- `cardElevation` `CardView` の昇格 &ndash; ます (Z 軸を表します)。

- `CardView`の昇格の最大値 &ndash; `cardMaxElevation` します。

`cardElevation` の値を大きくすると、影のサイズが増加し、`CardView` が背景より上にあるように見えます。 `cardElevation` 属性は、重なっているビューの描画順序も決定します。つまり、`CardView` は、昇格設定が高い別のビューの下に描画されます。また、重なり合っているビューの方が低いほど、昇格が遅くなります。
`cardMaxElevation` 設定は、アプリが昇格を動的に変更し &ndash; この設定で定義した制限を超えてシャドウが拡張されないようにする場合に便利です。

#### <a name="corner-radius-and-background-color"></a>角の半径と背景色

`CardView` には、角の半径と背景色を制御するために使用できる属性が用意されています。 この2つのプロパティを使用すると、`CardView`の全体的なスタイルを変更できます。

[CardView コーナーと背景色の属性の図を![](card-view-images/07-radius-bgcolor-sml.png)](card-view-images/07-radius-bgcolor.png#lightbox)

これらの属性について、次のように説明します。

- `CardView`のすべてのコーナーの角の半径 &ndash; `cardCornerRadius` します。

- `CardView`の背景色 &ndash; `cardBackgroundColor` します。

この図では、`cardCornerRadius` がより丸みのある10dp に設定され、`cardBackgroundColor` が `"#FFFFCC"` (明るい黄色) に設定されています。

## <a name="compatibility"></a>互換性

Android 5.0 ロリポップより前のバージョンの Android では、`CardView` を使用できます。 `CardView` は Android v7 サポートライブラリの一部であるため、Android 2.1 (API レベル 7) 以降で `CardView` を使用できます。
ただし、上記の[要件](#requirements)に従って、`Xamarin.Android.Support.v7.CardView` パッケージをインストールする必要があります。

`CardView` は、ロリポップ (API レベル 21) より前のデバイスで若干異なる動作をします。

- `CardView` は、追加のパディングを追加するプログラムシャドウの実装を使用します。

- `CardView` は、`CardView`の丸い角と交差する子ビューをクリップしません。

これらの互換性の違いを管理するために、`CardView` には、レイアウトで構成できるいくつかの追加の属性が用意されています。

- &ndash; `cardPreventCornerOverlap`、この属性を `true` に設定して、アプリが以前のバージョンの Android (API レベル20以前) で実行されている場合にパディングを追加します。 この設定により、`CardView` コンテンツが `CardView`の丸い角と交差しないようにします。

- アプリが API レベル21以降のバージョンで実行されている場合にパディングを追加するには、この属性を `true` に設定 &ndash; `cardUseCompatPadding` します。 ロリポップ以前のデバイスで `CardView` を使用し、ロリポップ (またはそれ以降) で同じ外観にする場合は、この属性を `true`に設定します。 この属性が有効になっている場合、`CardView` は、事前ロリポップデバイスで実行されるときに影を描画するための埋め込みを追加します。 これは、ロリポップのプログラムシャドウ実装が有効になっている場合に導入されるパディングの違いを克服するのに役立ちます。

以前のバージョンの Android との互換性を維持する方法の詳細については、「[互換性の維持](https://developer.android.com/training/material/compatibility.html)」を参照してください。

## <a name="summary"></a>まとめ

このガイドでは、Android 5.0 (ロリポップ) に含まれる新しい `CardView` ウィジェットについて紹介しました。 既定の `CardView` の外観を示し、昇格、角の丸み、コンテンツの埋め込み、および背景色を変更することで `CardView` をカスタマイズする方法を説明しました。 `CardView` レイアウト属性 (リファレンス図を含む) と、Android 5.0 ロリポップより前の Android デバイスで `CardView` を使用する方法について説明しました。 `CardView`の詳細については、「 [CardView クラスのリファレンス](https://developer.android.com/reference/android/support/v7/widget/CardView.html)」を参照してください。

## <a name="related-links"></a>関連リンク

- [RecyclerView (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-recyclerviewer)
- [ロリポップの概要](~/android/platform/lollipop.md)
- [CardView クラスの参照](https://developer.android.com/reference/android/support/v7/widget/CardView.html)
