---
title: ToggleButton
ms.prod: xamarin
ms.assetid: 9ADA8FCF-63ED-897A-DD56-D7D86535A92C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 22eb8f999450ed8fb46b1f7809c92540be13aa65
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50105923"
---
# <a name="togglebutton"></a>ToggleButton

このセクションを使用して、2 つの状態間を切り替えるためには、具体的には使用されるボタンを作成します、 [ `ToggleButton` ](https://developer.xamarin.com/api/type/Android.Widget.ToggleButton/)ウィジェット。 このウィジェットには相互に排他的である 2 つの簡単な状態がある場合にオプション ボタンの優れた代替機能 ("on"と"off"など)。 Android 4.0 (API レベル 14) と呼ばれる、トグル ボタンの代替の導入を[ `Switch`](https://developer.xamarin.com/api/type/Android.Widget.Switch/)します。

例を**ToggleButton**イメージの右側にあるペアの例を表示するときに、イメージの左側にあるペアで確認できます、**スイッチ**:

![オンとオフの状態を両方のスイッチ、トグル ボタンの例](toggle-button-images/togglebutton-switch.png)  

アプリケーションで使用するコントロールは、スタイルの問題です。 両方のウィジェットが機能的に同等です。

開く、 **Resources/layout/Main.axml**追加ファイルを開き、 [ `ToggleButton` ](https://developer.xamarin.com/api/type/Android.Widget.ToggleButton/)要素 (内部、 [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/))。

末尾に次のコードを追加を行うには何か、状態が変更されたときに、 [`OnCreate()`](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/Android.OS.PersistableBundle)
方法:

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

これをキャプチャ、 [ `ToggleButton` ](https://developer.xamarin.com/api/type/Android.Widget.ToggleButton/)要素をレイアウトから、ボタンがクリックされたときに実行するアクションを定義するをクリック イベントを処理します。 この例で、メソッドは、ボタンの新しい状態をチェックしを示しています、 [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/)現在の状態を示すメッセージ。

注意、 [ `ToggleButton` ](https://developer.xamarin.com/api/type/Android.Widget.ToggleButton/)が自身の状態を変更できるためだけに問い合わせることが checked と unchecked、間のハンドル。

アプリケーションを実行します。


**ヒント:** 自分で状態を変更する必要がある場合 (場合など、保存された読み込み[ `CheckBoxPreference` ](https://developer.xamarin.com/api/type/Android.Preferences.CheckBoxPreference/)) を使用して、 [`Checked`](https://developer.xamarin.com/api/property/Android.Widget.CompoundButton.Checked/)
プロパティ set アクセス操作子または [`Toggle()`](https://developer.xamarin.com/api/member/Android.Widget.CompoundButton.Toggle/)
メソッドをオーバーライドします。


## <a name="related-links"></a>関連リンク

- [トグル ボタン](http://developer.android.com/reference/android/widget/ToggleButton.html)
- [スイッチ](http://developer.android.com/reference/android/widget/Switch.html)
