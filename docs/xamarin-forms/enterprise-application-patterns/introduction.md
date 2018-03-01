---
title: "はじめに"
ms.topic: article
ms.prod: xamarin
ms.assetid: cbce0659-fa03-447a-86ec-140438143230
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: b8d8aed397f72df53fdca7e455b8ef723e48e0a3
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2018
---
# <a name="introduction"></a>はじめに

すべてのプラットフォームでいくつかの課題に直面してエンタープライズ アプリケーションの開発者。

-   時間の経過と共に変更可能なアプリの要件です。
-   新しいビジネス チャンスと課題です。
-   スコープおよびアプリケーションの要件に大きく影響するための開発中に継続的なフィードバックです。

以下の点には、簡単に変更したりできる時間の経過と共に拡張アプリを作成する必要があります。 このような適応性の設計は、個別に開発され、アプリの残りの部分に影響を与えずに分離してテストするアプリの個々 の部分を使用するためのアーキテクチャが必要な難しいがあります。

多くのエンタープライズ アプリは、複数の開発者を要求するように十分に複雑です。 複数の開発者できますで動作するよういない効果的にアプリのさまざまな独立して、確保した構成要素を取得すること一緒にシームレスにアプリに統合した場合、アプリを設計する方法を決定する重要な課題があります。

設計と、アプリでどのような結果を構築するには、従来のアプローチと呼びます、*モノリシック*アプリケーションでは、ここでコンポーネントが密接に連動してない明確に分離します。 通常、このモノリシックなアプローチは、アプリの他のコンポーネントを分断することがなく、バグを解決するのには困難があるために、困難および維持するため、非効率的なアプリにし、新しい機能を追加または既存の機能を置き換えるには難しい可能性があります。

これらの課題の効果的な救済手段は、アプリを簡単に統合できるまとめてをアプリに discrete、疎結合のコンポーネントに分割することです。 このようなアプローチには、いくつかの利点があります。

-   これにより、個々 の機能を開発、テスト、拡張、およびさまざまな個人またはチームで管理できます。
-   再利用して、アプリの水平方向などの機能、認証とデータ アクセスとアプリの特定のビジネス機能などの垂直方向、機能上の問題を明確に分離を昇格させます。 これにより、依存関係をより簡単に管理できるアプリのコンポーネント間の相互作用できます。
-   により、特定のタスクまたは 1 つの専門知識に従って機能に重点を置くには、別の個人やチームの役割の分離を維持するのに役立ちます。 具体的には、ユーザー インターフェイスと、アプリのビジネス ロジックの間で明確に分離を提供します。

ただし、discrete、疎結合されたコンポーネントにアプリをパーティション分割するときに解決する必要がある多くの問題があります。 次の設定があります。

-   ユーザー インターフェイス コントロールとそのロジックの間の問題を明確に分離を提供する方法を決定します。 Xamarin.Forms エンタープライズ アプリを作成するときに、最も重要な決定の 1 つは、分離コード ファイルにビジネス ロジックを配置するかどうか、またはユーザー インターフェイス コントロールとより、アプリを作成、ロジックの間での問題を明確に分離を作成するかどうか保守が容易な管理およびテストします。 詳細については、次を参照してください。[モデル View-viewmodel](~/xamarin-forms/enterprise-application-patterns/mvvm.md)です。
-   依存関係性の注入コンテナーを使用するかどうかを決定します。 依存性の注入コンテナーは、結合の挿入、その依存関係を持つクラスのインスタンスを構築するために機能を提供することによって、オブジェクト間の依存関係を削減し、コンテナーの構成に基づいてその有効期間を管理します。 詳細については、次を参照してください。[依存性の注入](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md)です。
-   プラットフォームが提供するイベント処理の間、緩やかなを選択するには、オブジェクトと型の参照によってリンクするときに便利ではないコンポーネント間のメッセージ ベースの通信が結合されています。 詳細については、概要を参照してください。[通信間疎結合されたコンポーネント](~/xamarin-forms/enterprise-application-patterns/communicating-between-loosely-coupled-components.md)です。
-   ナビゲーションを呼び出す方法を含めて、ページ間を移動する方法を決定し、ナビゲーション ロジックが存在する必要があります。 詳細については、「[ナビゲーション](~/xamarin-forms/enterprise-application-patterns/navigation.md)」を参照してください。
-   ユーザー入力が正しいことを検証する方法を決定します。 意思決定には、ユーザー入力を検証する方法と、検証エラーをユーザーに通知する方法を含める必要があります。 詳細については、次を参照してください。[検証](~/xamarin-forms/enterprise-application-patterns/validation.md)です。
-   権限を持つリソースを保護する方法と、認証を実行する方法を決定します。 詳細については、次を参照してください。[認証および承認](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md)です。
-   Web からリモート データにアクセスする方法を決定するサービスは、確実にデータを取得する方法とデータをキャッシュする方法が含まれています。 詳細については、次を参照してください。[リモート データにアクセスする](~/xamarin-forms/enterprise-application-patterns/accessing-remote-data.md)です。
-   アプリをテストする方法を決定します。 詳細については、次を参照してください。[単体テスト](~/xamarin-forms/enterprise-application-patterns/unit-testing.md)です。

