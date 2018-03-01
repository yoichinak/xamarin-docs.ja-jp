---
title: "Xamarin.Forms で、.NET 標準 2.0 のサポート"
description: "この記事では、.NET 標準 2.0 を使用する Xamarin.Forms アプリケーションに変換する方法について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 95805355-63a7-44e7-a3c6-6487a6276ab2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: 7923ccdf85ffcbbc239df9e9df751f561615baa1
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2018
---
# <a name="net-standard-20-support-in-xamarinforms"></a>Xamarin.Forms で、.NET 標準 2.0 のサポート

_この記事では、.NET 標準 2.0 を使用する Xamarin.Forms アプリケーションに変換する方法について説明します。_

.NET 標準は、すべての .NET 実装で利用できるものでは .NET Api の仕様です。 やすくをデスクトップ アプリケーション、モバイル アプリおよびゲームの間でコードを共有し、同じ Api をさまざまなプラットフォームに戻すことによって、クラウド サービスです。 .NET 標準でサポートされているプラットフォームについては、次を参照してください。 [.NET 実装サポート](/dotnet/standard/net-standard#net-implementation-support/)です。

.NET 標準ライブラリは、代わりのポータブル クラス ライブラリ (PCL) 用です。 ただし、.NET 標準を対象とするライブラリも PCL は、標準 .NET ベースの PCL と呼びます。 特定 PCL プロファイルは標準的な .NET のバージョンにマップされ、プロファイルの割り当てが設定されている場合、2 つのライブラリの型が互いを参照することにあります。 詳細については、次を参照してください。 [PCL 互換性](/dotnet/standard/net-standard#pcl-compatibility)です。

Xamarin.Forms 2.4 では、.NET 標準 2.0 ライブラリ、PCL に置き換えることによって対象 .NET 標準 2.0 Xamarin.Forms アプリケーションができます。 これを行うには、次のようにします。

- 確認[.NET Core 2.0](https://www.microsoft.com/net/download/core)がインストールされています。
- Xamarin.Forms 2.4、または大きい値を使用してにまず Xamarin.Forms ソリューションを更新します。
- .NET 標準ライブラリを .NET 標準 2.0 を対象とすると、ソリューションに追加します。
- .NET 標準ライブラリに追加されるクラスを削除します。
- Xamarin.Forms 2.4 (またはそれ以上) の NuGet パッケージを標準 .NET ライブラリに追加します。
- プラットフォーム プロジェクトでは、標準の .NET ライブラリへの参照を追加し、Xamarin.Forms のユーザー インターフェイス ロジックを含む PCL プロジェクトへの参照を削除します。
- .NET 標準ライブラリを PCL プロジェクトからファイルをコピーします。
- Xamarin.Forms のユーザー インターフェイス ロジックを含む PCL プロジェクトを削除します。


## <a name="related-links"></a>関連リンク

- [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)
- [コード共有のオプション](~/cross-platform/app-fundamentals/code-sharing.md)
