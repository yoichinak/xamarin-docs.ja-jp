---
title: XAML (プレビュー) の標準コントロール
description: Xamarin.Forms で XAML Standard プレビューを探索する方法
ms.prod: xamarin
ms.assetid: 287E6631-D1C5-46C5-8905-AB53D34E365D
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/15/2017
ms.openlocfilehash: 2fc7fb9581f344e0d54bd9f690d334eda78cc97a
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="xaml-standard-preview-controls"></a>XAML (プレビュー) の標準コントロール

![[プレビュー]](~/media/shared/preview.png)

このページでは、同等の Xamarin.Forms コントロールと共に、プレビューで使用できる標準的な XAML コントロールが一覧表示します。

Standard では XAML のプロパティおよび列挙体の新しい名前を持つコントロールの一覧もあります。

## <a name="controls"></a>コントロール

|Xamarin.Forms|XAML の標準|
|--- |--- |
|フレーム|境界線|
|ピッカー|ComboBox|
|ActivityIndicator|ProgressRing|
|StackLayout|StackPanel|
|group1|TextBlock|
|入力|TextBox|
|切り替え|ToggleSwitch|
|ContentView|UserControl|


## <a name="properties-and-enumerations"></a>プロパティおよび列挙

|更新されたプロパティを持つ Xamarin.Forms コントロール|Xamarin.Forms プロパティまたは列挙型|該当するショートカットは標準的な XAML|
|--- |--- |--- |
|ボタン、エントリ、ラベル、DatePicker、エディター、SearchBar、TimePicker|TextColor|前景|
|VisualElement|BackgroundColor|バック グラウンド *|
|ボタンの選択|BorderColor、囲んで|BorderBrush|
|ボタン|BorderWidth|BorderThickness|
|ProgressBar|進行状況|[値]|
|ボタン、エントリ、ラベル、エディター、SearchBar、スパン、フォント|FontAttributesBold、斜体、なし|FontStyleItalic、標準|
|ボタン、エントリ、ラベル、エディター、SearchBar、スパン、フォント|FontAttributes|FontWeights * 太字、標準|
|InputView|KeyboardDefault、Url、番号、電話、テキスト、チャット、電子メールで送信します。|InputScopeNameValue * 既定、Url、番号、TelephoneNumber、テキスト、チャット、EmailNameOrAddress|
|StackPanel|StackOrientation|印刷の向き *|

> [!IMPORTANT]
> マークされた項目 * 現在のプレビューで完全ではありません

## <a name="related-links"></a>関連リンク

- [NuGet のプレビュー](https://aka.ms/xf-xamlstandard-nuget)