このガイドでは、これらの問題に関するガイダンスを提供し、中核となるパターンと Xamarin.Forms を使用してエンタープライズのクロスプラット フォーム アプリを構築するためのアーキテクチャに重点を置いています。 ガイダンスの目的は、一般的な Xamarin.Forms エンタープライズ アプリの開発シナ リオをアドレス指定し、プレゼンテーション、プレゼンテーション ロジック、およびサポートによるエンティティの懸案事項を分離することにより、適応性、保守、およびテストが容易なコードを生成するために、モデル View-viewmodel (MVVM) パターン。

## <a name="sample-application"></a>サンプル アプリケーション

このガイドには、次の機能が含まれているオンライン ストアである eShopOnContainers、サンプル アプリケーションが含まれています。

-   認証とバックエンド サービスに対して承認します。
-   シャツ、コーヒーマグ、およびその他のマーケティングの項目のカタログを参照しています。
-   カタログのフィルター処理します。
-   カタログからの項目を順序付けです。
-   ユーザーの注文履歴を表示します。
-   設定の構成。

### <a name="sample-application-architecture"></a>サンプル アプリケーションのアーキテクチャ

図 1-1 では、サンプル アプリケーションのアーキテクチャの大まかな概要を説明します。

![](introduction-images/architecture.png "eShopOnContainers アーキテクチャの概要")

**図 1-1**: eShopOnContainers アーキテクチャの概要

サンプル アプリケーションは、3 つのクライアント アプリに付属します。

-   ASP.NET Core を使用して、MVC アプリケーション開発します。
-   Angular 2 と Typescript 単一ページ アプリケーション (SPA) を開発しました。 Web アプリケーション用には、この方法は、各操作を使用してサーバーへのラウンド トリップの実行を回避できます。
-   IOS、Android、およびユニバーサル Windows プラットフォーム (UWP) をサポートする、Xamarin.Forms を使用したモバイル アプリを開発しました。

