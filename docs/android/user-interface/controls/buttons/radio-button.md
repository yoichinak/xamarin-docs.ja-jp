---
title: RadioButton
ms.prod: xamarin
ms.assetid: 3C32EA3F-D917-C988-72C5-A17354DA791E
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: 9f51adcbd1accb4f780318cc0853e612ed8e5bed
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029299"
---
# <a name="radiobutton"></a>RadioButton

このセクションでは、2つの相互排他的なラジオボタンを作成し (有効にすると、もう一方を無効にする)、を使用して[`RadioGroup`](xref:Android.Widget.RadioGroup)
および[`RadioButton`](xref:Android.Widget.RadioButton)
ウィジェット. いずれかのオプションボタンを押すと、トーストメッセージが表示されます。

**Resources/layout/Main. axml**ファイルを開き、次の2つの[`RadioButton`](xref:Android.Widget.RadioButton)を追加します。これは、 [`LinearLayout`](xref:Android.Widget.LinearLayout)内の[`RadioGroup`](xref:Android.Widget.RadioGroup)に入れ子になっています。

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

[`RadioButton`](xref:Android.Widget.RadioButton)は、一度に1つ以上選択できるように、 [`RadioGroup`](xref:Android.Widget.RadioGroup)要素でグループ化されていることが重要です。 このロジックは、Android システムによって自動的に処理されます。 1 [`RadioButton`](xref:Android.Widget.RadioButton)
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
次に[`Toast`](xref:Android.Widget.Toast)
[メッセージ] 選択したラジオボタンのテキストを表示します。

次に、 [`OnCreate()`](xref:Android.App.Activity.OnCreate*)の下部にあります。
次のように、メソッドを追加します。

```csharp
RadioButton radio_red = FindViewById<RadioButton>(Resource.Id.radio_red);
RadioButton radio_blue = FindViewById<RadioButton>(Resource.Id.radio_blue);

radio_red.Click += RadioButtonClick;
radio_blue.Click += RadioButtonClick;
```

これにより、レイアウトから[`RadioButton`](xref:Android.Widget.RadioButton)の各がキャプチャされ、新しく作成されたイベントハンドラーがそれぞれに追加されます。

アプリケーションを実行します。

> [!TIP]
> 状態を自分で変更する必要がある場合 (保存されている[`CheckBoxPreference`](xref:Android.Preferences.CheckBoxPreference)を読み込む場合など) は、 [`Checked`](xref:Android.Widget.CompoundButton.Checked)を使用します。
> プロパティセッターまたは[`Toggle()`](xref:Android.Widget.CompoundButton.Toggle)
> メソッドをオーバーライドします。

*このページの一部は、Android オープンソースプロジェクトによって作成および共有*され、
[*Creative Commons 2.5 属性のライセンス*](https://creativecommons.org/licenses/by/2.5/)に記載されている条項に従って使用される作業に基づいて変更されます。 
