---
title: '第 5 章の概要: サイズの処理'
description: 'Xamarin.Forms でモバイル アプリを作成する: 第 5 章の概要: サイズの処理'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 486800E9-C09F-4B95-9AC2-C0F8FE563BCF
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 8cd68fdc3d7e94b6ae12ce6296dc4d698bc0ebd2
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93370261"
---
# <a name="summary-of-chapter-5-dealing-with-sizes"></a>第 5 章の概要: サイズの処理

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05)

> [!NOTE]
> この本は 2016 年春に発行されて以降、改訂されていません。 多くの情報はまだ価値がありますが、一部の資料は古くなっており、トピックの中にはまったく正しくないものまたは不完全なものもあります。

これまでに、Xamarin.Forms のいくつかのサイズについて説明しました。

- iOS の状態バーの高さは 20 です
- `BoxView` の幅と高さの規定値は 40 です
- `Frame` の `Padding` の既定値は 20 です
- `StackLayout` の `Spacing` の規定値は 6 です
- `Device.GetNamedSize` メソッドでは数値のフォント サイズが返されます

これらのサイズはピクセルではありません。 代わりに、各プラットフォームによって個別に認識される、デバイスに依存しない単位が使用されています。

## <a name="pixels-points-dps-dips-and-dius"></a>ピクセル、ポイント、dp、DIP、DIU

Apple Mac と Microsoft Windows の歴史の初期には、プログラマーはピクセル単位で作業を行っていました。 しかし、高解像度ディスプレイの出現により、画面の座標に対してより仮想化された、抽象的なアプローチが必要になりました。 Mac の世界では、プログラマーは "*ポイント*" の単位 (伝統的に 1/72 インチ) で作業を行っていました。一方、Windows の開発者は、1/96 インチをベースとする "*デバイスに依存しない単位*" (DIU) を使用しました。

しかし、モバイル デバイスは通常、顔のかなり近くに持つため、デスクトップ画面よりも解像度が高くなります。これは、より高いピクセル密度が許容されることを意味します。

Apple iPhone および iPad デバイスを対象とするプログラマーは、"*ポイント*" の単位で作業を続けていますが、1 インチは 160 ポイントに換算されます。 デバイスによっては、1 ポイントが 1、2、または 3 ピクセルに換算される場合があります。

Android も同様です。 プログラマーは、"*密度に依存しないピクセル*" (dp) の単位で作業を行います。dp とピクセルの関係は、1 インチあたり 160 dp に基づいています。

Windows のスマートフォンおよびモバイル デバイスに関しても、1 インチあたり 160 のデバイスに依存しない単位に近くなる倍率が確立されています。

> [!NOTE]
> Xamarin.Forms では、Windows ベースのスマートフォンやモバイル デバイスはサポートされなくなりました。

まとめると、スマートフォンとタブレットを対象とする Xamarin.Forms のプログラマーは、すべての測定単位が次の基準に基づいていると想定できます。

- 1 インチあたり 160 単位、または
- 1 センチメートルあたり 64 単位

`VisualElement` によって定義されている読み取り専用の [`Width`](xref:Xamarin.Forms.VisualElement.Width) および [`Height`](xref:Xamarin.Forms.VisualElement.Height) プロパティには、"偽の" 既定値 &ndash;1 が設定されています。 要素のサイズが設定されてレイアウトに収容された場合にのみ、この要素の実際のサイズが、デバイスに依存しない単位で、これらのプロパティに反映されます。 このサイズには、要素に設定されているすべての `Padding` が含まれますが、`Margin` は含まれません。

ビジュアル要素の `Width` または `Height` が変更されると、[`SizeChanged`](xref:Xamarin.Forms.VisualElement.SizeChanged) イベントが発生します。 [**WhatSize**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/WhatSize) サンプルでは、プログラムの画面のサイズを表示するためにこのイベントが使用されています。

## <a name="metrical-sizes"></a>測定基準のサイズ

[**MetricalBoxView**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/MetricalBoxView) では、[`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) と [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) を使用して、高さ 1 インチ、幅 1 センチメートルの `BoxView` が表示されます。

## <a name="estimated-font-sizes"></a>推定フォント サイズ

[**FontSizes**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FontSizes) サンプルでは、1 インチあたり 160 単位のルールに従って、ポイント単位でフォント サイズを指定する方法が示されています。 この手法を使用した場合、プラットフォーム間の視覚的な一貫性は、`Device.GetNamedSize` よりも向上します。

## <a name="fitting-text-to-available-size"></a>テキストを使用可能なサイズに調整する

次の条件を使用して `Label` の `FontSize` を計算することによって、テキストのブロックを特定の四角形内に合わせることができます。

- 行間がフォント サイズの 120% になる (Windows プラットフォームでは 130%)。
- 文字幅の平均がフォント サイズの 50% になる。

[**EstimatedFontSize**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/EstimatedFontSize) サンプルでは、この手法が示されています。 このプログラムは [`Margin`](xref:Xamarin.Forms.View.Margin) プロパティを使用できるようになる前に記述されたため、余白をシミュレートするために [`Padding`](xref:Xamarin.Forms.Layout.Padding) 設定と共に [`ContentView`](xref:Xamarin.Forms.ContentView) が使用されています。

[![推定フォント サイズのトリプル スクリーンショット](images/ch05fg07-small.png "使用可能なサイズにテキストを合わせる")](images/ch05fg07-large.png#lightbox "使用可能なサイズにテキストを合わせる")

## <a name="a-fit-to-size-clock"></a>サイズを合わせる時計

[**FitToSizeClock**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FitToSizeClock) サンプルでは、[`Device.StartTimer`](xref:Xamarin.Forms.Device.StartTimer(System.TimeSpan,System.Func{System.Boolean})) を使用して、時計を更新する時間になったことを定期的にアプリケーションに通知するタイマーを開始する方法が示されています。 フォント サイズは、可能な限り大きく表示されるように、ページ幅の 6 分の 1 に設定されます。

## <a name="accessibility-issues"></a>アクセシビリティの問題

**EstimatedFontSize** プログラムと **FitToSizeClock** プログラムには、どちらも次の小さな欠陥が含まれています:ユーザーが Android または Windows 10 Mobile 上でスマートフォンのアクセシビリティ設定を変更した場合、プログラムでは、フォント サイズに基づいてテキストがどの程度の大きさで表示されるかを推定できなくなります。 [**AccessibilityTest**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/AccessibilityTest) サンプルではこの問題が示されています。

## <a name="empirically-fitting-text"></a>経験的なテキストの調整

テキストを四角形に合わせるもう 1 つの方法は、表示されたテキスト サイズを経験的に計算し、それを上下に調整することです。 書籍のプログラムでは、ビジュアル要素で [`GetSizeRequest`](xref:Xamarin.Forms.VisualElement.GetSizeRequest(System.Double,System.Double)) を呼び出して、その要素の目的のサイズを取得しています。 このメソッドは非推奨になっていて、プログラムでは代わりに、[`Measure`](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags)) を呼び出す必要があります。

`Label` の場合、最初の引数は (折り返しを可能にするために) コンテナーの幅にする必要がありますが、2 番目の引数は、高さを非制限にするために `Double.PositiveInfinity` に設定する必要があります。 [**EmpiricalFontSize**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/EmpiricalFontSize) サンプルでは、この手法が示されています。

## <a name="related-links"></a>関連リンク

- [第 5 章の全文 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch05-Apr2016.pdf)
- [第 5 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05)
- [第 5 章の F# のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FS)
