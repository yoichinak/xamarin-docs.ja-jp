---
title: CheckBox
ms.prod: xamarin
ms.assetid: A884AF10-D5EA-72CA-2301-B80CEC7FFBE7
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: e03595e8d88a2f12341b9e339d0581c631224848
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50120802"
---
# <a name="checkbox"></a>CheckBox

このセクションでは作成する項目を選択するためのチェック ボックスを使用して、 [`CheckBox`](https://developer.xamarin.com/api/type/Android.Widget.CheckBox)
ウィジェット。 チェック ボックスが押されたときに、トースト メッセージは、チェック ボックスの現在の状態を示します。

開く、 **Resources/layout/Main.axml**追加ファイルを開き、 [ `CheckBox` ](https://developer.xamarin.com/api/type/Android.Widget.CheckBox/)要素 (内部、 [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout))。

```xml
<CheckBox android:id="@+id/checkbox"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="check it out" />
```

末尾に次のコードを追加を行うには何か、状態が変更されたときに、 [`OnCreate()`](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/Android.OS.PersistableBundle)
方法:

```csharp
CheckBox checkbox = FindViewById<CheckBox>(Resource.Id.checkbox);

checkbox.Click += (o, e) => {
    if (checkbox.Checked)
        Toast.MakeText (this, "Selected", ToastLength.Short).Show ();
    else
        Toast.MakeText (this, "Not selected", ToastLength.Short).Show ();
};
```

これをキャプチャします [`CheckBox`](https://developer.xamarin.com/api/type/Android.Widget.CheckBox/)
レイアウトからの要素は、チェック ボックスがクリックされたときにアクションを定義、Click イベントを処理します。 クリックすると、 [`Checked`](https://developer.xamarin.com/api/property/Android.Widget.CompoundButton.Checked/)
プロパティは、チェック ボックスの新しい状態を確認すると呼ばれます。 チェックされている場合、 [`Toast`](https://developer.xamarin.com/api/type/Android.Widget.Toast/)
「選択済み」、それ以外の場合「オフ」が表示されますが、メッセージが表示されます。 、 [`CheckBox`](https://developer.xamarin.com/api/type/Android.Widget.CheckBox/)
現在の状態をクエリするだけで済みますので、独自の状態の変更を処理します。

これを実行します。

**ヒント:** 自分で状態を変更する必要がある場合 (場合など、保存された読み込み[ `CheckBoxPreference`](https://developer.xamarin.com/api/type/Android.Preferences.CheckBoxPreference)を使用して、 [`Checked`](https://developer.xamarin.com/api/property/Android.Widget.CompoundButton.Checked)
プロパティ set アクセス操作子または [`Toggle()`](https://developer.xamarin.com/api/member/Android.Widget.CompoundButton.Toggle)
メソッドをオーバーライドします。

*このページの部分が作成および Android のオープン ソース プロジェクトで共有し、の条項に従って使用作業に基づいた変更、*
[*Creative Commons 2.5 Attribution License*](http://creativecommons.org/licenses/by/2.5/).
