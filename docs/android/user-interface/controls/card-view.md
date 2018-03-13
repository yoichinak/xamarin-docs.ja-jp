---
title: CardView
description: "Cardview ウィジェットは、カードのようになりますをビュー内のテキストとイメージのコンテンツを表示する UI コンポーネントです。 このガイドを使用する Android の以前のバージョンとの下位互換性を維持しながら Xamarin.Android アプリケーションで CardView をカスタマイズする方法について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: CF12FE85-D03A-4E64-95D2-D7115061A500
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 46eec10bbabec74719affabce1e8033a083680be
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="cardview"></a>CardView

_Cardview ウィジェットは、カードのようになりますをビュー内のテキストとイメージのコンテンツを表示する UI コンポーネントです。このガイドを使用する Android の以前のバージョンとの下位互換性を維持しながら Xamarin.Android アプリケーションで CardView をカスタマイズする方法について説明します。_


## <a name="overview"></a>概要

`Cardview` Android 5.0 (ロリポップ) で導入された、ウィジェットがカードのようになりますをビュー内のテキストとイメージのコンテンツを表示する UI コンポーネントです。 `CardView` として実装された、`FrameLayout`角の丸いと影ウィジェット。 通常、`CardView`内の 1 つの行項目を表示するために、`ListView`または`GridView`ビュー グループ化します。 たとえば、スクリーン ショットを次の例に示します旅行の予約アプリケーションを実装する`CardView`-ベースで、スクロール可能な旅行先カード`ListView`:

![各旅行先、CardView を使用する例のアプリ](card-view-images/01-cardview-example.png)

このガイドを追加する方法について説明、`CardView`パッケージを Xamarin.Android プロジェクトを追加する方法`CardView`の外観をカスタマイズする方法と、レイアウトに`CardView`アプリでします。 このガイドの詳細な一覧は、さらに、`CardView`変更できますが、使用するための属性を含む属性`CardView`Android 5.0 ロリポップより前のバージョンの Android でします。

<a name="requirements" />

## <a name="requirements"></a>必要条件

新しい Android 5.0 とそれ以降の機能を使用する、次が必要 (含む`CardView`) Xamarin ベースのアプリで。

-  **Xamarin.Android** &ndash; Xamarin.Android 4.20 or later must be installed and configured with either Visual Studio or Visual Studio for Mac.

-  **Android SDK** &ndash; Android 5.0 (API 21) 以降、Android SDK Manager を使用してをインストールする必要があります。

-  **Java JDK 1.8** &ndash; JDK 1.7 は、ターゲット API レベル具体的には 23 およびそれ以前の場合、使用できます。 JDK 1.8 はから利用可能な[Oracle](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)です。

アプリも含める必要があります、`Xamarin.Android.Support.v7.CardView`パッケージです。 追加する、 `Xamarin.Android.Support.v7.CardView` Mac 用の Visual Studio でパッケージ。

1. プロジェクトを開きを右クリックして**パッケージ**選択**パッケージを追加する**です。

2. **パッケージを追加する**ダイアログで、検索**CardView**です。

3. 選択**Xamarin サポート ライブラリ v7 CardView**をクリックし、**パッケージの追加**です。

追加する、 `Xamarin.Android.Support.v7.CardView` Visual Studio でパッケージ。

1. プロジェクトを開きを右クリックし、**参照**ノード (で、**ソリューション エクスプ ローラー**ウィンドウ) を選択して**NuGet パッケージの管理.**.

2. ときに、 **NuGet パッケージの管理**ダイアログが表示されたら、入力**CardView**検索ボックスにします。

3. ときに**Xamarin サポート ライブラリ v7 CardView**が表示されたら、をクリックして**インストール**です。

Android 5.0 アプリ プロジェクトを構成する方法については、次を参照してください。[設定を Android 5.0 プロジェクト](~/android/platform/lollipop.md)です。
NuGet パッケージのインストールの詳細については、次を参照してください。[チュートリアル: プロジェクトで、NuGet を含む](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)です。


## <a name="introducing-cardview"></a>CardView の概要

既定値`CardView`角の丸い最小限と薄い影白いカードのようになります。 次の例**Main.axml**レイアウトは、1 つを表示`CardView`ウィジェットを含む、 `TextView`:

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

既存の内容を置き換えるには、この XML を使用するかどうか**Main.axml**、内の任意のコードをコメント アウトすることを確認する**MainActivity.cs**前の XML 内のリソースを参照します。

このレイアウトの例は、既定値を作成`CardView`のテキストの次のスクリーン ショットに示すように 1 つの行にします。

