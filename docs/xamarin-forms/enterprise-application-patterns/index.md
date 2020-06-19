---
title: Xamarin.Forms 電子ブックを使用するエンタープライズ アプリケーション パターン
description: この電子ブックでは、柔軟で保守性の高い、テスト可能なエンタープライズアプリケーションを開発するためのアーキテクチャガイダンスを提供し Xamarin.Forms ます。
ms.prod: xamarin
ms.assetid: 28cfed6c-6175-4223-a8cc-798d40bf0832
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: cc0f98d94c76ff63e7a002dfb92b51eae153d7b4
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84139400"
---
# <a name="enterprise-application-patterns-using-xamarinforms-ebook"></a>Xamarin.Forms 電子ブックを使用するエンタープライズ アプリケーション パターン

_適応性、保守性、およびテスト可能なエンタープライズアプリケーションを開発するためのアーキテクチャガイダンス Xamarin.Forms_

![](images/cover-sml.png "Enterprise Application Patterns using Xamarin.Forms eBook")

この電子ブックでは、疎結合を維持しながら、モデルビュービューモデル (MVVM) パターン、依存関係の注入、ナビゲーション、検証、および構成管理を実装する方法についてのガイダンスを提供します。 また、認証と承認を実行する方法についても説明します。また、コンテナー化されたマイクロサービスからのデータへのアクセス、および単体テストについても説明します。

## <a name="preface"></a>[「はじめに」](preface.md)

この章では、ガイドの目的と範囲、およびその目的について説明します。

## <a name="introduction"></a>[はじめに](introduction.md)

エンタープライズアプリの開発者は、開発中にアプリのアーキテクチャを変更できるいくつかの課題に直面しています。 そのため、アプリをビルドして、時間の経過と共に変更または拡張できるようにすることが重要です。 このような適応性のための設計は困難ですが、通常はアプリをアプリに簡単に統合できる不連続で疎結合されたコンポーネントにアプリをパーティション分割する必要があります。

## <a name="mvvm"></a>[MVVM](mvvm.md)

モデルビュービューモデル (MVVM) パターンは、アプリケーションのビジネスロジックとプレゼンテーションロジックをユーザーインターフェイス (UI) から明確に分離するのに役立ちます。 アプリケーションロジックと UI を明確に分離することによって、さまざまな開発上の問題に対処し、アプリケーションを簡単にテスト、保守、および進化させることができます。 また、コードの再利用の機会を大幅に向上させることができ、開発者や UI デザイナーはアプリの各部分を開発する際により簡単に共同作業を行うことができます。

## <a name="dependency-injection"></a>[依存性の注入](dependency-injection.md)

依存関係の挿入を使用すると、これらの型に依存するコードから具象型を切り離すことができます。 通常は、インターフェイスと抽象型の間の登録とマッピングのリストを保持するコンテナーと、これらの型を実装または拡張する具象型を使用します。

依存関係挿入コンテナーは、クラスインスタンスをインスタンス化し、コンテナーの構成に基づいて有効期間を管理する機能を提供することで、オブジェクト間の結合を減らします。 オブジェクトの作成中に、コンテナーは、オブジェクトに必要なすべての依存関係を挿入します。 これらの依存関係がまだ作成されていない場合、コンテナーはまず依存関係を作成して解決します。

## <a name="communicating-between-loosely-coupled-components"></a>[疎結合コンポーネント間の通信](communicating-between-loosely-coupled-components.md)

Xamarin.Forms の [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) クラスでは、発行/サブスクライブ パターンが実装され、オブジェクトと型の参照によってリンクしにくいコンポーネント間で、メッセージ ベースの通信を行うことができます。 このメカニズムにより、パブリッシャーとサブスクライバーは相互に参照がなくても通信できるようになり、コンポーネント間の依存関係を軽減しながら、コンポーネントを個別に開発およびテストすることができます。

## <a name="navigation"></a>[ナビゲーション](navigation.md)

