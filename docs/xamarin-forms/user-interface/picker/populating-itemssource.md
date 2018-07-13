---
title: ピッカーの ItemsSource プロパティの設定
description: ピッカーの表示は、データの一覧から、テキスト項目を選択するためのコントロールです。 この記事では、ItemsSource プロパティを設定してデータの選択を設定する方法と、ユーザーが項目の選択に応答する方法について説明します。
ms.prod: xamarin
ms.assetid: 8ECF390C-9DB2-4441-B9A3-101AE7E5AEC5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/11/2017
ms.openlocfilehash: 3f82e4b7d52988bfef9736ace8c476a9cd2da02b
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994745"
---
# <a name="setting-a-pickers-itemssource-property"></a>ピッカーの ItemsSource プロパティの設定

_ピッカーの表示は、データの一覧から、テキスト項目を選択するためのコントロールです。この記事では、ItemsSource プロパティを設定してデータの選択を設定する方法と、ユーザーが項目の選択に応答する方法について説明します。_

Xamarin.Forms 2.3.4 が強化、 [ `Picker` ](xref:Xamarin.Forms.Picker)ビューを設定してデータを追加する機能を追加してその[ `ItemsSource` ](xref:Xamarin.Forms.Picker.ItemsSource)プロパティ、および、から選択した項目を取得するには[`SelectedItem` ](xref:Xamarin.Forms.Picker.SelectedItem)プロパティ。 選択した項目のテキストの色を変更して、設定してさらに、 [ `TextColor` ](xref:Xamarin.Forms.Picker.TextColor)プロパティを[ `Color`](xref:Xamarin.Forms.Color)します。

## <a name="populating-a-picker-with-data"></a>データの選択の設定

A [ `Picker` ](xref:Xamarin.Forms.Picker)を設定してデータを設定することができます、 [ `ItemsSource` ](xref:Xamarin.Forms.Picker.ItemsSource)プロパティを`IList`コレクション。 コレクション内の各項目ので、または型から派生する必要があります`object`します。 初期化することにより XAML で項目を追加できる、`ItemsSource`項目の配列からのプロパティ。

```xaml
<Picker x:Name="picker" Title="Select a monkey">
  <Picker.ItemsSource>
    <x:Array Type="{x:Type x:String}">
      <x:String>Baboon</x:String>
      <x:String>Capuchin Monkey</x:String>
      <x:String>Blue Monkey</x:String>
      <x:String>Squirrel Monkey</x:String>
      <x:String>Golden Lion Tamarin</x:String>
      <x:String>Howler Monkey</x:String>
      <x:String>Japanese Macaque</x:String>
    </x:Array>
  </Picker.ItemsSource>
</Picker>
```

> [!NOTE]
> なお、`x:Array`要素が必要です、`Type`配列内の項目の種類を示す属性です。

同等の c# コードは、以下に示します。

```csharp
var monkeyList = new List<string>();
monkeyList.Add("Baboon");
monkeyList.Add("Capuchin Monkey");
monkeyList.Add("Blue Monkey");
monkeyList.Add("Squirrel Monkey");
monkeyList.Add("Golden Lion Tamarin");
monkeyList.Add("Howler Monkey");
monkeyList.Add("Japanese Macaque");

var picker = new Picker { Title = "Select a monkey" };
picker.ItemsSource = monkeyList;
```

## <a name="responding-to-item-selection"></a>項目の選択への応答

A [ `Picker` ](xref:Xamarin.Forms.Picker)が一度に 1 つの項目の選択をサポートします。 ユーザーが、項目を選択すると、 [ `SelectedIndexChanged` ](xref:Xamarin.Forms.Picker.SelectedIndexChanged)イベントの起動、 [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex)プロパティの一覧で選択した項目のインデックスを表す整数が更新と[`SelectedItem` ](xref:Xamarin.Forms.Picker.SelectedItem)プロパティが更新され、`object`選択した項目を表します。 [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex)プロパティは、ユーザーが選択した項目を示す 0 から始まる番号。 項目が選択されていない場合ですが、ときに、 [ `Picker` ](xref:Xamarin.Forms.Picker)が最初に作成され、初期化、`SelectedIndex`は-1 になります。

