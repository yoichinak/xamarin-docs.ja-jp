---
title: Xamarin. Forms での .NET Standard 2.0 のサポート
description: この記事では、.NET Standard 2.0 を使用するように Xamarin アプリケーションを変換する方法について説明します。 .NET Standard は、すべての .NET 実装で使用できるようにすることを目的とした .NET Api の仕様です。
ms.prod: xamarin
ms.assetid: 95805355-63a7-44e7-a3c6-6487a6276ab2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: eaef607e3e6095ccc7dfb1f5831d2384d11cab5f
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70760057"
---
# <a name="net-standard-20-support-in-xamarinforms"></a>Xamarin. Forms での .NET Standard 2.0 のサポート

_この記事では、.NET Standard 2.0 を使用するように Xamarin アプリケーションを変換する方法について説明します。_

.NET Standard は、すべての .NET 実装で使用できるようにすることを目的とした .NET Api の仕様です。 これにより、さまざまなプラットフォームに同一の Api を導入することで、デスクトップアプリケーション、モバイルアプリ、ゲーム、クラウドサービス間でコードを簡単に共有できるようになります。 .NET Standard によってサポートされるプラットフォームの詳細については、「 [.net 実装サポート](/dotnet/standard/net-standard#net-implementation-support)」を参照してください。

.NET Standard ライブラリは、ポータブルクラスライブラリ (PCL) に代わるものです。 ただし、.NET Standard を対象とするライブラリはまだ PCL であり、.NET Standard ベースの PCL と呼ばれます。 特定の PCL プロファイルは .NET Standard バージョンにマップされ、マッピングが設定されているプロファイルでは、2つのライブラリの種類が相互に参照できます。 詳細については、「 [PCL compatibility](/dotnet/standard/net-standard#pcl-compatibility)」を参照してください。

Xamarin 2.4. forms アプリケーションでは、PCL を .NET Standard 2.0 ライブラリに置き換えることによって .NET Standard 2.0 をターゲットにすることができます。 これは、次のように実現できます。

- [.Net Core 2.0](https://www.microsoft.com/net/download/core)がインストールされていることを確認します。
- Xamarin. forms ソリューションを更新して、Xamarin. Forms 2.4 以上を使用するようにします。
- .NET Standard 2.0 を対象とする .NET Standard ライブラリをソリューションに追加します。
- .NET Standard ライブラリに追加されたクラスを削除します。
- .NET Standard ライブラリに Xamarin. Forms 2.4 (またはそれ以上) の NuGet パッケージを追加します。
- プラットフォームプロジェクトで、.NET Standard ライブラリへの参照を追加し、Xamarin. Forms ユーザーインターフェイスロジックを含む PCL プロジェクトへの参照を削除します。
- PCL プロジェクトのファイルを .NET Standard ライブラリにコピーします。
- Xamarin. フォームユーザーインターフェイスロジックを含む PCL プロジェクトを削除します。

## <a name="related-links"></a>関連リンク

- [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)
- [コード共有オプション](~/cross-platform/app-fundamentals/code-sharing.md)
