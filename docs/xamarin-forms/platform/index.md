---
title: Xamarin.Forms のプラットフォーム機能
description: このガイドでは、さまざまな手法を使用して Xamarin.Forms アプリケーションでプラットフォーム固有の機能を活用する方法について説明します。
ms.prod: xamarin
ms.assetid: 2C6CE42C-E380-4BB9-90CC-D0F4E60C4C03
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2018
ms.openlocfilehash: 9bac53f71178ac321dea162d346295556a8f7adb
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998756"
---
# <a name="xamarinforms-platform-features"></a>Xamarin.Forms のプラットフォーム機能

Xamarin.Forms は拡張可能なとを使用してプラットフォーム固有の機能を組み込むことができます[効果](~/xamarin-forms/app-fundamentals/effects/index.md)、[カスタム レンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)、 [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md)、 [MessagingCenter](~/xamarin-forms/app-fundamentals/messaging-center.md)など。

## <a name="androidandroidindexmd"></a>[Android](android/index.md)

このガイドでは、既存の Xamarin.Forms の Android アプリを更新してマテリアル デザインを実装する方法について説明します。

## <a name="application-indexing-and-deep-linkingdeep-linkingmd"></a>[アプリケーション インデックス作成とディープ リンク作成](deep-linking.md)

アプリケーションがインデックス作成により、いくつかを使用して、検索結果に表示して、最新の後、忘れてそれ以外の場合はアプリケーションです。 ディープ リンクから参照されているページに移動して、通常、アプリケーションのデータを含む検索結果に応答するアプリケーションは、ディープ リンクできます。

## <a name="device-classdevicemd"></a>[デバイス クラス](device.md)

使用する方法、`Device`共有コードとユーザー インターフェイス (XAML を使用してを含む) でプラットフォーム固有の動作を作成するクラス。 についても説明します`BeginInvokeOnMainThread`バック グラウンド スレッドから UI コントロールを変更する場合に、このことが不可欠です。

## <a name="iosiosindexmd"></a>[iOS](ios/index.md)

一部の iOS のスタイルを使用して実行できる**Info.plist**と`UIAppearance`API。 このガイドでは、コア スポット ライト検索含む Xamarin.Forms ソリューションの iOS アプリに iOS 9 の機能を含める方法の例を示します。

## <a name="gtkgtkmd"></a>[GTK](gtk.md)

Xamarin.Forms では、GTK # アプリのプレビューをサポートできるようになりました。

## <a name="macmacmd"></a>[Mac](mac.md)

Xamarin.Forms では、macOS アプリのプレビューをサポートできるようになりました。

## <a name="native-formsnative-formsmd"></a>[ネイティブ フォーム](native-forms.md)

ネイティブ フォームは、Xamarin.Forms を使用する[ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-ネイティブ Xamarin.iOS、Xamarin.Android、およびユニバーサル Windows プラットフォーム (UWP) プロジェクトで使用するページを派生します。

## <a name="native-viewsnative-viewsindexmd"></a>[ネイティブ ビュー](native-views/index.md)

IOS、Android、およびユニバーサル Windows プラットフォームからのネイティブ ビューは、Xamarin.Forms から直接参照できます。 プロパティとイベント ハンドラーは、ネイティブのビューを設定でき、Xamarin.Forms のビューとやり取りすることができます。

## <a name="platform-specificsplatform-specificsindexmd"></a>[プラットフォーム固有設定](platform-specifics/index.md)

プラットフォーム固有設定を使用すると、カスタム レンダラーや特殊効果を必要とせず、特定のプラットフォームで利用可能なのみの機能を使用できます。

## <a name="pluginspluginsmd"></a>[プラグイン](plugins.md)

Xamarin.Forms アプリを拡張するには、Github、Nuget、および Xamarin コンポーネント ストアで使用できるさまざまなオープン ソース プラグインされます。

## <a name="tizentizenmd"></a>[Tizen](tizen.md)

Tizen .NET では、Xamarin.Forms と Tizen .NET framework での .NET アプリケーションをビルドすることができます。

## <a name="windowswindowsindexmd"></a>[Windows](windows/index.md)

Xamarin.Forms では、Windows 10 ユニバーサル Windows プラットフォーム (UWP) をサポートしています。 この記事は、追加する方法を説明します、既存の Xamarin.Forms ソリューションに UWP プロジェクト。

## <a name="wpfwpfmd"></a>[WPF](wpf.md)

Xamarin.Forms では、Windows Presentation Foundation (WPF) アプリのプレビューをサポートできるようになりました。
