---
title: Xamarin.Forms 電子を使用してエンタープライズ アプリケーション パターン
description: この電子は適応性、保守、およびテストが容易な Xamarin.Forms のエンタープライズ アプリケーションを開発するためのアーキテクチャに関するガイダンスを提供します。
ms.prod: xamarin
ms.assetid: 28cfed6c-6175-4223-a8cc-798d40bf0832
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: c401465d8a57abe1d5a1cfaf9ee2616888332ea3
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "35242164"
---
# <a name="enterprise-application-patterns-using-xamarinforms-ebook"></a>Xamarin.Forms 電子を使用してエンタープライズ アプリケーション パターン

_適応性、保守、およびテストが容易な Xamarin.Forms のエンタープライズ アプリケーションを開発するためのアーキテクチャに関するガイダンス_

![](images/cover-sml.png "Xamarin.Forms 電子を使用してエンタープライズ アプリケーション パターン")

この電子疎結合を維持しながらモデル View-viewmodel (MVVM) パターン、依存関係の挿入、ナビゲーション、検証、および構成の管理を実装する方法について説明します。 さらもガイダンスにある認証と IdentityServer、コンテナー化 microservices、および単体テストからのデータ アクセスの承認を実行します。

## <a name="prefaceprefacemd"></a>[「はじめに」](preface.md)

この章では、このガイドでは、やの目的は、ユーザーのスコープと目的を説明します。

## <a name="introductionintroductionmd"></a>[はじめに](introduction.md)

エンタープライズ アプリケーションの開発者は、開発時に、アプリのアーキテクチャを変更するいくつかの課題に直面します。 そのため、変更または時間の経過と共に拡張できるようにする、アプリのビルドに重要なです。 このような適応性の設計では、困難ですを指定できますが、通常は、アプリを簡単に統合できるまとめてをアプリに discrete、疎結合のコンポーネントに分割します。

## <a name="mvvmmvvmmd"></a>[MVVM](mvvm.md)

モデル View-viewmodel (MVVM) パターンは、そのユーザー インターフェイス (UI) からのアプリケーションのビジネスやプレゼンテーション ロジックを明確に分離するのに役立ちます。 アプリケーション ロジックと UI の間で明確に分離を維持し、さまざまな開発上の問題に対処するのに役立つ、やすく、アプリケーションのテスト、保守、および進化します。 コードを再利用機会も大幅に向上し、開発者ができるし、それぞれの部品のアプリを開発するときに、UI 設計者は、複数が容易に共同作業します。

## <a name="dependency-injectiondependency-injectionmd"></a>[依存性の注入](dependency-injection.md)

依存関係の挿入は、これらの型に依存するコードの具体的な種類の分離を使用できます。 通常、登録とインターフェイスおよび抽象型は、間のマッピングの一覧を保持するコンテナーと実装またはこれらの型を拡張する具象型を使用します。

依存性の注入コンテナーは、クラスのインスタンスをインスタンス化し、コンテナーの構成に基づいてその有効期間を管理する機能を提供することによってオブジェクト間の結合を削減します。 オブジェクトの作成時に、コンテナーは、そこにオブジェクトを必要とするすべての依存関係を挿入します。 これらの依存関係がまだ作成されていない場合、コンテナーを作成し、その依存関係を最初に解決されます。

## <a name="communicating-between-loosely-coupled-componentscommunicating-between-loosely-coupled-componentsmd"></a>[疎結合コンポーネント間の通信](communicating-between-loosely-coupled-components.md)

Xamarin.Forms [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/)クラスは、発行、実装の定期受信パターン、オブジェクトと型の参照によってリンクするときに便利ではないコンポーネント間のメッセージ ベースの通信を許可します。 このメカニズムにより、パブリッシャーとサブスクライバーは、コンポーネントを個別に開発、テストすることもできますしながら、コンポーネント間の依存関係を削減すること、相互に参照しなくても通信します。

## <a name="navigationnavigationmd"></a>[ナビゲーション](navigation.md)

