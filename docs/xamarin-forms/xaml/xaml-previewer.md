---
title: Xamarin.Forms 用 XAML プレビューアー
description: この記事では、XAML プレビューアーを使用してレンダリングを入力すると、Xamarin.Forms のレイアウトを表示する方法について説明します。 XAML プレビューアーは Visual Studio 2017、Visual Studio for Mac、および Visual Studio 2019 (プレビュー) で使用できます。
zone_pivot_groups: platform-dev16
ms.prod: xamarin
ms.assetid: 84769ff1-72fd-4c44-8251-dd6d5bf8c7b2
ms.technology: xamarin-forms
author: maddyleger1
ms.author: maleger
ms.date: 02/04/2019
ms.openlocfilehash: 4258eca8e18229420ceb043a010c896137187653
ms.sourcegitcommit: c6ff24b524d025d7e87b7b9c25f04c740dd93497
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2019
ms.locfileid: "56240384"
---
# <a name="xaml-previewer-for-xamarinforms"></a>Xamarin.Forms 用 XAML プレビューアー

_入力するたびに描画されるXamarin.Formsレイアウトを表示しましょう!_

## <a name="requirements"></a>必要条件

XAMLプレビューアーが動作するためにはプロジェクトに最新の Xamarin.Forms NuGetパッケージが必要です。 Androidアプリをプレビューするには[JDK 1.8 x64](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)が必要です。

