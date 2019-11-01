---
title: CheckBox
ms.prod: xamarin
ms.assetid: A884AF10-D5EA-72CA-2301-B80CEC7FFBE7
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: 06908ad8993eb6d47476006b23865fa1c7fe694f
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029342"
---
# <a name="checkbox"></a>CheckBox

このセクションでは、 [`CheckBox`](xref:Android.Widget.CheckBox)ウィジェットを使用して項目を選択するためのチェックボックスを作成します。 チェックボックスをオンにすると、トーストメッセージはチェックボックスの現在の状態を示します。

**Resources/layout/Main. axml**ファイルを開き、 [`CheckBox`](xref:Android.Widget.CheckBox)要素 ( [`LinearLayout`](xref:Android.Widget.LinearLayout)内) を追加します。

```xml
<CheckBox android:id="@+id/checkbox"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="check it out" />
```

状態が変更されたときに操作を実行するには、 [`OnCreate()`](xref:Android.App.Activity.OnCreate*)メソッドの末尾に次のコードを追加します。

```csharp
CheckBox checkbox = FindViewById<CheckBox>(Resource.Id.checkbox);

checkbox.Click += (o, e) => {
    if (checkbox.Checked)
        Toast.MakeText (this, "Selected", ToastLength.Short).Show ();
    else
        Toast.MakeText (this, "Not selected", ToastLength.Short).Show ();
};
```

これにより、レイアウトから[`CheckBox`](xref:Android.Widget.CheckBox)要素がキャプチャされ、クリックイベントが処理されます。これは、チェックボックスがクリックされたときに実行されるアクションを定義します。 クリックすると、チェックボックスの新しい状態を確認するために[`Checked`](xref:Android.Widget.CompoundButton.Checked)プロパティが呼び出されます。 このチェックボックスがオンの場合[`Toast`](xref:Android.Widget.Toast) 、メッセージ "selected" が表示されます。それ以外の場合は、"選択されていません" と表示されます。 [`CheckBox`](xref:Android.Widget.CheckBox)は独自の状態変更を処理するため、現在の状態に対してのみクエリを実行する必要があります。

実行します。

> [!TIP]
> 状態を自分で変更する必要がある場合 (保存されている[`CheckBoxPreference`](xref:Android.Preferences.CheckBoxPreference)を読み込む場合など) は、 [`Checked`](xref:Android.Widget.CompoundButton.Checked)プロパティ setter または[`Toggle()`](xref:Android.Widget.CompoundButton.Toggle)メソッドを使用します。

*このページの一部は、Android オープンソースプロジェクトによって作成および共有され、[*Creative Commons 2.5 属性*](https://creativecommons.org/licenses/by/2.5/)で説明されている条項に従って使用される作業に基づいて変更され* ます。
