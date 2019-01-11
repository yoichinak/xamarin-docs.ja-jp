---
title: IOS での同時パン ジェスチャ認識
description: プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。 この記事では、アプリケーションで使用される同時パン ジェスチャ認識できるようにするプラットフォームに固有の iOS を使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 883D89DA-F8FF-4B97-9C3F-2DD05C96A495
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 32c3651734fcc94dd75372f0c47f0ffb22b4a4ee
ms.sourcegitcommit: 395774577f7524b57035c5cca3c9034a4b636489
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/10/2019
ms.locfileid: "54209707"
---
# <a name="simultaneous-pan-gesture-recognition-on-ios"></a>IOS での同時パン ジェスチャ認識

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

ときに、 [ `PanGestureRecognizer` ](xref:Xamarin.Forms.PanGestureRecognizer)がすべてのジェスチャがによってキャプチャされた pan のスクロール ビュー内のビューにアタッチされている、`PanGestureRecognizer`は、スクロール ビューに渡されます。 そのため、スクロール ビュー不要になったスクロールします。

この iOS プラットフォームに固有の有効、`PanGestureRecognizer`でスクロールして表示をキャプチャし、スクロール ビューとパン ジェスチャを共有します。 XAML で設定して使用される、 [ `Application.PanGestureRecognizerShouldRecognizeSimultaneously` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Application.PanGestureRecognizerShouldRecognizeSimultaneouslyProperty)添付プロパティを`true`:

```xaml
<Application ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Application.PanGestureRecognizerShouldRecognizeSimultaneously="true">
    ...
</Application>
```

代わりに、fluent API を使用して c# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

Xamarin.Forms.Application.Current.On<iOS>().SetPanGestureRecognizerShouldRecognizeSimultaneously(true);
```

`Application.On<iOS>`メソッドは、このプラットフォーム仕様が iOS上 でのみ動作することを指定します。  [ `Application.SetPanGestureRecognizerShouldRecognizeSimultaneously` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Application.SetPanGestureRecognizerShouldRecognizeSimultaneously(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Application},System.Boolean))メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)名前空間を使用して、スクロール ビューでのパン ジェスチャ認識エンジンは、パン ジェスチャのキャプチャまたはキャプチャおよびパンを共有するかどうかジェスチャのビューをスクロールします。 さらに、 [ `Application.GetPanGestureRecognizerShouldRecognizeSimultaneously` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Application.GetPanGestureRecognizerShouldRecognizeSimultaneously(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Application}))メソッドを使用して、パン ジェスチャは、スクロール ビューを含むと共有されているかどうかを返す、 [ `PanGestureRecognizer`](xref:Xamarin.Forms.PanGestureRecognizer)します。

そのため、有効にすると、このプラットフォーム固有で、 [ `ListView` ](xref:Xamarin.Forms.ListView)が含まれています、 [ `PanGestureRecognizer`](xref:Xamarin.Forms.PanGestureRecognizer)の両方を`ListView`、`PanGestureRecognizer`パン ジェスチャが表示されますおよびそれを処理します。 ただし、無効にすると、このプラットフォーム固有で、`ListView`が含まれています、 `PanGestureRecognizer`、`PanGestureRecognizer`をパン ジェスチャをキャプチャしてそれを処理し、`ListView`パン ジェスチャを受信しません。

## <a name="related-links"></a>関連リンク

- [プラットフォーム仕様 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [プラットフォーム仕様の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
