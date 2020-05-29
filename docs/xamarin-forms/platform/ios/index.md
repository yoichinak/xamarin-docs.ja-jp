---
title: の iOS プラットフォーム機能Xamarin.Forms
description: IOS 固有の機能をアプリケーションに追加する Xamarin.Forms 。
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 1008eab6e56be7a235498e01ffd3ea1b27d2bbae
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2020
ms.locfileid: "84130170"
---
# <a name="ios-platform-features-in-xamarinforms"></a>の iOS プラットフォーム機能Xamarin.Forms

Xamarin.FormsIOS 用アプリケーションの開発には、Visual Studio が必要です。 [[サポートされているプラットフォーム] ページ](~/get-started/supported-platforms.md)には、前提条件に関する詳細情報が表示されます。

## <a name="platform-specifics"></a>プラットフォーム固有設定

プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。

Xamarin.FormsIOS のビュー、ページ、およびレイアウトには、次のプラットフォーム固有の機能が用意されています。

- どのような場合でもぼかしがサポートさ [`VisualElement`](xref:Xamarin.Forms.VisualElement) れます。 詳細については、「 [iOS での Visualelement のぼかし](visualelement-blur.md)」を参照してください。
- サポートされているでレガシカラーモードを無効にする [`VisualElement`](xref:Xamarin.Forms.VisualElement) 。 詳細については、「 [Visualelement レガシカラーモード (iOS](legacy-color-mode.md))」を参照してください。
- でドロップシャドウを有効にする [`VisualElement`](xref:Xamarin.Forms.VisualElement) 。 詳細については、「 [iOS の Visualelement ドロップシャドウ](visualelement-drop-shadow.md)」を参照してください。
- オブジェクトを有効にし [`VisualElement`](xref:Xamarin.Forms.VisualElement) て、イベントをタッチする最初の応答側にします。 詳細については、「 [Visualelement First レスポンダー](visualelement-first-responder.md)」を参照してください。

IOS のビューには、次のプラットフォーム固有の機能が用意されてい Xamarin.Forms ます。

- [`Cell`](xref:Xamarin.Forms.Cell)背景色を設定します。 詳細については、「 [iOS のセルの背景色](cell-background-color.md)」を参照してください。
- で項目の選択が発生するタイミングを制御 [`DatePicker`](xref:Xamarin.Forms.DatePicker) します。 詳細については、「 [DatePicker Item Selection On iOS](datepicker-selection.md)」を参照してください。
- フォントサイズを調整して、入力テキストがに収まることを確認 [`Entry`](xref:Xamarin.Forms.Entry) します。 詳細については、 [iOS のエントリのフォントサイズ](entry-font-size.md)に関する記事をご覧ください。
- でカーソルの色を設定 [`Entry`](xref:Xamarin.Forms.Entry) します。 詳細については、「 [iOS のエントリカーソルの色](entry-cursor-color.md)」を参照してください。
- [`ListView`](xref:Xamarin.Forms.ListView)スクロール中にヘッダーセルをフローティングするかどうかを制御します。 詳細については、「 [iOS の ListView グループヘッダースタイル](listview-group-header-style.md)」を参照してください。
- 項目コレクションが更新されるときに行アニメーションを無効にするかどうかを制御 [`ListView`](xref:Xamarin.Forms.ListView) します。 詳細については、「 [iOS の ListView 行アニメーション](listview-row-animations.md)」を参照してください。
- で区切り記号のスタイルを設定 [`ListView`](xref:Xamarin.Forms.ListView) します。 詳細については、「 [iOS の ListView 区切り記号のスタイル](listview-separator-style.md)」を参照してください。
- で項目の選択が発生するタイミングを制御 [`Picker`](xref:Xamarin.Forms.Picker) します。 詳細については、「 [iOS でのピッカー項目の選択](picker-selection.md)」を参照してください。
- に背景があるかどうかを制御 [`SearchBar`](xref:Xamarin.Forms.SearchBar) します。 詳細については、「 [iOS の Searchbar スタイル](searchbar-style.md)」を参照してください。
- つまみを [`Slider.Value`](xref:Xamarin.Forms.Slider.Value) [`Slider`](xref:Xamarin.Forms.Slider) ドラッグするのではなく、バー上の位置をタップしてプロパティを設定できるようにし `Slider` ます。 詳細については、「 [iOS でのスライダー Thumb のタップ](slider-thumb.md)」を参照してください。
- を開くときに使用される遷移を制御し `SwipeView` ます。 詳細については、「 [SwipeView スワイプ切り替えモード](swipeview-swipetransitionmode.md)」を参照してください。
- で項目の選択が発生するタイミングを制御 [`TimePicker`](xref:Xamarin.Forms.TimePicker) します。 詳細については、「 [iOS での Timepicker 項目の選択](timepicker-selection.md)」を参照してください。

