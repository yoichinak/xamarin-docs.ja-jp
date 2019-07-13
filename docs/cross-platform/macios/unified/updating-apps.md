---
title: Unified API への既存のアプリの更新
description: このドキュメントは、Unified API への Xamarin アプリケーションを更新する方法を説明するさまざまなガイドにリンクしています。 Xamarin.iOS アプリでは、Xamarin.Mac アプリがについて説明します。 Xamarin.Forms アプリでは、クロス プラットフォーム アプリ、およびバインド プロジェクトでのネイティブ型です。
ms.prod: xamarin
ms.assetid: 8A654C95-5DCA-4BB5-A582-F96C2BECC81C
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 7730d19e4f261a0e414e4946fd0add5312f5d030
ms.sourcegitcommit: 7ccc7a9223cd1d3c42cd03ddfc28050a8ea776c2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/13/2019
ms.locfileid: "67864355"
---
# <a name="updating-existing-apps-to-the-unified-api"></a>Unified API への既存のアプリの更新

> [!IMPORTANT]
> Unified API の前に、Xamarin のクラシック API は非推奨とされました。
> - 最後のバージョンのクラシック API (monotouch.dll) をサポートするために Xamarin.iOS では、Xamarin.iOS 9.10 をしました。
> - Xamarin.Mac には、従来の API では、引き続きサポートされていますは更新されなく。 非推奨であるために、開発者は、Unified API アプリケーションを移動します。

## <a name="how-to-update-your-apps"></a>アプリを更新する方法

アプリを更新する次の 3 つの手順があります。

1. 特にの非推奨の Api に関連する、既存のコードで、コンパイラの警告を修正します。

2. Visual Studio for Mac に組み込まれている移行ツールを使用して、プロジェクト ファイルと名前空間を更新します。

3. 残りの関連する、新しいコンパイラ エラー修正[64 種類](~/cross-platform/macios/nativetypes.md)と[他の Api](~/cross-platform/macios/unified/overview.md#deprecated-typos)に変更されました。 チェック アウト[これらのヒント](~/cross-platform/macios/unified/updating-tips.md)詳細については、手動で更新する必要があります。

特定のガイドは、Unified API と 64 ビットをサポートするアプリを更新するためには、各製品使用できます。

### <a name="xamarinios-appscross-platformmaciosunifiedupdating-ios-appsmd"></a>[Xamarin.iOS アプリ](~/cross-platform/macios/unified/updating-ios-apps.md)

For mac。 Visual Studio に組み込まれている自動移行ツールを使用して、Unified API に既存の Xamarin.iOS アプリを更新できます。 いくつか追加の修正は次の必要な場合で説明したよう[手順](~/cross-platform/macios/unified/updating-ios-apps.md)と[ヒント](~/cross-platform/macios/unified/updating-tips.md)します。

### <a name="xamarinmac-appscross-platformmaciosunifiedupdating-mac-appsmd"></a>[Xamarin.Mac アプリ](~/cross-platform/macios/unified/updating-mac-apps.md)

For mac。 Visual Studio に組み込まれている自動移行ツールを使用して、Unified API に既存の Xamarin.Mac アプリを更新できます。 いくつか追加の修正は次の必要な場合で説明したよう[手順](~/cross-platform/macios/unified/updating-mac-apps.md)と[ヒント](~/cross-platform/macios/unified/updating-tips.md)します。

### <a name="xamarinforms-appscross-platformmaciosunifiedupdating-xamarin-forms-appsmd"></a>[Xamarin.Forms アプリ](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)

Unified API を使用して、iOS プロジェクトで既存の Xamarin.Forms ソリューションを更新する次の手順に従います。 統一された API のサポートは Xamarin.Forms 1.3 で使用できる以降のみように[指示](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)バージョン 1.3 に Xamarin.Forms アプリを更新する方法についても説明します。 これら[ヒント](~/cross-platform/macios/unified/updating-tips.md)カスタム レンダラーまたは依存関係サービスで、ネイティブ iOS コードの更新に役立つ場合があります。

## <a name="working-with-native-types-in-cross-platform-appscross-platformmaciosnativetypesmd"></a>[クロスプラットフォーム アプリでのネイティブ型の使用](~/cross-platform/macios/nativetypes.md)

この記事では、Android や Windows Phone Os iOS 以外のデバイスでコードが共有されているクロス プラットフォーム アプリケーションで新しい iOS Unified API をネイティブ型 (nint、nuint、nfloat) を使用する方法について説明します。 ネイティブ型を使用するかを把握し、クロス プラットフォームのコードで、新しい型を使用する必要がありますケースをいくつかの考えられる解決策を提供します。

## <a name="update-bindings-to-the-unified-api"></a>Unified API へのバインドを更新します。

Objective C ライブラリへのバインドを作成したお客様は、(一部の種類が今すぐされる 64 ビット)、基になる API の変更を反映するようにバインディング プロジェクトを更新する必要があります。
次の手順に従います[Unified API をサポートするために既存のバインド プロジェクト更新](~/cross-platform/macios/unified/update-binding.md)します。

## <a name="related-links"></a>関連リンク

- [IOS アプリの更新](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Mac アプリを更新します。](~/cross-platform/macios/unified/updating-mac-apps.md)
- [Xamarin.Forms アプリを更新します。](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)
- [バインドを更新しています](~/cross-platform/macios/unified/update-binding.md)
- [ヒントを更新しています](~/cross-platform/macios/unified/updating-tips.md)
- [クラシックと Unified API の相違点](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
