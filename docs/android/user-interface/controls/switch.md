---
title: Xamarin. Android スイッチ
description: Xamarin Android アプリケーションでスイッチウィジェットを使用する方法
ms.prod: xamarin
ms.assetid: 6E1F3324-EC41-454A-AEC0-0208813C7E50
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/29/2018
ms.openlocfilehash: 7ff10433ffe11965ccfb8c9a46a785b8cb0304e6
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510171"
---
# <a name="xamarinandroid-switch"></a>Xamarin. Android スイッチ

`Switch`ウィジェット (下図参照) では、ユーザーは2つの状態 (オン、オフなど) を切り替えることができます。 `Switch`既定値は OFF です。 次に示すのは、このウィジェットのオンとオフの両方の状態です。

[![スイッチウィジェットのスクリーンショット (オフと状態)](switch-images/16-switch-onoff.png)](switch-images/16-switch-onoff.png#lightbox)

## <a name="creating-a-switch"></a>スイッチを作成する

スイッチを作成するには、次`Switch`のように XML 内の要素を宣言するだけです。

```xml
<Switch android:layout_width="wrap_content"
        android:layout_height="wrap_content" />
```

これにより、基本的なスイッチが次のように作成されます。

[![オフ状態のスイッチを表示しているデモアプリのスクリーンショット](switch-images/07-switch.png)](switch-images/07-switch.png#lightbox)

## <a name="changing-default-values"></a>既定値の変更

オンとオフの状態に対してコントロールに表示されるテキストと、既定値の両方が構成可能です。 たとえば、スイッチを既定の on に設定し、OFF/ON ではなく read NO/YES を設定するに`checked`は`textOn`、次`textOff`の XML で、、およびの各属性を設定します。

```xml
<Switch android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:checked="true"
        android:textOn="YES"
        android:textOff="NO" />
```



## <a name="providing-a-title"></a>タイトルの指定

ウィ`Switch`ジェットでは、属性を`text`次のように設定することによって、テキストラベルを含めることもできます。

```xml
<Switch android:text="Is Xamarin.Android great?"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:checked="true"
        android:textOn="YES"
        android:textOff="NO" />
```

このマークアップは、実行時に次のスクリーンショットを生成します。

[![スイッチウィジェットの前に横書きでテキストが表示されたデモアプリのスクリーンショット](switch-images/08-switch.png)](switch-images/08-switch.png#lightbox)

の値`Switch`が変更されると、イベント`CheckedChange`が発生します。
たとえば、次のコードでは、このイベントをキャプチャし、 `Toast`の`Switch`値に`isChecked`基づいてメッセージを含むウィジェットを提示します。これは、 `CompoundButton.CheckedChangeEventArg`引数の一部としてイベントハンドラーに渡されます。

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
- [タブレイアウトのチュートリアル](~/android/user-interface/layouts/tab-layout/index.md)
