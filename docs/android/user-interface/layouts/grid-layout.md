---
title: GridLayout
ms.prod: xamarin
ms.assetid: B69A4BF5-9CFB-443A-9F7B-062D1E498F61
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: 3b2b6a3f1aa575553964e71be3a64cec16bc616f
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91457329"
---
# <a name="xamarinandroid-gridlayout"></a>Xamarin Android GridLayout

`GridLayout`は、次に示すように、HTML テーブルと同様に、 `ViewGroup` 2d グリッドでのビューのレイアウトをサポートする新しいサブクラスです。

 [![4つのセルを表示するトリミング GridLayout](grid-layout-images/21-gridlayoutcropped.png)](grid-layout-images/21-gridlayoutcropped.png#lightbox)

 `GridLayout` では、フラットビュー階層を使用します。ここで、子ビューでは、行と列を指定することにより、グリッド内の位置が設定されます。 これにより、 *GridLayout* は、TableLayout で使用されるテーブル行に表示されるように、中間ビューがテーブル構造を提供しなくても、グリッドにビューを配置できます。 フラットな階層を維持することで、 *GridLayout* は子ビューをより迅速にレイアウトできます。 コードでこの概念が実際に意味するものを示す例を見てみましょう。

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

上記の XML では、各行 `TextView` または列が指定されていないことに注意してください。 これらが指定されていない場合、では、 `GridLayout` 向きに基づいて各子ビューが順番に割り当てられます。 たとえば、次のように、GridLayout の向きを既定値から垂直方向に変更してみましょう。

```xml
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"    
        android:rowCount="2"
        android:columnCount="2"
        android:orientation="vertical">
</GridLayout>
```

次に示すように、では、 `GridLayout` 左から右ではなく、各列のセルが上から下に移動します。

 [![セルを垂直方向に配置する方法を示す図](grid-layout-images/gridlayoutorientation.png)](grid-layout-images/gridlayoutorientation.png#lightbox)

これにより、実行時に次のユーザーインターフェイスが生成されます。

 [![垂直方向に配置されたセルを含む GridLayoutDemo のスクリーンショット](grid-layout-images/02-gridlayout.png)](grid-layout-images/02-gridlayout.png#lightbox)

### <a name="specifying-explicit-position"></a>明示的な位置の指定

で子ビューの位置を明示的に制御する場合は、 `GridLayout` `layout_row` 属性と属性を設定でき `layout_column` ます。 たとえば、次の XML では、向きに関係なく、最初のスクリーンショット (上の図を参照) にレイアウトが表示されます。

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

の子ビューの間隔を指定するオプションがいくつかあり `GridLayout` ます。 `layout_margin`次に示すように、属性を使用して、各子ビューの余白を直接設定できます。

```xml
<TextView
            android:text="Cell 0"
            android:textSize="14dip"
            android:layout_row="0"
            android:layout_column="0"
            android:layout_margin="10dp" />
```

さらに、Android 4 では、という名前の新しい汎用的なスペースビュー `Space` が使用できるようになりました。 これを使用するには、単に子ビューとして追加します。
たとえば、次の XML では、を3に設定してに追加の行を追加 `GridLayout` `rowcount` し、と `Space` の間にスペースを提供するビューを追加して `TextViews` います。

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

この XML は、に次のようにスペースを作成し `GridLayout` ます。

 [![スペースを含む大きなセルを示す GridLayoutDemo のスクリーンショット](grid-layout-images/03-gridlayout.png)](grid-layout-images/03-gridlayout.png#lightbox)

新しいビューを使用する利点は、 `Space` スペースを許可し、すべての子ビューに属性を設定する必要がないことです。

### <a name="spanning-columns-and-rows"></a>列と行のスパニング

では、 `GridLayout` 複数の列および行にまたがるセルもサポートされています。 たとえば、次に示すように、ボタンを含む別の行をに追加した `GridLayout` とします。

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

これにより、次に示すように、ボタンのサイズに合わせての最初の列が `GridLayout` 拡張されます。

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

これを行うと、 `TextViews` 前に示したレイアウトに似たのレイアウトが生成され `GridLayout` ます。次に示すように、ボタンがの下部に追加されます。

 [![両方の列にまたがるボタンを含む GridLayoutDemo のスクリーンショット](grid-layout-images/05-gridlayout.png)](grid-layout-images/05-gridlayout.png#lightbox)

## <a name="related-links"></a>関連リンク

- [GridLayoutDemo (サンプル)](/samples/xamarin/monodroid-samples/gridlayoutdemo)