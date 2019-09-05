---
title: Xamarin を使用した継続的インテグレーションの概要
description: このドキュメントでは、Xamarin を使用した継続的インテグレーションについて説明します。 これは、バージョン管理およびさまざまな継続的インテグレーション環境について説明します。
ms.prod: xamarin
ms.assetid: C034200E-2947-4309-9DDD-80DAC505C43F
author: conceptdev
ms.author: crdun
ms.date: 07/19/2017
ms.openlocfilehash: d335a107d1520db3c76ee602d38adcb129f122b0
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70293093"
---
# <a name="introduction-to-continuous-integration-with-xamarin"></a>Xamarin を使用した継続的インテグレーションの概要

_継続的インテグレーションは、プロジェクトのバージョン管理リポジトリで開発者によってコードに変更や追加が加えられた場合に、自動化されたビルドがアプリをコンパイルし必要に応じてテストするというソフトウェア工学の実践です。この記事は、継続的インテグレーションの一般的な概念と、Xamarin プロジェクトの継続的インテグレーションに利用できるいくつかの選択を説明します。_

ソフトウェア プロジェクトでは開発者が並行して作業することが一般的です。 ある時点で、最終的な製品を構成する1つのコードベースに、これらの並行する作業の流れのすべてを統合する必要があります。 ソフトウェア開発の初期の頃は、この統合は、プロジェクトの最後に実行されていましたが、それは難しく危険な処理でした。

継続的インテグレーション (CI) は、通常、開発者がプロジェクトの共有コードリポジトリに変更をチェックインするたびに、すべての開発者の変更を共通のコードベースに継続的にマージすることで、このような複雑さを回避します。 各チェックインは、自動化されたビルドをトリガーし、新しく導入されたコードが既存のコードを壊していないかを確認するために、自動テストを実行します。  この方法では、CI によってエラーと問題が即座に発生し、すべてのチームメンバーが互いの作業を確実に最新の状態に保つことができます。 これにより結束的な安定したコードベースになります。

継続的インテグレーションシステムには、次の2つの主要な部分があります。

- **バージョン コントロール** – バージョン コントロール (VC)、またはソースコントロールやソースコードマネージメントとも呼ばれるものは、1 つの共有リポジトリにすべてのプロジェクトのコードを統合し、すべてのファイルのすべての変更の完全な履歴を保持します。 このリポジトリは、よく *mainline* または *master* ブランチと呼ばれ、 アプリの本番環境またはリリースバージョンをビルドするために使われる最終的なソースコートが含まれています。 このタスクには多くのオープンソースと商用の製品があり、通常、チームまたは個人が、マスターブランチへのリスクなしで、広範囲の変更や検証の実施を行える二次的なブランチへコードのコピーを分岐することを許可しています。 二次的なブランチの変更が検証されると、それらはマスターブランチに全てまとめてマージすることができます。
- **継続的インテグレーション サーバー** – 継続的インテグレーション サーバーは、プロジェクトの全ての成果物（ソースコード、画像、動画、データベース、自動テストなど）の収集、アプリのコンパイル、自動テストの実行を担当します。 これも、多くのオープンソースおよび商用の CI サーバー ツールが存在します。

開発者は通常、各自のワークステーションに、1 つまたは複数のブランチの作業コピーを持っており、そこで最初のうちは作業します。 作業の適切なセットが完了したら、変更は適切なブランチへ "チェックイン" または "コミット" され、他の開発者の作業コピーへそれらを伝搬します。 このようにして、チームが同じコード上で全て作業できるようにします。

ここでも、継続的インテグレーションがあれば、変更をコミットする機能により、CI サーバーが、プロジェクトをビルドし、ソースコードの正当性を確認する自動テストを実行できるようになります。 ビルドエラーやテストの失敗がある場合は、CI サーバーが、担当開発者に通知します(電子メール、IM、Twitter、Growl など)。これによって開発者が問題を修正できるようにします。 (CI サーバーは、失敗があった場合にコミットを拒否することもできます。これは、「ゲート チェックイン」と呼ばれます。)

