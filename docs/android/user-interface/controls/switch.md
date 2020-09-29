---
title: Xamarin. Android スイッチ
description: Xamarin Android アプリケーションでスイッチウィジェットを使用する方法
ms.prod: xamarin
ms.assetid: 6E1F3324-EC41-454A-AEC0-0208813C7E50
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/29/2018
ms.openlocfilehash: dabd4b5bd7d6dbd118314e94fe04bc4623cf11e1
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91457719"
---
# <a name="xamarinandroid-switch"></a>Xamarin. Android スイッチ

`Switch`ウィジェット (下図参照) では、ユーザーは2つの状態 (オン、オフなど) を切り替えることができます。 `Switch`既定値は OFF です。 次に示すのは、このウィジェットのオンとオフの両方の状態です。

[![スイッチウィジェットのスクリーンショット (オフと状態)](switch-images/16-switch-onoff.png)](switch-images/16-switch-onoff.png#lightbox)

## <a name="creating-a-switch"></a>スイッチを作成する

スイッチを作成するには、次のように `Switch` XML 内の要素を宣言するだけです。

```xml
<Switch android:layout_width="wrap_content"
        android:layout_height="wrap_content" />
```

これにより、基本的なスイッチが次のように作成されます。

[![オフ状態のスイッチを表示しているデモアプリのスクリーンショット](switch-images/07-switch.png)](switch-images/07-switch.png#lightbox)

## <a name="changing-default-values"></a>既定値の変更

オンとオフの状態に対してコントロールに表示されるテキストと、既定値の両方が構成可能です。 たとえば、スイッチを既定の ON に設定し、OFF/ON ではなく read NO/YES を設定するには、 `checked` `textOn` 次の XML で、、およびの各属性を設定し `textOff` ます。

```xml
<Switch android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:checked="true"
        android:textOn="YES"
        android:textOff="NO" />
```

## <a name="providing-a-title"></a>タイトルの指定

ウィジェットでは、 `Switch` 属性を次のように設定することによって、テキストラベルを含めることもでき `text` ます。

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

の値が変更されると `Switch` 、イベントが発生 `CheckedChange` します。
たとえば、次のコードでは、このイベントをキャプチャし、 `Toast` の値に基づいてメッセージを含むウィジェットを提示し `isChecked` `Switch` ます。これは、引数の一部としてイベントハンドラーに渡され `CompoundButton.CheckedChangeEventArg` ます。

```csharp
Switch s = FindViewById<Switch> (Resource.Id.monitored_switch);
           
s.CheckedChange += delegate(object sender, CompoundButton.CheckedChangeEventArgs e) {
    var toast = Toast.MakeText (this, "Your answer is " +
        (e.IsChecked ?  "correct" : "incorrect"), ToastLength.Short);
    toast.Show ();
};
```

## <a name="related-links"></a>関連リンク

- [SwitchDemo (サンプル)](/samples/xamarin/monodroid-samples/switchdemo)
- [タブレイアウトのチュートリアル](~/android/user-interface/layouts/tab-layout/index.md)