---
title: Android での ViewCell コンテキスト アクション
description: プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、ViewCell コンテキストアクションのレガシモードを有効にする、Android プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 8BD71B10-5037-443F-9975-B941CE393E5A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/24/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: e829c7f3763d25574b9ca70c1118ee9521633b48
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93368467"
---
# <a name="viewcell-context-actions-on-android"></a>Android での ViewCell コンテキスト アクション

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

4.3 から既定では、 Xamarin.Forms [`ViewCell`](xref:Xamarin.Forms.ViewCell) Android アプリケーションのがの各項目のコンテキストアクションを定義するときに、 [`ListView`](xref:Xamarin.Forms.ListView) で選択された項目が変更されると、コンテキストアクションメニューが更新され `ListView` ます。 ただし、以前のバージョンのでは、 Xamarin.Forms コンテキストアクションメニューは更新されていないため、この動作はレガシモードと呼ばれていました `ViewCell` 。 がを `ListView` 使用し [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) て、 `ItemTemplate` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 異なるコンテキストアクションを定義するオブジェクトからを設定する場合、このレガシモードでは正しくない動作が発生する可能性があります。

この Android プラットフォーム固有の機能により、 [`ViewCell`](xref:Xamarin.Forms.ViewCell) コンテキストアクションメニューレガシモードが有効になり、下位互換性が維持されるため、で選択した項目が変更されてもコンテキストアクションメニューは更新されません [`ListView`](xref:Xamarin.Forms.ListView) 。 これは、バインド可能なプロパティをに設定することによって XAML で使用され `ViewCell.IsContextActionsLegacyModeEnabled` `true` ます。

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

または、fluent API を使用して C# から使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

viewCell.On<Android>().SetIsContextActionsLegacyModeEnabled(true);
```

メソッドは、 `ViewCell.On<Android>` このプラットフォーム固有のが Android でのみ実行されることを指定します。 `ViewCell.SetIsContextActionsLegacyModeEnabled`名前空間のメソッドは、 [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) コンテキストアクションメニューレガシモードを有効にするために使用され [`ViewCell`](xref:Xamarin.Forms.ViewCell) ます。これにより、で選択された項目が変更されたときにコンテキストアクションメニューが更新されません [`ListView`](xref:Xamarin.Forms.ListView) 。 また、メソッドを使用して、 `ViewCell.GetIsContextActionsLegacyModeEnabled` コンテキストアクションのレガシモードが有効になっているかどうかを返すことができます。

次のスクリーンショットは、コンテキストアクションのレガシモードが有効であることを示してい [`ViewCell`](xref:Xamarin.Forms.ViewCell) ます。

![Android での ViewCell レガシモードが有効になっているスクリーンショット](viewcell-context-actions-images/legacy-mode-enabled.png "ViewCell レガシモードが有効")

このモードでは、セル2に対して異なるコンテキストメニュー項目が定義されているにもかかわらず、表示されているコンテキストアクションメニュー項目は、セル1とセル2で同一です。

次のスクリーンショットは、 [`ViewCell`](xref:Xamarin.Forms.ViewCell) 既定の動作である、レガシモードが無効になっているコンテキストアクションを示してい Xamarin.Forms ます。

![Android 上の ViewCell レガシモードが無効になっているスクリーンショット](viewcell-context-actions-images/legacy-mode-disabled.png "ViewCell レガシモードが無効です")

このモードでは、セル1とセル2に対して正しいコンテキストアクションメニュー項目が表示されます。

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific の AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)