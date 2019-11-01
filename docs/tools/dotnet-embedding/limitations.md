---
title: .NET 埋め込みの制限事項
description: このドキュメントでは、.net 埋め込みの制限事項について説明します。これは、他のプログラミング言語で .NET コードを使用するためのツールです。
ms.prod: xamarin
ms.assetid: EBBBB886-1CEF-4DF4-AFDD-CA96049F878E
author: davidortinau
ms.author: daortin
ms.date: 11/14/2017
ms.openlocfilehash: 4e2b653365a747b30016a1fbd42b8a01c4c87848
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029741"
---
# <a name="net-embedding-limitations"></a>.NET 埋め込みの制限事項

このドキュメントでは、.NET 埋め込みの制限事項について説明し、可能であれば、それらの回避策を示します。

## <a name="general"></a>全般

### <a name="use-more-than-one-embedded-library-in-a-project"></a>1つのプロジェクトで複数の埋め込みライブラリを使用する

同じアプリケーション内に2つの Mono ランタイムを共存させることはできません。 つまり、同じアプリケーション内で2つの異なる .NET 埋め込み生成ライブラリを使用することはできません。

**回避策:** ジェネレーターを使用すると、さまざまなプロジェクトからの複数のアセンブリを含む単一のライブラリを作成できます。

### <a name="subclassing"></a>サブクラス化

.NET 埋め込みにより、ターゲット言語およびプラットフォーム用のすぐに使用可能な Api のセットを公開することにより、アプリケーション内で Mono ランタイムの統合が容易になります。

ただし、これは双方向の統合ではありません。たとえば、マネージ型をサブクラス化して、マネージコードがネイティブコード内でコールバックすることを想定することはできません。これは、マネージコードがこの併置を認識しないためです。

必要に応じて、この制限の回避策として、

* マネージコードは、ネイティブコードに対して p/invoke を実行できます。 これには、ネイティブコードのカスタマイズを可能にするために、マネージコードをカスタマイズする必要があります。

* Xamarin のような製品を使用し、管理されたライブラリを公開します。これにより、目的の C (この場合は) によって一部のマネージ NSObject サブクラスがサブクラス化されます。

## <a name="objective-c-generated-code"></a>目的 C で生成されたコード

### <a name="nullability"></a>属性

.NET には、API に対して null 参照が許容できるかどうかを通知するメタデータはありません。 ほとんどの Api は、`null` 引数を扱うことができない場合に `ArgumentNullException` をスローします。 この問題が発生するのは、例外を C で処理することが、推奨される回避方法であるためです。

ヘッダーファイルで正確な null 値を許容する注釈を生成することはできず、マネージ例外を最小限に抑える必要があるため、既定では null 以外の引数 (`NS_ASSUME_NONNULL_BEGIN`) が使用されます。また、有効桁数が許容される場合は、null 値を許容する注釈が追加されます。

### <a name="bitcode-ios"></a>Bitcode (iOS)

現在、.NET 埋め込みは、一部の Xcode プロジェクトテンプレートに対して有効になっている iOS 上のビットコードをサポートしていません。 生成されたフレームワークを正常にリンクするには、この設定を無効にする必要があります。

![Bitcode オプション](images/ios-bitcode-option.png)
