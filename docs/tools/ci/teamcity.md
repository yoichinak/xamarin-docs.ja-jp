---
title: Xamarin での TeamCity の使用
description: このガイドでは、モバイル アプリケーションをコンパイルし、Xamarin Test Cloud に送信する TeamCity の使用するための手順を説明します。
ms.prod: xamarin
ms.assetid: AC2626CB-28A7-4808-B2A9-789D67899546
author: lobrien
ms.author: laobri
ms.date: 03/23/2017
ms.openlocfilehash: a3cb79f33b64d933aa8ab4d3555479cc16238992
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50118267"
---
# <a name="using-team-city-with-xamarin"></a>Xamarin での TeamCity の使用

_このガイドでは、モバイル アプリケーションをコンパイルし、Xamarin Test Cloud に送信する TeamCity の使用するための手順を説明します。_

説明したように、[継続的な統合の概要](~/tools/ci/intro-to-ci.md)ガイド、継続的インテグレーション (CI) が役に立つ作業品質のモバイル アプリケーションを開発するときにします。 継続的インテグレーション サーバー ソフトウェアの実行可能な多くのオプションがあります。このガイドに焦点[TeamCity](http://www.jetbrains.com/teamcity/) JetBrains から。

TeamCity のインストールのさまざまな順列をいくつかあります。 これらのいくつかの一覧を次には。

- **Windows サービス**– TeamCity が起動する Windows サービスとして Windows を起動する場合、このシナリオでします。 IOS アプリケーションをコンパイルする Mac ビルド ホストと組み合わせる必要があります。

- **OS X 上のデーモンを起動**– 概念的には、これは、前の手順で説明されている Windows サービスとしての実行に非常に似ています。 既定で、ルート アカウントで、ビルドが実行されます。

- **OS X 上のユーザー アカウント**– TeamCity を起動するたびに、ユーザーがログインするユーザー アカウントで実行することはできます。

前のシナリオのユーザー アカウントで OS X での TeamCity の実行は、最も簡単なセットアップする最も簡単なです。

TeamCity の設定には、いくつかの手順があります。

- **TeamCity インストール**– TeamCity のインストールは、このガイドでは説明しません。 このガイドでは、TeamCity がインストールされ、ユーザー アカウントで実行されていることを前提としています。 手順については[TeamCity をインストールする](http://confluence.jetbrains.com/display/TCD8/Installation)で見つかる、 [TeamCity 8 ドキュメント](http://confluence.jetbrains.com/display/TCD8/TeamCity+Documentation)jetbrains します。

- **ビルド サーバーを準備する**: この手順では、必要なソフトウェア、ツールをインストールしに証明書が必要なモバイル アプリケーションの構築および配布用に準備します。

- **ビルド スクリプトの作成**– この手順は、厳密には必要ありませんが、ビルド スクリプトが無人のアプリケーションの構築に役立ちます。 ビルド スクリプトを使用する可能性があると、継続的インテグレーションを練習していない場合でも、配布するバイナリを作成する一貫性のある反復可能な方法を提供するビルドの問題のトラブルシューティングに役立ちます。

- **TeamCity プロジェクトを作成する**– メタ データがすべて含まれます TeamCity プロジェクトを作成する必要があります前の 3 つの手順を完了すると、ソース コードを取得し、プロジェクトをコンパイルして、Xamarin Test Cloud にテストを送信するために必要です。

## <a name="requirements"></a>必要条件

使用したエクスペリエンス[Xamarin Test Cloud](https://developer.xamarin.com/guides/testcloud)が必要です。

TeamCity 8.1 に関する知識が必要です。 TeamCity のインストールでは、このドキュメントの範囲外です。 TeamCity は OS X Mavericks がインストールされているし、ルート アカウントではなく、通常のユーザー アカウントで実行するいると見なされます。

ビルド サーバーは、OS X、専用の継続的インテグレーションを実行するスタンドアロン コンピューターにあります。 理想的には、ビルド サーバーをデータベース サーバー、web サーバー、または開発者のワークステーションなど、他のロールの責任はできません。

> [!IMPORTANT]
> このガイドでは、Xamarin の「ヘッドレス」インストールは含まれません。

[!include[](~/tools/ci/includes/firewall-information.md)]

## <a name="preparing-the-build-server"></a>ビルド サーバーを準備します。

ビルド サーバーの構成で重要な手順では、すべての必要なツール、ソフトウェア、およびモバイル アプリケーションを構築する証明書をインストールします。 ビルド サーバー モバイル ソリューションをコンパイルしてテストを実行することである必要があります。 構成の問題を最小限に抑えるには、ソフトウェアとツールを TeamCity をホストしている同じユーザー アカウントでインストールしてください。 必要なものの一覧を次には。

1. **Visual Studio for Mac** – Xamarin.iOS および Xamarin.Android が含まれます。
2. **Xamarin コンポーネント ストアへのログイン**– このオプションの手順で、のみ、アプリケーションが、Xamarin コンポーネント ストアからコンポーネントを使用するかどうかに必要な。 事前にこの時点で、コンポーネント ストアにログインすると、TeamCity ビルド アプリケーションをコンパイルしようとしました。 問題ができなくなります。
3. **Xcode** – Xcode が iOS アプリケーションをコンパイルして署名するために必要です。
4. **Xcode コマンド ライン ツール**– これはのインストール」セクションの手順 1. で説明されている、 [Ruby rbenv と更新](https://developer.xamarin.com/guides/testcloud/calabash/updating-ruby-using-rbenv/)ガイド。
5. **署名 Id とプロビジョニング プロファイル**: 証明書をインポートし、XCode でプロファイルをプロビジョニングします。 Apple のガイドを参照してください[をエクスポートする署名 Id とプロビジョニング プロファイル](https://developer.apple.com/library/ios/recipes/xcode_help-accounts_preferences/articles/export_signing_assets.html)の詳細。
6. **Android キーストア**– 必要な Android キーストアを TeamCity ユーザーがつまり、アクセス権を持っているディレクトリにコピー`~/Documents/keystores/MyAndroidApp1`します。
7. **Calabash** – これは、アプリケーションがあるテスト Calabash を使用して作成された場合のオプションの手順。 参照してください、 [Calabash OS X Mavericks 上にインストールする](https://developer.xamarin.com/guides/testcloud/calabash/osx-installation/)ガイドと[Ruby rbenv と更新](https://developer.xamarin.com/guides/testcloud/calabash/updating-ruby-using-rbenv/)詳細ガイド。

次の図は、これらすべてのコンポーネントを示しています。

![](teamcity-images/image1.png "この図ではこれらすべてのコンポーネント")

すべてのソフトウェアがインストールされたら、ユーザー アカウントにログインし、すべてのソフトウェアが正しくインストールされた、動作していることを確認します。 ソリューションをコンパイルして、Test Cloud にアプリケーションを送信する必要があります。 これが大幅に簡略化できます、ビルド スクリプトを実行して、次のセクションで説明されているとします。

## <a name="create-a-build-script"></a>ビルド スクリプトを作成します。

TeamCity のコンパイルとテスト クラウドにモバイル アプリケーションの送信自体のすべての側面を処理することが可能でがビルド スクリプトを作成する強くお勧めします。 ビルド スクリプトには次の利点があります。

1. **ドキュメント**– ビルド スクリプトが、ソフトウェアを構築する方法に関するドキュメントの一種として機能します。 これは、いくつかの「マジック」に関連付けられたアプリケーションのデプロイを機能に集中できるように削除されます。
1. **再現性**– ビルド スクリプトにより、またはユーザーの作業内容に関係なく、まったく同じ方法で動作が、アプリケーションがコンパイルおよび展開するには、毎回です。 この反復可能な整合性では、問題や、正しく実行されたビルドが原因でクリープがエラーや人的ミスを削除します。
1. **バージョン管理**– ビルド スクリプトは、ソース管理システムに含めることができます。 これは、ビルド スクリプトへの変更の追跡、監視、およびエラーまたは誤りが見つかった場合に修正をできることを意味します。
1. **環境を準備する**– ビルド スクリプトが必要なサード パーティの依存関係をインストールするためのロジックを含めることができます。 適切なコンポーネントとアプリケーションをビルドするようになります。

ビルド スクリプトは (Windows) で Powershell ファイルまたは (OS X) 上の bash スクリプトとして簡単にできます。 ビルド スクリプトを作成する場合は、スクリプト言語のいくつかの選択肢があります。

- [**Rake** ](https://github.com/jimweirich/rake) – Ruby に基づいてプロジェクトを構築するためのドメイン固有言語 (DSL) になります。 Rake は、人気の利点とライブラリの豊富なエコシステムがあります。

- [**psake** ](https://github.com/psake/psake) – これは、ソフトウェアを構築するための Windows Powershell ライブラリ

- [**フェイク**](http://fsharp.github.io/FAKE/) – これは、ベースの DSLF#に必要な場合は、既存の .NET ライブラリを利用できるようにします。

どのスクリプト言語が使用されるは、ユーザー設定や要件によって異なります。 [TaskyPro Calabash](https://github.com/xamarin/test-cloud-samples/tree/master/TaskyPro/TaskyPro-Calabash)例にはとして、Rake を使用する例が含まれています、[ビルド スクリプト](https://github.com/xamarin/test-cloud-samples/blob/master/TaskyPro/TaskyPro-Calabash/Rakefile)します。

> [!NOTE]
> 表現力とソフトウェアの構築には専用の DSL の保守容易性、MSBuild または NAnt がこれらの不足などの XML ベース ビルド システムを使用することになります。

### <a name="parameterizing-the-build-script"></a>ビルド スクリプトのパラメーター化

構築およびソフトウェアのテストのプロセスには、秘密に保つ必要があります情報が必要です。 具体的には、APK を作成、キーストアまたはキーストア内のキーのエイリアスのパスワードが必要があります。 同様に、Test Cloud では、開発者に一意の API キーが必要です。 これらの種類の値はなりませんハード ビルド スクリプトのコード化されました。 代わりに変数として、ビルド スクリプトに渡す必要があります。

重要度の低いには、iOS デバイスの ID またはクラウドのテストに使用するデバイスのテストを識別する Android デバイス ID の実行などの値です。 これらは、保護する必要のある値ではありませんが、ビルドからビルドを変更することがあります。

ビルド スクリプトの外部変数のこれらの型を格納することも簡単など、開発者と、組織内のビルド スクリプトを共有します。 開発者は、ビルド サーバーとして同じスクリプトを使用することがありますが、独自のキーストアと API キーを使用することができます。

これらの機密性の高い値を格納するための 2 つの可能なオプションがあります。

- **構成ファイル**– この値は、バージョン管理にチェックインしない必要があります Test Cloud の API キーを保護します。 ファイルは、各マシンを作成できます。 このファイルから値を読み取る方法は、使用するスクリプト言語に依存します。

- **環境変数**– これら基になるスクリプト言語の共有に関係なく、コンピューターごとに簡単に設定することができます。

これらの選択肢のそれぞれに長所と短所があります。 TeamCity は、ため、このガイドは、ビルド スクリプトを作成するときに、この手法は推奨環境変数を適切に動作します。

### <a name="build-steps"></a>ビルド ステップ

ビルド スクリプトは、次の手順を実行できる必要があります。

- **アプリケーションをコンパイル**– これは、正しいプロビジョニング プロファイルで、アプリケーションに署名が含まれています。

- **Xamarin Test Cloud にアプリケーションを提出**– これは、署名と適切なキーストアで APK を整列 zip に含まれます。

これら 2 つの手順については、以下で詳しく説明します。

#### <a name="compiling-a-xamarinios-application"></a>Xamarin.iOS アプリケーションのコンパイル

[!include[](~/tools/ci/includes/commandline-compile-of-xamarin-ios-ipa.md)]

#### <a name="compiling-a-xamarinandroid-application"></a>Xamarin.Android アプリケーションのコンパイル

Android アプリケーションをコンパイルする**xbuild** (または**msbuild** Windows 上)。

```bash
/Library/Frameworks/Mono.framework/Commands/xbuild /t:SignAndroidPackage /p:Configuration=Release /path/to/android.csproj
```

Xamarin Android アプリケーションをコンパイルすることに注意して**xbuild** 、プロジェクトを使用して iOS アプリケーションをビルドする**xbuild**ソリューションが必要です。

#### <a name="submitting-xamarinuitests-to-test-cloud"></a>Test Cloud に Xamarin.UITests を送信します。

UITests を使用して送信、`test-cloud.exe`アプリケーションは、次のスニペットに示すようにします。

```bash
test-cloud.exe <path-to-apk-or-ipa-file> <test-cloud-team-api-key> --devices <device-selection-id> --assembly-dir <path-to-tests-containing-test-assemblies> --nunit-xml report.xml --user <email>
```

テストの実行は、テスト結果がという NUnit スタイル XML ファイルの形式で返されます**report.xml**します。 TeamCity ビルド ログに情報が表示されます。

Test Cloud に UITests を送信する方法の詳細については、このガイドを参照してで[アップロードの準備 Xamarin.UITests](/appcenter/test-cloud/preparing-for-upload/uitest/)します。

#### <a name="submitting-calabash-tests-to-test-cloud"></a>Calabash テスト クラウドに送信します。

Calabash テストを使用して送信されたが、 `test-cloud` gem、次のスニペットに示すようにします。

```bash
test-cloud submit /path/to/APK-or-IPA <test-cloud-team-api-key> --devices <device-id> --user <email>
```
Test Cloud に Android アプリケーションを送信するには、まず calabash android を使用して、APK テスト サーバーを再構築する必要があります。

```bash
$ calabash-android build </path/to/signed/APK>
$ test-cloud submit /path/to/APK <test-cloud-team-api-key> --devices <ANDROID_DEVICE_ID> --profile=android --config=config/cucumber.yml --pretty
```
Calabash テストの送信についての詳細についてを参照してください Xamarin のガイドで[Calabash テストを Test Cloud に送信する](https://developer.xamarin.com/guides/testcloud/calabash/working-with/submitting-tests-to-xamarin-test-cloud/)します。

## <a name="creating-a-teamcity-project"></a>TeamCity プロジェクトを作成します。

TeamCity がインストールされている、Visual Studio for Mac は、プロジェクトをビルドすると、プロジェクトのビルドし、Test Cloud に送信する TeamCity でプロジェクトを作成する時間になります。

1. Web ブラウザー経由での TeamCity にログインして開始します。 ルート プロジェクトに移動します。

    ![ルート プロジェクトに移動](teamcity-images/image2.png "ルート プロジェクトに移動")ルート プロジェクトの下に新しいサブ プロジェクトを作成します。

    ![ルート プロジェクト下に、ルート プロジェクトに移動し、新しいサブ プロジェクトを作成する](teamcity-images/image3.png "への移動、ルート下に、ルート、プロジェクトに新しいサブ プロジェクトの作成")
2. サブ プロジェクトが作成されたら、新しいビルドの構成を追加します。

    ![サブ プロジェクトが作成されたら、新しいビルド構成を追加します。](teamcity-images/image5.png "サブ プロジェクトが作成されたら、新しいビルド構成を追加します。")
3. VC プロジェクトをビルド構成にアタッチします。 これは、バージョン コントロールの設定画面を使用して行います。

    ![バージョン コントロールの設定画面を使用してこれには](teamcity-images/image6.png "バージョン コントロールの設定画面を使用してこれには")

    VC プロジェクトの作成がない場合は、次に示す新しい VCS ルート ページから作成するオプションがあります。

    ![VC プロジェクトの作成がない場合は、新しい VCS ルート ページから作成するオプションがある](teamcity-images/image7.png "VC プロジェクトの作成がない場合は、新しい VCS ルート ページから作成するオプションがあります。")

    VCS ルートがアタッチされると、プロジェクトをチェック アウト TeamCity はしようとして自動ビルド ステップを検出します。 TeamCity に慣れている場合、検出されたビルド手順のいずれかを選択できます。 ここでは、検出されたビルド手順を無視しても安全です。

4. 次に、ビルドのトリガーを構成します。 これはビルドをキューにユーザーがリポジトリにコードをコミットする場合など、特定の条件が満たされたときにします。 次のスクリーン ショットでは、ビルドのトリガーを追加する方法を示します。

    ![このスクリーン ショットは、ビルドのトリガーを追加する方法を示しています。](teamcity-images/image8.png "このスクリーン ショットは、ビルドのトリガーを追加する方法を示しています。")ビルド トリガーを構成する例を次のスクリーン ショットで確認できます。

    ![ビルドのトリガーの構成の例は、このスクリーン ショットで確認できます](teamcity-images/image9.png "ビルド トリガーの構成の例は、このスクリーン ショットで確認できます")

5. ビルド スクリプトのパラメーター化の前のセクションでは、環境変数としていくつかの値を格納することをお勧めします。 これらの変数は、パラメーターの画面を使用して、ビルド構成に追加できます。 テスト クラウド API キー、iOS デバイスの ID、および Android のデバイス ID の下のスクリーン ショットに示すように、変数を追加します。

    ![テスト クラウド API キー、iOS デバイスの ID、および Android のデバイス ID の変数を追加](teamcity-images/image11.png "テスト クラウド API キー、iOS デバイスの ID、および Android のデバイス ID の変数を追加")

6. 最後の手順では、アプリケーションとエンキュー Test Cloud にアプリケーションをコンパイルするビルド スクリプトで起動されるビルド ステップを追加します。 次のスクリーン ショットでは、アプリケーションを構築する、Rakefile を使用するビルド ステップの例を示します。

    ![このスクリーン ショットのようなアプリケーションを構築する、Rakefile を使用するビルド ステップの](teamcity-images/image12.png "このスクリーン ショットは、Rakefile を使用してアプリケーションを構築するビルド手順の例")

7. この時点では、ビルド構成が完了しました。 プロジェクトが正しく構成されていることを確認するビルドをトリガーすることをお勧めします。 これを行うには、意味のない小さな変更をリポジトリにコミットします。 TeamCity は、コミットを検出し、ビルドを開始する必要があります。

8. ビルドが完了すると、ビルド ログを検査し、問題や注意が必要なビルドの警告があるかどうかを参照してください。

## <a name="summary"></a>まとめ

このガイドでは、TeamCity を使用して、Xamarin モバイル アプリケーションを構築し、Test Cloud に送信する方法について説明します。 ビルド プロセスを自動化するビルド スクリプトの作成について説明しました。 ビルド スクリプトに任せ、アプリケーションのコンパイル、テスト クラウドに送信して結果の待機

開発者がコードをコミットするたびにビルドをキューは、ビルド スクリプトを呼び出す TeamCity でプロジェクトを作成する方法について説明しました。

## <a name="related-links"></a>関連リンク

- [Xamarin.UITests などのアップロードを準備します。](/appcenter/test-cloud/preparing-for-upload/uitest/)
- [インストールして、TeamCity の構成](http://confluence.jetbrains.com/display/TCD8/Installing+and+Configuring+the+TeamCity+Server)
