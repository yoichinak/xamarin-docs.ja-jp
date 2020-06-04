---
title: Xamarin.Forms のデュアル画面のプラットフォーム ヘルパー
description: このガイドでは、Xamarin.Forms の DualScreenHelper クラスを使用して Surface Duo や Surface Neo などのデュアル画面デバイスのアプリ エクスペリエンスを最適化する方法について説明します。
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: d9daf5a24c0dcfd07d529955c411259f4c1359df
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2020
ms.locfileid: "84138945"
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
