---
title: CheckBox
ms.prod: xamarin
ms.assetid: A884AF10-D5EA-72CA-2301-B80CEC7FFBE7
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 85c505d03e7a763b24fb176b6a94c0fe43009b79
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
ms.locfileid: "30765754"
---
# <a name="checkbox"></a>CheckBox

このセクションでは作成する、項目を選択するためのチェック ボックスを使用して、 [ `CheckBox` ](https://developer.xamarin.com/api/type/Android.Widget.CheckBox)ウィジェット。 チェック ボックスが押されたときに通知メッセージは、チェック ボックスの現在の状態を示します。

開く、 **Resources/layout/Main.axml**ファイルを追加、 [ `CheckBox` ](https://developer.xamarin.com/api/type/Android.Widget.CheckBox/)要素 (内部、 [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout))。

```xml
<CheckBox android:id="@+id/checkbox"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="check it out" />
```

状態が変更されたときに何かには、末尾に次のコードを追加、 [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/Android.OS.PersistableBundle)メソッド。

```csharp
CheckBox checkbox = FindViewById<CheckBox>(Resource.Id.checkbox);

checkbox.Click += (o, e) => {
    if (checkbox.Checked)
        Toast.MakeText (this, "Selected", ToastLength.Short).Show ();
    else
        Toast.MakeText (this, "Not selected", ToastLength.Short).Show ();
};
```

これをキャプチャ、 [ `CheckBox` ](https://developer.xamarin.com/api/type/Android.Widget.CheckBox/)がレイアウトから要素をイベントを処理しますをクリック、チェック ボックスをクリックしたときにアクションを定義します。 クリックすると、 [ `Checked` ](https://developer.xamarin.com/api/property/Android.Widget.CompoundButton.Checked/)チェック ボックスの新しい状態を確認するプロパティが呼び出されます。 チェックされている場合、 [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) 「選択」、それ以外の場合「選択されていません」が表示されますが、メッセージが表示されます。 [ `CheckBox` ](https://developer.xamarin.com/api/type/Android.Widget.CheckBox/)だけ現在の状態を照会する必要があるため、独自の状態の変更を処理します。

これを実行します。

**ヒント:** 状態を変更する必要がある場合 (場合など、保存された読み込み[ `CheckBoxPreference`](https://developer.xamarin.com/api/type/Android.Preferences.CheckBoxPreference)を使用して、 [ `Checked` ](https://developer.xamarin.com/api/property/Android.Widget.CompoundButton.Checked)プロパティ set アクセス操作子または[ `Toggle()`](https://developer.xamarin.com/api/member/Android.Widget.CompoundButton.Toggle)メソッドです。

*このページの部分は変更を作成し、Android のオープン ソース プロジェクトで共有しての条項に従って使用作業に基づく、*
[*クリエイティブ コモンズ 2.5 Attribution ライセンス*](http://creativecommons.org/licenses/by/2.5/).
