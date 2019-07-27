---
title: RadioButton
ms.prod: xamarin
ms.assetid: 3C32EA3F-D917-C988-72C5-A17354DA791E
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 2279282b08c9d97b239de424cf38aa6f1463dc4d
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510355"
---
# <a name="radiobutton"></a>RadioButton

このセクションでは、次を使用して相互に排他的な2つのラジオボタンを作成します (有効にするともう一方は無効になります)。[`RadioGroup`](xref:Android.Widget.RadioGroup)
そして[`RadioButton`](xref:Android.Widget.RadioButton)
ウィジェット. いずれかのオプションボタンを押すと、トーストメッセージが表示されます。


**Resources/layout/Main. axml**ファイルを開き、2つ[`RadioButton`](xref:Android.Widget.RadioButton)の[`RadioGroup`](xref:Android.Widget.RadioGroup)を追加します。その中[`LinearLayout`](xref:Android.Widget.LinearLayout)には、(内の) で入れ子になっています。

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

を[`RadioButton`](xref:Android.Widget.RadioButton)[要素`RadioGroup`](xref:Android.Widget.RadioGroup)ごとにグループ化して、一度に複数のを選択できないようにすることが重要です。 このロジックは、Android システムによって自動的に処理されます。 ある場合[`RadioButton`](xref:Android.Widget.RadioButton)
[グループ内] を選択すると、他のグループは自動的に選択解除されます。

各[`RadioButton`](xref:Android.Widget.RadioButton)を選択したときに何かを行うには、イベントハンドラーを記述する必要があります。

```csharp
private void RadioButtonClick (object sender, EventArgs e)
{
    RadioButton rb = (RadioButton)sender;
    Toast.MakeText (this, rb.Text, ToastLength.Short).Show ();
}
```

まず、渡された送信元は RadioButton にキャストされます。
次に、[`Toast`](xref:Android.Widget.Toast)
[メッセージ] 選択したラジオボタンのテキストを表示します。

次に、の下部にあります。[`OnCreate()`](xref:Android.App.Activity.OnCreate*)
次のように、メソッドを追加します。

```csharp
RadioButton radio_red = FindViewById<RadioButton>(Resource.Id.radio_red);
RadioButton radio_blue = FindViewById<RadioButton>(Resource.Id.radio_blue);

radio_red.Click += RadioButtonClick;
radio_blue.Click += RadioButtonClick;
```

これにより、 [`RadioButton`](xref:Android.Widget.RadioButton)レイアウトから各をキャプチャし、新しく作成されたイベントハンドラーをそれぞれに追加します。

アプリケーションを実行します。

> [!TIP]
> 状態を自分で変更する必要がある場合 (保存され[`CheckBoxPreference`](xref:Android.Preferences.CheckBoxPreference)たの読み込み時など) は、[`Checked`](xref:Android.Widget.CompoundButton.Checked)
> プロパティセッターまたは[`Toggle()`](xref:Android.Widget.CompoundButton.Toggle)
> メソッドをオーバーライドします。

*このページの一部は、Android オープンソースプロジェクトによって作成および共有*
され、[*Creative Commons 2.5 属性*](http://creativecommons.org/licenses/by/2.5/)で説明されている条項に従って使用される作業に基づいて変更されます。 
