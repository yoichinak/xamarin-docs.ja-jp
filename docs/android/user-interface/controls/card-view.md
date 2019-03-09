---
title: CardView
description: Cardview ウィジェットは、カードのようにビューのテキストとイメージのコンテンツを表示する UI コンポーネントです。 このガイドを使用して、Xamarin.Android アプリケーションで以前のバージョンの Android との下位互換性を維持しながら CardView をカスタマイズする方法について説明します。
ms.prod: xamarin
ms.assetid: CF12FE85-D03A-4E64-95D2-D7115061A500
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: cdb75207bff3f15a54d0cdd90fa0833da9c145e6
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57670650"
---
# <a name="cardview"></a>CardView

_Cardview ウィジェットは、カードのようにビューのテキストとイメージのコンテンツを表示する UI コンポーネントです。このガイドを使用して、Xamarin.Android アプリケーションで以前のバージョンの Android との下位互換性を維持しながら CardView をカスタマイズする方法について説明します。_


## <a name="overview"></a>概要

`Cardview` Android 5.0 (Lollipop) で導入されたウィジェットはカードのようにビューのテキストとイメージのコンテンツを表示する UI コンポーネントです。 `CardView` として実装されます、`FrameLayout`の角が丸いと影のウィジェット。 通常、`CardView`内の 1 つの行項目を表示するために、`ListView`または`GridView`ビュー グループ化します。 たとえば、次のスクリーン ショットにを実装する旅行予約アプリの例`CardView`-ベースの移動先のカードで、スクロール可能な`ListView`:

![各旅行先、CardView を使用してアプリの例](card-view-images/01-cardview-example.png)

このガイドを追加する方法について説明、`CardView`パッケージを Xamarin.Android プロジェクトを追加する方法`CardView`の外観をカスタマイズする方法と、レイアウトに`CardView`アプリでします。 このガイドの詳細な一覧は、さらに、`CardView`属性を使用するための属性を含む、変更できる`CardView`Android 5.0 Lollipop より前のバージョンの Android です。

<a name="requirements" />

## <a name="requirements"></a>必要条件

新しい Android 5.0 およびそれ以降の機能を使用する、次が必要 (を含む`CardView`) Xamarin ベースのアプリで。

-  **Xamarin.Android** &ndash; Xamarin.Android 4.20 or later must be installed and configured with either Visual Studio or Visual Studio for Mac.

-  **Android SDK** &ndash; Android 5.0 (API 21) 以降、Android SDK Manager を使用してをインストールする必要があります。

-  **Java JDK 1.8** &ndash;ターゲット API レベル具体的には 23 およびそれ以前の場合、JDK 1.7 を使用できます。 JDK 1.8 は[Oracle](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)します。

アプリも含める必要があります、`Xamarin.Android.Support.v7.CardView`パッケージ。 追加する、 `Xamarin.Android.Support.v7.CardView` Visual Studio for Mac でのパッケージ。

1. プロジェクトを開き、右クリック**パッケージ**選択**パッケージを追加する**します。

2. **パッケージを追加する**ダイアログで、検索**CardView**します。

3. 選択**Xamarin サポート ライブラリ v7 CardView**、 をクリックし、**パッケージの追加**します。

追加する、 `Xamarin.Android.Support.v7.CardView` Visual Studio でパッケージ。

1. プロジェクトを開き、右クリックし、**参照**ノード (で、**ソリューション エクスプ ローラー**ウィンドウ) を選択して**NuGet パッケージの管理.**.

2. ときに、 **NuGet パッケージの管理**ダイアログが表示されたら、入力**CardView**検索ボックスにします。

3. ときに**Xamarin サポート ライブラリ v7 CardView**が表示されたら、クリックして**インストール**します。

Android 5.0 アプリのプロジェクトを構成する方法についてを参照してください。[設定を、Android 5.0 プロジェクト](~/android/platform/lollipop.md)します。
NuGet パッケージのインストールの詳細については、次を参照してください。[チュートリアル。NuGet をプロジェクトに含める](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)します。


## <a name="introducing-cardview"></a>CardView の概要

既定の`CardView`白のカードの角が丸い最小限と薄い影に似ています。 次の例では、 **Main.axml**レイアウトは、1 つを表示します`CardView`ウィジェットを含む、 `TextView`:。

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

既存の内容を置き換えるには、この XML を使用するかどうか**Main.axml**、内の任意のコードをコメント アウトを必ず**MainActivity.cs**前の xml リソースを参照します。

このレイアウトの例は、既定値を作成します。 `CardView` 1 行のテキストの次のスクリーン ショットに示すようにします。

