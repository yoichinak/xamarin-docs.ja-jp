---
title: Xamarin.Forms で .NET standard 2.0 サポート
description: この記事では、.NET Standard 2.0 を使用して、Xamarin.Forms アプリケーションに変換する方法について説明します。 .NET standard には、すべての .NET 実装で使用可能にすることを意図した .NET Api の仕様を示します。
ms.prod: xamarin
ms.assetid: 95805355-63a7-44e7-a3c6-6487a6276ab2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: c3e46592bb8760ff85eaeb5dce119897a97dfe89
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57667855"
---
# <a name="net-standard-20-support-in-xamarinforms"></a>Xamarin.Forms で .NET standard 2.0 サポート

_この記事では、.NET Standard 2.0 を使用して、Xamarin.Forms アプリケーションに変換する方法について説明します。_

.NET standard には、すべての .NET 実装で使用可能にすることを意図した .NET Api の仕様を示します。 これにより、します簡単にデスクトップ アプリケーション、モバイル アプリ、ゲームでコードを共有して、さまざまなプラットフォームに同じ Api を導入することでクラウド サービス。 .NET Standard でサポートされるプラットフォームについては、[.NET 実装のサポート](/dotnet/standard/net-standard#net-implementation-support)を参照してください。

.NET standard ライブラリは、ポータブル クラス ライブラリ (PCL) に置換します。 ただし、.NET Standard を対象とするライブラリは、PCL ではまだあり、.NET Standard ベースの PCL と呼びます。 特定の PCL プロファイルは .NET Standard のバージョンにマップされ、マッピングであるプロファイルの場合は、2 つのライブラリの型は相互に参照することになります。 詳細については、[PCL 互換性](/dotnet/standard/net-standard#pcl-compatibility)を参照してください。

Xamarin.Forms 2.4 で .NET Standard 2.0 をターゲットに Xamarin.Forms アプリケーションは、.NET Standard 2.0 ライブラリを PCL に置き換えます。 これは、次のように実現できます。

- 確認[.NET Core 2.0](https://www.microsoft.com/net/download/core)がインストールされています。
- 2.4、以上の Xamarin.Forms を使用する Xamarin.Forms ソリューションを更新します。
- .NET Standard ライブラリを .NET Standard 2.0 を対象とすると、ソリューションに追加します。
- .NET Standard ライブラリに追加されるクラスを削除します。
- Xamarin.Forms 2.4 (またはそれ以上) の NuGet パッケージを .NET Standard ライブラリに追加します。
- プラットフォームのプロジェクトで .NET Standard ライブラリへの参照を追加し、Xamarin.Forms のユーザー インターフェイス ロジックを含む PCL プロジェクトに参照を削除します。
- .NET Standard ライブラリを PCL プロジェクトからファイルをコピーします。
- Xamarin.Forms のユーザー インターフェイス ロジックを含む PCL プロジェクトを削除します。


## <a name="related-links"></a>関連リンク

- [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)
- [コード共有オプション](~/cross-platform/app-fundamentals/code-sharing.md)
