---
ms.openlocfilehash: a7c2aa15521b89e4930746dc5421ce67fa26b2b9
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61382582"
---
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. **MainPage.xaml** で、子が垂直方法ではなく水平方向に配置されるように、[`StackLayout`](xref:Xamarin.Forms.StackLayout) 宣言を変更します。

    ```xaml
    <StackLayout Margin="20,35,20,25"
                 Orientation="Horizontal">
        <Label Text="The StackLayout has its Margin property set, to control the rendering position of the StackLayout." />
        <Label Text="The Padding property can be set to specify the distance between the StackLayout and its children." />
        <Label Text="The Spacing property can be set to specify the distance between views in the StackLayout." />
    </StackLayout>
    ```

    このコードでは、[`Orientation`](xref:Xamarin.Forms.StackLayout.Orientation) プロパティを [`Horizontal`](xref:Xamarin.Forms.StackOrientation.Horizontal) に設定します。

1. Visual Studio ツール バーで、**[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択したリモート iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。

    [![iOS と Android で、StackLayout で水平方向に配置された子ビューのスクリーンショット](../images/orientation.png "水平方向に配置された Label インスタンスを含む StackLayout")](../images/orientation-large.png#lightbox "水平方向に配置された Label インスタンスを含む StackLayout")

    [`StackLayout`](xref:Xamarin.Forms.StackLayout) 内の [`Label`](xref:Xamarin.Forms.Label) インスタンスが、垂直方向にではなく水平方向に配置されていることに注目してください。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **MainPage.xaml** で、子が垂直方法ではなく水平方向に配置されるように、[`StackLayout`](xref:Xamarin.Forms.StackLayout) 宣言を変更します。

    ```xaml
    <StackLayout Margin="20,35,20,25"
                 Orientation="Horizontal">
        <Label Text="The StackLayout has its Margin property set, to control the rendering position of the StackLayout." />
        <Label Text="The Padding property can be set to specify the distance between the StackLayout and its children." />
        <Label Text="The Spacing property can be set to specify the distance between views in the StackLayout." />
    </StackLayout>
    ```

    このコードでは、[`Orientation`](xref:Xamarin.Forms.StackLayout.Orientation) プロパティを [`Horizontal`](xref:Xamarin.Forms.StackOrientation.Horizontal) に設定します。

1. Visual Studio for Mac ツール バーで、**[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択した iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。

    [![iOS と Android で、StackLayout で水平方向に配置された子ビューのスクリーンショット](../images/orientation.png "水平方向に配置された Label インスタンスを含む StackLayout")](../images/orientation-large.png#lightbox "水平方向に配置された Label インスタンスを含む StackLayout")

    [`StackLayout`](xref:Xamarin.Forms.StackLayout) 内の [`Label`](xref:Xamarin.Forms.Label) インスタンスが、垂直方向にではなく水平方向に配置されていることに注目してください。
