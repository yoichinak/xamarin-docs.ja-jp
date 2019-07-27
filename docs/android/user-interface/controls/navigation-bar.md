---
title: Xamarin. Android ナビゲーションバー
ms.prod: xamarin
ms.assetid: 6023DB7E-9E72-4B90-A96A-11BC297B8A3D
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/01/2017
ms.openlocfilehash: 70e009ed1a017b2336b6acb443a4d9cd87ff3e68
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510263"
---
# <a name="xamarinandroid-navigation-bar"></a>Xamarin. Android ナビゲーションバー

Android 4 では、*ナビゲーションバー*と呼ばれる新しいシステムユーザーインターフェイス機能が導入されました。これは、**自宅**、**背面**、**メニュー**のハードウェアボタンを含まないデバイスにナビゲーションコントロールを提供します。
次のスクリーンショットは、1つのデバイスからのナビゲーションバーを示しています。

 [![Android のナビゲーションバーの例](navigation-bar-images/19-navbar.png)](navigation-bar-images/19-navbar.png#lightbox)

いくつかの新しいフラグを使用して、ナビゲーションバーとそのコントロールの表示を制御したり、Android 3 で導入されたシステムバーを表示したりすることができます。 フラグは`Android.View.View`クラスで定義され、以下に一覧表示されます。

-   `SystemUiFlagVisible`&ndash;ナビゲーションバーを表示します。 
-   `SystemUiFlagLowProfile`&ndash;ナビゲーションバーのコントロールを暗くします。 
-   `SystemUiFlagHideNavigation`&ndash;ナビゲーションバーを非表示にします。 


これらのフラグは、 `SystemUiVisibility`プロパティを設定することによって、ビュー階層内の任意のビューに適用できます。 複数のビューにこのプロパティが設定されている場合、システムはそれらをまたは操作と組み合わせ、フラグが設定されているウィンドウがフォーカスを保持する限り、適用します。 ビューを削除すると、設定されているすべてのフラグも削除されます。

次の例は、ボタンのいずれかをクリックすると、を`SystemUiVisibility`変更する単純なアプリケーションを示しています。

 [![表示、低プロファイル、および非表示の SystemUiVisibility を示すスクリーンショット](navigation-bar-images/18-systemuivisibility.png)](navigation-bar-images/18-systemuivisibility.png#lightbox)

を変更`SystemUiVisibility`するコードは、次に示すよう`TextView`に、各ボタンのクリックイベントハンドラーからのプロパティを設定します。

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

また、変更`SystemUiVisibility`によって`SystemUiVisibilityChange`イベントが発生します。 プロパティの`SystemUiVisibility`設定と同様に、 `SystemUiVisibilityChange`イベントのハンドラーは、階層内の任意のビューに登録できます。 たとえば、次のコードでは、 `TextView`インスタンスを使用してイベントに登録します。

```csharp
tv.SystemUiVisibilityChange +=
  delegate(object sender, View.SystemUiVisibilityChangeEventArgs e) {
        tv.Text = String.Format ("Visibility = {0}", e.Visibility);
  };
```



## <a name="related-links"></a>関連リンク

- [SystemUIVisibilityDemo (sample)](https://developer.xamarin.com/samples/monodroid/SystemUIVisibilityDemo/)
- [アイスクリームサンドイッチの導入](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 プラットフォーム](https://developer.android.com/sdk/android-4.0.html)
