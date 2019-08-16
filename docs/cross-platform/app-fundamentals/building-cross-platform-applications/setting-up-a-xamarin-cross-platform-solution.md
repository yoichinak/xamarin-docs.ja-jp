---
title: パート 3-Xamarin クロスプラットフォームソリューションの設定
description: このドキュメントでは、Xamarin でクロスプラットフォームソリューションを設定する方法について説明します。 共有プロジェクトや .NET Standard など、さまざまなコード共有方法について説明します。
ms.prod: xamarin
ms.assetid: 4139A6C2-D477-C563-C1AB-98CCD0D10A93
author: asb3993
ms.author: amburns
ms.date: 03/27/2017
ms.openlocfilehash: a33c924df3da8642f4b765868f213e6e196f7866
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2019
ms.locfileid: "69526808"
---
# <a name="part-3---setting-up-a-xamarin-cross-platform-solution"></a>パート 3-Xamarin クロスプラットフォームソリューションの設定

どのプラットフォームが使用されているかにかかわらず、Xamarin プロジェクトはすべて、同じソリューションファイル形式 (Visual Studio **.sln**ファイル形式) を使用します。 ソリューションは、個々のプロジェクトを読み込むことができない場合でも (Visual Studio for Mac の Windows プロジェクトなど)、開発環境間で共有できます。



新しいクロスプラットフォームアプリケーションを作成する場合、最初の手順は空のソリューションを作成することです。 このセクションでは、次の処理について説明します。クロスプラットフォームモバイルアプリをビルドするためのプロジェクトを設定します。

 <a name="Sharing_Code" />


## <a name="sharing-code"></a>コードの共有

プラットフォーム間でコード共有を実装する方法の詳細については、[コード共有オプション](~/cross-platform/app-fundamentals/code-sharing.md)に関するドキュメントを参照してください。

 <a name="Shared_Asset_Projects" />


### <a name="shared-projects"></a>共有プロジェクト

コードファイルを共有するための最も簡単な方法は、[共有プロジェクト](~/cross-platform/app-fundamentals/shared-projects.md)を使用することです。

このメソッドを使用すると、異なるプラットフォームプロジェクト間で同じコードを共有できます。また、コンパイラディレクティブを使用して、プラットフォーム固有のさまざまなコードパスを含めることができます。

 <a name="Portable_Class_Libraries" />


### <a name="portable-class-libraries-pcl"></a>ポータブル クラス ライブラリ (PCL)

従来、.NET プロジェクトファイル (および生成されたアセンブリ) は特定のバージョンのフレームワークを対象としていました。 これにより、プロジェクトまたはアセンブリが異なるフレームワークによって共有されるのを防ぐことができます。

ポータブルクラスライブラリ (PCL) は、特殊な種類のプロジェクトであり、Xamarin、ユニバーサル Windows プラットフォーム、Xbox などのさまざまな CLI プラットフォームで使用することができます。 ライブラリは、完全な .NET framework のサブセットのみを利用できます。対象となるプラットフォームによって制限されます。

Xamarin の[ポータブルクラスライブラリのサポートの](~/cross-platform/app-fundamentals/pcl.md)詳細を確認し、そこに記載されている手順に従って[taskyportable サンプル](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable)がどのように機能するかを確認できます。


### <a name="net-standard"></a>.NET Standard

2016で導入された[.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)プロジェクトを使用すると、プラットフォーム間でコードを簡単に共有でき、Windows、Xamarin プラットフォーム (IOS、Android、Mac)、Linux で使用できるアセンブリが生成されます。

.NET Standard ライブラリは、PCLs のように作成および使用できます。ただし、各バージョン (1.0 から 1.6) で利用可能な Api はより簡単に検出でき、各バージョンは下位バージョンの番号と下位互換性があります。



 <a name="Populating_the_Solution" />


## <a name="populating-the-solution"></a>ソリューションの設定

どのメソッドを使用してコードを共有するかにかかわらず、ソリューションの全体的な構造には、コード共有を促進するレイヤーアーキテクチャが実装されている必要があります。
Xamarin のアプローチでは、コードを次の2つのプロジェクトの種類にグループ化します。

- **コアプロジェクト**-再利用可能なコードを1か所に記述し、異なるプラットフォーム間で共有します。 カプセル化の原則を使用して、可能な限り実装の詳細を非表示にします。
- **プラットフォーム固有のアプリケーションプロジェクト**: 可能な限り少ない結合で再利用可能なコードを使用します。 このレベルでは、プラットフォーム固有の機能が追加されています。コアプロジェクトで公開されているコンポーネントに基づいて構築されています。


 <a name="Core_Project" />


### <a name="core-project"></a>コアプロジェクト

共有コードプロジェクトは、すべてのプラットフォーム (ie) で使用できるアセンブリのみを参照する必要があります。`System`、 、`System.Core`などの共通のフレームワーク名前空間。 `System.Xml`

共有プロジェクトは、可能な限り多くの UI 以外の機能を実装する必要があります。これには、次の層が含まれる可能性があります。

