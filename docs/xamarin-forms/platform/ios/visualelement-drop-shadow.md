---
title: "VisualElement Drop Shadows on iOS" の説明: "プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装しなくても、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、VisualElement でドロップシャドウを有効にする iOS プラットフォーム固有のを使用する方法について説明します。
ms. 製品: xamarin ms. assetid: 2147FD66-058E-4BE5-840A-369842B26EC4: xamarin-forms author: davidbritch ms. author: dabritch ms. date: 10/24/2018 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="visualelement-drop-shadows-on-ios"></a>IOS の VisualElement ドロップシャドウ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この iOS プラットフォーム固有のは、のドロップシャドウを有効にするために使用され [`VisualElement`](xref:Xamarin.Forms.VisualElement) ます。 これは、添付プロパティをに設定することによって XAML で使用され [`VisualElement.IsShadowEnabled`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.IsShadowEnabledProperty) `true` ます。また、ドロップシャドウを制御する追加のオプションの添付プロパティがいくつかあります。

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <BoxView ...
                 ios:VisualElement.IsShadowEnabled="true"
                 ios:VisualElement.ShadowColor="Purple"
                 ios:VisualElement.ShadowOpacity="0.7"
                 ios:VisualElement.ShadowRadius="12">
            <ios:VisualElement.ShadowOffset>
                <Size>
                    <x:Arguments>
                        <x:Double>10</x:Double>
                        <x:Double>10</x:Double>
                    </x:Arguments>
                </Size>
            </ios:VisualElement.ShadowOffset>
         </BoxView>
        ...
    </StackLayout>
</ContentPage>
```

または、fluent API を使用して C# から使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var boxView = new BoxView { Color = Color.Aqua, WidthRequest = 100, HeightRequest = 100 };
boxView.On<iOS>()
       .SetIsShadowEnabled(true)
       .SetShadowColor(Color.Purple)
       .SetShadowOffset(new Size(10,10))
       .SetShadowOpacity(0.7)
       .SetShadowRadius(12);
```

メソッドは、 `VisualElement.On<iOS>` このプラットフォーム固有のが iOS 上でのみ実行されることを指定します。 [ `VisualElement.SetIsShadowEnabled` ] (Xref: Xamarin.FormsPlatformConfiguration. iOSSpecific ( Xamarin.Forms .IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration。 iOS、 Xamarin.Forms 。VisualElement}, system.string) メソッドを名前空間で使用して、 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) でドロップシャドウを有効にするかどうかを制御し `VisualElement` ます。 さらに、次のメソッドを呼び出してドロップシャドウを制御できます。

- [ `SetShadowColor` ] (xref: Xamarin.Forms 。PlatformConfiguration. iOSSpecific ( Xamarin.Forms .IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration。 iOS、 Xamarin.Forms 。VisualElement}、 Xamarin.Forms 。[色]) –ドロップシャドウの色を設定します。 既定の色は [`Color.Default`](xref:Xamarin.Forms.Color.Default*) です。
- [ `SetShadowOffset` ] (xref: Xamarin.Forms 。PlatformConfiguration. iOSSpecific ( Xamarin.Forms .IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration。 iOS、 Xamarin.Forms 。VisualElement}、 Xamarin.Forms 。[サイズ]) –ドロップシャドウのオフセットを設定します。 オフセットは影がキャストされる方向を変更し、値として指定され [`Size`](xref:Xamarin.Forms.Size) ます。 `Size`構造体の値は、デバイスに依存しない単位で表されます。最初の値は左 (負の値) または右 (正の値)、2番目の値は上の距離 (負の値) または下 (正の値) です。 このプロパティの既定値は (0.0, 0.0) です。これにより、のすべての辺に影がキャストされ `VisualElement` ます。
- [ `SetShadowOpacity` ] (xref: Xamarin.Forms 。PlatformConfiguration. iOSSpecific ( Xamarin.Forms .IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration。 iOS、 Xamarin.Forms 。VisualElement}, system.string)-ドロップシャドウの不透明度を設定します。値は 0.0 (透明) から 1.0 (不透明) の範囲内です。 既定の不透明度の値は0.5 です。
- [ `SetShadowRadius` ] (xref: Xamarin.Forms 。PlatformConfiguration. iOSSpecific ( Xamarin.Forms .IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration。 iOS、 Xamarin.Forms 。VisualElement}, system.string) –ドロップシャドウのレンダリングに使用されるぼかしの半径を設定します。 既定の radius 値は10.0 です。

> [!NOTE]
> ドロップシャドウの状態を照会するには、[ `GetIsShadowEnabled` ] (xref: を呼び出します Xamarin.Forms 。PlatformConfiguration. iOSSpecific ( Xamarin.Forms .IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration。 iOS、 Xamarin.Forms 。VisualElement})、[ `GetShadowColor` ] (xref: Xamarin.Forms 。PlatformConfiguration. iOSSpecific ( Xamarin.Forms .IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration。 iOS、 Xamarin.Forms 。VisualElement})、[ `GetShadowOffset` ] (xref: Xamarin.Forms 。PlatformConfiguration. iOSSpecific ( Xamarin.Forms .IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration。 iOS、 Xamarin.Forms 。VisualElement})、[ `GetShadowOpacity` ] (xref: Xamarin.Forms 。PlatformConfiguration. iOSSpecific ( Xamarin.Forms .IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration。 iOS、 Xamarin.Forms 。VisualElement})、および [ `GetShadowRadius` ] (xref: Xamarin.Forms 。PlatformConfiguration. iOSSpecific ( Xamarin.Forms .IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration。 iOS、 Xamarin.Forms 。VisualElement})) メソッド。

結果として、でドロップシャドウを有効にすることができ [`VisualElement`](xref:Xamarin.Forms.VisualElement) ます。

![](drop-shadow-images/drop-shadow.png "Drop shadow enabled")

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
