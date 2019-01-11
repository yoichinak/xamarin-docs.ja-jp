---
title: Android で ImageButton ドロップ シャドウ
description: プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。 この記事では、Android プラットフォームに固有の ImageButton にドロップ シャドウをできるようにするを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: D3604D87-9F9F-4FE2-8B10-DF3B143C0734
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: 56415aff251149aee01c2e2eb7e335e157180962
ms.sourcegitcommit: 395774577f7524b57035c5cca3c9034a4b636489
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/10/2019
ms.locfileid: "54209957"
---
# <a name="imagebutton-drop-shadows-on-android"></a>Android で ImageButton ドロップ シャドウ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

この Android プラットフォームに固有の使用にドロップ シャドウを有効にする`ImageButton`します。 XAML で設定して使用される、`ImageButton.IsShadowEnabled`バインド可能なプロパティを`true`、と共にさまざまなドロップ シャドウを制御する追加の省略可能なバインド可能なプロパティ。

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
       <ImageButton ...
                    Source="XamarinLogo.png"
                    BackgroundColor="GhostWhite"
                    android:ImageButton.IsShadowEnabled="true"
                    android:ImageButton.ShadowColor="Gray"
                    android:ImageButton.ShadowRadius="12">
            <android:ImageButton.ShadowOffset>
                <Size>
                    <x:Arguments>
                        <x:Double>10</x:Double>
                        <x:Double>10</x:Double>
                    </x:Arguments>
                </Size>
            </android:ImageButton.ShadowOffset>
        </ImageButton>
        ...
    </StackLayout>
</ContentPage>
```

代わりに、fluent API を使用して c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

var imageButton = new Xamarin.Forms.ImageButton { Source = "XamarinLogo.png", BackgroundColor = Color.GhostWhite, ... };
imageButton.On<Android>()
           .SetIsShadowEnabled(true)
           .SetShadowColor(Color.Gray)
           .SetShadowOffset(new Size(10, 10))
           .SetShadowRadius(12);
```

> [!IMPORTANT]
> 一部としてドロップ シャドウを描画、`ImageButton`場合にバック グラウンド、および背景が描画されるのみ、`BackgroundColor`プロパティを設定します。 そのため、ドロップ シャドウがいない描画される場合、`ImageButton.BackgroundColor`プロパティが設定されていません。

`ImageButton.On<Android>`メソッドはこのプラットフォーム仕様が Android 上でのみ動作することを指定します。 `ImageButton.SetIsShadowEnabled`メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)にドロップ シャドウが有効になっているかどうか、名前空間を使用して、`ImageButton`します。 さらに、ドロップ シャドウを制御する、次のメソッドを呼び出すことができます。

- `SetShadowColor` – ドロップ シャドウの色を設定します。 既定の色は[ `Color.Default`](xref:Xamarin.Forms.Color.Default*)します。
- `SetShadowOffset` –、影のオフセットを設定します。 影がキャストされますとしてを指定して方向を変更して、オフセット、 [ `Size` ](xref:Xamarin.Forms.Size)値。 `Size`までの距離 (負の値) の左または右 (正の値)、最初の値と上記の距離をされている 2 番目の値 (負の値) または (正の値) の下、構造体の値は、デバイスに依存しない単位で表されます. このプロパティの既定値は (0.0, 0.0) のすべての面にキャスト、影がその結果、`ImageButton`します。
- `SetShadowRadius`– ドロップ シャドウを表示するために使用するぼかしの半径を設定します。 既定の半径の値には 10.0 です。

> [!NOTE]
> ドロップ シャドウの状態は呼び出すことによってクエリを実行できる、 `GetIsShadowEnabled`、 `GetShadowColor`、 `GetShadowOffset`、および`GetShadowRadius`メソッド。

結果は、ドロップ シャドウの有効になっている、 `ImageButton`:

![](imagebutton-drop-shadow-images/imagebutton-drop-shadow.png "ドロップ シャドウを伴って ImageButton")

## <a name="related-links"></a>関連リンク

- [プラットフォーム仕様 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [プラットフォーム仕様の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific.AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
