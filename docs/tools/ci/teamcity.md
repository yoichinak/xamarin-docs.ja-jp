---
title: "Xamarin を使用したチームの市区町村を使用します。"
description: "このガイドを使って、TeamCity するモバイル アプリケーションをコンパイルして Xamarin Test Cloud に送信するには必要な手順を説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: AC2626CB-28A7-4808-B2A9-789D67899546
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 03/23/2017
ms.openlocfilehash: caff1fb834ade35e68eb19683e87788a4aa70740
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="using-team-city-with-xamarin"></a>Xamarin を使用したチームの市区町村を使用します。

_このガイドを使って、TeamCity するモバイル アプリケーションをコンパイルして Xamarin Test Cloud に送信するには必要な手順を説明します。_

説明したように、[継続的インテグレーションの概要](~/tools/ci/intro-to-ci.md)ガイド、継続的インテグレーション (CI) が役に立つ作業品質のモバイル アプリケーションを開発するときにします。 継続的インテグレーション サーバー ソフトウェアの実行可能な多くのオプションがあります。このガイドに焦点を当てます[TeamCity](http://www.jetbrains.com/teamcity/) JetBrains からです。

TeamCity インストールのいくつかの異なる順列があります。 これらのいくつかの一覧を次に示します。

- **Windows サービス**-TeamCity が起動する Windows サービスとして Windows を起動するときに、このシナリオでします。 IOS アプリケーションをコンパイルする Mac ビルド ホストと共に必要があります。

- **OS X 上のデーモンを起動して**– 概念的には、前の手順で説明されている Windows サービスとして実行するには非常に似ています。 既定で、ルート アカウントでビルドが実行されます。

- **OS X 上のユーザー アカウント**– TeamCity を起動するたびに、ユーザーがログインするユーザー アカウントで実行することができます。

前のシナリオでは、OS X 上のユーザー アカウントで TeamCity を実行しているです。 最も単純でセットアップする最も簡単です。

TeamCity を設定するのには、いくつかの手順があります。

- **TeamCity をインストールする**– TeamCity のインストールは、このガイドでは説明しません。 このガイドでは、TeamCity がインストールされ、ユーザー アカウントで実行されていることを前提としています。 手順については[TeamCity をインストールする](http://confluence.jetbrains.com/display/TCD8/Installation)は含まれて、 [TeamCity 8 ドキュメント](http://confluence.jetbrains.com/display/TCD8/TeamCity+Documentation)JetBrains でします。

- **ビルド サーバーを準備する**– この手順では、ツールに必要なソフトウェアをインストールしてに証明書が必要なモバイル アプリケーションを構築し、配布用に準備します。

- **ビルド スクリプトの作成**– この手順は必ずしも必要はありませんが、ビルド スクリプトが自動のアプリケーションの構築に役立ちます。 ビルド スクリプトを使用して発生する可能性し、継続的インテグレーションを練習がない場合でも、配布対象のバイナリを作成する一貫性のある、反復可能な手段を提供するビルドの問題のトラブルシューティングに役立ちます。

- **A TeamCity プロジェクトの作成**– 前の 3 つの手順を完了するは前に、すべてのメタ データを格納する TeamCity プロジェクトを作成する必要がありますをソース コードを取得、プロジェクトをコンパイルおよび Xamarin Test Cloud テストを送信する必要です。

# <a name="requirements"></a>必要条件

エクスペリエンスを[Xamarin Test Cloud](https://developer.xamarin.com/guides/testcloud)が必要です。

TeamCity 8.1 に関する知識が必要です。 TeamCity のインストールでは、このドキュメントの範囲外です。 TeamCity は OS X Mavericks 上がインストールされているし、ルート アカウントではなく通常のユーザー アカウントで実行するいると見なされます。

ビルド サーバーは、OS X、専用の継続的な統合を実行する、スタンドアロン コンピューターにする必要があります。 理想的には、ビルド サーバーは、データベース サーバー、web サーバー、または開発者のワークステーションなど、他の任意の役割を担当するできません。

> [!IMPORTANT]
> このガイドでは、Xamarin の「ヘッドレス」インストールは説明しません。

[!include[](~/tools/ci/includes/firewall-information.md)]

## <a name="preparing-the-build-server"></a>ビルド サーバーを準備します。

ビルド サーバーの構成の重要なステップでは、すべての必要なツール、ソフトウェア、およびモバイル アプリケーションを構築する証明書をインストールします。 ビルド サーバーにできるモバイル ソリューションをコンパイルし、テストを実行する必要があります。 構成の問題を最小限に抑えるには、ソフトウェアおよびツールを TeamCity をホストしている同じユーザー アカウントでインストールしてください。 必要な新機能の一覧を次に示します。

1. **Visual Studio for Mac** – これには、Xamarin.iOS および Xamarin.Android が含まれます。
2. **Xamarin コンポーネント ストアへのログイン**– これは省略可能な手順は、のみ必要なかどうか、アプリケーションには、Xamarin コンポーネント ストアからコンポーネントを使用します。 事前にこの時点で、コンポーネント ストアにログインする場合は、TeamCity ビルド アプリケーションをコンパイルしようとしました。 問題ができなくなります。
3. **Xcode** – Xcode は iOS アプリケーションをコンパイルして署名するために必要です。
4. **Xcode コマンド ライン ツール**– これはのインストールに関するセクションの手順 1. で説明されている、 [rbenv で更新 Ruby](https://developer.xamarin.com/guides/testcloud/calabash/updating-ruby-using-rbenv/)ガイドです。
5. **Id およびプロビジョニング プロファイルを署名**– 証明書をインポートし、XCode でプロファイルをプロビジョニングします。 Apple のガイドを参照してください[署名 Id のエクスポートとプロビジョニング プロファイル](https://developer.apple.com/library/ios/recipes/xcode_help-accounts_preferences/articles/export_signing_assets.html)詳細についてはします。
6. **Android キーストア**– TeamCity ユーザーは、つまりに、アクセス権を持っているディレクトリに必要な Android キーストアをコピー`~/Documents/keystores/MyAndroidApp1`です。
7. **Calabash** – これは、アプリケーションが Calabash を使用して作成されたテストしている場合のオプションの手順です。 参照してください、 [Calabash OS X Mavericks 上にインストールする](https://developer.xamarin.com/guides/testcloud/calabash/osx-installation/)ガイドおよび[rbenv で更新 Ruby](https://developer.xamarin.com/guides/testcloud/calabash/updating-ruby-using-rbenv/)についてガイドします。

次の図は、これらすべてのコンポーネントを示しています。

![](teamcity-images/image1.png "この図ではこれらすべてのコンポーネント")

すべてのソフトウェアがインストールされたら、ユーザー アカウントにログインし、すべてのソフトウェアが正しくインストールされた、動作していることを確認します。 ソリューションをコンパイルして、テストのクラウドにアプリケーションを送信する必要があります。 これが大幅に簡略化すればビルド スクリプトを実行して次のセクションで説明しました。

## <a name="create-a-build-script"></a>ビルド スクリプトを作成します。

TeamCity のコンパイルとそれ自体で Test Cloud のモバイル アプリケーションを送信するすべての側面を処理することが可能ですが、ビルド スクリプトの作成を強くお勧めします。 ビルド スクリプトは、次の利点を提供します。

1. **ドキュメント**– ビルド スクリプトは、ソフトウェアの構築方法についてのドキュメントの形式として機能します。 これは、アプリケーションの展開に関連付けられて、機能に集中する開発者は、「マジック」の一部を削除します。
1. **再現性**– スクリプトをビルドするたびにアプリケーションをコンパイルおよび展開されると、たまにまたはユーザーの作業の実行内容に関係なく、まったく同じ方法でことを確認します。 この反復可能な整合性は、すべての問題または正しく実行されるビルドのためにクリープ可能性がありますエラーやヒューマン エラーを削除します。
1. **バージョン管理**– ビルド スクリプトは、ソース管理システムに含めることができます。 これは、ビルド スクリプトへの変更の追跡、監視、およびエラーまたは情報の不一致が見つからない場合は修正をできることを意味します。
1. **環境を準備**– ビルド スクリプトが必要なサード パーティの依存関係をインストールするためのロジックを含めることができます。 適切なコンポーネントとアプリケーションの構築するようになります。

ビルド スクリプトは Windows で Powershell ファイルまたは (OS X) でバッシュ スクリプトと同じくらい簡単にできます。 ビルド スクリプトを作成する場合は、スクリプト言語のいくつかの選択肢があります。

- [**Rake** ](https://github.com/jimweirich/rake) – これは、ドメイン固有言語 (DSL) Ruby に基づくプロジェクトをビルドします。 Rake あり、人気の利点は、ライブラリの機能豊富なエコシステムです。

- [**psake** ](https://github.com/psake/psake) – これは、ソフトウェアを作成するための Windows Powershell ライブラリ

- [**偽の**](http://fsharp.github.io/FAKE/) – これは、f# で必要な場合は、既存の .NET ライブラリを利用できるようにベース DSL です。

使用するスクリプト言語は、ユーザー設定や要件によって異なります。 [TaskyPro Calabash](https://github.com/xamarin/test-cloud-samples/tree/master/TaskyPro/TaskyPro-Calabash)例にはとして Rake の使用例が含まれています、[ビルド スクリプト](https://github.com/xamarin/test-cloud-samples/blob/master/TaskyPro/TaskyPro-Calabash/Rakefile)です。


> [!NOTE]
> これは、表現力とソフトウェアの構築する専用の DSL の保守容易性が、MSBuild または NAnt、これらが不足しているなど、XML ベースのビルド システムを使用することです。

### <a name="parameterizing-the-build-script"></a>ビルド スクリプトのパラメーター化

ソフトウェアのテストのビルドとのプロセスには、秘密に保つ必要があります情報が必要です。 具体的には、APK の作成、キーストア、キーストア内のキーの別名、またはその両方のパスワードが必要です。 同様に、Test Cloud では、開発者に一意の API キーが必要です。 これらの種類の値の必要がありますされませんハードコーディングするビルド スクリプトのです。 代わりに変数としてビルド スクリプトに渡す必要があります。

重要度の低い値などは、iOS デバイス ID またはクラウドのテストが使用するデバイスのテストを識別する Android デバイスの ID を実行します。 これらは、保護する必要がある値ではありませんが、ビルドからビルドを変更することがあります。

ビルド スクリプトの外部変数のこれらの型を格納するもやすくなどを開発者と、組織内でビルド スクリプトを共有します。 開発者は、ビルド サーバーと同じスクリプトを使用することがありますが、キーストアと API キーを使用できます。

これらの機密性の高い値を格納するための 2 つの可能なオプションがあります。

- **構成ファイル**: バージョン管理にこの値をチェックしてはなりませんテスト クラウド API キーを保護します。 各マシンには、ファイルを作成できます。 このファイルから値を読み取る方法は、使用するスクリプト言語によって異なります。

- **環境変数**-コンピューター単位および基になるスクリプティング言語に依存しない ared で簡単に設定これらです。

これらの選択肢のそれぞれに長所と短所があります。 ビルド スクリプトを作成するときに、このガイドのこの手法によって推奨するため、TeamCity は環境変数とうまく機能します。

### <a name="build-steps"></a>ビルド ステップ

ビルド スクリプトは、次の手順を実行できる必要があります。

- **アプリケーションをコンパイル**– これは、正しいプロビジョニング プロファイルで、アプリケーションに署名が含まれています。

- **Xamarin Test Cloud に申請**– これは、署名と適切なキー ストアと APK の整列 zip に含まれます。

これら 2 つの手順についてで詳しく説明します。

#### <a name="compiling-a-xamarinios-application"></a>Xamarin.iOS アプリケーションのコンパイル

[!include[](~/tools/ci/includes/commandline-compile-of-xamarin-ios-ipa.md)]

#### <a name="compiling-a-xamarinandroid-application"></a>Xamarin.Android アプリケーションのコンパイル

Android アプリケーションをコンパイルする**xbuild** (または**msbuild** Windows 上)。

```bash
/Library/Frameworks/Mono.framework/Commands/xbuild /t:SignAndroidPackage /p:Configuration=Release /path/to/android.csproj
```

Xamarin Android アプリケーションをコンパイルすることに注意して**xbuild** iOS アプリケーションをビルドすることと、プロジェクトを使用して**xbuild**ソリューションが必要です。

#### <a name="submitting-xamarinuitests-to-test-cloud"></a>Test Cloud に Xamarin.UITests を送信します。

使用して UITests が送信される、`test-cloud.exe`次のスニペットに示すように、アプリケーション。

```bash
test-cloud.exe <path-to-apk-or-ipa-file> <test-cloud-team-api-key> --devices <device-selection-id> --assembly-dir <path-to-tests-containing-test-assemblies> --nunit-xml report.xml --user <email>
```

という NUnit スタイル XML ファイルの形式で、テスト結果が返されます、テストの実行時に**report.xml**です。 TeamCity ビルド ログに情報が表示されます。

Xamarin のマニュアルを参照する Test Cloud UITests を送信する方法の詳細については、 [Test Cloud テストを送信する](https://developer.xamarin.com/guides/testcloud/uitest/working-with/submitting-tests-to-xamarin-test-cloud/)です。

#### <a name="submitting-calabash-tests-to-test-cloud"></a>Calabash テスト クラウドに送信します。

使用して Calabash テストが送信される、 `test-cloud` gem、次のスニペットに示すようにします。

```bash
test-cloud submit /path/to/APK-or-IPA <test-cloud-team-api-key> --devices <device-id> --user <email>
```
Android アプリケーションをクラウドのテストを送信するには、まずサーバーを再構築、APK テスト calabash android を使用する必要があります。

```bash
$ calabash-android build </path/to/signed/APK>
$ test-cloud submit /path/to/APK <test-cloud-team-api-key> --devices <ANDROID_DEVICE_ID> --profile=android --config=config/cucumber.yml --pretty
```
Calabash テストの送信に関する詳細についてを参照してください Xamarin のガイドに[Calabash テスト Test Cloud を送信する](https://developer.xamarin.com/guides/testcloud/calabash/working-with/submitting-tests-to-xamarin-test-cloud/)です。

## <a name="creating-a-teamcity-project"></a>TeamCity プロジェクトを作成します。

TeamCity がインストールされているし、Visual Studio for Mac は、プロジェクトをビルドできます、TeamCity Test Cloud に送信して、プロジェクトをビルドするでプロジェクトを作成する時間を勧めします。

1. Web ブラウザーを介して TeamCity にログインして開始します。 ルート プロジェクトに移動します。

    ![](teamcity-images/image2.png "ルート プロジェクトに移動")ルート プロジェクトの下に新しいサブ プロジェクトを作成します。

    ![](teamcity-images/image3.png "ルート プロジェクトすぐ下に、ルート プロジェクトに移動し、新しいサブ プロジェクトの作成")
2. サブ プロジェクトが作成されたら、新しいビルド構成を追加します。

    ![](teamcity-images/image5.png "サブ プロジェクトを作成すると、追加、新しいビルド構成")
3. VC プロジェクトをビルド構成にアタッチします。 これは、バージョン コントロール設定画面上で行います。

    ![](teamcity-images/image6.png "バージョン コントロール設定画面を使用してこれを行う")

    作成の VC プロジェクトがない場合は、次に示す新しい VCS ルート ページから作成するのにはオプションがあります。

    ![](teamcity-images/image7.png "新しい VCS ルート ページから 1 つを作成するオプションの VC プロジェクトの作成がない場合があります。")

    VCS ルートが結び付けられる、TeamCity はチェック アウト プロジェクトしようとして自動ビルド ステップを検出します。 を TeamCity としている場合、検出されたビルド手順のいずれかを選択できます。 ここでは、検出されたビルドの手順を無視しても安全であります。

4. 次に、ビルドのトリガーを構成します。 これはキューに入れるビルドなど、ユーザーがコードをリポジトリにコミットするときに、特定の条件が満たされたときにします。 次のスクリーン ショットは、ビルド トリガーを追加する方法を示します。

    ![](teamcity-images/image8.png "このスクリーン ショットは、ビルドのトリガーを追加する方法を示します")ビルド トリガーを構成する例を次のスクリーン ショットに表示できます。

    ![](teamcity-images/image9.png "ビルド トリガーの構成の例は、このスクリーン ショットに表示できます。")

5. ビルド スクリプトのパラメーター化の前のセクションでは、いくつかの値を環境変数として保存することをお勧めします。 これらの変数は、パラメーターの画面上でビルド構成に追加できます。 テスト クラウド API キー、iOS デバイス ID、および Android のデバイス ID の次のスクリーン ショットに示すように、変数を追加します。

    ![](teamcity-images/image11.png "テスト クラウド API キー、iOS デバイス ID、および Android のデバイス ID、変数を追加します。")

6. 最後の手順では、アプリケーションとエンキュー Test Cloud にアプリケーションをコンパイルするビルド スクリプトを起動するビルド ステップを追加します。 次のスクリーン ショットは、アプリケーションを構築する、Rakefile を使用するビルド ステップの例を示します。

    ![](teamcity-images/image12.png "このスクリーン ショットは、アプリケーションをビルド、Rakefile を使用するビルド ステップの例")

7. この時点では、ビルド構成が完了しました。 プロジェクトが正しく構成されていることを確認するビルドをトリガーすることをお勧めします。 これを行うことをお勧め、意味のない小さな変更をリポジトリにコミットを開始します。 TeamCity は、コミットを検出し、ビルドを開始する必要があります。

8. ビルドが完了すると、ビルド ログを確認し、問題または注意が必要なビルドで警告があるかどうかを参照してください。

## <a name="summary"></a>まとめ

このガイドでは、TeamCity を使用して、Xamarin のモバイル アプリケーションを構築し、テストのクラウドに送信するにする方法について説明します。 ビルド プロセスを自動化するビルド スクリプトの作成について説明しました。 ビルド スクリプトはアプリケーションのコンパイル、テストのクラウドへの送信および結果の待機の対処します。

ビルドをキューは、開発者がコードをコミットするたびに、ビルド スクリプトを呼び出す TeamCity でプロジェクトを作成する方法を説明します。

## <a name="related-links"></a>関連リンク

- [Xamarin Test Cloud (UITest) にテストを送信します。](https://developer.xamarin.com~/testcloud/uitest/working-with/submitting-tests-to-xamarin-test-cloud/)
- [Xamarin Test Cloud (Calabash) にテストを送信します。](https://developer.xamarin.com~/testcloud/calabash/working-with/submitting-tests-to-xamarin-test-cloud/)
- [インストールと構成 TeamCity](http://confluence.jetbrains.com/display/TCD8/Installing+and+Configuring+the+TeamCity+Server)
