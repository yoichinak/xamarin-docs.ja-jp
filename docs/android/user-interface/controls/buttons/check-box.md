---
title: CheckBox
ms.prod: xamarin
ms.assetid: A884AF10-D5EA-72CA-2301-B80CEC7FFBE7
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: f6f594d86cab8b1173ee9f67402862e1ec2890b2
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510373"
---
# <a name="checkbox"></a>CheckBox

このセクションでは、次を使用して項目を選択するためのチェックボックスを作成します。[`CheckBox`](xref:Android.Widget.CheckBox)
ウィジェット. チェックボックスをオンにすると、トーストメッセージはチェックボックスの現在の状態を示します。

**Resources/layout/Main. axml**ファイルを開き、 [`CheckBox`](xref:Android.Widget.CheckBox) [`LinearLayout`](xref:Android.Widget.LinearLayout)要素 (内) を追加します。

```xml
<CheckBox android:id="@+id/checkbox"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="check it out" />
```

状態が変更されたときに操作を行うには、の末尾に次のコードを追加します。[`OnCreate()`](xref:Android.App.Activity.OnCreate*)
b

```csharp
CheckBox checkbox = FindViewById<CheckBox>(Resource.Id.checkbox);

checkbox.Click += (o, e) => {
    if (checkbox.Checked)
        Toast.MakeText (this, "Selected", ToastLength.Short).Show ();
    else
        Toast.MakeText (this, "Not selected", ToastLength.Short).Show ();
};
```

これにより、[`CheckBox`](xref:Android.Widget.CheckBox)
次に、レイアウトの要素を使用して、クリックイベントを処理します。このイベントは、チェックボックスがクリックされたときに実行されるアクションを定義します。 クリックすると、[`Checked`](xref:Android.Widget.CompoundButton.Checked)
プロパティは、チェックボックスの新しい状態を確認するために呼び出されます。 このチェックボックスがオンになっている場合は、[`Toast`](xref:Android.Widget.Toast)
"選択された" というメッセージが表示されます。それ以外の場合は、"未選択" と表示されます。 、[`CheckBox`](xref:Android.Widget.CheckBox)
独自の状態変更を処理するため、現在の状態を照会するだけで済みます。

実行します。

> [!TIP]
> 状態を自分で変更する必要がある場合 (保存され[`CheckBoxPreference`](xref:Android.Preferences.CheckBoxPreference)たの読み込み時など) は、[`Checked`](xref:Android.Widget.CompoundButton.Checked)
> プロパティセッターまたは[`Toggle()`](xref:Android.Widget.CompoundButton.Toggle)
> メソッドをオーバーライドします。

*このページの一部は、Android オープンソースプロジェクトによって作成および共有*
され、[*Creative Commons 2.5 属性*](http://creativecommons.org/licenses/by/2.5/)で説明されている条項に従って使用される作業に基づいて変更されます。
