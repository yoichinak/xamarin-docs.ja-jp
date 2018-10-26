---
title: RadioButton
ms.prod: xamarin
ms.assetid: 3C32EA3F-D917-C988-72C5-A17354DA791E
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: be473580b24dba6b4f08384771e2097d368f8dc8
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50123597"
---
# <a name="radiobutton"></a>RadioButton

このセクションでは作成する (有効にする 1 つを使うと、他の無効に) 2 つの相互に排他的なラジオ ボタンを使用して、 [`RadioGroup`](https://developer.xamarin.com/api/type/Android.Widget.RadioGroup/)
そして [`RadioButton`](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/)
ウィジェット。 いずれかのラジオ ボタンが押されたときに、トースト メッセージが表示されます。


開く、 **Resources/layout/Main.axml**ファイルし、2 つ追加[ `RadioButton`](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/)で入れ子になった、s、 [ `RadioGroup` ](https://developer.xamarin.com/api/type/Android.Widget.RadioGroup/) (内で、 [ `LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)):

```xml
<RadioGroup
  android:layout_width="fill_parent"
  android:layout_height="wrap_content"
  android:orientation="vertical">
  <RadioButton android:id="@+id/radio_red"
      android:layout_width="wrap_content"
      android:layout_height="wrap_content"
      android:text="Red" />
  <RadioButton android:id="@+id/radio_blue"
      android:layout_width="wrap_content"
      android:layout_height="wrap_content"
      android:text="Blue" />
</RadioGroup>
```

重要ですが、 [ `RadioButton` ](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/)s をによってグループ化、 [ `RadioGroup` ](https://developer.xamarin.com/api/type/Android.Widget.RadioGroup/)要素、一度に 1 つ選択できるようにします。 このロジックは、Android のシステムによって自動的に処理されます。 1 つの場合 [`RadioButton`](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/)
内でグループを選択すると、その他すべてが自動的に選択が解除されます。

作業を行うときに各[ `RadioButton` ](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/)は選択すると、私たちは、イベント ハンドラーを記述する必要があります。

```csharp
private void RadioButtonClick (object sender, EventArgs e)
{
    RadioButton rb = (RadioButton)sender;
    Toast.MakeText (this, rb.Text, ToastLength.Short).Show ();
}
```

最初に、渡される送信者は、RadioButton にキャストされます。
、 [`Toast`](https://developer.xamarin.com/api/type/Android.Widget.Toast/)
メッセージには、選択したラジオ ボタンのテキストが表示されます。

ここで、下部にある、 [`OnCreate()`](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/Android.OS.PersistableBundle)
メソッドは、以下を追加します。

```csharp
RadioButton radio_red = FindViewById<RadioButton>(Resource.Id.radio_red);
RadioButton radio_blue = FindViewById<RadioButton>(Resource.Id.radio_blue);

radio_red.Click += RadioButtonClick;
radio_blue.Click += RadioButtonClick;
```

各キャプチャ、 [ `RadioButton`](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/)レイアウトから新しく作成されたイベント handlerto を追加します。

アプリケーションを実行します。

**ヒント:** 自分で状態を変更する必要がある場合 (場合など、保存された読み込み[ `CheckBoxPreference` ](https://developer.xamarin.com/api/type/Android.Preferences.CheckBoxPreference/)) を使用して、 [`Checked`](https://developer.xamarin.com/api/property/Android.Widget.CompoundButton.Checked/)
プロパティ set アクセス操作子または [`Toggle()`](https://developer.xamarin.com/api/member/Android.Widget.CompoundButton.Toggle/)
メソッドをオーバーライドします。

*このページの部分が作成および Android のオープン ソース プロジェクトで共有し、の条項に従って使用作業に基づいた変更、*
[*Creative Commons 2.5 Attribution License*](http://creativecommons.org/licenses/by/2.5/). 
