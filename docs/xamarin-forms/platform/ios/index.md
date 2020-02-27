---
title: Xamarin の iOS プラットフォーム機能
description: Xamarin. Forms アプリケーションに iOS 固有の機能を追加します。
ms.prod: xamarin
ms.assetid: 634AB62E-68C8-454C-838B-F1CC4E4E21BC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/15/2020
ms.openlocfilehash: 015db40ce983d979109d4cce32c011f8c61a729c
ms.sourcegitcommit: 10b4d7952d78f20f753372c53af6feb16918555c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/26/2020
ms.locfileid: "77635662"
---
# <a name="ios-platform-features-in-xamarinforms"></a>Xamarin の iOS プラットフォーム機能

IOS 用の Xamarin. Forms アプリケーションを開発するには、Visual Studio が必要です。 [[サポートされているプラットフォーム] ページ](~/get-started/supported-platforms.md)には、前提条件に関する詳細情報が表示されます。

## <a name="platform-specifics"></a>プラットフォーム固有設定

プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。

IOS では、次のプラットフォーム固有の機能が Xamarin のビュー、ページ、およびレイアウト用に用意されています。

- 任意の[`VisualElement`](xref:Xamarin.Forms.VisualElement)のぼかしサポート。 詳細については、「 [iOS での Visualelement のぼかし](visualelement-blur.md)」を参照してください。
- サポートされている[`VisualElement`](xref:Xamarin.Forms.VisualElement)でレガシカラーモードを無効にする。 詳細については、「 [Visualelement レガシカラーモード (iOS](legacy-color-mode.md))」を参照してください。
- [`VisualElement`](xref:Xamarin.Forms.VisualElement)でのドロップシャドウの有効化。 詳細については、「 [iOS の Visualelement ドロップシャドウ](visualelement-drop-shadow.md)」を参照してください。
- [`VisualElement`](xref:Xamarin.Forms.VisualElement)オブジェクトを有効にして、イベントをタッチする最初の応答側にします。 詳細については、「 [Visualelement First レスポンダー](visualelement-first-responder.md)」を参照してください。

IOS では、次のプラットフォーム固有の機能が Xamarin. Forms ビュー用に用意されています。

- [`Cell`](xref:Xamarin.Forms.Cell)の背景色を設定します。 詳細については、「 [iOS のセルの背景色](cell-background-color.md)」を参照してください。
- [`DatePicker`](xref:Xamarin.Forms.DatePicker)で項目選択が発生するタイミングを制御します。 詳細については、「 [DatePicker Item Selection On iOS](datepicker-selection.md)」を参照してください。
- フォントサイズを調整して、入力テキストが[`Entry`](xref:Xamarin.Forms.Entry)に収まることを確認します。 詳細については、 [iOS のエントリのフォントサイズ](entry-font-size.md)に関する記事をご覧ください。
- [`Entry`](xref:Xamarin.Forms.Entry)のカーソルの色を設定します。 詳細については、「 [iOS のエントリカーソルの色](entry-cursor-color.md)」を参照してください。
- スクロール中に[`ListView`](xref:Xamarin.Forms.ListView)ヘッダーセルをフローティングするかどうかを制御します。 詳細については、「 [iOS の ListView グループヘッダースタイル](listview-group-header-style.md)」を参照してください。
- [`ListView`](xref:Xamarin.Forms.ListView)項目のコレクションを更新するときに、行アニメーションを無効にするかどうかを制御します。 詳細については、「 [iOS の ListView 行アニメーション](listview-row-animations.md)」を参照してください。
- [`ListView`](xref:Xamarin.Forms.ListView)の区切り記号のスタイルを設定します。 詳細については、「 [iOS の ListView 区切り記号のスタイル](listview-separator-style.md)」を参照してください。
- [`Picker`](xref:Xamarin.Forms.Picker)で項目選択が発生するタイミングを制御します。 詳細については、「 [iOS でのピッカー項目の選択](picker-selection.md)」を参照してください。
- `Slider` thumb をドラッグするのではなく、 [`Slider`](xref:Xamarin.Forms.Slider)バー上の位置をタップして[`Slider.Value`](xref:Xamarin.Forms.Slider.Value)プロパティを有効にする。 詳細については、「 [iOS でのスライダー Thumb のタップ](slider-thumb.md)」を参照してください。
- `SwipeView`を開くときに使用される遷移を制御します。 詳細については、「 [SwipeView スワイプ切り替えモード](swipeview-swipetransitionmode.md)」を参照してください。
- [`TimePicker`](xref:Xamarin.Forms.TimePicker)で項目選択が発生するタイミングを制御します。 詳細については、「 [iOS での Timepicker 項目の選択](timepicker-selection.md)」を参照してください。

