---
title: エンタープライズアプリ開発の概要
description: この章では、エンタープライズアプリ開発の概要を説明し、eShopOnContainers モバイルアプリについて紹介します。
ms.prod: xamarin
ms.assetid: cbce0659-fa03-447a-86ec-140438143230
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: e085006f3f7479d35e4991503e52ce19ced7f75b
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93374538"
---
# <a name="introduction-to-enterprise-app-development"></a>エンタープライズアプリ開発の概要

> [!NOTE]
> この電子ブックは2017の spring で公開されており、その後、更新されていません。 本は貴重なものですが、一部のマテリアルは古くなっています。

プラットフォームに関係なく、エンタープライズアプリの開発者はいくつかの課題に直面します。

- 時間の経過と共に変化するアプリの要件。
- 新しいビジネスチャンスと課題。
- アプリのスコープと要件に大きく影響する可能性がある、開発中の継続的なフィードバック。

これらの点を考慮して、時間の経過と共に簡単に変更または拡張できるアプリを構築することが重要です。 アプリの他の部分に影響を与えることなく、アプリの個々の部分を個別に開発およびテストできるアーキテクチャが必要なため、このような適応性のために設計するのは困難な場合があります。

多くのエンタープライズアプリは、複数の開発者を必要とするために十分に複雑です。 複数の開発者がアプリのさまざまな部分で効率的に作業できるように、アプリを設計する方法を決定することは、大きな課題になる可能性があります。一方、アプリに統合されたときに、それらの要素がシームレスに統合されるようにします。

アプリを設計して構築するための従来のアプローチでは、 *モノリシック* アプリと呼ばれるものがあります。ここでは、コンポーネントが密に結合され、それらの間に明確な分離がありません。 通常、このモノリシックアプローチは、アプリ内の他のコンポーネントを破損させずにバグを解決するのが困難であり、新しい機能を追加したり、既存の機能を置き換えたりすることが困難になるため、管理が困難で非効率的なアプリにつながります。

これらの課題を効果的に解決するには、アプリをアプリに簡単に統合できる不連続で疎結合されたコンポーネントに分割します。 このようなアプローチには、いくつかの利点があります。

- 個々の機能をさまざまな個人またはチームによって開発、テスト、拡張、および管理できます。
- 再利用が促進され、アプリの水平機能 (認証とデータアクセスなど) と、アプリ固有のビジネス機能などの垂直機能の間の問題が明確に分離されます。 これにより、アプリコンポーネント間の依存関係と相互作用をより簡単に管理できるようになります。
- さまざまな個人またはチームが専門知識に基づいて特定のタスクや機能に専念できるようにすることで、ロールの分離を維持するのに役立ちます。 具体的には、ユーザーインターフェイスとアプリのビジネスロジックを明確に区別します。

ただし、アプリを疎結合された個別のコンポーネントにパーティション分割する場合は、解決する必要がある多くの問題があります。 チェックの内容は次のとおりです

- ユーザーインターフェイスコントロールとそのロジックとの間の問題を明確に分離する方法を決定します。 エンタープライズアプリを作成する際の最も重要な決定の1つ Xamarin.Forms は、分離コードファイルにビジネスロジックを配置するかどうか、またはアプリの保守性とテスト可能性を高めるために、ユーザーインターフェイスコントロールとそのロジックの間に問題の明確な分離を作成するかどうかです。 詳細については、「 [モデルビュー-ビューモデル](~/xamarin-forms/enterprise-application-patterns/mvvm.md)」を参照してください。
- 依存関係挿入コンテナーを使用するかどうかを判断します。 依存関係の挿入コンテナーを使用すると、オブジェクト間の依存関係の結合が軽減されます。これにより、依存関係が挿入されたクラスのインスタンスを構築する機能が提供され、コンテナーの構成に基づいて有効期間が管理されます。 詳細については、「 [依存関係の挿入](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md)」を参照してください。
- プラットフォームによって提供されるイベントと、オブジェクトと型の参照によってリンクするのが不便なコンポーネント間での疎結合のメッセージベースの通信を選択します。 詳細については、「 [疎結合コンポーネント間の通信](~/xamarin-forms/enterprise-application-patterns/communicating-between-loosely-coupled-components.md)の概要」を参照してください。
- ナビゲーションの呼び出し方法、ナビゲーションロジックを配置する方法など、ページ間を移動する方法を決定します。 詳細については、「[ナビゲーション](~/xamarin-forms/enterprise-application-patterns/navigation.md)」を参照してください。
- ユーザー入力を検証して正しいかどうかを確認する。 決定には、ユーザー入力の検証方法、および検証エラーについてユーザーに通知する方法が含まれている必要があります。 詳細については、「 [検証](~/xamarin-forms/enterprise-application-patterns/validation.md)」を参照してください。
- 認証の実行方法、および承認によってリソースを保護する方法を決定します。 詳細については、「 [認証と承認](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md)」を参照してください。
- データを確実に取得する方法やデータをキャッシュする方法など、web サービスからリモートデータにアクセスする方法を決定します。 詳細については、「 [リモートデータへのアクセス](~/xamarin-forms/enterprise-application-patterns/accessing-remote-data.md)」を参照してください。
- アプリのテスト方法を決定します。 詳細については、「[単体テスト](~/xamarin-forms/enterprise-application-patterns/unit-testing.md)」を参照してください。

