---
title: Apple プラットフォーム (iOS と Mac)
description: このドキュメントは、Xamarin.iOS および Xamarin.Mac 開発に関連するさまざまなトピックを説明します。 コード共有、Unified API、Objective C ライブラリ、ネイティブ参照、ネイティブな型および複数のバインド。
ms.prod: xamarin
ms.assetid: 67246203-D78E-4DCC-9E55-7D3D93968E54
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: b40758fa562e57415cd3c0818763ef0a7ce5dcca
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61199768"
---
# <a name="apple-platform-ios-and-mac"></a>Apple プラットフォーム (iOS と Mac)

## <a name="code-sharing"></a>コードの共有

なしのユーザー インターフェイス要素が共有する最善の方法が、コードの要素に対しては引き続きの使用に iOS および Mac 間でコード[ポータブル クラス ライブラリ](~/cross-platform/app-fundamentals/pcl.md)します。

ユーザー インターフェイスの操作を行う必要のあるコードを共有したいを使用する必要があります[共有プロジェクト](~/cross-platform/app-fundamentals/shared-projects.md)1 つのプロジェクトで共有して、それを参照したときに、iOS と Mac の両方でコンパイルするコードを配置できます。

##  <a name="unified-apiunifiedindexmd"></a>[Unified API](unified/index.md)

IOS と Mac のプロジェクトの Unified API は、同じコード ファイルは、シームレスなコード共有の両方のプラットフォームで使用できるように、フレームワークを同じ名前空間を使用します。 32 と 64 ビットのビルドもできます。 Unified API は、2015 年前半から既定のテンプレートが経ちあり - すべての新しいプロジェクトはお勧め*のみ*Unified API プロジェクトを App Store に送信することができます。

### <a name="classic-apis"></a>クラシック Api

> [!NOTE]
> **非推奨のクラシック プロファイル:** Xamarin.iOS で新しいプラットフォームが追加されるとクラシックのプロファイル (monotouch.dll) から機能を徐々 に廃止が開始します。 たとえば、非 NRC (参照カウントには新しい) オプションが削除されました。 NRC 常に有効になっている統合されたすべてのアプリケーション (つまり非 NRC がオプションではなかったことはありません) を既知の問題を持ちません。 今後のリリースでは、ガベージ コレクターと Boehm を使用するオプションを削除します。 また、オプションが統合されたアプリケーションで使用できることはありませんでした。 クラシックのサポートを完全に取り除く fall 2016 Xamarin.iOS 10.0 のリリースで予定です。

元の (非統合) Xamarin.iOS および Xamarin.Mac Api コードの共有は困難かネイティブ フレームワークがあったため、`MonoTouch.`または`MonoMac.`名前空間プレフィックス。  追加することでコードを共有できるようにするいくつか空の名前空間が用意されています`using`同じファイルが、これに MonoMac と MonoTouch の両方の名前空間を参照するステートメントが少し見にくいです。 内部的に分散するレガシ アプリで使用するクラシック API を続行する必要がありますのみ (Unified API へのアップグレードを推奨)。


### <a name="updating-from-classic-to-the-unified-api"></a>クラシックから Unified API への更新

Unified API に、クラシックから任意のアプリケーションを更新するための詳細な手順はありません。

## <a name="binding-objective-c-librariesbindingindexmd"></a>[Objective-C ライブラリのバインド](binding/index.md)

Xamarin では、バインドと、アプリにネイティブ ライブラリを取り込むことができます。 このセクションについて説明します。

- バインドのしくみ
- Objective C コードを Xamarin に表示できるバインド プロジェクトを手動で構築する方法と
- 使用する方法、**目標油性**プロセスの自動化に役立つツールです。

## <a name="native-referencesnative-referencesmd"></a>[ネイティブ参照](native-references.md)

##  <a name="macios-native-typesnativetypesmd"></a>[Mac と iOS のネイティブ型](nativetypes.md)

透過的に 32 ビットおよび 64 ビットのコードをサポートするためにC#とF#、新しいデータ型が導入されています。   ここで詳細を確認します。

##  <a name="building-32-and-64-bit-apps32-and-64indexmd"></a>[32 ビットおよび 64 ビット アプリの構築](32-and-64/index.md)

32 ビットおよび 64 ビット アプリケーションをサポートするために理解するために必要もの。

## <a name="working-with-native-types-in-cross-platform-appsnative-types-cross-platformmd"></a>[クロスプラットフォーム アプリでのネイティブ型の使用](native-types-cross-platform.md)

この記事では、新しい iOS Unified API をネイティブ型の使用方法について説明します (`nint`、 `nuint`、 `nfloat`) Android や Windows Phone Os iOS 以外のデバイスでコードが共有されているクロス プラットフォーム アプリケーションにします。
ネイティブ型を使用するかを把握し、クロス プラットフォームのコードで、新しい型を使用する必要がありますケースをいくつかの考えられる解決策を提供します。

## <a name="httpclient-stack-and-ssltls-implementation-selectorhttp-stackmd"></a>[HttpClient スタックと SSL/TLS の実装セレクター](http-stack.md)

新しい HttpClient スタック セレクターは、Xamarin.iOS、Xamarin.tvOS および Xamarin.Mac アプリで使用する HttpClient 実装を制御します。 TvOS のまたは OS x まで配送のネイティブの iOS を使用する実装に切り替えるようになりました (`NSUrlSession`または`CFNetwork`OS によって)。

SSL (Secure Socket Layer) とその後継の TLS (トランスポート層セキュリティ)、HTTP およびその他のネットワーク接続経由でのサポートを提供します。`System.Net.Security.SslStream`します。 SSL/TLS の実装の新しいビルド オプションは、Mono の TLS のスタック、および Apple の TLS スタックには、Mac、iOS を搭載した 1 つの間で切り替わります。