Xamarin.Formsでは、ページナビゲーションがサポートされています。これは通常、ユーザーが UI と対話するか、アプリ自体から、ロジックドリブンの状態が変更された結果として発生します。 ただし、MVVM パターンを使用するアプリでは、ナビゲーションが複雑になることがあります。

この章では `NavigationService` 、ビューモデルからビューモデルの最初のナビゲーションを実行するために使用されるクラスについて説明します。 ビューモデルクラスにナビゲーションロジックを配置すると、自動テストでロジックを実行できるようになります。 さらに、ビューモデルは、ナビゲーションを制御するロジックを実装して、特定のビジネスルールを確実に適用することができます。

## <a name="validation"></a>[検証](validation.md)

ユーザーからの入力を受け入れるアプリであれば、入力が有効であることを確認する必要があります。 検証を行わないと、ユーザーはアプリを失敗させるデータを提供できます。 検証では、ビジネスルールを適用し、攻撃者が悪意のあるデータを挿入するのを防ぎます。

モデルビュービューモデル (MVVM) パターンのコンテキストでは、データ検証を実行し、ユーザーが検証エラーを修正できるようにビューの検証エラーを通知するために、ビューモデルまたはモデルが必要になることがよくあります。

## <a name="configuration-management"></a>[構成管理](configuration-management.md)

設定を使用すると、アプリの動作を構成するデータをコードから分離できるため、アプリを再構築しなくても動作を変更できます。 アプリ設定はアプリによって作成および管理されるデータです。ユーザー設定は、アプリの動作に影響するアプリのカスタマイズ可能な設定であり、頻繁に再調整する必要はありません。

## <a name="containerized-microservices"></a>[コンテナー化 Microservices](containerized-microservices.md)

マイクロサービスは、最新のクラウドアプリケーションの機敏性、拡張性、信頼性の要件に適した、アプリケーションの開発とデプロイに対するアプローチを提供します。 マイクロサービスの主な利点の1つは、個別にスケールアウトできることです。これは、需要の増加に対応していないアプリケーションの領域を不必要に拡大することなく、需要をサポートするためにより多くの処理能力やネットワーク帯域幅を必要とする特定の機能領域をスケーリングできることを意味します。

## <a name="authentication-and-authorization"></a>[認証と承認](authentication-and-authorization.md)

Xamarin.FormsASP.NET MVC web アプリケーションと通信するアプリに認証と承認を統合する方法は多数あります。 ここでは、認証と承認は、ユーザー id を使用するコンテナー化された id マイクロサービスを使用して実行されます。 ユーザー Id は、ASP.NET Core Id と統合してベアラートークン認証を実行する ASP.NET Core 用のオープンソース OpenID Connect および OAuth 2.0 フレームワークです。

## <a name="accessing-remote-data"></a>[リモート データへのアクセス](accessing-remote-data.md)

多くの最新の web ベースソリューションでは、web サーバーによってホストされる web サービスを使用して、リモートクライアントアプリケーションの機能を提供しています。 Web サービスが公開する操作は、web API を構成します。また、クライアントアプリは、API が公開するデータや操作の実装方法を把握していなくても、web API を利用できなければなりません。

## <a name="unit-testing"></a>[単体テスト](unit-testing.md)

MVVM アプリケーションからモデルをテストし、モデルを表示することは、他のクラスのテストと同じであり、同じツールと手法を使用できます。 ただし、モデルクラスとビューモデルクラスには、特定の単体テスト手法の恩恵を受けることができるパターンがいくつかあります。

## <a name="feedback"></a>フィードバック

このプロジェクトには、質問を投稿したり、フィードバックを提供したりできるコミュニティサイトがあります。 コミュニティサイトは[GitHub](https://github.com/dotnet-architecture/eShopOnContainers)にあります。 または、電子ブックに関するフィードバックをに電子メールで送信することもでき [dotnet-architecture-ebooks-feedback@service.microsoft.com](mailto:dotnet-architecture-ebooks-feedback@service.microsoft.com) ます。

## <a name="related-links"></a>関連リンク

- [電子ブックのダウンロード (2 Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (サンプル)](https://github.com/dotnet-architecture/eShopOnContainers)
