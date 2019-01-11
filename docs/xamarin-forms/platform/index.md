---
title: Xamarin.Forms のプラットフォーム機能
description: このガイドでは、さまざまな手法を使用して Xamarin.Forms アプリケーションでプラットフォーム固有の機能を活用する方法について説明します。
ms.prod: xamarin
ms.assetid: 2C6CE42C-E380-4BB9-90CC-D0F4E60C4C03
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/08/2018
ms.openlocfilehash: 3f0156926f8d7a31e2e80318d7b05a909f158653
ms.sourcegitcommit: 395774577f7524b57035c5cca3c9034a4b636489
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/10/2019
ms.locfileid: "54207727"
---
# <a name="xamarinforms-platform-features"></a>Xamarin.Forms のプラットフォーム機能

Xamarin.Forms は拡張可能なとを使用してプラットフォーム固有の機能を組み込むことができます[効果](~/xamarin-forms/app-fundamentals/effects/index.md)、[カスタム レンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)、 [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md)、 [MessagingCenter](~/xamarin-forms/app-fundamentals/messaging-center.md)など。

## <a name="androidandroidindexmd"></a>[Android](android/index.md)

このガイドでは、Xamarin.Forms、および既存の Xamarin.Forms の Android アプリを更新してマテリアル デザインを実装する方法によって提供される Android プラットフォームの詳細について説明します。

## <a name="device-classdevicemd"></a>[デバイス クラス](device.md)

使用する方法、`Device`共有コードとユーザー インターフェイス (XAML を使用してを含む) でプラットフォーム固有の動作を作成するクラス。 についても説明します`BeginInvokeOnMainThread`バック グラウンド スレッドから UI コントロールを変更する場合に、このことが不可欠です。

## <a name="iosiosindexmd"></a>[iOS](ios/index.md)

このガイドは、Xamarin.Forms、およびスタイルを使用して追加の iOS を実行する方法によって提供される iOS プラットフォームの詳細を記述します。 **Info.plist**と`UIAppearance`API。

## <a name="native-formsnative-formsmd"></a>[ネイティブ フォーム](native-forms.md)

ネイティブ フォームは、Xamarin.Forms を使用する[ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-ネイティブ Xamarin.iOS、Xamarin.Android、およびユニバーサル Windows プラットフォーム (UWP) プロジェクトで使用するページを派生します。

## <a name="native-viewsnative-viewsindexmd"></a>[ネイティブ ビュー](native-views/index.md)

IOS、Android、およびユニバーサル Windows プラットフォームからのネイティブ ビューは、Xamarin.Forms から直接参照できます。 プロパティとイベント ハンドラーは、ネイティブのビューを設定でき、Xamarin.Forms のビューとやり取りすることができます。

## <a name="platform-specificsplatform-specificsindexmd"></a>[プラットフォーム固有設定](platform-specifics/index.md)

プラットフォーム固有設定を使用すると、カスタム レンダラーや特殊効果を必要とせず、特定のプラットフォームで利用可能なのみの機能を使用できます。 さらに、ベンダーは、効果を使用した独自プラットフォーム固有設定を作成できます。

## <a name="windowswindowsindexmd"></a>[Windows](windows/index.md)

このガイドでは、Xamarin.Forms、および UWP プロジェクトを既存の Xamarin.Forms ソリューションに追加する方法によって提供される Windows プラットフォームの詳細について説明します。
