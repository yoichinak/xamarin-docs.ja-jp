---
title: Xamarin. Android スイッチ
description: Xamarin Android アプリケーションでスイッチウィジェットを使用する方法
ms.prod: xamarin
ms.assetid: 6E1F3324-EC41-454A-AEC0-0208813C7E50
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/29/2018
ms.openlocfilehash: 73becb5e4424854c9be6cdc3554f6cf93507b9a9
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029138"
---
# <a name="xamarinandroid-switch"></a>Xamarin. Android スイッチ

`Switch` ウィジェット (下図参照) では、ユーザーは2つの状態 (オン、オフなど) を切り替えることができます。 `Switch` の既定値は OFF です。 次に示すのは、このウィジェットのオンとオフの両方の状態です。

[スイッチウィジェットのスクリーンショットをオフおよび状態に![](switch-images/16-switch-onoff.png)](switch-images/16-switch-onoff.png#lightbox)

## <a name="creating-a-switch"></a>スイッチを作成する

スイッチを作成するには、次のように XML 内の `Switch` 要素を宣言するだけです。

```xml
<Switch android:layout_width="wrap_content"
        android:layout_height="wrap_content" />
```

これにより、基本的なスイッチが次のように作成されます。

[オフ状態のスイッチを表示しているデモアプリのスクリーンショット![](switch-images/07-switch.png)](switch-images/07-switch.png#lightbox)

## <a name="changing-default-values"></a>既定値の変更

オンとオフの状態に対してコントロールに表示されるテキストと、既定値の両方が構成可能です。 たとえば、スイッチの既定値を ON に設定し、OFF/ON ではなく read NO/YES を設定するには、次の XML の `checked`、`textOn`、および `textOff` 属性を設定します。

```xml
<Switch android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:checked="true"
        android:textOn="YES"
        android:textOff="NO" />
```

## <a name="providing-a-title"></a>タイトルの指定

`Switch` ウィジェットでは、`text` 属性を次のように設定することによって、テキストラベルを含めることもできます。

```xml
<Switch android:text="Is Xamarin.Android great?"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:checked="true"
        android:textOn="YES"
        android:textOff="NO" />
```

このマークアップは、実行時に次のスクリーンショットを生成します。

[スイッチウィジェットの前に横書きのテキストを含むデモアプリのスクリーンショットを![します](switch-images/08-switch.png)](switch-images/08-switch.png#lightbox)

`Switch`の値が変更されると、`CheckedChange` イベントが発生します。
たとえば、次のコードでは、このイベントをキャプチャし、`Switch`の `isChecked` 値に基づいてメッセージを含む `Toast` ウィジェットを提示します。これは、`CompoundButton.CheckedChangeEventArg` 引数の一部としてイベントハンドラーに渡されます。

```csharp
Switch s = FindViewById<Switch> (Resource.Id.monitored_switch);
           
s.CheckedChange += delegate(object sender, CompoundButton.CheckedChangeEventArgs e) {
    var toast = Toast.MakeText (this, "Your answer is " +
        (e.IsChecked ?  "correct" : "incorrect"), ToastLength.Short);
    toast.Show ();
};
```

## <a name="related-links"></a>関連リンク

- [SwitchDemo (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/switchdemo)
- [タブレイアウトのチュートリアル](~/android/user-interface/layouts/tab-layout/index.md)
