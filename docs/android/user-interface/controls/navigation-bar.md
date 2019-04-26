---
title: ナビゲーション バー
ms.prod: xamarin
ms.assetid: 6023DB7E-9E72-4B90-A96A-11BC297B8A3D
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/01/2017
ms.openlocfilehash: 9455cac81a0f9ea81e08cf63397e45c1698e1c1b
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61153640"
---
# <a name="navigation-bar"></a>ナビゲーション バー

Android 4 と呼ばれる新しいシステム ユーザー インターフェイス機能の導入を*ナビゲーション バー*、用のハードウェア ボタンが含まれていないデバイス上のナビゲーション コントロールを提供する**ホーム**、**戻る**、および**メニュー**します。
次のスクリーン ショットには、素数の Nexus デバイスからナビゲーション バーが表示されます。

 [![Android のナビゲーション バーの例](navigation-bar-images/19-navbar.png)](navigation-bar-images/19-navbar.png#lightbox)

いくつかの新しいフラグが使用可能なナビゲーション バーと、コントロールの可視性と Android 3 で導入されたシステム バーの可視性を制御します。 フラグが定義されている、`Android.View.View`クラスを以下に示します。

-   `SystemUiFlagVisible` &ndash; ナビゲーション バーは、表示します。 
-   `SystemUiFlagLowProfile` &ndash; ナビゲーション バーでコントロールを dims します。 
-   `SystemUiFlagHideNavigation` &ndash; ナビゲーション バーを非表示にします。 


これらのフラグを設定して、ビューの階層内の任意のビューに適用できる、`SystemUiVisibility`プロパティ。 複数のビューには、このプロパティの設定がある、システムは、OR 演算とはそれらを結合し、フラグが設定されているウィンドウがフォーカスを保持する限り適用します。 ビューを削除すると、フラグが設定されても削除されます。

次の例は、単純なアプリケーションで変更のボタンをクリックすると、 `SystemUiVisibility`:

 [![Visible、低のプロファイルと SystemUiVisibility の非表示を示すスクリーン ショット](navigation-bar-images/18-systemuivisibility.png)](navigation-bar-images/18-systemuivisibility.png#lightbox)

コードを変更する、`SystemUiVisibility`プロパティに設定、`TextView`から各ボタンのクリック イベント ハンドラーを次に示すよう。

```csharp
var tv = FindViewById<TextView> (Resource.Id.systemUiFlagTextView);
var lowProfileButton = FindViewById<Button>(Resource.Id.lowProfileButton);
var hideNavButton = FindViewById<Button> (Resource.Id.hideNavigation);
var visibleButton = FindViewById<Button> (Resource.Id.visibleButton);
           
lowProfileButton.Click += delegate {
    tv.SystemUiVisibility =
        (StatusBarVisibility)View.SystemUiFlagLowProfile;
};
           
hideNavButton.Click += delegate {
    tv.SystemUiVisibility =
       (StatusBarVisibility)View.SystemUiFlagHideNavigation;        
};
           
visibleButton.Click += delegate {
    tv.SystemUiVisibility = (StatusBarVisibility)View.SystemUiFlagVisible;
}
```

また、`SystemUiVisibility`変更が発生する`SystemUiVisibilityChange`イベント。 設定と同様、`SystemUiVisibility`プロパティ、ハンドラーを`SystemUiVisibilityChange`階層内の任意のビューには、イベントを登録することができます。 使用して次のコードなど、`TextView`イベントに登録するインスタンス。

```csharp
tv.SystemUiVisibilityChange +=
  delegate(object sender, View.SystemUiVisibilityChangeEventArgs e) {
        tv.Text = String.Format ("Visibility = {0}", e.Visibility);
  };
```



## <a name="related-links"></a>関連リンク

- [SystemUIVisibilityDemo (sample)](https://developer.xamarin.com/samples/monodroid/SystemUIVisibilityDemo/)
- [Ice Cream Sandwich の概要](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 プラットフォーム](https://developer.android.com/sdk/android-4.0.html)
