---
title: "XAML (プレビュー) の標準コントロール"
description: "Xamarin.Forms で XAML Standard プレビューを探索する方法"
ms.topic: article
ms.prod: xamarin
ms.assetid: 287E6631-D1C5-46C5-8905-AB53D34E365D
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/15/2017
ms.openlocfilehash: b044cb849f9a8e591a8db5907211a55f77d6e45f
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2018
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

|更新されたプロパティを持つ Xamarin.FormsControls|Xamarin.FormsProperty または列挙型|XAML StandardEquivalent|
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
