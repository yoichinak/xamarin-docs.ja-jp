---
title: .NET Standard
ms.topic: article
ms.prod: xamarin
ms.assetid: 8C30F8D3-1920-453E-9E8B-D40696736FF2
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 04/12/2017
ms.openlocfilehash: 294d28c57978218986d62d1ee6579e8d283b8f72
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="net-standard"></a>.NET Standard

## <a name="using-net-standard-library-projects-to-share-code"></a>.NET 標準ライブラリ プロジェクトを使用してコードを共有するには

.NET Standard Library は、すべての .NET ランタイムで使用できるようにすることを目的とした .NET API の正式な仕様です。 Standard Library の背後にある動機は、.NET エコシステムの高度な統一性を確立することです。
[ECMA 335](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/dotnet-standards.md) は引き続き .NET ランタイムの動作の統一性を確立しますが、.NET ライブラリの実装用の .NET 基底クラス ライブラリ (BCL) に同様の仕様はありません。

考えることができますが、シンプルな次世代の[ポータブル クラス ライブラリ](https://msdn.microsoft.com/library/gg597391.aspx)です。
1 つのライブラリの .NET Core を含むすべての .NET プラットフォーム、統一された API を使用することをお勧めします。 先ほど単一 .NET 標準ライブラリを作成し、.NET 標準プラットフォームをサポートする任意のランタイムから使用します。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="xamarin-studio"></a>Xamarin Studio

最初に、ポータブル ライブラリ プロジェクトを作成することで Xamarin Studio 6.2 では、標準的な .NET のライブラリ プロジェクトを作成できます。

[ ![](net-standard-images/xs01-sml.png "新しいポータブル ライブラリ プロジェクトを作成します。")](net-standard-images/xs01.png)

プロジェクトが作成されたら、右クリックし、開く、**プロジェクト オプション**ウィンドウです。
**全般**プロジェクトを .NET 標準に変換しで特定のバージョンを使用する設定セクション、**プラットフォーム**ドロップ ダウン リスト。

[ ![](net-standard-images/xs02-sml.png "一般にオプションで .NET 標準に変換します。")](net-standard-images/xs02.png)

できます[NuGet パッケージを作成する](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/existing-library.md)を他の開発者とライブラリを共有します。

## <a name="visual-studio-for-mac-walkthrough"></a>Visual Studio for Mac のチュートリアル

このセクションで作成および for mac Visual Studio を使用する .NET 標準ライブラリを使用する方法について説明します。 完全な実装について .NET 標準ライブラリの例のセクションを参照してください。

### <a name="creating-a-net-standard-library"></a>.NET 標準ライブラリを作成します。

.NET 標準ライブラリを追加するソリューションには、比較的簡単です。

1. 新しいプロジェクトの追加 ダイアログ ボックスで、`.NET Core`カテゴリし選択`Class Library(.NET Core)`です。

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

## <a name="visual-studio-windows-walkthrough"></a>Visual Studio (Windows) のチュートリアル

このセクションで作成して Visual Studio を使用する .NET 標準ライブラリを使用する方法について説明します。 完全な実装について .NET 標準ライブラリの例のセクションを参照してください。

### <a name="creating-a-net-standard-library"></a>.NET 標準ライブラリを作成します。

#### <a name="visual-studio-2017"></a>Visual Studio 2017

.NET 標準ライブラリを追加するソリューションには、比較的簡単です。

1. 新しいプロジェクトの追加 ダイアログ ボックスで、`.NET Standard`カテゴリし選択`Class Library(.NET Standard)`です。

  ![](net-standard-images/vs01.png "標準的な .NET クラス ライブラリの新規作成します。")

2. .NET 標準ライブラリ プロジェクトは、ソリューション エクスプ ローラーで示すように表示されます。 依存関係ノードは、ライブラリを使用する、 [NETStandard.Library](https://www.nuget.org/packages/NETStandard.Library/)です。

  ![](net-standard-images/vs02.png ".NET 標準的なソリューション内のプロジェクト")

#### <a name="editing-net-standard-library-settings"></a>.NET 標準ライブラリの設定の編集

.NET 標準ライブラリの設定を表示し、プロジェクトを右クリックしを選択すると変更`Properties`このスクリーン ショットに示すようにします。

![](net-standard-images/vs03.png ".NET 標準ライブラリを参照するには、他のプロジェクトと同じ方法")

バージョンを変更する内部`netstandard`変更することによって、`Target Framework`ドロップダウンの値。

**さらに:**編集することができます、`.csproj`この値を変更するには、直接です。

#### <a name="using-net-standard-library"></a>.NET 標準ライブラリを使用します。

.NET 標準ライブラリが作成されたら、通常の参照を追加する同じ方法で任意の互換性のあるアプリケーションまたはライブラリ プロジェクトからへの参照を追加できます。 Visual Studio で参照ノードを右クリックして選択`Add Reference...`に切り替えて、`Solution : Projects`ように タブします。

![](net-standard-images/vs04.png "Visual Studio で、[参照] ノードを右クリックして参照の追加... しように、ソリューションのプロジェクトのタブに切り替えます")

-----


## <a name="related-links"></a>関連リンク

- [リリース ノート](https://developer.xamarin.com/releases/studio/xamarin.studio_6.2/xamarin.studio_6.2/#.NET_Standard_Support)
