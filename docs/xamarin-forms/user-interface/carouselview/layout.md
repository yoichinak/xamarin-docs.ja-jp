---
title: CarouselView
description: 既定では、CarouselView の項目が横方向の一覧に表示されます。 ただし、垂直方向を含む CollectionView と同じレイアウトにもアクセスできます。
ms.prod: xamarin
ms.assetid: fede0382-c972-4023-a4ea-fe5cadec91a6
ms.technology: xamarin-forms
author: pauldipietro
ms.author: padipi
ms.date: 09/09/2019
ms.openlocfilehash: 5bbaeead524089ea604cfd9a9fd7f3a85d04e724
ms.sourcegitcommit: e83035c746f165ee6d03f2e9fd0066ee4f20a9fb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/11/2019
ms.locfileid: "70908344"
---
# <a name="xamarinforms-carouselview-layouts"></a>Xamarin. Forms CarouselView のレイアウト

![](~/media/shared/preview.png "この API は、現在プレリリースです")

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/CarouselViewDemos/)

## <a name="introduction"></a>概要

CarouselView で使用できるレイアウト機能の多くは、CollectionView からのものです。 さまざまなレイアウトの使用方法については、CollectionView の[レイアウトのドキュメント](../collectionview/layout.md)を参照してください。

## <a name="differences-from-collectionview"></a>CollectionView との違い

既定では、CarouselView 内の項目は、アプリのカルーセルの一般的な機能と同様に、水平方向に配置されます。

CarouselView には、いくつかの追加プロパティもあります。

> [!IMPORTANT]
> CarouselView のその他のプロパティは開発中であり、この一覧はまだ完了していません。

| API | 関数 |
|---|---|---|
| NumberOfSideItems | 現在の項目の各辺に表示される項目の数を設定します。 既定値は 0 です。
| PeekAreaInsets | 現在の項目に隣接する可視性のレベルを調整することによって、CarouselView に追加の項目があることをユーザーに視覚的に示す方法を提供します。

## <a name="setting-the-number-of-fully-visible-items"></a>完全に表示される項目の数の設定

既定では、CarouselView は、画面上に1つの項目全体を表示します。 ユーザーは`NumberOfSideItems`プロパティを設定して、現在のアイテムに隣接する項目をさらに表示できるようにすることができます。 に`PeekAreaInsets`設定された値は引き続き適用されることに注意してください。

```xaml
<StackLayout Margin="20">
    <CarouselView ItemsSource="{Binding Monkeys}" HeightRequest="125" NumberOfSideItems="1">
        <CarouselView.ItemTemplate>
            <DataTemplate>
                <Frame BorderColor="Black">
                    <Grid>
                        <Grid.RowDefinitions>
                            <RowDefinition Height="Auto" />
                            <RowDefinition Height="Auto" />
                        </Grid.RowDefinitions>
                        <Grid.ColumnDefinitions>
                            <ColumnDefinition Width="Auto" />
                            <ColumnDefinition Width="Auto" />
                        </Grid.ColumnDefinitions>
                        <Label Grid.Column="1"
                            Text="{Binding Name}"
                            FontAttributes="Bold"
                            FontSize="14"/>
                        <Label Grid.Row="1"
                            Grid.Column="1"
                            Text="{Binding Location}"
                            FontAttributes="Italic"
                            FontSize="12"
                            VerticalOptions="End" />
                    </Grid>
                </Frame>
            </DataTemplate>
        </CarouselView.ItemTemplate>
    </CarouselView>
</StackLayout>
```

[Android(carouselview-images/side-items.png "CarouselView 側の項目")を![含む CarouselView のスクリーンショット]](carouselview-images/side-items-large.png#lightbox "CarouselView 側の項目")

## <a name="making-adjacent-items-partially-visible"></a>隣接する項目を部分的に表示する

アプリで CarouselView を使用する場合、 `PeekAreaInsets`プロパティを0以外の値 (既定) に設定することによって、CarouselView がこのような方法で機能することをユーザーに示すと便利な場合があります。これは、画面に部分的に公開します。

```xaml
<StackLayout Margin="20">
  <CarouselView ItemsSource="{Binding Monkeys}" HeightRequest="125" PeekAreaInsets="100">
      <CarouselView.ItemTemplate>
          <DataTemplate>
              <Frame BorderColor="Black">
                  <Grid>
                      <Grid.RowDefinitions>
                          <RowDefinition Height="Auto" />
                          <RowDefinition Height="Auto" />
                      </Grid.RowDefinitions>
                      <Grid.ColumnDefinitions>
                          <ColumnDefinition Width="Auto" />
                          <ColumnDefinition Width="Auto" />
                      </Grid.ColumnDefinitions>
                      <Label Grid.Column="1"
                          Text="{Binding Name}"
                          FontAttributes="Bold"
                          FontSize="14"/>
                      <Label Grid.Row="1"
                          Grid.Column="1"
                          Text="{Binding Location}"
                          FontAttributes="Italic"
                          FontSize="12"
                          VerticalOptions="End" />
                  </Grid>
              </Frame>
          </DataTemplate>
      </CarouselView.ItemTemplate>
  </CarouselView>
</StackLayout>
```

[Android(carouselview-images/peek-area-insets.png "CarouselView 側の項目")を![含む CarouselView のスクリーンショット]](carouselview-images/peek-area-insets-large.png#lightbox "CarouselView 側の項目")

## <a name="related-links"></a>関連リンク

- [CollectionView (サンプル)](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/CarouselViewDemos/)
- [CollectionView layout のドキュメント](../collectionview/layout.md)
