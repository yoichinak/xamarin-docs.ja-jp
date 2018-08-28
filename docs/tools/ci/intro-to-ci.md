---
title: Xamarin を使用した継続的インテグレーションの概要
description: このドキュメントでは、Xamarin を使用した継続的インテグレーションについて説明します。 これは、バージョン管理およびさまざまな継続的インテグレーション環境について説明します。
ms.prod: xamarin
ms.assetid: C034200E-2947-4309-9DDD-80DAC505C43F
author: topgenorth
ms.author: toopge
ms.date: 07/19/2017
ms.openlocfilehash: 67fc32fc9f79d54274642fbab2d0c2f8afd14d8c
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/28/2018
ms.locfileid: "37066508"
---
# <a name="introduction-to-continuous-integration-with-xamarin"></a>Xamarin を使用した継続的インテグレーションの概要

_継続的インテグレーションは、ソフトウェア エンジニア リングのプラクティスを自動化されたビルドがコンパイルし、必要に応じてコードを追加またはプロジェクトのバージョン管理リポジトリ内の開発者によって変更されたときにアプリをテストします。この記事は、継続的インテグレーションの一般的な概念といくつかのオプションの使用可能な継続的インテグレーション Xamarin プロジェクトで説明します。_

並列で作業する開発者向けのソフトウェア プロジェクトで一般的です。 これらすべてを統合する必要がある時点で、1 つの作業の並列ストリーム コードベース、最終的な製品を構成します。 ソフトウェア開発の初期の頃には、この統合は、プロジェクトは難しく、危険なプロセスの最後に実行されました。

継続的インテグレーション (CI)は、通常、開発者がプロジェクト共用コード・リポジトリの変更をチェックするたびに、継続的なベースに基づいて、共通コードへのすべての開発者の変更をマージするような複雑さを回避します。 各チェックインの自動ビルドをトリガーし、新しく導入されたコードが既存のコードを中断していないことを確認する自動テストを実行します。  この方法では、CI は、エラーおよび問題を直ちに解決し、各チームメンバーが互いの作業に合わせて最新の状態を保つようにします。 この結果、結束して安定したコードベースが得られます。

継続的インテグレーション システムでは、2 つの主要な部分があります。

-  **バージョン管理**– バージョン コントロール (VC)、ソース管理やソース コード管理とも呼ばれますが 1 つの共有リポジトリのすべてのプロジェクトのコードを統合し、すべてのファイルへのすべての変更の完全な履歴を保持します。 このリポジトリとも呼ば、*主要*または*マスター*分岐、最終的には、実稼働環境をビルドまたはリリース バージョンのアプリに使用されるソース コードが含まれています。 多くのオープン ソースと通常、チームまたは個人に広範に変更するか、マスター ブランチへのリスクなしの実験を実施セカンダリの分岐にコードのコピーを分岐に許可する、このタスクの商用の製品があります。 セカンダリの分岐の変更を検証すると後、できますまとめてマスター分岐にマージします。
-  **継続的インテグレーションサーバー** – の継続的インテグレーションサーバーはすべて、プロジェクトの成果物 (ソース コード、画像、ビデオ、データベース、自動テストなど) の収集、アプリをコンパイルおよび自動テストの実行を担当します。 繰り返しますが、多くのオープン ソースおよび商用の CI サーバー ツールがあります。

通常、開発者では、作業が実行される最初に、各自のワークステーション上の 1 つまたは複数の分岐の作業コピーがあります。 作業の適切なセットが完了したら、変更内容は「にチェックイン」または、適切な分岐は、それらを他の開発者の作業コピーを伝達するには、「コミット」します。 これは、どのチーム確実にすべて同一のコードで作業中です。

ここでも、継続的インテグレーションは、変更のコミットの機能により、CI サーバー プロジェクトをビルドし、ソース コードが正しいことを確認する自動テストを実行します。 CI サーバーが、担当開発者に通知ビルド エラーまたはエラーのテストがある場合は、(電子メール、IM、Twitter、Growl など)、問題を修正できるようにします。 (CI のサーバーも拒否することもできます。コミットした場合は、「ゲート チェックイン」と呼ばれるエラーが発生します。)

次の図は、このプロセスを示します。

