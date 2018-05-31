---
title: .NET Standard
ms.prod: xamarin
ms.assetid: 8C30F8D3-1920-453E-9E8B-D40696736FF2
author: asb3993
ms.author: amburns
ms.date: 04/12/2017
ms.openlocfilehash: c70a1cb1aa05426ba6d54d8af3787f014883bfa1
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/09/2018
ms.locfileid: "33919336"
---
# <a name="net-standard"></a>.NET Standard

## <a name="using-net-standard-library-projects-to-share-code"></a>.NET Standard ライブラリ プロジェクトを使用してコードを共有するには

.NET Standard ライブラリは、すべての .NET ランタイムで使用できるようにすることを目的とした .NET API の正式な仕様です。 .NET Standard ライブラリの背後にある動機は、.NET エコシステムの高度な統一性を確立することです。
[ECMA 335](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/dotnet-standards.md) は引き続き .NET ランタイムの動作の統一性を確立しますが、.NET ライブラリの実装用の .NET 基底クラス ライブラリ (BCL) に同様の仕様はありません。

考えることができますが、シンプルな次世代の[ポータブル クラス ライブラリ](https://msdn.microsoft.com/library/gg597391.aspx)です。
1 つのライブラリで、.NET Core を含むすべての .NET プラットフォームで利用可能なユニフォーム API を持ちます。 1 つの .NET Standard ライブラリを作成し、.NET Standard プラットフォームをサポートする任意のランタイムから使用するだけです。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="visual-studio-for-mac"></a>Visual Studio for Mac

このセクションでは、Visual Studio for Macを使用して.NET Standard ライブラリの作成と使用方法について説明します。 完全な実装については、.NET Standard ライブラリの例のセクションを参照してください。

### <a name="creating-a-net-standard-library"></a>.NET Standard ライブラリの作成

.NET Standard ライブラリをソリューションに追加するのは、比較的簡単です。

1. 新しいプロジェクトの追加ダイアログ ボックスで、`.NET Core`カテゴリし選択`Class Library(.NET Core)`です。

  **注:** このテンプレートの名前は、Visual Studio for Mac の将来のバージョンで `.NET Standard` に変更されます。

  ![.NET Standard ライブラリの作成](net-standard-images/vsm01.png)

2. .NET Standard ライブラリ プロジェクトは、ソリューション エクスプ ローラーで示されているように表示されます。 依存関係ノードは、ライブラリが [NETStandard.Library](https://www.nuget.org/packages/NETStandard.Library/) を使用していることを示します。

  ![ソリューション内の依存関係ノードが .NET Standardを示します](net-standard-images/vsm02.png)

#### <a name="editing-net-standard-library-settings"></a>.NET Standard ライブラリの設定の編集

.NET Standardライブラリの設定は、このスクリーン ショットで示されているように、プロジェクトを右クリックして`Options`を選択すことによって表示および変更できます。

![プロジェクトのオプションでの標準 .NET ターゲット フレームワークを編集します。](net-standard-images/vsm03.png)

バージョンを変更する内部`netstandard`変更することによって、`Target Framework`ドロップダウンの値。

**さらに:** `.csproj`を直接編集してこの値を変更できます。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="visual-studio-2017-windows"></a>Visual Studio 2017 (Windows)

このセクションでは、Visual Studioを使用した.NET Standard ライブラリの作成と使用方法について説明します。 完全な実装については、.NET Standard ライブラリの例のセクションを参照してください。

### <a name="creating-a-net-standard-library"></a>.NET Standard ライブラリの作成

#### <a name="visual-studio-2017"></a>Visual Studio 2017

.NET Standard ライブラリをソリューションに追加するのは、比較的簡単です。

1. 新しいプロジェクトの追加ダイアログ ボックスで、`.NET Standard`カテゴリし選択`Class Library(.NET Standard)`です。

  ![](net-standard-images/vs01.png ".NET Standard クラス ライブラリを新規作成します。")

2. .NET Standard ライブラリ プロジェクトは、ソリューション エクスプ ローラーで示されているように表示されます。 依存関係ノードは、ライブラリが [NETStandard.Library](https://www.nuget.org/packages/NETStandard.Library/) を使用していることを示します。

  ![](net-standard-images/vs02.png "ソリューション内の.NET Standard プロジェクト")

#### <a name="editing-net-standard-library-settings"></a>.NET Standard ライブラリの設定の編集

.NET Standard ライブラリの設定は、このスクリーンショットで示されているように、プロジェクトを右クリックし、`Properties` を選択することによって表示および変更できます。

![](net-standard-images/vs03.png ".NET 標準ライブラリを参照するには、他のプロジェクトと同じ方法")

バージョンを変更する内部`netstandard`変更することによって、`Target Framework`ドロップダウンの値。

**さらに:** `.csproj`を直接編集してこの値を変更できます。

#### <a name="using-net-standard-library"></a>.NET Standard ライブラリを使用します。

.NET Standard ライブラリが作成されたら、通常の参照を追加する同じ方法で任意の互換性のあるアプリケーションまたはライブラリ プロジェクトからへの参照を追加できます。 Visual Studio で参照ノードを右クリックして選択`Add Reference...`に切り替えて、`Solution : Projects`ようにタブします。

![](net-standard-images/vs04.png "Visual Studio で、[参照] ノードを右クリックして参照の追加... しように、ソリューションのプロジェクトのタブに切り替えます")

-----

