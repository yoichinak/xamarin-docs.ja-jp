---
タイトル: "Windows プラットフォーム機能" の説明: "この記事では、で使用できる Windows プラットフォームのサポートについて説明 Xamarin.Forms します。"
ms. 製品: xamarin ms. assetid: F6EA9E49-FB3E-442F-AF13-B7AD0C80D11F: xamarin-forms author: davidbritch ms. author: dabritch ms. date: 01/16/2020 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="windows-platform-features"></a>Windows プラットフォームの機能

Xamarin.FormsWindows プラットフォーム用のアプリケーションを開発するには、Visual Studio が必要です。 [[サポートされているプラットフォーム] ページ](~/get-started/supported-platforms.md)には、前提条件に関する詳細情報が表示されます。

![](images/allhanselman.png "Xamarin.Forms Applications Running on Windows")

## <a name="platform-specifics"></a>プラットフォーム固有設定

プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。

Xamarin.Formsユニバーサル Windows プラットフォーム (UWP) のビュー、ページ、およびレイアウトには、次のプラットフォーム固有の機能が用意されています。

- のアクセスキーを設定 [`VisualElement`](xref:Xamarin.Forms.VisualElement) します。 詳細については、「 [Windows の Visualelement アクセスキー](visualelement-access-keys.md)」を参照してください。
- サポートされているでレガシカラーモードを無効にする [`VisualElement`](xref:Xamarin.Forms.VisualElement) 。 詳細については、「 [Windows の Visualelement レガシカラーモード](legacy-color-mode.md)」を参照してください。

UWP のビューには、次のプラットフォーム固有の機能が用意されてい Xamarin.Forms ます。

- [`Entry`](xref:Xamarin.Forms.Entry)、 [`Editor`](xref:Xamarin.Forms.Editor) 、およびインスタンスのテキストコンテンツからの読み取り順序の検出 [`Label`](xref:Xamarin.Forms.Label) 。 詳細については、「 [Windows での Inputview の読み取り順序](inputview-reading-order.md)」を参照してください。
- で tap ジェスチャのサポートを有効に [`ListView`](xref:Xamarin.Forms.ListView) します。 詳細については、「 [ListView SelectionMode On Windows](listview-selectionmode.md)」を参照してください。
- のプル方向の `RefreshView` 変更を有効にします。 詳細については、「 [Windows 上の Refreshview Pull Direction](refreshview-pulldirection.md)」を参照してください。
- が [`SearchBar`](xref:Xamarin.Forms.SearchBar) スペルチェックエンジンと対話できるようにします。 詳細については、「 [Windows での Searchbar Spell Check](searchbar-spell-check.md)」を参照してください。
- を有効にして、 [`WebView`](xref:Xamarin.Forms.WebView) UWP メッセージダイアログに JavaScript の警告を表示します。 詳細については、「 [Windows での WebView JavaScript Alerts](webview-javascript-alert.md)」を参照してください。

UWP のページには、次のプラットフォーム固有の機能が用意されてい Xamarin.Forms ます。

- [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)ナビゲーションバーを折りたたみます。 詳細については、 [Windows の Master詳細ページナビゲーションバーに関するページ](masterdetailpage-navigation-bar.md)を参照してください。
- ツールバーの配置オプションを設定します。 詳細については、「 [Windows 上のページツールバーの配置](page-toolbar-placement.md)」を参照してください。
- ツールバーにページアイコンを表示できるように [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) します。 詳細については、「[Windows 上の TabbedPage アイコン](tabbedpage-icons.md)」を参照してください。

UWP のクラスには、次のプラットフォーム固有の機能が用意されてい Xamarin.Forms [`Application`](xref:Xamarin.Forms.Application) ます。

- イメージアセットの読み込み元となるプロジェクト内のディレクトリを指定します。 詳細については、「 [Windows の既定のイメージディレクトリ](default-image-directory.md)」を参照してください。

## <a name="platform-support"></a>プラットフォームのサポート

Xamarin.FormsVisual Studio で使用できるテンプレートには、ユニバーサル Windows プラットフォーム (UWP) プロジェクトが含まれています。

> [!NOTE]
> Xamarin.Forms1.x と2.x のサポート_Windows Phone 8 Silverlight_、 _Windows Phone 8.1_、 _Windows 8.1_アプリケーション開発。 ただし、これらのプロジェクトの種類は非推奨とされます。

## <a name="getting-started"></a>作業の開始

Visual Studio で [**ファイル] > [新しい > プロジェクト**] の順に選択し、[**クロスプラットフォーム > 空のアプリ ( Xamarin.Forms )** ] テンプレートの1つを選択して開始します。

以前の Xamarin.Forms ソリューション、または macOS で作成されたソリューションには、上記のすべての Windows プロジェクトが含まれているわけではありません (ただし、手動で追加する必要があります)。 対象とする Windows プラットフォームがまだソリューションに含まれていない場合は、[セットアップの指示に従っ](installation/index.md)て、必要な windows プロジェクトの種類/s を追加します。

## <a name="samples"></a>サンプル

[*Mobile Apps を作成 Xamarin.Forms する*](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)チャールズ petzold 著) の書籍のすべてのサンプルには[、](https://github.com/xamarin/xamarin-forms-book-preview-2)ユニバーサル Windows プラットフォーム (Windows 10) プロジェクトが含まれています。

["Scott サバイバル selman" デモアプリ](https://github.com/jamesmontemagno/Hanselman.Forms)は個別に利用できます。また、Apple Watch および Android の磨耗プロジェクトも含まれています (Xamarin および xamarin android を使用して、 Xamarin.Forms これらのプラットフォームでは実行されません)。

## <a name="related-links"></a>関連リンク

- [Windows プロジェクトのセットアップ](~/xamarin-forms/platform/windows/installation/index.md)
