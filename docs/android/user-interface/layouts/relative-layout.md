---
title: Xamarin. Android での RelativeLayout の使用
description: RelativeLayout を Xamarin Android アプリケーションで使用する方法
ms.prod: xamarin
ms.assetid: AFD9C849-02C3-E728-BC78-77A563612BC5
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/29/2018
ms.openlocfilehash: 6cac771f46242cc0475be0a7ec0d475950f4b4e1
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028796"
---
# <a name="xamarinandroid-relativelayout"></a>Xamarin Android RelativeLayout

[`RelativeLayout`](xref:Android.Widget.RelativeLayout)は、子を表示する[`ViewGroup`](xref:Android.Views.ViewGroup)です[`View`](xref:Android.Views.View)
相対位置の要素。 [`View`](xref:Android.Views.View)の位置は、兄弟要素 (特定の要素の左または下)、またはの相対位置に相対的に指定でき[`RelativeLayout`](xref:Android.Widget.RelativeLayout)
領域 (中央に配置されたなど)。

[`RelativeLayout`](xref:Android.Widget.RelativeLayout)は、入れ子になった[`ViewGroup`](xref:Android.Views.ViewGroup)s をなくすことができるため、ユーザーインターフェイスを設計するための非常に強力なユーティリティです。 入れ子になった[`LinearLayout`](xref:Android.Widget.LinearLayout)をいくつか使用している場合
グループは、1つの[`RelativeLayout`](xref:Android.Widget.RelativeLayout)に置き換えることができます。

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

`layout_below`、`layout_alignParentRight`、`layout_toLeftOf`などの `android:layout_*` の各属性に注意してください。
[`RelativeLayout`](xref:Android.Widget.RelativeLayout)を使用する場合は、これらの属性を使用して、各[`View`](xref:Android.Views.View)の配置方法を記述できます。 これらの属性のいずれかによって、異なる種類の相対位置が定義します。 一部の属性では、兄弟[`View`](xref:Android.Views.View)のリソース ID を使用して、独自の相対位置を定義します。 たとえば、最後の[`Button`](xref:Android.Widget.Button)は、ID `ok` (前の[`Button`](xref:Android.Widget.Button)) によって識別される[`View`](xref:Android.Views.View)の左側およびアラインによって指定されています。

使用可能なすべてのレイアウト属性は[`RelativeLayout.LayoutParams`](xref:Android.Widget.RelativeLayout.LayoutParams)で定義されています。

このレイアウトは、必ず[`OnCreate()`](xref:Android.App.Activity.OnCreate*)に読み込んでください。
b

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

[`SetContentView(int)`](xref:Android.App.Activity.SetContentView*)メソッドは、リソース ID によって指定された[`Activity`](xref:Android.App.Activity)のレイアウトファイルを読み込み `Resource.Layout.Main` &mdash; リソース **/レイアウト/メインの axml**レイアウトファイルを参照します。

アプリケーションを実行します。 次のレイアウトが表示されます。

[TextView、EditText、および2つのボタンを使用して相対レイアウトのスクリーンショットを![](relative-layout-images/helloviews2.png)](relative-layout-images/helloviews2.png#lightbox)

## <a name="resources"></a>リソース

- [`RelativeLayout`](xref:Android.Widget.RelativeLayout)
- [`RelativeLayout.LayoutParams`](xref:Android.Widget.RelativeLayout.LayoutParams)
- [`TextView`](xref:Android.Widget.TextView)
- [`EditText`](xref:Android.Widget.EditText)
- [`Button`](xref:Android.Widget.Button)

_このページの一部は、Android オープンソースプロジェクトによって作成および共有され、 [Creative Commons 2.5 属性](https://creativecommons.org/licenses/by/2.5/)で説明されている条項に従って使用される作業に基づいて変更されます。_