このガイドでは、これらの問題について説明し、を使用してクロスプラットフォームのエンタープライズアプリを構築するための主要なパターンとアーキテクチャに焦点を当てて Xamarin.Forms います。 このガイダンスは、一般的な Xamarin.Forms エンタープライズアプリの開発シナリオに対処し、モデルビューモデル (MVVM) パターンのサポートによってプレゼンテーション、プレゼンテーションロジック、およびエンティティの懸念を分離することによって、適応性、保守性、テスト可能なコードを作成することを目的としています。

## <a name="sample-application"></a>サンプル アプリケーション

このガイドには、サンプルアプリケーション eShopOnContainers が含まれています。これは、次の機能を含むオンラインストアです。

- バックエンドサービスに対する認証と承認
- シャツ、コーヒーマグ、その他のマーケティング項目のカタログを閲覧する。
- カタログをフィルター処理しています。
- カタログからのアイテムの順序付け。
- ユーザーの注文履歴を表示しています。
- 設定の構成。

### <a name="sample-application-architecture"></a>サンプルアプリケーションのアーキテクチャ

図1-1 は、サンプルアプリケーションのアーキテクチャの概要を示しています。

![eShopOnContainers のアーキテクチャの概要](introduction-images/architecture.png)

**図 1-1** : eShopOnContainers のアーキテクチャの概要

サンプルアプリケーションには、次の3つのクライアントアプリが付属しています。

- ASP.NET Core によって開発された MVC アプリケーション。
- 角速度2と Typescript を使用して開発されたシングルページアプリケーション (SPA)。 Web アプリケーションのこのアプローチでは、各操作でサーバーへのラウンドトリップが実行されることを回避します。
- Xamarin.FormsIOS、Android、ユニバーサル Windows プラットフォーム (UWP) をサポートする、で開発されたモバイルアプリ。

