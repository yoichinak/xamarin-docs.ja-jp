---
title: Xamarin. Android ナビゲーションバー
ms.prod: xamarin
ms.assetid: 6023DB7E-9E72-4B90-A96A-11BC297B8A3D
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/01/2017
ms.openlocfilehash: 99d0303dc1560796cb372d0b8af2fafd16c6097f
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725141"
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

 [表示、低プロファイル、および非表示の SystemUiVisibility を示すスクリーンショットの ![](navigation-bar-images/18-systemuivisibility.png)](navigation-bar-images/18-systemuivisibility.png#lightbox)

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
