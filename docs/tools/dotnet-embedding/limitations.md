---
title: .NET の埋め込みの制限事項
ms.prod: xamarin
ms.assetid: EBBBB886-1CEF-4DF4-AFDD-CA96049F878E
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 80d2402329020002a43e1f9dd7b518e2ce74747c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="net-embedding-limitations"></a>.NET の埋め込みの制限事項


このドキュメントでは、.NET の埋め込み (Embeddinator 4000) の制限について説明し、可能な限り、それらの回避策を提供します。

## <a name="general"></a>全般

### <a name="use-more-than-one-embedded-library-in-a-project"></a>プロジェクトで 1 つ以上の埋め込みライブラリを使用します。

2 つの mono ランタイムが、同じアプリケーションの内部既存の併置することはできません。 つまり、2 つ異なる embeddinator 4000 生成ライブラリ、同じアプリケーションの内部を使用することはできません。

**回避策:** (別のプロジェクト) からのいくつかのアセンブリに含まれている 1 つのライブラリを作成するコード ジェネレーターを使用することができます。

### <a name="subclassing"></a>サブクラス化

Embeddinator には、対象言語とプラットフォームですぐに使用できる Api のセットを公開することにより、アプリケーション内の mono ランタイムの統合が容易になります。

これは、双方向の統合ではありませんなど、マネージ型のサブクラスとできません、マネージ コードは、この共存を認識することはないため、ネイティブ コードの内部コールバックするマネージ コードを想定しています。

必要に応じて、可能性があります、このような制限の回避策の部分を例。

* マネージ コードをできます。 p/呼び出すネイティブ コードにします。 ネイティブ コードからカスタマイズできるように、マネージ コードをカスタマイズする必要があります。

* Xamarin.iOS などの製品を使用し、マネージ ライブラリを許可する ObjC (この場合は) サブクラスによってマネージ NSObject サブクラスを公開します。


## <a name="objc-generated-code"></a>生成された ObjC コード

### <a name="nullability"></a>Null 値許容属性

.NET では、メタデータはありませんを何かありましたら、null 参照が許容される場合、または API をされません。 ほとんどの Api がスローされます`ArgumentNullException`それらに対応できない場合、`null`引数。 これは問題のある例外の ObjC 処理は何かの優れた回避できます。

Null 以外の引数に既定のヘッダー ファイルに正確な null 値許容属性注釈を生成し、マネージ例外を最小化することはできませんのでお (`NS_ASSUME_NONNULL_BEGIN`) し、いくつかの固有の仕様、有効桁数が可能な場合、null 値許容属性注釈を追加します。
