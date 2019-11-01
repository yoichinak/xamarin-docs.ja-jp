---
title: ToggleButton
ms.prod: xamarin
ms.assetid: 9ADA8FCF-63ED-897A-DD56-D7D86535A92C
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: fe444d255beb9c08b4b5bcf5de36a8740e503b55
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029298"
---
# <a name="togglebutton"></a>ToggleButton

このセクションでは、 [`ToggleButton`](xref:Android.Widget.ToggleButton)ウィジェットを使用して、2つの状態を切り替えるために使用するボタンを作成します。 相互に排他的な2つの単純な状態 ("オン" と "オフ" など) がある場合、このウィジェットは、オプションボタンの代わりに適しています。 Android 4.0 (API レベル 14) では、 [`Switch`](xref:Android.Widget.Switch)と呼ばれるトグルボタンの代替手段が導入されました。

**トグル**ボタンの例は、イメージの左側のペアで確認できます。一方、イメージの右側のペアは**スイッチ**の例を示しています。

![オンとオフの両方の状態のスイッチと ToggleButtons の例](toggle-button-images/togglebutton-switch.png)  

アプリケーションが使用するコントロールは、スタイルに関係しています。 どちらのウィジェットも機能的に同等です。

**Resources/layout/Main. axml**ファイルを開き、 [`ToggleButton`](xref:Android.Widget.ToggleButton)要素 ( [`LinearLayout`](xref:Android.Widget.LinearLayout)内) を追加します。

状態が変更されたときに操作を実行するには、の末尾に次のコードを追加し[`OnCreate()`](xref:Android.App.Activity.OnCreate*)
b

```csharp
ToggleButton togglebutton = FindViewById<ToggleButton>(Resource.Id.togglebutton);

togglebutton.Click += (o, e) => {
    // Perform action on clicks
    if (togglebutton.Checked)
        Toast.MakeText(this, "Checked", ToastLength.Short).Show ();
    else
        Toast.MakeText(this, "Not checked", ToastLength.Short).Show ();
};
```

これにより、レイアウトから[`ToggleButton`](xref:Android.Widget.ToggleButton)要素がキャプチャされ、クリックイベントが処理されます。このイベントは、ボタンがクリックされたときに実行するアクションを定義します。 この例では、メソッドはボタンの新しい状態をチェックし、現在の状態を示す[`Toast`](xref:Android.Widget.Toast)メッセージを表示します。

[`ToggleButton`](xref:Android.Widget.ToggleButton)によって、checked と unchecked の間で独自の状態変更が処理されるので、それを確認するだけです。

アプリケーションを実行します。

> [!TIP]
> 状態を自分で変更する必要がある場合 (保存されている[`CheckBoxPreference`](xref:Android.Preferences.CheckBoxPreference)を読み込む場合など) は、 [`Checked`](xref:Android.Widget.CompoundButton.Checked)を使用します。
> プロパティセッターまたは[`Toggle()`](xref:Android.Widget.CompoundButton.Toggle)
> メソッドをオーバーライドします。

## <a name="related-links"></a>関連リンク

- [ToggleButton](https://developer.android.com/reference/android/widget/ToggleButton.html)
- [スイッチ](https://developer.android.com/reference/android/widget/Switch.html)
