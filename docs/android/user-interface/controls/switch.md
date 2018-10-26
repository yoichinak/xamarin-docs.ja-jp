---
title: 切り替え
description: Xamarin.Android アプリケーションでスイッチ ウィジェットを使用する方法
ms.prod: xamarin
ms.assetid: 6E1F3324-EC41-454A-AEC0-0208813C7E50
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/29/2018
ms.openlocfilehash: ef400aaa31992b577762ad695418b865882e2e2d
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50115966"
---
# <a name="switch"></a>切り替え

`Switch`ウィジェット (下記参照) により、ユーザーなど、2 つの状態の間で切り替えるか、オフにします。 `Switch`既定値は OFF です。 ウィジェットは、両方の ON、OFF の状態で、以下に示します。

[![スイッチのオンとオフの状態ウィジェットのスクリーン ショット](switch-images/16-switch-onoff.png)](switch-images/16-switch-onoff.png#lightbox)


## <a name="creating-a-switch"></a>スイッチの作成

スイッチを作成するには、単に宣言を`Switch`次のように XML ファイル内の要素。

```xml
<Switch android:layout_width="wrap_content"
        android:layout_height="wrap_content" />
```

次に示す基本的なスイッチが作成されます。

[![スイッチをオフの状態で表示するデモ アプリのスクリーン ショット](switch-images/07-switch.png)](switch-images/07-switch.png#lightbox)


## <a name="changing-default-values"></a>既定値を変更します。

ON、OFF の状態のコントロールによって表示されるテキストと、既定値は、構成できます。 たとえば、既定で ON と OFF または ON ではなくなし/[はい] を読み取り、スイッチをするためには、設定できます、 `checked`、 `textOn`、および`textOff`の次の XML の属性。

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

[![スイッチのウィジェットの水平方向に前のテキストを使ってデモ アプリのスクリーン ショット](switch-images/08-switch.png)](switch-images/08-switch.png#lightbox)

ときに、`Switch`の値の変更が発生する`CheckedChange`イベント。
たとえば、次のコードでこのイベントをキャプチャし、提示を`Toast`メッセージ ウィジェットがに基づいて、`isChecked`の値`Switch`の一部としてイベント ハンドラーに渡された、`CompoundButton.CheckedChangeEventArg`引数。

```csharp
Switch s = FindViewById<Switch> (Resource.Id.monitored_switch);
           
s.CheckedChange += delegate(object sender, CompoundButton.CheckedChangeEventArgs e) {
    var toast = Toast.MakeText (this, "Your answer is " +
        (e.IsChecked ?  "correct" : "incorrect"), ToastLength.Short);
    toast.Show ();
};
```


## <a name="related-links"></a>関連リンク

- [SwitchDemo (サンプル)](https://developer.xamarin.com/samples/monodroid/SwitchDemo/)
- [タブ レイアウトのチュートリアル](~/android/user-interface/layouts/tab-layout/index.md)