Web アプリケーションについては、次を参照してください。[を設計および ASP.NET Core と Microsoft Azure で最新の Web アプリケーションの開発](http://aka.ms/WebAppEbook)です。

サンプル アプリケーションには、次のバックエンド サービスが含まれます。

-   Identity マイクロ サービス、ASP.NET Core Id と IdentityServer を使用します。
-   カタログ マイクロ サービスは、データ ドリブンの作成には、読み取り、更新、EntityFramework コアを使用して SQL Server データベースを使用する (CRUD) サービスを削除します。
-   順序付けマイクロ サービスをドメインに基づくデザイン パターンを使用してドメイン ベースのサービスであります。
-   バスケット マイクロ サービス、Redis Cache を使用するデータ ドリブン CRUD サービスであります。

これらのバックエンド サービスは、ASP.NET Core の MVC を使用して microservices として実装され、1 つの Docker ホスト内で一意のコンテナーとして展開されます。 集合的に、これらのバックエンド サービスは呼ば、eShopOnContainers がアプリケーションを参照します。 クライアント アプリは、Representational State Transfer (REST) web インターフェイスからバックエンド サービスと通信します。 Microservices と Docker の詳細については、次を参照してください。[コンテナー Microservices](~/xamarin-forms/enterprise-application-patterns/containerized-microservices.md)です。

バックエンド サービスの実装については、次を参照してください。 [.NET Microservices: .NET アプリケーションのコンテナーをアーキテクチャ](https://aka.ms/microservicesebook)です。

### <a name="mobile-app"></a>モバイル アプリ

このガイドでは、Xamarin.Forms を使用してクロスプラット フォームのエンタープライズ アプリの構築に焦点を当てていて、例として eShopOnContainers モバイル アプリを使用します。 図 1-2 では、先に説明した機能を提供する eShopOnContainers モバイル アプリからページを表示します。

[![](introduction-images/screenshots.png "EShopOnContainers モバイル アプリ")](introduction-images/screenshots-large.png "eShopOnContainers モバイル アプリ")

**図 1-2**: eShopOnContainers モバイル アプリ

モバイル アプリでは、eShopOnContainers 参照アプリケーションによって提供されるバックエンド サービスを使用します。 ただし、バックエンド サービスのデプロイを避けたいと管理者向けのモック サービスからのデータを使用するよう構成できます。

EShopOnContainers モバイル アプリは Xamarin.Forms の次の機能を実行します。

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

この機能の詳細については、次を参照してください。、 [Xamarin.Forms ドキュメント](~/xamarin-forms/index.yml)、および[Xamarin.Forms を使用したモバイル アプリを作成する](https://aka.ms/xamebook)です。

さらに、単体テストは、クラス、eShopOnContainers モバイル アプリでは、一部で提供されます。

#### <a name="mobile-app-solution"></a>モバイル アプリのソリューション

EShopOnContainers モバイル アプリのソリューションでは、ソース コードおよびその他のリソースをプロジェクトに整理します。 すべてのプロジェクトは、フォルダーを使用して、ソース コードおよびその他のリソースのカテゴリを整理します。 次の表は、eShopOnContainers モバイル アプリを構成するプロジェクトを示します。

<table>
<thead>
<tr class="header">
<th>プロジェクト</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>eShopOnContainers.Core</td>
<td>このプロジェクトは、共有コードと共有 UI を含むポータブル クラス ライブラリ (PCL) プロジェクトです。</td>
</tr>
<tr class="even">
<td>eShopOnContainers.Droid</td>
<td>このプロジェクトは、特定の Android のコードを保持され、Android アプリ用のエントリ ポイントです。</td>
</tr>
<tr class="odd">
<td>eShopOnContainers.iOS</td>
<td>このプロジェクトは、iOS に固有のコードを保持して、iOS アプリのエントリ ポイントです。</td>
</tr>
<tr class="even">
<td>eShopOnContainers.UWP</td>
<td>このプロジェクトは、固有のコードをユニバーサル Windows プラットフォーム (UWP) を保持して、Windows アプリ用のエントリ ポイントです。</td>
</tr>
<tr class="odd">
<td>eShopOnContainers.TestRunner.Droid</td>
<td>このプロジェクトは、Android のテスト ランナー eShopOnContainers.UnitTests プロジェクトです。</td>
</tr>
<tr class="even">
<td>eShopOnContainers.TestRunner.iOS</td>
<td>このプロジェクトは、iOS のテスト ランナー eShopOnContainers.UnitTests プロジェクトです。</td>
</tr>
<tr class="odd">
<td>eShopOnContainers.TestRunner.Windows</td>
<td>このプロジェクトは、eShopOnContainers.UnitTests プロジェクトのユニバーサル Windows プラットフォームのテスト ランナーです。</td>
</tr>
<tr class="even">
<td>eShopOnContainers.UnitTests</td>
<td>このプロジェクトには、eShopOnContainers.Core プロジェクトの単体テストが含まれています。</td>
</tr>
</tbody>
</table>

EShopOnContainers モバイル アプリからのクラスは、ほとんどまたはまったく変更せずにすべての Xamarin.Forms アプリで再利用できます。

##### <a name="eshoponcontainerscore-project"></a>eShopOnContainers.Core プロジェクト

EShopOnContainers.Core PCL プロジェクトには、次のフォルダーが含まれています。

<table>
<thead>
<tr class="header">
<th>フォルダー</th>
<th>説明</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Animations</td>
<td>XAML で消費されるアニメーションを有効にするクラスが含まれています。</td>
</tr>
<tr class="even">
<td>ビヘイビアー</td>
<td>クラスを表示する公開されている動作が含まれています。</td>
</tr>
<tr class="odd">
<td>コントロール</td>
<td>アプリで使用するカスタム コントロールが含まれています。</td>
</tr>
<tr class="even">
<td>コンバーター</td>
<td>バインディングにカスタム ロジックを適用する値コンバーターが含まれています。</td>
</tr>
<tr class="odd">
<td>効果</td>
<td>含まれています、<code>EntryLineColorEffect</code>を特定の罫線の色を変更するために使用されるクラス<code>Entry</code>コントロール。</td>
</tr>
<tr class="even">
<td>例外</td>
<td>ユーザー設定を含む<code>ServiceAuthenticationException</code>です。</td>
</tr>
<tr class="odd">
<td>拡張機能</td>
<td>拡張メソッドを格納、<code>VisualElement</code>と<code>IEnumerable<T> </code>クラスです。</td>
</tr>
<tr class="even">
<td>ヘルパー</td>
<td>アプリ用にヘルパー クラスが含まれています。</td>
</tr>
<tr class="odd">
<td>モデル</td>
<td>アプリのモデル クラスを含みます。</td>
</tr>
<tr class="even">
<td>プロパティ</td>
<td>含む<code>AssemblyInfo.cs</code>、.NET アセンブリのメタデータ ファイル。</td>
</tr>
<tr class="odd">
<td>サービス</td>
<td>アプリに提供するサービスを実装するインターフェイスとクラスが含まれています。</td>
</tr>
<tr class="even">
<td>トリガー</td>
<td>含まれています、<code>BeginAnimation</code>トリガーで、XAML でのアニメーションの呼び出しに使用します。</td>
</tr>
<tr class="odd">
<td>検証</td>
<td>データ入力の検証に関連するクラスが含まれています。</td>
</tr>
<tr class="even">
<td>ViewModels</td>
<td>ページに公開されているアプリケーションのロジックが含まれています。</td>
</tr>
<tr class="odd">
<td>ビュー</td>
<td>アプリのページが含まれています。</td>
</tr>
</tbody>
</table>

##### <a name="platform-projects"></a>プラットフォームのプロジェクト

プラットフォームのプロジェクトには、影響の実装、カスタム レンダラー実装、およびその他のプラットフォームに固有のリソースが含まれます。

## <a name="summary"></a>まとめ

Xamarin のクロスプラット フォーム モバイル アプリ開発ツールとプラットフォーム、包括的なソリューションを提供 B2E、B2B および B2C のモバイル クライアント アプリでは、すべてのターゲット プラットフォーム (iOS、Android、および Windows) とレベルを下げるには、支援することでコードを共有する機能を提供する、総保有コスト。 アプリは、ネイティブ プラットフォームのルック アンド フィールを維持しながら、ユーザー インターフェイスとアプリケーション ロジックのコードを共有できます。

エンタープライズ アプリケーションの開発者は、開発時に、アプリのアーキテクチャを変更するいくつかの課題に直面します。 そのため、変更または時間の経過と共に拡張できるようにする、アプリのビルドに重要なです。 このような適応性の設計では、困難ですを指定できますが、通常は、アプリを簡単に統合できるまとめてをアプリに discrete、疎結合のコンポーネントに分割します。


## <a name="related-links"></a>関連リンク

- [電子 (2 Mb PDF) のダウンロードします。](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (サンプル)](https://github.com/dotnet-architecture/eShopOnContainers)
