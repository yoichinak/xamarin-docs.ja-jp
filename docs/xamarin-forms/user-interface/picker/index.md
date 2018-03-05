---
title: "ピッカー"
description: "ピッカー ビューは、データの一覧からテキスト アイテムを選択するためのコントロールです。"
ms.topic: article
ms.prod: xamarin
ms.assetid: D4815A4B-104B-4294-951B-BD8F2EC33C86
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/11/2017
ms.openlocfilehash: edc724eb73b314c0accd3e8775b9b26b6eac16d9
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="picker"></a>ピッカー

_ピッカー ビューは、データの一覧からテキスト アイテムを選択するためのコントロールです。_

A [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)ユーザーが選択できる項目の短い一覧が表示されます。 ただし、 [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)が最初に表示される場合、データを表示しません。 代わりの値、 [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Title/) iOS と Android プラットフォームでプレース ホルダーとして表示されるプロパティです。

[![](images/picker-initial.png "ピッカーの表示の初期")](images/picker-initial-large.png "ピッカーの表示の初期")

ときに、 [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)向上フォーカス、そのデータが表示され、ユーザーが項目を選択できます。

[![](images/picker-selection.png "項目を選択するピッカー")](images/picker-selection-large.png "ピッカーの項目を選択します。")

次の選択、選択した項目を表示するを[ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/):

![](images/picker-after-selection.png "選択範囲の後の選択")

設定するための 2 つの方法がある、 [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)データを使用します。

- 設定、 [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/)プロパティを表示するデータ。 これは、Xamarin.Forms 2.3.4 で導入されたことをお勧めの方法です。 詳細については、次を参照してください。[ピッカーの ItemsSource プロパティを設定](populating-itemssource.md)です。
- 表示するデータの追加、 [ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/)コレクション。 この手法は、元のプロセスを設定するため、 [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)データを使用します。 詳細については、次を参照してください。[ピッカーの項目コレクションへのデータの追加](populating-items.md)です。


## <a name="related-links"></a>関連リンク

- [ピッカー](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)
