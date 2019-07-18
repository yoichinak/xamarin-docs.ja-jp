---
title: データ選択コントロールの項目のコレクションを追加します。
description: ピッカーの表示は、データの一覧から、テキスト項目を選択するためのコントロールです。 この記事では、項目のコレクションに追加することによってデータの選択を設定する方法と、ユーザーが項目の選択に応答する方法について説明します。
ms.prod: xamarin
ms.assetid: 3C840F64-A430-457D-A4B2-3D7AF46F9DBE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/26/2019
ms.openlocfilehash: 3bbea036efef44077ccbd28a16af06c97cd7026b
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61230304"
---
# <a name="adding-data-to-a-pickers-items-collection"></a>データ選択コントロールの項目のコレクションを追加します。

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PickerDemo/)

_ピッカーの表示は、データの一覧から、テキスト項目を選択するためのコントロールです。この記事では、項目のコレクションに追加することによってデータの選択を設定する方法と、ユーザーが項目の選択に応答する方法について説明します。_

## <a name="populating-a-picker-with-data"></a>データの選択の設定

Xamarin.Forms 2.3.4 を設定するためのプロセスの前に、 [ `Picker` ](xref:Xamarin.Forms.Picker)データが、読み取り専用に表示するデータを追加する[ `Items` ](xref:Xamarin.Forms.Picker.Items) 型のコレクション、`IList<string>`. コレクション内の各項目は、型でなければなりません`string`します。 初期化することにより XAML で項目を追加できる、`Items`プロパティの一覧を`x:String`項目。

```xaml
<Picker Title="Select a monkey"
        TitleColor="Red">
  <Picker.Items>
    <x:String>Baboon</x:String>
    <x:String>Capuchin Monkey</x:String>
    <x:String>Blue Monkey</x:String>
    <x:String>Squirrel Monkey</x:String>
    <x:String>Golden Lion Tamarin</x:String>
    <x:String>Howler Monkey</x:String>
    <x:String>Japanese Macaque</x:String>
  </Picker.Items>
</Picker>
```

同等の C# コードは、以下に示します。

```csharp
var picker = new Picker { Title = "Select a monkey", TitleColor = Color.Red };
picker.Items.Add("Baboon");
picker.Items.Add("Capuchin Monkey");
picker.Items.Add("Blue Monkey");
picker.Items.Add("Squirrel Monkey");
picker.Items.Add("Golden Lion Tamarin");
picker.Items.Add("Howler Monkey");
picker.Items.Add("Japanese Macaque");
```

使用してデータを追加するだけでなく、`Items.Add`メソッドを使用してデータがコレクションに挿入することもできます、`Items.Insert`メソッド。

## <a name="responding-to-item-selection"></a>項目の選択への応答

A [ `Picker` ](xref:Xamarin.Forms.Picker)が一度に 1 つの項目の選択をサポートします。 ユーザーが、項目を選択すると、 [ `SelectedIndexChanged` ](xref:Xamarin.Forms.Picker.SelectedIndexChanged)イベントが発生、 [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex)プロパティが更新され、一覧で選択された項目のインデックスを表す整数。 `SelectedIndex`プロパティは、ユーザーが選択した項目を示す 0 から始まる番号。 項目が選択されていない場合ですが、ときに、`Picker`が最初に作成され、初期化、`SelectedIndex`は-1 になります。

> [!NOTE]
> 項目の選択動作を[ `Picker` ](xref:Xamarin.Forms.Picker)プラットフォーム固有の iOS 上でカスタマイズできます。 詳細については、「[Picker のアイテム選択の制御](~/xamarin-forms/platform/ios/picker-selection.md)」を参照してください。

次のコード例は、 `OnPickerSelectedIndexChanged` 、イベント ハンドラー メソッドがあるときに実行、 [ `SelectedIndexChanged` ](xref:Xamarin.Forms.Picker.SelectedIndexChanged)イベントが発生します。

```csharp
void OnPickerSelectedIndexChanged(object sender, EventArgs e)
{
  var picker = (Picker)sender;
  int selectedIndex = picker.SelectedIndex;

  if (selectedIndex != -1)
  {
    monkeyNameLabel.Text = picker.Items[selectedIndex];
  }
}
```

このメソッドは、取得、 [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex)プロパティの値は、値から選択した項目を取得して、 [ `Items` ](xref:Xamarin.Forms.Picker.Items)コレクション。 内の各項目であるため、`Items`コレクションが、`string`で表示することができます、 [ `Label` ](xref:Xamarin.Forms.Label)キャストを必要とせずします。

> [!NOTE]
> A [ `Picker` ](xref:Xamarin.Forms.Picker)設定して、特定のアイテムを表示する初期化することができます、 [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex)プロパティ。 ただし、`SelectedIndex`初期化した後にプロパティを設定する必要があります、 [ `Items` ](xref:Xamarin.Forms.Picker.Items)コレクション。

## <a name="related-links"></a>関連リンク

- [ピッカーのデモ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PickerDemo/)
- [ピッカー](xref:Xamarin.Forms.Picker)