- **データレイヤー** –物理的なデータストレージを扱うコード (  [SQLite-NET](https://github.com/praeclarum/sqlite-net)は、 [Realm.io](https://realm.io/products/realm-mobile-database/) 、偶数の XML ファイルなどの代替データベースです。 データレイヤークラスは、通常、データアクセス層によってのみ使用されます。
- **データアクセス層**–アプリケーションの機能に必要なデータ操作をサポートする API を定義します。たとえば、データのリストにアクセスするためのメソッド、個々のデータ項目、およびそれらを作成、編集、および削除するためのメソッドなどです。
- **サービスアクセスレイヤー** –アプリケーションにクラウドサービスを提供するためのオプションのレイヤーです。 リモートネットワークリソース (web サービス、イメージのダウンロードなど) にアクセスし、場合によっては結果をキャッシュするコードが含まれています。
- **ビジネス層**–モデルクラスの定義と、プラットフォーム固有のアプリケーションに機能を公開するファサードまたはマネージャークラス。


 <a name="Platform-Specific_Application_Projects" />


### <a name="platform-specific-application-projects"></a>プラットフォーム固有のアプリケーションプロジェクト

プラットフォーム固有のプロジェクトは、各プラットフォームの SDK (Xamarin、Xamarin、Android、Xamarin. Mac、または Windows) とコア共有コードプロジェクトにバインドするために必要なアセンブリを参照する必要があります。

プラットフォーム固有のプロジェクトでは、次のものを実装する必要があります。

- **アプリケーションレイヤー** –プラットフォーム固有の機能と、ビジネス層オブジェクトとユーザーインターフェイスの間のバインディング/変換。
- **ユーザーインターフェイスレイヤー** –画面、カスタムユーザーインターフェイスコントロール、検証ロジックの表示。


<a name="Example" />


### <a name="example"></a>例

アプリケーションのアーキテクチャを次の図に示します。

 [![](setting-up-a-xamarin-cross-platform-solution-images/conceptualarchitecture.png "アプリケーションのアーキテクチャを次の図に示します。")](setting-up-a-xamarin-cross-platform-solution-images/conceptualarchitecture.png#lightbox)

このスクリーンショットは、共有コアプロジェクト、iOS、および Android アプリケーションプロジェクトを使用したソリューションのセットアップを示しています。 共有プロジェクトには、各アーキテクチャレイヤー (ビジネス、サービス、データ、およびデータアクセスコード) に関連するコードが含まれています。

 ![](setting-up-a-xamarin-cross-platform-solution-images/core-solution-example.png "共有プロジェクトには、各アーキテクチャレイヤー (ビジネス、サービス、データ、およびデータアクセスコード) に関連するコードが含まれています。")


 <a name="Project_References" />


## <a name="project-references"></a>プロジェクトの参照

プロジェクト参照には、プロジェクトの依存関係が反映されます。 コアプロジェクトは、コードを簡単に共有できるように、共通アセンブリへの参照を制限します。
プラットフォーム固有のアプリケーションプロジェクトは、共有コードと、ターゲットプラットフォームを利用するために必要なその他のプラットフォーム固有のアセンブリを参照します。

アプリケーションは各参照共有プロジェクトを射影し、次のスクリーンショットに示すように、ユーザーに機能を提供するために必要なユーザーインターフェイスコードを含みます。

![](setting-up-a-xamarin-cross-platform-solution-images/solution-android.png "アプリケーションは、共有プロジェクトの参照をプロジェクト") ![](setting-up-a-xamarin-cross-platform-solution-images/solution-ios.png "アプリケーション プロジェクトの共有プロジェクトの参照")


ケーススタディでは、プロジェクトを構造化する方法の具体的な例を示します。

 <a name="Adding_Files" />


## <a name="adding-files"></a>ファイルの追加

 <a name="Build_Action" />


### <a name="build-action"></a>ビルド アクション

特定のファイルの種類に対して適切なビルドアクションを設定することが重要です。 この一覧には、いくつかの一般的なファイルの種類のビルドアクションが表示されます。

- **すべてC#のファイル**–ビルドアクション:コンパイル
- **Xamarin のイメージ & Windows** -ビルドアクション:Content
- **Xamarin の XIB ファイルとストーリーボードファイル**–ビルドアクション:InterfaceDefinition
- **Android でのイメージと AXML レイアウト**–ビルドアクション:AndroidResource
- **Windows プロジェクトの XAML ファイル**–ビルドアクション:ページ
- **Xamarin .xaml XAML ファイル**–ビルドアクション:EmbeddedResource


一般に、IDE はファイルの種類を検出し、正しいビルドアクションを提案します。

 <a name="Case_Sensitivity" />


### <a name="case-sensitivity"></a>大文字と小文字の区別

最後に、一部のプラットフォームでは大文字と小文字が区別されるファイルシステムがあることに注意してください (例:
iOS と Android) のため、必ず一貫性のあるファイル名前付け標準を使用し、コードで使用するファイル名がファイルシステムと完全に一致することを確認してください。 これは、コードで参照するイメージやその他のリソースに特に重要です。
