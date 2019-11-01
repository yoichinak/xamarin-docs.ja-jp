---
title: Xamarin. Android ナビゲーションバー
ms.prod: xamarin
ms.assetid: 6023DB7E-9E72-4B90-A96A-11BC297B8A3D
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/01/2017
ms.openlocfilehash: f28b9f19e901d75c432dfecbfec8a63588df3d70
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029227"
---
# <a name="xamarinandroid-navigation-bar"></a>Xamarin. Android ナビゲーションバー

Android 4 では、*ナビゲーションバー*と呼ばれる新しいシステムユーザーインターフェイス機能が導入されました。これは、**自宅**、**背面**、**メニュー**のハードウェアボタンを含まないデバイスにナビゲーションコントロールを提供します。
次のスクリーンショットは、1つのデバイスからのナビゲーションバーを示しています。

 [![Android のナビゲーションバーの例](navigation-bar-images/19-navbar.png)](navigation-bar-images/19-navbar.png#lightbox)

いくつかの新しいフラグを使用して、ナビゲーションバーとそのコントロールの表示を制御したり、Android 3 で導入されたシステムバーを表示したりすることができます。 フラグは `Android.View.View` クラスで定義され、以下に示します。

- `SystemUiFlagVisible` &ndash; を使用すると、ナビゲーションバーが表示されます。 
- `SystemUiFlagLowProfile` &ndash;、ナビゲーションバーのコントロールを淡色表示します。 
- `SystemUiFlagHideNavigation` &ndash; は、ナビゲーションバーを非表示にします。 

これらのフラグは、`SystemUiVisibility` プロパティを設定することによって、ビュー階層内の任意のビューに適用できます。 複数のビューにこのプロパティが設定されている場合、システムはそれらをまたは操作と組み合わせ、フラグが設定されているウィンドウがフォーカスを保持する限り、適用します。 ビューを削除すると、設定されているすべてのフラグも削除されます。

次の例は、いずれかのボタンをクリックすると `SystemUiVisibility`が変更される単純なアプリケーションを示しています。

 [表示、低プロファイル、および非表示の SystemUiVisibility を示すスクリーンショットの![](navigation-bar-images/18-systemuivisibility.png)](navigation-bar-images/18-systemuivisibility.png#lightbox)

`SystemUiVisibility` を変更するコードは、次に示すように、各ボタンのクリックイベントハンドラーから `TextView` のプロパティを設定します。

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

また、`SystemUiVisibility` の変更によって、`SystemUiVisibilityChange` イベントが発生します。 `SystemUiVisibility` プロパティの設定と同様に、`SystemUiVisibilityChange` イベントのハンドラーを階層内の任意のビューに登録できます。 たとえば、次のコードでは、`TextView` インスタンスを使用してイベントに登録します。

```csharp
tv.SystemUiVisibilityChange +=
  delegate(object sender, View.SystemUiVisibilityChangeEventArgs e) {
        tv.Text = String.Format ("Visibility = {0}", e.Visibility);
  };
```

## <a name="related-links"></a>関連リンク

- [SystemUIVisibilityDemo (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/systemuivisibilitydemo)
- [アイスクリームサンドイッチの導入](https://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 プラットフォーム](https://developer.android.com/sdk/android-4.0.html)
