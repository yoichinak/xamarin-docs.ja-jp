---
title: Xamarin. フォーム RefreshView
description: Xamarin. フォーム RefreshView は、スクロール可能なコンテンツに対してプルを更新する機能を提供するコンテナーコントロールです。
ms.prod: xamarin
ms.assetId: 58DBD23B-ADB9-40DA-B331-4DDB6E698990
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/19/2019
ms.openlocfilehash: e38987006025dad1c2ff49c3ea8916e2075d61d7
ms.sourcegitcommit: d1d4700b3b1b417a9d7b7da85ab5d28f8e8e599d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/06/2019
ms.locfileid: "73649293"
---
# <a name="xamarinforms-refreshview"></a>Xamarin. フォーム RefreshView

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-refreshviewdemo/)

`RefreshView` は、スクロール可能なコンテンツに対してプルを更新する機能を提供するコンテナーコントロールです。 したがって、`RefreshView` の子は、 [`ScrollView`](xref:Xamarin.Forms.ScrollView)、 [`CollectionView`](xref:Xamarin.Forms.CollectionView)、 [`ListView`](xref:Xamarin.Forms.ListView)などのスクロール可能なコントロールである必要があります。

`RefreshView` は、次のプロパティを定義します。

- `ICommand` 型の `Command`。更新がトリガーされたときに実行されます。
- `CommandParameter`: `object` 型、`Command`に渡されるパラメーターです。
- `bool` 型の `IsRefreshing`。 `RefreshView` の現在の状態を示します。
- `Color` 型の `RefreshColor`、更新中に表示される進行状況の円の色です。

これらのプロパティは[`BindableProperty`](xref:Xamarin.Forms.BindableProperty)のオブジェクトによって支えられています。これは、データバインディングのターゲットとスタイルを設定できることを意味します。

> [!NOTE]
> ユニバーサル Windows プラットフォームでは、プラットフォーム固有の `RefreshView` のプル方向を設定できます。 詳細については、「 [Refreshview Pull Direction](~/xamarin-forms/platform/windows/refreshview-pulldirection.md)」を参照してください。

## <a name="create-a-refreshview"></a>RefreshView を作成する

次の例は、XAML で `RefreshView` をインスタンス化する方法を示しています。

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

コードでは、`RefreshView` を作成することもできます。

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

この例では、`RefreshView` は、子が[`FlexLayout`](xref:Xamarin.Forms.FlexLayout)である[`ScrollView`](xref:Xamarin.Forms.ScrollView)にプルを更新する機能を提供します。 `FlexLayout` は、バインド可能なレイアウトを使用して、項目のコレクションにバインドしてコンテンツを生成し、各項目の外観を[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)で設定します。 バインド可能なレイアウトの詳細については、「 [Xamarin. Forms でのバインド](~/xamarin-forms/user-interface/layouts/bindable-layouts.md)可能なレイアウト」を参照してください。

`RefreshView.IsRefreshing` プロパティの値は、`RefreshView`の現在の状態を示します。 ユーザーによって更新がトリガーされると、このプロパティは自動的に `true` に移行します。 更新が完了したら、プロパティを `false` にリセットする必要があります。

ユーザーが更新を開始すると、`Command` プロパティによって定義された `ICommand` が実行されます。これにより、表示される項目が更新されます。 更新が行われている間に、更新の視覚化が表示されます。これは、アニメーションの進行状況の円で構成されます。

[![IOS と Android での RefreshView によるデータの更新のスクリーンショット](refreshview-images/default-progress-circle.png "RefreshView によるデータの更新")](refreshview-images/default-progress-circle-large.png#lightbox "RefreshView によるデータの更新")

> [!NOTE]
> `IsRefreshing` プロパティを手動で `true` に設定すると、更新視覚化がトリガーされ、`Command` プロパティによって定義された `ICommand` が実行されます。

## <a name="refreshview-appearance"></a>RefreshView の外観

[`VisualElement`](xref:Xamarin.Forms.VisualElement)クラスから継承 `RefreshView` プロパティに加えて、`RefreshView` `RefreshColor` プロパティも定義します。 このプロパティを設定して、更新中に表示される進行状況の円の色を定義できます。

```xaml
<RefreshView RefreshColor="Teal"
             ... />
```

次のスクリーンショットは、`RefreshColor` プロパティが設定されている `RefreshView` を示しています。

[![IOS と Android で、青緑の進行状況を示す RefreshView のスクリーンショット](refreshview-images/teal-progress-circle.png "青緑の進行状況を示す RefreshView")](refreshview-images/teal-progress-circle-large.png#lightbox "青緑の進行状況を示す RefreshView")

また、`BackgroundColor` プロパティは、進行状況の円の背景色を表す[`Color`](xref:Xamarin.Forms.Color)に設定できます。

> [!NOTE]
> IOS では、[`BackgroundColor`] プロパティによって、進行状況の円を含む `UIView` の背景色が設定されます。

## <a name="disable-a-refreshview"></a>RefreshView を無効にする

アプリケーションは、pull to refresh が有効な操作ではない状態になる場合があります。 このような場合は、`IsEnabled` プロパティを `false` に設定することによって、`RefreshView` を無効にできます。 これにより、ユーザーがプルをトリガーして更新できなくなります。

または、`Command` プロパティを定義するときに、`ICommand` の `CanExecute` デリゲートを指定して、コマンドを有効または無効にすることもできます。

## <a name="related-links"></a>関連リンク

- [RefreshView (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-refreshviewdemo/)
- [Xamarin. Forms のバインド可能なレイアウト](~/xamarin-forms/user-interface/layouts/bindable-layouts.md)
- [RefreshView のプル方向のプラットフォーム固有](~/xamarin-forms/platform/windows/refreshview-pulldirection.md)
