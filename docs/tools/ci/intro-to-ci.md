---
title: Xamarin との継続的インテグレーションの概要
description: このドキュメントでは、Xamarin との継続的な統合について説明します。 バージョン管理とさまざまな継続的インテグレーション環境について説明します。
ms.prod: xamarin
ms.assetid: C034200E-2947-4309-9DDD-80DAC505C43F
author: davidortinau
ms.author: daortin
ms.date: 07/19/2017
ms.openlocfilehash: 2862f05f2d183c9345d2b92268ddf2101cc2492e
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029817"
---
# <a name="introduction-to-continuous-integration-with-xamarin"></a>Xamarin との継続的インテグレーションの概要

_継続的インテグレーションは、プロジェクトのバージョン管理リポジトリ内の開発者によってコードが追加または変更されたときに、自動ビルドによってコンパイルされ、必要に応じてアプリをテストするソフトウェアエンジニアリング手法です。この記事では、継続的インテグレーションの一般的な概念と、Xamarin プロジェクトとの継続的な統合に使用できるいくつかのオプションについて説明します。_

これは、開発者が並行して作業するためのソフトウェアプロジェクトに共通しています。 ある時点で、このような並列処理のすべてを、最終的な成果物を構成する1つのコードベースに統合する必要があります。 ソフトウェア開発の初期段階では、この統合がプロジェクトの最後に実行されていましたが、これは困難でリスクの高いプロセスでした。

継続的インテグレーション (CI) は、開発者がプロジェクトの共有コードリポジトリに変更をチェックインするたびに、開発者のすべての変更を共通のコードベースに継続的にマージすることで、このような複雑さを回避します。 各チェックインは自動ビルドをトリガーし、自動テストを実行して、新しく導入されたコードが既存のコードを破壊していないことを確認します。  この方法では、CI によってエラーと問題が即座に発生し、すべてのチームメンバーが互いの作業を確実に最新の状態に保つことができます。 これにより、統一された、安定したコードベースが生成されます。

継続的インテグレーションシステムには、次の2つの主要な部分があります。

- **バージョン管理**(VC) は、ソース管理またはソースコード管理とも呼ばれ、すべてのプロジェクトのコードを1つの共有リポジトリに統合し、すべてのファイルに対するすべての変更の完全な履歴を保持します。 このリポジトリ (多くの場合、*メインライン*または*マスター*ブランチと呼ばれます) には、最終的にアプリの運用バージョンまたはリリースバージョンをビルドするために使用されるソースコードが含まれています。 このタスクには多くのオープンソースおよび商用製品があります。通常、チームまたは個人は、コードのコピーをセカンダリブランチにフォークすることができます。これにより、マスターブランチへのリスクなしに広範な変更を行ったり実験を実施したりすることができます。 セカンダリブランチの変更が検証されると、その変更をすべてマスターブランチにマージして戻すことができます。
- **継続的インテグレーションサーバー** –継続的インテグレーションサーバーは、プロジェクトのすべてのアーティファクト (ソースコード、画像、ビデオ、データベース、自動テストなど) を収集し、アプリをコンパイルし、自動テストを実行します。 ここでも、多数のオープンソースおよび商用 CI サーバーツールがあります。

通常、開発者はワークステーション上に1つ以上のブランチの作業コピーを作成します。作業は最初に完了します。 適切な一連の作業が完了すると、変更は適切な分岐に "チェックイン" または "コミット済み" になり、他の開発者の作業コピーに反映されます。 これは、チームがすべて同じコードで作業していることを確認する方法です。

さらに、継続的インテグレーションでは、変更をコミットする操作によって、CI サーバーはプロジェクトをビルドし、自動テストを実行してソースコードの正確性を検証します。 ビルドエラーまたはテストの失敗が発生した場合、CI サーバーは、担当開発者に (電子メール、IM、Twitter、Growl などを使用して) 通知し、問題を解決できるようにします。 ("ゲートチェックイン" と呼ばれるエラーがある場合、CI サーバーはコミットを拒否することもできます)。

