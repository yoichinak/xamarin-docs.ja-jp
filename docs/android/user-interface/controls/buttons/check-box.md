---
title: CheckBox
ms.prod: xamarin
ms.assetid: A884AF10-D5EA-72CA-2301-B80CEC7FFBE7
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: b947217706fc8ef7ce7945bf4c88349f4367ffcd
ms.sourcegitcommit: cf56d2bae34dc0f8e94c2d3d28d5f460d59807bf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/13/2019
ms.locfileid: "70985671"
---
# <a name="checkbox"></a>CheckBox

このセクションでは、 [`CheckBox`](xref:Android.Widget.CheckBox)ウィジェットを使用して項目を選択するためのチェックボックスを作成します。 チェックボックスをオンにすると、トーストメッセージはチェックボックスの現在の状態を示します。

**Resources/layout/Main. axml**ファイルを開き、 [`CheckBox`](xref:Android.Widget.CheckBox) [`LinearLayout`](xref:Android.Widget.LinearLayout)要素 (内) を追加します。

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

これをキャプチャ、 [ `CheckBox` ](xref:Android.Widget.CheckBox)がレイアウトから要素をイベントを処理しますをクリック、チェック ボックスをクリックしたときにアクションを定義します。 クリックすると、 [`Checked`](xref:Android.Widget.CompoundButton.Checked)プロパティが呼び出され、チェックボックスの新しい状態がチェックされます。 このチェックボックスがオン[`Toast`](xref:Android.Widget.Toast)になっている場合は、"選択された" というメッセージが表示されます。それ以外の場合は、[未選択] と表示されます。 は[`CheckBox`](xref:Android.Widget.CheckBox)独自の状態変更を処理するため、現在の状態に対してのみクエリを実行する必要があります。

実行します。

> [!TIP]
> 状態を自分で変更する必要がある場合 (保存され[`CheckBoxPreference`](xref:Android.Preferences.CheckBoxPreference)たの読み込み時など) は、 [`Checked`](xref:Android.Widget.CompoundButton.Checked)プロパティ setter または[`Toggle()`](xref:Android.Widget.CompoundButton.Toggle)メソッドを使用します。

*このページの一部は、Android オープンソースプロジェクトによって作成および共有された作業に基づいて変更され、「* [*Creative Commons 2.5 属性ライセンス*](http://creativecommons.org/licenses/by/2.5/)。
