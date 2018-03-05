---
title: "ピッカーの項目コレクションへのデータの追加"
description: "ピッカー ビューは、データの一覧からテキスト アイテムを選択するためのコントロールです。 この記事では、項目コレクションに追加してデータの選択を設定する方法と、ユーザーが項目の選択に応答する方法について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 3C840F64-A430-457D-A4B2-3D7AF46F9DBE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/11/2017
ms.openlocfilehash: 3468e38d8ef46dfef870a05bf72d93c28195dae7
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="adding-data-to-a-pickers-items-collection"></a>ピッカーの項目コレクションへのデータの追加

_ピッカー ビューは、データの一覧からテキスト アイテムを選択するためのコントロールです。この記事では、項目コレクションに追加してデータの選択を設定する方法と、ユーザーが項目の選択に応答する方法について説明します。_

## <a name="populating-a-picker-with-data"></a>データ ピッカーを設定します。

Xamarin.Forms 2.3.4 への書き込みを処理する前に、 [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) 、読み取り専用に表示するデータを追加するのに使用データ[ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/)型は、コレクション`IList<string>`. コレクション内の各項目は、型でなければなりません`string`です。 初期化することによって XAML で項目を追加することができます、`Items`プロパティの一覧を`x:String`項目。

```xaml
<Picker Title="Select a monkey">
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

同等の c# コードは、次に示します。

```csharp
var picker = new Picker { Title = "Select a monkey" };
picker.Items.Add("Baboon");
picker.Items.Add("Capuchin Monkey");
picker.Items.Add("Blue Monkey");
picker.Items.Add("Squirrel Monkey");
picker.Items.Add("Golden Lion Tamarin");
picker.Items.Add("Howler Monkey");
picker.Items.Add("Japanese Macaque");
```

使用してデータを追加するだけでなく、`Items.Add`メソッドを使用して、コレクションにデータが挿入もできる、`Items.Insert`メソッドです。

## <a name="responding-to-item-selection"></a>項目の選択への応答

A [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)一度に 1 つの項目の選択をサポートします。 ユーザーが、項目を選択したときに、 [ `SelectedIndexChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Picker.SelectedIndexChanged/)イベントの起動と[ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/)プロパティが更新され、一覧で選択した項目のインデックスを表す整数。 `SelectedIndex`プロパティは、ユーザーが選択された項目を示す 0 から始まる番号。 項目が選択されていない場合は、ケースときに、`Picker`が最初に作成され、初期化、`SelectedIndex`は-1 になります。

> [!NOTE]
> 項目の選択の動作で、 [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) iOS プラットフォーム固有の仕様と上でカスタマイズできます。 詳細については、次を参照してください。[の選択項目の選択を制御する](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#picker_update_mode)です。

次のコード例は、`OnPickerSelectedIndexChanged`はイベント ハンドラー メソッドは、実行すると実行、 [ `SelectedIndexChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Picker.SelectedIndexChanged/)イベントの起動。

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

このメソッドは、取得、 [ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/)プロパティの値から選択した項目を取得する値を使用して、 [ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/)コレクション。 内の各項目があるため、`Items`コレクションは、`string`で表示すること、 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)キャストを必要とせずします。

> [!NOTE]
> A [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)設定して、特定のアイテムを表示する初期化することができます、 [ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/)プロパティです。 ただし、`SelectedIndex`初期化した後にプロパティを設定する必要があります、 [ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/)コレクション。

## <a name="summary"></a>まとめ

[ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)ビューは、コントロールのデータの一覧からテキスト アイテムを選択します。 この記事の内容を設定する方法を説明した、`Picker`データに追加することによって、 [ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/)コレクション、およびユーザーが項目の選択に応答する方法です。 これは、プロセスを使用するため、 `Picker` Xamarin.Forms 2.3.4 する前にします。


## <a name="related-links"></a>関連リンク

- [ピッカー デモ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PickerDemo/)
- [ピッカー](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)
