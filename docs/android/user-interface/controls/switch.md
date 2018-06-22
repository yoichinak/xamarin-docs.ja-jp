---
title: 切り替え
ms.prod: xamarin
ms.assetid: 6E1F3324-EC41-454A-AEC0-0208813C7E50
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/05/2018
ms.openlocfilehash: 0f4bfc3646f1ccd956ee8151468b3de20f6e1e2b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
ms.locfileid: "30762660"
---
# <a name="switch"></a>切り替え

`Switch`ウィジェット (下図参照) など、2 つの状態間を切り替える] または [オフをユーザーに許可します。 `Switch`既定値は OFF です。 ウィジェットは、両方の ON、OFF の状態で、次に示します。

[![オンとオフの状態でスイッチ ウィジェットのスクリーン ショット](switch-images/16-switch-onoff.png)](switch-images/16-switch-onoff.png#lightbox)


## <a name="creating-a-switch"></a>スイッチを作成します。

スイッチを作成するには、単に宣言、`Switch`次のように XML ファイル内の要素。

```xml
<Switch android:layout_width="wrap_content"
        android:layout_height="wrap_content" />
```

次のように、スイッチの基本的なが作成されます。

[![スイッチをオフの状態で表示するデモ アプリのスクリーン ショット](switch-images/07-switch.png)](switch-images/07-switch.png#lightbox)


## <a name="changing-default-values"></a>既定値を変更します。

ON、OFF の状態のコントロールによって表示されるテキストと、既定値は、構成できます。 たとえば、既定で ON と OFF または ON ではなくはい/いいえを読み取り、スイッチをするためには、設定できます、 `checked`、 `textOn`、および`textOff`次の XML 属性です。

```xml
<Switch android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:checked="true"
        android:textOn="YES"
        android:textOff="NO" />
```



## <a name="providing-a-title"></a>タイトルを提供します。

`Switch`ウィジェットが設定して、テキスト ラベルを含むをサポートしても、`text`次のように属性します。

```xml
<Switch android:text="Is Xamarin.Android great?"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:checked="true"
        android:textOn="YES"
        android:textOff="NO" />
```

このマークアップでは、実行時に次のスクリーン ショットが生成されます。

[![テキストの水平方向にスイッチ ウィジェットの前に、デモ アプリのスクリーン ショット](switch-images/08-switch.png)](switch-images/08-switch.png#lightbox)

ときに、`Switch`の値の変更が発生、`CheckedChange`イベント。
たとえば、次のコードでおこのイベントをキャプチャして提示、`Toast`メッセージとウィジェットがに基づいて、`isChecked`値`Switch`の一部として、イベント ハンドラーに渡される、`CompoundButton.CheckedChangeEventArg`引数。

```csharp
Switch s = FindViewById<Switch> (Resource.Id.monitored_switch);
           
s.CheckedChange += delegate(object sender, CompoundButton.CheckedChangeEventArgs e) {
    var toast = Toast.MakeText (this, "Your answer is " +
        (e.IsChecked ?  "correct" : "incorrect"), ToastLength.Short);
    toast.Show ();
};
```


## <a name="related-links"></a>関連リンク

- [SwitchDemo (サンプル)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/ICS_Samples/SwitchDemo/)
- [タブ レイアウトのチュートリアル](~/android/user-interface/layouts/tab-layout/index.md)
- [アイスクリーム サンドイッチの概要](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 プラットフォーム](http://developer.android.com/sdk/android-4.0.html)
