---
title: Xamarin の iOS プラットフォーム機能
description: Xamarin. Forms アプリケーションに iOS 固有の機能を追加します。
ms.prod: xamarin
ms.assetid: 634AB62E-68C8-454C-838B-F1CC4E4E21BC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/22/2019
ms.openlocfilehash: c90cfc297914b585403ae84e7dbac11fd6e02836
ms.sourcegitcommit: eb23b7d745d1090376f9def07e0f11cb089494d0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/09/2019
ms.locfileid: "72170942"
---
# <a name="ios-platform-features-in-xamarinforms"></a>Xamarin の iOS プラットフォーム機能

IOS 用の Xamarin. Forms アプリケーションを開発するには、Visual Studio が必要です。 [[要件] ページ](~/get-started/requirements.md)には、前提条件に関する詳細情報が表示されます。

## <a name="platform-specifics"></a>プラットフォームの詳細

プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。

IOS では、次のプラットフォーム固有の機能が Xamarin のビュー、ページ、およびレイアウト用に用意されています。

- [ `VisualElement`](xref:Xamarin.Forms.VisualElement)へに対するぼかしのサポート。 詳細については、「 [iOS での Visualelement のぼかし](visualelement-blur.md)」を参照してください。
- サポートされている従来のカラー モードを無効にする[ `VisualElement`](xref:Xamarin.Forms.VisualElement)します。 詳細については、「 [Visualelement レガシカラーモード (iOS](legacy-color-mode.md))」を参照してください。
- ドロップ シャドウを有効にすると、 [ `VisualElement`](xref:Xamarin.Forms.VisualElement)します。 詳細については、「 [iOS の Visualelement ドロップシャドウ](visualelement-drop-shadow.md)」を参照してください。

IOS では、次のプラットフォーム固有の機能が Xamarin. Forms ビュー用に用意されています。

- [`Cell`](xref:Xamarin.Forms.Cell)の背景色を設定します。 詳細については、「 [iOS のセルの背景色](cell-background-color.md)」を参照してください。
- フォントサイズを調整することで入力した文字が [`Entry`](xref:Xamarin.Forms.Entry)内に収まるようにします。 詳細については、 [iOS のエントリのフォントサイズ](entry-font-size.md)に関する記事をご覧ください。
- カーソルの色の設定で、 [ `Entry`](xref:Xamarin.Forms.Entry)します。 詳細については、「 [iOS のエントリカーソルの色](entry-cursor-color.md)」を参照してください。
- スクロール中に[`ListView`](xref:Xamarin.Forms.ListView)ヘッダーセルをフローティングするかどうかを制御します。 詳細については、「 [iOS の ListView グループヘッダースタイル](listview-group-header-style.md)」を参照してください。
- [`ListView`](xref:Xamarin.Forms.ListView)項目のコレクションを更新するときに、行アニメーションを無効にするかどうかを制御します。 詳細については、「 [iOS の ListView 行アニメーション](listview-row-animations.md)」を参照してください。
- 区切り記号のスタイルを設定、 [ `ListView`](xref:Xamarin.Forms.ListView)します。 詳細については、「 [iOS の ListView 区切り記号のスタイル](listview-separator-style.md)」を参照してください。
- [`Picker`](xref:Xamarin.Forms.Picker)でアイテムの選択が発生するタイミングを制御します。 詳細については、「 [iOS でのピッカー項目の選択](picker-selection.md)」を参照してください。
- 有効にすると、 [ `Slider.Value` ](xref:Xamarin.Forms.Slider.Value)の位置をタップして設定されるプロパティを[ `Slider` ](xref:Xamarin.Forms.Slider)をドラッグすることではなく、横棒グラフ、`Slider`つまみ。 詳細については、「 [iOS でのスライダー Thumb のタップ](slider-thumb.md)」を参照してください。

IOS では、次のプラットフォーム固有の機能が Xamarin. Forms ページ用に用意されています。