IOS のページには、次のプラットフォーム固有の機能が用意されてい Xamarin.Forms ます。

- マスターページを公開するときに、の詳細ページ [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) に影が適用されているかどうかを制御します。 詳細については、「 [Masterのページシャドウ](masterdetailpage-shadow.md)」を参照してください。
- でナビゲーションバーの区切り記号を非表示にする [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 。 詳細については、「 [iOS の Navigationpage バーの区切り記号](navigation-bar-separator.md)」を参照してください。
- ナビゲーションバーを半透明にするかどうかを制御します。 詳細については、「 [iOS でのナビゲーションバーの透明](navigation-bar-translucent.md)度」を参照してください。
- ナビゲーションバーの明るさに合わせて、のステータスバーのテキストの色を調整するかどうか [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) を制御します。 詳細については、「 [iOS の Navigationpage バーテキストカラーモード](status-bar-text-color.md)」を参照してください。
- ページナビゲーションバーにページタイトルを大きなタイトルとして表示するかどうかを制御します。 詳細については、「 [iOS の大きなページタイトル](page-large-title.md)」を参照してください。
- でのホームインジケーターの表示を設定し [`Page`](xref:Xamarin.Forms.Page) ます。 詳細については、「 [iOS でのホームインジケーターの表示](page-home-indicator.md)」を参照してください。
- でステータスバーの可視性を設定し [`Page`](xref:Xamarin.Forms.Page) ます。 詳細については、「 [iOS でのページステータスバーの表示](page-status-bar-visibility.md)」を参照してください。
- すべての iOS デバイスに安全なページコンテンツが画面上に配置されていることを確認します。 詳細については、「 [iOS の安全な領域レイアウトガイド](page-safe-area-layout.md)」を参照してください。
- モーダルページのプレゼンテーションスタイルを設定します。 詳細については、「[モーダルページプレゼンテーションスタイル](page-presentation-style.md)」を参照してください。
- のタブバーの透明度モードを設定 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) します。 詳細については、「 [TabbedPage 半透明 TabBar On iOS](tabbedpage-translucent-tabbar.md)」を参照してください。

IOS のレイアウトには、次のプラットフォーム固有の機能が用意されてい Xamarin.Forms ます。

- が [`ScrollView`](xref:Xamarin.Forms.ScrollView) タッチジェスチャを処理するか、またはそのコンテンツに渡すかを制御します。 詳細については、「 [ScrollView Content see On iOS](scrollview-content-touches.md)」を参照してください。

IOS のクラスには、次のプラットフォーム固有の機能が用意されてい Xamarin.Forms [`Application`](xref:Xamarin.Forms.Application) ます。

- 名前付きフォントサイズのアクセシビリティスケーリングを無効にします。 詳細については、「 [iOS での名前付きフォントサイズのアクセシビリティスケーリング](named-font-size-scaling.md)」を参照してください。
- メインスレッドでコントロールレイアウトとレンダリング更新を実行できるようにします。 詳細については、「 [iOS でのメインスレッド制御の更新](main-thread-updates-ui.md)」を参照してください。
- [`PanGestureRecognizer`](xref:Xamarin.Forms.PanGestureRecognizer)スクロールビューでを有効にすると、スクロールビューでパンジェスチャをキャプチャして共有できます。 詳細については、「 [iOS でのパンジェスチャの同時認識](application-pan-gesture.md)」を参照してください。

## <a name="ios-specific-formatting"></a>iOS 固有の書式設定

Xamarin.Formsクロスプラットフォームのユーザーインターフェイスのスタイルと色を設定できますが、ios プロジェクトでプラットフォーム Api を使用して iOS のテーマを設定するためのオプションが他にもあります。

[詳細](formatting.md)については、「**情報 plist**構成」や「Api」など、iOS 固有の api を使用したユーザーインターフェイスの書式設定に関する説明を参照して `UIAppearance` ください。

![](images/status-white-sml.png "iOS Theming")

## <a name="other-ios-features"></a>その他の iOS 機能

[カスタムレンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)、 [dependencyservice](~/xamarin-forms/app-fundamentals/dependency-service/index.md)、および[messagingcenter](~/xamarin-forms/app-fundamentals/messaging-center.md)を使用すると、さまざまなネイティブ機能を Xamarin.Forms iOS 用のアプリケーションに組み込むことができます。
