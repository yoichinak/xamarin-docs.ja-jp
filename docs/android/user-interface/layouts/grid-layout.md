---
title: GridLayout
ms.prod: xamarin
ms.assetid: B69A4BF5-9CFB-443A-9F7B-062D1E498F61
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: bd05596ce8c6f8acb81b3ca68c6393a0be47768a
ms.sourcegitcommit: cb13fadbaa6d19dea94b9005bda20c2efd1b8039
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/17/2019
ms.locfileid: "72541915"
---
# <a name="xamarinandroid-gridlayout"></a>Xamarin Android GridLayout

@No__t_0 は、次に示すように、HTML テーブルと同様に、2D グリッドでのビューのレイアウトをサポートする新しい `ViewGroup` サブクラスです。

 [4つのセルを表示する ![Cropped GridLayout](grid-layout-images/21-gridlayoutcropped.png)](grid-layout-images/21-gridlayoutcropped.png#lightbox)

 `GridLayout` は、フラットビュー階層を使用します。この場合、子ビューでは、グリッド内の位置が、列に含まれる行と列を指定することによって設定されます。 これにより、 *GridLayout*は、TableLayout で使用されるテーブル行に表示されるように、中間ビューがテーブル構造を提供しなくても、グリッドにビューを配置できます。 フラットな階層を維持することで、 *GridLayout*は子ビューをより迅速にレイアウトできます。 コードでこの概念が実際に意味するものを示す例を見てみましょう。

## <a name="creating-a-grid-layout"></a>グリッドレイアウトの作成

次の XML は、 *GridLayout*にいくつかの `TextView` コントロールを追加します。

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

 [左側の2つのセルが右側より小さい場合に表示されるレイアウトの ![Diagram](grid-layout-images/gridlayout-cells.png)](grid-layout-images/gridlayout-cells.png#lightbox)

この結果、アプリケーションで次のユーザーインターフェイスが実行されます。

 [4つのセルを表示する GridLayoutDemo アプリの ![Screenshot](grid-layout-images/01-gridlayout.png)](grid-layout-images/01-gridlayout.png#lightbox)

## <a name="specifying-orientation"></a>方向の指定

上記の XML では、各 `TextView` が行または列を指定していないことに注意してください。 これらが指定されていない場合、`GridLayout` は、方向に基づいて各子ビューを順番に割り当てます。 たとえば、次のように、GridLayout の向きを既定値から垂直方向に変更してみましょう。

```xml
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"    
        android:rowCount="2"
        android:columnCount="2"
        android:orientation="vertical">
</GridLayout>
```

次に示すように、`GridLayout` では、左から右ではなく、各列のセルが上から下に配置されます。

 [セルが垂直方向に配置される方法を示す ![Diagram](grid-layout-images/gridlayoutorientation.png)](grid-layout-images/gridlayoutorientation.png#lightbox)

これにより、実行時に次のユーザーインターフェイスが生成されます。

 [垂直方向に配置されたセルを含む GridLayoutDemo の ![Screenshot](grid-layout-images/02-gridlayout.png)](grid-layout-images/02-gridlayout.png#lightbox)

### <a name="specifying-explicit-position"></a>明示的な位置の指定

@No__t_0 内の子ビューの位置を明示的に制御する場合は、それらの `layout_row` と `layout_column` 属性を設定します。 たとえば、次の XML では、向きに関係なく、最初のスクリーンショット (上の図を参照) にレイアウトが表示されます。

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

@No__t_0 の子ビューの間隔を指定するオプションがいくつかあります。 次に示すように、`layout_margin` 属性を使用して、各子ビューの余白を直接設定できます。

```xml
<TextView
            android:text="Cell 0"
            android:textSize="14dip"
            android:layout_row="0"
            android:layout_column="0"
            android:layout_margin="10dp" />
```

さらに、Android 4 では `Space` という名前の新しい汎用的なスペースビューが使用できるようになりました。 これを使用するには、単に子ビューとして追加します。
たとえば、次の XML では、その `rowcount` を3に設定して `GridLayout` に行を追加し、`TextViews` 間の間隔を提供する `Space` ビューを追加しています。

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

次に示すように、この XML では `GridLayout` にスペースが作成されます。

 [空白を含む大きなセルを示す GridLayoutDemo の ![Screenshot](grid-layout-images/03-gridlayout.png)](grid-layout-images/03-gridlayout.png#lightbox)

新しい `Space` ビューを使用する利点は、スペースを許可し、すべての子ビューに属性を設定する必要がないことです。

### <a name="spanning-columns-and-rows"></a>列と行のスパニング

@No__t_0 は、複数の列および行にまたがるセルもサポートしています。 たとえば、次に示すように、ボタンを含む別の行を `GridLayout` に追加するとします。

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

これにより、次に示すように、ボタンのサイズに合わせて `GridLayout` の最初の列が拡張されます。

[最初の列にのみまたがるボタンを含む GridLayoutDemo の ![Screenshot](grid-layout-images/04-gridlayout.png)](grid-layout-images/04-gridlayout.png#lightbox)

最初の列の伸縮を維持するには、次のように columnspan を設定して、2つの列をスパンするようにボタンを設定します。

```xml
<Button
    android:id="@+id/myButton"
    android:text="@string/hello"       
    android:layout_row="3"
    android:layout_column="0"
    android:layout_columnSpan="2" />
```

これを行うと、前に示したレイアウトに似た `TextViews` のレイアウトが生成されます。次に示すように、ボタンが `GridLayout` の下部に追加されます。

 [2つの列にまたがるボタンを使用した GridLayoutDemo の ![Screenshot](grid-layout-images/05-gridlayout.png)](grid-layout-images/05-gridlayout.png#lightbox)

## <a name="related-links"></a>関連リンク

- [GridLayoutDemo (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/gridlayoutdemo)
- [アイスクリームサンドイッチの導入](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 プラットフォーム](https://developer.android.com/sdk/android-4.0.html)
