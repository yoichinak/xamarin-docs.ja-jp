---
title: ToggleButton
ms.prod: xamarin
ms.assetid: 9ADA8FCF-63ED-897A-DD56-D7D86535A92C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 56372bb643cab545529d6a4a89c804471f3344bc
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70762456"
---
# <a name="togglebutton"></a>ToggleButton

このセクションでは、 [`ToggleButton`](xref:Android.Widget.ToggleButton)ウィジェットを使用して2つの状態を切り替えるために使用するボタンを作成します。 相互に排他的な2つの単純な状態 ("オン" と "オフ" など) がある場合、このウィジェットは、オプションボタンの代わりに適しています。 Android 4.0 (API レベル 14) では、 [`Switch`](xref:Android.Widget.Switch)と呼ばれるトグルボタンの代わりにが導入されました。

**トグル**ボタンの例は、イメージの左側のペアで確認できます。一方、イメージの右側のペアは**スイッチ**の例を示しています。

![オンとオフの両方の状態のスイッチと ToggleButtons の例](toggle-button-images/togglebutton-switch.png)  

アプリケーションが使用するコントロールは、スタイルに関係しています。 どちらのウィジェットも機能的に同等です。

**Resources/layout/Main. axml**ファイルを開き、 [`ToggleButton`](xref:Android.Widget.ToggleButton) [`LinearLayout`](xref:Android.Widget.LinearLayout)要素 (内) を追加します。

状態が変更されたときに操作を行うには、の末尾に次のコードを追加します。[`OnCreate()`](xref:Android.App.Activity.OnCreate*)
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

これをキャプチャ、 [ `ToggleButton` ](xref:Android.Widget.ToggleButton)要素をレイアウトから、ボタンがクリックされたときに実行するアクションを定義するをクリック イベントを処理します。 この例では、メソッドによってボタンの新しい状態がチェックされ[`Toast`](xref:Android.Widget.Toast) 、現在の状態を示すメッセージが表示されます。

では、 [`ToggleButton`](xref:Android.Widget.ToggleButton) checked と unchecked の間で独自の状態の変更が処理されるので、それを確認するだけです。

アプリケーションを実行します。

> [!TIP]
> 状態を自分で変更する必要がある場合 (保存され[`CheckBoxPreference`](xref:Android.Preferences.CheckBoxPreference)たの読み込み時など) は、[`Checked`](xref:Android.Widget.CompoundButton.Checked)
> プロパティセッターまたは[`Toggle()`](xref:Android.Widget.CompoundButton.Toggle)
> メソッドをオーバーライドします。

## <a name="related-links"></a>関連リンク

- [ToggleButton](https://developer.android.com/reference/android/widget/ToggleButton.html)
- [スイッチ](https://developer.android.com/reference/android/widget/Switch.html)
