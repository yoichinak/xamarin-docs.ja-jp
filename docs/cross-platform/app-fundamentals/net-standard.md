---
title: .NET Standard ライブラリを使用してコードを共有するには
description: このドキュメントでは、.NET Standard ライブラリを使用してコードを共有する方法について説明します。 これは、.NET Standard ライブラリの作成、設定を編集およびアプリケーションでの使用について説明します。
ms.prod: xamarin
ms.assetid: 8C30F8D3-1920-453E-9E8B-D40696736FF2
author: conceptdev
ms.author: crdun
ms.custom: video
ms.date: 07/18/2018
ms.openlocfilehash: d07b248b36feee909db9c863eb17f1a900f58e60
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61191361"
---
# <a name="net-standard-library-code-sharing"></a>.NET standard ライブラリ コードの共有

.NET standard ライブラリでは、Xamarin と .NET Core を含むすべての .NET プラットフォームの統一された API があります。 1 つの .NET Standard ライブラリを作成し、.NET Standard プラットフォームをサポートする任意のランタイムからそれを使用します。 参照してください[このグラフ](https://docs.microsoft.com/dotnet/standard/net-standard#net-implementation-support)サポートされているプラットフォームの詳細についてはします。

.NET Standard のバージョン 1.0 から 1.6 まで、.NET Framework のサブセットを段階的に大きいが、.NET Standard 2.0 は、Xamarin アプリケーションや既存のポータブル クラス ライブラリの移植の最高レベルのサポートを提供します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

## <a name="visual-studio-for-mac"></a>Visual Studio for Mac

このセクションでは、Visual Studio for Macを使用して.NET Standard ライブラリの作成と使用方法について説明します。

### <a name="creating-a-net-standard-library"></a>.NET Standard ライブラリの作成

.NET Standard Library は、次の手順でソリューションに追加できます。

1. **新しいプロジェクトの追加**ダイアログ ボックスで、 **.NET Core**カテゴリと、選択 **.NET Standard Library**:

    ![.NET Standard ライブラリを作成する](net-standard-images/vsm01-m157.png "新しい .NET Standard ライブラリを作成します。")

2. 次の画面でのターゲット フレームワークを選択して **.NET Standard 2.0**をお勧めします。

    [![.NET Standard 2.0 を選択します。](net-standard-images/vsm01a-m157-sml.png)](net-standard-images/vsm01a-m157.png#lightbox)

3. 最後の画面で、プロジェクト名を入力し、をクリックして**作成**です。

4. .NET Standard ライブラリ プロジェクトは、ソリューション エクスプ ローラーで示されているように表示されます。 依存関係ノードは、ライブラリが [NETStandard.Library](https://www.nuget.org/packages/NETStandard.Library/) を使用していることを示します。

    ![ソリューション内の依存関係ノードが .NET Standardを示します](net-standard-images/vsm02-m157.png)

#### <a name="editing-net-standard-library-settings"></a>.NET Standard Library の設定の編集

.NET Standard ライブラリの設定は、このスクリーンショットで示されているように、プロジェクトを右クリックし、`Options` を選択することによって表示および変更できます。

![プロジェクト オプションでの .NET Standard のターゲット フレームワークを編集](net-standard-images/vsm03-m157.png "プロジェクト オプションで .NET Standard ターゲット フレームワークのバージョンの編集")

バージョンを変更する内部`netstandard`変更することで、`Target Framework`ドロップダウンの値。

**かつ：** 編集することができます、`.csproj`この値を変更するには、直接します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="visual-studio-2017-windows"></a>Visual Studio 2017 (Windows)

このセクションでは、Visual Studioを使用した.NET Standard ライブラリの作成と使用方法について説明します。

### <a name="creating-a-net-standard-library"></a>.NET Standard ライブラリを作成します。

.NET Standard ライブラリをソリューションに追加するのは、比較的簡単です。

1. **新しいプロジェクト**ダイアログ ボックスで、 **.NET Standard**カテゴリと、選択**クラス ライブラリ (.NET Standard)** します。

    ![新しい .NET Standard クラス ライブラリを作成する](net-standard-images/vs01-w157.png "新しい .NET Standard クラス ライブラリの作成")

2. .NET Standard ライブラリ プロジェクトは、ソリューション エクスプ ローラーで示されているように表示されます。 依存関係ノードは、ライブラリが [NETStandard.Library](https://www.nuget.org/packages/NETStandard.Library/) を使用していることを示します。

    ![プロジェクト フォルダーで NETStandard.Library](net-standard-images/vs02-w157.png "ソリューションでの .NET Standard プロジェクト")

### <a name="editing-net-standard-library-settings"></a>.NET Standard ライブラリの設定の編集

.NET Standard Library の設定を表示および変更でプロジェクトを右クリックし、選択できる**プロパティ**このスクリーン ショットで示すようにします。

![プロジェクトのプロパティでの .NET standard のターゲット フレームワークを編集](net-standard-images/vs03-w157.png "他のプロジェクトと同じように、.NET Standard ライブラリを参照")

**かつ：** 編集することができます、`.csproj`を直接編集する、`TargetFramework`要素で変更されているバージョン (例: 対象となります。 `<TargetFramework>netstandard2.0</TargetFramework>`)

### <a name="using-a-net-standard-library-project"></a>.NET Standard ライブラリ プロジェクトを使用します。

.NET Standard ライブラリが作成されたら、通常の参照を追加する同じ方法で任意の互換性のあるアプリケーションまたはライブラリ プロジェクトからへの参照を追加できます。 Visual Studio で、[参照] ノードを右クリックし、選択**参照の追加.** に切り替えて、**プロジェクト > ソリューション**に示すようにタブします。

![.NET 標準ライブラリを参照する](net-standard-images/vs04.png "Visual Studio で参照ノードを右クリックし、[参照の追加...] のように、ソリューションのプロジェクトタブに切り替えます")

-----

## <a name="net-standard-and-xamarinforms-for-the-net-developer-video"></a>.NET standard と .NET の開発者 (ビデオ) の Xamarin.Forms

> [!Video https://channel9.msdn.com/Shows/XamarinShow/NET-Standard-and-XamarinForms-for-the-NET-Developer/player]

## <a name="related-links"></a>関連リンク

* [.NET standard](https://docs.microsoft.com/dotnet/standard/net-standard) -詳細な情報と PCL を比較します。
