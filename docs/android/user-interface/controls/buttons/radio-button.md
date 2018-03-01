---
title: RadioButton
ms.topic: article
ms.prod: xamarin
ms.assetid: 3C32EA3F-D917-C988-72C5-A17354DA791E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: c4f4909813f5c82a49ec51278b3b50cc36a8e17b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="radiobutton"></a>RadioButton

このセクションでは作成する (有効にする 1 つの無効化、他の) 2 つの相互に排他的なラジオ ボタンを使用して、 [ `RadioGroup` ](https://developer.xamarin.com/api/type/Android.Widget.RadioGroup/)と[ `RadioButton` ](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/)ウィジェット。 いずれかのラジオ ボタンが押されたときに通知メッセージが表示されます。


開く、 **Resources/layout/Main.axml**ファイルし、2 つ追加[ `RadioButton`](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/)で入れ子にされた、s、 [ `RadioGroup` ](https://developer.xamarin.com/api/type/Android.Widget.RadioGroup/) (内部、 [ `LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)):

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

重要です、 [ `RadioButton` ](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/)s はグループ化によって、 [ `RadioGroup` ](https://developer.xamarin.com/api/type/Android.Widget.RadioGroup/)要素、一度に 1 つ選択できるようにします。 このロジックは、Android のシステムによって自動的に処理されます。 ときに 1 つ[ `RadioButton` ](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/)内でグループを選択すると、他はすべて自動的に解除します。

何かを行うときに各[ `RadioButton` ](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/)は選択すると、私たちは、イベント ハンドラーを記述する必要があります。

```csharp
private void RadioButtonClick (object sender, EventArgs e)
{
    RadioButton rb = (RadioButton)sender;
    Toast.MakeText (this, rb.Text, ToastLength.Short).Show ();
}
```

最初に、渡される送信者は、RadioButton にキャストされます。
[ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/)メッセージには、選択したオプション ボタンのテキストが表示されます。

ここで、下部、 [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/Android.OS.PersistableBundle)メソッド、次の追加。

```csharp
RadioButton radio_red = FindViewById<RadioButton>(Resource.Id.radio_red);
RadioButton radio_blue = FindViewById<RadioButton>(Resource.Id.radio_blue);

radio_red.Click += RadioButtonClick;
radio_blue.Click += RadioButtonClick;
```

これは、それぞれのキャプチャ、 [ `RadioButton`](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/)レイアウトから s し、新しく作成されたイベント handlerto を追加します。

アプリケーションを実行します。

**ヒント:**状態を変更する必要がある場合 (場合など、保存された読み込み[ `CheckBoxPreference` ](https://developer.xamarin.com/api/type/Android.Preferences.CheckBoxPreference/))、使用、 [ `Checked` ](https://developer.xamarin.com/api/property/Android.Widget.CompoundButton.Checked/)プロパティ set アクセス操作子または[ `Toggle()`](https://developer.xamarin.com/api/member/Android.Widget.CompoundButton.Toggle/)メソッドです。

*このページの部分は変更を作成し、Android のオープン ソース プロジェクトで共有しての条項に従って使用作業に基づく、*
[*クリエイティブ コモンズ 2.5 Attribution ライセンス*](http://creativecommons.org/licenses/by/2.5/). 
