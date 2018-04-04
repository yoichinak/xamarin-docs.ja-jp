---
title: .NET Standard
ms.prod: xamarin
ms.assetid: 8C30F8D3-1920-453E-9E8B-D40696736FF2
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 04/12/2017
ms.openlocfilehash: 20bbb6386de3fb83df22de5df6e9ee99e3913eaa
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="net-standard"></a>.NET Standard

## <a name="using-net-standard-library-projects-to-share-code"></a>.NET 標準ライブラリ プロジェクトを使用してコードを共有するには

.NET Standard Library は、すべての .NET ランタイムで使用できるようにすることを目的とした .NET API の正式な仕様です。 Standard Library の背後にある動機は、.NET エコシステムの高度な統一性を確立することです。
[ECMA 335](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/dotnet-standards.md) は引き続き .NET ランタイムの動作の統一性を確立しますが、.NET ライブラリの実装用の .NET 基底クラス ライブラリ (BCL) に同様の仕様はありません。

考えることができますが、シンプルな次世代の[ポータブル クラス ライブラリ](https://msdn.microsoft.com/library/gg597391.aspx)です。
1 つのライブラリの .NET Core を含むすべての .NET プラットフォーム、統一された API を使用することをお勧めします。 先ほど単一 .NET 標準ライブラリを作成し、.NET 標準プラットフォームをサポートする任意のランタイムから使用します。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="visual-studio-for-mac"></a>Visual Studio for Mac

このセクションで作成および for mac Visual Studio を使用する .NET 標準ライブラリを使用する方法について説明します。 完全な実装について .NET 標準ライブラリの例のセクションを参照してください。

### <a name="creating-a-net-standard-library"></a>.NET 標準ライブラリを作成します。

.NET 標準ライブラリを追加するソリューションには、比較的簡単です。

1. 新しいプロジェクトの追加ダイアログ ボックスで、`.NET Core`カテゴリし選択`Class Library(.NET Core)`です。

  **注:**にこのテンプレートの名前を変更するは`.NET Standard`将来のバージョンの Visual Studio for mac

  ![.NET Core クラス ライブラリを作成します。](net-standard-images/vsm01.png)

2. .NET 標準ライブラリ プロジェクトは、ソリューション エクスプ ローラーで示すように表示されます。 依存関係ノードは、ライブラリを使用する、 [NETStandard.Library](https://www.nuget.org/packages/NETStandard.Library/)です。

  ![ソリューション内の依存関係ノードが .NET 標準を示します](net-standard-images/vsm02.png)

#### <a name="editing-net-standard-library-settings"></a>.NET 標準ライブラリの設定の編集

.NET 標準ライブラリの設定を表示し、プロジェクトを右クリックしを選択すると変更`Options`このスクリーン ショットに示すようにします。

![プロジェクトのオプションでの標準 .NET ターゲット フレームワークを編集します。](net-standard-images/vsm03.png)

バージョンを変更する内部`netstandard`変更することによって、`Target Framework`ドロップダウンの値。

**さらに:**編集することができます、`.csproj`この値を変更するには、直接です。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="visual-studio-2017-windows"></a>Visual Studio 2017 (Windows)

このセクションで作成して Visual Studio を使用する .NET 標準ライブラリを使用する方法について説明します。 完全な実装について .NET 標準ライブラリの例のセクションを参照してください。

### <a name="creating-a-net-standard-library"></a>.NET 標準ライブラリを作成します。

#### <a name="visual-studio-2017"></a>Visual Studio 2017

.NET 標準ライブラリを追加するソリューションには、比較的簡単です。

1. 新しいプロジェクトの追加ダイアログ ボックスで、`.NET Standard`カテゴリし選択`Class Library(.NET Standard)`です。

  ![](net-standard-images/vs01.png "標準的な .NET クラス ライブラリの新規作成します。")

2. .NET 標準ライブラリ プロジェクトは、ソリューション エクスプ ローラーで示すように表示されます。 依存関係ノードは、ライブラリを使用する、 [NETStandard.Library](https://www.nuget.org/packages/NETStandard.Library/)です。

  ![](net-standard-images/vs02.png ".NET 標準的なソリューション内のプロジェクト")

#### <a name="editing-net-standard-library-settings"></a>.NET 標準ライブラリの設定の編集

.NET 標準ライブラリの設定を表示し、プロジェクトを右クリックしを選択すると変更`Properties`このスクリーン ショットに示すようにします。

![](net-standard-images/vs03.png ".NET 標準ライブラリを参照するには、他のプロジェクトと同じ方法")

バージョンを変更する内部`netstandard`変更することによって、`Target Framework`ドロップダウンの値。

**さらに:**編集することができます、`.csproj`この値を変更するには、直接です。

#### <a name="using-net-standard-library"></a>.NET 標準ライブラリを使用します。

.NET 標準ライブラリが作成されたら、通常の参照を追加する同じ方法で任意の互換性のあるアプリケーションまたはライブラリ プロジェクトからへの参照を追加できます。 Visual Studio で参照ノードを右クリックして選択`Add Reference...`に切り替えて、`Solution : Projects`ようにタブします。

![](net-standard-images/vs04.png "Visual Studio で、[参照] ノードを右クリックして参照の追加... しように、ソリューションのプロジェクトのタブに切り替えます")

-----

