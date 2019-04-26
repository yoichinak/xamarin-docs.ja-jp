---
title: エンタープライズ アプリの開発の概要
description: この章では、エンタープライズ アプリの開発の概要について説明し、eShopOnContainers のモバイル アプリを紹介します。
ms.prod: xamarin
ms.assetid: cbce0659-fa03-447a-86ec-140438143230
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 9deb685c92092ceb0e1c775a1e53ac1bce5a4a57
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61299993"
---
# <a name="introduction-to-enterprise-app-development"></a>エンタープライズ アプリの開発の概要

プラットフォームにかかわらずエンタープライズ アプリの開発者はいくつかの課題に直面します。

-   時間の経過と共に変更できるアプリの要件。
-   新しいビジネス チャンス、課題です。
-   スコープと、アプリの要件に大きく影響する開発中に継続的なフィードバック。

これらを考慮してが簡単に変更または時間の経過と共に拡張が可能なアプリの構築に重要です。 アーキテクチャを個別に開発して、アプリの残りの部分に影響を与えずに分離してテスト アプリの個々 の部分を許可する必要がある、このような適応性の設計が難しいことができます。

多くのエンタープライズ アプリは、複数の開発者を要求するように十分に複雑です。 複数の開発者が作業できるようにしない効果的に、アプリの異なる部分に対して個別に、こと部分一体とシームレスに統合すると、アプリに確保しながら、アプリを設計する方法を決定する重要な課題になります。

設計とアプリでどのような結果を構築するには、従来のアプローチと呼ばれます、*モノリシック*コンポーネント密にクリア分離せずに結合されているアプリ。 通常、このモノリシック手法があるアプリケーションへは難しく、維持するため、非効率的なアプリでは、その他のコンポーネントを損なうことがなく、バグを解決することは難しいため、潜在顧客し、新しい機能を追加または既存の機能を置き換えるには難しい可能性があります。

これらの課題の効果的なの救済手段では、アプリを簡単に統合できるまとめてをアプリに、discrete、疎結合コンポーネントに分割します。 このアプローチには、いくつかの利点があります。

-   これにより、開発、テスト、拡張、およびさまざまな個人やチームによって管理されるに個々 の機能です。
-   再利用との間で認証とデータのアクセスなど、アプリの水平方向の機能とアプリの特定のビジネス機能など、垂直方向の機能の問題を明確に分離を昇格します。 これにより、依存関係とより簡単に管理するアプリのコンポーネント間の相互作用します。
-   により、特定のタスクまたはその専門知識に従って機能の一部に注目するには、別の個人やチームの役割の分離を維持することができます。 具体的には、ユーザー インターフェイスと、アプリのビジネス ロジックを明確に区別を提供します。

ただし、discrete、疎結合コンポーネントにアプリをパーティション分割するときに解決する必要がある多くの問題があります。 不足している機能には次が含まれます。

-   ユーザー インターフェイス コントロールとそのロジックの間の懸念事項の明確な分離を提供する方法を決定します。 Xamarin.Forms エンタープライズ アプリを作成するときの最も重要な決定事項の 1 つが、分離コード ファイルにビジネス ロジックを配置するかどうか、またはを明確により、アプリを作成、ロジックとユーザー インターフェイス コントロール間の懸念事項の分離を作成するかどうか管理およびテストします。 詳細については、次を参照してください。[モデル-ビュー-ビューモデル](~/xamarin-forms/enterprise-application-patterns/mvvm.md)します。
-   依存関係の注入コンテナーを使用するかどうかを決定します。 依存関係注入コンテナーでは、依存関係の挿入、関係を含むクラスのインスタンスを構築する機能を提供することでオブジェクト間の結合を削減し、コンテナーの構成に基づいて、有効期間を管理します。 詳細については、次を参照してください。[依存関係の注入](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md)します。
-   プラットフォームで提供されるイベントの間、緩やかなを選択するには、オブジェクトと型を参照することでリンクするは便利ではないコンポーネント間の通信のメッセージに基づく結合されています。 詳細については、概要を参照してください。[通信コンポーネント間の疎結合](~/xamarin-forms/enterprise-application-patterns/communicating-between-loosely-coupled-components.md)します。
-   ナビゲーションを呼び出す方法など、ページ間を移動する方法を決定し、ナビゲーション ロジックが存在する必要があります。 詳細については、「[ナビゲーション](~/xamarin-forms/enterprise-application-patterns/navigation.md)」を参照してください。
-   ユーザー入力が正しいことを検証する方法を決定します。 意思決定には、ユーザーの入力を検証する方法と、検証エラーをユーザーに通知する方法を含める必要があります。 詳細については、次を参照してください。[検証](~/xamarin-forms/enterprise-application-patterns/validation.md)です。
-   認証を実行する方法と権限を持つリソースを保護する方法を決定します。 詳細については、次を参照してください。[認証と承認](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md)します。
-   Web からリモート データにアクセスする方法を決定するサービスは、データをキャッシュする方法と、データを確実に取得する方法などです。 詳細については、次を参照してください。[にアクセスするリモート データ](~/xamarin-forms/enterprise-application-patterns/accessing-remote-data.md)します。
-   アプリをテストする方法を決定します。 詳細については、次を参照してください。 [Unit Testing](~/xamarin-forms/enterprise-application-patterns/unit-testing.md)します。

