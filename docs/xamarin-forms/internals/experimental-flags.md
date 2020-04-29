---
title: Xamarin. フォームの実験的なフラグ
description: Xamarin の実験的なフラグを使用すると、エンジニアリングチームは、安定したリリースに移行する前に機能 Api を変更できるだけでなく、ユーザーに新しい機能をより迅速に出荷できます。
ms.prod: xamarin
ms.assetid: AF4BDD27-89F6-48AE-A8CD-D7E4DDA2CCA2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/14/2020
ms.openlocfilehash: cca377a7a88599bc34fd66695ad303162e6be200
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/29/2020
ms.locfileid: "82516546"
---
# <a name="xamarinforms-experimental-flags"></a>Xamarin. フォームの実験的なフラグ

新しい Xamarin. Forms 機能が実装されると、実験用フラグの背後に配置されることがあります。 これにより、エンジニアリングチームは新しい機能をより迅速に提供できるようになりますが、安定したリリースに移行する前に機能 Api を変更することもできます。 その後、機能が安定したリリースに移行すると、実験的なフラグが削除されます。

Xamarin. フォームには、次の実験的なフラグが含まれています。

- `AppTheme_Experimental`
- `CarouselView_Experimental`
- `Expander_Experimental`
- `IndicatorView_Experimental`
- `Markup_Experimental`
- `MediaElement_Experimental`
- `RadioButton_Experimental`
- `Shell_UWP_Experimental`
- `StateTriggers_Experimental`
- `SwipeView_Experimental`

実験的なフラグの背後にある機能を使用するには、アプリケーションでフラグ (フラグ) を有効にする必要があります。 試験的なフラグを有効にするには、次の2つの方法があります。

- プラットフォームプロジェクトで実験的フラグ (フラグ) を有効にします。
- `App`クラスで実験的なフラグ (またはフラグ) を有効にします。

> [!WARNING]
> フラグを有効にせずに、実験的なフラグの背後にある機能を使用すると、アプリケーションでは、有効にする必要があるフラグを示す例外がスローされます。

## <a name="enable-flags-in-platform-projects"></a>プラットフォームプロジェクトでフラグを有効にする

`Xamarin.Forms.Forms.SetFlags`メソッドを使用して、プラットフォームプロジェクトで実験的なフラグを有効にすることができます。

```csharp
Xamarin.Forms.Forms.SetFlags("CarouselView_Experimental");
```

メソッド`SetFlags`は、iOS の`AppDelegate`クラス、ANDROID `MainActivity` `App`のクラス、UWP のクラスで呼び出す必要があります。

> [!IMPORTANT]
> プラットフォームプロジェクトで実験的フラグを有効にすることは、 `Forms.Init`メソッドが呼び出される前に行われる必要があります。

メソッド`Xamarin.Forms.Forms.SetFlags`は、 `string`配列引数を受け取ります。これにより、1つのメソッド呼び出しで複数の実験的なフラグを有効にすることができます。

```csharp
Xamarin.Forms.Forms.SetFlags(new string[] { "CarouselView_Experimental", "IndicatorView_Experimental", "SwipeView_Experimental" });
```

> [!WARNING]
> 後続の呼び出し`SetFlags`によって前回の呼び出しの結果が上書きされるため、メソッドを複数回呼び出すことはありません。

## <a name="enable-flags-in-your-app-class"></a>App クラスでフラグを有効にする

`Device.SetFlags`メソッドを使用して、共有コードプロジェクトの`App`クラスで実験的なフラグを有効にすることができます。

```csharp
Device.SetFlags(new string[]{ "MediaElement_Experimental" });
```

メソッド`Device.SetFlags`は`IReadOnlyList<string>`引数を受け取ります。これにより、1つのメソッド呼び出しで複数の実験的なフラグを有効にすることができます。

```csharp
Device.SetFlags(new string[]{ "CarouselView_Experimental", "MediaElement_Experimental", "SwipeView_Experimental" });
```

> [!WARNING]
> 後続の呼び出し`SetFlags`によって前回の呼び出しの結果が上書きされるため、メソッドを複数回呼び出すことはありません。
