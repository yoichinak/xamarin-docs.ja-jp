---
ms.openlocfilehash: f9bacad9732a47a143aadb3efa9096b51a22f97a
ms.sourcegitcommit: a5a5c5de7d04f046a64e4875e180fc93227bf495
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/21/2021
ms.locfileid: "98634779"
---
[`StackLayout`](xref:Xamarin.Forms.StackLayout) 内の子ビューのサイズと位置は、子ビューの [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) プロパティと [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) プロパティの値、および [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) プロパティと [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) プロパティの値によって異なります。

[`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) プロパティと [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) プロパティは、2 つのレイアウト設定をカプセル化する [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) 構造体のフィールドに設定できます。

- **配置** – 子ビューの優先配置で、親レイアウト内での位置とサイズを決定します。
- **展開** – 追加スペースが使用可能な場合に子ビューが追加スペースを使用する必要があるかどうかを示します ([`StackLayout`](xref:Xamarin.Forms.StackLayout) によってのみ使用されます)。

# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

1. **MainPage.xaml** で、[`StackLayout`](xref:Xamarin.Forms.StackLayout) 宣言を変更して、各 [`Label`](xref:Xamarin.Forms.Label) の配置オプションと展開オプションを設定します。

    ```xaml
    <StackLayout Margin="20,35,20,25">
        <Label Text="Start"
               HorizontalOptions="Start"
               BackgroundColor="Gray" />
        <Label Text="Center"
               HorizontalOptions="Center"
               BackgroundColor="Gray" />
        <Label Text="End"
               HorizontalOptions="End"
               BackgroundColor="Gray" />
        <Label Text="Fill"
               HorizontalOptions="Fill"
               BackgroundColor="Gray" />
        <Label Text="StartAndExpand"
               VerticalOptions="StartAndExpand"
               BackgroundColor="Gray" />
        <Label Text="CenterAndExpand"
               VerticalOptions="CenterAndExpand"
               BackgroundColor="Gray" />
        <Label Text="EndAndExpand"
               VerticalOptions="EndAndExpand"
               BackgroundColor="Gray" />
        <Label Text="FillAndExpand"
               VerticalOptions="FillAndExpand"
               BackgroundColor="Gray" />
    </StackLayout>
    ```

    このコードは、最初の 4 つの [`Label`](xref:Xamarin.Forms.Label) インスタンスに配置設定を指定し、最後の 4 つの `Label` インスタンスに展開設定を指定します。 [`Start`](xref:Xamarin.Forms.LayoutOptions.Start)、[`Center`](xref:Xamarin.Forms.LayoutOptions.Center)、[`End`](xref:Xamarin.Forms.LayoutOptions.End)、[`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill) の各フィールドは、親 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 内の [`Label`](xref:Xamarin.Forms.Label) インスタンスの配置を定義するために使用されます。 [`StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand)、[`CenterAndExpand`](xref:Xamarin.Forms.LayoutOptions.CenterAndExpand)、[`EndAndExpand`](xref:Xamarin.Forms.LayoutOptions.EndAndExpand)、[`FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand) の各フィールドは、配置設定の定義と、親 `StackLayout` 内で追加のスペースが使用可能な場合にビューがより多くのスペースを占有するかどうかの定義を行うために使用されます。

    > [!NOTE]
    > ビューの [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) プロパティと [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) プロパティの既定値は[`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill) です。

1. アプリケーションがまだ実行されている場合は、変更内容をファイルに保存すると、アプリケーションのユーザー インターフェイスがシミュレーターまたはエミュレーターで自動的に更新されます。 もしくは、Visual Studio ツール バーで、 **[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択したリモート iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。

    [![iOS および Android 上の、配置オプションと展開オプションを設定した StackLayout の子ビューのスクリーンショット](../images/alignment-expansion.png "配置と展開を設定した、Label インスタンスを含む StackLayout")](../images/alignment-expansion-large.png#lightbox "配置と展開を設定した、Label インスタンスを含む StackLayout")

    [`StackLayout`](xref:Xamarin.Forms.StackLayout) は、`StackLayout` の方向とは反対方向にある子ビューの配置設定のみに従います。 したがって、垂直方向の `StackLayout` 内の [`Label`](xref:Xamarin.Forms.Label) 子ビューは、それらの [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) プロパティを次の配置フィールドの 1 つに設定します。

    - [`Start`](xref:Xamarin.Forms.LayoutOptions.Start)。これは [`Label`](xref:Xamarin.Forms.Label) を [`StackLayout`](xref:Xamarin.Forms.StackLayout) の左側に配置します。
    - [`Center`](xref:Xamarin.Forms.LayoutOptions.Center)。これは [`Label`](xref:Xamarin.Forms.Label) を [`StackLayout`](xref:Xamarin.Forms.StackLayout) の中央に配置します。
    - [`End`](xref:Xamarin.Forms.LayoutOptions.End)。これは [`Label`](xref:Xamarin.Forms.Label) を [`StackLayout`](xref:Xamarin.Forms.StackLayout) の右側に配置します。
    - [`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)。これにより、[`Label`](xref:Xamarin.Forms.Label) によって [`StackLayout`](xref:Xamarin.Forms.StackLayout) の幅が埋まるようになります。

    [`StackLayout`](xref:Xamarin.Forms.StackLayout) は子ビューをその方向にのみ展開できます。 したがって、垂直方向の `StackLayout` は、[`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) プロパティをいずれかの展開フィールドに設定する [`Label`](xref:Xamarin.Forms.Label) 子ビューを展開できます。 つまり、垂直方向の配置では、各 `Label` が `StackLayout` 内で同じ量のスペースを占有します。 ただし、[`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) プロパティを [`FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand) に設定する最後の `Label` のみ、サイズが異なります。

    > [!IMPORTANT]
    > [`StackLayout`](xref:Xamarin.Forms.StackLayout) 内のすべてのスペースが使用されている場合、展開設定は無効になります。

    Visual Studio で、アプリケーションを停止します。

    配置と展開の詳細については、「[Layout Options in Xamarin.Forms](~/xamarin-forms/user-interface/layouts/layout-options.md)」(Xamarin.Forms のレイアウト オプション) をご覧ください。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **MainPage.xaml** で、[`StackLayout`](xref:Xamarin.Forms.StackLayout) 宣言を変更して、各 [`Label`](xref:Xamarin.Forms.Label) の配置オプションと展開オプションを設定します。

    ```xaml
    <StackLayout Margin="20,35,20,25">
        <Label Text="Start"
               HorizontalOptions="Start"
               BackgroundColor="Gray" />
        <Label Text="Center"
               HorizontalOptions="Center"
               BackgroundColor="Gray" />
        <Label Text="End"
               HorizontalOptions="End"
               BackgroundColor="Gray" />
        <Label Text="Fill"
               HorizontalOptions="Fill"
               BackgroundColor="Gray" />
        <Label Text="StartAndExpand"
               VerticalOptions="StartAndExpand"
               BackgroundColor="Gray" />
        <Label Text="CenterAndExpand"
               VerticalOptions="CenterAndExpand"
               BackgroundColor="Gray" />
        <Label Text="EndAndExpand"
               VerticalOptions="EndAndExpand"
               BackgroundColor="Gray" />
        <Label Text="FillAndExpand"
               VerticalOptions="FillAndExpand"
               BackgroundColor="Gray" />
    </StackLayout>
    ```

    このコードは、最初の 4 つの [`Label`](xref:Xamarin.Forms.Label) インスタンスに配置設定を指定し、最後の 4 つの `Label` インスタンスに展開設定を指定します。 [`Start`](xref:Xamarin.Forms.LayoutOptions.Start)、[`Center`](xref:Xamarin.Forms.LayoutOptions.Center)、[`End`](xref:Xamarin.Forms.LayoutOptions.End)、[`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill) の各フィールドは、親 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 内の [`Label`](xref:Xamarin.Forms.Label) インスタンスの配置を定義するために使用されます。 [`StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand)、[`CenterAndExpand`](xref:Xamarin.Forms.LayoutOptions.CenterAndExpand)、[`EndAndExpand`](xref:Xamarin.Forms.LayoutOptions.EndAndExpand)、[`FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand) の各フィールドは、配置設定の定義と、親 `StackLayout` 内で追加のスペースが使用可能な場合にビューがより多くのスペースを占有するかどうかの定義を行うために使用されます。

    > [!NOTE]
    > ビューの [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) プロパティと [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) プロパティの既定値は[`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill) です。

1. アプリケーションがまだ実行されている場合は、変更内容をファイルに保存すると、アプリケーションのユーザー インターフェイスがシミュレーターまたはエミュレーターで自動的に更新されます。 もしくは、Visual Studio for Mac ツール バーで、 **[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択した iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。

    [![iOS および Android 上の、配置オプションと展開オプションを設定した StackLayout の子ビューのスクリーンショット](../images/alignment-expansion.png "配置と展開を設定した、Label インスタンスを含む StackLayout")](../images/alignment-expansion-large.png#lightbox "配置と展開を設定した、Label インスタンスを含む StackLayout")

    [`StackLayout`](xref:Xamarin.Forms.StackLayout) は、`StackLayout` の方向とは反対方向にある子ビューの配置設定のみに従います。 したがって、垂直方向の `StackLayout` 内の [`Label`](xref:Xamarin.Forms.Label) 子ビューは、それらの [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) プロパティを次の配置フィールドの 1 つに設定します。

    - [`Start`](xref:Xamarin.Forms.LayoutOptions.Start)。これは [`Label`](xref:Xamarin.Forms.Label) を [`StackLayout`](xref:Xamarin.Forms.StackLayout) の左側に配置します。
    - [`Center`](xref:Xamarin.Forms.LayoutOptions.Center)。これは [`Label`](xref:Xamarin.Forms.Label) を [`StackLayout`](xref:Xamarin.Forms.StackLayout) の中央に配置します。
    - [`End`](xref:Xamarin.Forms.LayoutOptions.End)。これは [`Label`](xref:Xamarin.Forms.Label) を [`StackLayout`](xref:Xamarin.Forms.StackLayout) の右側に配置します。
    - [`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)。これにより、[`Label`](xref:Xamarin.Forms.Label) によって [`StackLayout`](xref:Xamarin.Forms.StackLayout) の幅が埋まるようになります。

    [`StackLayout`](xref:Xamarin.Forms.StackLayout) は子ビューをその方向にのみ展開できます。 したがって、垂直方向の `StackLayout` は、[`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) プロパティをいずれかの展開フィールドに設定する [`Label`](xref:Xamarin.Forms.Label) 子ビューを展開できます。 つまり、垂直方向の配置では、各 `Label` が `StackLayout` 内で同じ量のスペースを占有します。 ただし、[`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) プロパティを [`FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand) に設定する最後の `Label` のみ、サイズが異なります。

    > [!IMPORTANT]
    > [`StackLayout`](xref:Xamarin.Forms.StackLayout) 内のすべてのスペースが使用されている場合、展開設定は無効になります。

    Visual Studio for Mac で、アプリケーションを停止します。

    配置と展開の詳細については、「[Layout Options in Xamarin.Forms](~/xamarin-forms/user-interface/layouts/layout-options.md)」(Xamarin.Forms のレイアウト オプション) をご覧ください。
