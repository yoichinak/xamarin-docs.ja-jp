---
title: Xamarin.Essentials ランチャー
description: Xamarin.Essentials の Launcher クラスを使用すると、アプリケーションがシステムで URI を開くことができるようになります。
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 07/25/2018
ms.openlocfilehash: b9dc211a8b8b007e6975448a6c6c35f870021cc6
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/31/2018
ms.locfileid: "50674796"
---
# <a name="xamarinessentials-launcher"></a>Xamarin.Essentials: ランチャー

![プレリリースの NuGet](~/media/shared/pre-release.png)

**Launcher** クラスを使用すると、アプリケーションがシステムで URI を開くことができるようになります。 これは多くの場合、他のアプリケーションのカスタム URI スキームへのディープ リンクを設定するときに使用されます。 ブラウザーで Web サイトを開く場合は、**[Browser](open-browser.md)** API を参照する必要があります。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-launcher"></a>ランチャーの使用

自分のクラスの Xamarin.Essentials に参照を追加します。

```csharp
using Xamarin.Essentials;
```

ランチャーの機能を使用するには、`OpenAsync` メソッドを呼び出し、開く `string` または `Uri` を渡します。 必要に応じて、デバイス上のアプリケーションで URI スキーマを処理できるかどうかを確認するために `CanOpenAsync` メソッドを使用できます。

```csharp
public class LauncherTest
{
    public async Task OpenRideShareAsync()
    {
        var supportsUri = await Launcher.CanOpenAsync("lyft://");
        if (supportsUri)
            await Launcher.OpenAsync("lyft://ridetype?id=lyft_line");
    }
}
```

## <a name="platform-differences"></a>プラットフォームの差異

# <a name="androidtabandroid"></a>[Android](#tab/android)

`CanOpenAsync` から返されたタスクはすぐに完了します。

# <a name="iostabios"></a>[iOS](#tab/ios)

自分のアプリケーションから `OpenAsync` でこのデバイス上の目的のアプリケーションを開いたことがない場合、iOS によって、ユーザーは一度、自分のアプリがそれを開くことを許可するよう求められます。

`CanOpenAsync` から返されたタスクはすぐに完了します。

iOS の実装について詳しくは、[こちら](https://developer.xamarin.com/api/member/UIKit.UIApplication.CanOpenUrl/p/Foundation.NSUrl/)をご覧ください。

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

プラットフォームの差異はありません。

-----

## <a name="api"></a>API

- [Launcher のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Launcher)
- [Launcher API ドキュメント](xref:Xamarin.Essentials.Launcher)
