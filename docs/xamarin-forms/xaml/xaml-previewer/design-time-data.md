---
title: XAML プレビューアーでデザイン時のデータを使用します。
description: この記事では、デザイン時のデータを使用して、アプリを実行することがなく、XAML プレビューアーでデータ負荷の高いレイアウトを表示する方法について説明します。
ms.prod: xamarin
ms.assetid: 0F608019-5951-4BE6-80E0-9EEE1733D642
ms.technology: xamarin-forms
author: maddyleger1
ms.author: maleger
ms.date: 03/27/2019
ms.openlocfilehash: 60074c3c1b69a57d313ad0243246ba6db93dde3d
ms.sourcegitcommit: 0cb62b02a7efb5426f2356d7dbdfd9afd85f2f4a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/13/2019
ms.locfileid: "65557437"
---
# <a name="use-design-time-data-with-the-xaml-previewer"></a>XAML プレビューアーでデザイン時のデータを使用します。

_一部のレイアウトでは、データを視覚化する困難です。XAML プレビューアーで、データの量が多いページのプレビューを最大限に活用するには、これらのヒントを使用します。_

## <a name="design-time-data-basics"></a>デザイン時データの基礎

デザイン時のデータは、XAML プレビューアーでコントロールを簡単に視覚化できますを設定する偽のデータです。 開始するには、XAML ページのヘッダーに次のコード行を追加します。

```xaml
xmlns:d="http://xamarin.com/schemas/2014/forms/design"
xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
mc:Ignorable="d"
```

名前空間を追加した後に配置できます`d:`の属性または XAML プレビューアーで表示するコントロールの前にします。 要素を`d:`実行時に表示されません。

たとえば、通常は、データをバインドするラベルにテキストを追加できます。

```xaml
<Label Text="{Binding Name}" d:Text="Name!" />
```

[![デザイン時データ ラベルのテキストを](xaml-previewer-images/designtimedata-label-sm.png "デザイン時刻のテキスト データ ラベル")](xaml-previewer-images/designtimedata-label-lg.png#lightbox)

この例でなく`d:Text`、XAML プレビューアーには、ラベルについて何も表示されます。 代わりに、"Name!"を示します 場所ラベルは、実行時に実際のデータがあります。

使用することができます`d:`色、フォント サイズ、および間隔など、Xamarin.Forms コントロールのすべての属性。 コントロール自体にも追加できます。

```xaml
<d:Button Text="Design Time Button" />
```

[![時刻データをボタン コントロールをデザイン](xaml-previewer-images/designtimedata-controls-sm.png "ボタン コントロールと時刻のデータの設計")](xaml-previewer-images/designtimedata-controls-lg.png#lightbox)

この例で、ボタンはデザイン時にのみ表示されます。 このメソッドを使用して、プレース ホルダーを[XAML プレビューアーでサポートされていないカスタム コントロール](render-custom-controls.md)します。

## <a name="preview-images-at-design-time"></a>デザイン時にプレビュー イメージ

ページにバインドされているかに動的に読み込まれているイメージのデザイン時ソースを設定することができます。 Android プロジェクトで、追加する XAML プレビューアーで表示するイメージ、**リソース > ディスプレイ**フォルダー。 IOS プロジェクトで、イメージを追加、**リソース**フォルダー。 デザイン時に XAML プレビューアーでのイメージを表示できます。

```xaml
<Image Source={Binding ProfilePicture} d:Source="DesignTimePicture.jpg" />
```
[![デザイン時データ イメージを](xaml-previewer-images/designtimedata-image-sm.png "組み込まで時刻のデータの設計")](xaml-previewer-images/designtimedata-image-lg.png#lightbox)

## <a name="design-time-data-for-listviews"></a>Listview のデザイン時のデータ

Listview は、モバイル アプリでデータを表示する一般的な方法です。 ただし、これらはなく実際のデータを視覚化するが困難です。 でそれらをデザイン時のデータを使用するには、ItemsSource として使用するデザイン時の配列を作成する必要があります。 XAML プレビューアーでは、デザイン時に、ListView では、その配列があるものが表示されます。

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

[![時刻データを ListView の設計](xaml-previewer-images/designtimedata-itemssource-sm.png "時刻データを ListView の設計")](xaml-previewer-images/designtimedata-itemssource-lg.png#lightbox)

この例の 3 つ TextCells の ListView を XAML プレビューアーで表示されます。 変更することができます`x:String`プロジェクト内の既存のデータ モデルにします。

参照してください[James Montemagno 氏の Hanselman.Forms アプリ](https://github.com/jamesmontemagno/Hanselman.Forms/blob/vnext/src/Hanselman/Views/Podcasts/PodcastDetailsPage.xaml#L26-L47)より複雑な例です。

## <a name="alternative-hardcode-a-static-viewmodel"></a>代替方法:静的な ViewModel をハードコーディングします。

個々 のコントロールをデザイン時のデータを追加しない場合は、ページにバインドするモック データ ストアを設定できます。 XAMLの静的なViewModelにバインドする方法については、James Montemagnoの[デザイン時のデータの追加に関するブログ記事](http://motzcod.es/post/143702671962/xamarinforms-xaml-previewer-design-time-data)を参照してください。

## <a name="troubleshooting"></a>トラブルシューティング

### <a name="requirements"></a>必要条件

デザイン時のデータには、Xamarin.Forms 3.6 の最小バージョンが必要です。

### <a name="intellisense-shows-squiggly-lines-under-my-design-time-data"></a>IntelliSense は、デザイン時のデータは、下に波線を示しています。

既知の問題は、これは今後のバージョンの Visual Studio で修正されます。 プロジェクトがエラーなしまだビルドされます。

### <a name="the-xaml-previewer-stopped-working"></a>XAML プレビューアーの動作が停止しました

XAML ファイルをもう一度開いてとクリーニングを閉じてプロジェクトを再構築します。