このガイドでは、これらの問題のガイダンスを提供し、Xamarin.Forms を使用したクロスプラット フォーム対応のエンタープライズ アプリを構築するためのアーキテクチャと主要なパターンに重点を置いています。 ガイドの目的は、一般的な Xamarin.Forms エンタープライズ アプリの開発シナリオをアドレス指定し、プレゼンテーション、プレゼンテーション ロジック、およびサポート エンティティの懸念事項を分離することで、適応性のある、管理、およびテストのコードを生成するために、モデル-ビュー-ビューモデル (MVVM) パターンです。

## <a name="sample-application"></a>サンプル アプリケーション

このガイドには、eShopOnContainers で、次の機能が含まれているオンライン ストアである、サンプル アプリケーションが含まれています。

-   認証とバックエンド サービスに対して承認します。
-   シャツ、コーヒー マグ カップ、およびその他の商品のカタログを参照します。
-   カタログをフィルター処理します。
-   カタログの項目の順序付けします。
-   ユーザーの注文履歴を表示します。
-   設定の構成。

### <a name="sample-application-architecture"></a>サンプル アプリケーションのアーキテクチャ

図 1-1 では、サンプル アプリケーションのアーキテクチャの概要を示します。

![](introduction-images/architecture.png "eShopOnContainers のアーキテクチャの概要")

**図 1-1**: eShopOnContainers アーキテクチャの概要

サンプル アプリケーションは、次の 3 つのクライアント アプリが付属しています。

-   ASP.NET core MVC アプリケーションが開発しました。
-   Angular 2 と Typescript を使用して、シングル ページ アプリケーション (SPA) 開発します。 各操作で、サーバーへのラウンド トリップを実行する web アプリケーションには、このアプローチを回避できます。
-   IOS、Android、およびユニバーサル Windows プラットフォーム (UWP) をサポートする Xamarin.Forms を使用してモバイル アプリを開発しました。

