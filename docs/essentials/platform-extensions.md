---
title: Xamarin.Essentials プラットフォーム拡張
description: Xamarin.Essentials からは、プラットフォームの種類を使用する必要があるとき、Rect、Size、Point など、プラットフォーム拡張メソッドがいくつか提供されます。
ms.assetid: AB4D198A-4FD7-479E-8627-01F887A6D056
author: jamesmontemagno
ms.author: jamont
ms.date: 03/13/2019
ms.openlocfilehash: 4a80c004dd55486db18a3149dcd889a4673d7438
ms.sourcegitcommit: 1c87135a47780f34102952d4b140850b4f08b075
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/26/2019
ms.locfileid: "74536501"
---
# <a name="xamarinessentials-platform-extensions"></a>Xamarin.Essentials:プラットフォーム拡張

Xamarin.Essentials からは、プラットフォームの種類を使用する必要があるとき、Rect、Size、Point など、プラットフォーム拡張メソッドがいくつか提供されます。 つまり、iOS、Android、UWP 固有の種類に対して、これらの種類の `System` バージョン間で変換できます。 

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-platform-extensions"></a>プラットフォーム拡張の使用

自分のクラスに Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

プラットフォーム拡張はすべて、iOS、Android、UWP プロジェクトからのみ呼び出すことができます。

### <a name="point"></a>ポイント

```csharp
var system = new System.Drawing.Point(x, y);

// Convert to CoreGraphics.CGPoint, Android.Graphics.Point, and Windows.Foundation.Point
var platform = system.ToPlatformPoint();

// Back to System.Drawing.Point
var system2 = platform.ToSystemPoint();
```

### <a name="size"></a>サイズ

```csharp
var system = new System.Drawing.Size(width, height);

// Convert to CoreGraphics.CGSize, Android.Util.Size, and Windows.Foundation.Size
var platform = system.ToPlatformSize();

// Back to System.Drawing.Size
var system2 = platform.ToSystemSize();
```

### <a name="rectangle"></a>Rectangle

```csharp
var system = new System.Drawing.Rectangle(x, y, width, height);

// Convert to CoreGraphics.CGRect, Android.Graphics.Rect, and Windows.Foundation.Rect
var platform = system.ToPlatformRectangle();

// Back to System.Drawing.Rectangle
var system2 = platform.ToSystemRectangle();
```

## <a name="api"></a>API

- [変換機能のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Types/PlatformExtensions)
- [Point Converters の API ドキュメント](xref:Xamarin.Essentials.PointExtensions)
- [Rectangle Converters の API ドキュメント](xref:Xamarin.Essentials.RectangleExtensions)
- [Size Converters の API ドキュメント](xref:Xamarin.Essentials.SizeExtensions)
