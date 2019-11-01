---
title: Xamarin Android LinearLayout
ms.prod: xamarin
ms.assetid: B49D129C-AF24-3C5A-C833-5A34AFBB2442
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/07/2018
ms.openlocfilehash: b1cff01c66ae2581a68286e62bd8c8c5fb7f9d72
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028960"
---
# <a name="xamarinandroid-linearlayout"></a>Xamarin Android LinearLayout

[`LinearLayout`](xref:Android.Widget.LinearLayout)は[`ViewGroup`](xref:Android.Views.ViewGroup)
子[`View`](xref:Android.Views.View)が表示されます。
垂直方向または水平方向の線形方向の要素。

[`LinearLayout`](xref:Android.Widget.LinearLayout)の使用については、慎重に検討する必要があります。
複数の[`LinearLayout`](xref:Android.Widget.LinearLayout)s の入れ子を開始する場合は、を使用することを検討してください[`RelativeLayout`](xref:Android.Widget.RelativeLayout)
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

この XML を慎重に調査してください。 ルート[`LinearLayout`](xref:Android.Widget.LinearLayout)
これにより、縦方向の向きが定義され &ndash; すべての子[`View`](xref:Android.Views.View)(そのうち2つを含む) が垂直方向に積み上げられます。 最初の子はもう1つ[`LinearLayout`](xref:Android.Widget.LinearLayout)
水平方向を使用し、2番目の子が[`LinearLayout`](xref:Android.Widget.LinearLayout)である場合
これは垂直方向を使用します。 これらの入れ子になった[`LinearLayout`](xref:Android.Widget.LinearLayout)s にはいくつかの[`TextView`](xref:Android.Widget.TextView)が含まれています
要素は、親[`LinearLayout`](xref:Android.Widget.LinearLayout)によって定義された方法で相互に方向付けられます。

**HelloLinearLayout.cs**を開き、次のように[`OnCreate()`](xref:Android.App.Activity.OnCreate*) 、**リソース/レイアウト/メインの axml**レイアウトを読み込みます。
b

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

[`SetContentView(int)`](xref:Android.App.Activity.SetContentView*)) メソッドは、リソース ID によって指定された[`Activity`](xref:Android.App.Activity)のレイアウトファイルを読み込み `Resources.Layout.Main` &ndash; リソース **/レイアウト/メインの axml**レイアウトファイルを参照します。

アプリケーションを実行します。 次のように表示されます。

[アプリの最初の LinearLayout のスクリーンショット (2 番目に横方向に配置) のスクリーンショット![](linear-layout-images/helloviews1.png)](linear-layout-images/helloviews1.png#lightbox)

XML 属性によって各ビューの動作が定義されていることに注意してください。 `android:layout_weight` に対してさまざまな値を試してみて、各要素の重みに基づいて画面の実際の資産がどのように分布しているかを確認してください。 方法の詳細については、[一般的なレイアウトオブジェクト](https://developer.android.com/guide/topics/ui/declaring-layout.html)のドキュメントを参照してください[`LinearLayout`](xref:Android.Widget.LinearLayout)
`android:layout_weight` 属性を処理します。

## <a name="references"></a>関連項目

- [`LinearLayout`](xref:Android.Widget.LinearLayout)
- [`TextView`](xref:Android.Widget.TextView)

_このページの一部は、Android オープンソースプロジェクトによって作成および共有され、 [Creative Commons 2.5 属性](https://creativecommons.org/licenses/by/2.5/)で説明されている条項に従って使用される作業に基づいて変更されます。_