Web アプリケーションについては、次を参照してください。 [Architecting と ASP.NET Core と Microsoft Azure で最新の Web アプリケーションの開発](http://aka.ms/WebAppEbook)します。

サンプル アプリケーションには、次のバックエンド サービスが含まれます。

-   Id マイクロ サービス、ASP.NET Core Identity と IdentityServer を使用しています。
-   データに基づく作成、読み取り、カタログ マイクロ サービスは、更新、および entity Framework Core を使用して、SQL Server データベースを使用する (CRUD) サービスを削除します。
-   順序付けマイクロ サービスをドメイン駆動設計パターンを使用してドメイン ベースのサービスであります。
-   買い物かごマイクロ サービス、Redis Cache を使用するデータ ドリブン CRUD サービスであります。

これらのバックエンド サービスは、ASP.NET Core MVC を使用してマイクロ サービスとして実装され、単一の Docker ホスト内で一意のコンテナーとして展開されます。 総称して、eShopOnContainers 参照アプリケーションとして、これらのバックエンド サービスに呼ばれます。 クライアント アプリは、Representational State Transfer (REST) web インターフェイスでバックエンド サービスと通信します。 マイクロ サービスと Docker の詳細については、次を参照してください。 [: コンテナー化されたマイクロ サービス](~/xamarin-forms/enterprise-application-patterns/containerized-microservices.md)します。

バックエンド サービスの実装については、次を参照してください。 [.NET マイクロ サービス。Architecture for Containerized .NET Applications](https://aka.ms/microservicesebook)」(.NET マイクロサービス: コンテナー化された .NET アプリケーションのアーキテクチャ) を参照してください。

### <a name="mobile-app"></a>モバイル アプリ

このガイドでは、Xamarin.Forms を使用してクロスプラット フォーム対応のエンタープライズ アプリの構築に焦点を当てていて、例として、eShopOnContainers のモバイル アプリを使用します。 図 1-2 には、先に説明した機能を提供する eShopOnContainers のモバイル アプリからページが表示されます。

[![](introduction-images/screenshots.png "EShopOnContainers のモバイル アプリ")](introduction-images/screenshots-large.png#lightbox "eShopOnContainers のモバイル アプリ")

**図 1-2**:EShopOnContainers のモバイル アプリ

モバイル アプリでは、eShopOnContainers 参照アプリケーションが提供するバックエンド サービスを消費します。 ただし、バックエンド サービスのデプロイを回避したい方のためのモック サービスからデータを使用するよう構成できます。

EShopOnContainers のモバイル アプリでは、Xamarin.Forms の次の機能を実行します。

-   XAML
-   コントロール
-   バインディング
-   コンバーター
-   スタイル
-   Animations
-   コマンド
-   ビヘイビアー
-   トリガー
-   効果
-   カスタム レンダラー
-   MessagingCenter
-   カスタム コントロール

この機能の詳細については、次を参照してください。、 [Xamarin.Forms ドキュメント](~/xamarin-forms/index.yml)、および[を Xamarin.Forms での Mobile Apps の作成](https://aka.ms/xamebook)です。

さらに、単体テストは、eShopOnContainers のモバイル アプリ内のクラスのいくつか提供されます。

#### <a name="mobile-app-solution"></a>モバイル アプリのソリューション

EShopOnContainers のモバイル アプリのソリューションでは、ソース コードとその他のリソースをプロジェクトに整理します。 すべてのプロジェクトは、フォルダーを使用して、カテゴリに、ソース コードとその他のリソースを整理します。 次の表に、eShopOnContainers のモバイル アプリを構成するプロジェクト。

|プロジェクト|説明|
|--- |--- |
|eShopOnContainers.Core|このプロジェクトは、共有コードと共有 UI を含むポータブル クラス ライブラリ (PCL) プロジェクトです。|
|eShopOnContainers.Droid|このプロジェクトは、Android 固有のコードを保持して、Android アプリのエントリ ポイントです。|
|eShopOnContainers.iOS|このプロジェクトは、iOS 固有のコードを保持しているし、for iOS アプリのエントリ ポイントです。|
|eShopOnContainers.UWP|このプロジェクトは、ユニバーサル Windows プラットフォーム (UWP) 固有のコードを保持して、Windows アプリのエントリ ポイントです。|
|eShopOnContainers.TestRunner.Droid|このプロジェクトは eShopOnContainers.UnitTests プロジェクト用の Android のテスト ランナーです。|
|eShopOnContainers.TestRunner.iOS|このプロジェクトは、iOS テスト ランナー eShopOnContainers.UnitTests プロジェクトです。|
|eShopOnContainers.TestRunner.Windows|このプロジェクトは eShopOnContainers.UnitTests プロジェクトのユニバーサル Windows プラットフォームのテスト ランナーです。|
|eShopOnContainers.UnitTests|このプロジェクトには、eShopOnContainers.Core プロジェクトの単体テストが含まれています。|

EShopOnContainers のモバイル アプリからのクラスは、ほとんどまたはまったく変更をすべての Xamarin.Forms アプリで再利用できます。

##### <a name="eshoponcontainerscore-project"></a>eShopOnContainers.Core プロジェクト

EShopOnContainers.Core PCL プロジェクトには、次のフォルダーが含まれています。

|フォルダー|説明|
|--- |--- |
|Animations|XAML で使用するアニメーションを有効にするクラスが含まれています。|
|ビヘイビアー|クラスを表示するのには、動作が含まれています。|
|コントロール|アプリで使用されるカスタム コントロールが含まれています。|
|コンバーター|バインディングにカスタム ロジックを適用する値コンバーターが含まれています。|
|効果|含まれています、`EntryLineColorEffect`クラスは、特定の罫線の色を変更するために使用`Entry`コントロール。|
|例外|ユーザー設定を含む`ServiceAuthenticationException`します。|
|拡張機能|拡張メソッドを格納、`VisualElement`と`IEnumerable`クラス。|
|ヘルパー|アプリのヘルパー クラスが含まれています。|
|モデル|アプリのモデル クラスが含まれています。|
|プロパティ|含む`AssemblyInfo.cs`、.NET アセンブリのメタデータ ファイル。|
|Services|アプリに提供されるサービスを実装するインターフェイスとクラスが含まれています。|
|トリガー|含まれています、`BeginAnimation`トリガーで、XAML でアニメーションを起動するために使用します。|
|検証|データ入力の検証に関連するクラスが含まれています。|
|ViewModels|ページに公開されているアプリケーションのロジックが含まれています。|
|Views|アプリのページが含まれています。|

##### <a name="platform-projects"></a>プラットフォームのプロジェクト

プラットフォームのプロジェクトには、効果の実装、カスタム レンダラーの実装、およびその他のプラットフォームに固有のリソースが含まれます。

## <a name="summary"></a>まとめ

Xamarin のクロスプラット フォーム モバイル アプリ開発ツールとプラットフォームでは、包括的なソリューション B2E、B2B と B2C のモバイル クライアント アプリでは、すべてのターゲット プラットフォーム (iOS、Android、および Windows) と削減を支援することでコードを共有する機能を提供する、総保有コスト。 アプリでは、ネイティブ プラットフォームのルック アンド フィールを維持しながら、ユーザー インターフェイスとアプリケーション ロジック コードを共有できます。

エンタープライズ アプリの開発者は、開発中に、アプリのアーキテクチャを変更するいくつかの課題に直面します。 したがって、変更にまたは時間の経過と共に拡張されるようにアプリをビルドする重要なは。 このような適応性の設計では、困難であり、できますが、通常は、アプリを簡単に統合できるまとめてをアプリに、discrete、疎結合コンポーネントに分割します。


## <a name="related-links"></a>関連リンク

- [(2 Mb の PDF) 電子ブックをダウンロードします。](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (サンプル)](https://github.com/dotnet-architecture/eShopOnContainers)
