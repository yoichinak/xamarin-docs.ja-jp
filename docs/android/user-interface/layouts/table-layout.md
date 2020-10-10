---
title: Xamarin Android TableLayout
ms.prod: xamarin
ms.assetid: 0C7B9C95-5E5F-A069-BA37-984E49F7DCAD
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 10/08/2020
ms.openlocfilehash: e33dbae13dc9b7af7649777f16bf5d23e11209ad
ms.sourcegitcommit: 124d845f8d2768353e8b7fe1ab1d959a589367f7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/09/2020
ms.locfileid: "91872265"
---
# <a name="xamarinandroid-tablelayout"></a>Xamarin Android TableLayout

[`TableLayout`](xref:Android.Widget.TableLayout) はです。 [`ViewGroup`](xref:Android.Views.ViewGroup)
子を表示する [`View`](xref:Android.Views.View)
行と列の要素。

**HelloTableLayout**という名前の新しいプロジェクトを開始します。

**Resources/Layout/content_main.xml**ファイルを開き、次のように挿入します。

```xml
<?xml version="1.0" encoding="utf-8"?>
<TableLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:stretchColumns="1">

    <TableRow
        android:layout_width="match_parent"
        android:layout_height="match_parent">
        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_column="1"
            android:text="Open..."
            android:padding="3dip"/>
        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Ctrl-O"
            android:gravity="right"
            android:padding="3dip"/>
    </TableRow>

    <TableRow
        android:layout_width="match_parent"
        android:layout_height="match_parent">
        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_column="1"
            android:text="Save..."
            android:padding="3dip"/>
        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Ctrl-S"
            android:gravity="right"
            android:padding="3dip"/>
    </TableRow>

    <TableRow
        android:layout_width="match_parent"
        android:layout_height="match_parent">
        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_column="1"
            android:text="Save As..."
            android:padding="3dip"/>
        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Ctrl-Shift-S"
            android:gravity="right"
            android:padding="3dip"/>
    </TableRow>

    <View
        android:layout_width="wrap_content"
        android:layout_height="2dip"
        android:background="#FF909090"/>

    <TableRow
        android:layout_width="match_parent"
        android:layout_height="match_parent">
        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="X"
            android:padding="3dip"/>
        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Import..."
            android:padding="3dip"/>
    </TableRow>

    <TableRow
        android:layout_width="match_parent"
        android:layout_height="match_parent">
        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="X"
            android:padding="3dip"/>
        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Export..."
            android:padding="3dip"/>
        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Ctrl-E"
            android:gravity="right"
            android:padding="3dip"/>
    </TableRow>

    <View
        android:layout_width="wrap_content"
        android:layout_height="2dip"
        android:background="#FF909090"/>

    <TableRow
        android:layout_width="match_parent"
        android:layout_height="match_parent">
        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_column="1"
            android:text="Quit"
            android:padding="3dip"/>
    </TableRow>
</TableLayout>
```

これが HTML テーブルの構造に似ていることに注意してください。 [`TableLayout`](xref:Android.Widget.TableLayout)
要素は HTML 要素に似てい `<table>` ます。 [`TableRow`](xref:Android.Widget.TableRow)
は要素に似ていますが、セルの場合は `<tr>` 任意の種類の要素を使用でき [`View`](xref:Android.Views.View) ます。 この例では、 [`TextView`](xref:Android.Widget.TextView)
は、各セルに対して使用されます。 一部の行の間には、 [`View`](xref:Android.Views.View) 水平線を描画するために使用される基本もあります。

**HelloTableLayout**アクティビティによって、このレイアウトが[`OnCreate()`](xref:Android.App.Activity.OnCreate*)
b

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

[`SetContentView(int)`](xref:Android.App.Activity.SetContentView*)) メソッドは、 [`Activity`](xref:Android.App.Activity) リソース ID によって指定されたのレイアウトファイルを読み込み、リソース &mdash; `Resource.Layout.Main` **/レイアウト/メインの axml**レイアウトファイルを参照します。

アプリケーションを実行します。 次のように表示されます。

[![複数のテーブル行を表示している TableLayout アプリのスクリーンショットの例](table-layout-images/helloviews3.png)](table-layout-images/helloviews3.png#lightbox)

## <a name="references"></a>References

- [`TableLayout`](xref:Android.Widget.TableLayout)
- [`TableRow`](xref:Android.Widget.TableRow)
- [`TextView`](xref:Android.Widget.TextView)

_このページの一部は、Android オープンソースプロジェクトによって作成および共有され、 [Creative Commons 2.5 属性](https://creativecommons.org/licenses/by/2.5/)で説明されている条項に従って使用される作業に基づいて変更されます。_
