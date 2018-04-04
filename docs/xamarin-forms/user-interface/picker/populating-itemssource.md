---
title: ピッカーの ItemsSource プロパティの設定
description: ピッカー ビューは、データの一覧からテキスト アイテムを選択するためのコントロールです。 この記事では、ItemsSource プロパティを設定してデータの選択を設定する方法と、ユーザーが項目の選択に応答する方法について説明します。
ms.prod: xamarin
ms.assetid: 8ECF390C-9DB2-4441-B9A3-101AE7E5AEC5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/11/2017
ms.openlocfilehash: bf3940bc1bc0318bad4d785388f9dc9292af80ca
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="setting-a-pickers-itemssource-property"></a>ピッカーの ItemsSource プロパティの設定

_ピッカー ビューは、データの一覧からテキスト アイテムを選択するためのコントロールです。この記事では、ItemsSource プロパティを設定してデータの選択を設定する方法と、ユーザーが項目の選択に応答する方法について説明します。_

Xamarin.Forms 2.3.4 が強化され、 [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)ビューを設定して、データを設定する機能を追加してその[ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/)プロパティ、および、から選択した項目を取得するには[`SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedItem/)プロパティです。 選択した項目のテキストの色を変更して、設定してさらに、 [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.TextColor/)プロパティを[ `Color`](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/)です。

## <a name="populating-a-picker-with-data"></a>データ ピッカーを設定します。

A [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)データの設定によって設定することができます、 [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/)プロパティを`IList`コレクション。 コレクション内の各項目は、するか、型の派生元`object`です。 初期化することによって XAML で項目を追加することができます、`ItemsSource`配列項目のプロパティ。

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

同等の c# コードは、次に示します。

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

A [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)一度に 1 つの項目の選択をサポートします。 ユーザーが、項目を選択したときに、 [ `SelectedIndexChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Picker.SelectedIndexChanged/)イベントの起動、 [ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/)一覧で、選択した項目のインデックスを表す整数をプロパティが更新され、 [`SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedItem/)プロパティが更新され、`object`選択した項目を表すです。 [ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/)プロパティは、ユーザーが選択した項目を示す 0 から始まる番号。 項目が選択されていない場合は、ケースときに、 [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)が最初に作成され、初期化、`SelectedIndex`は-1 になります。

> [!NOTE]
> 項目の選択の動作で、 [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) iOS プラットフォーム固有の仕様と上でカスタマイズできます。 詳細については、次を参照してください。[の選択項目の選択を制御する](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#picker_update_mode)です。

次のコード例は、取得する方法を示します、 [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedItem/)からプロパティ値、 [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) XAML で。

```xaml
<Label Text="{Binding Source={x:Reference picker}, Path=SelectedItem}" />
```

同等の c# コードは、次に示します。

```csharp
var monkeyNameLabel = new Label();
monkeyNameLabel.SetBinding(Label.TextProperty, new Binding("SelectedItem", source: picker));
```

さらに、イベント ハンドラーができるときに実行、 [ `SelectedIndexChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Picker.SelectedIndexChanged/)イベントが発生します。

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

このメソッドは、取得、 [ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/)プロパティの値から選択した項目を取得する値を使用して、 [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/)コレクション。 これは、機能的にはから選択した項目を取得して、 [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedItem/)プロパティです。 注意してください内の各項目、`ItemsSource`コレクションの型は`object`にキャストする必要がありますので、`string`表示用です。

> [!NOTE]
> A [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)設定して、特定のアイテムを表示する初期化することができます、 [ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/)または[ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedItem/)プロパティです。 初期化した後、これらのプロパティを設定する必要があります、 [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/)コレクション。

## <a name="populating-a-picker-with-data-using-data-binding"></a>データ バインディングを使用してデータの選択を設定します。

A [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)できるものによって設定するデータにバインドするデータ バインディングを使用してその[ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/)プロパティを`IList`コレクション。 XAML でこれを実現すると、 [ `Binding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.BindingExtension/)マークアップ拡張機能。

```xaml
<Picker Title="Select a monkey" ItemsSource="{Binding Monkeys}" ItemDisplayBinding="{Binding Name}" />
```

同等の c# コードは、次に示します。

```csharp
var picker = new Picker { Title = "Select a monkey" };
picker.SetBinding(Picker.ItemsSourceProperty, "Monkeys");
picker.ItemDisplayBinding = new Binding("Name");
```

[ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/)プロパティ データにバインド、`Monkeys`を返す接続されているビュー モデルのプロパティ、`IList<Monkey>`コレクション。 次のコード例は、`Monkey`クラスは、4 つのプロパティが含まれています。

```csharp
public class Monkey
{
  public string Name { get; set; }
  public string Location { get; set; }
  public string Details { get; set; }
  public string ImageUrl { get; set; }
}
```

オブジェクトの一覧にバインドするとき、 [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)各オブジェクトを表示するプロパティを伝える必要があります。 これを実現するには、 [ `ItemDisplayBinding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemDisplayBinding/)各オブジェクトからプロパティの必須プロパティです。 上記のコード例で、`Picker`各表示に設定されている`Monkey.Name`プロパティの値。

### <a name="responding-to-item-selection"></a>項目の選択への応答

オブジェクトに設定するデータ バインディングを使用することができます、 [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedItem/)プロパティの値が変更されたとき。

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

同等の c# コードは、次に示します。

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

[ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedItem/)プロパティ データにバインド、`SelectedMonkey`プロパティの型は、接続されているビュー モデルの`Monkey`します。 そのため、ユーザーが内の項目を選択すると、 [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)、`SelectedMonkey`プロパティが設定されます、選択した`Monkey`オブジェクト。 `SelectedMonkey`オブジェクト データは、ユーザー インターフェイスに表示して[ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)と[ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/)ビュー。

![](populating-itemssource-images/monkeys.png "選択項目の選択")

> [!NOTE]
> なお、 [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedItem/)と[ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/)プロパティの両方が既定で双方向のバインディングをサポートします。

## <a name="summary"></a>まとめ

[ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)ビューは、コントロールのデータの一覧からテキスト アイテムを選択します。 この記事の内容を設定する方法を説明した、`Picker`を設定してデータを[ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/)プロパティ、およびユーザーが項目の選択に応答する方法です。 この方法は、Xamarin.Forms 2.3.4 で導入されたがやり取りする方法をお勧め、`Picker`です。


## <a name="related-links"></a>関連リンク

- [ピッカー デモ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PickerDemo/)
- [サル アプリ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/MonkeyAppPicker/)
- [バインド可能なピッカー (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BindablePicker/)
- [ピッカー](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)
