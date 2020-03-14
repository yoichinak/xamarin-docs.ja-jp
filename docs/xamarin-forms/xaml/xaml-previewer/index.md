---
title: Xamarin. フォーム用の XAML プレビューアー
description: この記事では、XAML プレビューアーを使用して、入力時に表示される Xamarin のレイアウトを確認する方法について説明します。 XAML プレビューアーは、Visual Studio 2019 および Visual Studio 2019 for Mac で使用できます。
zone_pivot_groups: platform
ms.prod: xamarin
ms.assetid: 84769ff1-72fd-4c44-8251-dd6d5bf8c7b2
ms.technology: xamarin-forms
author: maddyleger1
ms.author: maleger
ms.date: 02/04/2019
ms.openlocfilehash: b287d523101bb8ca7faca8ea95ee898ccf9c0bb1
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/13/2020
ms.locfileid: "79306423"
---
# <a name="xaml-previewer-for-xamarinforms"></a>Xamarin. フォーム用の XAML プレビューアー

_入力時に表示される Xamarin のフォームレイアウトを確認する_

## <a name="overview"></a>概要

XAML プレビューアーには、iOS と Android に関する Xamarin の XAML ページの外観が示されています。 XAML に変更を加えると、コードと共にすぐにプレビューが表示されます。 XAML プレビューアーは、Visual Studio および Visual Studio for Mac で使用できます。

## <a name="getting-started"></a>作業の開始

::: zone pivot="windows"

### <a name="visual-studio-2019"></a>Visual Studio 2019

[分割ビュー] ペインの矢印をクリックすると、XAML プレビューアーを開くことができます。 既定の分割ビューの動作を変更する場合は、**ツール > オプション > Xamarin > フォームプレビューアー**  ダイアログを使用します。 このダイアログボックスでは、既定のドキュメントビューと分割の向きを選択できます。

[![Visual Studio の Xamarin. フォームプレビューアーオプション](xaml-previewer-images/xamlp-options-vs-sm.png "Visual Studio の Xamarin. フォームプレビューアーオプション")](xaml-previewer-images/xamlp-options-vs-lg.png#lightbox)

XAML ファイルを開くと、エディターは、[**ツール > オプション] > [Xamarin > フォーム**] ポップアップダイアログで選択した設定に基づいて、フルサイズまたはプレビューアーの横に表示されます。 ただし、エディターウィンドウでは、ファイルごとに分割を変更できます。

#### <a name="xaml-preview-controls"></a>XAML プレビューコントロール

分割ビューペインでこれらのボタンを選択することにより、コード、XAML プレビューアー、またはその両方を表示するかどうかを選択します。 中央のボタンをクリックすると、プレビューアーとコードのサイドが交換されます。

[![Visual Studio でデザイン、ソース、および分割ビューを切り替えるための Xamarin フォームプレビューアーコントロール](xaml-previewer-images/xamlp-controls-splitview-vs-sm.png "Visual Studio でデザイン、ソース、および分割ビューを切り替えるための Xamarin フォームプレビューアーコントロール")](xaml-previewer-images/xamlp-controls-splitview-vs-lg.png#lightbox)

画面を縦または横に分割するか、ウィンドウ全体を折りたたむかを変更できます。

[![Visual Studio での Xamarin フォームのプレビューアーペインの向きコントロール](xaml-previewer-images/xamlp-controls-orientation-vs-sm.png "Visual Studio での Xamarin フォームのプレビューアーペインの向きコントロール")](xaml-previewer-images/xamlp-controls-orientation-vs-lg.png#lightbox)

::: zone-end
::: zone pivot="macos"

### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

**[プレビュー]** ボタンは、XAML ページを開いたときにエディターに表示されます。 任意の XAML ドキュメントウィンドウの左下にある **[プレビュー]** ボタンまたは **[分割]** ボタンを押して、プレビューアーを表示または非表示にします。

[[プレビュー] ボタンまたは [分割] ボタンを使用して ![Xamarin. フォームプレビューアーを有効にする](xaml-previewer-images/xamlp-list-sml.png)](xaml-previewer-images/xamlp-list.png#lightbox)

> [!NOTE]
> 以前のバージョンの Visual Studio for Mac では、**プレビュー**ボタンはウィンドウの右上にありました。

::: zone-end

## <a name="xaml-previewer-options"></a>XAML プレビューアーオプション

プレビューウィンドウの上部に表示されるオプションは次のとおりです。

* **Android** –画面の android バージョンを表示する
* **ios** –画面の ios バージョンを表示し*ます (注: Windows で Visual Studio を使用している場合、このモードを使用するには、 [Mac とペアリング](~/ios/get-started/installation/windows/connecting-to-mac/index.md)する必要があります)。*
* **デバイス**-解像度と画面サイズを含む Android または iOS デバイスのドロップダウンリスト
* **縦 (アイコン)** –プレビューで縦向きを使用します
* **横 (アイコン)** –プレビューに横向きを使用します

## <a name="detect-design-mode"></a>デザインモードの検出

静的[`DesignMode.IsDesignModeEnabled`](xref:Xamarin.Forms.DesignMode.IsDesignModeEnabled)プロパティは、アプリケーションがプレビューアーで実行されているかどうかを示します。 これを使用すると、アプリケーションがプレビューアーで実行されている場合、または実行されていない場合にのみ実行されるコードを指定できます。

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

このプロパティは、デザイン時に実行に失敗したページコンストラクター内のライブラリを初期化する場合に便利です。

## <a name="troubleshooting"></a>トラブルシューティング

プレビューアーが機能していない場合は、以下の問題と[Xamarin フォーラム](https://forums.xamarin.com/categories/xamarin-forms)を確認してください。

### <a name="xaml-previewer-isnt-showing-or-shows-an-error"></a>XAML プレビューアーが表示されていないか、エラーが表示されています

* プレビューアーが起動するまでに時間がかかることがあります。準備が整うまで、"レンダリングを初期化しています" と表示されます。
* XAML ファイルを閉じてから再度開いてみてください。
* `App` クラスにパラメーターなしのコンストラクターがあることを確認します。
* Xamarin. Forms バージョンを確認します。これは少なくとも Xamarin. Forms 3.6 である必要があります。 NuGet を使用して最新の Xamarin. Forms バージョンに更新できます。
* JDK のインストールを確認する-Android のプレビューには[jdk 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html)以上が必要です。
* `if (!DesignMode.IsDesignModeEnabled)`で、ページのC#分離コードで初期化されたクラスをラップしてみてください。

### <a name="custom-controls-arent-rendering"></a>カスタムコントロールがレンダリングしない

プロジェクトをビルドしてみてください。 コントロールをレンダリングできない場合、またはコントロールの作成者がデザイン時のレンダリングをオプトアウトした場合、プレビューアーによってコントロールの基本クラスが表示されます。 詳細については、「 [XAML プレビューアーでのカスタムコントロールのレンダリング](render-custom-controls.md)」を参照してください。
