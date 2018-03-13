---
title: "カスタム ボタン"
ms.topic: article
ms.prod: xamarin
ms.assetid: C523D41E-5855-248D-079D-6B12B74B7617
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 5131b4d09f01af6a6e8bed28a2df27bc801dfb80
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2018
---
# <a name="custom-button"></a>カスタム ボタン

このセクションでは、テキストの代わりにカスタム イメージをボタンを作成しますを使用して、 [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/)ウィジェットと異なるボタンの状態を使用する 3 種類のイメージを定義する XML ファイルです。 ボタンが押されたときに短いメッセージが表示されます。

右クリックし、下の 3 つのイメージをダウンロードするこれらのコピー、**リソース/描画**プロジェクトのディレクトリ。 これらは、さまざまなボタンの状態に対して使用されます。

 [![通常の状態の Android 緑のアイコン](custom-button-images/android-normal.png)](custom-button-images/android-normal.png#lightbox) [![フォーカスのある状態の Android オレンジ色のアイコン](custom-button-images/android-focused.png)](custom-button-images/android-focused.png#lightbox) [![押された状態の Android 黄色のアイコン](custom-button-images/android-pressed.png)](custom-button-images/android-pressed.png#lightbox)

新しいファイルを作成、**リソース/描画**という名前のディレクトリ**android_button.xml**です。 次の XML を挿入します。

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

これは、ボタンの現在の状態に基づいてそのイメージを変更する 1 つドロウアブル リソースを定義します。 最初の`<item>`定義**android_pressed.png**ボタンが押されたときにイメージとして (これは、アクティブ化されて); 2 番目`<item>`を定義**android_focused.png**イメージとしてときに、(ボールまたは方向パッドを使用して、ボタンがハイライトされます) 場合、ボタンにフォーカスが移動;3 番目の`<item>`定義**android_normal.png** (押されても、重点を置いてもない) 場合は、通常の状態のイメージとして。 この XML ファイルは 1 つのドロウアブル リソースを表しますによって参照されると、 [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/)その背景に表示されるイメージによって変わりますこれら 3 つの状態。


> [!NOTE]
> 順序、`<item>`要素が重要です。 このドロウアブルが参照されたとき、`<item>`は順序どおりのどれがボタンの現在の状態の適切な決定を走査します。
> 場合のみ適用されますが、"normal"イメージは最後であるため、条件`android:state_pressed`と`android:state_focused`false 両方評価します。

開く、 **Resources/layout/Main.axml**ファイルを追加、 [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/)要素。

```xml
<Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:padding="10dp"
        android:background="@drawable/android_button" />
```

`android:background`属性は、ボタンの背景に使用するドロウアブル リソースを指定 (に保存するときに**Resources/drawable/android.xml**、として参照される`@drawable/android`)。 これには、システム全体でのボタンに使用される通常のバック グラウンド イメージが置き換えられます。 ドロウアブル ボタンの状態に基づいて、そのイメージを変更するためには、バック グラウンドにイメージを適用する必要があります。

末尾に次のコードを追加するのには、ボタンが押されたとき、 [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/Android.OS.PersistableBundle/)メソッド。

```csharp
Button button = FindViewById<Button>(Resource.Id.button);

button.Click += (o, e) => {
    Toast.MakeText (this, "Beep Boop", ToastLength.Short).Show ();
};
```

これをキャプチャ、 [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) 、レイアウトから追加し、 [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/)ときに表示されるメッセージを[ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/)がクリックされました。

アプリケーションを実行します。


*このページの部分は変更を作成し、Android のオープン ソース プロジェクトで共有しての条項に従って使用作業に基づく、*
[*クリエイティブ コモンズ 2.5 Attribution ライセンス*](http://creativecommons.org/licenses/by/2.5/).
