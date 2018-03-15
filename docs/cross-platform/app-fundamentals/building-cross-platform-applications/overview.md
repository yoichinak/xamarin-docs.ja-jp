---
title: "クロス プラットフォーム アプリケーションの概要作成"
ms.topic: article
ms.prod: xamarin
ms.assetid: E442EEFB-FA9C-40E9-9668-5A3F915C8400
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: b46b5f36f0a4d8b4de499511a5d9fed299886006
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2018
---
# <a name="building-cross-platform-applications-overview"></a>クロス プラットフォーム アプリケーションの概要作成

このガイドでは、Xamarin プラットフォーム、およびコードの再利用を最大限活用し、すべての主要なモバイル プラットフォームで高品質のネイティブ エクスペリエンスを提供するクロス プラットフォーム アプリケーションを設計する方法が導入されています。 iOS、Android、Windows Phone です。

このドキュメントで使用するアプローチが生産性アプリとゲームのアプリの両方に該当する通常の生産性とユーティリティ (非ゲーム アプリケーション) にフォーカスがあります。 参照してください、 [MonoGame ドキュメントの概要](https://developer.xamarin.com/guides/cross-platform/game_development/monogame/introduction/)かをチェック アウト[Visual Studio Tools for Unity](https://docs.microsoft.com/en-us/visualstudio/cross-platform/visual-studio-tools-for-unity)クロスプラット フォーム ゲーム開発のガイダンスについてはします。

語句"書き込み-everywhere を実行すると、"のに、使用は、多くの場合、1 つの利点のコードベースの実行は、複数のプラットフォームで変更されていません。 コードの再利用の利点がある、その多くの場合、アプローチを最も一般的な分母の機能セットを持つアプリケーションとが収まらないジェネリック外見のユーザー インターフェイスを適切に、対象プラットフォームのいずれかにします。

Xamarin だけではなく、"書き込み-すべての場所を実行すると、"プラットフォーム、具体的には各プラットフォームのネイティブ ユーザー インターフェイスを実装する機能はその長所の 1 つあるためです。 ただし、共有することもある入念な設計とのほとんどの非ユーザー コードをインターフェイスおよびの両方の長所を取得します。 1 回、データ ストレージおよびビジネス ロジックのコードの記述と、各プラットフォームでネイティブ Ui を提示します。 このドキュメントでは、この目標を達成する一般的なアーキテクチャのアプローチについて説明します。

Xamarin クロスプラット フォーム アプリを作成するため、重要な点の概要を次に示します。

-   **C# を使用して**-C# の場合、アプリを記述します。 C# で記述された既存のコードは、非常に簡単には、Xamarin を使用して Android、iOS に移植し、言うまでもなく、Windows アプリで使用できます。
-   **MVC または MVVVM のデザイン パターンを利用して**-モデル/ビュー/コント ローラー パターンを使用して、アプリケーションのユーザー インターフェイスを開発します。 モデル/ビュー/コント ローラー アプローチまたはモデル View-viewmodel アプローチを使用してアプリケーションを設計者が、「モデル」と残りの部分を明確に区別します。 アプリケーションのどの部分が各プラットフォーム (iOS、Android、Windows、Mac) のネイティブ ユーザー インターフェイス要素を使用し、これを使用して、ガイドラインとして、2 つのコンポーネントを分割を決定します。"Core"と"ユーザー インターフェイス"です。
-   **ネイティブ Ui をビルド**-各 OS 固有のアプリケーションが別のユーザー インターフェイス レイヤー (実装されている c# アシスタンスにより、ネイティブ UI のデザイン ツールの) を提供します。

1.  Ios の場合は、必要に応じて、UI を視覚的に作成するのに Xamarin の iOS デザイナーを使用して、ネイティブのアプリケーションを作成するのに UIKit Api を使用します。
1.  Android では、Xamarin の UI のデザイナーのネイティブのアプリケーションを作成するのに Android.Views を使用します。
1.  Windows で使用する XAML のプレゼンテーション レイヤー、Visual Studio または Blend の UI のデザイナーで作成されました。
1.  Mac で Xcode で作成された、プレゼンテーション層のストーリー ボードを使用します。

Xamarin.Forms プロジェクトでは、すべてのプラットフォームでサポートされておよび Xamarin.Forms XAML を使用してプラットフォーム間で共有できるユーザー インターフェイスを作成すること。 

コードの再利用量はコードの量が保持される共有のコアとコードの量がユーザー インターフェイスの特定に大きく依存します。 主要なコードが、ユーザーと直接やり取りしませんが、代わりに、収集し、この情報を表示するアプリケーションの部分にサービスを提供するものです。

コードの再利用量を増やすなどこれらすべてのシステムで一般的なサービスを提供するプラットフォーム コンポーネントを採用することができます。

1.   [SQLite net](https://www.nuget.org/packages/sqlite-net-pcl/)ローカルの SQL ストレージ
1.   [Xamarin プラグイン](https://xamarin.com/plugins)などのカメラ、連絡先、および地理的位置情報、デバイスに固有の機能にアクセスするため
1.   [NuGet パッケージの](https://nuget.org)など、Xamarin のプロジェクトと互換性がある[Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/)、
1.  ネットワーク、web サービス、IO などの .NET framework の機能を使用します。


これらのコンポーネントの一部として実装された、 *Tasky*ケース スタディです。

 <a name="Separate_Reusable_Code_into_a_Core_Library" />


## <a name="separate-reusable-code-into-a-core-library"></a>再利用可能なコードを分割してコア ライブラリ

によって、アプリケーション アーキテクチャを重ねる移動し、プラットフォームの依存を再利用可能なコア ライブラリには、コア機能の役割の分離の原則に従うと、次の図に、プラットフォームで共有コードを最大化できます。示しています。

 ![](overview-images/layers2.png "によって、アプリケーション アーキテクチャを重ねる移動し、プラットフォームの依存を再利用可能なコア ライブラリには、コア機能の役割の分離の原則に従うと、プラットフォーム間で共有コードを最大化できます。")

 <a name="Case_Studies" />


## <a name="case-studies"></a>ケース スタディ

このドキュメントに付属している 1 つのケース スタディは*Tasky Pro*です。 各ケース スタディでは、実際の例では、このドキュメントに記載されている概念の実装について説明します。 コードはオープン ソースで使用できると[github](https://github.com/xamarin/mobile-samples/)です。