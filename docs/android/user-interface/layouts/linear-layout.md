---
title: Xamarin Android LinearLayout
ms.prod: xamarin
ms.assetid: B49D129C-AF24-3C5A-C833-5A34AFBB2442
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/07/2018
ms.openlocfilehash: 3171a89678e88a924198c3921d197c0f0378d29b
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2019
ms.locfileid: "69522628"
---
# <a name="xamarinandroid-linearlayout"></a>Xamarin Android LinearLayout

[`LinearLayout`](xref:Android.Widget.LinearLayout)はです。[`ViewGroup`](xref:Android.Views.ViewGroup)
子を表示する[`View`](xref:Android.Views.View)
垂直方向または水平方向の線形方向の要素。

を使用する場合は、 [`LinearLayout`](xref:Android.Widget.LinearLayout)を使用することをお勧めします。
複数[`LinearLayout`](xref:Android.Widget.LinearLayout)のを入れ子にする場合は、[`RelativeLayout`](xref:Android.Widget.RelativeLayout)
その代わりに。

**HelloLinearLayout**という名前の新しいプロジェクトを開始します。

**Resources/Layout/Main**を開き、次のように挿入します。

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation=    "vertical"
    android:layout_width=    "match_parent"
    android:layout_height=    "match_parent"    >

  <LinearLayout
      android:orientation=    "horizontal"
      android:layout_width=    "match_parent"
      android:layout_height=    "match_parent"
      android:layout_weight=    "1"    >
      <TextView
          android:text=    "red"
          android:gravity=    "center_horizontal"
          android:background=    "#aa0000"
          android:layout_width=    "wrap_content"
          android:layout_height=    "match_parent"
          android:layout_weight=    "1"    />
      <TextView
          android:text=    "green"
          android:gravity=    "center_horizontal"
          android:background=    "#00aa00"
          android:layout_width=    "wrap_content"
          android:layout_height=    "match_parent"
          android:layout_weight=    "1"    />
      <TextView
          android:text=    "blue"
          android:gravity=    "center_horizontal"
          android:background=    "#0000aa"
          android:layout_width=    "wrap_content"
          android:layout_height=    "match_parent"
          android:layout_weight=    "1"    />
      <TextView
          android:text=    "yellow"
          android:gravity=    "center_horizontal"
          android:background=    "#aaaa00"
          android:layout_width=    "wrap_content"
          android:layout_height=    "match_parent"
          android:layout_weight=    "1"    />
  </LinearLayout>
        
  <LinearLayout
    android:orientation=    "vertical"
    android:layout_width=    "match_parent"
    android:layout_height=    "match_parent"
    android:layout_weight=    "1"    >
    <TextView
        android:text=    "row one"
        android:textSize=    "15pt"
        android:layout_width=    "match_parent"
        android:layout_height=    "wrap_content"
        android:layout_weight=    "1"    />
    <TextView
        android:text=    "row two"
        android:textSize=    "15pt"
        android:layout_width=    "match_parent"
        android:layout_height=    "wrap_content"
        android:layout_weight=    "1"    />
    <TextView
        android:text=    "row three"
        android:textSize=    "15pt"
        android:layout_width=    "match_parent"
        android:layout_height=    "wrap_content"
        android:layout_weight=    "1"    />
    <TextView
        android:text=    "row four"
        android:textSize=    "15pt"
        android:layout_width=    "match_parent"
        android:layout_height=    "wrap_content"
       android:layout_weight=    "1"    />
  </LinearLayout>

</LinearLayout>
```

この XML を慎重に調査してください。 ルートがある[`LinearLayout`](xref:Android.Widget.LinearLayout)
その向きが垂直&ndash;方向に定義されている (そのうち2つの子[`View`](xref:Android.Views.View)) が垂直方向に積み上げられます。 最初の子はもう1つ[`LinearLayout`](xref:Android.Widget.LinearLayout)
水平方向が使用され、2番目の子が[`LinearLayout`](xref:Android.Widget.LinearLayout)
これは垂直方向を使用します。 入れ子になっ[`LinearLayout`](xref:Android.Widget.LinearLayout)た各には、次のものが含まれます。[`TextView`](xref:Android.Widget.TextView)
要素は、親[`LinearLayout`](xref:Android.Widget.LinearLayout)によって定義された方法で相互に方向付けられます。

**HelloLinearLayout.cs**を開き、次のように、**リソース/レイアウト/メインの axml**レイアウトが読み込まれていることを確認します。[`OnCreate()`](xref:Android.App.Activity.OnCreate*)
b

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

) [`SetContentView(int)`](xref:Android.App.Activity.SetContentView*)メソッドは、リソース ID [`Activity`](xref:Android.App.Activity) &ndash; `Resources.Layout.Main`によって指定されたのレイアウトファイルを読み込み、リソース **/レイアウト/メインの axml**レイアウトファイルを参照します。

アプリケーションを実行します。 次のように表示されます。

[![アプリの最初の LinearLayout のスクリーンショット、2番目の垂直方向に配置](linear-layout-images/helloviews1.png)](linear-layout-images/helloviews1.png#lightbox)

XML 属性によって各ビューの動作が定義されていることに注意してください。 の`android:layout_weight`さまざまな値を試してみると、各要素の重みに基づいて画面の実際の資産がどのように分布しているかがわかります。 方法の詳細については、[一般的なレイアウトオブジェクト](https://developer.android.com/guide/topics/ui/declaring-layout.html)に関するドキュメントを参照してください。[`LinearLayout`](xref:Android.Widget.LinearLayout)
属性を`android:layout_weight`処理します。


## <a name="references"></a>リファレンス

- [`LinearLayout`](xref:Android.Widget.LinearLayout)
- [`TextView`](xref:Android.Widget.TextView)

_このページの一部は、Android オープンソースプロジェクトによって作成および共有され、 [Creative Commons 2.5 属性](http://creativecommons.org/licenses/by/2.5/)で説明されている条項に従って使用される作業に基づいて変更されます。_
