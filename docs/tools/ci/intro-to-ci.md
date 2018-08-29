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

_継続的インテグレーションは、プロジェクトのバージョン管理リポジトリで開発者によってコードに変更や追加が加えられた場合に、自動化されたビルドがアプリをコンパイルし必要に応じてテストするというソフトウェア工学の実践です。この記事は、継続的インテグレーションの一般的な概念と、Xamarin プロジェクトの継続的インテグレーションに利用できるいくつかの選択を説明します。_

ソフトウェア プロジェクトでは開発者が並行して作業することが一般的です。 ある時点で、最終的な製品を構成する1つのコードベースに、これらの並行する作業の流れのすべてを統合する必要があります。 ソフトウェア開発の初期の頃は、この統合は、プロジェクトの最後に実行されていましたが、それは難しく危険な処理でした。

継続的インテグレーション (CI) は、通常、開発者がプロジェクトの共有コードリポジトリに変更をチェックインするたびに、すべての開発者の変更を共通のコードベースに継続的にマージすることで、このような複雑さを回避します。 各チェックインは、自動化されたビルドをトリガーし、新しく導入されたコードが既存のコードを壊していないかを確認するために、自動テストを実行します。  この方法で、CI はエラーや問題を直ちに表面化し、すべてのチームメンバーが互いの作業を合わせて最新の状態に保たれるようにします。 これにより結束的な安定したコードベースになります。

継続的インテグレーション システムでは、2 つの主要な部分があります。

-  **バージョン コントロール** – バージョン コントロール (VC)、またはソースコントロールやソースコードマネージメントとも呼ばれるものは、1 つの共有リポジトリにすべてのプロジェクトのコードを統合し、すべてのファイルのすべての変更の完全な履歴を保持します。 このリポジトリは、よく *mainline* または *master* ブランチと呼ばれ、 アプリの本番環境またはリリースバージョンをビルドするために使われる最終的なソースコートが含まれています。 このタスクには多くのオープンソースと商用の製品があり、通常、チームまたは個人が、マスターブランチへのリスクなしで、広範囲の変更や検証の実施を行える二次的なブランチへコードのコピーを分岐することを許可しています。 二次的なブランチの変更が検証されると、それらはマスターブランチに全てまとめてマージすることができます。
-  **継続的インテグレーション サーバー** – 継続的インテグレーション サーバーは、プロジェクトの全ての成果物（ソースコード、画像、動画、データベース、自動テストなど）の収集、アプリのコンパイル、自動テストの実行を担当します。 これも、多くのオープンソースおよび商用の CI サーバー ツールが存在します。

開発者は通常、各自のワークステーションに、1 つまたは複数のブランチの作業コピーを持っており、そこで最初のうちは作業します。 作業の適切なセットが完了したら、変更は適切なブランチへ "チェックイン" または "コミット" され、他の開発者の作業コピーへそれらを伝搬します。 このようにして、チームが同じコード上で全て作業できるようにします。

ここでも、継続的インテグレーションがあれば、変更をコミットする機能により、CI サーバーが、プロジェクトをビルドし、ソースコードの正当性を確認する自動テストを実行できるようになります。 ビルドエラーやテストの失敗がある場合は、CI サーバーが、担当開発者に通知します(電子メール、IM、Twitter、Growl など)。これによって開発者が問題を修正できるようにします。 (CI サーバーは、失敗があった場合にコミットを拒否することもできます。これは、「ゲート チェックイン」と呼ばれます。)

以下のダイアグラム図は、上記のプロセスを示します。

