---
title: ピッカー
description: ピッカー ビューは、データの一覧からテキスト アイテムを選択するためのコントロールです。
ms.prod: xamarin
ms.assetid: D4815A4B-104B-4294-951B-BD8F2EC33C86
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/04/2018
ms.openlocfilehash: 7f0050351ca28d7f8afeb82a85e82e51d399824b
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/07/2018
ms.locfileid: "34847499"
---
# <a name="picker"></a>ピッカー

_ピッカー ビューは、データの一覧からテキスト アイテムを選択するためのコントロールです。_

Xamarin.Forms [ `Picker` ](xref:Xamarin.Forms.Picker)ユーザーが、項目を選択できる項目の短い一覧を表示します。 `Picker` 8 つのプロパティを定義します。

- [`Title`](xref:Xamarin.Forms.Picker.Title) 型の`string`、既定値`null`です。
- [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource) 型の`IList`、既定値は、表示するアイテムのソース リスト`null`です。
- [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) 型の`int`、既定値は-1、選択した項目のインデックス。
- [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem) 型の`object`、既定値は、選択したアイテム`null`です。
- [`TextColor`](xref:Xamarin.Forms.Picker.TextColor) 型の[ `Color` ](xref:Xamarin.Forms.Color)、既定値はテキスト表示に使用する色[ `Color.Default`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Default/)です。
- [`FontAttributes`](xref:Xamarin.Forms.Picker.FontAttributes) 型の[ `FontAttributes` ](xref:Xamarin.Forms.FontAttributes)、既定値[ `FontAtributes.None`](xref:Xamarin.Forms.FontAttributes.None)です。
- [`FontFamily`](xref:Xamarin.Forms.Picker.FontFamily) 型の`string`、既定値`null`です。
- [`FontSize`](xref:Xamarin.Forms.Picker.FontSize) 型の`double`、-1.0 既定値です。

8 つのすべてのプロパティが裏付け[ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty)オブジェクト、つまり、スタイルを設定できる、し、プロパティがデータ バインドの対象になることができます。 [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex)と[ `SelectedItem` ](xref:Xamarin.Forms.Picker.SelectedItem)プロパティの既定のバインド モードを持っている[ `BindingMode.TwoWay` ](xref:Xamarin.Forms.BindingMode.TwoWay)、つまり、データ バインディングのターゲットがあることができます使用するアプリケーションで、[モデル View-viewmodel (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md)アーキテクチャ。 フォントのプロパティの設定については、次を参照してください。[フォント](~/xamarin-forms/user-interface/text/fonts.md)です。

A [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)が最初に表示される場合、データを表示しません。 代わりの値、 [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Title/) iOS と Android プラットフォームでプレース ホルダーとして表示されるプロパティです。

[![](images/picker-initial.png "ピッカーの表示の初期")](images/picker-initial-large.png#lightbox "ピッカーの表示の初期")

ときに、 [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)向上フォーカス、そのデータが表示され、ユーザーが項目を選択できます。

[![](images/picker-selection.png "項目を選択するピッカー")](images/picker-selection-large.png#lightbox "ピッカーの項目を選択します。")

[ `Picker` ](xref:Xamarin.Forms.Picker)発生、 [ `SelectedIndexChanged` ](xref:Xamarin.Forms.Picker.SelectedIndexChanged)イベント、ユーザーが項目を選択します。 次の選択、選択した項目を表示するを`Picker`:

![](images/picker-after-selection.png "選択範囲の後の選択")

設定するための 2 つの方法がある、 [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)データを使用します。

- 設定、 [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/)プロパティを表示するデータ。 これは、Xamarin.Forms 2.3.4 で導入されたことをお勧めの方法です。 詳細については、次を参照してください。[ピッカーの ItemsSource プロパティを設定](populating-itemssource.md)です。
- 表示するデータの追加、 [ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/)コレクション。 この手法は、元のプロセスを設定するため、 [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)データを使用します。 詳細については、次を参照してください。[ピッカーの項目コレクションへのデータの追加](populating-items.md)です。

## <a name="related-links"></a>関連リンク

- [ピッカー](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)
