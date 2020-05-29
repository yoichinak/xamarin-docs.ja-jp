---
title: の XAML プレビューアーXamarin.Forms
description: この記事では、XAML プレビューアーを使用して、入力時に表示されるレイアウトを確認する方法について説明し Xamarin.Forms ます。 XAML プレビューアーは、Visual Studio 2019 および Visual Studio 2019 for Mac で使用できます。
zone_pivot_groups: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 6916d5fde688c5b1162f12db0d36bc3ca27156d8
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2020
ms.locfileid: "84137320"
---
# <a name="xaml-previewer-for-xamarinforms"></a>の XAML プレビューアーXamarin.Forms

_入力時に表示される Xamarin.Forms レイアウトを確認する_

## <a name="overview"></a>概要

XAML プレビューアーは、 Xamarin.Forms iOS と Android で xaml ページがどのように表示されるかを示しています。 XAML に変更を加えると、コードと共にすぐにプレビューが表示されます。 XAML プレビューアーは、Visual Studio および Visual Studio for Mac で使用できます。

## <a name="getting-started"></a>はじめに

::: zone pivot="windows"

### <a name="visual-studio-2019"></a>Visual Studio 2019

[分割ビュー] ペインの矢印をクリックすると、XAML プレビューアーを開くことができます。 既定の分割ビューの動作を変更する場合は、[**ツール] > オプション > Xamarin > Xamarin.Forms XAML プレビューアー** ] ダイアログボックスを使用します。 このダイアログボックスでは、既定のドキュメントビューと分割の向きを選択できます。

[![Xamarin.FormsVisual Studio でのプレビューアーオプション](xaml-previewer-images/xamlp-options-vs-sm.png "[!ファンド.NO LOC (Xamarin. Forms)] プレビューアーオプション (Visual Studio)")](xaml-previewer-images/xamlp-options-vs-lg.png#lightbox)

XAML ファイルを開くと、エディターは、[**ツール > オプション] > [Xamarin > Xamarin.Forms XAML プレビューアー** ] ダイアログボックスで選択した設定に基づいて、すべてのサイズを開くか、プレビューアーの横に開きます。 ただし、エディターウィンドウでは、ファイルごとに分割を変更できます。

#### <a name="xaml-preview-controls"></a>XAML プレビューコントロール

分割ビューペインでこれらのボタンを選択することにより、コード、XAML プレビューアー、またはその両方を表示するかどうかを選択します。 中央のボタンをクリックすると、プレビューアーとコードのサイドが交換されます。

[![Xamarin.FormsVisual Studio でデザイン、ソース、および分割ビューを切り替えるためのプレビューアーコントロール](xaml-previewer-images/xamlp-controls-splitview-vs-sm.png "[!ファンド.NO LOC (Xamarin)] プレビューアーコントロールで、Visual Studio でデザイン、ソース、および分割ビューを切り替える")](xaml-previewer-images/xamlp-controls-splitview-vs-lg.png#lightbox)

画面を縦または横に分割するか、ウィンドウ全体を折りたたむかを変更できます。

[![Xamarin.FormsVisual Studio でのプレビューアーペインの向きコントロール](xaml-previewer-images/xamlp-controls-orientation-vs-sm.png "[!ファンド.NO LOC (Xamarin)] プレビューウィンドウの向きコントロール (Visual Studio)")](xaml-previewer-images/xamlp-controls-orientation-vs-lg.png#lightbox)

#### <a name="enable-or-disable-the-xaml-previewer"></a>XAML プレビューアーを有効または無効にする

**既定の Xaml エディター**として [**既定の XML エディター** ] を選択すると、[**ツール > オプション] > [Xamarin > Xamarin.Forms XAML プレビューアー** ] ダイアログボックスで xaml プレビューアーを無効にすることができます。 これにより、ドキュメントアウトライン、プロパティパネル、および XAML ツールボックスもオフになります。 XAML プレビューアーとそのツールを再び有効にするには、**既定の Xaml エディター**を** Xamarin.Forms プレビューアー**に変更します。

::: zone-end
::: zone pivot="macos"

### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

[**プレビュー** ] ボタンは、XAML ページを開いたときにエディターに表示されます。 任意の XAML ドキュメントウィンドウの左下にある [**プレビュー** ] ボタンまたは [**分割**] ボタンを押して、プレビューアーを表示または非表示にします。

[![Xamarin.Formsプレビューまたは分割ボタンでのプレビューアーの有効化](xaml-previewer-images/xamlp-list-sml.png)](xaml-previewer-images/xamlp-list.png#lightbox)

> [!NOTE]
> 以前のバージョンの Visual Studio for Mac では、**プレビュー**ボタンはウィンドウの右上にありました。

#### <a name="enable-or-disable-the-xaml-previewer"></a>XAML プレビューアーを有効または無効にする

既定の**Xaml エディター**として**既定の XML エディター**を選択すると、 **Visual Studio の [> の基本設定] > [テキストエディター > Xaml** ] ダイアログで xaml プレビューアーを無効にすることができます。 これにより、ドキュメントアウトライン、プロパティパネル、および XAML ツールボックスもオフになります。 XAML プレビューアーとそのツールを再び有効にするには、**既定の Xaml エディター**を** Xamarin.Forms プレビューアー**に変更します。

::: zone-end

## <a name="xaml-previewer-options"></a>XAML プレビューアーオプション

プレビューウィンドウの上部に表示されるオプションは次のとおりです。

* **Android** –画面の android バージョンを表示する
* **ios** –画面の ios バージョンを表示し*ます (注: Windows で Visual Studio を使用している場合、このモードを使用するには、 [Mac とペアリング](~/ios/get-started/installation/windows/connecting-to-mac/index.md)する必要があります)。*
* **デバイス**-解像度と画面サイズを含む Android または iOS デバイスのドロップダウンリスト
* **縦 (アイコン)** –プレビューで縦向きを使用します
* **横 (アイコン)** –プレビューに横向きを使用します

## <a name="detect-design-mode"></a>デザインモードの検出

静的プロパティは、 [`DesignMode.IsDesignModeEnabled`](xref:Xamarin.Forms.DesignMode.IsDesignModeEnabled) アプリケーションがプレビューアーで実行されているかどうかを示します。 これを使用すると、アプリケーションがプレビューアーで実行されている場合、または実行されていない場合にのみ実行されるコードを指定できます。

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
* クラスにパラメーターなしのコンストラクターがあることを確認し `App` ます。
* Xamarin.Formsバージョンを確認してください。3.6 以上である必要があり Xamarin.Forms ます。 NuGet を使用して最新バージョンに更新でき Xamarin.Forms ます。
* JDK のインストールを確認する-Android のプレビューには[jdk 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html)以上が必要です。
* にあるページの C# コードで、初期化されたクラスをラップしてみてください `if (!DesignMode.IsDesignModeEnabled)` 。

### <a name="custom-controls-arent-rendering"></a>カスタムコントロールがレンダリングしない

プロジェクトをビルドしてみてください。 コントロールをレンダリングできない場合、またはコントロールの作成者がデザイン時のレンダリングをオプトアウトした場合、プレビューアーによってコントロールの基本クラスが表示されます。 詳細については、「 [XAML プレビューアーでのカスタムコントロールのレンダリング](render-custom-controls.md)」を参照してください。
