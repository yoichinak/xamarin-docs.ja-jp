---
title: Xamarin. フォームピッカー
description: '[Xamarin] ピッカーには、アイテムを選択できる項目の短い一覧が表示されます。 この記事では、ピッカークラスを使用して、データの一覧からテキスト項目を選択する方法について説明します。'
ms.prod: xamarin
ms.assetid: D4815A4B-104B-4294-951B-BD8F2EC33C86
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/26/2019
ms.openlocfilehash: c8246f8316a2a579bc890935dc4f9beecccb8dcc
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "72695999"
---
# <a name="xamarinforms-picker"></a>Xamarin. フォームピッカー

_ピッカービューは、データの一覧からテキスト項目を選択するためのコントロールです。_

Xamarin [`Picker`](xref:Xamarin.Forms.Picker)には、アイテムを選択できる項目の短い一覧が表示されます。 `Picker` は、次のプロパティを定義します。

- `string` 型の[`Title`](xref:Xamarin.Forms.Picker.Title) 。既定では `null` になります。
- `Title` テキストの表示に使用する色[`Color`](xref:Xamarin.Forms.Color)型の `TitleColor`。
- `IList` 型の[`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource) 。表示する項目のソースリスト。既定では `null` になります。
- `int` 型の[`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) 、選択した項目のインデックス。既定値は-1 です。
- `object` 型の[`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem) 。選択された項目で、既定で `null` になります。
- [`Color`](xref:Xamarin.Forms.Color)型の[`TextColor`](xref:Xamarin.Forms.Picker.TextColor) 。テキストの表示に使用される色。既定では[`Color.Default`](xref:Xamarin.Forms.Color.Default)になります。
- [`FontAttributes`](xref:Xamarin.Forms.FontAttributes)型の[`FontAttributes`](xref:Xamarin.Forms.Picker.FontAttributes) 。既定では[`FontAtributes.None`](xref:Xamarin.Forms.FontAttributes.None)になります。
- `string` 型の[`FontFamily`](xref:Xamarin.Forms.Picker.FontFamily) 。既定では `null` になります。
- `double` 型の[`FontSize`](xref:Xamarin.Forms.Picker.FontSize) 。既定値は-1.0 です。
- `double` 型の `CharacterSpacing` は、`Picker` によって表示される項目の文字間の間隔です。

すべてのプロパティは、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)オブジェクトによって支えられています。つまり、スタイルを設定でき、プロパティはデータバインディングのターゲットにすることができます。 [@No__t_1](xref:Xamarin.Forms.Picker.SelectedIndex)プロパティと[`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem)プロパティには[`BindingMode.TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay)の既定のバインディングモードがあります。つまり、モデルビュービューモデル[(MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md)アーキテクチャを使用するアプリケーションでデータバインディングのターゲットにすることができます。 フォントプロパティの設定の詳細については、「[フォント](~/xamarin-forms/user-interface/text/fonts.md)」を参照してください。

[@No__t_1](xref:Xamarin.Forms.Picker)には、最初に表示されたときにデータが表示されません。 代わりに、 [`Title`](xref:Xamarin.Forms.Picker.Title)プロパティの値は、IOS および Android プラットフォームでプレースホルダーとして表示されます。

[![](images/picker-initial.png "Initial Picker Display")](images/picker-initial-large.png#lightbox "Initial Picker Display")

[@No__t_1](xref:Xamarin.Forms.Picker)がフォーカスを取得すると、そのデータが表示され、ユーザーは項目を選択できます。

[![](images/picker-selection.png "Picker Selecting an Item")](images/picker-selection-large.png#lightbox "Picker Selecting an Item")

[@No__t_1](xref:Xamarin.Forms.Picker)は、ユーザーが項目を選択したときに[`SelectedIndexChanged`](xref:Xamarin.Forms.Picker.SelectedIndexChanged)イベントを発生させます。 選択すると、選択した項目が `Picker` によって表示されます。

![](images/picker-after-selection.png "Picker after Selection")

[@No__t_1](xref:Xamarin.Forms.Picker)にデータを設定するには、次の2つの方法があります。

- 表示するデータに[`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource)プロパティを設定します。 この手法を使用することをお勧めします。 詳細については、「[ピッカーの ItemsSource プロパティの設定](populating-itemssource.md)」を参照してください。
- [@No__t_1](xref:Xamarin.Forms.Picker.Items)コレクションに表示されるデータを追加します。 この手法は、 [`Picker`](xref:Xamarin.Forms.Picker)にデータを設定するための元のプロセスでした。 詳細については、「[ピッカーの項目コレクションへのデータの追加](populating-items.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [ピッカー](xref:Xamarin.Forms.Picker)
