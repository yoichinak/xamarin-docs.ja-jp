---
title: Xamarin を使用した継続的インテグレーションの概要
description: このドキュメントでは、Xamarin を使用した継続的インテグレーションについて説明します。 これは、バージョン管理およびさまざまな継続的インテグレーション環境について説明します。
ms.prod: xamarin
ms.assetid: C034200E-2947-4309-9DDD-80DAC505C43F
author: lobrien
ms.author: laobri
ms.date: 07/19/2017
ms.openlocfilehash: 44e45d38ecd98be6f75c619125f9c14ce707b251
ms.sourcegitcommit: 9e9340999d569a3db01b4b59a0fcf24b8caa869c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/11/2019
ms.locfileid: "59509889"
---
# <a name="introduction-to-continuous-integration-with-xamarin"></a>Xamarin を使用した継続的インテグレーションの概要

_継続的インテグレーションは、ソフトウェア エンジニア リングのプラクティスを自動化されたビルドがコンパイルし、必要に応じてコードを追加または開発者がプロジェクトのバージョン管理リポジトリに変更されたときにアプリをテストします。 この記事では、継続的インテグレーションの一般的な概念と、使用できるオプションの継続的インテグレーションの Xamarin プロジェクトでいくつかについて説明します。_

ソフトウェア プロジェクトでは開発者が並行して作業することが一般的です。 ある時点で、最終的な製品を構成する1つのコードベースに、これらの並行する作業の流れのすべてを統合する必要があります。 ソフトウェア開発の初期の頃は、この統合は、プロジェクトの最後に実行されていましたが、それは難しく危険な処理でした。

継続的インテグレーション (CI) は、通常、開発者がプロジェクトの共有コードリポジトリに変更をチェックインするたびに、すべての開発者の変更を共通のコードベースに継続的にマージすることで、このような複雑さを回避します。 各チェックインは、自動化されたビルドをトリガーし、新しく導入されたコードが既存のコードを壊していないかを確認するために、自動テストを実行します。  この方法で、CI はエラーや問題を直ちに表面化し、すべてのチームメンバーが互いの作業を合わせて最新の状態に保たれるようにします。 これにより結束的な安定したコードベースになります。

継続的インテグレーション システムでは、2 つの主要な部分があります。

- **バージョン コントロール** – バージョン コントロール (VC)、またはソースコントロールやソースコードマネージメントとも呼ばれるものは、1 つの共有リポジトリにすべてのプロジェクトのコードを統合し、すべてのファイルのすべての変更の完全な履歴を保持します。 このリポジトリは、よく *mainline* または *master* ブランチと呼ばれ、 アプリの本番環境またはリリースバージョンをビルドするために使われる最終的なソースコートが含まれています。 このタスクには多くのオープンソースと商用の製品があり、通常、チームまたは個人が、マスターブランチへのリスクなしで、広範囲の変更や検証の実施を行える二次的なブランチへコードのコピーを分岐することを許可しています。 二次的なブランチの変更が検証されると、それらはマスターブランチに全てまとめてマージすることができます。
- **継続的インテグレーション サーバー** – 継続的インテグレーション サーバーは、プロジェクトの全ての成果物（ソースコード、画像、動画、データベース、自動テストなど）の収集、アプリのコンパイル、自動テストの実行を担当します。 これも、多くのオープンソースおよび商用の CI サーバー ツールが存在します。

開発者は通常、各自のワークステーションに、1 つまたは複数のブランチの作業コピーを持っており、そこで最初のうちは作業します。 作業の適切なセットが完了したら、変更は適切なブランチへ "チェックイン" または "コミット" され、他の開発者の作業コピーへそれらを伝搬します。 このようにして、チームが同じコード上で全て作業できるようにします。

ここでも、継続的インテグレーションがあれば、変更をコミットする機能により、CI サーバーが、プロジェクトをビルドし、ソースコードの正当性を確認する自動テストを実行できるようになります。 ビルドエラーやテストの失敗がある場合は、CI サーバーが、担当開発者に通知します(電子メール、IM、Twitter、Growl など)。これによって開発者が問題を修正できるようにします。 (CI サーバーは、失敗があった場合にコミットを拒否することもできます。これは、「ゲート チェックイン」と呼ばれます。)