IOS では、次のプラットフォーム固有の機能が Xamarin. Forms ページ用に用意されています。

- [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)のナビゲーションバーの区切り記号を非表示にする。 詳細については、「 [iOS の Navigationpage バーの区切り記号](navigation-bar-separator.md)」を参照してください。
- ナビゲーション バーが半透明かどうかを制御します。 詳細については、「 [iOS でのナビゲーションバーの透明](navigation-bar-translucent.md)度」を参照してください。
- [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)のステータスバーのテキストの色を、ナビゲーションバーの明るさに合わせて調整するかどうかを制御します。 詳細については、「 [iOS の Navigationpage バーテキストカラーモード](status-bar-text-color.md)」を参照してください。
- ナビゲーションバーでページタイトルを大タイトルとして表示するかどうかを制御します。 詳細については、「 [iOS の大きなページタイトル](page-large-title.md)」を参照してください。
- [`Page`](xref:Xamarin.Forms.Page)でのホームインジケーターの表示を設定します。 詳細については、「 [iOS でのホームインジケーターの表示](page-home-indicator.md)」を参照してください。
- [`Page`](xref:Xamarin.Forms.Page)でステータスバーの表示を設定します。 詳細については、「 [iOS でのページステータスバーの表示](page-status-bar-visibility.md)」を参照してください。
- すべての iOS デバイスの安全である画面の領域には、そのページの内容の確認が配置されています。 詳細については、「 [iOS の安全な領域レイアウトガイド](page-safe-area-layout.md)」を参照してください。
- モーダルページのプレゼンテーションスタイルを設定します。 詳細については、「[モーダルページプレゼンテーションスタイル](page-presentation-style.md)」を参照してください。
- [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)のタブバーの透明度モードを設定します。 詳細については、「 [TabbedPage 半透明 TabBar On iOS](tabbedpage-translucent-tabbar.md)」を参照してください。

IOS では、次のプラットフォーム固有の機能が Xamarin. フォームレイアウト用に用意されています。

- [`ScrollView`](xref:Xamarin.Forms.ScrollView)がタッチジェスチャを処理するか、またはそのコンテンツに渡すかを制御します。 詳細については、「 [ScrollView Content see On iOS](scrollview-content-touches.md)」を参照してください。

IOS 上の Xamarin. Forms [`Application`](xref:Xamarin.Forms.Application)クラスには、次のプラットフォーム固有の機能が用意されています。

- 名前付きフォントサイズのアクセシビリティスケーリングを無効にします。 詳細については、「 [iOS での名前付きフォントサイズのアクセシビリティスケーリング](named-font-size-scaling.md)」を参照してください。
- コントロールのレイアウトを有効にして、メイン スレッドで実行される更新プログラムを表示します。 詳細については、「 [iOS でのメインスレッド制御の更新](main-thread-updates-ui.md)」を参照してください。
- スクロールビューで[`PanGestureRecognizer`](xref:Xamarin.Forms.PanGestureRecognizer)を有効にすると、スクロールビューでパンジェスチャをキャプチャして共有できます。 詳細については、「 [iOS でのパンジェスチャの同時認識](application-pan-gesture.md)」を参照してください。

## <a name="ios-specific-formatting"></a>iOS 固有の書式設定

Xamarin では、クロスプラットフォームのユーザーインターフェイスのスタイルと色を設定できますが、ios プロジェクトでプラットフォーム Api を使用して iOS のテーマを設定するためのオプションは他にもあります。

[詳細](formatting.md)については、「**情報 plist**構成」や「`UIAppearance` Api」など、iOS 固有の api を使用したユーザーインターフェイスの書式設定に関する説明を参照してください。

![](images/status-white-sml.png "iOS Theming")

## <a name="other-ios-features"></a>その他の iOS 機能

[カスタムレンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)、 [dependencyservice](~/xamarin-forms/app-fundamentals/dependency-service/index.md)、および[messagingcenter](~/xamarin-forms/app-fundamentals/messaging-center.md)を使用すると、さまざまなネイティブ機能を iOS 用の Xamarin. Forms アプリケーションに組み込むことができます。
