---
title: Xamarin.Forms の XAML プレビューアー
description: この記事では、XAML プレビュー用のプログラムを使用して、Xamarin.Forms レイアウトを入力すると、レンダリング、表示する方法について説明します。 XAML プレビュー用のプログラムは Visual Studio 2017 と Visual Studio for mac の両方で使用できます。
ms.prod: xamarin
ms.assetid: 84769ff1-72fd-4c44-8251-dd6d5bf8c7b2
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 05/31/2018
ms.openlocfilehash: 25c8e1a34f8be5ab2f8491e75fa5aac470d55bc8
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245860"
---
# <a name="xaml-previewer-for-xamarinforms"></a>Xamarin.Forms の XAML プレビューアー

_入力するたびに描画されるXamarin.Formsレイアウトを表示しましょう!_

## <a name="requirements"></a>必要条件

XAMLプレビューアーが動作するためにはプロジェクトに最新の Xamarin.Forms NuGetパッケージが必要です。 Androidアプリをプレビューするには[JDK 1.8 x64](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)が必要です。

[リリース ノート](https://developer.xamarin.com/releases/studio/xamarin.studio_6.2/xamarin.studio_6.2/#Xamarin_Forms_Previewer)により詳細な情報があります。

## <a name="getting-started"></a>作業の開始

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

使用して、**ビュー > その他のウィンドウ > Xamarin.Forms プレビューアー**プレビュー ウィンドウを開いて Visual Studio のメニュー。 使用して、**ウィンドウ > 垂直タブ グループ**メニューにサイド バイ サイド配置します。

[![Visual Studio での ListView コントロールのプレビュー](xaml-previewer-images/xamlp-list-vs-sml.png "Visual Studio でのフォーム プレビューアー")](xaml-previewer-images/xamlp-list-vs.png#lightbox "Visual Studio でのフォーム プレビューアー")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

**プレビュー**を XAML ファイルを右クリックし、エディターでボタンを表示できる**ファイルを開く > フォーム プレビューアー**です。 プレビュー ウィンドウは、表示またはキーを押して非表示、**プレビュー**任意の XAML ドキュメント ウィンドウの右上隅にあるボタンをクリックします。

[![ListView コントロールではプレビュー版 Visual Studio for Mac](xaml-previewer-images/xamlp-list-sml.png "Mac 用の Visual Studio でのフォーム プレビューアー")](xaml-previewer-images/xamlp-list.png#lightbox "Mac 用の Visual Studio でのフォーム プレビューアー")

-----

## <a name="xaml-preview-options"></a>XAML プレビュー オプション

プレビュー ウィンドウの上部にあるオプションは次のとおりです。

* **Phone** – phone サイズ画面での表示
* **タブレット**– サイズ タブレットの画面で表示 (ウィンドウの右下にあるズーム コントロールがある注意してください)
* **Android** – 画面の Android バージョンを表示します。
* **iOS** – iOS バージョン、画面の表示
* Portait (アイコン): プレビューを使用して縦向き
* 横 (アイコン) – プレビューの使用が横向き

## <a name="adding-design-time-data"></a>デザイン時のデータを追加します。

一部のレイアウトは、ユーザー インターフェイス コントロールにバインドされているデータを含まない視覚化が難しい可能性があります。 ようにするプレビューでは、役に立つ、静的データの一部に割り当てますコントロール ハードコーディングするによって、バインディング コンテキスト (分離コードまたは XAML を使用していずれか)。

XAMLの静的なViewModelにバインドする方法については、James Montemagnoの[デザイン時のデータの追加に関するブログ記事](http://motzcod.es/post/143702671962/xamarinforms-xaml-previewer-design-time-data)を参照してください。

## <a name="detecting-design-mode"></a>デザイン モードの検出

静的な[ `DesignMode.IsDesignModeEnabled` ](xref:Xamarin.Forms.DesignMode.IsDesignModeEnabled)プレビュー用のプログラムで、アプリケーションが実行されているかどうかを決定するプロパティを調べることができます。 プレビュー用のプログラムで、アプリケーションが実行されている場合にのみ実行されるコードを指定できます。

```csharp
if (DesignMode.IsDesignModeEnabled)
{
  // Previewer only code  
}
```

## <a name="troubleshooting"></a>トラブルシューティング

問題が発生する場合は以下の問題と[Xamarinフォーラム](https://forums.xamarin.com/categories/xamarin-forms)を確認してください。

### <a name="xaml-preview-isnt-showing"></a>XAML のプレビューが表示されません。

次の点をチェックしてください。

* プロジェクトをビルドする (コンパイル) XAML ファイルをプレビューする前にします。
* デザイナーのエージェントは、XAML ファイルをプレビューしたときに、最初にセットアップに必要があります - これが準備できるまで、プレビュー用のプログラム、進行状況メッセージと共にに進行状況インジケーターが表示されます。
* Try 決算と XAML ファイルを開き直します。

### <a name="invalid-xaml-the-android-project-needs-to-built-before-preview-can-be-created"></a>無効な XAML: Android プロジェクトをプレビューを作成する前にビルドする必要があります。

XAML プレビュー用のプログラムでは、ページを表示する前に、プロジェクトをビルドすることが必要です。
プレビュー ウィンドウの上部にある次のエラーが表示された場合は、アプリケーションを再ビルドしてからやり直してください。

![エラー メッセージ: プロジェクトを最初にビルドされる必要があります](xaml-previewer-images/error-not-built-sml.png "エラー メッセージ: プロジェクトをリビルドします")
