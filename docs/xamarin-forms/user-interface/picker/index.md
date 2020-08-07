---
title: Xamarin.Forms効果
description: ピッカーには、アイテムを Xamarin.Forms 選択できる項目の短い一覧が表示されます。 この記事では、ピッカークラスを使用して、データの一覧からテキスト項目を選択する方法について説明します。
ms.prod: xamarin
ms.assetid: D4815A4B-104B-4294-951B-BD8F2EC33C86
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/20/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 30f5aefe5fcb327a7c3333bee8e8b553e2630f57
ms.sourcegitcommit: 08290d004d1a7e7ac579bf1f96abf8437921dc70
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/07/2020
ms.locfileid: "87918013"
---
# <a name="no-locxamarinforms-picker"></a>Xamarin.Forms効果

_ピッカービューは、データの一覧からテキスト項目を選択するためのコントロールです。_

では、項目 Xamarin.Forms [`Picker`](xref:Xamarin.Forms.Picker) の簡単な一覧が表示されます。このリストから、ユーザーは項目を選択できます。 `Picker` は次の特性を定義します。

- [`CharacterSpacing`](xref:Xamarin.Forms.Picker.CharacterSpacing)型の `double` は、によって表示される項目の文字間の間隔です `Picker` 。
- [`FontAttributes`](xref:Xamarin.Forms.Picker.FontAttributes)型の [`FontAttributes`](xref:Xamarin.Forms.FontAttributes) 。既定値は [`FontAtributes.None`](xref:Xamarin.Forms.FontAttributes.None) です。
- [`FontFamily`](xref:Xamarin.Forms.Picker.FontFamily)型の `string` 。既定値は `null` です。
- [`FontSize`](xref:Xamarin.Forms.Picker.FontSize)型の `double` 。既定値は-1.0 です。
- `HorizontalTextAlignment`型のは、 [`TextAlignment`](xref:Xamarin.Forms.TextAlignment) によって表示されるテキストの水平方向の配置です `Picker` 。
- [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource)型の `IList` 場合は、表示する項目のソースリスト。既定値は `null` です。
- [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex)型の `int` 場合は、選択された項目のインデックス。既定値は-1 です。
- [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem)型 `object` (選択された項目)。既定値は `null` です。
- [`TextColor`](xref:Xamarin.Forms.Picker.TextColor)型の [`Color`](xref:Xamarin.Forms.Color) 場合は、テキストの表示に使用する色。既定値は [`Color.Default`](xref:Xamarin.Forms.Color.Default) です。
- [`Title`](xref:Xamarin.Forms.Picker.Title)型の `string` 。既定値は `null` です。
- [`TitleColor`](xref:Xamarin.Forms.Picker.TitleColor)型の [`Color`](xref:Xamarin.Forms.Color) 。テキストの表示に使用する色 `Title` 。
- `VerticalTextAlignment`型のは、 [`TextAlignment`](xref:Xamarin.Forms.TextAlignment) によって表示されるテキストの垂直方向の配置です `Picker` 。

すべてのプロパティはオブジェクトによって支えられています [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 。これはスタイルを設定でき、プロパティはデータバインディングのターゲットにすることができることを意味します。 [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex)プロパティと [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem) プロパティには、の既定のバインディングモードがあります [`BindingMode.TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay) 。つまり、[モデルビュービューモデル (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md)アーキテクチャを使用するアプリケーションでデータバインディングのターゲットにすることができます。 フォントプロパティの設定の詳細については、「[フォント](~/xamarin-forms/user-interface/text/fonts.md)」を参照してください。

は、 [`Picker`](xref:Xamarin.Forms.Picker) 最初に表示されたときにデータを表示しません。 代わりに、そのプロパティの値 [`Title`](xref:Xamarin.Forms.Picker.Title) は、iOS および Android プラットフォームのプレースホルダーとして表示されます。

[![初期ピッカーの表示](images/picker-initial.png)](images/picker-initial-large.png#lightbox "初期ピッカーの表示")

がフォーカスを取得すると、 [`Picker`](xref:Xamarin.Forms.Picker) そのデータが表示され、ユーザーは項目を選択できるようになります。

[![ピッカー項目を選択する](images/picker-selection.png)](images/picker-selection-large.png#lightbox "ピッカー項目を選択する")

は、 [`Picker`](xref:Xamarin.Forms.Picker) [`SelectedIndexChanged`](xref:Xamarin.Forms.Picker.SelectedIndexChanged) ユーザーが項目を選択したときにイベントを発生させます。 選択すると、選択した項目がによって表示され `Picker` ます。

![選択後のピッカー](images/picker-after-selection.png)

にデータを読み込むには、次の2つの方法があり [`Picker`](xref:Xamarin.Forms.Picker) ます。

- 表示する [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource) データにプロパティを設定します。 この手法を使用することをお勧めします。 詳細については、「[ピッカーの ItemsSource プロパティの設定](populating-itemssource.md)」を参照してください。
- 表示するデータをコレクションに追加して [`Items`](xref:Xamarin.Forms.Picker.Items) います。 この手法は、データをに設定するための元のプロセスでした [`Picker`](xref:Xamarin.Forms.Picker) 。 詳細については、「[ピッカーの項目コレクションへのデータの追加](populating-items.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [ピッカー](xref:Xamarin.Forms.Picker)
