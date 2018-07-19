---
title: Xamarin.Forms の選択
description: Xamarin.Forms ピッカーには、ユーザーが項目を選択できる項目の短い一覧が表示されます。 この記事では、ピッカー クラスを使用して、データの一覧から、テキスト項目を選択する方法について説明します。
ms.prod: xamarin
ms.assetid: D4815A4B-104B-4294-951B-BD8F2EC33C86
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/04/2018
ms.openlocfilehash: c852cd29197b000ed1ff53853d64cfa25fb699e7
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996598"
---
# <a name="xamarinforms-picker"></a>Xamarin.Forms の選択

_ピッカーの表示は、データの一覧から、テキスト項目を選択するためのコントロールです。_

Xamarin.Forms [ `Picker` ](xref:Xamarin.Forms.Picker)ユーザーが項目を選択できる項目の短い一覧を表示します。 `Picker` 8 つのプロパティを定義します。

- [`Title`](xref:Xamarin.Forms.Picker.Title) 型の`string`、既定では`null`します。
- [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource) 型の`IList`、既定では、表示する項目のソース リスト`null`します。
- [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) 型の`int`、既定値は-1、選択した項目のインデックス。
- [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem) 型の`object`、既定値は、選択した項目`null`します。
- [`TextColor`](xref:Xamarin.Forms.Picker.TextColor) 型の[ `Color` ](xref:Xamarin.Forms.Color)、既定では、テキストを表示するために使用する色[ `Color.Default`](xref:Xamarin.Forms.Color.Default)します。
- [`FontAttributes`](xref:Xamarin.Forms.Picker.FontAttributes) 型の[ `FontAttributes` ](xref:Xamarin.Forms.FontAttributes)、既定では[ `FontAtributes.None`](xref:Xamarin.Forms.FontAttributes.None)します。
- [`FontFamily`](xref:Xamarin.Forms.Picker.FontFamily) 型の`string`、既定では`null`します。
- [`FontSize`](xref:Xamarin.Forms.Picker.FontSize) 型の`double`、既定では-1.0。

8 つすべてのプロパティが支え[ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty)オブジェクト、つまりスタイルして、プロパティがデータ バインドの対象になることができます。 [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex)と[ `SelectedItem` ](xref:Xamarin.Forms.Picker.SelectedItem)プロパティの既定のバインド モードがある[ `BindingMode.TwoWay` ](xref:Xamarin.Forms.BindingMode.TwoWay)、つまり、データ バインドのターゲットできること使用するアプリケーションで、[モデル-ビュー-ビューモデル (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md)アーキテクチャ。 フォントのプロパティの設定については、次を参照してください。[フォント](~/xamarin-forms/user-interface/text/fonts.md)します。

A [ `Picker` ](xref:Xamarin.Forms.Picker)が最初に表示されるときに、すべてのデータは表示されません。 代わりの値、 [ `Title` ](xref:Xamarin.Forms.Picker.Title)プロパティは、iOS および Android プラットフォームにプレース ホルダーとして表示されます。

[![](images/picker-initial.png "ピッカーの表示の初期")](images/picker-initial-large.png#lightbox "初期ピッカーの表示")

ときに、 [ `Picker` ](xref:Xamarin.Forms.Picker)向上フォーカス、そのデータが表示され、ユーザーが項目を選択できます。

[![](images/picker-selection.png "選択項目を選択する")](images/picker-selection-large.png#lightbox "ピッカーの項目を選択します。")

[ `Picker` ](xref:Xamarin.Forms.Picker)発生、 [ `SelectedIndexChanged` ](xref:Xamarin.Forms.Picker.SelectedIndexChanged)イベント、ユーザーが項目を選択します。 選択した場合、次は、選択した項目によって表示される、 `Picker`:

![](images/picker-after-selection.png "選択範囲の後の選択")

設定するための 2 つの手法がある、 [ `Picker` ](xref:Xamarin.Forms.Picker)データ。

- 設定、 [ `ItemsSource` ](xref:Xamarin.Forms.Picker.ItemsSource)プロパティを表示するデータ。 これは、Xamarin.Forms 2.3.4 で導入されたことをお勧めの方法です。 詳細については、次を参照してください。[ピッカーの ItemsSource プロパティを設定](populating-itemssource.md)します。
- 表示するデータの追加、 [ `Items` ](xref:Xamarin.Forms.Picker.Items)コレクション。 この手法が、元のプロセスを設定するため、 [ `Picker` ](xref:Xamarin.Forms.Picker)データ。 詳細については、次を参照してください。[ピッカーの項目のコレクションに追加データ](populating-items.md)します。

## <a name="related-links"></a>関連リンク

- [ピッカー](xref:Xamarin.Forms.Picker)
