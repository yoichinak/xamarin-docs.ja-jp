---
title: 第 5 章の概要です。 サイズの処理
description: 'Xamarin.Forms によるモバイル アプリの作成: 第 5 章の概要。 サイズの処理'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 486800E9-C09F-4B95-9AC2-C0F8FE563BCF
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: fd6694de756938ff564bed0923427fe62153116a
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/07/2018
ms.locfileid: "53056085"
---
# <a name="summary-of-chapter-5-dealing-with-sizes"></a>第 5 章の概要です。 サイズの処理

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05)

> [!NOTE]
> このページに関する注意事項は、この本で説明されている内容が Xamarin.Forms が異なっている領域を示しています。

Xamarin.Forms でいくつかのサイズがこれまでに発生しました。

- IOS のステータス バーの高さは 20 です。
- `BoxView`には、既定の幅と高さ 40
- 既定の`Padding`で、`Frame`は 20 です
- 既定の`Spacing`上、 `StackLayout` 6 は、
- `Device.GetNamedSize`メソッドは、数値のフォント サイズを返します。

これらのサイズはピクセル単位ではありません。 これらはむしろ、デバイスに依存しない単位を個別に各プラットフォームで認識されません。

## <a name="pixels-points-dps-dips-and-dius"></a>ピクセル、ポイント、dps、Dip、および DIUs

Microsoft Windows、Apple Mac の履歴、早い段階では、プログラマは、ピクセル単位でいました。 ただし、高解像度ディスプレイの登場には、画面座標により仮想化され、抽象アプローチが必要です。 Mac の世界でプログラマの作業の単位で*ポイント*、Windows 開発者が使用中に、1/72 インチ従来*デバイス非依存単位*1/96 インチに基づいて (DIUs)。

ただし、モバイル デバイスは、一般に、顔に近い保持されていて、デスクトップより高い解像度画面で、高いピクセル密度を許容できることを意味します。

Apple の iPhone および iPad デバイスを対象とするプログラマは単位の作業を続ける*ポイント*がこれらのポイントは 1 インチの 160 です。 デバイスによっては、ある可能性があります 1、2、または 3 のピクセルをポイントします。

Android は似ています。 プログラマが作業の単位で*密度に依存しないピクセル*(dp)、dps とピクセル間のリレーションシップが 160 dp は 1 インチに基づいています。

Windows のスマート フォンとモバイル デバイスでは、160 のデバイスに依存しない単位は 1 インチの近くに何のスケーリング因子も確立しています。

> [!NOTE]
> Xamarin.Forms には、任意の Windows ベースの電話またはモバイル デバイスがサポートされていません。

要約すると、スマート フォンやタブレットを対象とする Xamarin.Forms のプログラマはすべての測定単位が次の条件に基づいていると想定できます。

- 1 インチに相当する 160 ユニット
- 64、センチメートル単位

読み取り専用[ `Width` ](xref:Xamarin.Forms.VisualElement.Width)と[ `Height` ](xref:Xamarin.Forms.VisualElement.Height)によって定義されたプロパティ`VisualElement`既定の値を「モック」 &ndash;1。 要素がサイズし、レイアウトを収容されている場合にのみこれらのプロパティはデバイスに依存しない単位内の要素の実際のサイズが反映されます。 このサイズが含まれる`Padding`要素で設定がない、`Margin`します。

視覚的要素が発生した、 [ `SizeChanged` ](xref:Xamarin.Forms.VisualElement.SizeChanged)イベントとその`Width`または`Height`が変更されました。 [ **WhatSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/WhatSize)サンプルでは、このイベントを使用して、プログラムの画面のサイズを表示します。

## <a name="metrical-sizes"></a>Metrical サイズ

[ **MetricalBoxView** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/MetricalBoxView)使用[ `WidthRequest` ](xref:Xamarin.Forms.VisualElement.WidthRequest)と[ `HeightRequest` ](xref:Xamarin.Forms.VisualElement.HeightRequest)を表示する、 `BoxView` 1 インチ高さ、もう 1 つセンチメートルの幅。

## <a name="estimated-font-sizes"></a>推定のフォント サイズ

[**フォント サイズ**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FontSizes)サンプル 160 単位-the インチにルールを使用して、ポイント単位のフォント サイズを指定する方法を示しています。 この手法を使用してプラットフォーム間で視覚的な一貫性がよりも良い`Device.GetNamedSize`します。

## <a name="fitting-text-to-available-size"></a>使用可能なサイズに収まらない

特定の四角形内のテキストのブロックを計算することで適合することができます、`FontSize`の`Label`次の条件を使用します。

- 行間隔は、フォント サイズ (Windows プラットフォームで 130%) の 120% です。
- 平均文字幅は、フォント サイズの 50% です。

[ **EstimatedFontSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/EstimatedFontSize)サンプルは、この手法を示します。 このプログラムは、前に書き込まれた、 [ `Margin` ](xref:Xamarin.Forms.View.Margin)を使うようにプロパティが使用できる、 [ `ContentView` ](xref:Xamarin.Forms.ContentView)で、 [ `Padding` ](xref:Xamarin.Forms.Layout.Padding)をシミュレートする設定をマージンです。

[![推定のフォント サイズのトリプル スクリーン ショット](images/ch05fg07-small.png "テキストが使用可能なサイズに合わせる")](images/ch05fg07-large.png#lightbox "テキストが使用可能なサイズに合わせる")

## <a name="a-fit-to-size-clock"></a>サイズのサイズに合わせて時計

[ **FitToSizeClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FitToSizeClock)サンプルの使用例[ `Device.StartTimer` ](xref:Xamarin.Forms.Device.StartTimer(System.TimeSpan,System.Func{System.Boolean}))時計を更新するときにアプリケーションを定期的に通知するタイマーを開始します。 フォント サイズは、表示可能な限り大きくに 6 番目の 1 ページの幅に設定されます。

## <a name="accessibility-issues"></a>アクセシビリティの問題

**EstimatedFontSize**プログラムと**FitToSizeClock**両方のプログラムには微妙な欠陥が含まれている: ユーザーが Android や Windows 10 Mobile、不要になったプログラム上、電話のユーザー補助の設定を変更するかどうか見積もることができます、テキストのレンダリング方法に大きなフォント サイズに基づいています。 [ **AccessibilityTest** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/AccessibilityTest)サンプルは、この問題を示します。

## <a name="empirically-fitting-text"></a>経験的調整テキスト

四角形にテキストに合わせて別の方法を経験的にレンダリングされたテキストのサイズを計算し、アップまたはスケール ダウンを調整することです。 書籍の呼び出しでプログラム[ `GetSizeRequest` ](xref:Xamarin.Forms.VisualElement.GetSizeRequest(System.Double,System.Double))要素の目的のサイズを取得するビジュアル要素にします。 メソッドは非推奨とされて、そのプログラムが代わりに呼び出す必要があります[ `Measure`](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags))します。

`Label`、最初の引数 (折り返しを許可) するコンテナーの幅をする必要があります、2 番目に、引数を設定する必要がありますに`Double.PositiveInfinity`に制約なしの高さに揃えます。 [ **EmpiricalFontSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/EmpiricalFontSize)サンプルは、この手法を示します。



## <a name="related-links"></a>関連リンク

- [第 5 章「フル テキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch05-Apr2016.pdf)
- [第 5 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05)
- [第 5 章F#サンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FS)
