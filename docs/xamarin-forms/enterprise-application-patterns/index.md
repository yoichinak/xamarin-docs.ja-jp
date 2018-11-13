---
title: Xamarin.Forms 電子ブックを使用してエンタープライズ アプリケーション パターン
description: この電子ブックは、適応性のある、保守、およびテストが容易な Xamarin.Forms エンタープライズ アプリケーションを開発するためのアーキテクチャに関するガイダンスを提供します。
ms.prod: xamarin
ms.assetid: 28cfed6c-6175-4223-a8cc-798d40bf0832
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: f972a32f8daf920f2121e5aa56923c0f3a7f808a
ms.sourcegitcommit: 849bf6d1c67df943482ebf3c80c456a48eda1e21
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2018
ms.locfileid: "51528443"
---
# <a name="enterprise-application-patterns-using-xamarinforms-ebook"></a>Xamarin.Forms 電子ブックを使用してエンタープライズ アプリケーション パターン

_適応性のある、保守、およびテストが容易な Xamarin.Forms エンタープライズ アプリケーションを開発するためのアーキテクチャに関するガイダンス_

![](images/cover-sml.png "Xamarin.Forms 電子ブックを使用してエンタープライズ アプリケーション パターン")

この電子ブックは、疎結合を維持しながら、モデル-ビュー-ビューモデル (MVVM) パターン、依存関係の挿入、ナビゲーション、検証、および構成の管理を実装する方法のガイダンスを提供します。 さらに、またがガイダンス認証と IdentityServer は、コンテナー化されたマイクロ サービス、および単体テストからのデータ アクセスの承認を実行します。

## <a name="prefaceprefacemd"></a>[「はじめに」](preface.md)

この章では、目的と、ガイド、やの目的は、ユーザーのスコープについて説明します。

## <a name="introductionintroductionmd"></a>[はじめに](introduction.md)

エンタープライズ アプリの開発者は、開発中に、アプリのアーキテクチャを変更するいくつかの課題に直面します。 したがって、変更にまたは時間の経過と共に拡張されるようにアプリをビルドする重要なは。 このような適応性の設計では、困難であり、できますが、通常は、アプリを簡単に統合できるまとめてをアプリに、discrete、疎結合コンポーネントに分割します。

## <a name="mvvmmvvmmd"></a>[MVVM](mvvm.md)

モデル-ビュー-ビューモデル (MVVM) パターンは、そのユーザー インターフェイス (UI) からアプリケーションのビジネスとプレゼンテーション ロジックを明確に分離するのに役立ちます。 アプリケーション ロジックと UI の明確な分離を維持し、開発のさまざまな問題に対処するのに役立つ、し、アプリケーションのテスト、保守、および進化を簡単にします。 コードの再利用機会も大幅に向上し、により、開発者とより UI デザイナーは、それぞれの部品のアプリを開発するときに簡単に共同作業します。

## <a name="dependency-injectiondependency-injectionmd"></a>[依存性の注入](dependency-injection.md)

依存関係の挿入は、これらの型に依存するコードの具体的な種類の分離を使用できます。 通常、登録とインターフェイスと抽象型は、間のマッピングの一覧を保持するコンテナーと実装またはこれらの型を拡張する具象型を使用します。

依存関係注入コンテナーでは、クラスのインスタンスをインスタンス化し、コンテナーの構成に基づいて、有効期間を管理する機能を提供することでオブジェクト間の結合を弱めます。 オブジェクトの作成時に、コンテナーは、これに、オブジェクトが必要なすべての依存関係を挿入します。 これらの依存関係は、まだ作成されていない、コンテナーは作成し、最初にその依存関係を解決します。

## <a name="communicating-between-loosely-coupled-componentscommunicating-between-loosely-coupled-componentsmd"></a>[疎結合コンポーネント間の通信](communicating-between-loosely-coupled-components.md)

Xamarin.Forms [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter)クラスは、発行-サブスクライブ パターンでは、オブジェクトと型を参照することでリンクするは便利ではないコンポーネント間のメッセージ ベースの通信を許可します。 このメカニズムは、コンポーネントを個別に開発してテストできるようになります、コンポーネント間の依存関係を削減すること、相互に参照をしなくても通信するには、パブリッシャーとサブスクライバーを使用します。

## <a name="navigationnavigationmd"></a>[ナビゲーション](navigation.md)

