---
title: .NET の埋め込みの制限事項
description: このドキュメントでは、.NET の埋め込みを使用すると、他のプログラミング言語での .NET コードを使用する、ツールの制限事項について説明します。
ms.prod: xamarin
ms.assetid: EBBBB886-1CEF-4DF4-AFDD-CA96049F878E
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: fdd3ac4cd57ac7f79f9071d62e758625b30f05dd
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34794125"
---
# <a name="net-embedding-limitations"></a>.NET の埋め込みの制限事項

このドキュメントでは、.NET の埋め込みの制限について説明し、可能な限り、それらの回避策を提供します。

## <a name="general"></a>全般

### <a name="use-more-than-one-embedded-library-in-a-project"></a>プロジェクトで 1 つ以上の埋め込みライブラリを使用します。

2 つの Mono ランタイムが、同じアプリケーションの内部既存の併置することはできません。 つまり、2 つの異なる .NET 埋め込みで生成されたライブラリ、同じアプリケーションの内部を使用することはできません。

**回避策:** (別のプロジェクト) からのいくつかのアセンブリに含まれている 1 つのライブラリを作成するコード ジェネレーターを使用することができます。

### <a name="subclassing"></a>サブクラス化

.NET を埋め込むには、対象言語とプラットフォームですぐに使用できる Api のセットを公開することにより、アプリケーション内の Mono ランタイムの統合が容易になります。

これは、双方向の統合ではありませんなど、マネージ型のサブクラスとできません、マネージ コードは、この共存を認識することはないため、ネイティブ コードの内部コールバックするマネージ コードを想定しています。

必要に応じて、可能性があります、このような制限の回避策の部分を例。

* マネージ コードをできます。 p/呼び出すネイティブ コードにします。 ネイティブ コードからカスタマイズできるように、マネージ コードをカスタマイズする必要があります。

* Xamarin.iOS などの製品を使用し、OBJECTIVE-C (この場合は) サブクラスに管理された一部 NSObject サブクラスを可能にするマネージ ライブラリを公開します。

## <a name="objective-c-generated-code"></a>Objective C コードの生成

### <a name="nullability"></a>Null 値許容属性

メタデータまたは API のかどうかは null 参照は、使用をお聞かせ .NET ではありません。 ほとんどの Api がスローされます`ArgumentNullException`それらに対応できない場合、`null`引数。 これは問題のある Objective C の例外は何かの優れた回避できます。

Null 以外の引数に既定のヘッダー ファイルに正確な null 値許容属性注釈を生成し、マネージ例外を最小化することはできませんのでお (`NS_ASSUME_NONNULL_BEGIN`) し、いくつかの固有の仕様、有効桁数が可能な場合、null 値許容属性注釈を追加します。

### <a name="bitcode-ios"></a>Bitcode (iOS)

現在 .NET 埋め込みサポートしていません bitcode ios で、一部の Xcode プロジェクト テンプレートが有効になっています。 これは、正常に生成されたリンク フレームワークに無効にする必要があります。

![Bitcode オプション](images/ios-bitcode-option.png)
