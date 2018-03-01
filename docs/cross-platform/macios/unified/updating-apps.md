---
title: "統合 API への既存のアプリの更新"
ms.topic: article
ms.prod: xamarin
ms.assetid: 8A654C95-5DCA-4BB5-A582-F96C2BECC81C
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: b1b6338494b9be98e677cf9d338410eae759feb8
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="updating-existing-apps-to-the-unified-api"></a>統合 API への既存のアプリの更新

> [!IMPORTANT]
> **プロファイル非推奨のクラシック:**クラシック プロファイル (monotouch.dll) から機能を徐々 に廃止する開始 Xamarin.iOS で新しいプラットフォームが追加されるとします。 たとえば、非 NRC (ref カウントには新しい) オプションが削除されました。 NRC が常に有効になってすべて統合されたアプリケーション (つまり NRC 以外がオプションではなかったことはありません) の既知の問題はありません。 今後のリリースでは、ガベージ コレクターとして Boehm を使用するオプションを削除します。 サポートしても、オプションの統合されたアプリケーションで使用できることはありませんでした。 従来のサポートを完全に削除は、Xamarin.iOS 10.0 のリリースで [次へ] の秋にスケジュールされます。




## <a name="how-to-update-your-apps"></a>アプリを更新する方法

Xamarin 大学自由に利用可能なビデオが**iOS Unified API へのアップグレード**です。 参照してください[Xamarin 大学稲妻講義](http://university.xamarin.com/lightninglectures)を監視します。

[ ![](updating-apps-images/xamu-video-sml.png "Xamarin University")](http://university.xamarin.com/lightninglectures)

これには、アプリを更新する 3 つの手順があります。

1. 非推奨の Api に関連するものでは特に、既存のコードで、コンパイラ警告を修正します。

2. Mac 用の Visual Studio に組み込まれている移行ツールを使用して、プロジェクト ファイルと名前空間を更新します。

3. 残りの新しいに関連するコンパイラ エラー修正[64 型](~/cross-platform/macios/nativetypes.md)と[他の Api](~/cross-platform/macios/unified/index.md#deprecated-typos)に変更されました。 チェック アウト[これらのヒント](~/cross-platform/macios/unified/updating-tips.md)必要になる可能性がある手動で更新の詳細についてはします。

特定のガイドは、Unified API と 64 ビットをサポートするアプリを更新するためには、各製品利用できます。

### <a name="xamarinios-appscross-platformmaciosunifiedupdating-ios-appsmd"></a>[Xamarin.iOS アプリ](~/cross-platform/macios/unified/updating-ios-apps.md)

For mac を Visual Studio に組み込まれて自動的に移行ツールを使用して、統合 API に既存の Xamarin.iOS アプリを更新することができます。 いくつか追加の修正し、必要があります、」の説明に従って[手順](~/cross-platform/macios/unified/updating-ios-apps.md)と[ヒント](~/cross-platform/macios/unified/updating-tips.md)です。

###  <a name="xamarinmac-appscross-platformmaciosunifiedupdating-mac-appsmd"></a>[Xamarin.Mac アプリ](~/cross-platform/macios/unified/updating-mac-apps.md)

For mac を Visual Studio に組み込まれて自動的に移行ツールを使用して、統合 API に Xamarin.Mac の既存のアプリを更新することができます。 いくつか追加の修正し、必要があります、」の説明に従って[手順](~/cross-platform/macios/unified/updating-mac-apps.md)と[ヒント](~/cross-platform/macios/unified/updating-tips.md)です。

###  <a name="xamarinforms-appscross-platformmaciosunifiedupdating-xamarin-forms-appsmd"></a>[Xamarin.Forms アプリ](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)

Unified API を使用して、iOS プロジェクトで既存の Xamarin.Forms ソリューションを更新する次の手順に従います。 統一された API のサポートのみ利用可能で、Xamarin.Forms 1.3 にその後、ように[指示](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)バージョン 1.3 に Xamarin.Forms アプリを更新する方法についても説明します。 これら[ヒント](~/cross-platform/macios/unified/updating-tips.md)カスタム レンダラーや依存関係サービス内のすべてのネイティブの iOS コードを更新役立つ場合があります。

## <a name="working-with-native-types-in-cross-platform-appscross-platformmaciosnativetypesmd"></a>[クロスプラット フォーム アプリでネイティブ型の使用](~/cross-platform/macios/nativetypes.md)

この記事では、コードが Android や Windows Phone Os などの非 iOS デバイスと共有されているクロス プラットフォーム アプリケーションで新しい iOS Unified API をネイティブ型 (nint、nuint、nfloat) の使用について説明します。 ネイティブ型を使用すべき状況を把握し、クロス プラットフォームのコードで、新しい型を使用する必要がある場合に使用するいくつかの考えられる解決策を提供します。

## <a name="update-bindings-to-the-unified-api"></a>統一された API へのバインドを更新します。

Objective C のライブラリへのバインドを作成した顧客は、(一部の型が現在される 64 ビット)、基になる API の変更を反映するようにバインディング プロジェクトを更新する必要があります。
次の手順に従って[Unified API をサポートするために既存のバインドのプロジェクトを更新](~/cross-platform/macios/unified/update-binding.md)です。




## <a name="related-links"></a>関連リンク

- [IOS アプリの更新](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Mac アプリケーションの更新](~/cross-platform/macios/unified/updating-mac-apps.md)
- [Xamarin.Forms アプリの更新](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)
- [バインドの更新](~/cross-platform/macios/unified/update-binding.md)
- [ヒントの更新](~/cross-platform/macios/unified/updating-tips.md)
- [従来の vs Unified API の相違点](http://developer.xamarin.comhttps://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
