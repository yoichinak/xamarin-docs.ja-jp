---
title: テキストの編集
description: EditText ウィジェットを使用してユーザー入力を受け入れる方法。
ms.prod: xamarin
ms.assetid: E513BCBC-438E-15E8-B83A-4B768A8E8B32
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/09/2018
ms.openlocfilehash: 6180896002d19c51bce47bf53aaecdc11b0cae6e
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725150"
---
# <a name="xamarinandroid-edit-text"></a>Xamarin Android のテキストの編集

このセクションでは、 [EditText](xref:Android.Widget.EditText)ウィジェットを使用して、ユーザー入力用のテキストフィールドを作成します。 フィールドにテキストが入力されると、Enter キーを**押し**て、トーストメッセージにテキストが表示されます。

**Resources/layout/activity_main**を開き、 [EditText](xref:Android.Widget.EditText)要素を含んでいるレイアウトに追加します。 次の例**activity_main。 axml**には、`LinearLayout`に追加された `EditText` があります。

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

このコード例では、`EditText` 属性 `android:imeOptions` が `actionGo`に設定されています。 この設定では、 **Enter**キーをタップすると `KeyPress` 入力ハンドラーがトリガーされるように、[既定の完了] アクションを[[](https://developer.android.com/reference/android/view/inputmethod/EditorInfo#IME_ACTION_GO) [実行](https://developer.android.com/reference/android/view/inputmethod/EditorInfo#IME_ACTION_DONE)] アクションに変更します。
(通常は、`actionGo` を使用し**て、Enter**キーによって、に入力された URL のターゲットにユーザーが移動するようにします)。

ユーザーテキスト入力を処理するには、 **MainActivity.cs**の[OnCreate](xref:Android.App.Activity.OnCreate*)メソッドの末尾に次のコードを追加します。

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

さらに、次の `using` ステートメントを**MainActivity.cs**の先頭に追加します (まだ存在していない場合)。

```csharp
using Android.Views;
```

このコード例では、レイアウトから[EditText](xref:Android.Widget.EditText)要素を増えし、ウィジェットにフォーカスがあるときにキーが押されたときに実行されるアクションを定義する[KeyPress](xref:Android.Views.View.KeyPress)ハンドラーを追加します。 この場合、メソッドは**enter**キーをリッスンするように定義されています (タップした場合)。その後、入力されたテキストを含む[トースト](xref:Android.Widget.Toast)メッセージをポップアップ表示します。 イベントが処理された場合は、[処理](xref:Android.Views.View.KeyEventArgs.Handled)済みのプロパティを常に `true` する必要があることに注意してください。 これは、イベントがバブルアップされないようにするために必要です (これにより、テキストフィールドに復帰が返されます)。

アプリケーションを実行し、テキストフィールドにテキストを入力します。 **Enter キーを**押すと、右側に表示されるようにトーストが表示されます。

[EditText にテキストを入力する例の ![](edit-text-images/edit-text-sml.png)](edit-text-images/edit-text.png#lightbox)

*このページの一部は、Android オープンソースプロジェクトによって作成および共有され、Creative Commons 2.5 属性で説明されている条項に従って使用される作業に基づいて変更され* [*Creative Commons 2.5 Attribution License*](https://creativecommons.org/licenses/by/2.5/) *ます。このチュートリアルは、Android フォームに関するチュートリアルに基づいて* [*Android Form Stuff tutorial*](https://developer.android.com/resources/tutorials/views/hello-formstuff.html)い*ます。*

## <a name="related-links"></a>関連リンク

- [EditTextSample](https://docs.microsoft.com/samples/xamarin/monodroid-samples/userinterface-edittextsample)
