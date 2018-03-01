---
title: "ナビゲーション バー"
ms.topic: article
ms.prod: xamarin
ms.assetid: 6023DB7E-9E72-4B90-A96A-11BC297B8A3D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/01/2017
ms.openlocfilehash: 396ed31cba336976342a8dfb26f31eeda20cf494
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="navigation-bar"></a>ナビゲーション バー

Android 4 と呼ばれる新しいシステム ユーザー インターフェイス機能を導入された、*ナビゲーション バー*、ハードウェア ボタンが含まれていないデバイス上のナビゲーション コントロールを提供する**ホーム**、**戻る**、および**メニュー**です。
次のスクリーン ショットは、素数の Nexus デバイスからナビゲーション バーを示しています。

 [ ![Android のナビゲーション バーの例](navigation-bar-images/19-navbar.png)](navigation-bar-images/19-navbar.png)

いくつかの新しいフラグが使用可能なナビゲーション バーと、そのコントロールの表示だけでなく Android 3 で導入されたシステム バーの可視性を制御します。 フラグが定義されている、`Android.View.View`クラスし、以下に示します。

-   `SystemUiFlagVisible` &ndash; ナビゲーション バーは、表示します。 
-   `SystemUiFlagLowProfile` &ndash; ナビゲーション バーでコントロールを dims です。 
-   `SystemUiFlagHideNavigation` &ndash; ナビゲーション バーを非表示にします。 


これらのフラグは、設定で、ビューの階層内の任意のビューに適用できる、`SystemUiVisibility`プロパティです。 複数のビューには、このプロパティ設定がある、システムは OR 演算と組み合わされたし、フラグが設定されているウィンドウがフォーカスを保持する限りは、それらを適用します。 ビューを削除するときは、任意のフラグが設定されているも削除されます。

次の例は、単純なアプリケーションで変更のボタンをクリックすると、 `SystemUiVisibility`:

 [ ![Visible、低プロファイル、および SystemUiVisibility の非表示を示すスクリーン ショット](navigation-bar-images/18-systemuivisibility.png)](navigation-bar-images/18-systemuivisibility.png)

変更するコード、`SystemUiVisibility`でプロパティを設定、`TextView`から各ボタンのクリック イベント ハンドラーを次のようにします。

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

また、`SystemUiVisibility`変更が発生し、`SystemUiVisibilityChange`イベント。 設定と同じように、`SystemUiVisibility`プロパティ、ハンドラーを`SystemUiVisibilityChange`階層内のすべてのビュー イベントを登録することができます。 使用して次のコードなど、`TextView`のインスタンスをイベントの登録します。

```csharp
tv.SystemUiVisibilityChange +=
  delegate(object sender, View.SystemUiVisibilityChangeEventArgs e) {
        tv.Text = String.Format ("Visibility = {0}", e.Visibility);
  };
```



## <a name="related-links"></a>関連リンク

- [SystemUIVisibilityDemo (sample)](https://developer.xamarin.com/samples/monodroid/SystemUIVisibilityDemo/)
- [アイスクリーム サンドイッチの概要](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 プラットフォーム](http://developer.android.com/sdk/android-4.0.html)