以下のダイアグラム図は、上記のプロセスを示します。

[![](intro-to-ci-images/intro01-small.png "このダイアグラム図では、上記のプロセスを示しています。")](intro-to-ci-images/intro01.png#lightbox)

モバイルアプリでは、継続的インテグレーションに特有の課題を導入します。 アプリには、物理デバイスでのみ使用できる、GPS やカメラなどのセンサーが要求されることがあります。 さらに、シミュレーターやエミュレーターは、ハードウェアの近似にすぎず、問題が隠れたり分かりにくくなったりします。 結局は、実機上のモバイルアプリをテストして、確実に顧客への準備ができていることを保証する必要があります。

[App Center Test](https://docs.microsoft.com/appcenter/test-cloud)は、数百台の物理デバイス上でアプリを直接テストすることで、この特有の問題に対処します。 開発者は、強力な UI テストを可能にする自動化された受け入れテストを作成します。 これらのテストが App Center にアップロードされると、CI サーバーは、次のダイアグラム図に示すように CI プロセスの一部として自動的にそれらを実行することができます。

[![](intro-to-ci-images/intro02-small.png "これらのテストが App Center にアップロードされると、CI サーバーは、この図が示すように CI のプロセスの一部として自動的にそれらを実行できます. ")](intro-to-ci-images/intro02.png#lightbox)

## <a name="components-of-continuous-integration"></a>継続的インテグレーションの構成要素

CI をサポートするように設計された商用およびオープンソースのツールによる豊富なエコシステムがあります。 このセクションでは、最も一般的ないくつかのツールについて説明します。

### <a name="version-control"></a>バージョン コントロール

#### <a name="azure-devops-and-team-foundation-server"></a>Azure DevOps と Team Foundation Server

[Azure DevOps](https://azure.microsoft.com/services/devops/) and [Team Foundation Server](https://visualstudio.microsoft.com/tfs/) (TFS) は、継続的インテグレーションビルドサービス、タスク追跡、アジャイル計画およびレポートツール、バージョン管理を行うための Microsoft のコラボレーションツールです。 バージョン管理を使用すると、Azure DevOps および TFS は、独自のシステム (Team Foundation バージョン管理または TFVC)、または GitHub でホストされているプロジェクトと連携できます。

- Azure DevOps は、クラウド経由でサービスを提供します。 主な利点は、専用のハードウェアまたはインフラストラクチャを必要とせず、どこからでも web ブラウザーや Visual Studio などの一般的な開発ツールを介してアクセスできることです。地理的に分散しているチームにとって魅力があります. これは 5 人以下の開発者のチームまで無料で使用でき、その後、増大するチームに対応するために追加のライセンスを購入することができます。
- TFS は、オンプレミスな Windows サーバー用に設計されており、ネットワークにローカルネットワークまたは VPN 接続経由でアクセスします。 その主な利点は、ビルドサーバーの構成を完全に制御でき、追加のソフトウェアや必要なサービスを自由にインストールできることです。 TFS は、小規模なチーム向けに無料のエントリーレベルの Express edition があります。

TFS と Azure DevOps はどちらも Visual Studio と密接に統合されており、開発者は1つの IDE から快適に多くのバージョンコントロールと CI タスクを実行できます。 Team Explorer Everywhere plugin for Eclipse (下記参照) も使用できます。 Visual Studio for Mac には[、TFVC のプレビューが用意](/visualstudio/mac/tf-version-control/)されています。

[Azure DevOps パイプライン](https://docs.microsoft.com/azure/devops/pipelines/languages/xamarin/)では、Xamarin プロジェクトが直接サポートされており、対象とするプラットフォームごとにビルド定義を作成します (Android、IOS、Windows)。 適切な Xamarin ライセンスが各ビルド定義のために必要です。 この目的のために、Xamarin 対応のローカルの TFS ビルドサーバーを Azure DevOps に接続することもできます。 このセットアップでは、Azure DevOps のキューに登録されているビルドがローカルサーバーに委任されます。 詳細については、「[ビルドエージェントとリリースエージェント](https://docs.microsoft.com/azure/devops/pipelines/agents/agents)」を参照してください。 また、Jenkins や Team City などの別のビルドツールを使用することもできます。

Visual Studio、Azure DevOps、Team Foundation Server のすべてのアプリケーションライフサイクル管理 (ALM) 機能の完全な概要については、「 [DevOps With Xamarin Apps](https://docs.microsoft.com/visualstudio/cross-platform/application-lifecycle-management-alm-with-xamarin-apps)」を参照してください。

#### <a name="team-explorer-everywhere"></a>Team Explorer Everywhere

[Team Explorer Everywhere](https://docs.microsoft.com/azure/devops/java/download-eclipse-plug-in/)は、Visual Studio の外部で開発するチームに Team Foundation Server と Azure DevOps の機能をもたらします。 これにより、開発者がオンプレミスで、または OS X と Linux 版の Eclipse やクロスプラットフォームのコマンドラインから、クラウド内のチームプロジェクトに接続できるようになります。 Team Explorer Everywhere は、バージョン管理 (Gitを含む) 、作業アイテム、Windows 以外のプラットフォームのビルド機能への完全なアクセスを提供します。

#### <a name="git"></a>Git

[Git](http://git-scm.com)は 当初 Linux カーネルでソースコードを管理するために開発された一般的なオープン ソース バージョン管理ソリューションです。 これは、あらゆる規模のソフトウェア プロジェクトで人気のある非常に高速で柔軟なシステムです。 貧弱なインターネットアクセスの個人開発者から世界中に及ぶ大規模なチームまで簡単にスケーリングします。 Git はまた、とても簡単にブランチを作成でき、さらに最小限のリスクで開発の並列のストリームを促進することができます。

Git は、web ブラウザー、または Linux、Mac OSX、Windows で実行されている[GUI クライアント](http://git-scm.com/downloads/guis)を使用して完全に動作できます。 パブリック リポジトリは無料で、プライベート リポジトリは [有料プラン](https://github.com/pricing) が必要です。

現在のバージョンの Visual Studio for Windows と Mac では、Git のネイティブサポートが提供されています。 Microsoft では、以前のバージョンの Visual Studio で[Git のダウンロード可能な拡張機能](http://visualstudiogallery.msdn.microsoft.com/abafc7d6-dcaa-40f4-8a5e-d6724bdb980c)を提供しています。 前述のように、Azure DevOps と TFS では、TFVC ではなくバージョン管理に Git を使用できます。

#### <a name="subversion"></a>破壊

[Subversion](http://subversion.apache.org) (SVN) は、2000年から使用されている一般的なオープン ソース バージョン コントロール システムです。 SVN は、OS X、Windows、FreeBSD、Linux および Unix のすべての最新バージョンで動作します。 Visual Studio for Mac は、SVN に対するネイティブ サポートがあります。 Visual Studio に SVN サポートをもたらすサード パーティ製の拡張機能があります。

### <a name="continuous-integration-environments"></a>継続的インテグレーション環境

継続的インテグレーション環境のセットアップは、バージョン管理システムとビルドサービスを組み合わせることを意味します。  後者で最も一般的な2つとして以下のようなものがあります。

- [Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/)は、Azure DEVOPS と TFS のビルドシステムです。 これは Visual Studio と緊密に統合されており、開発者がビルドをトリガーしたり、自動的にテストを実行したり、結果を確認したりすることを容易にします。
- Jenkins は、あらゆる種類のソフトウェア開発をサポートするプラグインの豊富なエコシステムを備えたオープンソースの CI サーバーです。 これは Windows と Mac OS X で動作します。 Jenkins は、特定の IDE とは統合しません。 その代わり、web インターフェイスを通して設定と管理を行います。 Jenkins CI もインストールと設定は容易で、小規模なチームにとって魅力的です。

TFS/Azure DevOps を単独で使用することも、次のセクションで説明するように、Jenkins と TFS/Azure DevOps または Git を組み合わせて使用することもできます。

#### <a name="azure-devops-and-team-foundation-server"></a>Azure DevOps と Team Foundation Server

既に説明したように、Azure DevOps と Team Foundation Server には、バージョンコントロールとビルドサービスの両方が用意されています。 ビルドサービスは、常にターゲットプラットフォームごとに、Xamarin Business または Enterprise のライセンスが必要です。

Azure DevOps では、ターゲットプラットフォームごとに個別のビルド定義を作成し、そこに適切なライセンスを入力します。 構成が完了すると、Azure DevOps はクラウドでビルドとテストを実行します。 詳細については、「 [Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/) 」を参照してください。

Team Foundation Server では、特定のターゲット プラットフォームに対して次のようにビルドマシンを構成します。

- **Android と Windows:** Visual Studio と Xamarin ツール (Android と Windows の両方) をインストールし、Xamarin のライセンスでを構成します。 また Android SDK の場所を TFS ビルドエージェントが見つけられるサーバー上の共有の場所に移動する必要があります。 詳細については、[TFVC の構成](https://docs.microsoft.com/azure/devops/repos/tfvc/overview) を参照してください。
- **iOS と Xamarin:** 適切なライセンスを使用して、Visual Studio と Xamarin ツールを Windows server にインストールします。 そして、Visual Studio for Mac をネットワークアクセスが可能な Mac OS X マシンにインストールします。 これはビルドホストとして機能し、最終的なアプリのパッケージ (iOS の場合は IPS、OS X の場合は APP) を作成します。

次の図は、上記のトポロジを示しています。

[![](intro-to-ci-images/intro03-small.png "この図は次のようになっています。")](intro-to-ci-images/intro03.png#lightbox)

また、ローカルの TFS サーバーを Azure DevOps プロジェクトにリンクして、Azure DevOps のビルドがローカルサーバーに委任されるようにすることもできます。 詳細については、「[ビルドエージェントとリリースエージェント](https://docs.microsoft.com/azure/devops/pipelines/agents/agents/)」を参照してください。

#### <a name="azure-devops-and-jenkins"></a>Azure DevOps と Jenkins

Jenkins を使用してアプリをビルドする場合は、Azure DevOps または Team Foundation Server にコードを保存し、CI ビルドに対して Jenkins を引き続き使用することができます。 チーム プロジェクトの Git リポジトリにプッシュしたとき、または TFVC にコードをチェックインしたときに、Jenkins ビルドをトリガーできます。 詳細については、「 [Jenkins With Azure DevOps](https://docs.microsoft.com/azure/devops/service-hooks/services/jenkins)」を参照してください。

[![](intro-to-ci-images/intro04-small.png "Jenkins を使用してアプリをビルドする場合は、Azure DevOps または Team Foundation Server にコードを保存し、CI ビルドに対して Jenkins を引き続き使用することができます。")](intro-to-ci-images/intro04.png#lightbox)

#### <a name="git-and-jenkins"></a>Git と Jenkins

もう一つの一般的な CI 環境では、完全に OS X をベースにすることができます。 このシナリオは、ソースコート管理に Gitを、ビルドサーバーに Jenkins を使用します。 これらのどちらも Visual Studio for Mac がインストールされた単一の Mac OS X コンピュータで実行します。 これは、前のセクションで説明した Azure DevOps + Jenkins 環境とよく似ています。

[![](intro-to-ci-images/intro05-small.png "これは、前のセクションで説明した Azure DevOps + Jenkins 環境とよく似ています。")](intro-to-ci-images/intro05.png#lightbox)

> [!IMPORTANT]
> **Jenkins は[Microsoft がサポートしていません](~/cross-platform/troubleshooting/questions/xamarin-jenkins.md)。**
