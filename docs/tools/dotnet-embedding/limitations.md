---
title: .NET の埋め込みの制限事項
description: このドキュメントでは、.NET の埋め込み、他のプログラミング言語で .NET コードを使用できるツールの制限事項について説明します。
ms.prod: xamarin
ms.assetid: EBBBB886-1CEF-4DF4-AFDD-CA96049F878E
author: lobrien
ms.author: laobri
ms.date: 11/14/2017
ms.openlocfilehash: 7a162d632c98b4e412fa1b7b0c0c40ac945ff09f
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "60945797"
---
# <a name="net-embedding-limitations"></a>.NET の埋め込みの制限事項

このドキュメントでは、.NET の埋め込みの制限事項について説明し、可能であれば、それらの回避策を提供します。

## <a name="general"></a>全般

### <a name="use-more-than-one-embedded-library-in-a-project"></a>プロジェクトで 1 つ以上の埋め込みライブラリを使用します。

2 つの Mono ランタイムが、同じアプリケーション内で共存させることはできません。 つまり、2 つの異なる .NET に埋め込むことによって生成されたライブラリと同じアプリケーション内でを使用することはできません。

**回避策:** コード ジェネレーターを使用すると、(別のプロジェクト) からのいくつかのアセンブリを含む 1 つのライブラリを作成します。

### <a name="subclassing"></a>サブクラス化

.NET を埋め込むには、対象言語とプラットフォームにすぐに使用できる Api のセットを公開することで、アプリケーション内で Mono ランタイムの統合が容易になります。

これは双方向の統合ではありません、例: マネージ型のサブクラス化できないし、マネージ コードは、この共存を意識することはないため、ネイティブ コード内でコールバックをマネージ コードを期待します。

必要に応じて、可能性があります、この制限の回避策の部分になど

* マネージ コードは、p/invoke をできる、ネイティブ コードにします。 ネイティブ コードからカスタマイズできるように、マネージ コードをカスタマイズする必要があります。

* Xamarin.iOS のような製品を使用し、Objective C (この場合は) サブクラスにいくつかマネージ NSObject サブクラスできるようにするマネージ ライブラリを公開します。

## <a name="objective-c-generated-code"></a>Objective C で生成されたコード

### <a name="nullability"></a>null 値の許容

メタデータ API ではなくまたは null 参照が許容されるかどうかはお知らせを .NET ではありません。 ほとんどの Api がスローされます`ArgumentNullException`対応できない場合、`null`引数。 Objective C の例外処理は、もっと優れた回避これは問題です。

Null 以外の引数に既定のヘッダー ファイルに正確な null 値許容属性注釈を生成し、マネージ例外を最小化することはできませんので (`NS_ASSUME_NONNULL_BEGIN`) によって、特定の有効桁数が可能な場合、null 値許容属性注釈を追加します。

### <a name="bitcode-ios"></a>Bitcode (iOS)

現在 .NET の埋め込みはサポートされていません bitcode ios では、いくつかの Xcode プロジェクト テンプレートを有効になっています。 これは、正常に生成されたリンク フレームワークに無効にする必要があります。

![Bitcode オプション](images/ios-bitcode-option.png)
