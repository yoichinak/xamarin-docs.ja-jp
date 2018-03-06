---
title: ToggleButton
ms.topic: article
ms.prod: xamarin
ms.assetid: 9ADA8FCF-63ED-897A-DD56-D7D86535A92C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 9e1e9711d218f4f4be825ff223b650ae932ad041
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="togglebutton"></a>ToggleButton

このセクションで作成を使用して、2 つの状態の切り替え専用のボタン、 [ `ToggleButton` ](https://developer.xamarin.com/api/type/Android.Widget.ToggleButton/)ウィジェット。 このウィジェットは相互に排他的である 2 つの簡単な状態の場合はオプション ボタンの優れた代替機能 ("on"と"off"など)。 Android 4.0 (API レベル 14) の導入と呼ばれるトグル ボタンをクリックする代わりに、 [ `Switch`](https://developer.xamarin.com/api/type/Android.Widget.Switch/)です。

例、 **ToggleButton**でわかるように、イメージの左側にあるペアのイメージの右側のペアの例を表示するときに、**スイッチ**:

![オンとオフの状態を両方のスイッチ、トグル ボタンの例](toggle-button-images/togglebutton-switch.png)  

アプリケーションで使用するコントロールは、スタイルの問題です。 両方のウィジェットが機能的に同等です。

開く、 **Resources/layout/Main.axml**ファイルを追加、 [ `ToggleButton` ](https://developer.xamarin.com/api/type/Android.Widget.ToggleButton/)要素 (内部、 [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/))。

状態が変更されたときに何かには、末尾に次のコードを追加、 [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/Android.OS.PersistableBundle)メソッド。

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

これをキャプチャ、 [ `ToggleButton` ](https://developer.xamarin.com/api/type/Android.Widget.ToggleButton/)要素をレイアウトから、ボタンがクリックされたときに実行するアクションを定義するをクリック イベントを処理します。 この例では、メソッドは、ボタンの新しい状態を確認しを示しています、 [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/)を現在の状態を示すメッセージ。

注意して、 [ `ToggleButton` ](https://developer.xamarin.com/api/type/Android.Widget.ToggleButton/) checked と unchecked、間が自身の状態を変更するためだけに問い合わせることであるハンドル。

アプリケーションを実行します。


**ヒント:**状態を変更する必要がある場合 (場合など、保存された読み込み[ `CheckBoxPreference` ](https://developer.xamarin.com/api/type/Android.Preferences.CheckBoxPreference/))、使用、 [ `Checked` ](https://developer.xamarin.com/api/property/Android.Widget.CompoundButton.Checked/)プロパティ set アクセス操作子または[ `Toggle()`](https://developer.xamarin.com/api/member/Android.Widget.CompoundButton.Toggle/)メソッドです。


## <a name="related-links"></a>関連リンク

- [ToggleButton](http://developer.android.com/reference/android/widget/ToggleButton.html)
- [スイッチ](http://developer.android.com/reference/android/widget/Switch.html)
