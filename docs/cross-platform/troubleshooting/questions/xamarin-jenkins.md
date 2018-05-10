---
title: なぜ Jenkins でサポートされていない Xamarin しますか。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9951F980-2C6C-47C0-8A35-A78F06C20BEB
author: asb3993
ms.author: amburns
ms.openlocfilehash: 37fc134f7e97af74f5bb019f3262972273f0c4cf
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/09/2018
---
# <a name="why-isnt-jenkins-supported-by-xamarin"></a>なぜ Jenkins でサポートされていない Xamarin しますか。

## <a name="jenkins-support-explanation"></a>Jenkins サポートの説明

Jenkins は、オープン ソースの CI スイートです。Jenkins によって直接発生は、このような多数の問題があるため*自体*に対してコードを取得した; などの問題としては扱わする必要があります、[メインの Jenkins リポジトリ](https://github.com/jenkinsci/jenkins)、またはのリポジトリ[Jenkins.app](https://github.com/stisti/jenkins-app)です。

この例外は、Xamarin のツールで特定のバグを分離することのある問題です。ケースであると思われる場合は、確認、[オプションをサポートして](~/cross-platform/troubleshooting/support-options.md)もう一度、問題があります、Xamarin のサポートの何か外部チームことができますが、*直接*有用です。

## <a name="setup-jenkins-with-xamarin"></a>Xamarin を使用したセットアップ Jenkins

Jenkins の問題が私たちのチームによって直接サポートされていない前述の中に[Xamarin を使用して Jenkins](~/tools/ci/jenkins-walkthrough.md)ガイドは、Xamarin と統合されている Jenkins CI サーバーを設定するために使用できます。 

## <a name="fixes-for-common-issues"></a>一般的な問題の修正プログラム
### <a name="jenkins-is-unable-to-find-the-android-sdk"></a>Jenkins は、Android SDK を見つけることができません。

この問題は、エラー メッセージは、次のようにします。

> エラー XA5205:、Android SDK ディレクトリが見つかりませんでした。 /P:AndroidSdkDirectory 経由で設定してください。

によって、正確な Jenkins Android プラグインを使用している SDK の場所を設定するためのオプションが異なる場合があります。これを設定する方法を確認する適切な場所は、プラグインのガイドでです。 例を示します。[Android エミュレーター プラグイン](https://wiki.jenkins-ci.org/display/JENKINS/Android+Emulator+Plugin#AndroidEmulatorPlugin-Systemconfiguration)場合は、SDK を自動的に検索検出できない以外の場所は、そのプラグインの Jenkins システムの構成ページを使用して設定することもできます。 


## <a name="deprecated-errors"></a>非推奨のエラー

> [!IMPORTANT]
> Xamarin の最近のバージョンでは、この問題を解決されています。 ただし、ソフトウェアの最新のバージョンで問題が発生した場合を送信してください、[新しいバグ](~/cross-platform/troubleshooting/questions/howto-file-bug.md)すべてのバージョン管理情報と完全のビルド ログ出力します。



### <a name="jenkins-reports-an-invalid-xamarin-license"></a>Jenkins レポートには、無効な Xamarin ライセンス
この問題のエラー メッセージは、通常はのようなもの

> XA9008 エラー: コマンドラインからのビルドには、ビジネス ライセンスが必要です。

または

> エラー: Xamarin.iOS のスタート エディションは Xamarin Studio の外部でビルドをサポートしません 

このシナリオの最も一般的な原因は、Xamarin ライセンスに関連付けられていないユーザー アカウントでログインして Jenkins の使用です。 これを解決する最も簡単な方法では、ユーザー アカウントを使用して直接アプリとして Jenkins をインストールします。 そのプロセスおよび他の考慮事項は、次に示します。 [https://forums.xamarin.com/discussion/comment/99397/#Comment_99397](https://forums.xamarin.com/discussion/comment/99397/#Comment_99397)
