---
title: "Xamarin.Forms の XAML プレビューアー"
description: "入力すると表示される Xamarin.Forms レイアウトを参照してください!"
ms.topic: article
ms.prod: xamarin
ms.assetid: 84769ff1-72fd-4c44-8251-dd6d5bf8c7b2
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 02/24/2017
ms.openlocfilehash: 8f4d8253d56708f77ede7b5173f3dd771e1da0ea
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="xaml-previewer-for-xamarinforms"></a>Xamarin.Forms の XAML プレビューアー

_入力すると表示される Xamarin.Forms レイアウトを参照してください!_

## <a name="requirements"></a>必要条件

プロジェクトでは、作業を XAML プレビュー用のプログラムの最新の Xamarin.Forms NuGet パッケージが必要です。 Android アプリをプレビューする必要があります[JDK 1.8 x64](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)です。

詳細については、[リリース ノート](https://developer.xamarin.com/releases/studio/xamarin.studio_6.2/xamarin.studio_6.2/#Xamarin_Forms_Previewer)です。

## <a name="getting-started"></a>作業の開始

### <a name="visual-studio-for-mac-on-mac"></a>Mac 上の Mac 用の visual Studio

**プレビュー**を XAML ファイルを右クリックし、エディターでボタンを表示できる**ファイルを開く > XAML ビューアー**です。 プレビュー ウィンドウは、表示またはキーを押して非表示、**プレビュー**任意の XAML ドキュメント ウィンドウの右上隅にあるボタンをクリックします。

[![ListView コントロールではプレビュー版 Visual Studio for Mac](xaml-previewer-images/xamlp-list-sml.png "Mac 用の Visual Studio でのフォーム プレビューアー")](xaml-previewer-images/xamlp-list.png#lightbox "Mac 用の Visual Studio でのフォーム プレビューアー")

### <a name="visual-studio-on-windows"></a>Windows の Visual Studio

使用して、**ビュー > その他のウィンドウ > Xamarin.Forms プレビューアー**プレビュー ウィンドウを開いて Visual Studio のメニュー。 使用して、**ウィンドウ > 垂直タブ グループ**メニューにサイド バイ サイド配置します。

[![Visual Studio での ListView コントロールのプレビュー](xaml-previewer-images/xamlp-list-vs-sml.png "Visual Studio でのフォーム プレビューアー")](xaml-previewer-images/xamlp-list-vs.png#lightbox "Visual Studio でのフォーム プレビューアー")

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

James Montemagno を参照してください[デザイン時のデータの追加に関するブログの投稿](http://motzcod.es/post/143702671962/xamarinforms-xaml-previewer-design-time-data)XAML で静的 ViewModel にバインドする方法を確認します。

## <a name="troubleshooting"></a>トラブルシューティング

以下の問題の確認と[Xamarin フォーラム](https://forums.xamarin.com/categories/xamarin-forms)問題が発生する場合は、します。

### <a name="xaml-preview-isnt-showing"></a>XAML のプレビューが表示されません。

次の点をチェックしてください。

* プロジェクトをビルドする (コンパイル) XAML ファイルをプレビューする前にします。
* デザイナーのエージェントは、XAML ファイルをプレビューしたときに、最初にセットアップに必要があります - これが準備できるまで、プレビュー用のプログラム、進行状況メッセージと共にに進行状況インジケーターが表示されます。
* Try 決算と XAML ファイルを開き直します。

### <a name="invalid-xaml-the-android-project-needs-to-built-before-preview-can-be-created"></a>無効な XAML: Android プロジェクトをプレビューを作成する前にビルドする必要があります。

XAML プレビュー用のプログラムでは、ページを表示する前に、プロジェクトをビルドすることが必要です。
プレビュー ウィンドウの上部にある次のエラーが表示された場合は、アプリケーションを再ビルドしてからやり直してください。

![エラー メッセージ: プロジェクトを最初にビルドされる必要があります](xaml-previewer-images/error-not-built-sml.png "エラー メッセージ: プロジェクトをリビルドします")
