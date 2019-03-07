---
title: iOS プラットフォーム機能
description: Xamarin.Forms アプリケーションには、iOS 固有の機能を追加します。
ms.prod: xamarin
ms.assetid: 634AB62E-68C8-454C-838B-F1CC4E4E21BC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/22/2019
---

# <a name="ios-platform-features"></a>iOS プラットフォーム機能

IOS 用の Xamarin.Forms アプリケーションの開発には、Visual Studio が必要です。 [要件ページ](~/get-started/requirements.md)の前提条件の詳細が含まれています。

## <a name="platform-specifics"></a>プラットフォーム固有設定

プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。

Xamarin.Forms のビュー、ページ、および iOS でのレイアウトを次のプラットフォーム固有の機能が提供されます。

- [ `VisualElement`](xref:Xamarin.Forms.VisualElement)へに対するぼかしのサポート。 詳細については、次を参照してください。 [ios VisualElement ぼかし](visualelement-blur.md)します。
- サポートされている従来のカラー モードを無効にする[ `VisualElement`](xref:Xamarin.Forms.VisualElement)します。 詳細については、次を参照してください。 [ios VisualElement レガシ カラー モード](legacy-color-mode.md)します。
- ドロップ シャドウを有効にすると、 [ `VisualElement`](xref:Xamarin.Forms.VisualElement)します。 詳細については、次を参照してください。 [iOS に VisualElement ドロップ シャドウ](visualelement-drop-shadow.md)します。

IOS で Xamarin.Forms のビューでは、次のプラットフォームに固有の機能が提供されます。

- 設定、 [ `Cell` ](xref:Xamarin.Forms.Cell)背景色。 詳細については、次を参照してください。 [iOS でのセルの背景色](cell-background-color.md)します。
- フォントサイズを調整することで入力した文字が [`Entry`](xref:Xamarin.Forms.Entry)内に収まるようにします。 詳細については、次を参照してください。 [iOS でのエントリのフォント サイズ](entry-font-size.md)します。
- カーソルの色の設定で、 [ `Entry`](xref:Xamarin.Forms.Entry)します。 詳細については、次を参照してください。[エントリ iOS でのカーソルの色](entry-cursor-color.md)します。
- 制御するかどうか[ `ListView` ](xref:Xamarin.Forms.ListView)ヘッダー セルをスクロール中には float です。 詳細については、次を参照してください。 [iOS で ListView グループ ヘッダーのスタイル](listview-group-header-style.md)します。
- 行のアニメーションが無効になっているかどうかを制御するときに、 [ `ListView` ](xref:Xamarin.Forms.ListView)項目のコレクションが更新されます。 詳細については、次を参照してください。 [iOS で ListView 行アニメーション](listview-row-animations.md)します。
- 区切り記号のスタイルを設定、 [ `ListView`](xref:Xamarin.Forms.ListView)します。 詳細については、次を参照してください。 [iOS での ListView の区切り記号のスタイル](listview-separator-style.md)します。
- [`Picker`](xref:Xamarin.Forms.Picker)でアイテムの選択が発生するタイミングを制御します。 詳細については、次を参照してください。 [iOS での選択項目の選択](picker-selection.md)します。
- 有効にすると、 [ `Slider.Value` ](xref:Xamarin.Forms.Slider.Value)の位置をタップして設定されるプロパティを[ `Slider` ](xref:Xamarin.Forms.Slider)をドラッグすることではなく、横棒グラフ、`Slider`つまみ。 詳細については、次を参照してください。[スライダー Thumb が iOS でタップ](slider-thumb.md)します。

IOS 上の Xamarin.Forms ページの次のプラットフォーム固有の機能が提供されます。

- ナビゲーション バーの区切り記号を非表示を[ `NavigationPage`](xref:Xamarin.Forms.NavigationPage)します。 詳細については、次を参照してください。 [ios NavigationPage バーの区切り記号](navigation-bar-separator.md)します。
- ナビゲーション バーが半透明かどうかを制御します。 詳細については、次を参照してください。 [iOS でのナビゲーション バーの透明度](navigation-bar-translucent.md)します。
- [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)上のステータスバーのテキストの色をナビゲーションバーの明るさに合わせて調整するかどうかを制御します。 詳細については、次を参照してください。 [iOS で、バーのテキスト色 NavigationPage モード](status-bar-text-color.md)します。
- ナビゲーションバーでページタイトルを大タイトルとして表示するかどうかを制御します。 詳細については、次を参照してください。 [iOS での大きいページ タイトル](page-large-title.md)します。
- [`Page`](xref:Xamarin.Forms.Page) のステータス バーの可視性を設定します。 詳細については、次を参照してください。 [iOS でのステータス バーの可視性をページ](page-status-bar-visibility.md)します。
- すべての iOS デバイスの安全である画面の領域には、そのページの内容の確認が配置されています。 詳細については、次を参照してください。 [iOS での安全領域レイアウト ガイド](page-safe-area-layout.md)します。
- IPad でのモーダル ページ表示スタイルを設定します。 詳細については、次を参照してください。 [iPad モーダル ページ表示スタイル](ipad-page-presentation-style.md)します。

IOS で Xamarin.Forms のレイアウトでは、次のプラットフォームに固有の機能が提供されます。

- [`ScrollView`](xref:Xamarin.Forms.ScrollView)がタッチ ジェスチャを処理するか、そのコンテンツに渡すかを制御します。 詳細については、次を参照してください。 [ios ScrollView コンテンツ仕上げ](scrollview-content-touches.md)します。

Xamarin.Forms のプラットフォーム固有の次の機能が提供される[ `Application` ](xref:Xamarin.Forms.Application) iOS 上のクラス。

- コントロールのレイアウトを有効にして、メイン スレッドで実行される更新プログラムを表示します。 詳細については、次を参照してください。 [iOS でのメイン スレッド コントロール更新](main-thread-updates-ui.md)します。
- 有効にすると、 [ `PanGestureRecognizer` ](xref:Xamarin.Forms.PanGestureRecognizer)でスクロールして表示をキャプチャし、スクロール ビューとパン ジェスチャを共有します。 詳細については、次を参照してください。 [iOS での同時パン ジェスチャ認識](application-pan-gesture.md)します。

## <a name="ios-specific-formatting"></a>iOS 固有の書式設定

Xamarin.Forms により、クロス プラットフォームのユーザー インターフェイスのスタイルと色を設定するの iOS プロジェクトでは、プラットフォーム Api を使用して、iOS のテーマを設定するためには、その他のオプションがあります。

[詳細](formatting.md)などの iOS 固有の Api を使用してユーザー インターフェイスの書式設定に関する**Info.plist**構成と`UIAppearance`API。

![](images/status-white-sml.png "iOS のテーマ")

## <a name="other-ios-features"></a>その他の iOS の機能

使用して[カスタム レンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)、 [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md)、および[MessagingCenter](~/xamarin-forms/app-fundamentals/messaging-center.md)さまざまなネイティブの機能を組み込むことが可能になりますIOS 用の Xamarin.Forms アプリケーション。
