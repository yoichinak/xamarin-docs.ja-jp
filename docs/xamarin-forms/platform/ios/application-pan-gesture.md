---
title: IOS での同時パンジェスチャの認識
description: プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、アプリケーションでパンジェスチャ認識を同時に使用できるようにする iOS プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 883D89DA-F8FF-4B97-9C3F-2DD05C96A495
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: f1baf5d71cc25a1f84ff683e9d0072f34715c0da
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93373238"
---
# <a name="simultaneous-pan-gesture-recognition-on-ios"></a>IOS での同時パンジェスチャの認識

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

[`PanGestureRecognizer`](xref:Xamarin.Forms.PanGestureRecognizer)がスクロールビュー内のビューにアタッチされている場合、すべてのパンジェスチャはによってキャプチャされ、 `PanGestureRecognizer` スクロールビューには渡されません。 そのため、スクロールビューはスクロールされなくなります。

この iOS プラットフォーム固有の機能により、スクロールビューでを使用して、 `PanGestureRecognizer` パンジェスチャをキャプチャし、スクロールビューと共有できます。 添付プロパティをに設定することにより、XAML で使用 [`Application.PanGestureRecognizerShouldRecognizeSimultaneously`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Application.PanGestureRecognizerShouldRecognizeSimultaneouslyProperty) され `true` ます。

```xaml
<Application ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Application.PanGestureRecognizerShouldRecognizeSimultaneously="true">
    ...
</Application>
```

または、fluent API を使用して C# から使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

Xamarin.Forms.Application.Current.On<iOS>().SetPanGestureRecognizerShouldRecognizeSimultaneously(true);
```

メソッドは、 `Application.On<iOS>` このプラットフォーム固有のが iOS 上でのみ実行されることを指定します。 [ `Application.SetPanGestureRecognizerShouldRecognizeSimultaneously` ] (Xref: Xamarin.FormsPlatformConfiguration. iOSSpecific ( Xamarin.Forms .IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration。 iOS、 Xamarin.Forms 。Application}, Boolean) メソッドを名前空間で使用して、 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) スクロールビューのパンジェスチャレコグナイザーがパンジェスチャをキャプチャするか、またはスクロールビューと共にパンジェスチャをキャプチャして共有するかを制御します。 また、[ `Application.GetPanGestureRecognizerShouldRecognizeSimultaneously` ] (xref: Xamarin.FormsPlatformConfiguration. iOSSpecific ( Xamarin.Forms .IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration。 iOS、 Xamarin.Forms 。Application}) メソッドを使用して、パンジェスチャがを含むスクロールビューで共有されているかどうかを返すことができ [`PanGestureRecognizer`](xref:Xamarin.Forms.PanGestureRecognizer) ます。

したがって、このプラットフォーム固有のを有効にすると、にが含まれている場合、 [`ListView`](xref:Xamarin.Forms.ListView) [`PanGestureRecognizer`](xref:Xamarin.Forms.PanGestureRecognizer) `ListView` との両方が `PanGestureRecognizer` パンジェスチャを受け取り、それを処理します。 ただし、このプラットフォーム固有の無効化では、にが含まれている場合、は `ListView` `PanGestureRecognizer` `PanGestureRecognizer` パンジェスチャをキャプチャして処理し、は `ListView` パンジェスチャを受け取りません。

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)