Web アプリケーションの詳細については、「 [ASP.NET Core と Microsoft Azure を使用した最新の Web アプリケーションの設計と開発](https://aka.ms/WebAppEbook)」を参照してください。

サンプルアプリケーションには、次のバックエンドサービスが含まれています。

- Id マイクロサービス。 ASP.NET Core Id とユーザーサーバーを使用します。
- カタログマイクロサービス。 EntityFramework Core を使用して SQL Server データベースを使用する、データドリブンの作成、読み取り、更新、削除 (CRUD) サービスです。
- ドメイン駆動型の設計パターンを使用するドメイン駆動型サービスである注文マイクロサービス。
- バスケットマイクロサービス。 Redis Cache を使用するデータドリブン CRUD サービスです。

これらのバックエンドサービスは ASP.NET Core MVC を使用してマイクロサービスとして実装され、1つの Docker ホスト内に一意のコンテナーとしてデプロイされます。 これらのバックエンドサービスをまとめて、eShopOnContainers reference アプリケーションと呼びます。 クライアントアプリは、表現可能な状態転送 (REST) web インターフェイスを介してバックエンドサービスと通信します。 マイクロサービスと Docker の詳細については、「コンテナー化された [マイクロサービス](~/xamarin-forms/enterprise-application-patterns/containerized-microservices.md)」を参照してください。

バックエンドサービスの実装の詳細については、「 [.Net マイクロサービス: コンテナー化された .Net アプリケーションのアーキテクチャ](https://aka.ms/microservicesebook)」を参照してください。

### <a name="mobile-app"></a>モバイル アプリ

このガイドでは、を使用してクロスプラットフォームのエンタープライズアプリを構築することに重点を置いて、 Xamarin.Forms 例として eShopOnContainers mobile アプリを使用します。 図1-2 に、前に説明した機能を提供する eShopOnContainers モバイルアプリのページを示します。

[![EShopOnContainers モバイルアプリ](introduction-images/screenshots.png)](introduction-images/screenshots-large.png#lightbox "EShopOnContainers モバイルアプリ")

**図 1-2** : eShopOnContainers モバイルアプリ

モバイルアプリは、eShopOnContainers 参照アプリケーションによって提供されるバックエンドサービスを使用します。 ただし、バックエンドサービスのデプロイを回避するために、モックサービスからのデータを使用するように構成することもできます。

EShopOnContainers mobile アプリでは、次の Xamarin.Forms 機能を実行します。

- XAML
- コントロール
- バインド
- コンバーター
- スタイル
- アニメーション
- コマンド
- 動作
- トリガー
- 効果
- カスタム レンダラー
- MessagingCenter
- カスタム コントロール

この機能の詳細については、の[ Xamarin.Forms ドキュメント](~/xamarin-forms/index.yml)を参照し、[を使用 Xamarin.Forms ](https://aka.ms/xamformsebook)して Mobile Apps を作成してください。

また、eShopOnContainers モバイルアプリの一部のクラスに対して単体テストが提供されます。

#### <a name="mobile-app-solution"></a>モバイルアプリソリューション

EShopOnContainers mobile app ソリューションは、ソースコードとその他のリソースをプロジェクトに編成します。 すべてのプロジェクトは、フォルダーを使用してソースコードやその他のリソースをカテゴリに整理します。 次の表に、eShopOnContainers モバイルアプリを構成するプロジェクトの概要を示します。

|プロジェクト|説明|
|--- |--- |
|eShopOnContainers|このプロジェクトは、共有コードと共有 UI を含む、ポータブルクラスライブラリ (PCL) プロジェクトです。|
|eShopOnContainers|このプロジェクトは Android 固有のコードを保持し、Android アプリのエントリポイントです。|
|eShopOnContainers|このプロジェクトは、iOS 固有のコードを保持し、iOS アプリのエントリポイントです。|
|eShopOnContainers|このプロジェクトは、ユニバーサル Windows プラットフォーム (UWP) 固有のコードを保持し、Windows アプリのエントリポイントです。|
|eShopOnContainers Testrunner.completed|このプロジェクトは、eShopOnContainers テストプロジェクトの Android テストランナーです。|
|eShopOnContainers Testrunner.completed|このプロジェクトは、eShopOnContainers テストプロジェクトの iOS テストランナーです。|
|eShopOnContainers Testrunner.completed|このプロジェクトは、eShopOnContainers テストプロジェクトのユニバーサル Windows プラットフォームテストランナーです。|
|eShopOnContainers テスト|このプロジェクトには、eShopOnContainers プロジェクトの単体テストが含まれています。|

EShopOnContainers モバイルアプリのクラスは、ほとんど変更せずに、任意のアプリで再利用でき Xamarin.Forms ます。

##### <a name="eshoponcontainerscore-project"></a>eShopOnContainers プロジェクト

EShopOnContainers PCL プロジェクトには、次のフォルダーが含まれています。

|Folder|説明|
|--- |--- |
|アニメーション|XAML でアニメーションを使用できるようにするクラスが含まれています。|
|動作|ビュークラスに公開される動作を格納します。|
|コントロール|アプリによって使用されるカスタムコントロールが含まれます。|
|コンバーター|カスタムロジックをバインディングに適用する値コンバーターを格納します。|
|効果|特定の `EntryLineColorEffect` コントロールの境界線の色を変更するために使用されるクラスが含まれてい `Entry` ます。|
|例外|カスタムを格納し `ServiceAuthenticationException` ます。|
|拡張機能|クラスおよびクラスの拡張メソッドが含まれてい `VisualElement` `IEnumerable` ます。|
|ヘルパー|アプリのヘルパークラスが含まれています。|
|モデル|アプリのモデルクラスが含まれています。|
|Properties|`AssemblyInfo.cs`.Net アセンブリメタデータファイルを含みます。|
|サービス|アプリに提供されるサービスを実装するインターフェイスとクラスが含まれています。|
|トリガー|`BeginAnimation`XAML でアニメーションを呼び出すために使用されるトリガーが含まれています。|
|確認|データ入力の検証に関連するクラスが含まれています。|
|ViewModels|ページに公開されているアプリケーションロジックを格納します。|
|ビュー|アプリのページが含まれています。|

##### <a name="platform-projects"></a>プラットフォームプロジェクト

プラットフォームプロジェクトには、効果の実装、カスタムレンダラーの実装、およびその他のプラットフォーム固有のリソースが含まれています。

## <a name="summary"></a>まとめ

Xamarin のクロスプラットフォームモバイルアプリ開発ツールとプラットフォームは、B2E、B2B、B2C モバイルクライアントアプリ向けの包括的なソリューションを提供し、すべてのターゲットプラットフォーム (iOS、Android、および Windows) 間でコードを共有し、総保有コストを削減する機能を提供します。 アプリは、ネイティブプラットフォームのルックアンドフィールを維持しながら、ユーザーインターフェイスとアプリロジックコードを共有できます。

エンタープライズアプリの開発者は、開発中にアプリのアーキテクチャを変更できるいくつかの課題に直面しています。 そのため、アプリをビルドして、時間の経過と共に変更または拡張できるようにすることが重要です。 このような適応性のための設計は困難ですが、通常はアプリをアプリに簡単に統合できる不連続で疎結合されたコンポーネントにアプリをパーティション分割する必要があります。

## <a name="related-links"></a>関連リンク

- [電子ブックのダウンロード (2 Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (サンプル)](https://github.com/dotnet-architecture/eShopOnContainers)
