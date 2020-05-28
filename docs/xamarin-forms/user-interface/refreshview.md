---
title: Xamarin.FormsRefreshView
description: Xamarin.FormsRefreshview は、スクロール可能なコンテンツに対してプルを更新する機能を提供するコンテナーコントロールです。
ms.prod: ''
ms.assetId: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: d84e6bb6ed41f2fbc213cd15051d071521f588cd
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2020
ms.locfileid: "84127596"
---
# <a name="xamarinforms-refreshview"></a>Xamarin.FormsRefreshView

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-refreshviewdemo/)

は、スクロール可能な `RefreshView` コンテンツに対してプルを更新する機能を提供するコンテナーコントロールです。 したがって、の子は `RefreshView` 、、、など、スクロール可能なコントロールである必要があり [`ScrollView`](xref:Xamarin.Forms.ScrollView) [`CollectionView`](xref:Xamarin.Forms.CollectionView) [`ListView`](xref:Xamarin.Forms.ListView) ます。

`RefreshView` は次の特性を定義します。

- `Command``ICommand`更新がトリガーされたときに実行される、型の。
- `CommandParameter`: `object` 型、`Command`に渡されるパラメーターです。
- `IsRefreshing`の `bool` 現在の状態を示す型の `RefreshView` 。
- `RefreshColor`型の `Color` 。更新中に表示される進行状況の円の色。

これらのプロパティは、オブジェクトによって支えられています [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 。これは、データバインディングのターゲットとスタイルを設定できることを意味します。

> [!NOTE]
> ユニバーサル Windows プラットフォームでは、プラットフォーム固有のを使用してのプル方向を `RefreshView` 設定できます。 詳細については、「 [Refreshview Pull Direction](~/xamarin-forms/platform/windows/refreshview-pulldirection.md)」を参照してください。

## <a name="create-a-refreshview"></a>RefreshView を作成する

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

[![IOS と Android での RefreshView によるデータの更新のスクリーンショット](refreshview-images/default-progress-circle.png "RefreshView によるデータの更新")](refreshview-images/default-progress-circle-large.png#lightbox "RefreshView によるデータの更新")

> [!NOTE]
> 手動で `IsRefreshing` プロパティをに設定する `true` と、更新の視覚化がトリガーされ、プロパティによって定義されたが実行され `ICommand` `Command` ます。

## <a name="refreshview-appearance"></a>RefreshView の外観

は、クラスから継承されるプロパティに加えて `RefreshView` [`VisualElement`](xref:Xamarin.Forms.VisualElement) 、 `RefreshView` プロパティも定義し `RefreshColor` ます。 このプロパティを設定して、更新中に表示される進行状況の円の色を定義できます。

```xaml
<RefreshView RefreshColor="Teal"
             ... />
```

次のスクリーンショットは、プロパティが設定されたを示してい `RefreshView` `RefreshColor` ます。

[![IOS と Android で、青緑の進行状況を示す RefreshView のスクリーンショット](refreshview-images/teal-progress-circle.png "青緑の進行状況を示す RefreshView")](refreshview-images/teal-progress-circle-large.png#lightbox "青緑の進行状況を示す RefreshView")

さらに、プロパティは、 `BackgroundColor` [`Color`](xref:Xamarin.Forms.Color) 進行状況の円の背景色を表すに設定できます。

> [!NOTE]
> IOS では、 `BackgroundColor` プロパティは、進行状況の円を含むの背景色を設定し `UIView` ます。

## <a name="disable-a-refreshview"></a>RefreshView を無効にする

アプリケーションは、pull to refresh が有効な操作ではない状態になる場合があります。 このような場合は、 `RefreshView` プロパティをに設定することで、を無効にすることができ `IsEnabled` `false` ます。 これにより、ユーザーがプルをトリガーして更新できなくなります。

または、プロパティを定義するときに、のデリゲートを指定して、 `Command` `CanExecute` コマンドを `ICommand` 有効または無効にすることもできます。

## <a name="related-links"></a>関連リンク

- [RefreshView (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-refreshviewdemo/)
- [バインド可能なレイアウトXamarin.Forms](~/xamarin-forms/user-interface/layouts/bindable-layouts.md)
- [RefreshView のプル方向のプラットフォーム固有](~/xamarin-forms/platform/windows/refreshview-pulldirection.md)
