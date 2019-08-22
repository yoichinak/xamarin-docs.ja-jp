---
title: XAML プレビューアーでデザイン時データを使用する
description: この記事では、デザイン時データを使用して、アプリを実行せずに XAML プレビューアーにデータを多用するレイアウトを表示する方法について説明します。
ms.prod: xamarin
ms.assetid: 0F608019-5951-4BE6-80E0-9EEE1733D642
ms.technology: xamarin-forms
author: maddyleger1
ms.author: maleger
ms.date: 03/27/2019
ms.openlocfilehash: a6a34615adc9cf290ff6bf9dd344487e5f29cfa2
ms.sourcegitcommit: 5f972a757030a1f17f99177127b4b853816a1173
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/21/2019
ms.locfileid: "69887850"
---
# <a name="use-design-time-data-with-the-xaml-previewer"></a>XAML プレビューアーでデザイン時データを使用する

_一部のレイアウトでは、データなしで視覚化するのが困難です。これらのヒントを使用すると、データの多いページを XAML プレビューアーで最大限にプレビューすることができます。_

## <a name="design-time-data-basics"></a>デザイン時のデータの基礎

デザイン時データは、コントロールを XAML プレビューアーで簡単に視覚化できるように設定する偽のデータです。 まず、XAML ページのヘッダーに次のコード行を追加します。

```xaml
xmlns:d="http://xamarin.com/schemas/2014/forms/design"
xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
mc:Ignorable="d"
```

名前空間を追加した後、 `d:`任意の属性またはコントロールの前に配置して、XAML プレビューアーに表示することができます。 の`d:`要素は実行時に表示されません。

たとえば、通常、データがバインドされているラベルにテキストを追加できます。

```xaml
<Label Text="{Binding Name}" d:Text="Name!" />
```

[![ラベル内のテキストを使用したデザイン時データ](xaml-previewer-images/designtimedata-label-sm.png "テキストでラベルをデザインする時間データ")](xaml-previewer-images/designtimedata-label-lg.png#lightbox)

この例では、 `d:Text`を指定しないと、XAML プレビューアーによってラベルに対して何も表示されません。 代わりに、"Name!" と表示されます。 実行時に、ラベルに実際のデータが格納されます。

は、カラー `d:` 、フォントサイズ、空白文字など、Xamarin の任意の属性と共に使用できます。 コントロール自体に追加することもできます。

```xaml
<d:Button Text="Design Time Button" />
```

[![ボタンコントロールを使用したデザイン時データ](xaml-previewer-images/designtimedata-controls-sm.png "ボタンコントロールを使用したデザイン時データ")](xaml-previewer-images/designtimedata-controls-lg.png#lightbox)

この例では、ボタンはデザイン時にのみ表示されます。 このメソッドは、 [XAML プレビューアーでサポートされていないカスタムコントロール](render-custom-controls.md)のプレースホルダーをに配置するために使用します。

## <a name="preview-images-at-design-time"></a>デザイン時にイメージをプレビューする

ページにバインドされている、または動的に読み込まれたイメージのデザイン時のソースを設定できます。 Android プロジェクトで、XAML プレビューアーに表示するイメージを **[リソース >]** [作成] フォルダーに追加します。 IOS プロジェクトで、 **[リソース]** フォルダーにイメージを追加します。 その後、デザイン時に XAML プレビューアーにそのイメージを表示できます。

```xaml
<Image Source={Binding ProfilePicture} d:Source="DesignTimePicture.jpg" />
```

[![イメージを使用したデザイン時データ](xaml-previewer-images/designtimedata-image-sm.png "Iamges を使用したデザイン時データ")](xaml-previewer-images/designtimedata-image-lg.png#lightbox)

## <a name="design-time-data-for-listviews"></a>ListViews のデザイン時データ

ListViews は、モバイルアプリでデータを表示するための一般的な方法です。 ただし、実際のデータを使用せずに視覚化することは困難です。 デザイン時のデータを使用するには、ItemsSource として使用するデザイン時の配列を作成する必要があります。 XAML プレビューアーでは、デザイン時に ListView 内のその配列の内容が表示されます。

```xaml
<StackLayout>
    <ListView ItemsSource="{Binding Items}">
        <d:ListView.ItemsSource>
            <x:Array Type="{x:Type x:String}">
                <x:String>Item One</x:String>
                <x:String>Item Two</x:String>
                <x:String>Item Three</x:String>
            </x:Array>
        </d:ListView.ItemsSource>
        <ListView.ItemTemplate>
            <DataTemplate>
                <TextCell Text="{Binding ItemName}"
                          d:Text="{Binding .}" />
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</StackLayout>
```

[![ListView を使用したデザイン時データ](xaml-previewer-images/designtimedata-itemssource-sm.png "ListView を使用したデザイン時データ")](xaml-previewer-images/designtimedata-itemssource-lg.png#lightbox)

この例では、XAML プレビューアー内の3つの TextCells の ListView が表示されます。 プロジェクト内の`x:String`既存のデータモデルに変更できます。

より複雑な例については、 [James Montemagno のマン Selman. Forms アプリ](https://github.com/jamesmontemagno/Hanselman.Forms/blob/vnext/src/Hanselman/Views/Podcasts/PodcastDetailsPage.xaml#L26-L47)を参照してください。

## <a name="alternative-hardcode-a-static-viewmodel"></a>ソリューション静的ビューモデルをハードコーディングする

デザイン時のデータを個々のコントロールに追加しない場合は、ページにバインドするためのモックデータストアを設定できます。 XAMLの静的なViewModelにバインドする方法については、James Montemagnoの[デザイン時のデータの追加に関するブログ記事](http://motzcod.es/post/143702671962/xamarinforms-xaml-previewer-design-time-data)を参照してください。

## <a name="troubleshooting"></a>トラブルシューティング

### <a name="requirements"></a>必要条件

デザイン時データには、Xamarin. Forms 3.6 の最小バージョンが必要です。

### <a name="intellisense-shows-squiggly-lines-under-my-design-time-data"></a>IntelliSense でデザイン時データの下に波線が表示される

これは既知の問題であり、今後のバージョンの Visual Studio で修正される予定です。 プロジェクトはエラーなしでビルドされます。

### <a name="the-xaml-previewer-stopped-working"></a>XAML プレビューアーが動作を停止しました

XAML ファイルを閉じて再度開き、プロジェクトのクリーンとリビルドを試してみてください。
