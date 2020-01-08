---
title: Android プラットフォーム機能
description: この記事では、Android 固有の機能を Xamarin Forms アプリケーションに追加する方法について説明します。
ms.prod: xamarin
ms.assetid: E24168F3-0138-4814-86EA-B467F6B8A545
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/11/2019
ms.openlocfilehash: 94523bb019e366738de65ce0b05c70264fce738b
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75489766"
---
# <a name="android-platform-features"></a>Android プラットフォーム機能

Android 用の Xamarin. Forms アプリケーションを開発するには、Visual Studio が必要です。 [[要件] ページ](~/get-started/requirements.md)には、前提条件に関する詳細情報が表示されます。

## <a name="platform-specifics"></a>プラットフォームの詳細

Platform-specifics は custom renderers や effects を実装することなく、特定のプラットフォーム上でのみ利用できる機能の使用を可能にします。

Android では、次のプラットフォーム固有の機能が Xamarin のビュー、ページ、およびレイアウト用に用意されています。

- 描画の順番を決定するための視覚要素の Z オーダーの制御。 詳細については、「 [Android での Visualelement の昇格](visualelement-elevation.md)」を参照してください。
- サポートされている従来のカラー モードを無効にする[ `VisualElement`](xref:Xamarin.Forms.VisualElement)します。 詳細については、「 [Android での Visualelement のレガシカラーモード](legacy-color-mode.md)」を参照してください。

Android の Xamarin ビューでは、次のプラットフォーム固有の機能が用意されています。

- 既定のパディングと Android のボタンの影の値を使用します。 詳細については、「 [Android でのボタンの余白と影](button-padding-shadow.md)」を参照してください。
- 入力方式のソフト キーボードのエディター オプションの設定、 [ `Entry`](xref:Xamarin.Forms.Entry)します。 詳細については、「 [Android の入力方式エディターオプション](entry-ime-options.md)」を参照してください。
- ドロップ シャドウを有効にすると、`ImageButton`します。 詳細については、「 [Android での ImageButton ドロップシャドウ](imagebutton-drop-shadow.md)」を参照してください。
- [`ListView`](xref:Xamarin.Forms.ListView)での高速スクロールを有効にする詳細については、「 [ListView Fast スクロール (Android](listview-fast-scrolling.md))」を参照してください。
- `SwipeView`を開くときに使用される遷移を制御します。 詳細については、「 [SwipeView スワイプ切り替えモード](swipeview-swipetransitionmode.md)」を参照してください。
- 制御するかどうかを[ `WebView` ](xref:Xamarin.Forms.WebView)混合コンテンツを表示できます。 詳細については、「 [Android での WebView 混合コンテンツ](webview-mixed-content.md)」を参照してください。
- [`WebView`](xref:Xamarin.Forms.WebView)のズームを有効にする。 詳細については、「 [WebView Zoom On Android](webview-zoom-controls.md)」を参照してください。

Android 上の Xamarin. フォームセルには、次のプラットフォーム固有の機能が用意されています。

- [`ListView`](xref:Xamarin.Forms.ListView)内で選択された項目が変更されたときにコンテキストアクションメニューが更新されないように[`ViewCell`](xref:Xamarin.Forms.ViewCell)コンテキストアクションのレガシモードを有効にします。 詳細については、「 [Viewcell コンテキストアクション (Android](viewcell-context-actions.md))」を参照してください。

Android では、次のプラットフォーム固有の機能が Xamarin. Forms ページ用に用意されています。

- 上のナビゲーション バーの高さを設定、 [ `NavigationPage`](xref:Xamarin.Forms.NavigationPage)します。 詳細については、「 [Android での Navigationpage バーの高さ](navigationpage-bar-height.md)」を参照してください。
- 内のページ間を移動するときに、遷移アニメーションを無効にすると、 [ `TabbedPage`](xref:Xamarin.Forms.TabbedPage)します。 詳細については、「 [Android での TabbedPage ページ切り替えアニメーション](tabbedpage-transition-animations.md)」を参照してください。
- [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)でのページ間のスワイプ操作の有効化。 詳細については、「 [TabbedPage Page スワイプ On Android](tabbedpage-page-swiping.md)」を参照してください。
- ツールバーの配置と色を設定、 [ `TabbedPage`](xref:Xamarin.Forms.TabbedPage)します。 詳細については、「 [TabbedPage Toolbar Placement And Color On Android](tabbedpage-toolbar-placement-color.md)」を参照してください。

Android 上の Xamarin. Forms [`Application`](xref:Xamarin.Forms.Application)クラスには、次のプラットフォーム固有の機能が用意されています。

- ソフトキーボードの操作モードの設定。 詳細については、「 [Android でのソフトキーボード入力モード](soft-keyboard-input-mode.md)」を参照してください。
- AppCompat を使用するアプリケーションに対し、一時停止や再開時に [`Disappearing`](xref:Xamarin.Forms.Page.Appearing)や[`Appearing`](xref:Xamarin.Forms.Page.Appearing) といったぺージのライフサイクルのイベントをそれぞれ無効化する。 詳細については、「 [Android でのページライフサイクルイベント](page-lifecycle-events.md)」を参照してください。

## <a name="platform-support"></a>プラットフォームのサポート

もともと、既定の Xamarin. Forms Android プロジェクトでは、Android 5.0 より前に共通だった従来のコントロールレンダリングが使用されていました。 テンプレートを使用してビルドされたアプリケーションは、メインアクティビティの基本クラスとして `FormsApplicationActivity` ます。

## <a name="material-design-via-appcompat"></a>AppCompat によるマテリアル設計

Xamarin. Forms Android プロジェクトでは、メインアクティビティの基本クラスとして `FormsAppCompatActivity` が使用されるようになりました。 このクラスは、Android によって提供される**AppCompat**機能を使用して、マテリアルデザインテーマを実装します。

マテリアルデザインのテーマを Xamarin. Forms Android プロジェクトに追加するには、 [AppCompat サポートのインストール手順](appcompat-material-design.md)に従ってください。

次に示すのは、既定の `FormsApplicationActivity`を使用した**Todo**サンプルです。

[![](images/before-appcompat-sml.png "Todo Sample Application Without AppCompat")](images/before-appcompat.png#lightbox "Todo Sample Application Without AppCompat")

これは、`FormsAppCompatActivity` を使用するためにプロジェクトをアップグレードした後のコードと同じです (追加のテーマ情報を追加します)。

[![](images/post-appcompat-sml.png "Todo Sample Application With AppCompat and Theming")](images/post-appcompat.png#lightbox "Todo Sample Application With AppCompat and Theming")

> [!NOTE]
> `FormsAppCompatActivity`を使用する場合、[一部の Android カスタムレンダラーの基本クラス](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)は異なります。

## <a name="related-links"></a>関連リンク

- [マテリアルデザインサポートの追加](appcompat-material-design.md)
