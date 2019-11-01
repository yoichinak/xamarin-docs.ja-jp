---
title: Android での ViewCell コンテキストアクション
description: プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、ViewCell コンテキストアクションのレガシモードを有効にする、Android プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 8BD71B10-5037-443F-9975-B941CE393E5A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/24/2019
ms.openlocfilehash: b040c7c5b7f132b0832469efba91dd89d6b2ddbd
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "72697747"
---
# <a name="viewcell-context-actions-on-android"></a>Android での ViewCell コンテキストアクション

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

4\.3 既定では、Android アプリケーションの[`ViewCell`](xref:Xamarin.Forms.ViewCell)が[`ListView`](xref:Xamarin.Forms.ListView)内の各項目のコンテキストアクションを定義するときに、`ListView` で選択されている項目が変更されると、コンテキストアクションメニューが更新されます。 ただし、以前のバージョンの Xamarin では、コンテキストアクションメニューは更新されなかったため、この動作は `ViewCell` レガシモードと呼ばれていました。 `ListView` が[`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)を使用して、異なるコンテキストアクションを定義する[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)オブジェクトから `ItemTemplate` を設定した場合、このレガシモードでは正しくない動作が発生する可能性があります。

この Android プラットフォーム固有の機能を使用すると、 [`ListView`](xref:Xamarin.Forms.ListView)内の選択した項目が変更されたときにコンテキストアクションメニューが更新されないように、下位互換性のために[`ViewCell`](xref:Xamarin.Forms.ViewCell)コンテキストアクションメニューレガシモードが有効になります。 XAML では、`ViewCell.IsContextActionsLegacyModeEnabled` バインド可能なプロパティを `true` に設定することによって使用されます。

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <ListView ItemsSource="{Binding Items}">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <ViewCell android:ViewCell.IsContextActionsLegacyModeEnabled="true">
                        <ViewCell.ContextActions>
                            <MenuItem Text="{Binding Item1Text}" />
                            <MenuItem Text="{Binding Item2Text}" />
                        </ViewCell.ContextActions>
                        <Label Text="{Binding Text}" />
                    </ViewCell>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </StackLayout>
</ContentPage>
```

または、fluent API C#を使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

viewCell.On<Android>().SetIsContextActionsLegacyModeEnabled(true);
```

@No__t_0 メソッドは、このプラットフォーム固有のが Android でのみ実行されることを指定します。 [@No__t_4](xref:Xamarin.Forms.ViewCell)コンテキストアクションメニューレガシモードを有効にするために、 [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)名前空間の `ViewCell.SetIsContextActionsLegacyModeEnabled` メソッドを使用します。これにより、 [`ListView`](xref:Xamarin.Forms.ListView)で選択した項目が変更されたときにコンテキストアクションメニューが更新されません。 また、`ViewCell.GetIsContextActionsLegacyModeEnabled` メソッドを使用して、コンテキストアクションのレガシモードが有効になっているかどうかを返すことができます。

次のスクリーンショットは[`ViewCell`](xref:Xamarin.Forms.ViewCell)コンテキストアクションのレガシモードが有効になっていることを示しています。

![Android での ViewCell レガシモードが有効になっているスクリーンショット](viewcell-context-actions-images/legacy-mode-enabled.png "ViewCell レガシモードが有効")

このモードでは、セル2に対して異なるコンテキストメニュー項目が定義されているにもかかわらず、表示されているコンテキストアクションメニュー項目は、セル1とセル2で同一です。

次のスクリーンショットは[`ViewCell`](xref:Xamarin.Forms.ViewCell)コンテキストアクションのレガシモードが無効になっていることを示しています。これは、既定の Xamarin. フォームの動作です。

![Android 上の ViewCell レガシモードが無効になっているスクリーンショット](viewcell-context-actions-images/legacy-mode-disabled.png "ViewCell レガシモードが無効です")

このモードでは、セル1とセル2に対して正しいコンテキストアクションメニュー項目が表示されます。

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific の AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
