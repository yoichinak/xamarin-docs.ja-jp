---
title: Android での ImageButton ドロップシャドウ
description: プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、ImageButton でドロップシャドウを有効にする、Android プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: D3604D87-9F9F-4FE2-8B10-DF3B143C0734
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: d6f3304c9eeb87405ab303a80450a1cd8b2af267
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86938061"
---
# <a name="imagebutton-drop-shadows-on-android"></a>Android での ImageButton ドロップシャドウ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この Android プラットフォーム固有のは、のドロップシャドウを有効にするために使用され `ImageButton` ます。 このメソッドは、バインド可能なプロパティをに設定することによって XAML で使用され `ImageButton.IsShadowEnabled` `true` ます。また、ドロップシャドウを制御する追加のオプションのバインド可能なプロパティがいくつかあります。

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

または、fluent API を使用して C# から使用することもできます。

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
> ドロップシャドウは背景の一部として描画され、 `ImageButton` プロパティが設定されている場合にのみ背景が描画され `BackgroundColor` ます。 したがって、プロパティが設定されていない場合、ドロップシャドウは描画されません `ImageButton.BackgroundColor` 。

メソッドは、 `ImageButton.On<Android>` このプラットフォーム固有のが Android でのみ実行されることを指定します。 `ImageButton.SetIsShadowEnabled`名前空間のメソッドは、 [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) でドロップシャドウを有効にするかどうかを制御するために使用され `ImageButton` ます。 さらに、次のメソッドを呼び出してドロップシャドウを制御できます。

- `SetShadowColor`–ドロップシャドウの色を設定します。 既定の色は [`Color.Default`](xref:Xamarin.Forms.Color.Default*) です。
- `SetShadowOffset`–ドロップシャドウのオフセットを設定します。 オフセットは影がキャストされる方向を変更し、値として指定され [`Size`](xref:Xamarin.Forms.Size) ます。 `Size`構造体の値は、デバイスに依存しない単位で表されます。最初の値は左 (負の値) または右 (正の値)、2番目の値は上の距離 (負の値) または下 (正の値) です。 このプロパティの既定値は (0.0, 0.0) です。これにより、のすべての辺に影がキャストされ `ImageButton` ます。
- `SetShadowRadius`–ドロップシャドウの描画に使用するぼかしの半径を設定します。 既定の radius 値は10.0 です。

> [!NOTE]
> ドロップシャドウの状態を照会するには、、、 `GetIsShadowEnabled` 、およびの各メソッドを呼び出し `GetShadowColor` `GetShadowOffset` `GetShadowRadius` ます。

結果として、でドロップシャドウを有効にすることができ `ImageButton` ます。

![ドロップシャドウ付きの ImageButton](imagebutton-drop-shadow-images/imagebutton-drop-shadow.png)

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific の AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
