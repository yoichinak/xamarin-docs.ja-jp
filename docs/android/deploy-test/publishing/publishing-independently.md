---
title: 個別公開
ms.prod: xamarin
ms.assetid: 6FB4DEF2-01AD-C5FE-0950-CE1BF088A9C6
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/21/2017
ms.openlocfilehash: 6d278f3ae046a31be6e4335119572fb509672a66
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="publishing-independently"></a>個別公開

既存の Android マーケットプレースを使用しないで、アプリケーションを公開することができます。 このセクションでは、これらの他の公開方法、および Xamarin.Android のライセンス レベルについて説明します。


## <a name="xamarin-licensing"></a>Xamarin のライセンス

次の 4 つのライセンスを、Xamarin.Android アプリの開発、配置、および配布に使用できます。

-   **Visual Studio Community** &ndash; Windows を使用する学生、小規模なチーム、およびオープン ソース ソフトウェア (OSS) の開発者向け。

-   **Visual Studio Professional** &ndash; 個々の開発者や小規模なチーム向け (Windows のみ)。 このライセンスは、標準またはクラウド サブスクリプション、Xamarin University の追加コンテンツへのアクセスを提供し、使用法制限はありません。

-   **Visual Studio Enterprise** &ndash; すべての規模のチーム向け (Windows のみ)。 このライセンスには、エンタープライズ機能、標準またはクラウド サブスクリプション、および Xamarin Test Cloud の 25% 割引が含まれています。

[visualstudio.com](https://www.visualstudio.com/xamarin/) にアクセスして Community Edition をダウンロードするか、Professional Edition と Enterprise Edition の購入に関する詳細を確認します。


## <a name="allow-installation-from-unknown-sources"></a>不明なソースからのインストールを許可する

既定では、Android は、ユーザーが Google Play 以外の場所からアプリケーションをダウンロードやインストールを行えないようにしています。 マーケットプレース以外のソースからのインストールを許可するには、ユーザーはアプリケーションをインストールする前に、デバイス上で *[不明なソース]* の設定を有効にする必要があります。 この設定は、次の図に示すように、**[設定] > [セキュリティ]** で確認できます。

[![セキュリティ設定画面](publishing-independently-images/settings.png)](publishing-independently-images/settings.png#lightbox)


> [!IMPORTANT]
> ネットワーク プロバイダーは、この設定に関係なく、不明なソースからのアプリケーションのインストールを禁止する可能性があります。



## <a name="publishing-by-e-mail"></a>電子メールでの発行

電子メールにリリース APK を添付するのは、アプリケーションをユーザーに配布する迅速かつ簡単な方法です。 ユーザーが Android を利用したデバイスで電子メールを開くと、Android は APK 添付ファイルを認識し、次の画像のような **[インストール]** ボタンが表示されます。

[![添付ファイルの [インストール] ボタン](publishing-independently-images/publishing-via-email.png)](publishing-independently-images/publishing-via-email.png#lightbox)

電子メールによる配布が簡単ですが、プライバシーや承認されていない配布に対してほとんど保護を提供しません。 アプリケーションの受信者が少なく、受信者がアプリケーションを配布しないことが信頼できる場合の予備の方法として最適です。


## <a name="publishing-by-web"></a>Web での発行

Web サーバーでアプリケーションを配布することができます。 これは、Web サーバーにアプリケーションをアップロードして、ユーザーにダウンロードのリンクを提供することで完了します。 Android を利用したデバイスでリンクを参照して、アプリケーションをダウンロードする場合、アプリケーションはダウンロードが完了すると、自動的にインストールされます。


## <a name="manually-installing-an-apk"></a>APK の手動インストール

手動インストールは、アプリケーションをインストールするための 3 番目のオプションです。 アプリケーションの手動インストールを有効にするには、次の操作を行います。

1.   **ユーザーに APK のコピーを配布する** &ndash; たとえば、このコピーは、CD または USB フラッシュ ドライブで配布される可能性があります。
1.   **(ユーザーが) Android デバイスにアプリケーションをインストールする** &ndash; コマンドラインの *Android Debug Bridge* (**adb**) ツールを使用します。 **adb** は、エミュレーター インスタンスまたは Android を利用したデバイスのいずれかとの通信を可能にする、多用途のコマンドライン ツールです。 Android SDK には、**adb** が含まれます。これは、**<sdk>/platform-tools/** ディレクトリで見つけることができます。

Android デバイスは、USB ケーブルを使用してコンピューターに接続されている必要があります。
また、Windows コンピューターが **adb** によって認識されるには、携帯電話のベンダーから追加の USB ドライバーも必要です。 これらの追加の USB ドライバーのインストール手順は、このドキュメントには含まれていません。

**adb** コマンドを発行する前に、存在する場合は、どのエミュレーター インスタンスまたはデバイスが接続されているかを確認すると役立ちます。 次のスニペットに示すように、`devices` コマンドを使用して添付される内容の一覧を表示できます。

```shell
$ adb devices
List of devices attached
        0149B2EC03012005device
```

接続されているデバイスが確認されると、**adb** と共に `install` コマンドを発行して、アプリケーションをインストールすることができます。

```shell
$ adb install <path-to-apk>
```

次のスニペットは、接続されているデバイスにアプリケーションをインストールする例を示しています。

```shell
$ adb install helloworld.apk
3772 KB/s (3013594 bytes in 0.780s)
        pkg: /data/local/tmp/helloworld.apk
Success
```

アプリケーションが既にインストールされている場合、`adb install` で APK をインストールすることはできません。また、次の例に示すように、エラーが報告されます。

```shell
$ adb install helloworld.apk
4037 KB/s (3013594 bytes in 0.728s)
        pkg: /data/local/tmp/helloworld.apk
Failure [INSTALL_FAILED_ALREADY_EXISTS]
```

デバイスからアプリケーションをアンインストールする必要があります。 最初に、次の `adb uninstall` コマンドを実行します。

```shell
adb uninstall <package_name>
```

次のスニペットは、アプリケーションをアンインストールする例を示しています。

```shell
$ adb uninstall mono.samples.helloworld
Success
```
