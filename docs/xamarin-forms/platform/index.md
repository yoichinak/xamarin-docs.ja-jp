---
title: Xamarin.Forms プラットフォーム機能
description: このガイドでは、さまざまな手法を使用して Xamarin.Forms アプリケーション プラットフォームに固有の機能のメリットを利用する方法について説明します。
ms.prod: xamarin
ms.assetid: 2C6CE42C-E380-4BB9-90CC-D0F4E60C4C03
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2018
ms.openlocfilehash: 2e8eb19411799e7723be338e9e3f6df35058eb8c
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "35242801"
---
# <a name="xamarinforms-platform-features"></a>Xamarin.Forms プラットフォーム機能

Xamarin.Forms を拡張しを使用してプラットフォーム固有の機能を組み込むことができます[効果](~/xamarin-forms/app-fundamentals/effects/index.md)、[カスタム レンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)、 [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md)、 [MessagingCenter](~/xamarin-forms/app-fundamentals/messaging-center.md)などです。

## <a name="androidandroidindexmd"></a>[Android](android/index.md)

このガイドでは、既存の Xamarin.Forms Android アプリを更新することによってマテリアルのデザインを実装する方法について説明します。

## <a name="application-indexing-and-deep-linkingdeep-linkingmd"></a>[アプリケーション インデックス作成とディープ リンク作成](deep-linking.md)

いくつかを使用して検索結果に表示される関連を把握して後忘れてそれ以外の場合はアプリケーションをアプリケーションがインデックス作成できます。 ディープ リンクをディープ リンクから参照されているページに移動して、通常、アプリケーションのデータを含む検索結果に応答するアプリケーションはできます。

## <a name="device-classdevicemd"></a>[デバイス クラス](device.md)

使用する方法、`Device`共有コードと、ユーザー インターフェイス (XAML を使用してを含む) でプラットフォーム固有の動作を作成するクラス。 についても説明`BeginInvokeOnMainThread`バック グラウンド スレッドから UI コントロールを変更する場合にすることが不可欠です。

## <a name="iosiosindexmd"></a>[iOS](ios/index.md)

使用して一部の iOS のスタイル処理を実行できる**Info.plist**と`UIAppearance`API です。 このガイドでは、コア Spotlight 検索を含む Xamarin.Forms ソリューションの iOS アプリを iOS 9 の機能を組み込む方法の例を示します。

## <a name="gtkgtkmd"></a>[GTK](gtk.md)

Xamarin.Forms では、GTK # アプリ用のプレビューがサポートできるようになりました。

## <a name="macmacmd"></a>[Mac](mac.md)

Xamarin.Forms では、macOS アプリ用のプレビューがサポートできるようになりました。

## <a name="native-formsnative-formsmd"></a>[ネイティブ フォーム](native-forms.md)

ネイティブのフォームは、Xamarin.Forms を許可する[ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-ネイティブ Xamarin.iOS、Xamarin.Android、およびユニバーサル Windows プラットフォーム (UWP) プロジェクトで使用されるページを派生します。

## <a name="native-viewsnative-viewsindexmd"></a>[ネイティブ ビュー](native-views/index.md)

IOS、Android、およびユニバーサル Windows プラットフォームのネイティブのビューは、Xamarin.Forms から直接参照できます。 プロパティとイベント ハンドラーは、ネイティブのビューを設定でき、Xamarin.Forms のビューとやり取りすることができます。

## <a name="platform-specificsplatform-specificsindexmd"></a>[プラットフォーム固有](platform-specifics/index.md)

プラットフォーム固有では、カスタム レンダラーや特殊効果を必要とせず、特定のプラットフォームで利用可能なだけの機能を使用できます。

## <a name="pluginspluginsmd"></a>[プラグイン](plugins.md)

さまざまなオープン ソース プラグインは Xamarin.Forms アプリの拡張に役立つ、Github、Nuget、および Xamarin コンポーネント ストアで使用できます。

## <a name="tizentizenmd"></a>[Tizen](tizen.md)

Tizen .NET では、Xamarin.Forms と Tizen .NET framework での .NET アプリケーションをビルドすることができます。

## <a name="windowswindowsindexmd"></a>[Windows](windows/index.md)

Xamarin.Forms では、Windows 10 のユニバーサル Windows プラットフォーム (UWP) のサポートがあります。 この記事の内容を追加する方法を説明します、既存の Xamarin.Forms ソリューションを UWP プロジェクト。

## <a name="wpfwpfmd"></a>[WPF](wpf.md)

Xamarin.Forms では、Windows Presentation Foundation (WPF) アプリ用のプレビューがサポートできるようになりました。
