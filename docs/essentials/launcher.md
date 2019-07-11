---
title: Xamarin.Essentials ランチャー
description: Xamarin.Essentials の Launcher クラスを使用すると、アプリケーションがシステムで URI を開くことができるようになります。
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
ms.openlocfilehash: 26ab3687e9bcfecbd003ddd0a97f3e46a0cd92b2
ms.sourcegitcommit: 3489c281c9eb5ada2cddf32d73370943342a1082
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/18/2019
ms.locfileid: "58870054"
---
# <a name="xamarinessentials-launcher"></a>Xamarin.Essentials:ランチャー

**Launcher** クラスを使用すると、アプリケーションがシステムで URI を開くことができるようになります。 これは多くの場合、他のアプリケーションのカスタム URI スキームへのディープ リンクを設定するときに使用されます。 ブラウザーで Web サイトを開く場合は、**[Browser](open-browser.md)** API を参照する必要があります。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-launcher"></a>ランチャーの使用

自分のクラスに Xamarin.Essentials への参照を追加します。

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

## <a name="platform-differences"></a>プラットフォームによる違い

# <a name="androidtabandroid"></a>[Android](#tab/android)

`CanOpenAsync` から返されたタスクはすぐに完了します。

# <a name="iostabios"></a>[iOS](#tab/ios)

自分のアプリケーションから `OpenAsync` でこのデバイス上の目的のアプリケーションを開いたことがない場合、iOS によって、ユーザーは一度、自分のアプリがそれを開くことを許可するよう求められます。

`CanOpenAsync` から返されたタスクはすぐに完了します。

iOS の実装について詳しくは、[こちら](xref:UIKit.UIApplication.CanOpenUrl*)をご覧ください。

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

プラットフォームによる違いはありません。

-----

## <a name="api"></a>API

- [Launcher のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Launcher)
- [Launcher API ドキュメント](xref:Xamarin.Essentials.Launcher)
