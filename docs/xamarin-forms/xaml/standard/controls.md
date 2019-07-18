---
title: XAML Standard (プレビュー) のコントロール
description: この記事では、Xamarin.Forms で使用できる XAML 標準のコントロールについて説明します。
ms.prod: xamarin
ms.assetid: 287E6631-D1C5-46C5-8905-AB53D34E365D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/15/2017
ms.openlocfilehash: b9bf0e1ba14f4e8584bfd8492776ac7c8668df87
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61175179"
---
# <a name="xaml-standard-preview-controls"></a>XAML Standard (プレビュー) のコントロール

![[プレビュー]](~/media/shared/preview.png)

このページには、プレビューでの同等の Xamarin.Forms コントロールと共に使用できる標準的な XAML コントロールが一覧表示されます。

XAML Standard で新しいプロパティと列挙型の名前を持つコントロールの一覧もあります。

## <a name="controls"></a>コントロール

|Xamarin.Forms|XAML Standard|
|--- |--- |
|フレーム|境界線|
|ピッカー|ComboBox|
|ActivityIndicator|ProgressRing|
|StackLayout|StackPanel|
|group1|TextBlock|
|入力|TextBox|
|切り替え|ToggleSwitch|
|ContentView|UserControl|


## <a name="properties-and-enumerations"></a>プロパティと列挙型

|更新されたプロパティを持つ Xamarin.Forms コントロール|Xamarin.Forms プロパティまたは列挙型|標準的な XAML と同等|
|--- |--- |--- |
|ボタン、エントリ、ラベル、DatePicker、エディター、SearchBar、TimePicker|TextColor|前景|
|VisualElement|BackgroundColor|バック グラウンド *|
|ボタンの選択|BorderColor、OutlineColor|BorderBrush|
|ボタン|BorderWidth|BorderThickness|
|ProgressBar|進行状況|[値]|
|ボタン、エントリ、ラベル、エディター、SearchBar、スパン、フォント|FontAttributesBold、斜体、なし|通常、FontStyleItalic|
|ボタン、エントリ、ラベル、エディター、SearchBar、スパン、フォント|FontAttributes|FontWeights *Bold, Normal|
|InputView|電子メール KeyboardDefault、Url、番号、電話、テキスト、チャット、|InputScopeNameValue * 既定、Url、数値、TelephoneNumber、テキスト、チャット、EmailNameOrAddress|
|StackPanel|StackOrientation|印刷の向き *|

> [!IMPORTANT]
> マークされた項目 * 現在のプレビューで完全ではありません

## <a name="related-links"></a>関連リンク

- [NuGet のプレビュー](https://aka.ms/xf-xamlstandard-nuget)