Xamarin.Forms には、ページ ナビゲーションは、通常、UI を使用したユーザーの相互作用やロジックに基づく内部の状態を変更した結果、アプリ自体からの結果のサポートが含まれています。 ただし、ナビゲーションは MVVM パターンを使用するアプリに実装する複雑になることができます。

この章では、`NavigationService`クラスは、ビュー モデルからビュー モデル優先のナビゲーションを実行するために使用します。 ビューでナビゲーション ロジックを配置するモデル クラスは、ロジックを自動テストを実行できることを意味します。 さらに、ビュー モデル コントロールへのナビゲーションを特定のビジネス ルールが適用されていることを確認するためのロジックを実装できます。

## <a name="validationvalidationmd"></a>[検証](validation.md)

ユーザーからの入力を受け取るすべてのアプリでは、入力が有効なことを確認してください。 検証なしでは、ユーザーは、アプリが失敗する原因となったデータを指定できます。 検証は、ビジネス ルールを強制し、攻撃者が悪意のあるデータを挿入することを防止します。

モデル-ビュー-ビューモデル (MVVM) のコンテキストでのパターンをビュー モデルまたはモデルがデータ検証を実行し、ユーザーは、それを解決できるように、ビューに検証エラーを通知する必要な多くの場合は。

## <a name="configuration-managementconfiguration-managementmd"></a>[構成管理](configuration-management.md)

設定は、コードから、アプリの動作を構成するデータの分離を許可する、アプリを再構築せずに変更する動作。 アプリの設定は、アプリを作成および管理するには、データとユーザー設定は、アプリの動作に影響を頻繁に再調整を必要としないアプリのカスタマイズ可能な設定です。

## <a name="containerized-microservicescontainerized-microservicesmd"></a>[コンテナー化 Microservices](containerized-microservices.md)

マイクロ サービスは、アプリケーションの開発と最新のクラウド アプリケーションの機敏性、スケール、および信頼性の要件に適した展開するためのアプローチを提供します。 できますであるスケール アウトされた個別に、つまり、特定の機能領域はスケールされることが必要な処理能力やネットワーク帯域幅の領域を不必要にスケーリングすることがなく、要求をサポートするためにマイクロ サービスの主な利点の 1 つです。要求の増加が発生していないアプリケーションです。

## <a name="authentication-and-authorizationauthentication-and-authorizationmd"></a>[認証と承認](authentication-and-authorization.md)

ASP.NET MVC web アプリケーションと通信する Xamarin.Forms アプリへの認証と承認の統合に多くの方法はあります。 ここでは、認証と承認は、IdentityServer は 4 を使用する識別情報のコンテナー化されたマイクロ サービスで実行されます。 IdentityServer は、オープン ソースの OpenID Connect と OAuth 2.0 ベアラー トークン認証を実行する ASP.NET Core Identity と統合する ASP.NET Core のフレームワークです。

## <a name="accessing-remote-dataaccessing-remote-datamd"></a>[リモート データへのアクセス](accessing-remote-data.md)

Web ベース ソリューションの多くの最新のリモート クライアント アプリケーションの機能を提供する、web サーバーでホストされている、web サービスを使用します。 Web サービスを公開する操作を構成して web API、およびクライアント アプリは、データや、API により公開される操作を実装する方法を知ることがなく、web API を利用できる必要があります。

## <a name="unit-testingunit-testingmd"></a>[単体テスト](unit-testing.md)

モデルと MVVM アプリケーションからビュー モデルのテストは、他のクラスをテストと同じと同じツールと手法を使用できます。 ただしがいくつかのモデルに一般的なパターンとビュー モデル クラスの特定の単体テスト テクニックから利点を得られることができます。

## <a name="feedback"></a>フィードバック

このプロジェクトには、コミュニティ サイト、いる質問を投稿し、フィードバックを提供します。 コミュニティ サイトがある[GitHub](https://github.com/dotnet-architecture/eShopOnContainers)します。 電子ブックに関するフィードバックを送信する代わりに、 [ dotnet-architecture-ebooks-feedback@service.microsoft.com](mailto:dotnet-architecture-ebooks-feedback@service.microsoft.com)します。


## <a name="related-links"></a>関連リンク

- [(2 Mb の PDF) 電子ブックをダウンロードします。](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (サンプル)](https://github.com/dotnet-architecture/eShopOnContainers)
