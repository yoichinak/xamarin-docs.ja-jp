---
title: Xamarin の iOS プラットフォーム機能
description: Xamarin. Forms アプリケーションに iOS 固有の機能を追加します。
ms.prod: xamarin
ms.assetid: 634AB62E-68C8-454C-838B-F1CC4E4E21BC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/05/2020
ms.openlocfilehash: 97b9b70a1df61beb7b064c99721b4277e2570b41
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/29/2020
ms.locfileid: "82517053"
---
# <a name="ios-platform-features-in-xamarinforms"></a>Xamarin の iOS プラットフォーム機能

IOS 用の Xamarin. Forms アプリケーションを開発するには、Visual Studio が必要です。 [[サポートされているプラットフォーム] ページ](~/get-started/supported-platforms.md)には、前提条件に関する詳細情報が表示されます。

## <a name="platform-specifics"></a>プラットフォーム固有設定

プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。

IOS では、次のプラットフォーム固有の機能が Xamarin のビュー、ページ、およびレイアウト用に用意されています。

- どのような場合[`VisualElement`](xref:Xamarin.Forms.VisualElement)でもぼかしがサポートされます。 詳細については、「 [iOS での Visualelement のぼかし](visualelement-blur.md)」を参照してください。
- サポートされている[`VisualElement`](xref:Xamarin.Forms.VisualElement)でレガシカラーモードを無効にする。 詳細については、「 [Visualelement レガシカラーモード (iOS](legacy-color-mode.md))」を参照してください。
- でドロップシャドウを有効に[`VisualElement`](xref:Xamarin.Forms.VisualElement)する。 詳細については、「 [iOS の Visualelement ドロップシャドウ](visualelement-drop-shadow.md)」を参照してください。
- [`VisualElement`](xref:Xamarin.Forms.VisualElement)オブジェクトを有効にして、イベントをタッチする最初の応答側にします。 詳細については、「 [Visualelement First レスポンダー](visualelement-first-responder.md)」を参照してください。

IOS では、次のプラットフォーム固有の機能が Xamarin. Forms ビュー用に用意されています。

- 背景色[`Cell`](xref:Xamarin.Forms.Cell)を設定します。 詳細については、「 [iOS のセルの背景色](cell-background-color.md)」を参照してください。
- で項目の選択が発生する[`DatePicker`](xref:Xamarin.Forms.DatePicker)タイミングを制御します。 詳細については、「 [DatePicker Item Selection On iOS](datepicker-selection.md)」を参照してください。
- フォントサイズを調整して、 [`Entry`](xref:Xamarin.Forms.Entry)入力テキストがに収まることを確認します。 詳細については、 [iOS のエントリのフォントサイズ](entry-font-size.md)に関する記事をご覧ください。
- でカーソルの色を設定[`Entry`](xref:Xamarin.Forms.Entry)します。 詳細については、「 [iOS のエントリカーソルの色](entry-cursor-color.md)」を参照してください。
- スクロール中[`ListView`](xref:Xamarin.Forms.ListView)にヘッダーセルをフローティングするかどうかを制御します。 詳細については、「 [iOS の ListView グループヘッダースタイル](listview-group-header-style.md)」を参照してください。
- [`ListView`](xref:Xamarin.Forms.ListView)項目コレクションが更新されるときに行アニメーションを無効にするかどうかを制御します。 詳細については、「 [iOS の ListView 行アニメーション](listview-row-animations.md)」を参照してください。
- で区切り記号のスタイルを[`ListView`](xref:Xamarin.Forms.ListView)設定します。 詳細については、「 [iOS の ListView 区切り記号のスタイル](listview-separator-style.md)」を参照してください。
- で項目の選択が発生する[`Picker`](xref:Xamarin.Forms.Picker)タイミングを制御します。 詳細については、「 [iOS でのピッカー項目の選択](picker-selection.md)」を参照してください。
- に[`SearchBar`](xref:Xamarin.Forms.SearchBar)背景があるかどうかを制御します。 詳細については、「 [iOS の Searchbar スタイル](searchbar-style.md)」を参照してください。
- つまみを[`Slider.Value`](xref:Xamarin.Forms.Slider.Value)ドラッグするのではなく、 [`Slider`](xref:Xamarin.Forms.Slider)バー上の位置をタップしてプロパティを設定できるようにします。 `Slider` 詳細については、「 [iOS でのスライダー Thumb のタップ](slider-thumb.md)」を参照してください。
- を`SwipeView`開くときに使用される遷移を制御します。 詳細については、「 [SwipeView スワイプ切り替えモード](swipeview-swipetransitionmode.md)」を参照してください。
- で項目の選択が発生する[`TimePicker`](xref:Xamarin.Forms.TimePicker)タイミングを制御します。 詳細については、「 [iOS での Timepicker 項目の選択](timepicker-selection.md)」を参照してください。

