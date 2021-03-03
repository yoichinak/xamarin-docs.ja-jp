---
title: Xamarin.Forms RefreshView
description: は、スクロール可能な Xamarin.Forms RefreshView コンテンツに対してプルを更新する機能を提供するコンテナーコントロールです。
ms.prod: xamarin
ms.assetId: 58DBD23B-ADB9-40DA-B331-4DDB6E698990
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/19/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
- RefreshView
- Universal Windows Platform
ms.openlocfilehash: 470093465191897a56cd54a6edaf828afbf40e11
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93372879"
---
# <a name="xamarinforms-refreshview"></a>Xamarin.Forms RefreshView

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/userinterface-refreshviewdemo/)

は、スクロール可能な `RefreshView` コンテンツに対してプルを更新する機能を提供するコンテナーコントロールです。 したがって、の子は `RefreshView` 、、、など、スクロール可能なコントロールである必要があり [`ScrollView`](xref:Xamarin.Forms.ScrollView) [`CollectionView`](xref:Xamarin.Forms.CollectionView) [`ListView`](xref:Xamarin.Forms.ListView) ます。

`RefreshView` は次の特性を定義します。

- `Command``ICommand`更新がトリガーされたときに実行される、型の。
- `CommandParameter`: `object` 型、`Command` に渡されるパラメーター。
- `IsRefreshing`の `bool` 現在の状態を示す型の `RefreshView` 。
- `RefreshColor`型の `Color` 。更新中に表示される進行状況の円の色。

これらのプロパティは、[`BindableProperty`](xref:Xamarin.Forms.BindableProperty) オブジェクトが基になっています。つまり、これらは、データ バインディングの対象にすることができ、スタイルを設定できます。

> [!NOTE]
> では、 Universal Windows Platform のプル方向を `RefreshView` プラットフォーム固有ので設定できます。 詳細については、「 [ RefreshView プル方向](~/xamarin-forms/platform/windows/refreshview-pulldirection.md)」を参照してください。

## <a name="create-a-refreshview"></a>認証要求の処理に使用する RefreshView

次の例は、XAML でをインスタンス化する方法を示してい `RefreshView` ます。

```xaml
<RefreshView IsRefreshing="{Binding IsRefreshing}"
             Command="{Binding RefreshCommand}">
    <ScrollView>
        <FlexLayout Direction="Row"
                    Wrap="Wrap"
                    AlignItems="Center"
                    AlignContent="Center"
                    BindableLayout.ItemsSource="{Binding Items}"
                    BindableLayout.ItemTemplate="{StaticResource ColorItemTemplate}" />
    </ScrollView>
</RefreshView>
```

は、 `RefreshView` コードで作成することもできます。

```csharp
RefreshView refreshView = new RefreshView();
ICommand refreshCommand = new Command(() =>
{
    // IsRefreshing is true
    // Refresh data here
    refreshView.IsRefreshing = false;
});
refreshView.Command = refreshCommand;

ScrollView scrollView = new ScrollView();
FlexLayout flexLayout = new FlexLayout { ... };
scrollView.Content = flexLayout;
refreshView.Content = scrollView;
```

この例では、は、子がであるに対して `RefreshView` プルを更新する機能を提供し [`ScrollView`](xref:Xamarin.Forms.ScrollView) [`FlexLayout`](xref:Xamarin.Forms.FlexLayout) ます。 は、バインド可能 `FlexLayout` なレイアウトを使用して、項目のコレクションにバインドしてコンテンツを生成し、各項目の外観をで設定し [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) ます。 バインド可能なレイアウトの詳細については、「 [」の Xamarin.Forms 「バインド](~/xamarin-forms/user-interface/layouts/bindable-layouts.md)可能なレイアウト」を参照してください。

プロパティの値は `RefreshView.IsRefreshing` 、の現在の状態を示し `RefreshView` ます。 ユーザーによって更新がトリガーされると、このプロパティは自動的にに移行 `true` します。 更新が完了したら、プロパティをにリセットする必要があり `false` ます。

ユーザーが更新を開始すると、 `ICommand` プロパティによって定義されたが実行され `Command` ます。これにより、表示されている項目が更新されます。 更新が行われている間に、更新の視覚化が表示されます。これは、アニメーションの進行状況の円で構成されます。

[![IOS および Android での::: no loc (RefreshView)::: データの更新中のスクリーンショット](refreshview-images/default-progress-circle.png "::: なし (RefreshView)::: データを更新しています")](refreshview-images/default-progress-circle-large.png#lightbox "::: なし (RefreshView)::: データを更新しています")

> [!NOTE]
> 手動で `IsRefreshing` プロパティをに設定する `true` と、更新の視覚化がトリガーされ、プロパティによって定義されたが実行され `ICommand` `Command` ます。

## <a name="refreshview-appearance"></a>RefreshView 内容

は、クラスから継承されるプロパティに加えて `RefreshView` [`VisualElement`](xref:Xamarin.Forms.VisualElement) 、 `RefreshView` プロパティも定義し `RefreshColor` ます。 このプロパティを設定して、更新中に表示される進行状況の円の色を定義できます。

```xaml
<RefreshView RefreshColor="Teal"
             ... />
```

次のスクリーンショットは、プロパティが設定されたを示してい `RefreshView` `RefreshColor` ます。

[![IOS と Android での::: no loc (RefreshView)::: 青緑の進行状況の円のスクリーンショット](refreshview-images/teal-progress-circle.png "::: なし (RefreshView)::: 青緑のイナズママーク")](refreshview-images/teal-progress-circle-large.png#lightbox "::: なし (RefreshView)::: 青緑のイナズママーク")

さらに、プロパティは、 `BackgroundColor` [`Color`](xref:Xamarin.Forms.Color) 進行状況の円の背景色を表すに設定できます。

> [!NOTE]
> IOS では、 `BackgroundColor` プロパティは、進行状況の円を含むの背景色を設定し `UIView` ます。

## <a name="disable-a-refreshview"></a>を無効にする RefreshView

アプリケーションは、pull to refresh が有効な操作ではない状態になる場合があります。 このような場合は、 `RefreshView` プロパティをに設定することで、を無効にすることができ `IsEnabled` `false` ます。 これにより、ユーザーがプルをトリガーして更新できなくなります。

または、プロパティを定義するときに、のデリゲートを指定して、 `Command` `CanExecute` コマンドを `ICommand` 有効または無効にすることもできます。

## <a name="related-links"></a>関連リンク

- [RefreshView サンプル](/samples/xamarin/xamarin-forms-samples/userinterface-refreshviewdemo/)
- [バインド可能なレイアウト Xamarin.Forms](~/xamarin-forms/user-interface/layouts/bindable-layouts.md)
- [RefreshView プル方向プラットフォーム固有](~/xamarin-forms/platform/windows/refreshview-pulldirection.md)