---
title: TableLayout
ms.prod: xamarin
ms.assetid: 0C7B9C95-5E5F-A069-BA37-984E49F7DCAD
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 25b2393d2133c0a1f3f8354584c276fcd7ddaa4b
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61305161"
---
# <a name="tablelayout"></a>TableLayout

[`TableLayout`](https://developer.xamarin.com/api/type/Android.Widget.TableLayout/) は、 [`ViewGroup`](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/)
子を表示します。 [`View`](https://developer.xamarin.com/api/type/Android.Views.View/)
行と列内の要素。

という名前の新しいプロジェクトを開始**HelloTableLayout**します。

開く、 **Resources/Layout/Main.axml**ファイルを開き、次を挿入します。

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

HTML テーブルの構造に似て 方法に注意してください。 、 [`TableLayout`](https://developer.xamarin.com/api/type/Android.Widget.TableLayout/)
要素は、HTML と同様に、`<table>`要素。 [`TableRow`](https://developer.xamarin.com/api/type/Android.Widget.TableRow/)
似ています、`<tr>`要素は、セルを使用することがいずれかの種類[ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/)要素。 この例で、 [`TextView`](https://developer.xamarin.com/api/type/Android.Widget.TextView/)
各セルに使用されます。 一部の行には、間も、基本的な[ `View`](https://developer.xamarin.com/api/type/Android.Views.View/)水平線を描画に使用されます。

必ず、 **HelloTableLayout**アクティビティでは、このレイアウトの読み込み、 [`OnCreate()`](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/)
方法:

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

[ `SetContentView(int)` ](https://developer.xamarin.com/api/member/Android.App.Activity.SetContentView/(System.Int32))メソッドのレイアウト ファイルを読み込み、 [ `Activity`](https://developer.xamarin.com/api/type/Android.App.Activity/)リソース ID で指定された&mdash;`Resource.Layout.Main`を指す、**リソース/レイアウト/Main.axml**レイアウト ファイルです。

アプリケーションを実行します。 次が表示されます。

[![複数のテーブル行を表示する TableLayout アプリのスクリーン ショットの例](table-layout-images/helloviews3.png)](table-layout-images/helloviews3.png#lightbox)



## <a name="references"></a>参照

-   [`TableLayout`](https://developer.xamarin.com/api/type/Android.Widget.TableLayout/) 

-   [`TableRow`](https://developer.xamarin.com/api/type/Android.Widget.TableRow/) 

-   [`TextView`](https://developer.xamarin.com/api/type/Android.Widget.TextView/) 

*このページの部分が作成および Android のオープン ソース プロジェクトで共有し、の条項に従って使用作業に基づいた変更、*
[*Creative Commons 2.5 Attribution License*](http://creativecommons.org/licenses/by/2.5/).