Xamarin.Forms には、ページ ナビゲーションは、通常、UI でのユーザーの操作とは、アプリ自体のロジックに基づく内部の状態を変更した結果からの結果のサポートが含まれています。 ただし、ナビゲーションを MVVM パターンを使用するアプリケーションで実装する複雑になることができます。

この章では、`NavigationService`クラスは、モデルの表示からビュー モデル優先のナビゲーションを実行するために使用します。 ビューでナビゲーション ロジックを配置するモデルのクラスは、ロジックを自動テストを実行できることを意味します。 さらに、モデルの表示コントロールのナビゲーションに特定のビジネス ルールが適用されていることを確認するためのロジックを実装できます。

## <a name="validationvalidationmd"></a>[検証](validation.md)

すべてのアプリをユーザーからの入力を受け付けるは、入力が有効であることを確認する必要があります。 検証なしで、ユーザーは、アプリが失敗する原因となるデータを指定できます。 検証は、ビジネス ルールの適用し、攻撃者が悪意のあるデータを挿入するを防ぎます。

コンテキストの ViewModel モデル (MVVM) のビュー モデルのパターンまたはモデルがデータ検証を行い、ユーザーが修正できるように、ビューに検証エラーを通知する必要多くの場合があります。

## <a name="configuration-managementconfiguration-managementmd"></a>[構成管理](configuration-management.md)

設定は、アプリをリビルドしなくても変更する動作を許可する、コードからのアプリの動作を構成するデータの分離を許可します。 アプリの設定は、アプリを作成および管理するには、データとユーザー設定は、アプリの動作に影響し、頻繁に再調整を必要としないアプリのカスタマイズ可能な設定です。

## <a name="containerized-microservicescontainerized-microservicesmd"></a>[コンテナー化 Microservices](containerized-microservices.md)

Microservices は、アプリケーションの開発とは、先進的なクラウド アプリケーションのアジリティ、スケール、および信頼性の要件に適しています展開する方法を提供します。 することができましていないスケール アウト、独立している特定の機能領域スケールできます必要な処理電源やネットワーク帯域幅の領域を不必要にスケーリングがない、要求をサポートするためにすることを意味するは microservices の主な利点の 1 つ要求の増加が発生していないアプリケーションです。

## <a name="authentication-and-authorizationauthentication-and-authorizationmd"></a>[認証と承認](authentication-and-authorization.md)

ASP.NET MVC web アプリケーションと通信する Xamarin.Forms アプリへの認証と承認の統合に多くの方法はあります。 ここでは、認証と承認は IdentityServer 4 を使用するコンテナー化 identity マイクロ サービスを実行します。 IdentityServer は、ベアラー トークンの認証を実行する ASP.NET Core Id と統合する ASP.NET core オープン ソースの OpenID Connect および OAuth 2.0 フレームワークです。

## <a name="accessing-remote-dataaccessing-remote-datamd"></a>[リモート データへのアクセス](accessing-remote-data.md)

最新 web ベースのソリューションの多くを行うリモート クライアント アプリケーションの機能を提供する、web サーバーでホストされる web サービスを使用します。 Web サービスによって公開される操作を構成して、web API、およびクライアント アプリは、データまたは API で公開される操作の実装方法がわからなくても、web API を利用できる必要があります。

## <a name="unit-testingunit-testingmd"></a>[単体テスト](unit-testing.md)

モデルおよび MVVM アプリケーションからビューのモデルのテストは、他のクラスをテストして同じ、同じツールと手法を使用できます。 ただしは一部のモデルに一般的なパターンとビューのモデル クラスを特定のユニット テスト手法を活用できます。

## <a name="feedback"></a>フィードバック

このプロジェクトには、コミュニティ サイトのサイトに質問を投稿し、フィードバックを提供します。 コミュニティ サイトがある[GitHub](https://github.com/dotnet-architecture/eShopOnContainers)です。 また、eBook に関するフィードバックを送ることができますにメール送信[ dotnet-architecture-ebooks-feedback@service.microsoft.com](mailto:dotnet-architecture-ebooks-feedback@service.microsoft.com)です。


## <a name="related-links"></a>関連リンク

- [電子 (2 Mb PDF) のダウンロードします。](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (サンプル)](https://github.com/dotnet-architecture/eShopOnContainers)
