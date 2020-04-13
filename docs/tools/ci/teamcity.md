---
title: チームシティをザマリンで使用する
description: このガイドでは、TeamCity を使用してモバイル アプリケーションをコンパイルし、App Center テストに提出する手順について説明します。
ms.prod: xamarin
ms.assetid: AC2626CB-28A7-4808-B2A9-789D67899546
author: davidortinau
ms.author: daortin
ms.date: 04/01/2020
ms.openlocfilehash: 6ecd453180e8c392ba7d7527778617eb40950a9e
ms.sourcegitcommit: 6f3281a32017cfcebadde8a2d6e10651a277828f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/02/2020
ms.locfileid: "80587461"
---
# <a name="using-team-city-with-xamarin"></a>チームシティをザマリンで使用する

_このガイドでは、TeamCity を使用してモバイル アプリケーションをコンパイルし、App Center テストに提出する手順について説明します。_

[「継続的インテグレーションの概要](~/tools/ci/intro-to-ci.md)」で説明したように、継続的インテグレーション (CI) は、高品質のモバイル アプリケーションを開発する場合に役立ちます。 継続的インテグレーション・サーバー・ソフトウェアには、多くの実行可能なオプションがあります。このガイドでは、ジェットブレイン[ズのチームシティ](https://www.jetbrains.com/teamcity/)に焦点を当てます。

TeamCity のインストールには、いくつかの異なる順列があります。 次の一覧では、これらの順列の一部について説明します。

- **Windows サービス**– このシナリオでは、Windows が Windows サービスとして起動すると、チームシティが起動します。 iOS アプリケーションをコンパイルするには、Mac ビルド ホストとペアリングする必要があります。

- **OS X でデーモンを起動**する - 概念的には、これは前の手順で説明した Windows サービスとして実行するのと似ています。 既定では、ビルドはルート アカウントで実行されます。

- **OS X のユーザー アカウント**– ユーザーがログインするたびに起動するユーザー アカウントで TeamCity を実行できます。

前のシナリオでは、OS X でユーザー アカウントで TeamCity を実行するのが最も簡単で簡単なセットアップです。

TeamCity のセットアップには、次の手順が必要です。

- **TeamCity のインストール**– TeamCity のインストールについては、このガイドでは説明しません。 このガイドは、TeamCity がインストールされ、ユーザー アカウントで実行されていることを前提としています。 [TeamCity](https://confluence.jetbrains.com/display/TCD8/Installation)のインストール手順は、JetBrains の[TeamCity 8 のドキュメントを参照](https://confluence.jetbrains.com/display/TCD8/TeamCity+Documentation)してください。

- **ビルド サーバーの準備**– この手順では、モバイル アプリケーションを構築し、配布用に準備するために必要なソフトウェア、ツール、および証明書をインストールします。

- **ビルド スクリプトの作成**– この手順は厳密には必要ありませんが、ビルド スクリプトはアプリケーションを無人で構築する際に役立ちます。 ビルド スクリプトを使用すると、継続的インテグレーションが実施されていない場合でも、ビルドの問題のトラブルシューティングに役立ち、配布用のバイナリを作成するための一貫した繰り返し可能な方法が提供されます。

- **TeamCity プロジェクトの作成**– 前の 3 つの手順が完了したら、ソース コードの取得、プロジェクトのコンパイル、およびテストの App Center テストへの提出に必要なすべてのメタデータを含む TeamCity プロジェクトを作成する必要があります。

## <a name="requirements"></a>必要条件

アプリ[センター テスト](https://docs.microsoft.com/appcenter/test-cloud/)の経験が必要です。

チームシティ8.1に精通していることが必要です。 TeamCity のインストールは、このドキュメントの範囲を超えています。 TeamCity は OS X Mavericks にインストールされており、ルートアカウントではなく通常のユーザーアカウントで実行されていると想定しています。

ビルド サーバーは、継続的インテグレーション専用の OS X を実行するスタンドアロン のコンピューターである必要があります。 理想的には、データベース サーバー、Web サーバー、開発者ワークステーションなどの他のロールについては、ビルド サーバーが責任を負わないのが理想的です。

> [!IMPORTANT]
> このガイドでは、Xamarin の "ヘッドレス" インストールは取り上げません。

[!include[](~/tools/ci/includes/firewall-information.md)]

## <a name="preparing-the-build-server"></a>ビルド サーバーの準備

ビルド サーバーを構成する際の重要な手順は、モバイル アプリケーションを構築するために必要なすべてのツール、ソフトウェア、および証明書をインストールすることです。 ビルド サーバーがモバイル ソリューションをコンパイルし、テストを実行できることが重要です。 構成の問題を最小限に抑えるには、TeamCity をホストしているのと同じユーザー アカウントにソフトウェアとツールをインストールする必要があります。 次の一覧で、必要な事項を詳しく説明します。

1. **Mac 用のビジュアル スタジオ**– これには、Xamarin.iOS と Xamarin.Android が含まれています。
2. **Xamarin コンポーネント ストアにログイン –** この手順はオプションで、アプリケーションが Xamarin コンポーネント ストアのコンポーネントを使用する場合にのみ必要です。 この時点でコンポーネント ストアに事前にログインすると、TeamCity ビルドがアプリケーションのコンパイルを試みたときに問題が発生するのを防ぐことができます。
3. **Xcode** – iOS アプリケーションをコンパイルして署名するには、Xcode が必要です。
4. **Xcode コマンドラインツール**– これは、「ruby を[rbenv で更新](https://github.com/calabash/calabash-ios/wiki)する」ガイドの「インストール」セクションのステップ 1 で説明されています。
5. **ID &プロビジョニングプロファイルの署名**- XCode を使用して証明書とプロビジョニング プロファイルをインポートします。 詳細については、[署名 ID とプロビジョニング プロファイルのエクスポート](https://developer.apple.com/library/ios/recipes/xcode_help-accounts_preferences/articles/export_signing_assets.html)に関する Apple のガイドを参照してください。
6. **Android キーストア**– 必要な Android キーストアを、TeamCity ユーザーがアクセスできるディレクトリに`~/Documents/keystores/MyAndroidApp1`コピーします。
7. **Calabash** – アプリケーションに Calabash を使用して書かれたテストがある場合、これはオプションのステップです。 詳細については[、『OS X マーベリックスへのカラバッシュのインストール](https://github.com/calabash/calabash-ios/wiki)』ガイドおよび[『rbenv を使用した Ruby の更新](https://github.com/calabash/calabash-ios/wiki)』ガイドを参照してください。

次の図は、これらのコンポーネントをすべて示しています。

![](teamcity-images/image1.png "This diagram illustrates all of these components")

すべてのソフトウェアがインストールされたら、ユーザーアカウントにログインし、すべてのソフトウェアが正しくインストールされ、動作していることを確認します。 これには、ソリューションをコンパイルし、App Center Test にアプリケーションを提出する必要があります。 これは、次のセクションで説明するビルド スクリプトを実行することで簡略化できます。

## <a name="create-a-build-script"></a>ビルド スクリプトの作成

TeamCity は、モバイル アプリケーションのコンパイルとアプリ センター テストへの提出のあらゆる側面を単独で処理することは可能ですが、それ自体では、アプリケーション センター テストに対して実行できます。ビルド スクリプトを作成することをお勧めします。 ビルド スクリプトには、次の利点があります。

1. **ドキュメント**– ビルド スクリプトは、ソフトウェアのビルド方法に関するドキュメントの形式として機能します。 これにより、アプリケーションのデプロイに関連する "マジック" の一部が削除され、開発者は機能に集中できます。
1. **反復性**– ビルド スクリプトは、アプリケーションがコンパイルおよび配置されるたびに、誰が何を行うかに関係なく、同じ方法で実行されるようにします。 この繰り返し可能な一貫性により、ビルドまたはヒューマン エラーが正しく実行されなかったために発生する可能性のある問題やエラーが削除されます。
1. **バージョン管理**– ビルド スクリプトをソース管理システムに含めることができます。 つまり、ビルド スクリプトへの変更は、エラーや不正確さが見つかった場合に追跡、監視、および修正できます。
1. **環境の準備**– ビルド スクリプトには、必要なサード パーティの依存関係をインストールするロジックを含めることができます。 これにより、アプリケーションが適切なコンポーネントでビルドされます。

ビルド スクリプトは、PowerShell ファイル (Windows 上) や bash スクリプト (OS X) と同じくらい単純にできます。 ビルド スクリプトを作成する場合、スクリプト言語にはいくつかの選択肢があります。

- [**Rake**](https://github.com/jimweirich/rake) – これは Ruby に基づいてプロジェクトを構築するためのドメイン固有言語 (DSL) です。 Rakeは人気の利点と図書館の豊かな生態系を持っています。

- [**psake**](https://github.com/psake/psake) – これは、ソフトウェアを構築するための Windows PowerShell ライブラリです。

- [**FAKE**](https://fsharp.github.io/FAKE/) – これは、必要に応じて既存の .NET ライブラリを使用することを可能にする F# ベースの DSL です。

どのスクリプト言語を使用するかは、ユーザーの好みや要件によって異なります。

> [!NOTE]
> MSBuild や NAnt などの XML ベースのビルド システムを使用することは可能ですが、これらはソフトウェアの構築専用の DSL の表現力と保守性に欠けます。

### <a name="parameterizing-the-build-script"></a>ビルド スクリプトのパラメーター化

ソフトウェアの構築とテストのプロセスには、秘密にする必要のある情報が必要です。 APK を作成するには、キーストアやキーストア内のキーエイリアスのパスワードが必要な場合があります。 同様に、App Center テストには、開発者に固有の[API キー](/appcenter/api-docs/)が必要です。 これらの種類の値は、ビルド スクリプトでハードコーディングしないでください。 代わりに、ビルド スクリプトに変数として渡す必要があります。

機密性の低いのは、iOS デバイス ID や、App Center がテストの実行に使用するデバイスを識別する Android デバイス ID などの値です。 これらは保護する必要がある値ではありませんが、ビルドによって変更される可能性があります。

これらの種類の変数をビルド スクリプトの外部に格納すると、ビルド スクリプトを組織内で、開発者などと簡単に共有できるようになります。 開発者はビルド サーバーとまったく同じスクリプトを使用できますが、独自のキーストアや[API キー](/appcenter/api-docs/)を使用できます。

これらの重要な値を格納するオプションは 2 つあります。

- **構成ファイル**– API[キー](/appcenter/api-docs/)を保護するために、この値をバージョン管理にチェックインしないでください。 ファイルは、マシンごとに作成できます。 このファイルから値を読み取る方法は、使用するスクリプト言語によって異なります。

- **環境変数**– これらは、コンピューターごとに簡単に設定でき、基礎となるスクリプト言語とは無関係です。

これらの選択肢には、それぞれ長所と短所があります。 TeamCity は環境変数とうまく機能するため、このガイドでは、ビルド スクリプトを作成するときにこの手法を推奨します。

### <a name="build-steps"></a>ビルド ステップ

ビルド スクリプトは、次の手順を実行する必要があります。

- **アプリケーションのコンパイル**– これには、適切なプロビジョニング プロファイルを使用してアプリケーションに署名が含まれます。

- **Xamarin テスト クラウドにアプリケーションを提出**する – これには、APK を適切なキーストアに合わせて署名および zip で調整する機能が含まれます。

これら2つのステップについては、以下で詳しく説明します。

#### <a name="compiling-a-xamarinios-application"></a>Xamarin.iOS アプリケーションのコンパイル

[!include[](~/tools/ci/includes/commandline-compile-of-xamarin-ios-ipa.md)]

#### <a name="compiling-a-xamarinandroid-application"></a>Xamarin.Android アプリケーションのコンパイル

Android アプリケーションをコンパイルするには **、xbuild** (または Windows の**msbuild)** を使用します。

```bash
/Library/Frameworks/Mono.framework/Commands/xbuild /t:SignAndroidPackage /p:Configuration=Release /path/to/android.csproj
```
Android アプリケーションの**xbuild**をコンパイルするとプロジェクトが使用され、iOS アプリケーション**の xbuild**はソリューションを使用します。

#### <a name="submitting-xamarinuitests-to-app-center"></a>Xamarin.UI テストをアプリ センターに提出する

UI テストは、次のスニペットに示すように[、App Center CLI](https://github.com/microsoft/appcenter-cli)を使用して送信されます。

```bash
appcenter test run uitest --app <TEAM-NAME/APP-NAME> --devices <DEVICE_SET> --token <API_KEY> --app-path <appname.APK-or-appname.IPA> --merge-nunit-xml report.xml --build-dir pathToUITestBuildDir
```

テストを実行すると、テスト結果は**report.xml**という NUnit スタイルの XML ファイルの形式で返されます。 TeamCity はビルド ログに情報を表示します。

アプリ センターに UI テストを提出する方法の詳細については[、「Xamarin.Android アプリの準備](/appcenter/test-cloud/uitest/preparing-for-upload-android)」または[「Xamarin.iOS アプリの準備](/appcenter/test-cloud/uitest/preparing-for-upload-ios)」を参照してください。

#### <a name="submitting-calabash-tests-to-app-center"></a>アプリ センターへのカラバッシュ テストの提出

Calabash テストは、次のスニペットに示すように[、App Center CLI](https://github.com/microsoft/appcenter-cli)を使用して送信されます。

```bash
appcenter test run calabash --app <TEAM-NAME/APP-NAME> --devices <DEVICE_SET> --token <API_KEY> --app-path <appname.APK-or-appname.IPA> --project-dir pathToProjectDir
```

アプリ センター テストに Android アプリケーションを提出するには、最初にキャラバシュアンドロイドを使用して APK テスト サーバーを再構築する必要があります。

```bash
$ calabash-android build </path/to/signed/APK>
$ appcenter test run calabash --app <TEAM-NAME/APP-NAME> --devices <DEVICE_SET> --token <API_KEY> --app-path <appname.APK> --project-dir pathToProjectDir
```

Calabash テストの提出の詳細については、テスト クラウドへの[Calabash テストの提出に関する Xamarin](https://github.com/calabash/calabash-ios/wiki)のガイドを参照してください。

## <a name="creating-a-teamcity-project"></a>チームシティ プロジェクトの作成

TeamCity がインストールされ、Visual Studio for Mac がプロジェクトをビルドできたら、TeamCity でプロジェクトを作成してプロジェクトをビルドし、App Center に提出します。

1. Webブラウザを介してチームシティにログインすることによって開始しました。 ルート プロジェクトに移動します。

    ![ルート プロジェクトに移動する](teamcity-images/image2.png "ルート プロジェクトに移動する")ルート プロジェクトの下に新しいサブプロジェクトを作成します。

    ![ルート プロジェクトの下にあるルート プロジェクトに移動し、新しいサブプロジェクトを作成します。](teamcity-images/image3.png "ルート プロジェクトの下にあるルート プロジェクトに移動し、新しいサブプロジェクトを作成します。")
2. サブプロジェクトが作成されたら、新しいビルド構成を追加します。

    ![サブプロジェクトが作成されたら、新しいビルド構成を追加します。](teamcity-images/image5.png "サブプロジェクトが作成されたら、新しいビルド構成を追加します。")
3. VCS プロジェクトをビルド構成にアタッチします。 これは、バージョン管理設定画面で行われます。

    ![これはバージョン管理設定画面で行われます](teamcity-images/image6.png "これはバージョン管理設定画面で行われます")

    VCS プロジェクトが作成されていない場合は、次に示す [新しい VCS ルート] ページから作成できます。

    ![VCS プロジェクトが作成されていない場合は、[新しい VCS ルート] ページから作成できます。](teamcity-images/image7.png "VCS プロジェクトが作成されていない場合は、[新しい VCS ルート] ページから作成するオプションがあります。")

    VCS ルートがアタッチされると、TeamCity はプロジェクトをチェックアウトし、ビルド ステップの自動検出を試みます。 TeamCity に精通している場合は、検出されたビルド ステップのいずれかを選択できます。 今のところ、検出されたビルドステップを無視しても安全です。

4. 次に、ビルド トリガーを構成します。 これは、ユーザーがリポジトリにコードをコミットするときなど、特定の条件が満たされたときにビルドをキューに入れます。 次のスクリーンショットは、ビルド トリガーを追加する方法を示しています。

    ![このスクリーンショットは、ビルドトリガーを追加する方法を示しています](teamcity-images/image8.png "このスクリーンショットは、ビルドトリガーを追加する方法を示しています")ビルド トリガーの構成例を次のスクリーン ショットで確認できます。

    ![ビルド トリガーの構成例は、このスクリーン ショットで確認できます。](teamcity-images/image9.png "ビルド トリガーの構成例は、このスクリーン ショットで確認できます。")

5. 前のセクション「ビルド スクリプトのパラメーター化」では、いくつかの値を環境変数として格納することを提案しました。 これらの変数は、パラメーター画面からビルド構成に追加できます。 次のスクリーンショットに示すように、App Center [API キー](/appcenter/api-docs/)、iOS デバイス ID、および Android デバイス ID の変数を追加します。

    ![アプリ センター テスト API キー、iOS デバイス ID、および Android デバイス ID の変数を追加します。](teamcity-images/image11.png "テスト クラウド API キー、iOS デバイス ID、および Android デバイス ID の変数を追加します。")

6. 最後の手順では、ビルド スクリプトを呼び出してアプリケーションをコンパイルし、アプリケーションを App Center Test にキューに入れるビルド ステップを追加します。 次のスクリーンショットは、Rakefile を使用してアプリケーションをビルドするビルドステップの例です。

    ![このスクリーンショットは、Rakefile を使用してアプリケーションをビルドするビルドステップの例です。](teamcity-images/image12.png "このスクリーンショットは、Rakefile を使用してアプリケーションをビルドするビルドステップの例です。")

7. この時点で、ビルド構成は完了です。 ビルドをトリガーして、プロジェクトが正しく構成されていることを確認することをお勧めします。 これを行う良い方法は、小さな、重要な変更をリポジトリにコミットすることです。 TeamCity はコミットを検出し、ビルドを開始する必要があります。

8. ビルドが完了したら、ビルド ログを調べて、注意が必要なビルドに問題や警告があるかどうかを確認します。

## <a name="summary"></a>まとめ

このガイドでは、TeamCity を使用して Xamarin モバイル アプリケーションをビルドし、App Center テストに提出する方法について説明しました。 ビルド プロセスを自動化するビルド スクリプトの作成について説明しました。 ビルド スクリプトは、アプリケーションのコンパイル、App Center テストへの送信、および結果の待機を処理します。

次に、開発者がコードをコミットするたびにビルドをキューに入れてビルド スクリプトを呼び出すプロジェクトを TeamCity で作成する方法について説明しました。

## <a name="related-links"></a>関連リンク

- [Xamarin.Android アプリの準備](/appcenter/test-cloud/uitest/preparing-for-upload-android)
- [Xamarin.iOS アプリの準備](/appcenter/test-cloud/uitest/preparing-for-upload-ios)
- [チームシティのインストールと構成](https://confluence.jetbrains.com/display/TCD8/Installing+and+Configuring+the+TeamCity+Server)
