---
title: "ピッカーの ItemsSource プロパティの設定" 説明: "ピッカービューは、データの一覧からテキスト項目を選択するためのコントロールです。 この記事では、ItemsSource プロパティを設定してピッカーにデータを設定する方法と、ユーザーが項目の選択に応答する方法について説明します。
ms. 製品: xamarin ms. assetid: 8ECF390C-9DB2-4441-B9A3-101AE7E5AEC5: xamarin-forms author: davidbritch ms. author: dabritch ms. date: 02/26/2019 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="setting-a-pickers-itemssource-property"></a>ピッカーの ItemsSource プロパティの設定

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-monkeyapppicker)

_ピッカービューは、データの一覧からテキスト項目を選択するためのコントロールです。この記事では、ItemsSource プロパティを設定してピッカーにデータを設定する方法と、ユーザーが項目の選択に応答する方法について説明します。_

Xamarin.Forms2.3.4 では、 [`Picker`](xref:Xamarin.Forms.Picker) プロパティを設定してデータを設定する機能を追加し、 [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource) プロパティから選択した項目を取得する機能を追加することで、ビューを拡張しました [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem) 。 また、プロパティをに設定すると、選択した項目のテキストの色を変更でき [`TextColor`](xref:Xamarin.Forms.Picker.TextColor) [`Color`](xref:Xamarin.Forms.Color) ます。

## <a name="populating-a-picker-with-data"></a>データを使用したピッカーの設定

にデータを設定するには、 [`Picker`](xref:Xamarin.Forms.Picker) [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource) プロパティをコレクションに設定し `IList` ます。 コレクション内の各項目は、型であるか、型から派生している必要があり `object` ます。 項目の配列からプロパティを初期化することによって、XAML で項目を追加でき `ItemsSource` ます。

```xaml
<Picker x:Name="picker"
        Title="Select a monkey"
        TitleColor="Red">
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
> `x:Array` 要素には、配列内の項目の型を示す `Type` 属性が必要です。

同等の C# コードを次に示します。

```csharp
var monkeyList = new List<string>();
monkeyList.Add("Baboon");
monkeyList.Add("Capuchin Monkey");
monkeyList.Add("Blue Monkey");
monkeyList.Add("Squirrel Monkey");
monkeyList.Add("Golden Lion Tamarin");
monkeyList.Add("Howler Monkey");
monkeyList.Add("Japanese Macaque");

var picker = new Picker { Title = "Select a monkey", TitleColor = Color.Red };
picker.ItemsSource = monkeyList;
```

## <a name="responding-to-item-selection"></a>項目の選択への応答

は、一度 [`Picker`](xref:Xamarin.Forms.Picker) に1つの項目の選択をサポートします。 ユーザーが項目を選択すると、 [`SelectedIndexChanged`](xref:Xamarin.Forms.Picker.SelectedIndexChanged) イベントが発生します [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) 。プロパティは、リスト内で選択されている項目のインデックスを表す整数に更新され、 [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem) プロパティは選択した項目を表すに更新され `object` ます。 プロパティは、 [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) ユーザーが選択した項目を示す0から始まる数値です。 項目が選択されていない場合 (が最初に作成されて初期化された場合 [`Picker`](xref:Xamarin.Forms.Picker) )、 `SelectedIndex` は-1 になります。

> [!NOTE]
> での項目選択の動作は、 [`Picker`](xref:Xamarin.Forms.Picker) プラットフォーム固有の iOS でカスタマイズできます。 詳細については、「[ピッカー項目の選択の制御](~/xamarin-forms/platform/ios/picker-selection.md)」を参照してください。

次のコード例は [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem) 、XAML でからプロパティ値を取得する方法を示してい [`Picker`](xref:Xamarin.Forms.Picker) ます。

```xaml
<Label Text="{Binding Source={x:Reference picker}, Path=SelectedItem}" />
```

同等の C# コードを次に示します。

```csharp
var monkeyNameLabel = new Label();
monkeyNameLabel.SetBinding(Label.TextProperty, new Binding("SelectedItem", source: picker));
```

さらに、イベントが発生したときに、イベントハンドラーを実行でき [`SelectedIndexChanged`](xref:Xamarin.Forms.Picker.SelectedIndexChanged) ます。

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

このメソッドは、 [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) プロパティ値を取得し、値を使用して、選択した項目をコレクションから取得し [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource) ます。 これは、プロパティから選択された項目を取得するのと機能的には同じです [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem) 。 コレクション内の各項目 `ItemsSource` は型であるため、表示するに `object` はにキャストする必要があり `string` ます。

> [!NOTE]
> [`Picker`](xref:Xamarin.Forms.Picker) [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) は、プロパティまたはプロパティを設定することによって、特定の項目を表示するために初期化でき [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem) ます。 ただし、これらのプロパティは、コレクションを初期化した後に設定する必要があり [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource) ます。

## <a name="populating-a-picker-with-data-using-data-binding"></a>データバインディングを使用してピッカーにデータを読み込む

また、データバインディングを使用してデータを設定し、 [`Picker`](xref:Xamarin.Forms.Picker) そのプロパティをコレクションにバインドすることもでき [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource) `IList` ます。 XAML では、これはマークアップ拡張機能を使用して実現され [`Binding`](xref:Xamarin.Forms.Xaml.BindingExtension) ます。

```xaml
<Picker Title="Select a monkey"
        TitleColor="Red"
        ItemsSource="{Binding Monkeys}"
        ItemDisplayBinding="{Binding Name}" />