[![](intro-to-ci-images/intro01-small.png "この図では、このプロセスを示しています。")](intro-to-ci-images/intro01.png#lightbox)

モバイル アプリでは、継続的インテグレーションの特異な課題を紹介します。 アプリには、物理デバイスで使用できる、GPS やカメラなどのセンサーが必要です。 さらに、シミュレーターまたはエミュレーターはハードウェアの近似にすぎません可能性がありますを非表示や問題がわかりにくくなります。 最後に、実際にお客様の準備完了であることを保証する実際のハードウェアでのモバイル アプリをテストする必要があります。

[アプリ Center Test](https://docs.microsoft.com/appcenter/test-cloud)数百台の物理デバイスに直接アプリのテストでこの特定の問題に対処します。 開発者は、強力な UI テストのため、自動承諾テストを記述します。 これらのテストは、アプリのセンターにアップロード CI サーバーに自動的に実行できます CI のプロセスの一部として次の図に示すように。

[![](intro-to-ci-images/intro02-small.png "これらのテストが App Center にアップロードされると、CI サーバーは、この図が示すように CI のプロセスの一部として自動的にそれらを実行できます. ")](intro-to-ci-images/intro02.png#lightbox)

# <a name="components-of-continuous-integration"></a>継続的インテグレーションの構成要素

構成項目をサポートするように設計および商用のオープン ソースのツールの広範なエコシステムです。 このセクションでは、いくつかの最も一般的な原因について説明します。

## <a name="version-control"></a>バージョン コントロール

### <a name="visual-studio-team-services-and-team-foundation-server"></a>Visual Studio Team Services と Team Foundation Server

[Visual Studio Team Services](https://visualstudio.microsoft.com/team-services/) (VSTS) および[Team Foundation Server](https://visualstudio.microsoft.com/tfs/) (TFS) は、継続的インテグレーションのコラボレーション ツールを Microsoft のサービス、タスクの追跡、アジャイル計画およびレポート作成ツール、およびバージョンのビルドコントロール。 バージョン管理、VSTS と TFS 作業ができ、独自のシステム Team Foundation バージョン管理 (TFVC) または GitHub でホストされるプロジェクトです。

 - Visual Studio Team Services では、クラウド経由でサービスを提供します。 主な利点は、専用のハードウェアやインフラストラクチャは必要ありませんからアクセスできる任意の場所と地理的には、チームに魅力的なので、Visual Studio などの一般的な開発ツールの web ブラウザーを通じて分散されます。 これは無料の 5 人の開発者のチーム以下後に、増大するチームを対応する追加するライセンスを購入することができます。
 - TFS は、内部設置型 Windows サーバー用に設計され、ローカル ネットワークまたはそのネットワークに VPN 接続経由でアクセスします。 その主な利点は、完全にビルド サーバーの構成を制御してインストールすることをどのような追加のソフトウェアまたはサービスが必要なです。 TFS では、小規模なチーム向けの空きエントリ レベル Express edition があります。

TFS および VSTS の両方が Visual Studio と密接に統合し、開発者に多くのバージョン管理と快適なシングル IDE 内から CI タスクを実行できるようにします。 Team Explorer Everywhere plugin for Eclipse (下記参照) も使用できます。 Mac 用の visual Studio では、TFS または VSTS の任意のサポートは提供しません。

ビジュアルの Studio チーム サービスのビルド システムが、Xamarin プロジェクトでは、(Android、iOS、および Windows) のターゲットにする各プラットフォームのビルド定義を作成する内部での直接サポートします。 Xamarin ライセンスを適切な各ビルド定義が必要です。 ローカルに接続することも、Xamarin 対応 TFS は、この目的のための Visual Studio Team Services にサーバーを構築します。 この設定により、VSTS をキューに入っているビルドは、ローカル サーバーに委任されます。 詳細についてを参照してください[配置ビルド サーバーを構成および](https://docs.microsoft.com/vsts/pipelines/agents/agents?view=vsts)です。 また、Jenkins またはチームの市区町村などの別のビルド ツールを使用することができます。

Visual Studio、Visual Studio Team Services、および Team Foundation Server では、参照のすべてのアプリケーション ライフ サイクル管理 (ALM) 機能の概要については、 [Xamarin アプリでのアプリケーション ライフ サイクル管理](https://msdn.microsoft.com/library/mt162217(v=vs.140).aspx)msdn です。


### <a name="team-explorer-everywhere"></a>Team Explorer Everywhere

[Team Explorer Everywhere](http://msdn.microsoft.com/library/gg413285.aspx)により Visual Studio の外部で開発チームが Team Foundation Server と Visual Studio Team Services の機能を導入します。 OS X、Linux for Eclipse またはクロス プラットフォーム コマンド ライン クライアントから内部設置方式かクラウド内のチーム プロジェクトに接続できます。 Team Explorer Everywhere は、完全では、(Git など)、バージョン管理へのアクセスを作業項目と、Windows 以外のプラットフォームの機能を構築します。


### <a name="git"></a>Git

[Git](http://git-scm.com)は Linux カーネルのソース コードの管理に開発された一般的なオープン ソース バージョンの管理ソリューションです。 これは、あらゆる規模のソフトウェア プロジェクトで一般的な方法です非常に高速、柔軟なシステムです。 簡単にスケーリングする不適切なインターネットにアクセスできる 1 人の開発者からに広がっている大規模なチームにします。 Git では、分岐非常に簡単ですが、さらに、リスクを最小化を使用した開発の並列のストリームを促進することができますも行われます。

完全 web ブラウザー、または Git を操作できる[GUI クライアント](http://git-scm.com/downloads/guis)Linux、Mac os X、および Windows 上で実行します。 パブリック リポジトリの無料です。プライベート リポジトリが必要な[有料プラン](https://github.com/pricing)です。

Visual Studio 2015 と Visual Studio for Mac Git; に対するネイティブ サポートを提供します。以前のバージョン、Microsoft が提供しています、 [Git のダウンロード可能な拡張](http://visualstudiogallery.msdn.microsoft.com/abafc7d6-dcaa-40f4-8a5e-d6724bdb980c)です。 前述のようは、TFVC の代わりにバージョン管理の Visual Studio Team Services と TFS が Git を使用できます。


### <a name="subversion"></a>Subversion

[Subversion](http://subversion.apache.org) (SVN) は、人気のオープン ソース バージョン コントロール システムを 2000年以降使用されています。 SVN は、OS X、Windows、FreeBSD、Linux および Unix のすべての最新バージョンで実行されます。 Visual Studio for Mac は、SVN に対するネイティブ サポートがします。 Visual Studio に SVN サポートをもたらすサード パーティ製の拡張機能があります。


## <a name="continuous-integration-environments"></a>継続的インテグレーション環境

継続的インテグレーション環境のセットアップでは、ビルド サービスとバージョン管理システムを組み合わせることを意味します。  後者の 2 つの最も一般的なものは。

- [Team Foundation ビルド](https://msdn.microsoft.com/Library/vs/alm/Build/overview)Visual Studio Team Services と TFS のビルド システムです。 緊密に統合されている Visual Studio でビルドをトリガーする開発者にとって便利にするためは自動的にテストを実行し、結果を参照してください。
- Jenkins はオープン ソース CI サーバーですべての種類のソフトウェア開発をサポートするプラグインの豊富なエコシステムを備えています。 Windows で実行され、Mac OS x マークの Jenkins が、特定の IDE と統合されていません。 代わりに、ように構成され、web インターフェイスを使用して管理されます。 Jenkins CI もインストールして構成される小規模なチームを魅力的なので簡単です。

TFS/VSTS を単独で使用することができますか、Jenkins を TFS/VSTS または Git のように、次のセクションで説明されていると組み合わせて使用することができます。

### <a name="visual-studio-team-services-and-team-foundation-server"></a>Visual Studio Team Services と Team Foundation Server

Visual Studio Team Services と Team Foundation Server のバージョンの両方は、前述のように、管理機能やサービスをビルドします。 ビルド サービスは、ターゲット プラットフォームごとに、Xamarin Business または Enterprise のライセンスを常に必要です。

Visual Studio Team Services では、ターゲット プラットフォームごとに別個のビルド定義を作成し、適切なライセンスがあるを入力します。 VSTS がビルドを実行構成されると、クラウドでテストおよびテストします。 参照してください[Team Foundation ビルド](https://msdn.microsoft.com/Library/vs/alm/Build/overview)詳細についてはします。

Team Foundation Server では、特定のターゲット プラットフォームの次のようにビルド コンピューターを構成します。

- **Android および Windows:** Visual Studio をインストールし、Xamarin の (Android および Windows 用両方) ツールし、Xamarin ライセンスを構成します。 TFS ビルド エージェント サーバーの共有の場所に Android SDK が見つけることができますを移動する必要もできます。 詳細については、「[構成 TFVC](https://docs.microsoft.com/vsts/tfvc/overview)です。
- **iOS および Xamarin:** Visual Studio をインストールし、適切なライセンスを持つ Windows server での Xamarin ツールです。 ビルド ホストとして使用され、最終的なアプリ パッケージ (iOS、OS X 向けアプリケーションの IPA) を作成するネットワーク経由でアクセスの Mac OS X コンピューターでは、Mac 用の Visual Studio をインストールし。

次の図は、このトポロジを示しています。

[![](intro-to-ci-images/intro03-small.png "この図では、このトポロジを示しています。")](intro-to-ci-images/intro03.png#lightbox)

VSTS ビルドは、ローカル サーバーに委任するように、Visual Studio Team Services プロジェクトにローカル TFS サーバーをリンクすることもできます。 詳細については、次を参照してください。[配置ビルド サーバーを構成および](http://msdn.microsoft.com/library/ms181712.aspx)msdn です。

### <a name="visual-studio-team-services-and-jenkins"></a>Visual Studio Team Services と Jenkins

Jenkins を使用して、アプリをビルドする場合は、Visual Studio Team Services または Team Foundation Server にコードを格納しを引き続き使用する Jenkins CI ビルドできます。 チーム プロジェクトの Git リポジトリまたはチェックインするときにコードを TFVC にコードをプッシュするときに、Jenkins ビルドをトリガーできます。 詳細については、「[の Visual Studio Team Services Jenkins](https://docs.microsoft.com/en-us/vsts/service-hooks/services/jenkins?view=vsts)です。

[![](intro-to-ci-images/intro04-small.png "Jenkins を使用して、アプリをビルドする場合は、Visual Studio Team Services または Team Foundation Server にコードを格納およびを引き続き使用する Jenkins CI ビルド")](intro-to-ci-images/intro04.png#lightbox)

### <a name="git-and-jenkins"></a>Git と Jenkins

別の一般的な CI 環境は、完全 OS X ベースで指定できます。 このシナリオには、Git およびを使用したソース コード管理 Jenkins ビルド サーバーが含まれます。 インストールされている Mac 用 Visual Studio での単一の Mac OS X コンピューターにこれらの両方の実行しています。 これは、Visual Studio Team Services + 前のセクションで説明する Jenkins 環境によく似ています。

[![](intro-to-ci-images/intro05-small.png "これは、Visual Studio Team Services と、前のセクションで説明する Jenkins 環境によく似ています、")](intro-to-ci-images/intro05.png#lightbox)

> [!IMPORTANT]
> **注: Jenkins は[Xamarin でサポートされていない](~/cross-platform/troubleshooting/questions/xamarin-jenkins.md)です。**


# <a name="summary"></a>まとめ

このドキュメントには、ソフトウェア開発チームにもたらす利点と継続的インテグレーションの概念が導入されました。 バージョン管理の重要度は、ロールと、ビルド サーバーの役割と一緒に説明しました。 ドキュメント、通っていたのソース コード管理に使用してサーバーを構築できる、ツールの一部について説明します。 により、開発者は、品質と、アプリの機能を証明する自動テストを実行して、優れたアプリを発行するアプリ Center テストを紹介します。 アプリとアプリ センターへのテストの送信に関するドキュメントをより詳細な[ここ](https://docs.microsoft.com/appcenter/test-cloud)です。 最後に、理解しやすく組織では継続的インテグレーションの設定がいくつかの異なる CI 環境がここで説明した組み合わせるには、これらすべてのツールとコンポーネント、方法です。 Xamarin プロジェクトで Visual Studio Team Services と Team Foundation Server の使用の詳細については、次を参照してください。[構成 TFVC](https://docs.microsoft.com/vsts/tfvc/overview)され、この[継続的インテグレーション概要](https://docs.microsoft.com/vsts/build-release/actions/ci-cd-part-1)です。 同様に、Jenkins を使用している場合を参照してください[Xamarin を使用して Jenkins](~/tools/ci/jenkins-walkthrough.md)の継続的インテグレーションを設定する方法の詳細情報。
