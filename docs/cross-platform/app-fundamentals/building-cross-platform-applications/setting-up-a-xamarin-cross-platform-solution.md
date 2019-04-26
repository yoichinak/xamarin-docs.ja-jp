---
title: パート 3 - Xamarin クロスプラットフォーム ソリューションのセットアップ
description: このドキュメントでは、Xamarin でクロス プラットフォーム ソリューションを設定する方法について説明します。 これで共有戦略など、さまざまなコードがプロジェクトと .NET Standard を共有します。
ms.prod: xamarin
ms.assetid: 4139A6C2-D477-C563-C1AB-98CCD0D10A93
author: asb3993
ms.author: amburns
ms.date: 03/27/2017
ms.openlocfilehash: f802e31d851915d33cb6dbf5866f8cba3ab90303
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61276628"
---
# <a name="part-3---setting-up-a-xamarin-cross-platform-solution"></a>パート 3 - Xamarin クロスプラットフォーム ソリューションのセットアップ

Xamarin プロジェクトのすべてが同じソリューション ファイルの形式を使用して、使用されているどのようなプラットフォームに関係なく (Visual Studio **.sln**ファイル形式)。 (Visual Studio for Mac で Windows プロジェクトの場合) など、個々 のプロジェクトを読み込むことができない場合でも、ソリューションを開発の環境間で共有できます。



新しいクロス プラットフォーム アプリケーションを作成する場合、最初の手順では、空のソリューションを作成します。 これは、セクションの次の動作: クロス プラットフォーム モバイル アプリを構築するためのプロジェクトを設定します。

 <a name="Sharing_Code" />


## <a name="sharing-code"></a>コードの共有

参照してください、[コード共有オプション](~/cross-platform/app-fundamentals/code-sharing.md)プラットフォーム間でコード共有を実装する方法の詳細についてはドキュメントです。

 <a name="Shared_Asset_Projects" />


### <a name="shared-projects"></a>共有プロジェクト

コード ファイルを共有する最も簡単な方法は、使用、[共有プロジェクト](~/cross-platform/app-fundamentals/shared-projects.md)します。

このメソッドを使用すると、さまざまなプラットフォームのプロジェクトで同じコードを共有して、コンパイラ ディレクティブを使用して、さまざまなプラットフォーム固有のコード パスを含めることができます。

 <a name="Portable_Class_Libraries" />


### <a name="portable-class-libraries-pcl"></a>ポータブル クラス ライブラリ (PCL)

これまで .NET プロジェクト ファイル (および最終的なアセンブリ) の対象となってを特定のフレームワークのバージョン。 これには、プロジェクトまたはさまざまなフレームワークによって共有されているアセンブリができないようにします。

ポータブル クラス ライブラリ (PCL) は、Xamarin.iOS と Xamarin.Android だけでなく WPF、ユニバーサル Windows プラットフォーム、および Xbox などの異種の CLI プラットフォーム全体で使用できるプロジェクトの特殊な型です。 ライブラリは、対象となるプラットフォームによって制限、完全な .NET framework のサブセットをのみ利用できます。

詳細については、Xamarin の[ポータブル クラス ライブラリのサポート](~/cross-platform/app-fundamentals/pcl.md)表示する手順を実行し、方法、 [TaskyPortable サンプル](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable)動作します。


### <a name="net-standard"></a>.NET Standard

2016 年に導入された[.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)プロジェクトは、プラットフォームでは、Windows 間で使用できるアセンブリを作成、Xamarin プラットフォーム (iOS、Android、Mac)、および Linux でコードを共有する簡単な方法を提供します。

.NET standard ライブラリを作成して (バージョン 1.6 1.0) からの各バージョンで利用できる Api がより簡単に検出され、各バージョンは下位互換性のある点を除いて、Pcl のように使用できるバージョン番号の下限を持つ。



 <a name="Populating_the_Solution" />


## <a name="populating-the-solution"></a>ソリューションの作成

コードを共有する際に使用する方法に関係なく、全体的なソリューションの構造は、コードの共有を促進する階層型アーキテクチャを実装する必要があります。
Xamarin アプローチは、2 種類のプロジェクトにコードをグループには。

-   **Core プロジェクト**– さまざまなプラットフォーム間で共有する 1 つの場所で再利用可能なコードを記述します。 可能な場合は、実装の詳細を非表示にするのにには、カプセル化する場合の原則を使用します。
-   **プラットフォーム固有のアプリケーション プロジェクト**– できるだけ小さな結合としてを再利用可能なコードを使用します。 Core プロジェクトで公開されているコンポーネント上に構築された、このレベルでは、プラットフォーム固有の機能が追加されます。


 <a name="Core_Project" />


### <a name="core-project"></a>Core プロジェクト

共有コード プロジェクトには、利用できるすべてのプラットフォームでは – ie アセンブリだけを参照する必要があります。などの一般的なフレームワークの名前空間`System`、`System.Core`と`System.Xml`します。

共有プロジェクトは次のレイヤーを含めることが可能な限り多くの非 UI 機能を実装する必要があります。

