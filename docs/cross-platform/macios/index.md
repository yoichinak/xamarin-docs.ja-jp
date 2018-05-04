---
title: Apple のプラットフォーム (iOS と Mac)
description: このセクションでは、Xamarin.iOS および Xamarin.Mac プロジェクトでコードを共有する方法を説明します。
ms.prod: xamarin
ms.assetid: 67246203-D78E-4DCC-9E55-7D3D93968E54
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: a842e89b74419a03e4ea8f4355162960d9109f9b
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/03/2018
---
# <a name="apple-platform-ios-and-mac"></a>Apple のプラットフォーム (iOS と Mac)

_このセクションでは、Xamarin.iOS および Xamarin.Mac プロジェクトでコードを共有する方法を説明します。_

## <a name="code-sharing"></a>コードの共有

要素のコードのないユーザー インターフェイスの要素を持つを共有する最良の方法 iOS と Mac 間のコードは引き続きの使用[ポータブル クラス ライブラリ](~/cross-platform/app-fundamentals/pcl.md)です。

ユーザー インターフェイスの操作を行う必要のあるコードを共有する場合、まだ使用する必要があります[共有プロジェクト](~/cross-platform/app-fundamentals/shared-projects.md)を単一のプロジェクトで共有し、Mac と参照されている場合、iOS の両方でコンパイルされたコードを配置することです。

##  <a name="unified-apiunifiedindexmd"></a>[Unified API](unified/index.md)

IOS と Mac のプロジェクトの Unified API は、同じコード ファイルは、シームレスなコード共有の両方のプラットフォームで使用できるようにする、フレームワークを同じ名前空間を使用します。 32 と 64 ビットのビルドこともできます。 Unified API は、テンプレートの既定初期 2015 は、以降され、新しいプロジェクトのすべての推奨*のみ*Unified API プロジェクトは、アプリ ストアに送信できます。

### <a name="classic-apis"></a>従来の Api

> [!NOTE]
> **プロファイル非推奨のクラシック:** クラシック プロファイル (monotouch.dll) から機能を徐々 に廃止する開始 Xamarin.iOS で新しいプラットフォームが追加されるとします。 たとえば、非 NRC (ref カウントには新しい) オプションが削除されました。 NRC が常に有効になってすべて統合されたアプリケーション (つまり NRC 以外がオプションではなかったことはありません) の既知の問題はありません。 今後のリリースでは、ガベージ コレクターとして Boehm を使用するオプションを削除します。 サポートしても、オプションの統合されたアプリケーションで使用できることはありませんでした。 従来のサポートを完全に取り除く秋 2016 Xamarin.iOS 10.0 のリリースではスケジュールされます。

元の (非統合) Xamarin.iOS および Xamarin.Mac Api コード共有より困難にネイティブ フレームワークでは、いずれかの必要があるため`MonoTouch.`または`MonoMac.`名前空間プレフィックス。  一部の空の名前空間は開発者を追加することでコードを共有できるようにするが備えられています`using`同じファイルが、これで MonoMac と MonoTouch の両方の名前空間を参照するステートメントが少し悪いです。 従来の API がのみ引き続きは内部的に分散しているレガシのアプリで使用可能 (Unified API へのアップグレードを推奨)。


### <a name="updating-from-classic-to-the-unified-api"></a>統一された API をクラシックから更新

クラシック Unified API へのすべてのアプリケーションを更新するための詳細な指示があります。

## <a name="binding-objective-c-librariesbindingindexmd"></a>[Objective-C ライブラリのバインド](binding/index.md)

Xamarin では、バインドで、アプリにネイティブ ライブラリを表示できます。 このセクションの内容について説明します。

- バインディングのしくみ
- Objective C コードを取り込む Xamarin を使用できるバインディング プロジェクトを手動で構築する方法と
- 使用する方法、**目標ペンを使わず**プロセスの自動化に役立つツールです。

## <a name="native-referencesnative-referencesmd"></a>[ネイティブ参照](native-references.md)



##  <a name="macios-native-typesnativetypesmd"></a>[Mac/iOS のネイティブ型](nativetypes.md)

32 と 64 ビット コードが透過的に c# f# をサポートするには、新しいデータ型が導入されました。   ここでそれらについて説明します。

##  <a name="building-32-and-64-bit-apps32-and-64indexmd"></a>[32 と 64 ビット アプリの構築](32-and-64/index.md)

どのようなしておく必要があります 32 と 64 ビット アプリケーションをサポートします。

## <a name="working-with-native-types-in-cross-platform-appsnative-types-cross-platformmd"></a>[クロスプラットフォーム アプリでのネイティブ型の使用](native-types-cross-platform.md)

この記事では、新しい iOS Unified API をネイティブ型の使用方法について説明 (`nint`、 `nuint`、 `nfloat`) コードが Android や Windows Phone Os などの非 iOS デバイスと共有されているクロス プラットフォーム アプリケーションでします。
ネイティブ型を使用すべき状況を把握し、クロス プラットフォームのコードで、新しい型を使用する必要がある場合に使用するいくつかの考えられる解決策を提供します。


## <a name="httpclient-stack-and-ssltls-implementation-selectorhttp-stackmd"></a>[HttpClient スタックと SSL/TLS の実装セレクター](http-stack.md)

新しい HttpClient スタック セレクターは、Xamarin.iOS、Xamarin.tvOS および Xamarin.Mac アプリで使用するどの HttpClient の実装を制御します。 TvOS のまたは OS X ネイティブ トランスポートを iOS を使用して実装に切り替えるようになりました (`NSUrlSession`または`CFNetwork`OS によって異なります)。

SSL (Secure Socket Layer) とその後続バージョンの TLS (Transport Layer Security)、HTTP およびその他のネットワーク接続経由でのサポートを提供`System.Net.Security.SslStream`です。 SSL や TLS の実装の新しいビルド オプションは、Mono の TLS スタック、および Mac の iOS 内にある Apple の TLS スタックでの電源 1 の間で切り替わります。
