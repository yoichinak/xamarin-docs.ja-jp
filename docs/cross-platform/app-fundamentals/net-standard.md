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
---
# <a name="net-standard"></a>.NET Standard

## <a name="using-net-standard-library-projects-to-share-code"></a>.NET Standard ライブラリ プロジェクトを使用してコードを共有するには

.NET Standard ライブラリは、すべての .NET ランタイムで使用できるようにすることを目的とした .NET API の正式な仕様です。 .NET Standard ライブラリの背後にある動機は、.NET エコシステムの高度な統一性を確立することです。
[ECMA 335](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/dotnet-standards.md) は引き続き .NET ランタイムの動作の統一性を確立しますが、.NET Standard ライブラリの実装用の .NET 基底クラス ライブラリ (BCL) に同様の仕様はありません。

考えることができますが、シンプルな次世代の[ポータブル クラス ライブラリ](https://msdn.microsoft.com/library/gg597391.aspx)です。
1 つのライブラリの .NET Core を含むすべての .NET プラットフォーム、統一された API を使用することをお勧めします。 先ほど単一 .NET Standard ライブラリを作成し、.NET Standard プラットフォームをサポートする任意のランタイムから使用します。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="visual-studio-for-mac"></a>Visual Studio for Mac

このセクションでは、Visual Studio for Macを使用して.NET Standard ライブラリの作成と使用方法について説明します。完全な実装について .NET Standard ライブラリの例のセクションを参照してください。

### <a name="creating-a-net-standard-library"></a>.NET Standard ライブラリを作成します。

.NET Standard ライブラリを追加するソリューションには、比較的簡単です。

1. 新しいプロジェクトの追加ダイアログ ボックスで、`.NET Core`カテゴリし選択`Class Library(.NET Core)`です。

  **注:** にこのテンプレートの名前を変更するは`.NET Standard`将来のバージョンの Visual Studio for Mac

  ![.NET Core クラス ライブラリを作成します。](net-standard-images/vsm01.png)

2. .NET Standard ライブラリ プロジェクトは、ソリューション エクスプ ローラーで示すように表示されます。 依存関係ノードは、ライブラリを使用する、 [NETStandard.Library](https://www.nuget.org/packages/NETStandard.Library/)です。

  ![ソリューション内の依存関係ノードが .NET Standardを示します](net-standard-images/vsm02.png)

#### <a name="editing-net-standard-library-settings"></a>.NET Standard ライブラリの設定の編集

.NET Standardライブラリの設定を表示し、プロジェクトを右クリックしを選択すると変更`Options`このスクリーン ショットに示すようにします。

![プロジェクトのオプションでの標準 .NET ターゲット フレームワークを編集します。](net-standard-images/vsm03.png)

バージョンを変更する内部`netstandard`変更することによって、`Target Framework`ドロップダウンの値。

**さらに:** 編集することができます、`.csproj`この値を変更するには、直接です。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="visual-studio-2017-windows"></a>Visual Studio 2017 (Windows)

このセクションでは、Visual Studioを使用して.NET Standard ライブラリの作成と使用方法について説明します。完全な実装について .NET Standard ライブラリの例のセクションを参照してください。

### <a name="creating-a-net-standard-library"></a>.NET Standard ライブラリを作成します。

#### <a name="visual-studio-2017"></a>Visual Studio 2017

.NET Standard ライブラリを追加するソリューションには、比較的簡単です。

1. 新しいプロジェクトの追加ダイアログ ボックスで、`.NET Standard`カテゴリを選択し`Class Library(.NET Standard)`を選択します。

  ![](net-standard-images/vs01.png ".NET Standard クラス ライブラリの新規作成します。")

2. .NET Standard ライブラリ プロジェクトは、ソリューション エクスプローラーで示すように表示されます。 依存関係ノードは、ライブラリを使用する、 [NETStandard.Library](https://www.nuget.org/packages/NETStandard.Library/)です。

  ![](net-standard-images/vs02.png "ソリューション内の.NET Standard プロジェクト")

#### <a name="editing-net-standard-library-settings"></a>.NET Standard ライブラリの設定の編集

.NET Standard ライブラリの設定を表示し、プロジェクトを右クリックしを選択すると変更`Properties`このスクリーン ショットに示すようにします。

![](net-standard-images/vs03.png ".NET Standard ライブラリを参照するには、他のプロジェクトと同じ方法")

バージョンを変更する内部`netstandard`変更することによって、`Target Framework`ドロップダウンの値。

**さらに:** `.csproj`を編集して直接この値を変更できます。。

#### <a name="using-net-standard-library"></a>.NET Standard ライブラリを使用します。

.NET Standard ライブラリが作成されたら、通常の参照を追加する同じ方法で任意の互換性のあるアプリケーションまたはライブラリ プロジェクトからへの参照を追加できます。 Visual Studio で参照ノードを右クリックして選択`Add Reference...`に切り替えて、`Solution : Projects`ようにタブします。

![](net-standard-images/vs04.png "Visual Studio で、[参照] ノードを右クリックして参照の追加... しように、ソリューションのプロジェクトのタブに切り替えます")

-----

