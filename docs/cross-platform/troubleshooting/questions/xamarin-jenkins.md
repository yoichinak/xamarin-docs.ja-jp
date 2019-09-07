---
title: Jenkins が Microsoft によってサポートされないのはなぜですか。
description: このドキュメントでは、大まかな Jenkins CI システムとの対話について説明します。 また、Jenkins を使用するときに発生するいくつかの一般的な問題についても説明します。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9951F980-2C6C-47C0-8A35-A78F06C20BEB
author: conceptdev
ms.author: crdun
ms.date: 06/05/2018
ms.openlocfilehash: a8dc27574dc9959cc375a98fc0d7a18aac8bd6b7
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70756969"
---
# <a name="why-isnt-jenkins-supported-by-microsoft"></a>Jenkins が Microsoft によってサポートされないのはなぜですか。

## <a name="jenkins-support-explanation"></a>Jenkins のサポートの説明

Jenkins はオープンソースの CI スイートです。このため、Jenkins*自体*によって直接発生する多くの問題は、コードを入手した場所に対する問題としてファイリングする必要があります。たとえば、[メインの Jenkins リポジトリ](https://github.com/jenkinsci/jenkins)や[Jenkins](https://github.com/stisti/jenkins-app)のリポジトリなどです。

この例外は、Xamarin のツールで特定のバグに分離される可能性のある問題を対象としています。この問題が発生したと思われる場合は、[サポートオプション](~/cross-platform/troubleshooting/support-options.md)を確認することができます。ただし、この問題は、Xamarin サポートチームが*直接*支援できるもの以外のものである可能性があります。

## <a name="setup-jenkins-with-xamarin"></a>Xamarin を使用した Jenkins のセットアップ

前述のように、Jenkins の問題はチームが直接サポートしていません。xamarin での[Jenkins の使用](~/tools/ci/jenkins-walkthrough.md)に関するガイドを使用して、xamarin と統合された Jenkins CI サーバーを設定できます。 

## <a name="fixes-for-common-issues"></a>一般的な問題の修正

### <a name="jenkins-is-unable-to-find-the-android-sdk"></a>Jenkins が Android SDK を見つけることができません

この問題のエラーメッセージは次のようになります。

> エラー XA5205:Android SDK ディレクトリが見つかりませんでした。 /P を使用して設定してください: AndroidSdkDirectory

SDK の場所を設定するためのオプションは、使用している Jenkins Android プラグインによって異なる場合があります。この設定方法については、「プラグインガイド」を参照してください。 たとえば、次のようになります。[Android Emulator プラグイン](https://wiki.jenkins-ci.org/display/JENKINS/Android+Emulator+Plugin#AndroidEmulatorPlugin-Systemconfiguration)は SDK を自動的に検索しますが、見つからない場合は検出します。この場所は、そのプラグインの [Jenkins システム構成] ページで設定することもできます。 

## <a name="deprecated-errors"></a>非推奨のエラー

> [!IMPORTANT]
> この問題は、最新バージョンの Xamarin で解決されました。 ただし、最新バージョンのソフトウェアで問題が発生した場合は、完全なバージョン管理情報と完全ビルドログ出力を使用して[新しいバグを作成](~/cross-platform/troubleshooting/questions/howto-file-bug.md)してください。

### <a name="jenkins-reports-an-invalid-xamarin-license"></a>Jenkins は無効な Xamarin ライセンスを報告します
通常、この問題のエラーメッセージは次のようになります。

> XA9008 エラー:コマンドラインから構築するには、ビジネスライセンスが必要です

または

> エラー :Xamarin の Starter Edition では、Xamarin Studio 外でのビルドはサポートされていません 

このシナリオの最も一般的な原因は、Xamarin ライセンスに関連付けられていないユーザーアカウントでログインすることによって Jenkins を使用することです。 これを解決する最も簡単な方法は、ユーザーアカウントを使用して直接 Jenkins をアプリとしてインストールすることです。 そのプロセスとその他の考慮事項については、次を参照してください。[https://forums.xamarin.com/discussion/comment/99397/#Comment_99397](https://forums.xamarin.com/discussion/comment/99397/#Comment_99397)