IOS では、次のプラットフォーム固有の機能が Xamarin. Forms ページ用に用意されています。

- マスターページを公開するときに[`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) 、の詳細ページに影が適用されているかどうかを制御します。 詳細については、「 [Masterのページシャドウ](masterdetailpage-shadow.md)」を参照してください。
- でナビゲーションバーの区切り記号を[`NavigationPage`](xref:Xamarin.Forms.NavigationPage)非表示にする。 詳細については、「 [iOS の Navigationpage バーの区切り記号](navigation-bar-separator.md)」を参照してください。
- ナビゲーションバーを半透明にするかどうかを制御します。 詳細については、「 [iOS でのナビゲーションバーの透明](navigation-bar-translucent.md)度」を参照してください。
- ナビゲーションバーの明るさに合わせて、の[`NavigationPage`](xref:Xamarin.Forms.NavigationPage)ステータスバーのテキストの色を調整するかどうかを制御します。 詳細については、「 [iOS の Navigationpage バーテキストカラーモード](status-bar-text-color.md)」を参照してください。
- ページナビゲーションバーにページタイトルを大きなタイトルとして表示するかどうかを制御します。 詳細については、「 [iOS の大きなページタイトル](page-large-title.md)」を参照してください。
- でのホームインジケーターの表示を設定し[`Page`](xref:Xamarin.Forms.Page)ます。 詳細については、「 [iOS でのホームインジケーターの表示](page-home-indicator.md)」を参照してください。
- でステータスバーの可視性を[`Page`](xref:Xamarin.Forms.Page)設定します。 詳細については、「 [iOS でのページステータスバーの表示](page-status-bar-visibility.md)」を参照してください。
- すべての iOS デバイスに安全なページコンテンツが画面上に配置されていることを確認します。 詳細については、「 [iOS の安全な領域レイアウトガイド](page-safe-area-layout.md)」を参照してください。
- モーダルページのプレゼンテーションスタイルを設定します。 詳細については、「[モーダルページプレゼンテーションスタイル](page-presentation-style.md)」を参照してください。
- のタブバーの透明度モードを設定[`TabbedPage`](xref:Xamarin.Forms.TabbedPage)します。 詳細については、「 [TabbedPage 半透明 TabBar On iOS](tabbedpage-translucent-tabbar.md)」を参照してください。

IOS では、次のプラットフォーム固有の機能が Xamarin. フォームレイアウト用に用意されています。

- がタッチジェスチャ[`ScrollView`](xref:Xamarin.Forms.ScrollView)を処理するか、またはそのコンテンツに渡すかを制御します。 詳細については、「 [ScrollView Content see On iOS](scrollview-content-touches.md)」を参照してください。

IOS の Xamarin. Forms [`Application`](xref:Xamarin.Forms.Application)クラスには、次のプラットフォーム固有の機能が用意されています。

- 名前付きフォントサイズのアクセシビリティスケーリングを無効にします。 詳細については、「 [iOS での名前付きフォントサイズのアクセシビリティスケーリング](named-font-size-scaling.md)」を参照してください。
- メインスレッドでコントロールレイアウトとレンダリング更新を実行できるようにします。 詳細については、「 [iOS でのメインスレッド制御の更新](main-thread-updates-ui.md)」を参照してください。
- スクロールビュー [`PanGestureRecognizer`](xref:Xamarin.Forms.PanGestureRecognizer)でを有効にすると、スクロールビューでパンジェスチャをキャプチャして共有できます。 詳細については、「 [iOS でのパンジェスチャの同時認識](application-pan-gesture.md)」を参照してください。

## <a name="ios-specific-formatting"></a>iOS 固有の書式設定

Xamarin では、クロスプラットフォームのユーザーインターフェイスのスタイルと色を設定できますが、ios プロジェクトでプラットフォーム Api を使用して iOS のテーマを設定するためのオプションは他にもあります。

[詳細](formatting.md)については、「**情報 plist**構成」や「 `UIAppearance` api」など、iOS 固有の api を使用したユーザーインターフェイスの書式設定に関する説明を参照してください。

![](images/status-white-sml.png "iOS Theming")

## <a name="other-ios-features"></a>その他の iOS 機能

[カスタムレンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)、 [dependencyservice](~/xamarin-forms/app-fundamentals/dependency-service/index.md)、および[messagingcenter](~/xamarin-forms/app-fundamentals/messaging-center.md)を使用すると、さまざまなネイティブ機能を iOS 用の Xamarin. Forms アプリケーションに組み込むことができます。
