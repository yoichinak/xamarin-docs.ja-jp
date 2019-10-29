---
title: .NET Standard ライブラリを使用してコードを共有する
description: このドキュメントでは、.NET Standard ライブラリを使用してコードを共有する方法について説明します。 ここでは、.NET Standard ライブラリの作成、その設定の編集、アプリケーションでの使用について説明します。
ms.prod: xamarin
ms.assetid: 8C30F8D3-1920-453E-9E8B-D40696736FF2
author: davidortinau
ms.author: daortin
ms.custom: video
ms.date: 07/18/2018
ms.openlocfilehash: cae59053374f673a56d02e86cd59fb85f313c41b
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016808"
---
# <a name="net-standard-library-code-sharing"></a>.NET Standard ライブラリコード共有

.NET Standard ライブラリには、Xamarin や .NET Core を含むすべての .NET プラットフォーム用の統一された API があります。 1つの .NET Standard ライブラリを作成し、.NET Standard プラットフォームをサポートする任意のランタイムから使用します。 サポートされているプラットフォームの詳細については、[このグラフ](https://docs.microsoft.com/dotnet/standard/net-standard#net-implementation-support)を参照してください。

.NET Standard バージョン 1.0 ~ 1.6 では .NET Framework の段階的な部分のサブセットが提供されますが、.NET Standard 2.0 では Xamarin アプリケーションの最高レベルのサポートが提供され、既存のポータブルクラスライブラリを移植することができます。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

## <a name="visual-studio-for-mac"></a>Visual Studio for Mac

このセクションでは、Visual Studio for Mac を使用して .NET Standard ライブラリを作成して使用する方法について説明します。

### <a name="creating-a-net-standard-library"></a>.NET Standard ライブラリの作成

次の手順に従って、.NET Standard ライブラリをソリューションに追加できます。

1. **[新しいプロジェクトの追加]** ダイアログで、 **[.net Core]** カテゴリを選択し、 **[.NET Standard ライブラリ]** を選択します。

    ![.NET Standard ライブラリを作成する](net-standard-images/vsm01-m157.png "新しい .NET Standard ライブラリを作成する")

2. 次の画面で、ターゲットフレームワーク- **.NET Standard 2.0**を選択することをお勧めします。

    [.NET Standard 2.0 を選択![](net-standard-images/vsm01a-m157-sml.png)](net-standard-images/vsm01a-m157.png#lightbox)

3. 最後の画面で、プロジェクト名を入力し、 **[作成]** をクリックします。

4. ソリューションエクスプローラーに示すように、.NET Standard ライブラリプロジェクトが表示されます。 依存関係ノードは、ライブラリが[Netstandard ライブラリ](https://www.nuget.org/packages/NETStandard.Library/)を使用することを示します。

    ![ソリューションの Dependencies ノードは .NET Standard を示しています](net-standard-images/vsm02-m157.png)

#### <a name="editing-net-standard-library-settings"></a>.NET Standard ライブラリの設定の編集

.NET Standard ライブラリの設定は、次のスクリーンショットに示すように、プロジェクトを右クリックして [`Options`] を選択すると表示および変更できます。

![プロジェクトオプションで .NET Standard のターゲットフレームワークを編集する](net-standard-images/vsm03-m157.png "プロジェクトオプションで .NET Standard ターゲットフレームワークのバージョンを編集する")

内部では、[`Target Framework`] ドロップダウンの値を変更することによって、`netstandard` のバージョンを変更できます。

**さらに:** `.csproj` を直接編集して、この値を変更することができます。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="visual-studio-2017-windows"></a>Visual Studio 2017 (Windows)

このセクションでは、Visual Studio を使用して .NET Standard ライブラリを作成して使用する方法について説明します。

### <a name="creating-a-net-standard-library"></a>.NET Standard ライブラリの作成

.NET Standard ライブラリをソリューションに追加するのは非常に簡単です。

1. **[新しいプロジェクト]** ダイアログで、 **[.NET Standard]** カテゴリを選択し、 **[クラスライブラリ (.NET Standard)]** を選択します。

    ![新しい .NET Standard クラスライブラリの作成](net-standard-images/vs01-w157.png "新しい .NET Standard クラスライブラリの作成")

2. ソリューションエクスプローラーに示すように、.NET Standard ライブラリプロジェクトが表示されます。 依存関係ノードは、ライブラリが[Netstandard ライブラリ](https://www.nuget.org/packages/NETStandard.Library/)を使用することを示します。

    ![プロジェクトフォルダー内の NETStandard ライブラリ](net-standard-images/vs02-w157.png "ソリューション内の .NET Standard プロジェクト")

### <a name="editing-net-standard-library-settings"></a>.NET Standard ライブラリの設定の編集

.NET Standard ライブラリの設定を表示および変更するには、プロジェクトを右クリックし、次のスクリーンショットに示すように **[プロパティ]** を選択します。

![プロジェクトプロパティでの .NET standard ターゲットフレームワークの編集](net-standard-images/vs03-w157.png "他のプロジェクトと同じ方法で .NET Standard ライブラリを参照する")

**さらに:** `.csproj` を直接編集して `TargetFramework` 要素を編集し、対象となるバージョンを変更することができます (例として、 `<TargetFramework>netstandard2.0</TargetFramework>`)

### <a name="using-a-net-standard-library-project"></a>.NET Standard ライブラリプロジェクトの使用

.NET Standard ライブラリを作成したら、通常は参照を追加するのと同じ方法で互換性のあるアプリケーションまたはライブラリプロジェクトから参照を追加できます。 Visual Studio で 参照 ノードを右クリックし、**参照の追加** をクリックします。次に、次のように、**プロジェクト > ソリューション** タブに切り替えます。

![.NET Standard ライブラリの参照](net-standard-images/vs04.png "Visual Studio で、[参照] ノードを右クリックし、[参照の追加] を選択します。次に示すように、[ソリューションプロジェクト] タブに切り替えます。")

-----

## <a name="net-standard-and-xamarinforms-for-the-net-developer-video"></a>.NET 開発者向けの .NET Standard と Xamarin. フォーム (ビデオ)

> [!Video https://channel9.msdn.com/Shows/XamarinShow/NET-Standard-and-XamarinForms-for-the-NET-Developer/player]

## <a name="related-links"></a>関連リンク

* [.NET Standard](https://docs.microsoft.com/dotnet/standard/net-standard) -詳細情報と PCL との比較。
