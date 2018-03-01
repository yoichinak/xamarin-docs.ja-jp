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
ms.openlocfilehash: 3f30a77975a9f42380ecf7efd73426763ec83ef0
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="xaml-standard-preview-controls"></a>XAML (プレビュー) の標準コントロール

![[プレビュー]](~/media/shared/preview.png)

このページでは、同等の Xamarin.Forms コントロールと共に、プレビューで使用できる標準的な XAML コントロールが一覧表示します。

Standard では XAML のプロパティおよび列挙体の新しい名前を持つコントロールの一覧もあります。

## <a name="controls"></a>コントロール

<table style="width:300px">
  <tr><th>Xamarin.Forms</th><th>XAML の標準</th></tr>
  <tr><td>フレーム</td><td>境界線</td></tr>
  <tr><td>ピッカー</td><td>ComboBox</td></tr>
  <tr><td>ActivityIndicator</td><td>ProgressRing</td></tr>
  <tr><td>StackLayout</td><td>StackPanel</td></tr>
  <tr><td>group1</td><td>TextBlock</td></tr>
  <tr><td>入力</td><td>TextBox</td></tr>
  <tr><td>切り替え</td><td>ToggleSwitch</td></tr>
  <tr><td>ContentView</td><td>UserControl</td></tr>
</table>

## <a name="properties-and-enumerations"></a>プロパティおよび列挙

<table>
  <tr><th>Xamarin.Forms<br/>更新されたプロパティを持つコントロール</th><th>Xamarin.Forms<br/>プロパティまたは列挙型</th><th>XAML の標準<br/>同等の表記</th></tr>
  <tr><td>ボタン、エントリ、ラベル、DatePicker、エディター、SearchBar、TimePicker</td><td>TextColor</td><td>前景</td></tr>
  <tr><td>VisualElement</td><td>BackgroundColor</td><td><i>バック グラウンド *</i></td></tr>
  <tr><td>ボタンの選択</td><td>BorderColor、囲んで</td><td>BorderBrush</td></tr>
  <tr><td>ボタン</td><td>BorderWidth</td><td>BorderThickness</td></tr>
  <tr><td>ProgressBar</td><td>進行状況</td><td>[値]</td></tr>
  <tr><td>ボタン、エントリ、ラベル、エディター、SearchBar、スパン、フォント</td><td>FontAttributes<br/>太字、斜体、なし</td><td>FontStyle<br/>斜体、標準</td></tr>
  <tr><td>ボタン、エントリ、ラベル、エディター、SearchBar、スパン、フォント</td><td>FontAttributes</td><td><i>FontWeights *</i><br/>太字、通常</td></tr>
  <tr><td>InputView</td><td>キーボード<br/>既定値は、Url、番号、電話、テキスト、チャット、電子メール</td><td><i>InputScopeNameValue *</i><br/>既定値は、Url、番号、TelephoneNumber、テキスト、チャット、EmailNameOrAddress</td></tr>
  <tr><td>StackPanel</td><td>StackOrientation</td><td><i>印刷の向き *</i></td></tr>
</table>

> [!IMPORTANT]
> マークされた項目 * 現在のプレビューで完全ではありません


## <a name="related-links"></a>関連リンク

- [NuGet のプレビュー](https://aka.ms/xf-xamlstandard-nuget)
