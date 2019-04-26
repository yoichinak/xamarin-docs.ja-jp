---
title: Microsoft Jenkins がサポートされていないのはなぜですか
description: このドキュメントには、Jenkins CI システムでの Xamarin の相互作用が大まかに言えば、について説明します。 Jenkins を使用する場合に生じるいくつかの一般的な問題についても説明します。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9951F980-2C6C-47C0-8A35-A78F06C20BEB
author: asb3993
ms.author: amburns
ms.date: 06/05/2018
ms.openlocfilehash: c2e409b796d5ef2525079e02aafdd0c6e8db5d81
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61158937"
---
# <a name="why-isnt-jenkins-supported-by-microsoft"></a>Microsoft Jenkins がサポートされていないのはなぜですか

## <a name="jenkins-support-explanation"></a>Jenkins のサポートの説明

Jenkins は、オープン ソースの CI スイートです。Jenkins によって直接発生する多くの問題があるため*自体*に対してコードを取得した; などの問題として提出する必要があります、 [Jenkins のメイン リポジトリ](https://github.com/jenkinsci/jenkins)、または、リポジトリの[Jenkins.app](https://github.com/stisti/jenkins-app)します。

Xamarin のツールでバグを特定するために分離可能性のある問題については、例外です。場合にこれが疑われる場合は、確認、[サポート オプション](~/cross-platform/troubleshooting/support-options.md)できたとして問題からチームでは、Xamarin サポート外のものがあるもう一度、*直接*を支援します。

## <a name="setup-jenkins-with-xamarin"></a>Xamarin を使用した Jenkins をセットアップします。

Jenkins の問題が私たちのチームで直接サポートされていない前述の中に[と Xamarin を使用して Jenkins](~/tools/ci/jenkins-walkthrough.md)ガイドは、Xamarin と統合されている Jenkins CI サーバーを設定して使用できます。 

## <a name="fixes-for-common-issues"></a>一般的な問題の修正プログラム

### <a name="jenkins-is-unable-to-find-the-android-sdk"></a>Jenkins は、Android SDK を見つけることができません。

この問題のエラー メッセージは、このようなものです。

> エラー XA5205:Android SDK ディレクトリが見つかりませんでした。 /P:AndroidSdkDirectory を使用して設定してください。

SDK の場所を設定するためのオプションは; を使用している正確な Android の Jenkins プラグインによって異なる場合があります。これを設定する方法を確認するに適してが、プラグインのガイドです。 たとえば、[Android エミュレーター プラグイン](https://wiki.jenkins-ci.org/display/JENKINS/Android+Emulator+Plugin#AndroidEmulatorPlugin-Systemconfiguration)場合は、SDK を自動的に検索が見つかりません。 そのプラグインを Jenkins システムの構成 ページで、場所を設定することもできます。 


## <a name="deprecated-errors"></a>非推奨のエラー

> [!IMPORTANT]
> Xamarin の最近のバージョンでは、この問題を解決されています。 ただし、ソフトウェアの最新バージョンで問題が発生した場合を提出してください、[新しいバグ](~/cross-platform/troubleshooting/questions/howto-file-bug.md)完全なバージョン管理情報と完全のビルド ログ出力します。



### <a name="jenkins-reports-an-invalid-xamarin-license"></a>Jenkins は、無効な Xamarin ライセンスを報告します。
この問題のエラー メッセージは、ようなもので、通常

> XA9008 エラー:コマンドラインからのビルドには、ビジネス ライセンスが必要です。

または

> エラー :Xamarin.iOS の Starter Edition では、Xamarin Studio の外部でビルドすることはできません。 

このシナリオの最も一般的な原因は、Xamarin ライセンスに関連付けられていないユーザー アカウントでログインして Jenkins の使用です。 これを解決する最も簡単な方法では、ユーザー アカウントを使用して直接アプリとして Jenkins をインストールします。 そのプロセスといくつか追加の考慮事項がここで説明します。 [https://forums.xamarin.com/discussion/comment/99397/#Comment_99397](https://forums.xamarin.com/discussion/comment/99397/#Comment_99397)
