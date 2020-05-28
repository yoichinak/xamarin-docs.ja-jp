---
title: ''
description: この記事では、Android 固有の機能をアプリケーションに追加する方法について説明し Xamarin.Forms ます。
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 3045db1248aa16529d4e43b9a8afc97377cfd9cb
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2020
ms.locfileid: "84128955"
---
# <a name="android-platform-features"></a>Android プラットフォーム機能

Xamarin.FormsAndroid 用アプリケーションの開発には、Visual Studio が必要です。 [[サポートされているプラットフォーム] ページ](~/get-started/supported-platforms.md)には、前提条件に関する詳細情報が表示されます。

## <a name="platform-specifics"></a>プラットフォーム固有設定

プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。

Xamarin.FormsAndroid のビュー、ページ、およびレイアウトには、次のプラットフォーム固有の機能が用意されています。

- 描画順序を決定するために、ビジュアル要素の Z オーダーを制御します。 詳細については、「 [Android での Visualelement の昇格](visualelement-elevation.md)」を参照してください。
- サポートされているでレガシカラーモードを無効にする [`VisualElement`](xref:Xamarin.Forms.VisualElement) 。 詳細については、「 [Android での Visualelement のレガシカラーモード](legacy-color-mode.md)」を参照してください。

Android のビューには、次のプラットフォーム固有の機能が用意されてい Xamarin.Forms ます。

- Android ボタンの既定の埋め込み値とシャドウ値を使用します。 詳細については、「 [Android でのボタンの余白と影](button-padding-shadow.md)」を参照してください。
- のソフトキーボードの Input Method Editor オプションを設定 [`Entry`](xref:Xamarin.Forms.Entry) します。 詳細については、「 [Android の入力方式エディターオプション](entry-ime-options.md)」を参照してください。
- でドロップシャドウを有効にする `ImageButton` 。 詳細については、「 [Android での ImageButton ドロップシャドウ](imagebutton-drop-shadow.md)」を参照してください。
- での高速スクロールを有効にする [`ListView`](xref:Xamarin.Forms.ListView) 詳細については、「 [ListView fast スクロール (Android](listview-fast-scrolling.md))」を参照してください。
- を開くときに使用される遷移を制御し `SwipeView` ます。 詳細については、「 [SwipeView スワイプ切り替えモード](swipeview-swipetransitionmode.md)」を参照してください。
- で混合コンテンツを表示できるかどうかを制御 [`WebView`](xref:Xamarin.Forms.WebView) します。 詳細については、「 [Android での WebView 混合コンテンツ](webview-mixed-content.md)」を参照してください。
- でズームを有効にする [`WebView`](xref:Xamarin.Forms.WebView) 。 詳細については、「 [WebView Zoom On Android](webview-zoom-controls.md)」を参照してください。

Android のセルには、次のプラットフォーム固有の機能が用意されてい Xamarin.Forms ます。

- [`ViewCell`](xref:Xamarin.Forms.ViewCell)コンテキストアクションのレガシモードを有効にし、で選択した項目が変更されたときにコンテキストアクションメニューが更新されないようにし [`ListView`](xref:Xamarin.Forms.ListView) ます。 詳細については、「 [Viewcell コンテキストアクション (Android](viewcell-context-actions.md))」を参照してください。

Android のページには、次のプラットフォーム固有の機能が用意されてい Xamarin.Forms ます。

- のナビゲーションバーの高さを設定 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) します。 詳細については、「 [Android での Navigationpage バーの高さ](navigationpage-bar-height.md)」を参照してください。
- 内のページ間を移動するときに切り替えアニメーションを無効にする [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) 。 詳細については、「 [Android での TabbedPage ページ切り替えアニメーション](tabbedpage-transition-animations.md)」を参照してください。
- 内のページ間をスワイプできるように [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) します。 詳細については、「 [TabbedPage Page スワイプ On Android](tabbedpage-page-swiping.md)」を参照してください。
- のツールバーの配置と色を設定し [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) ます。 詳細については、「 [TabbedPage Toolbar Placement And Color On Android](tabbedpage-toolbar-placement-color.md)」を参照してください。

Android のクラスには、次のプラットフォーム固有の機能が用意されてい Xamarin.Forms [`Application`](xref:Xamarin.Forms.Application) ます。

- ソフトキーボードの動作モードを設定します。 詳細については、「 [Android でのソフトキーボード入力モード](soft-keyboard-input-mode.md)」を参照してください。
- AppCompat を [`Disappearing`](xref:Xamarin.Forms.Page.Appearing) [`Appearing`](xref:Xamarin.Forms.Page.Appearing) 使用するアプリケーションの場合、それぞれ一時停止とページのライフサイクルイベントを一時停止および再開します。 詳細については、「 [Android でのページライフサイクルイベント](page-lifecycle-events.md)」を参照してください。

## <a name="platform-support"></a>プラットフォームのサポート

当初、既定の Xamarin.Forms android プロジェクトでは、android 5.0 より前に共通であった古いスタイルのコントロールレンダリングが使用されていました。 テンプレートを使用してビルドされたアプリケーションは、 `FormsApplicationActivity` メインアクティビティの基本クラスとして機能します。

## <a name="material-design-via-appcompat"></a>AppCompat によるマテリアル設計

Xamarin.FormsAndroid プロジェクトは、 `FormsAppCompatActivity` メインアクティビティの基本クラスとしてを使用するようになりました。 このクラスは、Android によって提供される**AppCompat**機能を使用して、マテリアルデザインテーマを実装します。

素材デザインのテーマを Android プロジェクトに追加するには Xamarin.Forms 、 [AppCompat サポートのインストール手順](appcompat-material-design.md)に従ってください。

既定値を使用した**Todo**サンプルを次に示し `FormsApplicationActivity` ます。

[![](images/before-appcompat-sml.png "Todo Sample Application Without AppCompat")](images/before-appcompat.png#lightbox "Todo Sample Application Without AppCompat")

これは、を使用するためにプロジェクトをアップグレードした後のコードと同じです `FormsAppCompatActivity` (追加のテーマ情報を追加します)。

[![](images/post-appcompat-sml.png "Todo Sample Application With AppCompat and Theming")](images/post-appcompat.png#lightbox "Todo Sample Application With AppCompat and Theming")

> [!NOTE]
> を使用する場合 `FormsAppCompatActivity` 、[一部の Android カスタムレンダラーの基本クラス](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)は異なります。

## <a name="androidx-migration"></a>AndroidX への移行

AndroidX は、Android サポートライブラリを置き換えます。 AndroidX と、AndroidX ライブラリを使用するようにアプリを移行する方法の詳細について Xamarin.Forms は、「 [」の「androidx Xamarin.Forms の移行](~/xamarin-forms/platform/android/androidx-migration.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [マテリアルデザインサポートの追加](appcompat-material-design.md)
