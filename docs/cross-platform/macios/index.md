---
title: Apple プラットフォーム (iOS および Mac)
description: このドキュメントでは、Xamarin の iOS および Xamarin の開発に関連するさまざまなトピックについて説明します。コード共有、Unified API、バインディングの目的 C ライブラリ、ネイティブ参照、ネイティブ型などがあります。
ms.prod: xamarin
ms.assetid: 67246203-D78E-4DCC-9E55-7D3D93968E54
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: 10ab9b379344ab6c514eba84f1ef3fd9c7400b73
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70765543"
---
# <a name="apple-platform-ios-and-mac"></a>Apple プラットフォーム (iOS および Mac)

## <a name="code-sharing"></a>コード共有

ユーザー インターフェイス要素を持たないコードの構成の場合、iOS および Mac の間でコードを共有するための最善な方法は、依然として[ポータブル クラス ライブラリ](~/cross-platform/app-fundamentals/pcl.md)を使用することです。

いくつかのユーザーインターフェイスの動作を実行する必要があるコードの場合は、共有する必要があります。共有[プロジェクト](~/cross-platform/app-fundamentals/shared-projects.md)を使用すると、コードを1つのプロジェクトで共有し、Mac と iOS の両方を参照してコンパイルすることができます。

## <a name="unified-apiunifiedindexmd"></a>[Unified API](unified/index.md)

IOS プロジェクトと Mac プロジェクトの Unified API では、同じコードファイルを両方のプラットフォームで使用できるように、同じ名前空間をフレームワークに使用します。これにより、シームレスなコード共有が可能になります。 また、32と64の両方のビットビルドが有効になります。 Unified API は、初期2015以降のテンプレートの既定の設定であり、すべての新しいプロジェクトに対して、Unified API プロジェクト*のみ*を App Store に送信できるようにすることをお勧めします。

### <a name="classic-apis"></a>クラシック Api

> [!NOTE]
> **クラシックプロファイルの非推奨:** 新しいプラットフォームが Xamarin に追加されるにつれて、クラシックプロファイル (monotouch.dialog) の機能が徐々に廃止されるようになります。 たとえば、NRC 以外のオプション (新しい参照カウント) が削除されました。 NRC は、すべての統合アプリケーションに対して常に有効になっています (つまり、NRC 以外のオプションはありません)。また、既知の問題はありません。 将来のリリースでは、Boehm をガベージコレクターとして使用するオプションが削除されます。 これは、統合アプリケーションで使用できないオプションでもあります。 クラシックサポートを完全に削除するには、Xamarin. iOS 10.0 のリリースで2016の秋が予定されています。

ネイティブフレームワーク`MonoTouch.`にはまたは`MonoMac.`名前空間プレフィックスがあるため、元の (統合されていない) xamarin. iOS および xamarin. Mac api は、コード共有をより困難にしました。  同じファイルで、モノの mac と monotouch.dialog の両方の名前`using`空間を参照するステートメントを追加することにより、開発者がコードを共有できるようにする空の名前空間がいくつか用意されていますが、これは少し厄介です。 Classic API は、内部で配布されたレガシアプリでのみ使用できます (Unified API へのアップグレードをお勧めします)。

### <a name="updating-from-classic-to-the-unified-api"></a>クラシックから Unified API への更新

アプリケーションをクラシックから Unified API に更新するための詳細な手順があります。

## <a name="binding-objective-c-librariesbindingindexmd"></a>[Objective-C ライブラリのバインド](binding/index.md)

Xamarin では、バインドを使用してアプリにネイティブライブラリを取り込むことができます。 このセクションの説明は次のとおりです。

- バインディングのしくみ
- 目的の C コードを Xamarin に取り込むことができるバインドプロジェクトを手動でビルドする方法
- **目標マジックペン**ツールを使用してプロセスを自動化する方法。

## <a name="native-referencesnative-referencesmd"></a>[ネイティブ参照](native-references.md)

## <a name="macios-native-typesnativetypesmd"></a>[Mac/iOS ネイティブ型](nativetypes.md)

とC# F#から透過的に32と64のビットコードをサポートするために、新しいデータ型を導入しています。   詳細については、こちらを参照してください。

## <a name="building-32-and-64-bit-apps32-and-64indexmd"></a>[32および64ビットアプリのビルド](32-and-64/index.md)

32および64ビットアプリケーションをサポートするために必要な知識。

## <a name="working-with-native-types-in-cross-platform-appsnative-types-cross-platformmd"></a>[クロスプラットフォーム アプリでのネイティブ型の使用](native-types-cross-platform.md)

この記事では、Android や Windows Phone os などの`nint`ios `nuint`以外のデバイスでコードを共有するクロスプラットフォームアプリケーションで、新しい ios Unified API ネイティブ型 (、、 `nfloat`) を使用する方法について説明します。
ネイティブ型を使用するタイミングについての洞察を提供し、新しい型をクロスプラットフォームコードと共に使用する必要がある場合に、いくつかの考えられる解決策を提供します。

## <a name="httpclient-stack-and-ssltls-implementation-selectorhttp-stackmd"></a>[HttpClient スタックと SSL/TLS の実装セレクター](http-stack.md)

新しい HttpClient スタックセレクターは、Xamarin. iOS、tvOS、および Xamarin. Mac アプリで使用する HttpClient 実装を制御します。 IOS の、tvOS、または os X のネイティブトランスポート (`NSUrlSession`または os によっては`CFNetwork` ) を使用する実装に切り替えることができるようになりました。

SSL (Secure Socket Layer) とその後継となる TLS (トランスポート層セキュリティ) では、を介し`System.Net.Security.SslStream`た HTTP およびその他のネットワーク接続がサポートされます。 新しい SSL/TLS 実装のビルドオプションは、Mono の独自の TLS スタックと、Mac および iOS に存在する Apple の TLS スタックを利用したものを切り替えます。
