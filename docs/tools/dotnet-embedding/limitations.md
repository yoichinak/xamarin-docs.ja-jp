---
title: .NET 埋め込みの制限事項
description: このドキュメントでは、.net 埋め込みの制限事項について説明します。これは、他のプログラミング言語で .NET コードを使用するためのツールです。
ms.prod: xamarin
ms.assetid: EBBBB886-1CEF-4DF4-AFDD-CA96049F878E
author: davidortinau
ms.author: daortin
ms.date: 11/14/2017
ms.openlocfilehash: 2fbd42efcdb3de10a7f094db2197a75d88d27b66
ms.sourcegitcommit: 8fa0cb9ccbc107d697aa5b9113a4e5d1e75d6eb9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/27/2020
ms.locfileid: "96303027"
---
# <a name="net-embedding-limitations"></a>.NET 埋め込みの制限事項

このドキュメントでは、.NET 埋め込みの制限事項について説明し、可能であれば、それらの回避策を示します。

## <a name="general"></a>全般

### <a name="use-more-than-one-embedded-library-in-a-project"></a>1つのプロジェクトで複数の埋め込みライブラリを使用する

同じアプリケーション内に2つの Mono ランタイムを共存させることはできません。 つまり、同じアプリケーション内で2つの異なる .NET 埋め込み生成ライブラリを使用することはできません。

**回避策:** ジェネレーターを使用すると、さまざまなプロジェクトからの複数のアセンブリを含む単一のライブラリを作成できます。

### <a name="subclassing"></a>サブクラス化

.NET 埋め込みにより、ターゲット言語およびプラットフォーム用のすぐに使用可能な Api のセットを公開することにより、アプリケーション内で Mono ランタイムの統合が容易になります。

ただし、これは双方向の統合ではありません。たとえば、マネージ型をサブクラス化して、マネージコードがネイティブコード内でコールバックすることを想定することはできません。これは、マネージコードがこの共存を認識しないためです。

必要に応じて、この制限の回避策として、

* マネージコードは、ネイティブコードに対して p/invoke を実行できます。 これには、ネイティブコードのカスタマイズを可能にするために、マネージコードをカスタマイズする必要があります。

* Xamarin のような製品を使用し、管理されたライブラリを公開します。これにより、目的の C (この場合は) によって一部のマネージ NSObject サブクラスがサブクラス化されます。

## <a name="objective-c-generated-code"></a>目的 C で生成されたコード

### <a name="nullability"></a>NULL 値の許容

.NET には、API に対して null 参照が許容できるかどうかを通知するメタデータはありません。 ほとんどの Api は `ArgumentNullException` 、引数を扱うことができない場合にスロー `null` します。 この問題が発生するのは、例外を C で処理することが、推奨される回避方法であるためです。

ヘッダーファイル内で正確な null 値を許容する注釈を生成することはできず、マネージ例外を最小限に抑える必要があるため、既定では null 以外の引数 ( `NS_ASSUME_NONNULL_BEGIN` ) が使用されます。また、有効桁数が許容される場合は、null 値の許容属性を追加します。

### <a name="bitcode-ios"></a>Bitcode (iOS)

現在、.NET 埋め込みは、一部の Xcode プロジェクトテンプレートに対して有効になっている iOS 上のビットコードをサポートしていません。 生成されたフレームワークを正常にリンクするには、この設定を無効にする必要があります。

* IOS の場合、Apple の AppStore にアプリを送信するには、bitcode を省略できます。 生成された bitcode が "inline assembly" であるため、Xamarin は iOS 用にこれをサポートしていません。 これにより、サーバー側を最適化することはできず、バイナリを大きくしてビルド時間を長くすることができないため、iOS プラットフォームにはメリットがありません。

* TvOS と watchOS の場合、Apple の AppStore にアプリを送信するには、bitcode が必要です。 Xamarin iOS では、この要件を満たすために、tvOS ("inline assembly") と watchOS ("LLVM/IR" として) の bitcode がサポートされています。

* MacOS では、現在、bitcode サポートは必要なく、Xamarin. Mac でもサポートされていません。

![Bitcode オプション](images/ios-bitcode-option.png)
