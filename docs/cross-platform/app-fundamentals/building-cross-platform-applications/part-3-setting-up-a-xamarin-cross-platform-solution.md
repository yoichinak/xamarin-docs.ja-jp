---
title: "パート 3 - Xamarin クロス プラットフォーム ソリューションを設定します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 4139A6C2-D477-C563-C1AB-98CCD0D10A93
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/27/2017
ms.openlocfilehash: be781ba9457fdb91189cbe33ab9703b9ce8701de
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="part-3---setting-up-a-xamarin-cross-platform-solution"></a>パート 3 - Xamarin クロス プラットフォーム ソリューションを設定します。

Xamarin プロジェクトのすべてを使用しているどのプラットフォームに関係なく、同じソリューション ファイル形式を使用して (Visual Studio **.sln**ファイル形式)。 (Visual Studio for Mac で Windows プロジェクトの場合) など、個々 のプロジェクトを読み込むことができない場合でも、ソリューションを開発環境で共有することができます。



新しいクロス プラットフォーム アプリケーションを作成するとき、最初の手順は、空のソリューションを作成するは。 これは、セクションは次の動作: クロス プラットフォーム モバイル アプリを構築するためのプロジェクトを設定します。

 <a name="Sharing_Code" />


## <a name="sharing-code"></a>コードの共有

参照してください、[共有オプションをコード](~/cross-platform/app-fundamentals/code-sharing.md)プラットフォーム間でコードの共有を実装する方法の詳細については、ドキュメントです。

 <a name="Shared_Asset_Projects" />


### <a name="shared-projects"></a>共有プロジェクト

コード ファイルを共有する最も簡単な方法は、使用、[共有プロジェクト](~/cross-platform/app-fundamentals/shared-projects.md)です。

このメソッドを使用すると、別のプラットフォームのプロジェクト間で同じコードを共有して、コンパイラ ディレクティブを使用して、別のプラットフォーム固有のコード パスをインクルードすることができます。

 <a name="Portable_Class_Libraries" />


### <a name="portable-class-libraries-pcl"></a>ポータブル クラス ライブラリ (PCL)

これまで、特定のフレームワーク バージョンには、.NET プロジェクト ファイル (および結果として得られるアセンブリ) の対象となっています。 これには、プロジェクトまたはさまざまなフレームワークによって共有されているアセンブリができなくなります。

ポータブル クラス ライブラリ (PCL) は、Xamarin.iOS、Xamarin.Android、だけでなく WPF、ユニバーサル Windows プラットフォーム、および Xbox などの異種の CLI プラットフォーム間で使用できるプロジェクトの特殊な型です。 ライブラリは、対象となるプラットフォームによって制限、完全な .NET framework のサブセットをのみ利用できます。

詳細を読み取ることができますの xamarin の[ポータブル クラス ライブラリのサポート](~/cross-platform/app-fundamentals/pcl.md)手順を実行、表示して方法、 [TaskyPortable サンプル](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable)動作します。


### <a name="net-standard"></a>.NET Standard

2016 で導入された[.NET 標準](~/cross-platform/app-fundamentals/net-standard.md)プロジェクトがプラットフォームでは、Windows で使用できるアセンブリを作成、Xamarin プラットフォーム (iOS、Android、Mac)、および Linux の間でコードを共有する簡単な方法を提供します。

.NET 標準ライブラリを作成および (バージョン 1.6 1.0) から各バージョンで利用可能な Api がより簡単に検出され、各バージョンは下位互換性のある点を除いて、Pcl のように使用できるバージョン番号の下限を持つ。



 <a name="Populating_the_Solution" />


## <a name="populating-the-solution"></a>ソリューションを設定します。

コードを共有する方法を使用すると、ソリューションの全体的な構造は、コードを共有するよう促す複数層アーキテクチャを実装する必要があります。
Xamarin のアプローチは、2 種類のプロジェクトにコードをグループには。

-   **コア プロジェクト**– さまざまなプラットフォームで共有する 1 つの場所で再利用可能なコードを記述します。 可能な限り実装の詳細を非表示にするのにには、カプセル化の原則を使用します。
-   **プラットフォーム固有のアプリケーション プロジェクト**– できるだけ小さな結合としてを使用して再利用可能なコードを使用します。 コア プロジェクトで公開されているコンポーネント上に構築された、このレベルでは、プラットフォーム固有の機能が追加されます。


 <a name="Core_Project" />


### <a name="core-project"></a>コア プロジェクト

共有コード プロジェクトには、利用できるすべてのプラットフォームで ie アセンブリのみを参照する必要があります。framework の一般的な名前空間と同様に`System`、`System.Core`と`System.Xml`です。

共有プロジェクトは次のレイヤーを含めることが可能な限り多くの非 UI 機能を実装する必要があります。

