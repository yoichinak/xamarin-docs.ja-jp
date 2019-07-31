---
title: GridLayout
ms.prod: xamarin
ms.assetid: B69A4BF5-9CFB-443A-9F7B-062D1E498F61
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: a14930d52b551b35dcf5a36e2475b021e5bf27d1
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68646588"
---
# <a name="xamarinandroid-gridlayout"></a>Xamarin Android GridLayout

は`GridLayout` 、次に`ViewGroup`示すように、HTML テーブルと同様に、2d グリッドでのビューのレイアウトをサポートする新しいサブクラスです。

 [![4つのセルを表示するトリミング GridLayout](grid-layout-images/21-gridlayoutcropped.png)](grid-layout-images/21-gridlayoutcropped.png#lightbox)

 `GridLayout`では、フラットビュー階層を使用します。ここで、子ビューでは、行と列を指定することにより、グリッド内の位置が設定されます。 これにより、 *GridLayout*は、TableLayout で使用されるテーブル行に表示されるように、中間ビューがテーブル構造を提供しなくても、グリッドにビューを配置できます。 フラットな階層を維持することで、 *GridLayout*は子ビューをより迅速にレイアウトできます。 コードでこの概念が実際に意味するものを示す例を見てみましょう。


## <a name="creating-a-grid-layout"></a>グリッドレイアウトの作成

次の XML は、 `TextView` *GridLayout*に複数のコントロールを追加します。

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

次の図に示すように、レイアウトは行と列のサイズを調整して、セルがコンテンツに適合するようにします。

 [![左側の2つのセルが右側より小さい場合のレイアウトの図](grid-layout-images/gridlayout-cells.png)](grid-layout-images/gridlayout-cells.png#lightbox)

この結果、アプリケーションで次のユーザーインターフェイスが実行されます。

 [![4つのセルを表示する GridLayoutDemo アプリのスクリーンショット](grid-layout-images/01-gridlayout.png)](grid-layout-images/01-gridlayout.png#lightbox)



## <a name="specifying-orientation"></a>方向の指定

上記の XML では、 `TextView`各行または列が指定されていないことに注意してください。 これらが指定されてい`GridLayout`ない場合、では、向きに基づいて各子ビューが順番に割り当てられます。 たとえば、次のように、GridLayout の向きを既定値から垂直方向に変更してみましょう。

```xml
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"    
        android:rowCount="2"
        android:columnCount="2"
        android:orientation="vertical">
</GridLayout>
```

次に示す`GridLayout`ように、では、左から右ではなく、各列のセルが上から下に移動します。

 [![セルを垂直方向に配置する方法を示す図](grid-layout-images/gridlayoutorientation.png)](grid-layout-images/gridlayoutorientation.png#lightbox)

これにより、実行時に次のユーザーインターフェイスが生成されます。

 [![垂直方向に配置されたセルを含む GridLayoutDemo のスクリーンショット](grid-layout-images/02-gridlayout.png)](grid-layout-images/02-gridlayout.png#lightbox)



### <a name="specifying-explicit-position"></a>明示的な位置の指定

で`GridLayout`子ビューの位置を明示的に制御する場合は、属性`layout_row`と属性を`layout_column`設定できます。 たとえば、次の XML では、向きに関係なく、最初のスクリーンショット (上の図を参照) にレイアウトが表示されます。

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



### <a name="specifying-spacing"></a>間隔の指定

の子ビュー `GridLayout`の間隔を指定するオプションがいくつかあります。 次に示すよう`layout_margin`に、属性を使用して、各子ビューの余白を直接設定できます。

```xml
<TextView
            android:text="Cell 0"
            android:textSize="14dip"
            android:layout_row="0"
            android:layout_column="0"
            android:layout_margin="10dp" />
```

さらに、Android 4 では、という名前`Space`の新しい汎用的なスペースビューが使用できるようになりました。 これを使用するには、単に子ビューとして追加します。
たとえば、次の XML では`GridLayout` 、を`Space` 3 `rowcount`に設定してに追加の行を追加し、との間に`TextViews`スペースを提供するビューを追加しています。

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

この XML は、 `GridLayout`に次のようにスペースを作成します。

 [![スペースを含む大きなセルを示す GridLayoutDemo のスクリーンショット](grid-layout-images/03-gridlayout.png)](grid-layout-images/03-gridlayout.png#lightbox)

新しい`Space`ビューを使用する利点は、スペースを許可し、すべての子ビューに属性を設定する必要がないことです。



### <a name="spanning-columns-and-rows"></a>列と行のスパニング

で`GridLayout`は、複数の列および行にまたがるセルもサポートされています。 たとえば、次に示すように、ボタンを含む別の`GridLayout`行をに追加したとします。

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

これにより、次に示すように`GridLayout` 、ボタンのサイズに合わせての最初の列が拡張されます。

[![最初の列にのみまたがるボタンを含む GridLayoutDemo のスクリーンショット](grid-layout-images/04-gridlayout.png)](grid-layout-images/04-gridlayout.png#lightbox)

最初の列の伸縮を維持するには、次のように columnspan を設定して、2つの列をスパンするようにボタンを設定します。

```xml
<Button
    android:id="@+id/myButton"
    android:text="@string/hello"       
    android:layout_row="3"
    android:layout_column="0"
    android:layout_columnSpan="2" />
```

これを行うと、前に示し`TextViews`たレイアウトに似たのレイアウトが生成されます。次に示すように、 `GridLayout`ボタンがの下部に追加されます。

 [![両方の列にまたがるボタンを含む GridLayoutDemo のスクリーンショット](grid-layout-images/05-gridlayout.png)](grid-layout-images/05-gridlayout.png#lightbox)


## <a name="related-links"></a>関連リンク

- [GridLayoutDemo (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/platformfeatures-ics-samples-gridlayoutdemo)
- [アイスクリームサンドイッチの導入](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 プラットフォーム](https://developer.android.com/sdk/android-4.0.html)
