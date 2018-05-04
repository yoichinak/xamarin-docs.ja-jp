---
title: Xamarin.Android で、[相対レイアウト] の使用
ms.prod: xamarin
ms.assetid: AFD9C849-02C3-E728-BC78-77A563612BC5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/25/2018
ms.openlocfilehash: cd2d7537036978e30c97b5776155e429178b6dac
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/03/2018
---
# <a name="relativelayout"></a>RelativeLayout

[`RelativeLayout`](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/) [ `ViewGroup` ](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/)子を表示する[ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/)相対的な位置の要素。 位置、 [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) (左のか、または指定された要素の下など) の兄弟要素に対して相対的に指定するかに相対的に配置、 [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/) (などの領域整列センターの左下)。

A [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/)は入れ子になったユーザー インターフェイスのデザインを排除できるので非常に強力なユーティリティ[ `ViewGroup` ](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/)s。 自分で検索する場合は、いくつか使用して入れ子になった[ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)グループ、ことができます、1 つに置き換える[ `RelativeLayout`](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/)です。

という名前の新しいプロジェクトを開始**HelloRelativeLayout**です。

開く、 **Resources/Layout/Main.axml**ファイルし、次の挿入します。

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent">
    <TextView
        android:id="@+id/label"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:text="Type here:"/>
    <EditText
        android:id="@+id/entry"
        android:layout_width="fill_parent"
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

各に注意してください、`android:layout_*`など、属性`layout_below`、 `layout_alignParentRight`、および`layout_toLeftOf`です。
使用する場合、 [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/)、これらの属性を使用するにはそれぞれを配置する方法を説明する[ `View`](https://developer.xamarin.com/api/type/Android.Views.View/)です。 これらの属性のいずれかでは、別の種類の相対位置を定義します。 いくつかの属性は、兄弟のリソース ID を使用して[ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/)をそれ自体の相対位置を定義します。 たとえば、過去[ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/)を左の配置-と-、-最上位の線上に定義されて、 [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) ID によって識別される`ok`(以前は[`Button`](https://developer.xamarin.com/api/type/Android.Widget.Button/)).

定義されているすべての可能なレイアウト属性は[ `RelativeLayout.LayoutParams`](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout+LayoutParams/)です。

このレイアウトをロードすることを確認、 [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/)メソッド。

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

[ `SetContentView(int)` ](https://developer.xamarin.com/api/member/Android.App.Activity.SetContentView/p/System.Int32/)メソッドのレイアウト ファイルの読み込み、 [ `Activity`](https://developer.xamarin.com/api/type/Android.App.Activity/)リソース ID で指定された&mdash;`Resource.Layout.Main`を指す、**リソース/レイアウト/[Main.axml]** レイアウト ファイルです。

アプリケーションを実行します。 次のレイアウトが表示されます。

[![TextView、EditText、2 つのボタンとの相対的なレイアウトのスクリーン ショット](relative-layout-images/helloviews2.png)](relative-layout-images/helloviews2.png#lightbox)


## <a name="resources"></a>リソース

-   [`RelativeLayout`](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/)
-   [`RelativeLayout.LayoutParams`](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout+LayoutParams/)
-   [`TextView`](https://developer.xamarin.com/api/type/Android.Widget.TextView/)
-   [`EditText`](https://developer.xamarin.com/api/type/Android.Widget.EditText/)
-   [`Button`](https://developer.xamarin.com/api/type/Android.Widget.Button/)


*このページの部分は変更を作成し、Android のオープン ソース プロジェクトで共有しての条項に従って使用作業に基づく、*
[*クリエイティブ コモンズ 2.5 Attribution ライセンス*](http://creativecommons.org/licenses/by/2.5/).
