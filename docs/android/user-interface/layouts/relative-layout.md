---
title: Xamarin. Android での RelativeLayout の使用
description: RelativeLayout を Xamarin Android アプリケーションで使用する方法
ms.prod: xamarin
ms.assetid: AFD9C849-02C3-E728-BC78-77A563612BC5
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/29/2018
ms.openlocfilehash: af74ae3c7c87f501bff519bcfa361264205ca3f1
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2019
ms.locfileid: "69522381"
---
# <a name="xamarinandroid-relativelayout"></a>Xamarin Android RelativeLayout

[`RelativeLayout`](xref:Android.Widget.RelativeLayout)子を表示するです。 [`ViewGroup`](xref:Android.Views.ViewGroup)[`View`](xref:Android.Views.View)
相対位置の要素。 の位置は、 [`View`](xref:Android.Views.View)兄弟要素 (特定の要素の左または下) に対する相対値として指定することも、[`RelativeLayout`](xref:Android.Widget.RelativeLayout)
領域 (中央に配置されたなど)。

は[`RelativeLayout`](xref:Android.Widget.RelativeLayout) 、入れ子になっ[`ViewGroup`](xref:Android.Views.ViewGroup)たを排除できるため、ユーザーインターフェイスを設計するための非常に強力なユーティリティです。 入れ子になった複数のを使用している場合[`LinearLayout`](xref:Android.Widget.LinearLayout)
グループは、1つ[`RelativeLayout`](xref:Android.Widget.RelativeLayout)のに置き換えることができます。

**HelloRelativeLayout**という名前の新しいプロジェクトを開始します。

**Resources/Layout/Main. axml**ファイルを開き、次のように挿入します。

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <TextView
        android:id="@+id/label"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Type here:"/>
    <EditText
        android:id="@+id/entry"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:background="@android:drawable/editbox_background"
        android:layout_below="@id/label"/>
    <Button
        android:id="@+id/ok"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_below="@id/entry"
        android:layout_alignParentRight="true"
        android:layout_marginLeft="10dip"
        android:text="OK" />
    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_toLeftOf="@id/ok"
        android:layout_alignTop="@id/ok"
        android:text="Cancel" />
</RelativeLayout>
```

`android:layout_*` `layout_below`、 、などの各属性に注意してください。`layout_alignParentRight` `layout_toLeftOf`
を使用する[`RelativeLayout`](xref:Android.Widget.RelativeLayout)場合は、これらの属性を使用して、各[`View`](xref:Android.Views.View)の配置方法を記述できます。 これらの属性のいずれかによって、異なる種類の相対位置が定義します。 一部の属性では、兄弟[`View`](xref:Android.Views.View)のリソース ID を使用して、独自の相対位置を定義します。 たとえば、最後[`Button`](xref:Android.Widget.Button)のは、 `ok` ID によって[`View`](xref:Android.Views.View)識別される (前[`Button`](xref:Android.Widget.Button)の) の左側および固定されたに対して定義されます。

使用可能なすべてのレイアウト属性は、 [`RelativeLayout.LayoutParams`](xref:Android.Widget.RelativeLayout.LayoutParams)で定義されています。

このレイアウトは、[`OnCreate()`](xref:Android.App.Activity.OnCreate*)
b

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

メソッド[`SetContentView(int)`](xref:Android.App.Activity.SetContentView*) [`Activity`](xref:Android.App.Activity)は、リソース&mdash; ID`Resource.Layout.Main`で指定されたのレイアウトファイルを読み込み、リソース **/レイアウト/メインの axml**レイアウトファイルを参照します。

アプリケーションを実行します。 次のレイアウトが表示されます。

[![TextView、EditText、および2つのボタンを使用した相対レイアウトのスクリーンショット](relative-layout-images/helloviews2.png)](relative-layout-images/helloviews2.png#lightbox)

## <a name="resources"></a>リソース

- [`RelativeLayout`](xref:Android.Widget.RelativeLayout)
- [`RelativeLayout.LayoutParams`](xref:Android.Widget.RelativeLayout.LayoutParams)
- [`TextView`](xref:Android.Widget.TextView)
- [`EditText`](xref:Android.Widget.EditText)
- [`Button`](xref:Android.Widget.Button)

_このページの一部は、Android オープンソースプロジェクトによって作成および共有され、 [Creative Commons 2.5 属性](http://creativecommons.org/licenses/by/2.5/)で説明されている条項に従って使用される作業に基づいて変更されます。_