このプロセスを次の図に示します。

[![](intro-to-ci-images/intro01-small.png "This diagram illustrates this process")](intro-to-ci-images/intro01.png#lightbox)

モバイルアプリでは、継続的インテグレーションのための固有の課題が生じます。 アプリには、物理デバイスでのみ使用できる GPS やカメラなどのセンサーが必要になる場合があります。 さらに、シミュレーターまたはエミュレーターはハードウェアの近似値であり、問題を隠すか不明瞭にすることができます。 最終的には、実際のハードウェアでモバイルアプリをテストして、本当に顧客の準備が整っていることを確信する必要があります。

[App Center テスト](https://docs.microsoft.com/appcenter/test-cloud)では、数百の物理デバイス上で直接アプリをテストすることで、この問題に対処します。 開発者は、強力な UI テストを可能にする自動化された受け入れテストを作成します。 これらのテストが App Center にアップロードされると、CI サーバーは、次の図に示すように CI プロセスの一部として自動的に実行できます。

[![](intro-to-ci-images/intro02-small.png "Once these tests are uploaded to App Center, the CI server can run them automatically as part of a CI process as shown in this diagram")](intro-to-ci-images/intro02.png#lightbox)

## <a name="components-of-continuous-integration"></a>継続的インテグレーションのコンポーネント

CI をサポートするように設計された商用およびオープンソースツールの広範なエコシステムが用意されています。 このセクションでは、最も一般的なもののいくつかについて説明します。

### <a name="version-control"></a>バージョン コントロール

#### <a name="azure-devops-and-team-foundation-server"></a>Azure DevOps と Team Foundation Server

[Azure DevOps](https://azure.microsoft.com/services/devops/) and [Team Foundation Server](https://visualstudio.microsoft.com/tfs/) (TFS) は、継続的インテグレーションビルドサービス、タスク追跡、アジャイル計画およびレポートツール、バージョン管理を行うための Microsoft のコラボレーションツールです。 バージョン管理を使用すると、Azure DevOps および TFS は、独自のシステム (Team Foundation バージョン管理または TFVC)、または GitHub でホストされているプロジェクトと連携できます。

- Azure DevOps は、クラウド経由でサービスを提供します。 主な利点は、専用のハードウェアまたはインフラストラクチャを必要とせず、どこからでも web ブラウザーや Visual Studio などの一般的な開発ツールを介してアクセスできることです。地理的に分散しているチームにとって魅力があります. 5人の開発者のチームにとっては無料です。その後、拡大するチームに対応するために追加のライセンスを購入できます。
- TFS は、オンプレミスの Windows サーバー向けに設計されており、ローカルネットワークまたはそのネットワークへの VPN 接続を介してアクセスします。 主な利点は、ビルドサーバーの構成を完全に制御し、必要な追加のソフトウェアやサービスをインストールできることです。 TFS には、小規模なチーム向けの無料のエントリレベルの Express edition があります。

TFS と Azure DevOps はどちらも Visual Studio と密接に統合されており、開発者は1つの IDE から快適に多くのバージョンコントロールと CI タスクを実行できます。 Eclipse 用の Team Explorer Everywhere プラグイン (下記参照) も使用できます。 Visual Studio for Mac には[、TFVC のプレビューが用意](/visualstudio/mac/tf-version-control/)されています。

[Azure DevOps パイプライン](https://docs.microsoft.com/azure/devops/pipelines/languages/xamarin/)では、Xamarin プロジェクトが直接サポートされており、対象とするプラットフォームごとにビルド定義を作成します (Android、IOS、Windows)。 各ビルド定義には、適切な Xamarin ライセンスが必要です。 この目的のために、Xamarin 対応のローカルの TFS ビルドサーバーを Azure DevOps に接続することもできます。 このセットアップでは、Azure DevOps のキューに登録されているビルドがローカルサーバーに委任されます。 詳細については、「[ビルドエージェントとリリースエージェント](https://docs.microsoft.com/azure/devops/pipelines/agents/agents)」を参照してください。 または、Jenkins やチーム City など、別のビルドツールを使用することもできます。

Visual Studio、Azure DevOps、Team Foundation Server のすべてのアプリケーションライフサイクル管理 (ALM) 機能の完全な概要については、「 [DevOps With Xamarin Apps](https://docs.microsoft.com/visualstudio/cross-platform/application-lifecycle-management-alm-with-xamarin-apps)」を参照してください。

#### <a name="team-explorer-everywhere"></a>Team Explorer Everywhere

[Team Explorer Everywhere](https://docs.microsoft.com/azure/devops/java/download-eclipse-plug-in/)は、Visual Studio の外部で開発するチームに Team Foundation Server と Azure DevOps の機能をもたらします。 これにより、開発者は、Eclipse または OS X および Linux 用のクロスプラットフォームコマンドラインクライアントから、オンプレミスまたはクラウド内のチームプロジェクトに接続できます。 Team Explorer Everywhere を使用すると、Windows 以外のプラットフォームのバージョンコントロール (Git を含む)、作業項目、およびビルド機能にフルアクセスできます。

#### <a name="git"></a>Git

[Git](https://git-scm.com)は、Linux カーネルのソースコードを管理するために最初に開発された、広く普及しているオープンソースのバージョン管理ソリューションです。 これは、あらゆる規模のソフトウェアプロジェクトでよく使用される、非常に高速で柔軟なシステムです。 インターネットアクセスが不十分な単一の開発者から、世界中の大規模なチームに簡単に拡張できます。 Git では、ブランチを非常に簡単にすることもできます。これにより、最小限のリスクで開発の並列ストリームを奨励することができます。

Git は、web ブラウザー、または Linux、Mac OSX、Windows で実行されている[GUI クライアント](https://git-scm.com/downloads/guis)を使用して完全に動作できます。 パブリックリポジトリでは無料です。プライベートリポジトリには[有料プラン](https://github.com/pricing)が必要です。

現在のバージョンの Visual Studio for Windows と Mac では、Git のネイティブサポートが提供されています。 Microsoft では、以前のバージョンの Visual Studio で[Git のダウンロード可能な拡張機能](https://visualstudiogallery.msdn.microsoft.com/abafc7d6-dcaa-40f4-8a5e-d6724bdb980c)を提供しています。 前述のように、Azure DevOps と TFS では、TFVC ではなくバージョン管理に Git を使用できます。

#### <a name="subversion"></a>破壊

[Subversion](https://subversion.apache.org) (SVN) は、2000以降に使用されていた、広く普及しているオープンソースのバージョン管理システムです。 SVN は、OS X、Windows、FreeBSD、Linux、および Unix のすべての最新バージョンで実行されます。 Visual Studio for Mac は、SVN をネイティブでサポートしています。 SVN サポートを Visual Studio に導入するサードパーティ製の拡張機能があります。

### <a name="continuous-integration-environments"></a>継続的インテグレーション環境

継続的インテグレーション環境をセットアップするということは、バージョンコントロールシステムとビルドサービスを組み合わせることです。  後者の場合、最も一般的な2つの方法は次のとおりです。

- [Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/)は、Azure DEVOPS と TFS のビルドシステムです。 これは Visual Studio と密接に統合されているため、開発者がビルドをトリガーし、自動的にテストを実行し、結果を確認するのに便利です。
- Jenkins は、あらゆる種類のソフトウェア開発をサポートするプラグインの豊富なエコシステムを備えたオープンソースの CI サーバーです。 Windows および Mac OS X で実行されます。 Jenkins は特定の IDE と統合されていません。 代わりに、web インターフェイスを使用して構成および管理されます。 また、Jenkins CI は簡単にインストールして構成できるので、小規模なチームにとって魅力があります。

TFS/Azure DevOps を単独で使用することも、次のセクションで説明するように、Jenkins と TFS/Azure DevOps または Git を組み合わせて使用することもできます。

#### <a name="azure-devops-and-team-foundation-server"></a>Azure DevOps と Team Foundation Server

既に説明したように、Azure DevOps と Team Foundation Server には、バージョンコントロールとビルドサービスの両方が用意されています。 ビルドサービスは、ターゲットプラットフォームごとに Xamarin Business または Enterprise のライセンスを常に必要とします。

Azure DevOps では、ターゲットプラットフォームごとに個別のビルド定義を作成し、そこに適切なライセンスを入力します。 構成が完了すると、Azure DevOps はクラウドでビルドとテストを実行します。 詳細については、「 [Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/) 」を参照してください。

Team Foundation Server では、特定のターゲットプラットフォームに合わせてビルドコンピューターを次のように構成します。

- **Android と Windows:** Visual Studio と Xamarin ツール (Android と Windows の両方) をインストールし、Xamarin のライセンスでを構成します。 また、Android SDK を TFS ビルドエージェントが検出できるサーバー上の共有の場所に移動することも必要です。 詳細については、「 [TFVC の構成](https://docs.microsoft.com/azure/devops/repos/tfvc/overview)」を参照してください。
- **iOS と Xamarin:** 適切なライセンスを使用して、Visual Studio と Xamarin ツールを Windows server にインストールします。 次に、ネットワークからアクセス可能な Mac OS X マシンに Visual Studio for Mac をインストールします。これはビルドホストとして機能し、最終的なアプリケーションパッケージを作成します (iOS の場合は IPA、OS X の場合は APP)。

次の図は、このようになっていることを示しています。

[![](intro-to-ci-images/intro03-small.png "This diagram illustrates this topography")](intro-to-ci-images/intro03.png#lightbox)

また、ローカルの TFS サーバーを Azure DevOps プロジェクトにリンクして、Azure DevOps のビルドがローカルサーバーに委任されるようにすることもできます。 詳細については、「[ビルドエージェントとリリースエージェント](https://docs.microsoft.com/azure/devops/pipelines/agents/agents/)」を参照してください。

#### <a name="azure-devops-and-jenkins"></a>Azure DevOps と Jenkins

Jenkins を使用してアプリをビルドする場合は、Azure DevOps または Team Foundation Server にコードを保存し、CI ビルドに対して Jenkins を引き続き使用することができます。 チームプロジェクトの Git リポジトリにコードをプッシュするとき、または TFVC にコードをチェックインするときに、Jenkins ビルドをトリガーできます。 詳細については、「 [Jenkins With Azure DevOps](https://docs.microsoft.com/azure/devops/service-hooks/services/jenkins)」を参照してください。

[![](intro-to-ci-images/intro04-small.png "If you use Jenkins to build your apps, you can store your code in Azure DevOps or Team Foundation Server and continue to use Jenkins for your CI builds")](intro-to-ci-images/intro04.png#lightbox)

#### <a name="git-and-jenkins"></a>Git と Jenkins

もう1つの一般的な CI 環境は、完全に OS X ベースにすることができます。 このシナリオでは、ソースコード管理に Git を使用し、ビルドサーバーに Jenkins を使用します。 これらはどちらも、Visual Studio for Mac がインストールされている1台の Mac OS X コンピューター上で実行されています。 これは、前のセクションで説明した Azure DevOps + Jenkins 環境とよく似ています。

[![](intro-to-ci-images/intro05-small.png "This is very similar to the Azure DevOps + Jenkins environment discussed in the previous section")](intro-to-ci-images/intro05.png#lightbox)

> [!IMPORTANT]
> **Jenkins は[Microsoft がサポートしていません](~/cross-platform/troubleshooting/questions/xamarin-jenkins.md)。**
