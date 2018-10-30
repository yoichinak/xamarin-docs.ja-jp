---
title: Xamarin.Forms 用 XAML プレビューアー
description: この記事では、XAML プレビューアーを使用してレンダリングを入力すると、Xamarin.Forms のレイアウトを表示する方法について説明します。 XAML プレビューアーは Visual Studio 2017 と Visual Studio for mac の両方で使用できます。
ms.prod: xamarin
ms.assetid: 84769ff1-72fd-4c44-8251-dd6d5bf8c7b2
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 05/31/2018
ms.openlocfilehash: def7a7a3bdd9e165252c5ad1928b89ec654e5d74
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50108087"
---
# <a name="xaml-previewer-for-xamarinforms"></a>Xamarin.Forms 用 XAML プレビューアー

_入力するたびに描画されるXamarin.Formsレイアウトを表示しましょう!_

## <a name="requirements"></a>必要条件

XAMLプレビューアーが動作するためにはプロジェクトに最新の Xamarin.Forms NuGetパッケージが必要です。 Androidアプリをプレビューするには[JDK 1.8 x64](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)が必要です。

[リリース ノート](https://developer.xamarin.com/releases/studio/xamarin.studio_6.2/xamarin.studio_6.2/#Xamarin_Forms_Previewer)により詳細な情報があります。

## <a name="getting-started"></a>作業の開始

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

XAML プレビューアーが既定でオンおよび管理することができます、**ツール > オプション > Xamarin > フォーム プレビューアー**ダイアログ。 このダイアログ ボックスでは、既定のドキュメント ビューと分割の向きを選択できます。

[![Visual Studio での ListView コントロールのプレビュー](xaml-previewer-images/xamlp-options-vs.png "Visual Studio でのフォームのプレビューアー オプション")](xaml-previewer-images/xamlp-options-vs.png#lightbox "Visual Studio でのフォームのプレビューアー オプション")

選択した設定に基づき、エディターは分割 XAML ページを開くときに、**ツール > オプション > Xamarin > フォーム プレビューアー**ダイアログ。 ただし、これらの設定は、エディター ウィンドウで変更できます。

## <a name="xaml-preview-controls"></a>XAML プレビュー コントロール

エディター ウィンドウの上部には、どのペインが使用して、デザイン ペインに切り替え、一番上のボタンと [ソース] ウィンドウへの切り替え、下部にボタンが選択するボタンがあります。 中央のボタンは、ペインの順序を入れ替えます。

[![Visual Studio での ListView コントロールのプレビュー](xaml-previewer-images/xamlp-controls-vs.png "Visual Studio でコントロールをフォーム プレビューアー ウィンドウ")](xaml-previewer-images/xamlp-controls-vs.png#lightbox "Visual Studio でコントロールをフォーム プレビューアー ウィンドウ")

エディター ウィンドウの下部には、垂直方向および水平方向の分割ペイン、および展開または現在のサブ ペインを折りたたむボタンがあります。

[![Visual Studio での ListView コントロールのプレビュー](xaml-previewer-images/xamlp-controls2-vs.png "Visual Studio でコントロールをフォーム プレビューアー ウィンドウ")](xaml-previewer-images/xamlp-controls2-vs.png#lightbox "Visual Studio でコントロールをフォーム プレビューアー ウィンドウ")

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

**プレビュー** XAML ページを開くと、エディター ボタンが表示されます。 プレビュー ウィンドウを表示または非表示にキーを押してことができます、**プレビュー**で任意の XAML ドキュメント ウィンドウの右上隅にあるボタンをクリックします。

[![Visual studio for Mac の ListView コントロール プレビュー](xaml-previewer-images/xamlp-list-sml.png "Visual Studio for Mac でのフォームのプレビューアー")](xaml-previewer-images/xamlp-list.png#lightbox "Visual Studio for Mac でのフォームのプレビューアー")

-----

## <a name="xaml-preview-options"></a>XAML プレビュー オプション

プレビュー ウィンドウの上部のオプションがあります。

* **Phone** – 電話サイズの画面に表示
* **タブレット**– タブレット サイズの画面にレンダリング (ウィンドウの右下にあるズーム コントロールがあるに注意してください)
* **Android** – 画面の Android バージョンを表示します。
* **iOS** – 画面の iOS バージョンを表示します。
* Portait (アイコン)-縦向きを使用して、プレビュー
* (アイコン) – ランドス ケープは横長の向きのプレビュー

## <a name="adding-design-time-data"></a>デザイン時データの追加

一部のレイアウトは、ユーザーインターフェイスコントロールにバインドされたデータなしでは視覚化が難しい場合があります。 プレビューをより遣いやすくするには、バインディングコンテキストをハードコードすることによって、いくつかの静的データをコントロールに割り当てます。（コードビハインドまたはXAMLを使用）。

XAMLの静的なViewModelにバインドする方法については、James Montemagnoの[デザイン時のデータの追加に関するブログ記事](http://motzcod.es/post/143702671962/xamarinforms-xaml-previewer-design-time-data)を参照してください。

## <a name="detecting-design-mode"></a>デザイン モードの検出

静的な[ `DesignMode.IsDesignModeEnabled` ](xref:Xamarin.Forms.DesignMode.IsDesignModeEnabled)プレビューアーで、アプリケーションが実行されているかどうかを決定するプロパティを調べることができます。 プレビューアーで、アプリケーションが実行されている場合にのみ実行するコードを指定できます。

```csharp
if (DesignMode.IsDesignModeEnabled)
{
  // Previewer only code  
}
```

## <a name="troubleshooting"></a>トラブルシューティング

問題が発生する場合は以下の問題と[Xamarinフォーラム](https://forums.xamarin.com/categories/xamarin-forms)を確認してください。

### <a name="xaml-preview-isnt-showing"></a>XAML プレビューが表示されません。

次の点をチェックしてください。

* プロジェクトをビルドする (コンパイル) XAML ファイルをプレビューする前にします。
* デザイナー エージェントは、XAML ファイルをプレビューする最初の時間にセットアップに必要があります - これが準備できるまで、進行状況のメッセージと共に、プレビューアーで進行状況インジケーターが表示されます。
* 閉じてから再度 XAML ファイルを開き直します。
* いることを確認、`App`クラスにはパラメーターなしのコンス トラクター。

### <a name="invalid-xaml-the-android-project-needs-to-built-before-preview-can-be-created"></a>無効な XAML: Android プロジェクトはプレビューを作成する前にビルドする必要があります。

XAML プレビューアーでは、ページを表示する前に、プロジェクトをビルドすることが必要です。
プレビュー ウィンドウの上部にある次のエラーが表示された場合は、アプリケーションを再構築してからやり直してください。

![エラー メッセージ: プロジェクトを最初にビルドされる必要があります](xaml-previewer-images/error-not-built-sml.png "エラー メッセージ: プロジェクトをリビルドします")
