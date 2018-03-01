---
title: LinearLayout
ms.topic: article
ms.prod: xamarin
ms.assetid: B49D129C-AF24-3C5A-C833-5A34AFBB2442
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 3cc5db39280c72f0de9dbdae07a49b56416c90a5
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="linearlayout"></a>LinearLayout

[`LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) [ `ViewGroup` ](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/)子を表示する[ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/)要素を線形の方向に垂直または水平方向にします。

過剰な使用については注意する必要があります、 [ `LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)です。
入れ子になった複数を開始する場合[ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)s、することも検討して、 [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/)代わりにします。

という名前の新しいプロジェクトを開始**HelloLinearLayout**です。

開いている**Resources/Layout/Main.axml**し、次の挿入します。

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation=    "vertical"
    android:layout_width=    "fill_parent"
    android:layout_height=    "fill_parent"    >

  <LinearLayout
      android:orientation=    "horizontal"
      android:layout_width=    "fill_parent"
      android:layout_height=    "fill_parent"
      android:layout_weight=    "1"    >
      <TextView
          android:text=    "red"
          android:gravity=    "center_horizontal"
          android:background=    "#aa0000"
          android:layout_width=    "wrap_content"
          android:layout_height=    "fill_parent"
          android:layout_weight=    "1"    />
      <TextView
          android:text=    "green"
          android:gravity=    "center_horizontal"
          android:background=    "#00aa00"
          android:layout_width=    "wrap_content"
          android:layout_height=    "fill_parent"
          android:layout_weight=    "1"    />
      <TextView
          android:text=    "blue"
          android:gravity=    "center_horizontal"
          android:background=    "#0000aa"
          android:layout_width=    "wrap_content"
          android:layout_height=    "fill_parent"
          android:layout_weight=    "1"    />
      <TextView
          android:text=    "yellow"
          android:gravity=    "center_horizontal"
          android:background=    "#aaaa00"
          android:layout_width=    "wrap_content"
          android:layout_height=    "fill_parent"
          android:layout_weight=    "1"    />
  </LinearLayout>
        
  <LinearLayout
    android:orientation=    "vertical"
    android:layout_width=    "fill_parent"
    android:layout_height=    "fill_parent"
    android:layout_weight=    "1"    >
    <TextView
        android:text=    "row one"
        android:textSize=    "15pt"
        android:layout_width=    "fill_parent"
        android:layout_height=    "wrap_content"
        android:layout_weight=    "1"    />
    <TextView
        android:text=    "row two"
        android:textSize=    "15pt"
        android:layout_width=    "fill_parent"
        android:layout_height=    "wrap_content"
        android:layout_weight=    "1"    />
    <TextView
        android:text=    "row three"
        android:textSize=    "15pt"
        android:layout_width=    "fill_parent"
        android:layout_height=    "wrap_content"
        android:layout_weight=    "1"    />
    <TextView
        android:text=    "row four"
        android:textSize=    "15pt"
        android:layout_width=    "fill_parent"
        android:layout_height=    "wrap_content"
       android:layout_weight=    "1"    />
  </LinearLayout>

</LinearLayout>
```

この XML を慎重に検査します。 ルートがある[ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)を垂直方向の方向を定義する&ndash;すべての子[ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/)(そのされているは 2 つ) は値には積み上げ縦方向にします。 最初の子は、別[ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)を水平方向を使用して、2 番目の子は、 [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)垂直方向を使用します。 入れ子になったこれらの各[ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)s には、いくつか含まれて[ `TextView` ](https://developer.xamarin.com/api/type/Android.Widget.TextView/)要素で、その親によって定義された方法で互いを向いた[ `LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/).

これで開く**HelloLinearLayout.cs**を読み込むことを確認して、 **Resources/Layout/Main.axml**レイアウトで、 [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/)メソッド。

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

[ `SetContentView(int)` ](https://developer.xamarin.com/api/member/Android.App.Activity.SetContentView/(System.Int32))メソッドのレイアウト ファイルの読み込み、 [ `Activity`](https://developer.xamarin.com/api/type/Android.App.Activity/)リソース ID で指定された&ndash;`Resources.Layout.Main`を指す、**リソース/レイアウト/[Main.axml]**レイアウト ファイルです。

アプリケーションを実行します。 次が表示されます。

[ ![スクリーン ショットの最初の LinearLayout が水平方向に配置されたアプリの 2 番目の垂直方向に](linear-layout-images/helloviews1.png)](linear-layout-images/helloviews1.png)

XML 属性が各ビューの動作を定義する方法に注意してください。 値が異なるを試し`android:layout_weight`の各要素の重み付けに基づいて、実際の画面に配布する方法を参照してください。 参照してください、[共通オブジェクトのレイアウト](http://developer.android.com/guide/topics/ui/declaring-layout.html)方法の詳細については、ドキュメント[ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)ハンドル、`android:layout_weight`属性。

<a name="References" />

## <a name="references"></a>参照

-   [`LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) 
-   [`TextView`](https://developer.xamarin.com/api/type/Android.Widget.TextView/) 

*このページの部分は変更を作成し、Android のオープン ソース プロジェクトで共有しての条項に従って使用作業に基づく、*
[*クリエイティブ コモンズ 2.5 Attribution ライセンス*](http://creativecommons.org/licenses/by/2.5/).

