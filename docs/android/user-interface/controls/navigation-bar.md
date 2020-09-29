---
title: Xamarin. Android ナビゲーションバー
ms.prod: xamarin
ms.assetid: 6023DB7E-9E72-4B90-A96A-11BC297B8A3D
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/01/2017
ms.openlocfilehash: 67c5655c3bbea8cd0a8c21f27719221f599bf481
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91457433"
---
# <a name="xamarinandroid-navigation-bar"></a>Xamarin. Android ナビゲーションバー

Android 4 では、 *ナビゲーションバー*と呼ばれる新しいシステムユーザーインターフェイス機能が導入されました。これは、 **自宅**、 **背面**、 **メニュー**のハードウェアボタンを含まないデバイスにナビゲーションコントロールを提供します。
次のスクリーンショットは、1つのデバイスからのナビゲーションバーを示しています。

 [![Android のナビゲーションバーの例](navigation-bar-images/19-navbar.png)](navigation-bar-images/19-navbar.png#lightbox)

いくつかの新しいフラグを使用して、ナビゲーションバーとそのコントロールの表示を制御したり、Android 3 で導入されたシステムバーを表示したりすることができます。 フラグはクラスで定義され、 `Android.View.View` 以下に一覧表示されます。

- `SystemUiFlagVisible`&ndash;ナビゲーションバーを表示します。
- `SystemUiFlagLowProfile`&ndash;ナビゲーションバーのコントロールを暗くします。
- `SystemUiFlagHideNavigation`&ndash;ナビゲーションバーを非表示にします。

これらのフラグは、プロパティを設定することによって、ビュー階層内の任意のビューに適用でき `SystemUiVisibility` ます。 複数のビューにこのプロパティが設定されている場合、システムはそれらをまたは操作と組み合わせ、フラグが設定されているウィンドウがフォーカスを保持する限り、適用します。 ビューを削除すると、設定されているすべてのフラグも削除されます。

次の例は、ボタンのいずれかをクリックすると、を変更する単純なアプリケーションを示してい `SystemUiVisibility` ます。

 [![表示、低プロファイル、および非表示の SystemUiVisibility を示すスクリーンショット](navigation-bar-images/18-systemuivisibility.png)](navigation-bar-images/18-systemuivisibility.png#lightbox)

を変更するコードは、 `SystemUiVisibility` `TextView` 次に示すように、各ボタンのクリックイベントハンドラーからのプロパティを設定します。

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

また、変更によって `SystemUiVisibility` イベントが発生 `SystemUiVisibilityChange` します。 プロパティの設定と同様 `SystemUiVisibility` に、イベントのハンドラーは、 `SystemUiVisibilityChange` 階層内の任意のビューに登録できます。 たとえば、次のコードでは、インスタンスを使用して `TextView` イベントに登録します。

```csharp
tv.SystemUiVisibilityChange +=
  delegate(object sender, View.SystemUiVisibilityChangeEventArgs e) {
        tv.Text = String.Format ("Visibility = {0}", e.Visibility);
  };
```

## <a name="related-links"></a>関連リンク

- [SystemUIVisibilityDemo (サンプル)](/samples/xamarin/monodroid-samples/systemuivisibilitydemo)