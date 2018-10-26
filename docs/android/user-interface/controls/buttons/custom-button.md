---
title: カスタム ボタン
ms.prod: xamarin
ms.assetid: C523D41E-5855-248D-079D-6B12B74B7617
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: b5ccefa1eb7e659584c1c82481bbd4473a3a8abc
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50122856"
---
# <a name="custom-button"></a>カスタム ボタン

このセクションで、テキストの代わりにカスタム イメージでボタンを作成するを使用して、 [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/)ウィジェットとボタンのさまざまな状態に使用する 3 つの異なるイメージを定義する XML ファイル。 ボタンが押されたときに短いメッセージが表示されます。

右クリックし、次の 3 つのイメージをダウンロード、コピー、**リソース/drawable**プロジェクトのディレクトリ。 これらは、ボタンのさまざまな状態用に使用されます。

 [![通常の状態のアイコンが緑 Android](custom-button-images/android-normal.png)](custom-button-images/android-normal.png#lightbox) [![フォーカスのある状態のアイコンをオレンジ色の Android](custom-button-images/android-focused.png)](custom-button-images/android-focused.png#lightbox) [![押された状態の Android 黄色のアイコン](custom-button-images/android-pressed.png)](custom-button-images/android-pressed.png#lightbox)

新しいファイルを作成、**リソース/drawable**という名前のディレクトリ**android_button.xml**します。 次の XML を挿入します。

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

これには、ボタンの現在の状態に基づいて、そのイメージを変更する単一の描画可能なリソースを定義します。 最初の`<item>`定義**android_pressed.png**ボタンが押されたときにイメージとして (これはアクティブで)、2 つ目`<item>`を定義します**android_focused.png**イメージとしてときに、(トラック ボールまたは方向パッドを使用して、ボタンが強調表示) する場合、ボタンにフォーカスがある;3 番目の`<item>`定義**android_normal.png** (押されても、重点を置いています) 場合は、通常の状態のイメージとして。 この XML ファイルが 1 つの描画可能なリソースを表すようになりましたによって参照されると、 [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/)がの背景を表示するイメージによって変わりますこれら 3 つの状態。


> [!NOTE]
> 順序、`<item>`要素が重要です。 この drawable が参照されると、`<item>`は、どれがボタンの現在の状態に適したを判断するためにスキャンします。
> 場合のみ適用されますが、"normal"のイメージは最後であるため、条件`android:state_pressed`と`android:state_focused`両方が false を評価します。

開く、 **Resources/layout/Main.axml**追加ファイルを開き、 [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/)要素。

```xml
<Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:padding="10dp"
        android:background="@drawable/android_button" />
```

`android:background`属性は、ボタンの背景に使用する描画可能なリソースを指定します (これに保存するときに**Resources/drawable/android.xml**、として参照されます`@drawable/android`)。 これには、システム全体でのボタンに使用される通常のバック グラウンド イメージが置き換えられます。 描画可能なボタンの状態に基づいて、そのイメージを変更するためには、画像を背景に適用する必要があります。

末尾に次のコードを追加するのには、ボタンが押されたときに何か、 [`OnCreate()`](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/Android.OS.PersistableBundle/)
方法:

```csharp
Button button = FindViewById<Button>(Resource.Id.button);

button.Click += (o, e) => {
    Toast.MakeText (this, "Beep Boop", ToastLength.Short).Show ();
};
```

これをキャプチャ、 [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) 、レイアウトから追加し、 [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/)ときに表示されるメッセージを[ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/)がクリックされました。

ここで、アプリケーションを実行します。


*このページの部分が作成および Android のオープン ソース プロジェクトで共有し、の条項に従って使用作業に基づいた変更、*
[*Creative Commons 2.5 Attribution License*](http://creativecommons.org/licenses/by/2.5/).