[![](intro-to-ci-images/intro01-small.png "このダイアグラム図では、上記のプロセスを示しています。")](intro-to-ci-images/intro01.png#lightbox)

モバイルアプリでは、継続的インテグレーションに特有の課題を導入します。 アプリには、物理デバイスでのみ使用できる、GPS やカメラなどのセンサーが要求されることがあります。 さらに、シミュレーターやエミュレーターは、ハードウェアの近似にすぎず、問題が隠れたり分かりにくくなったりします。 結局は、実機上のモバイルアプリをテストして、確実に顧客への準備ができていることを保証する必要があります。

[App Center Test](https://docs.microsoft.com/appcenter/test-cloud)は、数百台の物理デバイス上でアプリを直接テストすることで、この特有の問題に対処します。 開発者は、自動受諾テストを記述し、強力な UI テストを可能にします。 これらのテストが App Center にアップロードされると、CI サーバーは、次のダイアグラム図に示すように CI プロセスの一部として自動的にそれらを実行することができます。

[![](intro-to-ci-images/intro02-small.png "これらのテストが App Center にアップロードされると、CI サーバーは、この図が示すように CI のプロセスの一部として自動的にそれらを実行できます. ")](intro-to-ci-images/intro02.png#lightbox)

# <a name="components-of-continuous-integration"></a>継続的インテグレーションの構成要素

CI をサポートするように設計された商用およびオープンソースのツールによる豊富なエコシステムがあります。 このセクションでは、最も一般的ないくつかのツールについて説明します。

## <a name="version-control"></a>バージョン コントロール

### <a name="visual-studio-team-services-and-team-foundation-server"></a>Visual Studio Team Services と Team Foundation Server

[Visual Studio Team Services](https://visualstudio.microsoft.com/team-services/) (VSTS) および [Team Foundation Server](https://visualstudio.microsoft.com/tfs/) (TFS) は、継続的インテグレーション ビルドサービスである、タスクの追跡・アジャイルの計画・レポート作成のツール、およびバージョンコントロールのための Microsoft のコラボレーションツールです。 バージョンコントロールを使うと、VSTS と TFS は独自のシステム (Team Foundation Version Control または TFVC) または GitHubでホストされるプロジェクトで作業ができます。

 - Visual Studio Team Services では、クラウド経由でサービスを提供します。 主な利点は、専用のハードウェアやインフラストラクチャが不要で、どこからでも web ブラウザや Visual Studio などの一般的な開発ツールを通じてアクセスできることで、これは地理的に離れたチームにとって魅力的です。 これは 5 人以下の開発者のチームまで無料で使用でき、その後、増大するチームに対応するために追加のライセンスを購入することができます。
 - TFS は、オンプレミスな Windows サーバー用に設計されており、ネットワークにローカルネットワークまたは VPN 接続経由でアクセスします。 その主な利点は、ビルドサーバーの構成を完全に制御でき、追加のソフトウェアや必要なサービスを自由にインストールできることです。 TFS は、小規模なチーム向けに無料のエントリーレベルの Express edition があります。

TFS および VSTS の両方は Visual Studio と密接に統合しており、開発者に多くのバージョン管理と CI タスクを 1つの IDE 内から快適に実行できるようにします。 Team Explorer Everywhere plugin for Eclipse (下記参照) も使用できます。 Visual Studio for Mac では、TFS または VSTS のサポートは提供しません。

Visual Studio Team Service のビルドシステムは、Xamarin プロジェクトを直接サポートします。その中で、希望するターゲット (Android、iOS、および Windows) の各プラットフォームのビルド定義を作成します。 適切な Xamarin ライセンスが各ビルド定義のために必要です。 この目的のために、ローカルや Xamarin 対応 TFS ビルドサーバーを Visual Studio Team Service に接続することも可能です。 この設定により、VSTS のキューに入っているビルドは、ローカル サーバーに委任されます。 詳細については [ビルドサーバーのデプロイと構成](https://docs.microsoft.com/vsts/pipelines/agents/agents?view=vsts) を参照してください。 また、Jenkins や Team City などの別のビルドツールを使用することもできます。

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

[![](intro-to-ci-images/intro04-small.png "Jenkins を使用して、アプリをビルドする場合は、Visual Studio Team Services または Team Foundation Server にコードを保存し、CI ビルドとして Jenkins を使い続けることができます. ")](intro-to-ci-images/intro04.png#lightbox)

### <a name="git-and-jenkins"></a>Git と Jenkins

別の一般的な CI 環境は、完全 OS X ベースで指定できます。 このシナリオには、Git およびを使用したソース コード管理 Jenkins ビルド サーバーが含まれます。 インストールされている Mac 用 Visual Studio での単一の Mac OS X コンピューターにこれらの両方の実行しています。 これは、Visual Studio Team Services + 前のセクションで説明する Jenkins 環境によく似ています。

[![](intro-to-ci-images/intro05-small.png "これは、前のセクションで説明した Visual Studio Team Services + Jenkins の環境によく似ています. ")](intro-to-ci-images/intro05.png#lightbox)

> [!IMPORTANT]
> **注: Jenkins は[Xamarin でサポートされていない](~/cross-platform/troubleshooting/questions/xamarin-jenkins.md)です。**


# <a name="summary"></a>まとめ

このドキュメントには、ソフトウェア開発チームにもたらす利点と継続的インテグレーションの概念が導入されました。 バージョン管理の重要度は、ロールと、ビルド サーバーの役割と一緒に説明しました。 ドキュメント、通っていたのソース コード管理に使用してサーバーを構築できる、ツールの一部について説明します。 により、開発者は、品質と、アプリの機能を証明する自動テストを実行して、優れたアプリを発行するアプリ Center テストを紹介します。 アプリとアプリ センターへのテストの送信に関するドキュメントをより詳細な[ここ](https://docs.microsoft.com/appcenter/test-cloud)です。 最後に、理解しやすく組織では継続的インテグレーションの設定がいくつかの異なる CI 環境がここで説明した組み合わせるには、これらすべてのツールとコンポーネント、方法です。 Xamarin プロジェクトで Visual Studio Team Services と Team Foundation Server の使用の詳細については、次を参照してください。[構成 TFVC](https://docs.microsoft.com/vsts/tfvc/overview)され、この[継続的インテグレーション概要](https://docs.microsoft.com/vsts/build-release/actions/ci-cd-part-1)です。 同様に、Jenkins を使用している場合を参照してください[Xamarin を使用して Jenkins](~/tools/ci/jenkins-walkthrough.md)の継続的インテグレーションを設定する方法の詳細情報。
