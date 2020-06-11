---
title: "iOS のセルの背景色" 説明: "プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、ios のセルの既定の背景色を設定する iOS プラットフォーム固有のを使用する方法について説明します。
ms. 製品: xamarin ms. assetid: 2A3FDACF-5AE2-40DE-8488-6FE41733712F: xamarin-forms author: davidbritch ms. author: dabritch ms. date: 10/24/2018 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="cell-background-color-on-ios"></a>IOS のセルの背景色

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この iOS プラットフォーム固有のは、インスタンスの既定の背景色を設定 [`Cell`](xref:Xamarin.Forms.Cell) します。 これは、バインド可能なプロパティをに設定することによって XAML で使用され `Cell.DefaultBackgroundColor` [`Color`](xref:Xamarin.Forms.Color) ます。

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <ListView ItemsSource="{Binding GroupedEmployees}"
                  IsGroupingEnabled="true">
            <ListView.GroupHeaderTemplate>
                <DataTemplate>
                    <ViewCell ios:Cell.DefaultBackgroundColor="Teal">
                        <Label Margin="10,10"
                               Text="{Binding Key}"
                               FontAttributes="Bold" />
                    </ViewCell>
                </DataTemplate>
            </ListView.GroupHeaderTemplate>
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

または、fluent API を使用して C# から使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var viewCell = new ViewCell { View = ... };
viewCell.On<iOS>().SetDefaultBackgroundColor(Color.Teal);
```

メソッドは、 `ListView.On<iOS>` このプラットフォーム固有のが iOS 上でのみ実行されることを指定します。 `Cell.SetDefaultBackgroundColor`名前空間のメソッドは、 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) セルの背景色を指定されたに設定し [`Color`](xref:Xamarin.Forms.Color) ます。 また、メソッドを使用して、 `Cell.DefaultBackgroundColor` 現在のセルの背景色を取得することもできます。

結果として、の背景色を [`Cell`](xref:Xamarin.Forms.Cell) 特定のに設定でき [`Color`](xref:Xamarin.Forms.Color) ます。

[![IOS 上の青緑のグループヘッダーセルのスクリーンショット](cell-background-color-images/group-header-cell-color.png "緑色のグループヘッダーセルを含む ListView")](cell-background-color-images/group-header-cell-color-large.png#lightbox "緑色のグループヘッダーセルを含む ListView")

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
