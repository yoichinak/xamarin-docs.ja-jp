---
title: クロスプラットフォームアプリケーションの構築の概要
description: このドキュメントでは、クロスプラットフォームアプリケーションの構築の概要について説明します。 ここでは、C# の値、MVC/MVVM、ネイティブ Ui などの設計パターンについて説明します。
ms.prod: xamarin
ms.assetid: E442EEFB-FA9C-40E9-9668-5A3F915C8400
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
ms.openlocfilehash: bdf641ca15919958627850e000c117eeedd12de9
ms.sourcegitcommit: 836d54779190b1bef1b43bc0c2016c9b3034bfda
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2020
ms.locfileid: "93281249"
---
# <a name="building-cross-platform-applications-overview"></a>クロスプラットフォームアプリケーションの構築の概要

このガイドでは、Xamarin プラットフォームを紹介し、クロスプラットフォームアプリケーションを設計してコードの再利用を最大化し、メインのすべてのモバイルプラットフォーム (iOS、Android、および Windows Phone) で高品質のネイティブエクスペリエンスを提供する方法について説明します。

このドキュメントで使用されるアプローチは、一般に生産性向上アプリとゲームアプリの両方に適用されます。ただし、この方法は生産性とユーティリティ (ゲーム以外のアプリケーション) に重点を置いています。 クロスプラットフォームゲーム開発ガイダンスについては、「 [Visual Studio Tools for Unity](/visualstudio/cross-platform/visual-studio-tools-for-unity) 」を参照してください。

"Write-once, すべての場所で実行する" という語句は、多くの場合、複数のプラットフォームで未変更で実行される単一のコードベースの多くを extol するために使用されます。 コードを再利用するという利点がありますが、このアプローチでは、共通の分母の特徴セットを持つアプリケーションや、ターゲットプラットフォームに適切に適合しない汎用的なユーザーインターフェイスを使用することがよくあります。

Xamarin は、プラットフォームごとに固有のネイティブユーザーインターフェイスを実装できるという点で、"1 回限りの書き込み、どこでも実行" というプラットフォームではありません。 しかし、わかりやすい設計では、ほとんどの非ユーザーインターフェイスコードを共有して、両方の長所を最大限に活用することができます。1回のデータストレージとビジネスロジックコードを記述し、各プラットフォームにネイティブ Ui を提供します。 このドキュメントでは、この目標を達成するための一般的なアーキテクチャアプローチについて説明します。

Xamarin クロスプラットフォームアプリを作成するための主要なポイントの概要を次に示します。

- **C# を使用する** -アプリを c# で記述します。 C# で記述された既存のコードは、Xamarin を使用して iOS および Android に簡単に移植でき、Windows アプリでも使用できます。
- **MVC または MVVM のデザインパターンを利用** する-モデル/ビュー/コントローラーパターンを使用してアプリケーションのユーザーインターフェイスを開発します。 モデル/ビュー/コントローラーアプローチを使用するか、"モデル" と rest の間に明確な分離があるモデル/ビュー/ビューモデルの方法を使用して、アプリケーションを設計します。 アプリケーションのどの部分が各プラットフォームのネイティブユーザーインターフェイス要素 (iOS、Android、Windows、Mac) を使用するかを決定し、これをガイドラインとして使用して、"Core" と "User Interface" の2つのコンポーネントにアプリケーションを分割します。
- **ネイティブ** Ui のビルド-OS 固有の各アプリケーションには、さまざまなユーザーインターフェイスレイヤーが用意されています (ネイティブ UI デザインツールを使用して C# で実装されています)。

1. IOS では、UIKit Api を使用してネイティブアプリケーションを作成し、必要に応じて Xamarin の iOS デザイナーを利用して UI を視覚的に作成できます。
1. Android では、Xamarin の UI デザイナーを利用して、ネイティブなアプリケーションを作成するために Android を使用します。
1. Windows では、Visual Studio または Blend の UI デザイナーで作成されたプレゼンテーション層に XAML を使用します。
1. Mac では、Xcode で作成されたプレゼンテーション層にストーリーボードを使用します。

Xamarin. Forms プロジェクトはすべてのプラットフォームでサポートされており、Xamarin を使用してプラットフォーム間で共有できるユーザーインターフェイスを作成することができます。

コードの再使用量は、共有コアに保持されるコードの量と、ユーザーインターフェイスに固有のコードの量に大きく左右されます。 コアコードは、ユーザーと直接やり取りしないものですが、代わりに、この情報を収集して表示するアプリケーションの一部のサービスを提供します。

コードの再利用量を増やすために、次のようなすべてのシステムで共通のサービスを提供するクロスプラットフォームコンポーネントを採用できます。

1. ローカル SQL ストレージの場合は[SQLite-net](https://www.nuget.org/packages/sqlite-net-pcl/) 、
1. カメラ、連絡先、位置情報など、デバイス固有の機能にアクセスするための[Xamarin プラグイン](https://xamarin.com/plugins)
1. [Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/)など、Xamarin プロジェクトと互換性のある[NuGet パッケージ](https://nuget.org)
1. ネットワーク、web サービス、IO などに .NET framework の機能を使用します。

これらのコンポーネントの一部は、 *Tasky* のケーススタディで実装されています。

 <a name="Separate_Reusable_Code_into_a_Core_Library"></a>

## <a name="separate-reusable-code-into-a-core-library"></a>再利用可能なコードをコアライブラリに分離する

アプリケーションアーキテクチャを階層化し、プラットフォームに依存しないコア機能を再利用可能なコアライブラリに移動することによって、役割を分離するという原則に従うことで、次の図に示すように、プラットフォーム間でコードを共有できます。

 ![アプリケーションアーキテクチャを階層化し、プラットフォームに依存しないコア機能を再利用可能なコアライブラリに移動することで、責任の分離の原則に従うことで、プラットフォーム間でのコード共有を最大化できます。](overview-images/layers2.png)

 <a name="Case_Studies"></a>

## <a name="case-studies"></a>ケース スタディ

このドキュメントには、 *Tasky Pro* という1つのケーススタディが付属しています。 各ケーススタディでは、このドキュメントに記載されている概念の実装について、実際の例を通じて説明します。 このコードはオープンソースであり、 [github](https://github.com/xamarin/mobile-samples/)で入手できます。
