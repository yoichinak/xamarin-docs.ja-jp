---
title: Xamarin Android TableLayout
ms.prod: xamarin
ms.assetid: 0C7B9C95-5E5F-A069-BA37-984E49F7DCAD
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 1c477f030dc69394ba601b31d71a772f5037af48
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2019
ms.locfileid: "69522299"
---
# <a name="xamarinandroid-tablelayout"></a>Xamarin Android TableLayout

[`TableLayout`](xref:Android.Widget.TableLayout)はです。[`ViewGroup`](xref:Android.Views.ViewGroup)
子を表示する[`View`](xref:Android.Views.View)
行と列の要素。

**HelloTableLayout**という名前の新しいプロジェクトを開始します。

**Resources/Layout/Main. axml**ファイルを開き、次のように挿入します。

```xml
<?xml version="1.0" encoding="utf-8"?>
<TableLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:stretchColumns="1">

    <TableRow>
        <TextView
            android:layout_column="1"
            android:text="Open..."
            android:padding="3dip"/>
        <TextView
            android:text="Ctrl-O"
            android:gravity="right"
            android:padding="3dip"/>
    </TableRow>

    <TableRow>
        <TextView
            android:layout_column="1"
            android:text="Save..."
            android:padding="3dip"/>
        <TextView
            android:text="Ctrl-S"
            android:gravity="right"
            android:padding="3dip"/>
    </TableRow>

    <TableRow>
        <TextView
            android:layout_column="1"
            android:text="Save As..."
            android:padding="3dip"/>
        <TextView
            android:text="Ctrl-Shift-S"
            android:gravity="right"
            android:padding="3dip"/>
    </TableRow>

    <View
        android:layout_height="2dip"
        android:background="#FF909090"/>

    <TableRow>
        <TextView
            android:text="X"
            android:padding="3dip"/>
        <TextView
            android:text="Import..."
            android:padding="3dip"/>
    </TableRow>

    <TableRow>
        <TextView
            android:text="X"
            android:padding="3dip"/>
        <TextView
            android:text="Export..."
            android:padding="3dip"/>
        <TextView
            android:text="Ctrl-E"
            android:gravity="right"
            android:padding="3dip"/>
    </TableRow>

    <View
        android:layout_height="2dip"
        android:background="#FF909090"/>

    <TableRow>
        <TextView
            android:layout_column="1"
            android:text="Quit"
            android:padding="3dip"/>
    </TableRow>
</TableLayout>
```

これが HTML テーブルの構造に似ていることに注意してください。 、[`TableLayout`](xref:Android.Widget.TableLayout)
要素は HTML `<table>`要素に似ています。[`TableRow`](xref:Android.Widget.TableRow)
は要素に`<tr>`似ていますが、セルの場合は任意の種類[`View`](xref:Android.Views.View)の要素を使用できます。 この例では、[`TextView`](xref:Android.Widget.TextView)
は、各セルに対して使用されます。 一部の行の間には、水平線を描画[`View`](xref:Android.Views.View)するために使用される基本もあります。

**HelloTableLayout**アクティビティによって、このレイアウトが[`OnCreate()`](xref:Android.App.Activity.OnCreate*)
b

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

) [`SetContentView(int)`](xref:Android.App.Activity.SetContentView*)メソッドは、リソース ID [`Activity`](xref:Android.App.Activity) &mdash; `Resource.Layout.Main`によって指定されたのレイアウトファイルを読み込み、リソース **/レイアウト/メインの axml**レイアウトファイルを参照します。

アプリケーションを実行します。 次のように表示されます。

[![複数のテーブル行を表示している TableLayout アプリのスクリーンショットの例](table-layout-images/helloviews3.png)](table-layout-images/helloviews3.png#lightbox)



## <a name="references"></a>リファレンス

- [`TableLayout`](xref:Android.Widget.TableLayout)
- [`TableRow`](xref:Android.Widget.TableRow)
- [`TextView`](xref:Android.Widget.TextView)

_このページの一部は、Android オープンソースプロジェクトによって作成および共有され、 [Creative Commons 2.5 属性](http://creativecommons.org/licenses/by/2.5/)で説明されている条項に従って使用される作業に基づいて変更されます。_