```

同等の C# コードを次に示します。

```csharp
var picker = new Picker { Title = "Select a monkey", TitleColor = Color.Red };
picker.SetBinding(Picker.ItemsSourceProperty, "Monkeys");
picker.ItemDisplayBinding = new Binding("Name");
```

[`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource)プロパティデータは、 `Monkeys` 接続されたビューモデルのプロパティにバインドされます。このプロパティは、コレクションを返し `IList<Monkey>` ます。 次のコード例は、 `Monkey` 4 つのプロパティを含むクラスを示しています。

```csharp
public class Monkey
{
  public string Name { get; set; }
  public string Location { get; set; }
  public string Details { get; set; }
  public string ImageUrl { get; set; }
}
```

オブジェクトのリストにバインドする場合は、 [`Picker`](xref:Xamarin.Forms.Picker) 各オブジェクトからどのプロパティを表示するかを指定する必要があります。 これを実現するには、 [`ItemDisplayBinding`](xref:Xamarin.Forms.Picker.ItemDisplayBinding) プロパティを各オブジェクトの必須プロパティに設定します。 上記のコード例では、 `Picker` 各プロパティ値を表示するようにが設定されてい `Monkey.Name` ます。

### <a name="responding-to-item-selection"></a>項目の選択への応答

データバインディングを使用して、変更時にオブジェクトをプロパティ値に設定でき [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem) ます。

```xaml
<Picker Title="Select a monkey"
        TitleColor="Red"
        ItemsSource="{Binding Monkeys}"
        ItemDisplayBinding="{Binding Name}"
        SelectedItem="{Binding SelectedMonkey}" />
<Label Text="{Binding SelectedMonkey.Name}" ... />
<Label Text="{Binding SelectedMonkey.Location}" ... />
<Image Source="{Binding SelectedMonkey.ImageUrl}" ... />
<Label Text="{Binding SelectedMonkey.Details}" ... />
```

同等の C# コードを次に示します。

```csharp
var picker = new Picker { Title = "Select a monkey", TitleColor = Color.Red };
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

[`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem)プロパティデータは、 `SelectedMonkey` 接続されたビューモデルのプロパティにバインドされます。これは型 `Monkey` です。 そのため、ユーザーが内の項目を選択すると、プロパティは選択され [`Picker`](xref:Xamarin.Forms.Picker) `SelectedMonkey` たオブジェクトに設定され `Monkey` ます。 `SelectedMonkey`オブジェクトデータは、およびビューによってユーザーインターフェイスに表示され [`Label`](xref:Xamarin.Forms.Label) [`Image`](xref:Xamarin.Forms.Image) ます。

![](populating-itemssource-images/monkeys.png "Picker Item Selection")

> [!NOTE]
> [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem)プロパティとプロパティは、 [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) どちらも既定で双方向のバインディングをサポートしていることに注意してください。

## <a name="related-links"></a>関連リンク

- [ピッカーのデモ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-pickerdemo)
- [サルアプリ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-monkeyapppicker)
- [バインド可能なピッカー (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablepicker)
- [ピッカー API](xref:Xamarin.Forms.Picker)