> [!NOTE]
> 項目の選択動作を[ `Picker` ](xref:Xamarin.Forms.Picker)プラットフォーム固有の iOS 上でカスタマイズできます。 詳細については、「[Picker のアイテム選択の制御](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#picker_update_mode)」を参照してください。

次のコード例は、取得する方法を示します、 [ `SelectedItem` ](xref:Xamarin.Forms.Picker.SelectedItem)プロパティの値から、 [ `Picker` ](xref:Xamarin.Forms.Picker) XAML で。

```xaml
<Label Text="{Binding Source={x:Reference picker}, Path=SelectedItem}" />
```

同等の c# コードは、以下に示します。

```csharp
var monkeyNameLabel = new Label();
monkeyNameLabel.SetBinding(Label.TextProperty, new Binding("SelectedItem", source: picker));
```

イベント ハンドラーは、さらに、実行すると実行、 [ `SelectedIndexChanged` ](xref:Xamarin.Forms.Picker.SelectedIndexChanged)イベントが発生します。

```csharp
void OnPickerSelectedIndexChanged(object sender, EventArgs e)
{
  var picker = (Picker)sender;
  int selectedIndex = picker.SelectedIndex;

  if (selectedIndex != -1)
  {
    monkeyNameLabel.Text = (string)picker.ItemsSource[selectedIndex];
  }
}
```

このメソッドは、取得、 [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex)プロパティの値は、値から選択した項目を取得して、 [ `ItemsSource` ](xref:Xamarin.Forms.Picker.ItemsSource)コレクション。 これは、機能的から選択した項目を取得するのには、 [ `SelectedItem` ](xref:Xamarin.Forms.Picker.SelectedItem)プロパティ。 なお内の各項目、`ItemsSource`型のコレクションは、`object`にキャストする必要がありますので、`string`表示用です。

> [!NOTE]
> A [ `Picker` ](xref:Xamarin.Forms.Picker)設定して、特定のアイテムを表示する初期化することができます、 [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex)または[ `SelectedItem` ](xref:Xamarin.Forms.Picker.SelectedItem)プロパティ。 ただし、これらのプロパティを初期化した後に設定する必要があります、 [ `ItemsSource` ](xref:Xamarin.Forms.Picker.ItemsSource)コレクション。

## <a name="populating-a-picker-with-data-using-data-binding"></a>データ バインディングを使用してデータの選択の設定

A [ `Picker` ](xref:Xamarin.Forms.Picker)も設定できますデータにバインドするデータ バインディングを使用してその[ `ItemsSource` ](xref:Xamarin.Forms.Picker.ItemsSource)プロパティを`IList`コレクション。 XAML 内でこれは、 [ `Binding` ](xref:Xamarin.Forms.Xaml.BindingExtension)マークアップ拡張機能。

```xaml
<Picker Title="Select a monkey" ItemsSource="{Binding Monkeys}" ItemDisplayBinding="{Binding Name}" />
```

同等の c# コードは、以下に示します。

```csharp
var picker = new Picker { Title = "Select a monkey" };
picker.SetBinding(Picker.ItemsSourceProperty, "Monkeys");
picker.ItemDisplayBinding = new Binding("Name");
```

[ `ItemsSource` ](xref:Xamarin.Forms.Picker.ItemsSource)プロパティ データにバインド、`Monkeys`を返す接続されているビュー モデルのプロパティ、`IList<Monkey>`コレクション。 次のコード例は、`Monkey`クラスは、4 つのプロパティが含まれています。

```csharp
public class Monkey
{
  public string Name { get; set; }
  public string Location { get; set; }
  public string Details { get; set; }
  public string ImageUrl { get; set; }
}
```

オブジェクトの一覧にバインドする場合、 [ `Picker` ](xref:Xamarin.Forms.Picker)の各オブジェクトから表示するプロパティを伝える必要があります。 これを設定することで実現されます、 [ `ItemDisplayBinding` ](xref:Xamarin.Forms.Picker.ItemDisplayBinding)の各オブジェクトから必要なプロパティをプロパティ。 上のコード例で、`Picker`各表示に設定されている`Monkey.Name`プロパティの値。

### <a name="responding-to-item-selection"></a>項目の選択への応答

データ バインディングを使用オブジェクトを設定することができます、 [ `SelectedItem` ](xref:Xamarin.Forms.Picker.SelectedItem)プロパティの値を変更するとき。

```xaml
<Picker Title="Select a monkey"
        ItemsSource="{Binding Monkeys}"
        ItemDisplayBinding="{Binding Name}"
        SelectedItem="{Binding SelectedMonkey}" />
<Label Text="{Binding SelectedMonkey.Name}" ... />
<Label Text="{Binding SelectedMonkey.Location}" ... />
<Image Source="{Binding SelectedMonkey.ImageUrl}" ... />
<Label Text="{Binding SelectedMonkey.Details}" ... />
```

同等の c# コードは、以下に示します。

```csharp
var picker = new Picker { Title = "Select a monkey" };
picker.SetBinding(Picker.ItemsSourceProperty, "Monkeys");
picker.SetBinding(Picker.SelectedItemProperty, "SelectedMonkey");
picker.ItemDisplayBinding = new Binding("Name");

var nameLabel = new Label { ... };
nameLabel.SetBinding(Label.TextProperty, "SelectedMonkey.Name");

var locationLabel = new Label { ... };
locationLabel.SetBinding(Label.TextProperty, "SelectedMonkey.Location");

var image = new Image { ... };
image.SetBinding(Image.SourceProperty, "SelectedMonkey.ImageUrl");

var detailsLabel = new Label();
detailsLabel.SetBinding(Label.TextProperty, "SelectedMonkey.Details");
```

[ `SelectedItem` ](xref:Xamarin.Forms.Picker.SelectedItem)プロパティ データにバインド、`SelectedMonkey`プロパティの種類は、接続されているビュー モデルの`Monkey`します。 そのため、ユーザーが内の項目を選択すると、 [ `Picker` ](xref:Xamarin.Forms.Picker)、`SelectedMonkey`プロパティが設定されます、選択した`Monkey`オブジェクト。 `SelectedMonkey`オブジェクト データが、ユーザー インターフェイスに表示される[ `Label` ](xref:Xamarin.Forms.Label)と[ `Image` ](xref:Xamarin.Forms.Image)ビュー。

![](populating-itemssource-images/monkeys.png "選択項目の選択")

> [!NOTE]
> なお、 [ `SelectedItem` ](xref:Xamarin.Forms.Picker.SelectedItem)と[ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex)プロパティの両方が既定で双方向のバインディングをサポートします。

## <a name="summary"></a>まとめ

[ `Picker` ](xref:Xamarin.Forms.Picker)ビューは、データの一覧から、テキスト項目を選択するコントロール。 この記事の説明を設定する方法、`Picker`にデータを設定、 [ `ItemsSource` ](xref:Xamarin.Forms.Picker.ItemsSource)プロパティ、およびユーザーが項目の選択に応答する方法。 このアプローチでは、Xamarin.Forms 2.3.4 で導入されたが対話するための推奨される方法、`Picker`します。


## <a name="related-links"></a>関連リンク

- [ピッカーのデモ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PickerDemo/)
- [Monkey アプリ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/MonkeyAppPicker/)
- [バインド可能なピッカー (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BindablePicker/)
- [ピッカー](xref:Xamarin.Forms.Picker)
