---
title: プラットフォーム機能
description: Xamarin.Forms を使用したプラットフォームに固有の機能を活用します。
ms.prod: xamarin
ms.assetid: 2C6CE42C-E380-4BB9-90CC-D0F4E60C4C03
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/20/2017
ms.openlocfilehash: 454969d8a59128423d632452033b8ba84ee0bc98
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="platform-features"></a>プラットフォーム機能

Xamarin.Forms を拡張しを使用してプラットフォーム固有の機能を組み込むことができます[効果](~/xamarin-forms/app-fundamentals/effects/index.md)、[カスタム レンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)、 [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md)、 [MessagingCenter](~/xamarin-forms/app-fundamentals/messaging-center.md)などです。

## <a name="androidandroidindexmd"></a>[Android](android/index.md)

このガイドでは、既存の Xamarin.Forms Android アプリを更新することによってマテリアルのデザインを実装する方法について説明します。

## <a name="application-indexing-and-deep-linkingdeep-linkingmd"></a>[アプリケーション インデックス作成とディープ リンク作成](deep-linking.md)

いくつかを使用して検索結果に表示される関連を把握して後忘れてそれ以外の場合はアプリケーションをアプリケーションがインデックス作成できます。 ディープ リンクをディープ リンクから参照されているページに移動して、通常、アプリケーションのデータを含む検索結果に応答するアプリケーションはできます。

## <a name="device-classdevicemd"></a>[デバイス クラス](device.md)

使用する方法、`Device`共有コードと、ユーザー インターフェイス (XAML を使用してを含む) でプラットフォーム固有の動作を作成するクラス。 についても説明`BeginInvokeOnMainThread`バック グラウンド スレッドから UI コントロールを変更する場合にすることが不可欠です。

## <a name="iosiosindexmd"></a>[iOS](ios/index.md)

使用して一部の iOS のスタイル処理を実行できる**Info.plist**と`UIAppearance`API です。 このガイドでは、コア Spotlight 検索を含む Xamarin.Forms ソリューションの iOS アプリを iOS 9 の機能を組み込む方法の例を示します。

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

## <a name="windowswindowsindexmd"></a>[Windows](windows/index.md)

Xamarin.Forms では、Windows プロジェクトの 4 つの異なる型のサポートがあります。

* Windows Phone 8 Silverlight (元 Windows によってサポートされたプラットフォーム Xamarin.Forms)、
* Windows Phone 8.1 (WinRT)
* Windows 8.1 (WinRT) と
* ユニバーサル Windows プラットフォーム (Windows 10)。

このセクションでは、既存の Xamarin.Forms ソリューションに追加する方法と、それらの違いについて説明します。
