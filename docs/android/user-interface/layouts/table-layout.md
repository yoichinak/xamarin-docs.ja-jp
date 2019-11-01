---
title: Xamarin Android TableLayout
ms.prod: xamarin
ms.assetid: 0C7B9C95-5E5F-A069-BA37-984E49F7DCAD
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: 4175b1fa62b2bc0e4209d13934c2bdbdd1e2a085
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028749"
---
# <a name="xamarinandroid-tablelayout"></a>Xamarin Android TableLayout

[`TableLayout`](xref:Android.Widget.TableLayout)は[`ViewGroup`](xref:Android.Views.ViewGroup)
子[`View`](xref:Android.Views.View)が表示されます。
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

これが HTML テーブルの構造に似ていることに注意してください。 [`TableLayout`](xref:Android.Widget.TableLayout)
要素は HTML `<table>` 要素に似ています。[`TableRow`](xref:Android.Widget.TableRow)
は `<tr>` の要素に似ています。ただし、セルに対しては、任意の種類の[`View`](xref:Android.Views.View)要素を使用できます。 この例では、 [`TextView`](xref:Android.Widget.TextView)
は、各セルに対して使用されます。 一部の行の間には、基本的な[`View`](xref:Android.Views.View)もあります。これは、水平線を描画するために使用されます。

**HelloTableLayout**アクティビティによって、このレイアウトが読み込まれることを確認します。 [`OnCreate()`](xref:Android.App.Activity.OnCreate*)
b

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

[`SetContentView(int)`](xref:Android.App.Activity.SetContentView*)) メソッドは、リソース ID によって指定された[`Activity`](xref:Android.App.Activity)のレイアウトファイルを読み込み `Resource.Layout.Main` &mdash; リソース **/レイアウト/メインの axml**レイアウトファイルを参照します。

アプリケーションを実行します。 次のように表示されます。

[![複数のテーブル行を表示している TableLayout アプリのスクリーンショットの例](table-layout-images/helloviews3.png)](table-layout-images/helloviews3.png#lightbox)

## <a name="references"></a>関連項目

- [`TableLayout`](xref:Android.Widget.TableLayout)
- [`TableRow`](xref:Android.Widget.TableRow)
- [`TextView`](xref:Android.Widget.TextView)

_このページの一部は、Android オープンソースプロジェクトによって作成および共有され、 [Creative Commons 2.5 属性](https://creativecommons.org/licenses/by/2.5/)で説明されている条項に従って使用される作業に基づいて変更されます。_