-   **データ層**– 物理データ記憶域の例は処理するコードです。  [SQLite NET](https://github.com/praeclarum/sqlite-net)などの代替データベース[Realm.io](https://realm.io/products/realm-mobile-database/)も XML ファイルです。 データ レイヤーのクラスは、通常、データ アクセス レイヤーでのみ使用します。
-   **データ アクセス層**– データ、個々 のデータ項目のリストにアクセスしたり、併せて作成メソッドを編集、および削除など、アプリケーションの機能に必要なデータ操作をサポートする API を定義します。
-   **サービス アクセス層**– クラウドを提供するオプションの層のサービス アプリケーションにします。 リモート ネットワーク リソース (web サービス、イメージをダウンロードしてなど) にアクセスするコードが含まれており、可能性のある結果をキャッシュします。
-   **ビジネス層**– モデル クラスおよびプラットフォーム固有のアプリケーションに機能を公開するファサードまたはマネージャーのクラスの定義。


 <a name="Platform-Specific_Application_Projects" />


### <a name="platform-specific-application-projects"></a>プラットフォーム固有のアプリケーション プロジェクト

プラットフォーム固有のプロジェクトでは、コア共有コード プロジェクトだけでなく、各プラットフォームの SDK (Xamarin.iOS、Xamarin.Android、Xamarin.Mac、または Windows) にバインドするために必要なアセンブリを参照する必要があります。

プラットフォーム固有のプロジェクトを実装する必要があります。

-   **アプリケーション層**-プラットフォーム固有の機能とビジネス層のオブジェクトと、ユーザー インターフェイスの間でバインド/変換します。
-   **ユーザー インターフェイス レイヤー** – 画面、カスタム ユーザー インターフェイス コントロール、検証ロジックのプレゼンテーションです。


<a name="Example" />


### <a name="example"></a>例

アプリケーションのアーキテクチャは、この図に示します。

 [ ![](part-3-setting-up-a-xamarin-cross-platform-solution-images/conceptualarchitecture.png "この図では、アプリケーションのアーキテクチャを示します")](part-3-setting-up-a-xamarin-cross-platform-solution-images/conceptualarchitecture.png)

このスクリーン ショットは、共有の中核となるプロジェクト、iOS と Android アプリケーション プロジェクトをソリューションのセットアップを示しています。 共有プロジェクトには、それぞれのアーキテクチャ レイヤー (Business、サービス、データおよびデータ アクセス コード) に関連するコードが含まれています。

 ![](part-3-setting-up-a-xamarin-cross-platform-solution-images/core-solution-example.png "共有プロジェクトには、それぞれのアーキテクチャ レイヤー (Business、サービス、データおよびデータ アクセス コード) に関連するコードが含まれています。")


 <a name="Project_References" />


## <a name="project-references"></a>プロジェクト参照

プロジェクト参照では、プロジェクトの依存関係を反映します。 Core プロジェクトは、コードを簡単に共有されるように、一般的なアセンブリへの参照を制限します。
プラットフォーム固有のアプリケーション プロジェクトでは、共有コードに加えて、ターゲット プラットフォームを活用するために必要な他のプラットフォーム固有アセンブリを参照します。

アプリケーション プロジェクトの各共有プロジェクト参照し、これらのスクリーン ショットに示すように、ユーザーに現在の機能に必要なユーザー インターフェイスのコードを含めます。

![](part-3-setting-up-a-xamarin-cross-platform-solution-images/solution-android.png "アプリケーションは、共有プロジェクトの参照をプロジェクト") ![ ](part-3-setting-up-a-xamarin-cross-platform-solution-images/solution-ios.png "アプリケーション プロジェクトの共有プロジェクトの参照")


プロジェクトの構築方法の具体的な例については、ケース スタディで表されます。

 <a name="Adding_Files" />


## <a name="adding-files"></a>ファイルを追加します。

 <a name="Build_Action" />


### <a name="build-action"></a>ビルド アクション

これが、特定のファイルの種類の適切なビルド アクションを設定する重要です。 この一覧は、よく使用されるファイル タイプのビルド アクションを示しています。

-  **すべての c# ファイル**– ビルド アクション: コンパイル
-   **Xamarin.iOS & Windows イメージ**– ビルド アクション: コンテンツ
-   **Xamarin.iOS 内 XIB とストーリー ボード ファイル**– ビルド アクション: インタ フェース定義
-   **イメージおよび Android で AXML レイアウト**– ビルド アクション: AndroidResource
-  **Windows プロジェクトでの XAML ファイル**– ビルド アクション: ページ
-  **Xamarin.Forms の XAML ファイル**– ビルド アクション: 埋め込まれたリソース


一般に、IDE は、ファイルの種類を検出し、適切なビルド アクションを推奨します。

 <a name="Case_Sensitivity" />


### <a name="case-sensitivity"></a>大文字と小文字の区別

なお、一部のプラットフォームは、区別するファイル システム (であります。
iOS および Android) ので、必ずを一貫性のあるファイルの名前付け標準を使用し、コードで使用するファイル名が、filesystem を正確に一致するかどうかを確認します。 これは、イメージ、およびコードで参照されているその他のリソースに特に重要です。
