---
title: Xamarin.Forms試験的なフラグ
description: Xamarin.Forms試験的なフラグを使用すると、エンジニアリングチームは新しい機能をより迅速に出荷できますが、安定したリリースに移行する前に機能 Api を変更することもできます。
ms.prod: xamarin
ms.assetid: AF4BDD27-89F6-48AE-A8CD-D7E4DDA2CCA2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/13/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 94fed78d7cf67ec5b6d783b1ced25a81266242d6
ms.sourcegitcommit: f7fe46c0236a7130b63a33d9d1670d5111582dd2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/13/2020
ms.locfileid: "88186110"
---
# <a name="no-locxamarinforms-experimental-flags"></a>Xamarin.Forms試験的なフラグ

新しい Xamarin.Forms 機能が実装されると、実験用フラグの背後に配置されることがあります。 これにより、エンジニアリングチームは新しい機能をより迅速に提供できるようになりますが、安定したリリースに移行する前に機能 Api を変更することもできます。 その後、機能が安定したリリースに移行すると、実験的なフラグが削除されます。

Xamarin.Formsには、次の実験的なフラグが含まれています。

- `Brush_Experimental`
- `CarouselView_Experimental`
- `DragAndDrop_Experimental`
- `Expander_Experimental`
- `Markup_Experimental`
- `MediaElement_Experimental`
- `RadioButton_Experimental`
- `Shapes_Experimental`
- `Shell_UWP_Experimental`
- `SwipeView_Experimental`

実験的なフラグの背後にある機能を使用するには、アプリケーションでフラグ (フラグ) を有効にする必要があります。 試験的なフラグを有効にするには、次の2つの方法があります。

- プラットフォームプロジェクトで実験的フラグ (フラグ) を有効にします。
- クラスで実験的なフラグ (またはフラグ) を有効にし `App` ます。

> [!WARNING]
> フラグを有効にせずに、実験的なフラグの背後にある機能を使用すると、アプリケーションでは、有効にする必要があるフラグを示す例外がスローされます。

## <a name="enable-flags-in-platform-projects"></a>プラットフォームプロジェクトでフラグを有効にする

`Xamarin.Forms.Forms.SetFlags`メソッドを使用して、プラットフォームプロジェクトで実験的なフラグを有効にすることができます。

```csharp
Xamarin.Forms.Forms.SetFlags("CarouselView_Experimental");
```

メソッドは、 `SetFlags` `AppDelegate` iOS のクラス、Android のクラス、UWP のクラスで呼び出す必要があり `MainActivity` `App` ます。

> [!IMPORTANT]
> プラットフォームプロジェクトで実験的フラグを有効にすることは、メソッドが呼び出される前に行われる必要があり `Forms.Init` ます。

メソッドは、 `Xamarin.Forms.Forms.SetFlags` `string` 配列引数を受け取ります。これにより、1つのメソッド呼び出しで複数の実験的なフラグを有効にすることができます。

```csharp
Xamarin.Forms.Forms.SetFlags(new string[] { "CarouselView_Experimental", "MediaElement_Experimental", "SwipeView_Experimental" });
```

> [!WARNING]
> 後続の呼び出しによっ `SetFlags` て前回の呼び出しの結果が上書きされるため、メソッドを複数回呼び出すことはありません。

## <a name="enable-flags-in-your-app-class"></a>App クラスでフラグを有効にする

`Device.SetFlags`メソッドを使用して、 `App` 共有コードプロジェクトのクラスで実験的なフラグを有効にすることができます。

```csharp
Device.SetFlags(new string[]{ "MediaElement_Experimental" });
```

`Device.SetFlags`メソッドは `IReadOnlyList<string>` 引数を受け取ります。これにより、1つのメソッド呼び出しで複数の実験的なフラグを有効にすることができます。

```csharp
Device.SetFlags(new string[]{ "CarouselView_Experimental", "MediaElement_Experimental", "SwipeView_Experimental" });
```

> [!WARNING]
> 後続の呼び出しによっ `SetFlags` て前回の呼び出しの結果が上書きされるため、メソッドを複数回呼び出すことはありません。

## <a name="old-experimental-flags"></a>古い実験的なフラグ

次の表は、一般公開されている機能の実験的なフラグと、実験的なフラグが削除されたリリースを示してい Xamarin.Forms ます。

| フラグ | Xamarin.Forms解除 |
| ---- | --------------------- |
| `AppTheme_Experimental` | 4.8 |
| `CollectionView_Experimental` | 4.3 |
| `FastRenderers_Experimental` | 4.0 |
| `IndicatorView_Experimental` | 4.7 |
| `Shell_Experimental` | 4.0  |
| `StateTriggers_Experimental` | 4.7 |
| `Visual_Experimental` | 3.6 |
