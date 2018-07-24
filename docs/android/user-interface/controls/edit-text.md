---
title: テキストの編集
ms.prod: xamarin
ms.assetid: E513BCBC-438E-15E8-B83A-4B768A8E8B32
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: d6be8ae1587742a8c2a37b22b2da3701187dde2a
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
ms.locfileid: "30774604"
---
# <a name="edit-text"></a>テキストの編集

このセクションで、ユーザー入力を使用するためのテキスト フィールドを作成するが、 [EditText](https://developer.xamarin.com/api/type/Android.Widget.EditText/)ウィジェット。 フィールドにテキストを入力すると、"Enter"キー トースト メッセージのテキストが表示されます。

開く、<code>Resources\layout\main.xml</code>ファイルを追加、 [EditText](https://developer.xamarin.com/api/type/Android.Widget.EditText/)要素 (内部、 [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/))。

```xml
<pre><code class=" syntax brush-C#">&lt;EditText
    android:id="@+id/edittext"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"/&gt;</code></pre>
```

ユーザー型がの末尾に次のコードを追加するテキストを操作する、 [OnCreate](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/)メソッド。

```csharp
EditText edittext = FindViewById<EditText>(Resource.Id.edittext);
edittext.KeyPress += (object sender, View.KeyEventArgs e) => {
 e.Handled = false;
 if (e.Event.Action == KeyEventActions.Down && e.KeyCode == Keycode.Enter) {
  Toast.MakeText (this, edittext.Text, ToastLength.Short).Show ();
  e.Handled = true;
         }
};
```

これをキャプチャ、 [ `EditText` ](https://developer.xamarin.com/api/type/Android.Widget.EditText/) 、レイアウトとセットから要素を[ `KeyPress` ](https://developer.xamarin.com/api/event/Android.Views.View.KeyPress/)ハンドラーで、ウィジェットにフォーカスがあるときにキーが押されたときにアクションを定義します。 ここでは、メソッドが (押されている) 場合は、Enter キーをリッスンし、ポップアップに定義されて、 [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/)が入力されたテキストでメッセージ。 [ `Handled` ](https://developer.xamarin.com/api/property/Android.Views.View+KeyEventArgs.Handled/)プロパティは常に`true`かどうか、イベントが処理されたイベントがバブルアップ (あるため、テキスト フィールドにキャリッジ リターン) できるようにします。

アプリケーションを実行します。

*このページの部分は、作成した作業に基づいて変更と* [ *Android のオープン ソース プロジェクトで共有される*](http://code.google.com/policies.html) *の条項に従って使用と* [ *クリエイティブ コモンズ 2.5 Attribution ライセンス*](http://creativecommons.org/licenses/by/2.5/) *です。このチュートリアルがに基づいて、* [ *Android フォーム Stuff チュートリアル*](http://developer.android.com/resources/tutorials/views/hello-formstuff.html) *です。*



## <a name="related-links"></a>関連リンク

- [EditTextSample](https://developer.xamarin.com/samples/monodroid/UserInterface/EditTextSample/)