[リリース ノート](https://developer.xamarin.com/releases/studio/xamarin.studio_6.2/xamarin.studio_6.2/#Xamarin_Forms_Previewer)により詳細な情報があります。

## <a name="getting-started"></a>作業の開始

Xamarin.Forms の XAML プレビューアーは、編集するときに、リアルタイムで XAML ファイルのプラットフォーム固有プレビューを示します。

::: zone pivot="windows"

### <a name="visual-studio"></a>Visual Studio

XAML プレビューアーは分割ビューのウィンドウを展開して使用できます。 既定の分割ビューの動作を制御することができます、**ツール > オプション > Xamarin > フォーム プレビューアー**ダイアログ。 このダイアログ ボックスでは、既定のドキュメント ビューと分割の向きを選択できます。

[![Visual Studio のオプションの Xamarin.Forms プレビューアー](xaml-previewer-images/xamlp-options-vs-sm.png "Visual Studio で Xamarin.Forms プレビューアーのオプション")](xaml-previewer-images/xamlp-options-vs-lg.png#lightbox)

エディターで選択した設定に基づいて分割 XAML ページを開くときに、**ツール > オプション > Xamarin > フォーム プレビューアー**ダイアログ。 ただし、エディター ウィンドウ内の各ファイルの分割を変更できます。

#### <a name="xaml-preview-controls"></a>XAML プレビュー コントロール

エディター ウィンドウの上部には、どのペインが使用して、デザイン ペインに切り替え、一番上のボタンと [ソース] ウィンドウへの切り替え、下部にボタンが選択するボタンがあります。 中央のボタンは、ペインの順序を入れ替えます。

[![デザイン、ソース、および Visual Studio での分割ビューの切り替えにコントロールを Xamarin.Forms プレビューアー](xaml-previewer-images/xamlp-controls-splitview-vs-sm.png "Xamarin.Forms プレビューアーのコントロールをデザイン、ソース、および Visual Studio での分割ビューを切り替える")](xaml-previewer-images/xamlp-controls-splitview-vs-lg.png#lightbox)

エディター ウィンドウの下部には、垂直方向および水平方向の分割ペイン、および展開または現在のサブ ペインを折りたたむボタンがあります。

[![Visual Studio で Xamarin.Forms プレビューアー ウィンドウの向きコントロール](xaml-previewer-images/xamlp-controls-orientation-vs-sm.png "Visual Studio で Xamarin.Forms プレビューアー ウィンドウの向きのコントロール")](xaml-previewer-images/xamlp-controls-orientation-vs-lg.png#lightbox)

::: zone-end
::: zone pivot="macos"

### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

**プレビュー** XAML ページを開くと、エディター ボタンが表示されます。 プレビュー ウィンドウを表示または非表示にキーを押してことができます、**プレビュー**で任意の XAML ドキュメント ウィンドウの右上隅にあるボタンをクリックします。

[![Visual Studio for Mac で Xamarin.Forms プレビューアー](xaml-previewer-images/xamlp-list-sml.png "Visual Studio for Mac で Xamarin.Forms プレビューアー")](xaml-previewer-images/xamlp-list.png#lightbox)

::: zone-end
::: zone pivot="win-dev16"

### <a name="visual-studio-2019-preview"></a>Visual Studio 2019 (プレビュー)

XAML プレビューアーは分割ビューのウィンドウを展開して使用できます。 既定の分割ビューの動作を制御することができます、**ツール > オプション > Xamarin > フォーム プレビューアー**ダイアログ。 このダイアログ ボックスでは、既定のドキュメント ビューと分割の向きを選択できます。

[![Visual Studio のオプションの Xamarin.Forms プレビューアー](xaml-previewer-images/xamlp-options-vs-sm.png "Visual Studio で Xamarin.Forms プレビューアーのオプション")](xaml-previewer-images/xamlp-options-vs-lg.png#lightbox)

エディターで選択した設定に基づいて分割 XAML ページを開くときに、**ツール > オプション > Xamarin > フォーム プレビューアー**ダイアログ。 ただし、エディター ウィンドウ内の各ファイルの分割を変更できます。

#### <a name="xaml-preview-controls"></a>XAML プレビュー コントロール

エディター ウィンドウの上部には、どのペインが使用して、デザイン ペインに切り替え、一番上のボタンと [ソース] ウィンドウへの切り替え、下部にボタンが選択するボタンがあります。 中央のボタンは、ペインの順序を入れ替えます。

[![デザイン、ソース、および Visual Studio での分割ビューの切り替えにコントロールを Xamarin.Forms プレビューアー](xaml-previewer-images/xamlp-controls-splitview-vs-sm.png "Xamarin.Forms プレビューアーのコントロールをデザイン、ソース、および Visual Studio での分割ビューを切り替える")](xaml-previewer-images/xamlp-controls-splitview-vs-lg.png#lightbox)

エディター ウィンドウの下部には、垂直方向および水平方向の分割ペイン、および展開または現在のサブ ペインを折りたたむボタンがあります。

[![Visual Studio で Xamarin.Forms プレビューアー ウィンドウの向きコントロール](xaml-previewer-images/xamlp-controls-orientation-vs-sm.png "Visual Studio で Xamarin.Forms プレビューアー ウィンドウの向きのコントロール")](xaml-previewer-images/xamlp-controls-orientation-vs-lg.png#lightbox)

::: zone-end

## <a name="xaml-previewer-options"></a>XAML プレビューアー オプション

プレビュー ウィンドウの上部のオプションがあります。

* **Phone** – サイズ電話の画面上のプレビュー
* **タブレット**-サイズ タブレット画面のプレビュー
* **Android** – 画面の Android バージョンを表示します。
* **iOS** – 画面の iOS バージョンを表示する (*に注意してください。Windows を Visual Studio を使用している場合あります[、Mac とペアリングして](~/ios/get-started/installation/windows/connecting-to-mac/index.md)このモードを使用する*)
* **縦 (アイコン)** -縦向きを使用して、プレビュー
* **横 (アイコン)** – 使用して横長の向きのプレビュー

::: zone pivot="win-dev16"

## <a name="device-drop-down-menu"></a>デバイスのドロップダウン メニュー

Visual Studio 2019 (プレビュー) での Android または iOS デバイスのドロップダウン リストからをプレビューするデバイスを選択できます。 このドロップダウン リストには、「電話」や「タブレット」のオプションが置き換えられます。 IOS をプレビューするときに、iPhone および iPad デバイスがドロップダウン リストに表示されます。 Android をプレビューするときに、ドロップダウン リストは、さまざまな Android フォンおよびタブレットに表示されます。 両方の一覧には、画面サイズと画面解像度が含まれます。

::: zone-end

## <a name="detect-design-mode"></a>デザイン モードを検出します。

静的な[ `DesignMode.IsDesignModeEnabled` ](xref:Xamarin.Forms.DesignMode.IsDesignModeEnabled)プレビューアーで、アプリケーションが実行されているかどうかを決定するプロパティを調べることができます。 プレビューアーで、アプリケーションが実行されている場合にのみ実行するコードを指定できます。

```csharp
if (DesignMode.IsDesignModeEnabled)
{
  // Previewer only code  
}
```

::: zone pivot="win-dev16"

## <a name="enable-design-time-rendering-for-custom-controls"></a>カスタム コントロールのデザイン時のレンダリングを有効にします。

カスタム コントロールは、設計時のコントロールがプロジェクトに存在するまたはライブラリからインポートされるかどうかに関係なく、プレビューアーで表示される表示をオプトインする必要があります。 これは、追加することで実現できます、 [ `[DesignTimeVisible(true)]` ](xref:System.ComponentModel.DesignTimeVisibleAttribute)コントロールのクラスに。

```csharp
namespace MyProject
{
  [DesignTimeVisible(true)]
  public class MyControl : BaseControl
  {
    // Your control's code here
  }

}
```

この例で確認できます[ImageCirclePlugin James Montemagno 氏の基本クラス](https://github.com/jamesmontemagno/ImageCirclePlugin/blob/master/src/ImageCircle/CircleImage.shared.cs)します。

::: zone-end

## <a name="add-design-time-data"></a>デザイン時データを追加します。

一部のレイアウトは、ユーザーインターフェイスコントロールにバインドされたデータなしでは視覚化が難しい場合があります。 プレビューをより遣いやすくするには、バインディングコンテキストをハードコードすることによって、いくつかの静的データをコントロールに割り当てます。（コードビハインドまたはXAMLを使用）。

XAMLの静的なViewModelにバインドする方法については、James Montemagnoの[デザイン時のデータの追加に関するブログ記事](http://motzcod.es/post/143702671962/xamarinforms-xaml-previewer-design-time-data)を参照してください。

## <a name="troubleshooting"></a>トラブルシューティング

問題が発生する場合は以下の問題と[Xamarinフォーラム](https://forums.xamarin.com/categories/xamarin-forms)を確認してください。

### <a name="xaml-previewer-isnt-showing-or-shows-an-error"></a>XAML プレビューアーが表示されていない、またはエラーを示します

次の点をチェックしてください。

* デザイナー エージェントは、XAML ファイルをプレビューする最初の時間にセットアップに必要があります - これが準備できるまで、進行状況のメッセージと共に、プレビューアーで進行状況インジケーターが表示されます。
* 閉じてから再度 XAML ファイルを開き直します。
* いることを確認、`App`クラスにはパラメーターなしのコンス トラクター。

::: zone pivot="win-dev16"

### <a name="custom-controls-arent-rendering"></a>カスタム コントロールが表示されません。

プロジェクトが構築されていることを確認します。 プレビューアーことはできません、エラーなしのコントロールの表示、またはコントロールの作成者でしたないでは、デザイン時のレンダリング、プレビューアーはコントロールの基本クラスを示します。

::: zone-end
