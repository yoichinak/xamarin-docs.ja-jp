---
title: カスタム ボタン
ms.prod: xamarin
ms.assetid: C523D41E-5855-248D-079D-6B12B74B7617
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 4504045eb1692d95ee1e981bbec3da3a45699db3
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70758926"
---
# <a name="custom-button"></a>カスタム ボタン

このセクションでは、テキストではなくカスタムイメージを使用するボタンを作成します[`Button`](xref:Android.Widget.Button) 。ウィジェットと、さまざまなボタンの状態に使用する3つの異なるイメージを定義する XML ファイルを使用します。 このボタンが押されると、短いメッセージが表示されます。

次の3つのイメージを右クリックしてダウンロードし、プロジェクトの**Resources/** 作成ディレクトリにコピーします。 これらは、さまざまなボタンの状態に使用されます。

 通常の[ ![](custom-button-images/android-pressed.png)](custom-button-images/android-pressed.png#lightbox)状態[ ![](custom-button-images/android-normal.png)](custom-button-images/android-normal.png#lightbox) [の緑色のandroidアイコンフォーカス状態![用の android アイコン](custom-button-images/android-focused.png)](custom-button-images/android-focused.png#lightbox)押された状態の黄色の android アイコン

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

これにより、1つの描画リソースが定義されます。これにより、ボタンの現在の状態に基づいてイメージが変更されます。 最初`<item>`のは、ボタンが押されたときにイメージとして**android_pressed**を定義します (アクティブ化`<item>`されています)。2番目のは、ボタンがフォーカスされたときにイメージとして**android_focused**を定義します (ボタンがトラックボールまたは方向パッドを使用して強調表示);3番目`<item>`のは、通常の状態 (押されていない場合またはフォーカスされていない場合) のイメージとして**android_normal**を定義します。 この XML ファイルは、 [`Button`](xref:Android.Widget.Button) 1 つの作成されたリソースを表し、によってバックグラウンドで参照されるときに、この3つの状態に基づいて表示されるイメージを変更します。

> [!NOTE]
> `<item>`要素の順序は重要です。 この描画が参照されて`<item>`いる場合、現在のボタンの状態に適しているかどうかを判断するために、が走査されます。
> "通常の" イメージは最後にあるため、条件`android:state_pressed`とが両方と`android:state_focused`も false を評価した場合にのみ適用されます。

**Resources/layout/Main. axml**ファイルを開き、要素を[`Button`](xref:Android.Widget.Button)追加します。

```xml
<Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:padding="10dp"
        android:background="@drawable/android_button" />
```

属性`android:background`は、ボタンの背景に使用する、描画できるリソースを指定します。これは、**リソース/描画元/android .xml**で`@drawable/android`保存したときに、として参照されます。 これにより、システム全体のボタンに使用される通常の背景イメージが置き換えられます。 描画用のイメージをボタンの状態に基づいて変更するには、イメージを背景に適用する必要があります。

ボタンが押されたときに実行されるようにするには、の末尾に次のコードを追加します。[`OnCreate()`](xref:Android.App.Activity.OnCreate*)
b

```csharp
Button button = FindViewById<Button>(Resource.Id.button);

button.Click += (o, e) => {
    Toast.MakeText (this, "Beep Boop", ToastLength.Short).Show ();
};
```

これにより[`Button`](xref:Android.Widget.Button) 、レイアウトからがキャプチャされ[`Toast`](xref:Android.Widget.Toast) 、 [`Button`](xref:Android.Widget.Button)がクリックされたときに表示されるメッセージが追加されます。

次に、アプリケーションを実行します。

*このページの一部は、Android オープンソースプロジェクトによって作成および共有*
され、[*Creative Commons 2.5 属性*](http://creativecommons.org/licenses/by/2.5/)で説明されている条項に従って使用される作業に基づいて変更されます。