[![白い背景色とテキストの行 CardView のスクリーン ショット](card-view-images/02-basic-cardview-sml.png)](card-view-images/02-basic-cardview.png#lightbox)

ライト マテリアル テーマに設定されているアプリのスタイルでこの例では、(`Theme.Material.Light`) できるように、`CardView`シャドウとエッジがわかりやすくします。 テーマ Android 5.0 アプリに関する詳細については、次を参照してください。[マテリアル テーマ](~/android/user-interface/material-theme.md)です。 次のセクションで学習をカスタマイズする方法`CardView`アプリケーションです。


## <a name="customizing-cardview"></a>CardView をカスタマイズします。

基本的なを変更することができます`CardView`の外観をカスタマイズする属性、`CardView`アプリ。 たとえばの昇格の`CardView`(float 型の高い、背景の上にないように見える場合、カードは、) をより大きなシャドウをキャストを増やすことができます。 また、角の半径を丸め、カードの詳細の角を強化できます。

次のレイアウトの例で、カスタマイズされた`CardView`プリント写真 (「スナップショット」) のシミュレーションを作成するために使用します。 `ImageView`に追加、 `CardView` 、イメージを表示するため、`TextView`の下にある、`ImageView`イメージのタイトルを表示するためです。
このレイアウトの例で、`CardView`が次のカスタマイズ。

-  `cardElevation`が大きいシャドウをキャストする 4dp に増加しました。
-  `cardCornerRadius` 5dp 表示より角の丸い角を作成するには増加します。

`CardView`で Android v7 サポート ライブラリでは、その属性はできません。 から提供されるのは、`android:`名前空間。 したがって、独自の XML 名前空間を定義し、としてその名前空間を使用する必要があります、`CardView`属性のプレフィックス。 次のレイアウトの例では行を使用するこのと呼ばれる名前空間を定義`cardview`:

```xml
    xmlns:cardview="http://schemas.android.com/apk/res-auto"
```

この名前空間を呼び出すことができます`card_view`またはでも`myapp`を選択した場合 (このファイルのスコープ内でのみアクセス可能です)。 この名前空間の呼び出しを選択したあらゆる項目、プレフィックスを使用する必要があります、`CardView`を変更する属性。 このレイアウト例では、`cardview`名前空間のプレフィックスは、`cardElevation`と`cardCornerRadius`:

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

このレイアウトの例を使用して写真表示アプリケーションでは、イメージを表示する場合、`CardView`フォト スナップショットの外観を次のスクリーン ショットに示すようには。

[![イメージとイメージの下のキャプションがある CardView](card-view-images/03-photo-cardview-sml.png)](card-view-images/03-photo-cardview.png#lightbox)

取得されてこのスクリーン ショット、 [RecyclerViewer](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer)サンプル アプリを使用して、`RecyclerView`のスクロール リストに表示するウィジェット`CardView`写真を表示するためのイメージです。 詳細については`RecyclerView`を参照してください、 [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)ガイドです。

注意して、`CardView`そのコンテンツ領域に 1 つ以上の子ビューを表示できます。 たとえば、アプリの例を表示する上記の写真のコンテンツ領域で構成されます、`ListView`を格納している、`ImageView`と`TextView`です。 `CardView`インスタンスが垂直方向に配置された多くの場合、水平方向に配置しても (を参照してください[カスタム ビューのスタイルを作成する](~/android/user-interface/material-theme.md#customview)サンプルのスクリーン ショットの)。


### <a name="cardview-layout-options"></a>CardView レイアウト オプション

`CardView` そのパディング、昇格、角の半径、および背景色に影響を与える 1 つまたは複数の属性を設定して、レイアウトをカスタマイズできます。

[![CardView 属性のダイアグラム](card-view-images/04-attributes-sml.png)](card-view-images/04-attributes.png#lightbox)

各属性も動的に変化を呼び出して、対応する`CardView`メソッド (詳細について`CardView`メソッドを参照してください、 [CardView クラス参照](https://developer.android.com/reference/android/support/v7/widget/CardView.html))。
これらの属性 (を除く背景色) が、ユニット続く 10 進数であるディメンションの値を受け入れることに注意してください。 たとえば、 `11.5dp` 11.5 密度非依存のピクセルを指定します。


#### <a name="padding"></a>[間隔]
`
CardView` カード内のコンテンツを配置する 5 つのパディング属性を提供します。 設定できるは、レイアウトの XML でまたはコードで類似のメソッドを呼び出すことができます。

[![埋め込み属性 CardView のダイアグラム](card-view-images/05-padding-sml.png)](card-view-images/05-padding.png#lightbox)

パディング属性は以下のとおりです。

-  `contentPadding` &ndash; 内側の子ビューの間の余白、`CardView`カードのすべての端とします。

-  `contentPaddingBottom` &ndash; 内側の子ビューの間の余白、`CardView`とカードの下端。

-  `contentPaddingLeft` &ndash; 内側の子ビューの間の余白、`CardView`とカードの左端とします。

-  `contentPaddingRight` &ndash; 内側の子ビューの間の余白、`CardView`とカードの右端とします。

-  `contentPaddingTop` &ndash; 内側の子ビューの間の余白、`CardView`とカードの上端とします。

コンテンツ領域内にある任意の特定のウィジェットではなくコンテンツ エリアの境界はコンテンツの余白の属性です。
たとえば場合、`contentPadding`写真表示するアプリで十分に増加した、`CardView`イメージと、カード上に表示されるテキストの両方がトリミングします。



#### <a name="elevation"></a>昇格されます。

`CardView` その昇格を制御する 2 つの昇格属性を提供し、その結果、その影のサイズ。

[![CardView が昇格される属性のダイアグラム](card-view-images/06-elevation-sml.png)](card-view-images/06-elevation.png#lightbox)

昇格の属性は以下のとおりです。

-  `cardElevation` &ndash; 昇格、 `CardView` (の Z 軸を表します)。

-  `cardMaxElevation` &ndash; 最大値、`CardView`の昇格します。

値を大きく`cardElevation`サイズの再構成にシャドウ`CardView`float、背景の上に高いいないと思われます。 `cardElevation`属性は重複するビューの描画順序も決まります。 つまり、`CardView`以降では、高レベルの昇格の設定の昇格の低い設定で、重複するビューの他の重複するビューの下に描画されます。
`cardMaxElevation`設定は、アプリが変更されたときの昇格に動的に適して&ndash;この設定で定義した制限を超えて拡張の影を阻止します。


#### <a name="corner-radius-and-background-color"></a>角の半径と背景色

`CardView` その角の半径と背景色を制御するのに使用できる属性を提供します。 これら 2 つのプロパティの全体的なスタイルの変更を許可する、 `CardView`:

[![交信 CardView コーナーと背景色の属性の図](card-view-images/07-radius-bgcolor-sml.png)](card-view-images/07-radius-bgcolor.png#lightbox)

これらの属性は以下のとおりです。

-  `cardCornerRadius` &ndash; すべてのコーナーの角の半径、`CardView`です。

-  `cardBackgroundColor` &ndash; 背景色、`CardView`です。

この図で`cardCornerRadius`より角の丸い 10dp に設定されていると`cardBackgroundColor`に設定されている`"#FFFFCC"`(薄い黄色)。


## <a name="compatibility"></a>互換性

使用することができます`CardView`Android 5.0 ロリポップより前のバージョンの Android でします。 `CardView`一部を使用できる Android v7 サポート ライブラリの`CardView`Android 2.1 以降 (API レベル 7) 以降。
ただし、インストールする必要があります、 `Xamarin.Android.Support.v7.CardView` 」の説明に従ってパッケージ[要件](#requirements)上、します。

`CardView` ロリポップ (API レベル 21) を実行する前にデバイスで若干異なる動作します。

-  `CardView` 追加の埋め込みを追加するプログラムによるシャドウの実装を使用します。

-  `CardView` 交差する子ビューはクリップされません、`CardView`角を丸くです。

これらの互換性の相違点の管理に役立つ`CardView`レイアウトで構成できるいくつかの追加属性を提供します。

-   `cardPreventCornerOverlap` &ndash; この属性を設定`true`余白を追加するアプリが以前の Android バージョン (API レベル 20 およびそれ以前) で実行されているときにします。 この設定では、`CardView`と重なり合うからコンテンツを`CardView`角を丸くです。

-   `cardUseCompatPadding` &ndash; この属性を設定`true`余白を追加するバージョンの API レベル 21 以上で Android アプリを実行するときにします。 使用する場合`CardView`前ロリポップ デバイスでをロリポップ上 (またはそれ以降) 同じように見える、この属性を設定して`true`です。 この属性を有効にすると、`CardView`前ロリポップ デバイスでの実行時に影を描画する追加の埋め込みを追加します。 これにより、有効な場合に事前ロリポップ プログラムによるシャドウの実装に導入されたパディングの相違を解決します。

詳細については、Android の旧バージョンとの互換性を保ちを参照してください。[互換性を維持する](https://developer.android.com/training/material/compatibility.html)です。


## <a name="summary"></a>まとめ

このガイドに導入された新しい`CardView`ウィジェット Android 5.0 (ロリポップ) に含まれています。 既定値が示されている`CardView`外観をカスタマイズする方法を説明および`CardView`の昇格、角の丸みを変更すると、コンテンツの余白、および背景色。 表示されること、`CardView`レイアウト属性 (図) を参照してで使用する方法を説明し、 `CardView` Android デバイスで Android 5.0 ロリポップより前にします。 詳細については`CardView`を参照してください、 [CardView クラス参照](https://developer.android.com/reference/android/support/v7/widget/CardView.html)です。


## <a name="related-links"></a>関連リンク

- [RecyclerView (サンプル)](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer)
- [ロリポップの概要](~/android/platform/lollipop.md)
- [CardView クラスのリファレンス](https://developer.android.com/reference/android/support/v7/widget/CardView.html)
