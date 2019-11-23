---
title: カスタム ボタン
ms.prod: xamarin
ms.assetid: C523D41E-5855-248D-079D-6B12B74B7617
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: d85c67cf18c61af04cf12bfab58a5b516d380f62
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029354"
---
# <a name="custom-button"></a>カスタム ボタン

このセクションでは、 [`Button`](xref:Android.Widget.Button)ウィジェットと、さまざまなボタンの状態に使用する3つの異なるイメージを定義する XML ファイルを使用して、テキストではなくカスタムイメージを含むボタンを作成します。 このボタンが押されると、短いメッセージが表示されます。

次の3つのイメージを右クリックしてダウンロードし、プロジェクトの**Resources/** 作成ディレクトリにコピーします。 これらは、さまざまなボタンの状態に使用されます。

 [通常の状態の緑色の android アイコン](custom-button-images/android-normal.png)](custom-button-images/android-normal.png#lightbox) [![フォーカス状態の Android アイコンがオレンジ色](custom-button-images/android-focused.png)](custom-button-images/android-focused.png#lightbox)に[![黄色の android](custom-button-images/android-pressed.png)](custom-button-images/android-pressed.png#lightbox)アイコンが押された状態であることを示す![

**Android_button**という名前の**リソース/** 作成されたディレクトリに新しいファイルを作成します。 次の XML を挿入します。

```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:drawable="@drawable/android_pressed"
          android:state_pressed="true" />
    <item android:drawable="@drawable/android_focused"
          android:state_focused="true" />
    <item android:drawable="@drawable/android_normal" />
</selector>
```

これにより、1つの描画リソースが定義されます。これにより、ボタンの現在の状態に基づいてイメージが変更されます。 最初の `<item>` は、ボタンが押されたときにイメージとして android_pressed を定義し**ます**(アクティブ化されています)。2番目の `<item>` は、ボタンがフォーカスされたときに画像として**android_focused .png**を定義します (ボタンがトラックボールまたは方向パッドを使用して強調表示されている場合)。3番目の `<item>` は、通常の状態 (押されていない場合またはフォーカスされていない場合) のイメージとして**android_normal .png**を定義します。 この XML ファイルは、1つの作成されたリソースを表し、背景の[`Button`](xref:Android.Widget.Button)によって参照されると、表示されるイメージはこれら3つの状態に基づいて変化します。

> [!NOTE]
> `<item>` 要素の順序は重要です。 この描画が参照されている場合、`<item>`s は、現在のボタンの状態に適しているかどうかを判断するために走査されます。
> "通常の" イメージは最後のものであるため、条件 `android:state_pressed` と `android:state_focused` 評価が false の場合にのみ適用されます。

**Resources/layout/Main. axml**ファイルを開き、 [`Button`](xref:Android.Widget.Button)要素を追加します。

```xml
<Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:padding="10dp"
        android:background="@drawable/android_button" />
```

`android:background` 属性は、ボタンの背景に使用する、描画できるリソースを指定します (これは、 **Resources/** `@drawable/android`で保存されている場合、または config.xml として参照されます)。 これにより、システム全体のボタンに使用される通常の背景イメージが置き換えられます。 描画用のイメージをボタンの状態に基づいて変更するには、イメージを背景に適用する必要があります。

ボタンが押されたときに実行されるようにするには、の末尾に次のコードを追加し[`OnCreate()`](xref:Android.App.Activity.OnCreate*)
b

```csharp
Button button = FindViewById<Button>(Resource.Id.button);

button.Click += (o, e) => {
    Toast.MakeText (this, "Beep Boop", ToastLength.Short).Show ();
};
```

これにより、レイアウトから[`Button`](xref:Android.Widget.Button)がキャプチャされ、 [`Button`](xref:Android.Widget.Button)をクリックしたときに表示される[`Toast`](xref:Android.Widget.Toast)メッセージが追加されます。

次に、アプリケーションを実行します。

*このページの一部は、Android オープンソースプロジェクトによって作成および共有*され、
[*Creative Commons 2.5 属性のライセンス*](https://creativecommons.org/licenses/by/2.5/)に記載されている条項に従って使用される作業に基づいて変更されます。
