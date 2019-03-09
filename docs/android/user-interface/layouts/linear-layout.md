---
title: LinearLayout
ms.prod: xamarin
ms.assetid: B49D129C-AF24-3C5A-C833-5A34AFBB2442
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/07/2018
ms.openlocfilehash: f3d0394f6b2388918f728bd5a25e9e809a832ca6
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57670975"
---
# <a name="linearlayout"></a>LinearLayout

[`LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) は、 [`ViewGroup`](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/)
子を表示します。 [`View`](https://developer.xamarin.com/api/type/Android.Views.View/)
要素を線形の方向、垂直方向または水平方向にします。

過剰使用について注意が必要、 [ `LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)します。
複数の入れ子を開始する場合[ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)s の使用を検討する可能性があります、 [`RelativeLayout`](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/)
その代わりに。

という名前の新しいプロジェクトを開始**HelloLinearLayout**します。

開いている**Resources/Layout/Main.axml**し、次を挿入します。

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

この XML をよく確認します。 ルートがあります。 [`LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)
垂直方向にその向きを定義する&ndash;すべての子[ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/)s (どの it が 2 つあります) になります積み上げ縦方向にします。 最初の子は、別 [`LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)
水平方向の向きを使用して、2 番目の子が、 [`LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)
垂直方向を使用するとします。 入れ子になったこれらの各[ `LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)いくつかに含まれています [`TextView`](https://developer.xamarin.com/api/type/Android.Widget.TextView/)
要素で、その親によって定義された方法で互いを向いた[ `LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)します。

今すぐ開きます**HelloLinearLayout.cs**を読み込むことを確認して、 **Resources/Layout/Main.axml**レイアウトで、 [`OnCreate()`](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/)
方法:

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

[ `SetContentView(int)` ](https://developer.xamarin.com/api/member/Android.App.Activity.SetContentView/(System.Int32))メソッドのレイアウト ファイルを読み込み、 [ `Activity`](https://developer.xamarin.com/api/type/Android.App.Activity/)リソース ID で指定された&ndash;`Resources.Layout.Main`を指す、**リソース/レイアウト/Main.axml**レイアウト ファイルです。

アプリケーションを実行します。 次が表示されます。

[![スクリーン ショット最初 LinearLayout が水平方向に配置されたアプリの 2 つ目の垂直方向に](linear-layout-images/helloviews1.png)](linear-layout-images/helloviews1.png#lightbox)

XML 属性が各ビューの動作を定義する方法に注意してください。 さまざまな値を試し、`android:layout_weight`の各要素のウエイトに基づいて、実際の画面に配布する方法を参照してください。 参照してください、[共通レイアウト オブジェクト](https://developer.android.com/guide/topics/ui/declaring-layout.html)方法の詳細についてのドキュメント [`LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)
ハンドル、`android:layout_weight`属性。


## <a name="references"></a>関連項目

-   [`LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) 
-   [`TextView`](https://developer.xamarin.com/api/type/Android.Widget.TextView/) 

*このページの部分が作成および Android のオープン ソース プロジェクトで共有し、の条項に従って使用作業に基づいた変更、*
[*Creative Commons 2.5 Attribution License*](http://creativecommons.org/licenses/by/2.5/).

