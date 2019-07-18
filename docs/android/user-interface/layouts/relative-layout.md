---
title: Xamarin.Android で、[相対レイアウト] を使用します。
description: '[相対レイアウト] を Xamarin.Android アプリケーションで使用する方法'
ms.prod: xamarin
ms.assetid: AFD9C849-02C3-E728-BC78-77A563612BC5
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/29/2018
ms.openlocfilehash: af2972ecc92435836a75013e6203ba47c2c04627
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61303627"
---
# <a name="relativelayout"></a>RelativeLayout

[`RelativeLayout`](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/) [ `ViewGroup` ](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/)子を表示します。 [`View`](https://developer.xamarin.com/api/type/Android.Views.View/)
相対的な位置の要素。 位置を[ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) (左のか、または特定の要素の下など) の兄弟要素に対して相対的に指定できるまたは配置からの相対、 [`RelativeLayout`](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/)
(このような配置されているので、一番下の左 center) の領域。

A [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/)入れ子になったユーザー インターフェイスのデザインを排除できるので非常に強力なユーティリティは、 [ `ViewGroup`](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/)秒。 入れ子になったいくつか使用して自分で検索する場合 [`LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)
グループ、ことができます、1 つに置き換える[ `RelativeLayout`](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/)します。

という名前の新しいプロジェクトを開始**HelloRelativeLayout**します。

開く、 **Resources/Layout/Main.axml**ファイルを開き、次を挿入します。

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

各に注意してください、`android:layout_*`などの属性`layout_below`、 `layout_alignParentRight`、および`layout_toLeftOf`します。
使用する場合、 [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/)、これらの属性を使用するにはそれぞれを配置する方法を説明する[ `View`](https://developer.xamarin.com/api/type/Android.Views.View/)します。 これらの属性のそれぞれは、別の種類の相対位置を定義します。 いくつかの属性は、兄弟のリソース ID を使用して[ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/)相対の位置を定義します。 最後の例では、 [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/)の左と配置-と-、-最上位の線上に定義されて、 [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) ID で識別される`ok`(これは、前の[`Button`](https://developer.xamarin.com/api/type/Android.Widget.Button/)).

定義されているすべての利用可能なレイアウト属性は[ `RelativeLayout.LayoutParams`](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout+LayoutParams/)します。

このレイアウトでの負荷になっていることを確認します [`OnCreate()`](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/)
方法:

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

[ `SetContentView(int)` ](https://developer.xamarin.com/api/member/Android.App.Activity.SetContentView/p/System.Int32/)メソッドのレイアウト ファイルを読み込み、 [ `Activity`](https://developer.xamarin.com/api/type/Android.App.Activity/)リソース ID で指定された&mdash;`Resource.Layout.Main`を指す、**リソース/レイアウト/Main.axml**レイアウト ファイルです。

アプリケーションを実行します。 次のレイアウトが表示されます。

[![TextView、EditText、2 つのボタンと相対的なレイアウトのスクリーン ショット](relative-layout-images/helloviews2.png)](relative-layout-images/helloviews2.png#lightbox)


## <a name="resources"></a>リソース

-   [`RelativeLayout`](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/)
-   [`RelativeLayout.LayoutParams`](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout+LayoutParams/)
-   [`TextView`](https://developer.xamarin.com/api/type/Android.Widget.TextView/)
-   [`EditText`](https://developer.xamarin.com/api/type/Android.Widget.EditText/)
-   [`Button`](https://developer.xamarin.com/api/type/Android.Widget.Button/)


*このページの部分が作成および Android のオープン ソース プロジェクトで共有し、の条項に従って使用作業に基づいた変更、*
[*Creative Commons 2.5 Attribution License*](http://creativecommons.org/licenses/by/2.5/).
