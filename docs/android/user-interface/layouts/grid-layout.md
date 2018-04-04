---
title: GridLayout
ms.prod: xamarin
ms.assetid: B69A4BF5-9CFB-443A-9F7B-062D1E498F61
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 53f2ba8ff14a4338310e02244acdbfd7fa9bc13c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="gridlayout"></a>GridLayout

`GridLayout`は、新しい`ViewGroup`サブクラス次に示すように、HTML テーブルのような 2 次元のグリッド内のビューのレイアウトをサポートします。

 [![次の 4 つのセルを表示する GridLayout のトリミング](grid-layout-images/21-gridlayoutcropped.png)](grid-layout-images/21-gridlayoutcropped.png#lightbox)

 `GridLayout` ここで子ビューの場所のグリッドで設定の行と列である必要がありますを指定することでフラット ビュー階層で動作します。 このように、 *GridLayout*レイアウトで使用されるテーブルの行に見られるよう、中級者向けのすべてのビューが、テーブル構造を提供することを必要とせず、グリッド内のビューを配置することができます。 フラットな階層を維持することにより*GridLayout*レイアウトより迅速には、子ビュー。 この概念実際に意味のコードに示すために例を見てをみましょう。


## <a name="creating-a-grid-layout"></a>グリッド レイアウトの作成

次の XML にいくつか追加`TextView`コントロールを*GridLayout*です。

```xml
<?xml version="1.0" encoding="utf-8"?>
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"    
        android:rowCount="2"
        android:columnCount="2">
     <TextView
            android:text="Cell 0"
            android:textSize="14dip" />
     <TextView
            android:text="Cell 1"
            android:textSize="14dip" />
     <TextView
            android:text="Cell 2"
            android:textSize="14dip" />
     <TextView
            android:text="Cell 3"
            android:textSize="14dip" />
</GridLayout>
```

レイアウトは調整行と列のサイズ、セルがそのコンテンツに収まるように次の図に示すように。

 [![左側の 2 つのセルを右によりも小さいを表示するレイアウトの図](grid-layout-images/gridlayout-cells.png)](grid-layout-images/gridlayout-cells.png#lightbox)

これは、結果、アプリケーション内で実行時に、次のユーザー インターフェイスになります。

 [![次の 4 つのセルを表示するスクリーン ショットの GridLayoutDemo アプリ](grid-layout-images/01-gridlayout.png)](grid-layout-images/01-gridlayout.png#lightbox)



## <a name="specifying-orientation"></a>印刷の向きを指定します。

それぞれ、上記の XML で注目`TextView`行または列が指定されていません。 これらが指定されていないときに、`GridLayout`方向に基づく順序で各子ビューを割り当てます。 たとえば、水平、次のように垂直方向には、既定値からみましょう GridLayout の向きを変更します。

```xml
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"    
        android:rowCount="2"
        android:columnCount="2"
        android:orientation="vertical">
</GridLayout>
```

ここで、`GridLayout`次に示すように左から右、代わりに、各列の下に上からセルを配置します。

 [![垂直方向のセルを配置する方法を示すダイアグラム](grid-layout-images/gridlayoutorientation.png)](grid-layout-images/gridlayoutorientation.png#lightbox)

これは、結果は、実行時に次のユーザー インターフェイスになります。

 [![垂直方向に配置されているセルを持つ GridLayoutDemo のスクリーン ショット](grid-layout-images/02-gridlayout.png)](grid-layout-images/02-gridlayout.png#lightbox)



### <a name="specifying-explicit-position"></a>明示的な位置を指定します。

内の子ビューの位置を明示的に制御するかどうか、 `GridLayout`、設定することがその`layout_row`と`layout_column`属性。 たとえば、次の XML は、レイアウトの方向に関係なく (上記)、最初のスクリーン ショットになります。

```xml
<?xml version="1.0" encoding="utf-8"?>
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"    
        android:rowCount="2"
        android:columnCount="2">
     <TextView
            android:text="Cell 0"
            android:textSize="14dip"
            android:layout_row="0"
            android:layout_column="0" />
     <TextView
            android:text="Cell 1"
            android:textSize="14dip"
            android:layout_row="0"
            android:layout_column="1" />
     <TextView
            android:text="Cell 2"
            android:textSize="14dip"
            android:layout_row="1"
            android:layout_column="0" />
     <TextView
            android:text="Cell 3"
            android:textSize="14dip"
            android:layout_row="1"
            android:layout_column="1"  />
</GridLayout>
```



### <a name="specifying-spacing"></a>間隔を指定します。

いくつかの子間の間隔のビューを提供するオプションがある、`GridLayout`です。 使用して、`layout_margin`次に示すように各子ビューに余白を直接設定する属性

```xml
<TextView
            android:text="Cell 0"
            android:textSize="14dip"
            android:layout_row="0"
            android:layout_column="0"
            android:layout_margin="10dp" />
```

さらに、Android 4 で、新しい汎用間隔ビューと呼ばれる`Space`が利用できるようになりました。 これを使用するには、だけでとして追加子ビュー。
次の XML がする追加の行を追加するなど、`GridLayout`を設定してその`rowcount`3 にし、追加、`Space`間のスペースを提供するビュー、`TextViews`です。

```xml
<?xml version="1.0" encoding="utf-8"?>
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"    
        android:rowCount="3"
        android:columnCount="2"
        android:orientation="vertical">
     <TextView
            android:text="Cell 0"
            android:textSize="14dip"
            android:layout_row="0"
            android:layout_column="0" />
     <TextView
            android:text="Cell 1"
            android:textSize="14dip"
            android:layout_row="0"        
            android:layout_column="1" />
     <Space
            android:layout_row="1"
            android:layout_column="0"
            android:layout_width="50dp"         
            android:layout_height="50dp" />    
     <TextView
            android:text="Cell 2"
            android:textSize="14dip"
            android:layout_row="2"        
            android:layout_column="0" />
     <TextView
            android:text="Cell 3"
            android:textSize="14dip"
            android:layout_row="2"        
            android:layout_column="1" />
</GridLayout>
```

この XML で表される間隔を作成する、`GridLayout`次のようにします。

 [![間隔を持つ大規模なセルを示す GridLayoutDemo のスクリーン ショット](grid-layout-images/03-gridlayout.png)](grid-layout-images/03-gridlayout.png#lightbox)

新しいを使用する利点`Space`ビューは、その間隔を必要し、しないすべての子ビューに属性を設定することです。



### <a name="spanning-columns-and-rows"></a>列と行のまたがりメモリ割り当てください。

`GridLayout`も複数の列と行にわたるセルをサポートしています。 たとえば、ボタンを含む別の行を追加、`GridLayout`次のようにします。

```xml
<?xml version="1.0" encoding="utf-8"?>
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"    
        android:rowCount="4"
        android:columnCount="2"
        android:orientation="vertical">
     <TextView
            android:text="Cell 0"
            android:textSize="14dip"
            android:layout_row="0"
            android:layout_column="0" />
     <TextView
            android:text="Cell 1"
            android:textSize="14dip"
            android:layout_row="0"        
            android:layout_column="1" />
     <Space
            android:layout_row="1"
            android:layout_column="0"
            android:layout_width="50dp"        
            android:layout_height="50dp" />   
     <TextView
            android:text="Cell 2"
            android:textSize="14dip"
            android:layout_row="2"        
            android:layout_column="0" />
     <TextView
            android:text="Cell 3"
            android:textSize="14dip"        
            android:layout_row="2"        
            android:layout_column="1" />
     <Button
            android:id="@+id/myButton"
            android:text="@string/hello"        
            android:layout_row="3"
            android:layout_column="0" />
</GridLayout>
```

最初の列と、これが、`GridLayout`ここでわかるように、ボタンのサイズに合わせて伸縮しません。

[![最初の列のみのまたがりメモリ割り当てのボタンで GridLayoutDemo のスクリーン ショット](grid-layout-images/04-gridlayout.png)](grid-layout-images/04-gridlayout.png#lightbox)

最初の列が、拡大をするを防止するには、ことができますにするには次のように、そのうち 2 つの列をまたがる ボタンを設定します。

```xml
<Button
    android:id="@+id/myButton"
    android:text="@string/hello"       
    android:layout_row="3"
    android:layout_column="0"
    android:layout_columnSpan="2" />
```

結果のレイアウトにこれを行う、`TextViews`が発生しました。 以前の下部に追加のボタンでレイアウトに似た、`GridLayout`次のようにします。

 [![両方の列のまたがりメモリ割り当てのボタンで GridLayoutDemo のスクリーン ショット](grid-layout-images/05-gridlayout.png)](grid-layout-images/05-gridlayout.png#lightbox)


## <a name="related-links"></a>関連リンク

- [GridLayoutDemo (sample)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/ICS_Samples/GridLayoutDemo/)
- [アイスクリーム サンドイッチの概要](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 プラットフォーム](http://developer.android.com/sdk/android-4.0.html)