以下のダイアグラム図は、上記のプロセスを示します。

[![](intro-to-ci-images/intro01-small.png "この図では、このプロセスを示しています。")](intro-to-ci-images/intro01.png#lightbox)

モバイルアプリでは、継続的インテグレーションに特有の課題を導入します。 アプリには、物理デバイスでのみ使用できる、GPS やカメラなどのセンサーが要求されることがあります。 さらに、シミュレーターやエミュレーターは、ハードウェアの近似にすぎず、問題が隠れたり分かりにくくなったりします。 結局は、実機上のモバイルアプリをテストして、確実に顧客への準備ができていることを保証する必要があります。

[App Center Test](https://docs.microsoft.com/appcenter/test-cloud)は、数百台の物理デバイス上でアプリを直接テストすることで、この特有の問題に対処します。 開発者は、自動受諾テストを記述し、強力な UI テストを可能にします。 これらのテストが App Center にアップロードされると、CI サーバーは、次のダイアグラム図に示すように CI プロセスの一部として自動的にそれらを実行することができます。

[![](intro-to-ci-images/intro02-small.png "これらのテストは、App Center にアップロード、CI サーバーに自動的に実行できます CI プロセスの一部としてこの図に示すように")](intro-to-ci-images/intro02.png#lightbox)

## <a name="components-of-continuous-integration"></a>継続的インテグレーションの構成要素

CI をサポートするように設計された商用およびオープンソースのツールによる豊富なエコシステムがあります。 このセクションでは、最も一般的ないくつかのツールについて説明します。

### <a name="version-control"></a>バージョン コントロール

#### <a name="azure-devops-and-team-foundation-server"></a>Azure DevOps と Team Foundation Server

[Azure DevOps](https://azure.microsoft.com/services/devops/)と[Team Foundation Server](https://visualstudio.microsoft.com/tfs/) (TFS) は、継続的インテグレーションのマイクロソフトのコラボレーション ツールは、サービス、タスクの追跡、アジャイル計画およびレポート ツール、およびバージョン管理を構築します。 バージョン管理、Azure DevOps と TFS 作業できます独自のシステム (Team Foundation バージョン管理または TFVC) または GitHub でホストされるプロジェクト。

- Visual Studio Team Services では、クラウド経由でサービスを提供します。 主な利点は、専用のハードウェアやインフラストラクチャが不要で、どこからでも web ブラウザや Visual Studio などの一般的な開発ツールを通じてアクセスできることで、これは地理的に離れたチームにとって魅力的です。 これは 5 人以下の開発者のチームまで無料で使用でき、その後、増大するチームに対応するために追加のライセンスを購入することができます。
- TFS は、オンプレミスな Windows サーバー用に設計されており、ネットワークにローカルネットワークまたは VPN 接続経由でアクセスします。 その主な利点は、ビルドサーバーの構成を完全に制御でき、追加のソフトウェアや必要なサービスを自由にインストールできることです。 TFS は、小規模なチーム向けに無料のエントリーレベルの Express edition があります。

TFS と Azure DevOps の両方は Visual Studio と緊密に統合し、開発者に多くのバージョン管理との 1 つの IDE 内からの CI タスクを実行できるようにします。 Team Explorer Everywhere plugin for Eclipse (下記参照) も使用できます。 Visual Studio for Mac は[の TFVC を使用可能なプレビュー](/visualstudio/mac/tf-version-control/)します。

[Azure DevOps パイプライン](https://docs.microsoft.com/azure/devops/pipelines/languages/xamarin/)Xamarin プロジェクトでは、ターゲット (Android、iOS、および Windows) する各プラットフォームのビルド定義の作成を直接サポートしています。 適切な Xamarin ライセンスが各ビルド定義のために必要です。 ローカルに接続することも、Xamarin 対応の TFS は、この目的のため Azure DevOps へのサーバーをビルドします。 この設定により、Azure DevOps にキューに置かれたビルドは、ローカル サーバーに委任されます。 詳細については、[ビルドし、リリース エージェント](https://docs.microsoft.com/azure/devops/pipelines/agents/agents)します。 また、Jenkins や Team City などの別のビルドツールを使用することもできます。

Visual Studio、Azure DevOps、および Team Foundation Server では、参照のすべてのアプリケーション ライフ サイクル管理 (ALM) 機能の完全な概要[Xamarin アプリを使用した DevOps](https://docs.microsoft.com/visualstudio/cross-platform/application-lifecycle-management-alm-with-xamarin-apps)します。

#### <a name="team-explorer-everywhere"></a>Team Explorer Everywhere

[Team Explorer Everywhere](https://docs.microsoft.com/azure/devops/java/download-eclipse-plug-in/) により Visual Studio 外のチーム開発に、Team Foundation Server と Visual Studio Team Services の機能を利用できます。 これにより、開発者がオンプレミスで、または OS X と Linux 版の Eclipse やクロスプラットフォームのコマンドラインから、クラウド内のチームプロジェクトに接続できるようになります。 Team Explorer Everywhere は、バージョン管理 (Gitを含む) 、作業アイテム、Windows 以外のプラットフォームのビルド機能への完全なアクセスを提供します。

#### <a name="git"></a>Git

[Git](http://git-scm.com)は 当初 Linux カーネルでソースコードを管理するために開発された一般的なオープン ソース バージョン管理ソリューションです。 これは、あらゆる規模のソフトウェア プロジェクトで人気のある非常に高速で柔軟なシステムです。 貧弱なインターネットアクセスの個人開発者から世界中に及ぶ大規模なチームまで簡単にスケーリングします。 Git はまた、とても簡単にブランチを作成でき、さらに最小限のリスクで開発の並列のストリームを促進することができます。

Git は、web ブラウザーや Linux、Mac OSX、Windows 上の [GUI クライアント](http://git-scm.com/downloads/guis) で完全に操作できます。 パブリック リポジトリは無料で、プライベート リポジトリは [有料プラン](https://github.com/pricing) が必要です。

Visual Studio for Windows と Mac の現在のバージョンでは、Git のネイティブ サポートを提供します。 Microsoft が提供、 [Git のダウンロード可能な拡張機能](http://visualstudiogallery.msdn.microsoft.com/abafc7d6-dcaa-40f4-8a5e-d6724bdb980c)Visual Studio の以前のバージョン。 前述したように、Visual Studio Team Services と TFS は、TFVC の代わりにバージョン管理として Git を使用できます。

#### <a name="subversion"></a>Subversion

[Subversion](http://subversion.apache.org) (SVN) は、2000年から使用されている一般的なオープン ソース バージョン コントロール システムです。 SVN は、OS X、Windows、FreeBSD、Linux および Unix のすべての最新バージョンで動作します。 Visual Studio for Mac は、SVN に対するネイティブ サポートがあります。 Visual Studio に SVN サポートをもたらすサード パーティ製の拡張機能があります。

### <a name="continuous-integration-environments"></a>継続的インテグレーション環境

継続的インテグレーション環境のセットアップは、バージョン管理システムとビルドサービスを組み合わせることを意味します。  後者で最も一般的な2つとして以下のようなものがあります。

- [Azure のパイプライン](https://docs.microsoft.com/azure/devops/pipelines/)は Azure DevOps と TFS のビルド システムです。 これは Visual Studio と緊密に統合されており、開発者がビルドをトリガーしたり、自動的にテストを実行したり、結果を確認したりすることを容易にします。
- Jenkins はオープン ソース CI サーバーで、すべての種類のソフトウェア開発をサポートするプラグインの豊富なエコシステムを備えています。 これは Windows と Mac OS X で動作します。 Jenkins は、特定の IDE とは統合しません。 その代わり、web インターフェイスを通して設定と管理を行います。 Jenkins CI もインストールと設定は容易で、小規模なチームにとって魅力的です。

TFS/Azure DevOps を使用するには、単独でまたは、次のセクションで説明されていると、TFS または Azure DevOps または Git での組み合わせに Jenkins を使用できます。

#### <a name="visual-studio-team-services-and-team-foundation-server"></a>Visual Studio Team Services と Team Foundation Server

前述したように、Visual Studio Team Services と Team Foundation Server は、バージョン管理とビルドサービスの両方を提供します。 ビルドサービスは、常にターゲットプラットフォームごとに、Xamarin Business または Enterprise のライセンスが必要です。

Visual Studio Team Services では、ターゲットプラットフォームごとに個別のビルド定義を作成し、そこに適切なライセンスを入力します。 構成されている、Azure DevOps し実行をビルドおよびクラウドでテストします。 参照してください[Azure パイプライン](https://docs.microsoft.com/azure/devops/pipelines/)の詳細。

Team Foundation Server では、特定のターゲット プラットフォームに対して次のようにビルドマシンを構成します。

- **Android および Windows。**(Android および Windows 両方) の Visual Studio と Xamarin ツールをインストールし、Xamarin ライセンスを構成します。 また Android SDK の場所を TFS ビルドエージェントが見つけられるサーバー上の共有の場所に移動する必要があります。 詳細については、[TFVC の構成](https://docs.microsoft.com/azure/devops/repos/tfvc/overview) を参照してください。
- **iOS、および Xamarin。** 適切なライセンスを持つ Windows server には、Visual Studio と Xamarin ツールをインストールします。 そして、Visual Studio for Mac をネットワークアクセスが可能な Mac OS X マシンにインストールします。 これはビルドホストとして機能し、最終的なアプリのパッケージ (iOS の場合は IPS、OS X の場合は APP) を作成します。

次の図は、上記のトポロジを示しています。

[![](intro-to-ci-images/intro03-small.png "この図では、このトポロジを示しています。")](intro-to-ci-images/intro03.png#lightbox)

プロジェクトは Azure DevOps を構築するため、ローカル サーバーに委任は、Visual Studio Team Services にローカル、TFS サーバーをリンクすることもできます。 詳細については、次を参照してください。[ビルドとリリース エージェント](https://docs.microsoft.com/azure/devops/pipelines/agents/agents/)します。

#### <a name="visual-studio-team-services-and-jenkins"></a>Visual Studio Team Services と Jenkins

Jenkins を使用して、アプリをビルドする場合は、Visual Studio Team Services または Team Foundation Server にコードを保存し、CI ビルドとして Jenkins を使い続けることができます。 チーム プロジェクトの Git リポジトリにプッシュしたとき、または TFVC にコードをチェックインしたときに、Jenkins ビルドをトリガーできます。 詳細については、次を参照してください。 [Azure DevOps での Jenkins](https://docs.microsoft.com/azure/devops/service-hooks/services/jenkins)します。

[![](intro-to-ci-images/intro04-small.png "Jenkins を使用して、アプリを構築する場合は、コード Visual Studio Team Services または Team Foundation Server に格納して引き続き CI ビルドに Jenkins を使用するには")](intro-to-ci-images/intro04.png#lightbox)

#### <a name="git-and-jenkins"></a>Git と Jenkins

もう一つの一般的な CI 環境では、完全に OS X をベースにすることができます。 このシナリオは、ソースコート管理に Gitを、ビルドサーバーに Jenkins を使用します。 これらのどちらも Visual Studio for Mac がインストールされた単一の Mac OS X コンピュータで実行します。 これは、前のセクションで説明した Visual Studio Team Services + Jenkins の環境によく似ています。

[![](intro-to-ci-images/intro05-small.png "これは、Visual Studio Team Services と、前のセクションで説明する Jenkins 環境を非常に似ています")](intro-to-ci-images/intro05.png#lightbox)

> [!IMPORTANT]
> **Jenkins は[Microsoft でサポートされていない](~/cross-platform/troubleshooting/questions/xamarin-jenkins.md)します。**
