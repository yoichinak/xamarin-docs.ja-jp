---
title: Xamarin.Forms の DualScreenHelper
description: このガイドでは、Xamarin.Forms の DualScreenHelper を使用して Surface Duo や Surface Neo などのデュアル画面デバイスのアプリ エクスペリエンスを最適化する方法について説明します。
ms.prod: xamarin
ms.assetid: 5aa184c2-5611-427d-85c7-1c56486c3e1b
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 02/08/2020
ms.openlocfilehash: b06e105560de635a21b42e8366bb47842372cf6a
ms.sourcegitcommit: 07941cf9704ff88cf4087de5ebdea623ff54edb1
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/11/2020
ms.locfileid: "77145546"
---
# <a name="xamarinforms-dualscreenhelper"></a>Xamarin.Forms の DualScreenHelper
[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://github.com/xamarin/xamarin-forms-samples/UserInterface/DualScreenDemos)
_DualScreenHelper を使用すると、デバイスでピクチャ イン ピクチャ モードがサポートされているかどうかを検出し、ピクチャ イン ピクチャ ウィンドウとして ContentPage を開くことができます。作成モードの Neo デバイスで実行している場合は、ページが WonderBar に表示されます。_
## <a name="properties"></a>プロパティ
- `HasCompactModeSupport` はプラットフォームが CompactMode で ContentPage を開くことができるかどうかを確認するために使用されます。
- `OpenCompactMode` がプラットフォームでサポートされている場合は、CompactMode で ContentPage が開きます。
## <a name="example-usage"></a>使用例
```c#
async void OpenCompactWindowClicked(object sender, EventArgs e)
{
    if(!DualScreenHelper.HasCompactModeSupport())
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