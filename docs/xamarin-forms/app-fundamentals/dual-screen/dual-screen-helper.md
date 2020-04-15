---
title: Xamarin.Forms のデュアル画面のプラットフォーム ヘルパー
description: このガイドでは、Xamarin.Forms の DualScreenHelper クラスを使用して Surface Duo や Surface Neo などのデュアル画面デバイスのアプリ エクスペリエンスを最適化する方法について説明します。
ms.prod: xamarin
ms.assetid: 5aa184c2-5611-427d-85c7-1c56486c3e1b
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 02/08/2020
ms.openlocfilehash: 9ab6106b8b92660d416e8a22cc3b1bdf1b41c5cf
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "80628277"
---
# <a name="xamarinforms-dual-screen-platform-helpers"></a>Xamarin.Forms のデュアル画面のプラットフォーム ヘルパー

![](~/media/shared/preview.png "This API is currently pre-release")

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-dualscreendemos/)

`DualScreenHelper` クラスを使用すると、デバイスでピクチャ イン ピクチャ モードがサポートされているかどうかを検出し、ピクチャ イン ピクチャ ウィンドウとして `ContentPage` を開くことができます。 作成モードの Neo デバイスで実行している場合は、ページが WonderBar に表示されます。

## <a name="properties"></a>プロパティ

- `HasCompactModeSupport` はプラットフォームが CompactMode で `ContentPage` を開くことができるかどうかを確認するために使用されます。
- プラットフォームでサポートされている場合は、`OpenCompactMode` で `ContentPage` が開きます。

## <a name="example"></a>例

```csharp
async void OpenCompactWindowClicked(object sender, EventArgs e)
{
    if (!DualScreenHelper.HasCompactModeSupport())
    {
        await DisplayAlert("Unsupported", "This platform doesn't support this feature", "Ok");
        return;
    }

    ContentPage page = new ContentPage() { BackgroundColor = Color.Purple };

    var button = new Button()
    {
        Text = "Close this Window"
    };

    var layout = new StackLayout()
    {
        Children =
        {
            new Label(){ Text = "Welcome to your Compact Mode Window" },
            button
        },
        BackgroundColor = Color.Yellow,
        HorizontalOptions = LayoutOptions.Fill,
        VerticalOptions = LayoutOptions.Fill
    };

    page.Content = new ScrollView() { Content = layout };
    var args = await DualScreenHelper.OpenCompactMode(page);

    button.Command = new Command(async () =>
    {
        await args.CloseAsync();
    });
}
```