- ナビゲーション バーの区切り記号を非表示を[ `NavigationPage`](xref:Xamarin.Forms.NavigationPage)します。 詳細については、「 [iOS の Navigationpage バーの区切り記号](navigation-bar-separator.md)」を参照してください。
- ナビゲーション バーが半透明かどうかを制御します。 詳細については、「 [iOS でのナビゲーションバーの透明](navigation-bar-translucent.md)度」を参照してください。
- [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)上のステータスバーのテキストの色をナビゲーションバーの明るさに合わせて調整するかどうかを制御します。 詳細については、「 [iOS の Navigationpage バーテキストカラーモード](status-bar-text-color.md)」を参照してください。
- ナビゲーションバーでページタイトルを大タイトルとして表示するかどうかを制御します。 詳細については、「 [iOS の大きなページタイトル](page-large-title.md)」を参照してください。
- [`Page`](xref:Xamarin.Forms.Page)でのホームインジケーターの表示を設定します。 詳細については、「 [iOS でのホームインジケーターの表示](page-home-indicator.md)」を参照してください。
- [`Page`](xref:Xamarin.Forms.Page) のステータス バーの可視性を設定します。 詳細については、「 [iOS でのページステータスバーの表示](page-status-bar-visibility.md)」を参照してください。
- すべての iOS デバイスの安全である画面の領域には、そのページの内容の確認が配置されています。 詳細については、「 [iOS の安全な領域レイアウトガイド](page-safe-area-layout.md)」を参照してください。
- モーダルページのプレゼンテーションスタイルを設定します。 詳細については、「[モーダルページプレゼンテーションスタイル](page-presentation-style.md)」を参照してください。

IOS では、次のプラットフォーム固有の機能が Xamarin. フォームレイアウト用に用意されています。

- [`ScrollView`](xref:Xamarin.Forms.ScrollView)がタッチ ジェスチャを処理するか、そのコンテンツに渡すかを制御します。 詳細については、「 [ScrollView Content see On iOS](scrollview-content-touches.md)」を参照してください。

IOS 上の Xamarin. Forms [`Application`](xref:Xamarin.Forms.Application)クラスには、次のプラットフォーム固有の機能が用意されています。

- 名前付きフォントサイズのアクセシビリティスケーリングを無効にします。 詳細については、「 [iOS での名前付きフォントサイズのアクセシビリティスケーリング](named-font-size-scaling.md)」を参照してください。
- コントロールのレイアウトを有効にして、メイン スレッドで実行される更新プログラムを表示します。 詳細については、「 [iOS でのメインスレッド制御の更新](main-thread-updates-ui.md)」を参照してください。
- 有効にすると、 [ `PanGestureRecognizer` ](xref:Xamarin.Forms.PanGestureRecognizer)でスクロールして表示をキャプチャし、スクロール ビューとパン ジェスチャを共有します。 詳細については、「 [iOS でのパンジェスチャの同時認識](application-pan-gesture.md)」を参照してください。

## <a name="ios-specific-formatting"></a>iOS 固有の書式設定

Xamarin では、クロスプラットフォームのユーザーインターフェイスのスタイルと色を設定できますが、ios プロジェクトでプラットフォーム Api を使用して iOS のテーマを設定するためのオプションは他にもあります。

[詳細](formatting.md)については、「**情報 plist**構成」や「`UIAppearance` Api」など、iOS 固有の api を使用したユーザーインターフェイスの書式設定に関する説明を参照してください。

![](images/status-white-sml.png "iOS のテーマ")

## <a name="other-ios-features"></a>その他の iOS 機能

[カスタムレンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)、 [dependencyservice](~/xamarin-forms/app-fundamentals/dependency-service/index.md)、および[messagingcenter](~/xamarin-forms/app-fundamentals/messaging-center.md)を使用すると、さまざまなネイティブ機能を iOS 用の Xamarin. Forms アプリケーションに組み込むことができます。
