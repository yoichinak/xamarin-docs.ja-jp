---
title: テキストの編集
description: EditText ウィジェットを使用して、ユーザー入力をそのまま使用する方法。
ms.prod: xamarin
ms.assetid: E513BCBC-438E-15E8-B83A-4B768A8E8B32
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/09/2018
ms.openlocfilehash: d42cec1ee0939bead9ede83a042f5b6cbb5298cd
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57670871"
---
# <a name="edit-text"></a>テキストの編集

このセクションでは使用して、 [EditText](https://developer.xamarin.com/api/type/Android.Widget.EditText/)ウィジェットをユーザーの入力テキスト フィールドを作成します。 フィールドにテキストを入力した後、 **Enter**キー トースト メッセージにテキストが表示されます。

開いている**Resources/layout/activity_main.axml**を追加し、 [EditText](https://developer.xamarin.com/api/type/Android.Widget.EditText/)要素を含むレイアウトにします。 次の例では、 **activity_main.axml**が、`EditText`に追加されている、 `LinearLayout`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <EditText
        android:id="@+id/edittext"
        android:layout_width="match_parent"
        android:imeOptions="actionGo"
        android:inputType="text"
        android:layout_height="wrap_content" />
</LinearLayout>
```

このコード例では、`EditText`属性`android:imeOptions`に設定されている`actionGo`します。 この設定は、既定値を変更[完了](https://developer.android.com/reference/android/view/inputmethod/EditorInfo#IME_ACTION_DONE)アクションを[移動](https://developer.android.com/reference/android/view/inputmethod/EditorInfo#IME_ACTION_GO)アクションをタップするように、 **」と入力**トリガーをキー、`KeyPress`入力ハンドラー。
(通常、`actionGo`されるように、 **」と入力**キーは、ユーザーに型指定された URL のターゲットにします)。

ユーザーのテキスト入力を処理するための末尾に次のコードを追加、 [OnCreate](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/)メソッド**MainActivity.cs**:

```csharp
EditText edittext = FindViewById<EditText>(Resource.Id.edittext);
edittext.KeyPress += (object sender, View.KeyEventArgs e) => {
    e.Handled = false;
    if (e.Event.Action == KeyEventActions.Down && e.KeyCode == Keycode.Enter) 
    {
        Toast.MakeText(this, edittext.Text, ToastLength.Short).Show();
        e.Handled = true;
    }
};
```

さらに、次を追加`using`ステートメントの先頭に**MainActivity.cs**が存在しない場合。

```csharp
using Android.Views;
```

このコード例の拡張、 [EditText](https://developer.xamarin.com/api/type/Android.Widget.EditText/)レイアウトから要素を追加し、 [KeyPress](https://developer.xamarin.com/api/event/Android.Views.View.KeyPress/)ウィジェットにフォーカスがあるキーが押されたときにアクションを定義するハンドラー。 リッスンするように、メソッドが定義されているこの場合、 **」と入力**(タップ) する場合のキーし、ポップアップ、[トースト](https://developer.xamarin.com/api/type/Android.Widget.Toast/)のメッセージに入力されています。 なお、 [Handled](https://developer.xamarin.com/api/property/Android.Views.View+KeyEventArgs.Handled/)プロパティは常に`true`イベントが処理された場合。 これは、イベントのバブリングを防ぐために必要です (キャリッジ リターン、テキスト フィールドになります) がアップします。

アプリケーションを実行し、テキスト フィールドにテキストを入力します。 押したときに、 **」と入力**キー、トーストが表示されます、右に示すようにします。

[![EditText にテキストを入力の例](edit-text-images/edit-text-sml.png)](edit-text-images/edit-text.png#lightbox)

*このページの部分が作成された作業に基づいた変更と* [ *Android オープン ソース プロジェクトで共有*](http://code.google.com/policies.html) *で説明されている条項にしたがって使用* [ *2.5 の creative Commons Attribution License* ](http://creativecommons.org/licenses/by/2.5/) *します。このチュートリアルがに基づいて、* [ *フォーム Stuff の Android チュートリアル*](https://developer.android.com/resources/tutorials/views/hello-formstuff.html) *します。*


## <a name="related-links"></a>関連リンク

- [EditTextSample](https://developer.xamarin.com/samples/monodroid/UserInterface/EditTextSample/)
