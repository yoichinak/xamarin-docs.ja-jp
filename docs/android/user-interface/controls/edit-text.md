---
title: テキストの編集
description: EditText ウィジェットを使用してユーザー入力を受け入れる方法。
ms.prod: xamarin
ms.assetid: E513BCBC-438E-15E8-B83A-4B768A8E8B32
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/09/2018
ms.openlocfilehash: fb29478791c97028ff4d62f97922c672f7a8b17b
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91457259"
---
# <a name="xamarinandroid-edit-text"></a>Xamarin Android のテキストの編集

このセクションでは、 [EditText](xref:Android.Widget.EditText) ウィジェットを使用して、ユーザー入力用のテキストフィールドを作成します。 フィールドにテキストが入力されると、Enter キーを **押し** て、トーストメッセージにテキストが表示されます。

**Resources/layout/activity_main**を開き、 [EditText](xref:Android.Widget.EditText)要素を含んでいるレイアウトに追加します。 次の例では **activity_main ます。 axml** には `EditText` 、に追加されたが `LinearLayout` あります。

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

このコード例では、 `EditText` 属性 `android:imeOptions` はに設定されて `actionGo` います。 この設定は、 **Enter**キーをタップして入力ハンドラーをトリガーするように、 [[](https://developer.android.com/reference/android/view/inputmethod/EditorInfo#IME_ACTION_GO)既定の完了] アクションを [[実行](https://developer.android.com/reference/android/view/inputmethod/EditorInfo#IME_ACTION_DONE)] アクションに変更し `KeyPress` ます。
(通常 `actionGo` は、 **Enter** キーがで入力された URL のターゲットに移動するために使用されます)。

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

さらに、次の `using` ステートメントを **MainActivity.cs** の先頭に追加します (まだ存在していない場合)。

```csharp
using Android.Views;
```

このコード例では、レイアウトから [EditText](xref:Android.Widget.EditText) 要素を増えし、ウィジェットにフォーカスがあるときにキーが押されたときに実行されるアクションを定義する [KeyPress](xref:Android.Views.View.KeyPress) ハンドラーを追加します。 この場合、メソッドは **enter** キーをリッスンするように定義されています (タップした場合)。その後、入力されたテキストを含む [トースト](xref:Android.Widget.Toast) メッセージをポップアップ表示します。 [Handled](xref:Android.Views.View.KeyEventArgs.Handled) `true` イベントが処理された場合、処理済みのプロパティは常ににする必要があることに注意してください。 これは、イベントがバブルアップされないようにするために必要です (これにより、テキストフィールドに復帰が返されます)。

アプリケーションを実行し、テキストフィールドにテキストを入力します。 **Enter キーを**押すと、右側に表示されるようにトーストが表示されます。

[![EditText にテキストを入力する例](edit-text-images/edit-text-sml.png)](edit-text-images/edit-text.png#lightbox)

*このページの一部は、Android オープンソースプロジェクトによって作成および共有され、Creative Commons 2.5 属性で説明されている条項に従って使用される作業に基づいて変更され* [*Creative Commons 2.5 Attribution License*](https://creativecommons.org/licenses/by/2.5/) *ます。このチュートリアルは、Android フォームに関するチュートリアルに基づいて* [*Android Form Stuff tutorial*](https://developer.android.com/resources/tutorials/views/hello-formstuff.html)い*ます。*

## <a name="related-links"></a>関連リンク

- [EditTextSample](/samples/xamarin/monodroid-samples/userinterface-edittextsample)