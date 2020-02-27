---
title: Windows プラットフォームの機能
description: この記事では、Xamarin. Forms で使用できる Windows プラットフォームのサポートについて説明します。
ms.prod: xamarin
ms.assetid: F6EA9E49-FB3E-442F-AF13-B7AD0C80D11F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/16/2020
ms.openlocfilehash: 694ec24697937b114036eceab1cafd5aae3617d8
ms.sourcegitcommit: 10b4d7952d78f20f753372c53af6feb16918555c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/26/2020
ms.locfileid: "77635550"
---
# <a name="windows-platform-features"></a>Windows プラットフォームの機能

Windows プラットフォーム用の Xamarin. Forms アプリケーションを開発するには、Visual Studio が必要です。 [[サポートされているプラットフォーム] ページ](~/get-started/supported-platforms.md)には、前提条件に関する詳細情報が表示されます。

![](images/allhanselman.png "Xamarin.Forms Applications Running on Windows")

## <a name="platform-specifics"></a>プラットフォーム固有設定

プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。

次のプラットフォーム固有の機能は、ユニバーサル Windows プラットフォーム (UWP) の Xamarin ビュー、ページ、レイアウトに対して用意されています。

- [`VisualElement`](xref:Xamarin.Forms.VisualElement)のアクセスキーを設定します。 詳細については、「 [Windows の Visualelement アクセスキー](visualelement-access-keys.md)」を参照してください。
- サポートされている[`VisualElement`](xref:Xamarin.Forms.VisualElement)でレガシカラーモードを無効にする。 詳細については、「 [Windows の Visualelement レガシカラーモード](legacy-color-mode.md)」を参照してください。

UWP の Xamarin ビューでは、次のプラットフォーム固有の機能が用意されています。

- [`Entry`](xref:Xamarin.Forms.Entry)、 [`Editor`](xref:Xamarin.Forms.Editor)、および[`Label`](xref:Xamarin.Forms.Label)インスタンスのテキストコンテンツからの読み取り順序の検出。 詳細については、「 [Windows での Inputview の読み取り順序](inputview-reading-order.md)」を参照してください。
- [`ListView`](xref:Xamarin.Forms.ListView)で tap ジェスチャのサポートを有効にします。 詳細については、「 [ListView SelectionMode On Windows](listview-selectionmode.md)」を参照してください。
- `RefreshView` のプル方向の変更を有効にします。 詳細については、「 [Windows 上の Refreshview Pull Direction](refreshview-pulldirection.md)」を参照してください。
- [`SearchBar`](xref:Xamarin.Forms.SearchBar)がスペルチェックエンジンと対話できるようにします。 詳細については、「 [Windows での Searchbar Spell Check](searchbar-spell-check.md)」を参照してください。
- [`WebView`](xref:Xamarin.Forms.WebView)による UWP メッセージダイアログでの JavaScript 警告の表示を有効にする。 詳細については、「 [Windows での WebView JavaScript Alerts](webview-javascript-alert.md)」を参照してください。

UWP には、次のプラットフォーム固有の機能が用意されています。

- [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)ナビゲーションバーを折りたたみます。 詳細については、 [Windows の Master詳細ページナビゲーションバーに関するページ](masterdetailpage-navigation-bar.md)を参照してください。
- ツールバーの配置オプションを設定します。 詳細については、「 [Windows 上のページツールバーの配置](page-toolbar-placement.md)」を参照してください。
- [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)ツールバーにページアイコンを表示できるようにします。 詳細については、「[Windows 上の TabbedPage アイコン](tabbedpage-icons.md)」を参照してください。

UWP の Xamarin [`Application`](xref:Xamarin.Forms.Application)クラスには、次のプラットフォーム固有の機能が用意されています。

- イメージアセットの読み込み元となるプロジェクト内のディレクトリを指定します。 詳細については、「 [Windows の既定のイメージディレクトリ](default-image-directory.md)」を参照してください。

## <a name="platform-support"></a>プラットフォームのサポート

Visual Studio で使用可能な Xamarin. Forms テンプレートには、ユニバーサル Windows プラットフォーム (UWP) プロジェクトが含まれています。

> [!NOTE]
> Xamarin 1.x および2.x は_Windows Phone 8 Silverlight_、 _Windows Phone 8.1_、および_Windows 8.1_アプリケーション開発をサポートしています。 ただし、これらのプロジェクトの種類は非推奨とされます。

## <a name="getting-started"></a>作業の開始

Visual Studio で **[ファイル > 新しい > プロジェクト]** に移動し、 **[クロスプラットフォーム > 空のアプリ (Xamarin)]** テンプレートの1つを選択して開始します。

以前の Xamarin. Forms ソリューション、または macOS で作成されたソリューションには、上記のすべての Windows プロジェクトが含まれているわけではありません (ただし、手動で追加する必要があります)。 対象とする Windows プラットフォームがまだソリューションに含まれていない場合は、[セットアップの指示に従っ](installation/index.md)て、必要な windows プロジェクトの種類/s を追加します。

## <a name="samples"></a>Samples

Petzold 著)[*を使用して Mobile Apps を作成する*](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)チャールズの書籍のすべてのサンプルには[、](https://github.com/xamarin/xamarin-forms-book-preview-2)ユニバーサル Windows プラットフォーム (Windows 10) プロジェクトが含まれています。

["Scott マン Selman" デモアプリ](https://github.com/jamesmontemagno/Hanselman.Forms)は個別に利用できます。また、Apple Watch および Android の磨耗プロジェクトも含まれています (Xamarin. IOS と xamarin を使用して、Xamarin. Forms はこれらのプラットフォームでは実行されません)。

## <a name="related-links"></a>関連リンク

- [Windows プロジェクトのセットアップ](~/xamarin-forms/platform/windows/installation/index.md)