[![白の背景とテキストの行で CardView のスクリーン ショット](card-view-images/02-basic-cardview-sml.png)](card-view-images/02-basic-cardview.png#lightbox)

ライト テーマのマテリアルに設定されているアプリのスタイルでは、この例では、(`Theme.Material.Light`) ように、 `CardView` shadows とエッジが見やすくなります。 Android 5.0 のテーマのアプリの詳細については、次を参照してください。[マテリアル テーマ](~/android/user-interface/material-theme.md)します。 次のセクションでカスタマイズする方法について説明します`CardView`アプリケーション。


## <a name="customizing-cardview"></a>CardView をカスタマイズします。

基本を変更する`CardView`の外観をカスタマイズする属性、`CardView`アプリ。 昇格など、`CardView`増やす (これは、float、背景の上に高いと思われるカードをにより) より大きなシャドウをキャストすることができます。 また、丸め、カードの詳細の角にする角の半径を増やすことができます。

[次へ] レイアウトの例で、カスタマイズされた`CardView`プリント写真 (「スナップショット」) のシミュレーションを作成するために使用します。 `ImageView`に追加されます、 `CardView` 、イメージを表示するため、`TextView`の下にある、`ImageView`画像のタイトルを表示するためです。
このレイアウトの例で、`CardView`が次のカスタマイズ。

-  `cardElevation`が大きいシャドウをキャストする 4dp に増加しました。
-  `cardCornerRadius` 5dp 角の丸みのある複数の表示にするには増加します。

`CardView`で Android v7 サポート ライブラリでは、その属性はできません。 から提供されるのは、`android:`名前空間。 したがって、独自の XML 名前空間を定義し、としてその名前空間を使用する必要があります、`CardView`属性のプレフィックス。 次のレイアウトの例では行を使用してこのという名前空間を定義する`cardview`:

```xml
    xmlns:cardview="http://schemas.android.com/apk/res-auto"
```

この名前空間を呼び出すことができます`card_view`またはでも`myapp`を選択した場合 (このファイルのスコープ内でのみアクセス可能です)。 この名前空間を呼び出すものを選択すると、プレフィックスを使用する必要があります、`CardView`を変更する属性。 このレイアウト例では、`cardview`名前空間のプレフィックスは、`cardElevation`と`cardCornerRadius`:

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

このレイアウトの例を使用して写真表示アプリケーションでは、イメージを表示する場合、`CardView`写真のスナップショットの外観を次のスクリーン ショットに示すようには。

[![イメージとイメージの下のキャプションを CardView](card-view-images/03-photo-cardview-sml.png)](card-view-images/03-photo-cardview.png#lightbox)

このスクリーン ショットは撮影、 [RecyclerViewer](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer)を使用するサンプル アプリを`RecyclerView`のスクロール リストに表示するウィジェット`CardView`写真を表示するためのイメージ。 詳細については`RecyclerView`を参照してください、 [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)ガイド。

なお、`CardView`のコンテンツ領域には、複数の子ビューを表示できます。 たとえば、アプリの例を表示する上記の写真でコンテンツ領域で構成、`ListView`を格納している、`ImageView`と`TextView`。 `CardView`インスタンスが垂直方向に配置された多くの場合、水平方向に配置しても (を参照してください[カスタム ビューのスタイルを作成する](~/android/user-interface/material-theme.md#customview)スクリーン ショットの例の)。


### <a name="cardview-layout-options"></a>CardView レイアウト オプション

`CardView` そのパディング、昇格、角の半径、および背景色に影響を与える 1 つまたは複数の属性を設定して、レイアウトをカスタマイズできます。

[![CardView 属性のダイアグラム](card-view-images/04-attributes-sml.png)](card-view-images/04-attributes.png#lightbox)

各属性が対応を呼び出すことによって動的に変更することもできます`CardView`メソッド (の詳細については`CardView`メソッドを参照してください、 [CardView クラス参照](https://developer.android.com/reference/android/support/v7/widget/CardView.html))。
(背景色) を除くこれらの属性が単位に続く 10 進数では、ディメンションの値を受け入れることに注意してください。 たとえば、 `11.5dp` 11.5 密度に依存しないピクセルを指定します。


#### <a name="padding"></a>[間隔]
`
CardView` カード内のコンテンツを配置する 5 つの埋め込み属性を提供します。 XML のレイアウトに設定することができますか、コード内と同様のメソッドを呼び出すことができます。

[![埋め込み属性 CardView のダイアグラム](card-view-images/05-padding-sml.png)](card-view-images/05-padding.png#lightbox)

パディング属性は以下のとおりです。

-  `contentPadding` &ndash; 内側の子ビューの間の余白、`CardView`とカードのすべての端とします。

-  `contentPaddingBottom` &ndash; 内側の子ビューの間の余白、`CardView`とカードの下端。

-  `contentPaddingLeft` &ndash; 内側の子ビューの間の余白、`CardView`とカードの左端とします。

-  `contentPaddingRight` &ndash; 内側の子ビューの間の余白、`CardView`とカードの右端とします。

-  `contentPaddingTop` &ndash; 内側の子ビューの間の余白、`CardView`とカードの上端とします。

コンテンツの埋め込み属性では、特定のコンテンツ領域内にあるウィジェットではなく、コンテンツ エリアの境界を基準としました。
たとえば場合、`contentPadding`写真表示アプリで、十分に増加した、`CardView`イメージと、カード上に表示されるテキストの両方をトリミングするとします。



#### <a name="elevation"></a>昇格されます。

`CardView` その昇格を制御する 2 つの昇格属性を提供し、その結果、その影のサイズ。

[![CardView 昇格属性のダイアグラム](card-view-images/06-elevation-sml.png)](card-view-images/06-elevation.png#lightbox)

昇格属性は以下のとおりです。

-  `cardElevation` &ndash; 昇格、 `CardView` (Z 軸を表します)。

-  `cardMaxElevation` &ndash; 最大値、`CardView`の昇格。

値を大きく`cardElevation`する影のサイズを増やす`CardView`float、背景の上に高いようです。 `cardElevation`属性では、ビューの重複の描画順序も決定します。 つまり、`CardView`高レベルの昇格時の設定と、昇格の低い設定で重複する、ビュー上の別の重複するビューの下に描画されます。
`cardMaxElevation`設定が、アプリが変更されたときの昇格に動的に役立ちます&ndash;この設定で定義した制限を超えて拡張の影を阻止します。


#### <a name="corner-radius-and-background-color"></a>角の半径と背景色

`CardView` 角を丸める半径と背景色を制御するために使用できる属性を提供します。 これら 2 つのプロパティの全体的なスタイルの変更を許可する、 `CardView`:

[![CardView 上隅にある交信と背景色の属性のダイアグラム](card-view-images/07-radius-bgcolor-sml.png)](card-view-images/07-radius-bgcolor.png#lightbox)

これらの属性は以下のとおりです。

-  `cardCornerRadius` &ndash; すべての角の角の半径、`CardView`します。

-  `cardBackgroundColor` &ndash; 背景色、`CardView`します。

この図で`cardCornerRadius`より角の丸い 10dp に設定されていると`cardBackgroundColor`に設定されている`"#FFFFCC"`(薄い黄色)。


## <a name="compatibility"></a>互換性

使用することができます`CardView`Android 5.0 Lollipop より前のバージョンの Android です。 `CardView`部分は、使用できる Android v7 サポート ライブラリでは、 `CardView` Android 2.1 (API レベル 7) 以降。
ただし、インストールする必要があります、 `Xamarin.Android.Support.v7.CardView` 」の説明に従ってパッケージ[要件](#requirements)上、します。

`CardView` Lollipop (API レベル 21) の前にデバイスで若干異なる動作します。

-  `CardView` 追加の埋め込みを追加するプログラムによるシャドウの実装を使用します。

-  `CardView` 交差する子ビューはクリップされません、`CardView`丸きます。

これらの互換性の相違点の管理に役立つ`CardView`レイアウトでは、構成可能ないくつかの追加属性を提供します。

-   `cardPreventCornerOverlap` &ndash; この属性を設定して`true`以前の Android バージョン (API レベル 20 およびそれ以前) で、アプリが実行されているときに余白を追加します。 この設定によって禁止`CardView`と重なり合うからコンテンツを`CardView`丸きます。

-   `cardUseCompatPadding` &ndash; この属性を設定して`true`のバージョンの API レベル 21 以上に Android アプリの実行時に、パディングを追加します。 使用する場合`CardView`前ロリポップ デバイスでの見た目は同じ Lollipop で (またはそれ以降)、この属性を設定すること`true`します。 この属性を有効にすると、`CardView`前ロリポップ デバイスで実行時に影を描画するために追加の埋め込みを追加します。 これにより、事前 Lollipop のプログラムによるシャドウの実装が有効なときに導入されている埋め込みの相違を克服するためにします。

Android の旧バージョンとの互換性を維持する詳細については、次を参照してください。[互換性の維持](https://developer.android.com/training/material/compatibility.html)します。


## <a name="summary"></a>まとめ

このガイドに導入された新しい`CardView`ウィジェットは Android 5.0 (Lollipop) に含まれています。 既定値を示す`CardView`外観をカスタマイズする方法を説明および`CardView`の昇格、角の丸みを変更すると、コンテンツの埋め込み、および背景色。 一覧に、`CardView`レイアウト属性 (で図) を参照して、使用する方法について説明し、 `CardView` Android デバイスを Android 5.0 Lollipop より前で。 詳細については`CardView`を参照してください、 [CardView クラス参照](https://developer.android.com/reference/android/support/v7/widget/CardView.html)します。


## <a name="related-links"></a>関連リンク

- [RecyclerView (サンプル)](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer)
- [ロリポップの概要](~/android/platform/lollipop.md)
- [CardView クラスのリファレンス](https://developer.android.com/reference/android/support/v7/widget/CardView.html)
