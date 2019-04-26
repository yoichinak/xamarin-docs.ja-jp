---
title: GridLayout
ms.prod: xamarin
ms.assetid: B69A4BF5-9CFB-443A-9F7B-062D1E498F61
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 45e625b28bbdf0009b5cfcf661b00ce17638771d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61311254"
---
# <a name="gridlayout"></a>GridLayout

`GridLayout`は、新しい`ViewGroup`次に示すように、HTML テーブルと同様に、2 次元グリッド ビューのレイアウトをサポートするサブクラスです。

 [![4 つのセルを表示する GridLayout をトリミングします。](grid-layout-images/21-gridlayoutcropped.png)](grid-layout-images/21-gridlayoutcropped.png#lightbox)

 `GridLayout` 子ビューで設定される場所の場所、グリッド行と列にする必要がありますを指定することでフラット ビューの階層で動作します。 これにより、 *GridLayout* TableLayout で使用されるテーブルの行に見られるよう、すべての中間のビューがテーブルの構造を提供することを必要とせず、グリッド内のビューを配置することができます。 フラットな階層では、維持することによって*GridLayout*よりすばやくレイアウトがその子ビュー。 例を示し、この概念実際に意味のコードを見ていきましょう。


## <a name="creating-a-grid-layout"></a>グリッド レイアウトの作成

次の XML がいくつか追加します`TextView`にコントロールを*GridLayout*します。

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

レイアウトに次の図に示すようには、セルが、そのコンテンツに適合するように、行と列のサイズが調整されます。

 [![右側の小さいよりも左側の 2 つのセルを表示するレイアウトの図](grid-layout-images/gridlayout-cells.png)](grid-layout-images/gridlayout-cells.png#lightbox)

これは、アプリケーションの実行時に、次のユーザー インターフェイスが得られます。

 [![4 つのセルを表示するスクリーン ショットの GridLayoutDemo アプリ](grid-layout-images/01-gridlayout.png)](grid-layout-images/01-gridlayout.png#lightbox)



## <a name="specifying-orientation"></a>印刷の向きを指定します。

それぞれ、上記の XML で注目`TextView`行または列を指定しません。 これらは指定されていない場合、`GridLayout`向きに基づく順序で各子ビューを割り当てます。 たとえば、水平方向、次のように垂直に既定値からみましょう GridLayout の向きを変更します。

```xml
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"    
        android:rowCount="2"
        android:columnCount="2"
        android:orientation="vertical">
</GridLayout>
```

ここで、`GridLayout`次に示すように上から下、左の代わりに、各列にセルを配置します。

 [![垂直方向のセルの配置方法を示す図](grid-layout-images/gridlayoutorientation.png)](grid-layout-images/gridlayoutorientation.png#lightbox)

これは、実行時に、次のユーザー インターフェイスが得られます。

 [![セルが垂直方向の配置で GridLayoutDemo のスクリーン ショット](grid-layout-images/02-gridlayout.png)](grid-layout-images/02-gridlayout.png#lightbox)



### <a name="specifying-explicit-position"></a>明示的な位置を指定します。

内の子ビューの位置を明示的に制御するかどうか、 `GridLayout`、設定することがその`layout_row`と`layout_column`属性。 たとえば、次の XML の向きに関係なく (上図を参照)、最初のスクリーン ショットをレイアウトが発生します。

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

いくつかの子間の間隔のビューを提供するオプションがある、`GridLayout`します。 使用して、`layout_margin`次に示すように各子ビューで、余白を直接設定する属性

```xml
<TextView
            android:text="Cell 0"
            android:textSize="14dip"
            android:layout_row="0"
            android:layout_column="0"
            android:layout_margin="10dp" />
```

さらに、Android 4 は、新しい汎用的な間隔ビューという`Space`られるようになりました。 これを使用するには、として追加するだけ、子ビュー。
たとえば、次の XML に追加の行を追加します。、`GridLayout`を設定してその`rowcount`3 にし、追加、`Space`間の間隔を示すビュー、`TextViews`します。

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

この XML の作成の間隔、`GridLayout`次に示すよう。

 [![大きなセルの間隔を示す GridLayoutDemo のスクリーン ショット](grid-layout-images/03-gridlayout.png)](grid-layout-images/03-gridlayout.png#lightbox)

新しいを使用する利点`Space`ビューの間隔し、すべての子ビューで属性を設定することを必要としないことができます。



### <a name="spanning-columns-and-rows"></a>列と行のまたがりメモリ割り当てください。

`GridLayout`も複数の列と行にわたるセルをサポートしています。 たとえば、ボタンを格納している別の行を追加、`GridLayout`次に示すよう。

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

最初の列と、これが、`GridLayout`ここでわかるとおり、ボタンのサイズに合わせて伸縮しません。

[![最初の列のみのまたがりメモリ割り当て ボタンのスクリーン ショットの GridLayoutDemo](grid-layout-images/04-gridlayout.png)](grid-layout-images/04-gridlayout.png#lightbox)

ストレッチから最初の列を保持するには、その columnspan ようを設定して 2 つの列にまたがって表示するボタンを設定することができます。

```xml
<Button
    android:id="@+id/myButton"
    android:text="@string/hello"       
    android:layout_row="3"
    android:layout_column="0"
    android:layout_columnSpan="2" />
```

結果のレイアウト、これを行う、`TextViews`の下部に追加するボタンにあった以前では、レイアウトに似ています、`GridLayout`次に示すよう。

 [![ボタンの両方の列にまたがる GridLayoutDemo のスクリーン ショット](grid-layout-images/05-gridlayout.png)](grid-layout-images/05-gridlayout.png#lightbox)


## <a name="related-links"></a>関連リンク

- [GridLayoutDemo (サンプル)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/ICS_Samples/GridLayoutDemo/)
- [Ice Cream Sandwich の概要](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 プラットフォーム](https://developer.android.com/sdk/android-4.0.html)
