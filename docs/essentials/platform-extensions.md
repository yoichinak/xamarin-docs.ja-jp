---
title: Xamarin.Essentials プラットフォーム拡張
description: Xamarin.Essentials からは、プラットフォームの種類を使用する必要があるとき、Rect、Size、Point など、プラットフォーム拡張メソッドがいくつか提供されます。
ms.assetid: AB4D198A-4FD7-479E-8627-01F887A6D056
author: jamesmontemagno
ms.author: jamont
ms.date: 03/13/2019
ms.openlocfilehash: 4e43159fb9cae6646be54d8efc24c334bc071477
ms.sourcegitcommit: fec87846fcb262fc8b79774a395908c8c8fc8f5b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/21/2020
ms.locfileid: "77545150"
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

## <a name="android-extensions"></a>Android の拡張機能

これらの拡張機能には、Android プロジェクトからのみアクセスできます。

### <a name="application-context--activity"></a>アプリケーション コンテキストおよびアクティビティ

`Platform` クラスのプラットフォーム拡張を使用すると、実行中のアプリの現在の `Context` または `Activity` にアクセスできます。

```csharp

var context = Platform.AppContext;

// Current Activity or null if not initialized or not started.
var activity = Platform.CurrentActivity;
```

`Activity` が必要であるものの、アプリケーションが完全に開始されていないという状況が発生する場合は、`WaitForActivityAsync` メソッドを使用する必要があります。

```csharp
var activity = await Platform.WaitForActivityAsync();
```

### <a name="activity-lifecycle"></a>アクティビティのライフサイクル

現在のアクティビティを取得するだけでなく、ライフサイクル イベントに登録することもできます。

```csharp
protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);

    Xamarin.Essentials.Platform.Init(this, bundle);

    Xamarin.Essentials.Platform.ActivityStateChanged += Platform_ActivityStateChanged;
}

protected override void OnDestroy()
{
    base.OnDestroy();
    Xamarin.Essentials.Platform.ActivityStateChanged -= Platform_ActivityStateChanged;
}

void Platform_ActivityStateChanged(object sender, Xamarin.Essentials.ActivityStateChangedEventArgs e) =>
    Toast.MakeText(this, e.State.ToString(), ToastLength.Short).Show();
```

アクティビティの状態は次のとおりです。

* Created
* Resumed
* Paused
* Destroyed
* SaveInstanceState
* Started
* Stopped

詳細については、「[アクティビティのライフサイクル](https://docs.microsoft.com/xamarin/android/app-fundamentals/activity-lifecycle/)」のドキュメントを参照してください。

## <a name="ios-extensions"></a>iOS の拡張機能

これらの拡張機能には、iOS プロジェクトからのみアクセスできます。

### <a name="current-uiviewcontroller"></a>現在の UIViewController

現在表示されている `UIViewController` にアクセスできるようにします。

```csharp
var vc = Platform.GetCurrentUIViewController();
```

`UIViewController` を検出できない場合、このメソッドでは `null` が返されます。

## <a name="cross-platform-extensions"></a>クロスプラットフォーム拡張機能

次の拡張機能は、すべてのプラットフォームで存在します。

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
