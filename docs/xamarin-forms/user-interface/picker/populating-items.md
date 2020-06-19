---
title: ピッカーの項目コレクションへのデータの追加
description: ピッカービューは、データの一覧からテキスト項目を選択するためのコントロールです。 この記事では、項目コレクションに項目を追加することによって、ピッカーにデータを設定する方法と、ユーザーが項目の選択に応答する方法について説明します。
ms.prod: xamarin
ms.assetid: 3C840F64-A430-457D-A4B2-3D7AF46F9DBE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/26/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 8872c6748ba778a2622d82803d580c781bd282cd
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84139634"
---
# <a name="adding-data-to-a-pickers-items-collection"></a>ピッカーの項目コレクションへのデータの追加

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-pickerdemo)

_ピッカービューは、データの一覧からテキスト項目を選択するためのコントロールです。この記事では、項目コレクションに項目を追加することによって、ピッカーにデータを設定する方法と、ユーザーが項目の選択に応答する方法について説明します。_

## <a name="populating-a-picker-with-data"></a>データを使用したピッカーの設定

2.3.4 より前 Xamarin.Forms では、 [`Picker`](xref:Xamarin.Forms.Picker) データをに設定するプロセスは、表示されるデータを、型の読み取り専用コレクションに追加する必要がありました [`Items`](xref:Xamarin.Forms.Picker.Items) `IList<string>` 。 コレクション内の各項目の型はである必要があり `string` ます。 項目のリストを使用してプロパティを初期化することによって、XAML で項目を追加でき `Items` `x:String` ます。

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

同等の C# コードを次に示します。

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

メソッドを使用してデータを追加するだけで `Items.Add` なく、メソッドを使用してデータをコレクションに挿入することもでき `Items.Insert` ます。

## <a name="responding-to-item-selection"></a>項目の選択への応答

は、一度 [`Picker`](xref:Xamarin.Forms.Picker) に1つの項目の選択をサポートします。 ユーザーが項目を選択すると、 [`SelectedIndexChanged`](xref:Xamarin.Forms.Picker.SelectedIndexChanged) イベントが発生し、 [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) プロパティはリスト内で選択された項目のインデックスを表す整数に更新されます。 プロパティは、ユーザーが選択した `SelectedIndex` 項目を示す0から始まる数値です。 項目が選択されていない場合 (が最初に作成されて初期化された場合 `Picker` )、 `SelectedIndex` は-1 になります。

> [!NOTE]
> での項目選択の動作は、 [`Picker`](xref:Xamarin.Forms.Picker) プラットフォーム固有の iOS でカスタマイズできます。 詳細については、「[ピッカー項目の選択の制御](~/xamarin-forms/platform/ios/picker-selection.md)」を参照してください。

次のコード例は、イベント `OnPickerSelectedIndexChanged` の発生時に実行されるイベントハンドラーメソッドを示してい [`SelectedIndexChanged`](xref:Xamarin.Forms.Picker.SelectedIndexChanged) ます。

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

このメソッドは、 [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) プロパティ値を取得し、値を使用して、選択した項目をコレクションから取得し [`Items`](xref:Xamarin.Forms.Picker.Items) ます。 コレクション内の各項目はであるため `Items` `string` 、 [`Label`](xref:Xamarin.Forms.Label) キャストを必要とせずに、によって表示できます。

> [!NOTE]
> を初期化すると、 [`Picker`](xref:Xamarin.Forms.Picker) プロパティを設定することによって特定の項目を表示でき [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) ます。 ただし、 `SelectedIndex` コレクションを初期化した後にプロパティを設定する必要があり [`Items`](xref:Xamarin.Forms.Picker.Items) ます。

## <a name="related-links"></a>関連リンク

- [ピッカーのデモ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-pickerdemo)
- [ピッカー](xref:Xamarin.Forms.Picker)