-   **データ層**– 物理データ ストレージの次のような処理するコードです。  [SQLite NET](https://github.com/praeclarum/sqlite-net)などの代替データベース[Realm.io](https://realm.io/products/realm-mobile-database/)も XML ファイル。 データ層のクラスは、通常、データ アクセス層でのみ使用します。
-   **データ アクセス層**– アクセス データを個々 のデータ項目のリストを作成しても、メソッドの編集、および削除など、アプリケーションの機能に必要なデータ操作をサポートする API を定義します。
-   **サービスへのアクセス層**– クラウドを提供するオプションの層のサービス アプリケーションにします。 リモート ネットワーク リソース (web サービス、イメージをダウンロードしてなど) にアクセスするコードが含まれ、結果のキャッシュ可能性があります。
-   **ビジネス層**– モデル クラスとプラットフォーム固有のアプリケーションに機能を公開するファサードやマネージャー クラスの定義。


 <a name="Platform-Specific_Application_Projects" />


### <a name="platform-specific-application-projects"></a>プラットフォーム固有のアプリケーション プロジェクト

プラットフォーム固有のプロジェクトでは、コアの共有コード プロジェクトと、各プラットフォームの SDK (Xamarin.iOS、Xamarin.Android、Xamarin.Mac、または Windows) にバインドするために必要なアセンブリを参照する必要があります。

プラットフォーム固有のプロジェクトを実装する必要があります。

-   **アプリケーション層**– プラットフォーム固有の機能とビジネス層のオブジェクトとユーザー インターフェイスの間には、バインド/変換します。
-   **ユーザー インターフェイス層**– 画面、カスタム ユーザー インターフェイス コントロール、検証ロジックのプレゼンテーションです。


<a name="Example" />


### <a name="example"></a>例

この図では、アプリケーションのアーキテクチャを示します。

 [ ![](setting-up-a-xamarin-cross-platform-solution-images/conceptualarchitecture.png "アプリケーションのアーキテクチャは、この図に示します")](setting-up-a-xamarin-cross-platform-solution-images/conceptualarchitecture.png#lightbox)

このスクリーン ショットでは、共有のコア プロジェクト、iOS および Android アプリケーション プロジェクトとソリューションのセットアップを示します。 共有プロジェクトには、各アーキテクチャのレイヤー (ビジネス、サービス、データおよびデータ アクセス コード) に関連するコードが含まれています。

 ![](setting-up-a-xamarin-cross-platform-solution-images/core-solution-example.png "共有プロジェクトには、各アーキテクチャのレイヤー (ビジネス、サービス、データおよびデータ アクセス コード) に関連するコードが含まれています。")


 <a name="Project_References" />


## <a name="project-references"></a>プロジェクトの参照

プロジェクト参照では、プロジェクトの依存関係を反映します。 Core プロジェクトは、コードが簡単に共有できるように、共通のアセンブリへの参照を制限します。
プラットフォーム固有のアプリケーション プロジェクトでは、共有コード、およびターゲット プラットフォームを活用するために必要なその他のすべてのプラットフォーム固有アセンブリを参照します。

アプリケーション プロジェクトの各共有のプロジェクトの参照し、これらのスクリーン ショットに示すように、ユーザーに存在する機能に必要なユーザー インターフェイス コードを含めることが。

![](setting-up-a-xamarin-cross-platform-solution-images/solution-android.png "アプリケーションは、共有プロジェクトの参照をプロジェクト") ![](setting-up-a-xamarin-cross-platform-solution-images/solution-ios.png "アプリケーション プロジェクトの共有プロジェクトの参照")


プロジェクトの構成方法の具体的な例については、ケース スタディで与えられます。

 <a name="Adding_Files" />


## <a name="adding-files"></a>ファイルを追加します。

 <a name="Build_Action" />


### <a name="build-action"></a>ビルド アクション

特定のファイルの種類の正しいビルド アクションを設定するのには重要です。 この一覧は、一般的なファイルの種類のビルド アクションを示しています。

-  **すべてC#ファイル**– ビルド アクション。Compile
-   **Xamarin.iOS & Windows イメージ**– ビルド アクション。Content
-   **Xamarin.iOS の XIB およびストーリー ボード ファイル**– ビルド アクション。インタ フェース定義
-   **イメージと Android で AXML レイアウト**– ビルド アクション。AndroidResource
-  **Windows プロジェクトの XAML ファイル**– ビルド アクション。ページ
-  **Xamarin.Forms XAML ファイル**– ビルド アクション。EmbeddedResource


一般に、IDE は、ファイルの種類を検出し、適切なビルド アクションをお勧めします。

 <a name="Case_Sensitivity" />


### <a name="case-sensitivity"></a>大文字と小文字の区別

最後に、一部のプラットフォームに大文字のファイル システム (例: があることに注意してください。
iOS と Android) あるため、一貫性のあるファイルの名前付け標準を使用し、コードで使用するファイル名が、ファイル システムを正確に一致するかどうかを確認してください。 これは、画像やその他のコードで参照されているリソースの特に重要です。
