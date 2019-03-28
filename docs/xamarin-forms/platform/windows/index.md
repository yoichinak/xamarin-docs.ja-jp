---
title: Windows プラットフォームの機能
description: この記事では、Xamarin.Forms で使用できる Windows プラットフォームのサポートについて説明します。
ms.prod: xamarin
ms.assetid: F6EA9E49-FB3E-442F-AF13-B7AD0C80D11F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/08/2018
---

# <a name="windows-platform-features"></a>Windows プラットフォームの機能

Windows プラットフォーム用の Xamarin.Forms アプリケーションの開発には、Visual Studio が必要です。 [要件ページ](~/get-started/requirements.md)の前提条件の詳細が含まれています。

![](images/allhanselman.png "Windows で実行される Xamarin.Forms アプリケーション")

## <a name="platform-specifics"></a>プラットフォーム固有設定

プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。

Xamarin.Forms のビュー、ページ、およびユニバーサル Windows プラットフォーム (UWP) でのレイアウトを次のプラットフォーム固有の機能が提供されます。

- アクセス キーの設定、 [ `VisualElement`](xref:Xamarin.Forms.VisualElement)します。 詳細については、次を参照してください。 [Windows のアクセス キーを VisualElement](visualelement-access-keys.md)します。
- サポートされている従来のカラー モードを無効にする[ `VisualElement`](xref:Xamarin.Forms.VisualElement)します。 詳細については、次を参照してください。 [VisualElement レガシ カラー モード Windows](legacy-color-mode.md)します。

UWP の Xamarin.Forms のビューでは、次のプラットフォームに固有の機能が提供されます。

- 検出、テキスト コンテンツからの読み取り順序[ `Entry` ](xref:Xamarin.Forms.Entry)、 [ `Editor` ](xref:Xamarin.Forms.Editor)、および[ `Label` ](xref:Xamarin.Forms.Label)インスタンス。 詳細については、次を参照してください。 [Windows 上の読み取り順序を InputView](inputview-reading-order.md)します。
- タップ ジェスチャのサポートを有効にすると、 [ `ListView`](xref:Xamarin.Forms.ListView)します。 詳細については、次を参照してください。 [Windows 上の ListView SelectionMode](listview-selectionmode.md)します。
- 有効にすると、 [ `SearchBar` ](xref:Xamarin.Forms.SearchBar)スペル チェック エンジンと対話します。 詳細については、次を参照してください。 [Windows で SearchBar Spell Check](searchbar-spell-check.md)します。
- 有効にすると、 [ `WebView` ](xref:Xamarin.Forms.WebView) UWP メッセージ ダイアログ ボックスで、JavaScript のアラートを表示します。 詳細については、次を参照してください。 [Windows 上の web ビュー JavaScript アラート](webview-javascript-alert.md)します。

UWP の Xamarin.Forms のページでは、次のプラットフォームに固有の機能が提供されます。

- 折りたたみ、 [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage)ナビゲーション バー。 詳細については、次を参照してください。 [Windows 上のナビゲーション バーの MasterDetailPage](masterdetailpage-navigation-bar.md)します。
- ツールバーの配置オプションを設定します。 詳細については、次を参照してください。 [Windows でページのツールバーの配置](page-toolbar-placement.md)します。
- 表示されるページのアイコンを有効にすると、 [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage)ツールバー。 詳細については、次を参照してください。 [Windows 上のアイコンを TabbedPage](tabbedpage-icons.md)します。

## <a name="platform-support"></a>プラットフォームのサポート

Visual Studio で使用可能な Xamarin.Forms テンプレートには、ユニバーサル Windows プラットフォーム (UWP) プロジェクトが含まれます。

> [!NOTE]
> Xamarin.Forms の 1.x と 2.x のサポート_Windows Phone 8 Silverlight_、 _Windows Phone 8.1_、および_Windows 8.1_アプリケーション開発。 ただし、これらのプロジェクト タイプが推奨されていません。

## <a name="getting-started"></a>作業の開始

移動して**ファイル > 新規 > プロジェクト**Visual Studio でのいずれかを選択し、**クロス プラットフォーム > 空のアプリ (Xamarin.Forms)** を開始するテンプレート。

Xamarin.Forms ソリューションの古い、または macos で作成されたものがない上記のすべての Windows プロジェクト (ただし、手動で追加する必要があります)。 Windows プラットフォームを対象にするは、ソリューションでは、アクセスにいない場合は、[セットアップ手順](installation/index.md)目的の Windows プロジェクトの種類/秒を追加します。

## <a name="samples"></a>サンプル

[すべてのサンプル](https://github.com/xamarin/xamarin-forms-book-preview-2)Charles Petzold の書籍の[*を Xamarin.Forms での Mobile Apps の作成*](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md) (Windows 10) 用のユニバーサル Windows プラットフォーム プロジェクトが含まれます。

["Scott Hanselman"デモ アプリ](https://github.com/jamesmontemagno/Hanselman.Forms)は個別に使用して、Apple Watch、Android Wear のプロジェクトも含まれます (Xamarin.iOS と Xamarin.Android のそれぞれを使用して、Xamarin.Forms で稼働していないこれらのプラットフォーム)。

## <a name="related-links"></a>関連リンク

- [セットアップの Windows プロジェクト](~/xamarin-forms/platform/windows/installation/index.md)
