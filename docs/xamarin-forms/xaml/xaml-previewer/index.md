---
title: Xamarin.Forms 用 XAML プレビューアー
description: この記事では、XAML プレビューアーを使用してレンダリングを入力すると、Xamarin.Forms のレイアウトを表示する方法について説明します。 XAML プレビューアーには、Visual Studio 2019 および for mac。 Visual Studio 2019 です。
zone_pivot_groups: platform
ms.prod: xamarin
ms.assetid: 84769ff1-72fd-4c44-8251-dd6d5bf8c7b2
ms.technology: xamarin-forms
author: maddyleger1
ms.author: maleger
ms.date: 02/04/2019
ms.openlocfilehash: db243a9c8dcb25f51bc7926a7aa239531e9c24f6
ms.sourcegitcommit: 3489c281c9eb5ada2cddf32d73370943342a1082
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/18/2019
ms.locfileid: "58859012"
---
# <a name="xaml-previewer-for-xamarinforms"></a>Xamarin.Forms 用 XAML プレビューアー

_レンダリングを入力すると、Xamarin.Forms のレイアウトを参照してください。_

## <a name="overview"></a>概要

XAML プレビューアーは iOS と Android で Xamarin.Forms XAML ページがどのように表示されるかを示しています。 XAML を変更するときに、コードと共にすぐにプレビューを確認します。 XAML プレビューアーには、Visual Studio および Visual Studio for mac です。

## <a name="getting-started"></a>作業の開始

::: zone pivot="windows"

### <a name="visual-studio-2019"></a>Visual Studio 2019

分割ビューのウィンドウにある矢印をクリックして、XAML プレビューアーを開くことができます。 既定値に分割表示動作を変更する場合は、使用、**ツール > オプション > Xamarin > フォーム プレビューアー**ダイアログ。 このダイアログ ボックスでは、既定のドキュメント ビューと分割の向きを選択できます。

[![Visual Studio のオプションの Xamarin.Forms プレビューアー](xaml-previewer-images/xamlp-options-vs-sm.png "Visual Studio で Xamarin.Forms プレビューアーのオプション")](xaml-previewer-images/xamlp-options-vs-lg.png#lightbox)

エディターが開きますフルサイズまたは次へ で選択した設定に基づいて、プレビューアーを XAML ファイルを開くときに、**ツール > オプション > Xamarin > フォーム プレビューアー**ダイアログ。 ただし、エディター ウィンドウ内の各ファイルの分割を変更できます。

#### <a name="xaml-preview-controls"></a>XAML プレビュー コントロール

XAML プレビューアーをコードを表示するかどうか、またはウィンドウを表示、分割ボタンを選択すると、両方とも選択します。 中央のボタンは、何側プレビューアーと、コードが上を交換します。

[![デザイン、ソース、および Visual Studio での分割ビューの切り替えにコントロールを Xamarin.Forms プレビューアー](xaml-previewer-images/xamlp-controls-splitview-vs-sm.png "Xamarin.Forms プレビューアーのコントロールをデザイン、ソース、および Visual Studio での分割ビューを切り替える")](xaml-previewer-images/xamlp-controls-splitview-vs-lg.png#lightbox)

画面が垂直方向または水平方向に分割するかどうかを変更するか、完全に 1 つのペインを折りたたみます。

[![Visual Studio で Xamarin.Forms プレビューアー ウィンドウの向きコントロール](xaml-previewer-images/xamlp-controls-orientation-vs-sm.png "Visual Studio で Xamarin.Forms プレビューアー ウィンドウの向きのコントロール")](xaml-previewer-images/xamlp-controls-orientation-vs-lg.png#lightbox)

::: zone-end
::: zone pivot="macos"

### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

**プレビュー** XAML ページを開くと、エディター ボタンが表示されます。 表示またはキーを押して、プレビューアーを非表示、**プレビュー**任意の XAML ドキュメント ウィンドウの右上隅にあるボタン。

[![Visual Studio for Mac で Xamarin.Forms プレビューアー](xaml-previewer-images/xamlp-list-sml.png "Visual Studio for Mac で Xamarin.Forms プレビューアー")](xaml-previewer-images/xamlp-list.png#lightbox)

::: zone-end

## <a name="xaml-previewer-options"></a>XAML プレビューアー オプション

プレビュー ウィンドウの上部のオプションがあります。

* **Android** – 画面の Android バージョンを表示します。
* **iOS** – 画面の iOS バージョンを表示する (*に注意してください。Windows を Visual Studio を使用している場合あります[、Mac とペアリングして](~/ios/get-started/installation/windows/connecting-to-mac/index.md)このモードを使用する*)
* **デバイス**-解像度や画面サイズを含む、Android または iOS のデバイスのドロップダウン リスト
* **縦 (アイコン)** -縦向きを使用して、プレビュー
* **横 (アイコン)** – 使用して横長の向きのプレビュー

## <a name="detect-design-mode"></a>デザイン モードを検出します。

静的な[ `DesignMode.IsDesignModeEnabled` ](xref:Xamarin.Forms.DesignMode.IsDesignModeEnabled)プロパティは、プレビューアーで、アプリケーションが実行されているかどうかを示します。 これを使用して、アプリケーションが、または、プレビューアーで実行されていないときにのみ実行されるコードを指定できます。

```csharp
if (DesignMode.IsDesignModeEnabled)
{
  // Previewer only code  
}

if (!DesignMode.IsDesignModeEnabled)
{
  // Don't run in the Previewer  
}
```

このプロパティはデザイン時に実行に失敗した、ページのコンス トラクターでライブラリを初期化する場合に便利です。

## <a name="troubleshooting"></a>トラブルシューティング

以下の問題の確認と[Xamarin Forums](https://forums.xamarin.com/categories/xamarin-forms)プレビューアーが動作していない場合、します。

### <a name="xaml-previewer-isnt-showing-or-shows-an-error"></a>XAML プレビューアーが表示されていない、またはエラーを示します

* 起動するには、プレビューアーの時間がかかることができます - わかります「初期化レンダリング」準備が整うまでです。
* 閉じてみてください XAML ファイル。
* いることを確認、`App`クラスにはパラメーターなしのコンス トラクター。
* -Xamarin.Forms バージョンを確認する以上である必要がある Xamarin.Forms 3.6 です。 NuGet から最新の Xamarin.Forms バージョンを更新したことができます。
* Android のプレビュー - JDK のインストールに必要な最小チェック[JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html)します。
* 折り返しのいずれかをお試しください ページのクラスを初期化するC#で分離コード`if (!DesignMode.IsDesignModeEnabled)`します。

### <a name="custom-controls-arent-rendering"></a>カスタム コントロールが表示されません。

プロジェクトのビルドを再試行してください。 プレビューアーには、コントロールを表示するために失敗した場合、または場合のデザイン時レンダリングのコントロールの作成者のオプトアウトのコントロールの基底クラスが表示されます。 詳細については、次を参照してください。 [XAML プレビューアーでカスタム コントロールのレンダリング](render-custom-controls.md)します。
