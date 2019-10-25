---
title: Xamarin でのチーム City の使用
description: このガイドでは、TeamCity を使用してモバイルアプリケーションをコンパイルし、Xamarin Test Cloud に送信するために必要な手順について説明します。
ms.prod: xamarin
ms.assetid: AC2626CB-28A7-4808-B2A9-789D67899546
author: conceptdev
ms.author: crdun
ms.date: 03/23/2017
ms.openlocfilehash: ee1ef1ecda18ee9817fcf10b7dda0c7b4489bf9f
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "72273131"
---
# <a name="using-team-city-with-xamarin"></a>Xamarin でのチーム City の使用

_このガイドでは、TeamCity を使用してモバイルアプリケーションをコンパイルし、Xamarin Test Cloud に送信するために必要な手順について説明します。_

「[継続的インテグレーションの概要](~/tools/ci/intro-to-ci.md)」で説明したように、継続的インテグレーション (CI) は、品質の高いモバイルアプリケーションを開発するときに便利な方法です。 継続的インテグレーションサーバーソフトウェアには、さまざまなオプションが用意されています。このガイドでは、JetBrains からの[Teamcity](http://www.jetbrains.com/teamcity/)に注目します。

TeamCity のインストールには、いくつかの異なる順列があります。 これらのいくつかの一覧を次に示します。

- **Windows サービス**–このシナリオでは、Windows が windows サービスとして起動すると teamcity が開始されます。 IOS アプリケーションをコンパイルするには、Mac ビルドホストとペアリングする必要があります。

- **OS X でデーモンを起動**する–概念的には、これは前の手順で説明した Windows サービスとして実行するのとよく似ています。 既定では、ビルドはルートアカウントで実行されます。

- **OS X のユーザーアカウント**–ユーザーがログインするたびに起動するユーザーアカウントで teamcity を実行できます。

前のシナリオでは、OS X のユーザーアカウントで TeamCity を実行するのが最も簡単で、簡単にセットアップできます。

TeamCity の設定には、次のようないくつかの手順が含まれます。

- **Teamcity のインストール**– teamcity のインストールについては、このガイドでは説明しません。 このガイドでは、TeamCity がインストールされ、ユーザーアカウントで実行されていることを前提としています。 [TeamCity のインストール](http://confluence.jetbrains.com/display/TCD8/Installation)手順については、JetBrains による[teamcity 8 のドキュメント](http://confluence.jetbrains.com/display/TCD8/TeamCity+Documentation)を参照してください。

- **ビルドサーバーの準備**: この手順では、モバイルアプリケーションを構築し、配布の準備を行うために必要なソフトウェア、ツール、および証明書をインストールします。

- **ビルドスクリプトの作成**-この手順は、厳密には必要ありませんが、ビルドスクリプトは、アプリケーションを無人でビルドするのに役立ちます。 ビルドスクリプトを使用すると、発生する可能性のあるビルドの問題のトラブルシューティングに役立ち、継続的インテグレーションが実行されていない場合でも、一貫性のある反復可能な方法で配布用のバイナリを作成できます。

- **TeamCity プロジェクトの作成**–前の3つの手順が完了したら、ソースコードを取得し、プロジェクトをコンパイルして、テストを Xamarin Test Cloud に送信するために必要なすべてのメタデータを含む teamcity プロジェクトを作成する必要があります。

## <a name="requirements"></a>［要件］

[App Center テスト](https://docs.microsoft.com/appcenter/test-cloud/)の経験が必要です。

TeamCity 8.1 に関する知識が必要です。 TeamCity のインストールについては、このドキュメントでは説明しません。 TeamCity は OS X Mavericks にインストールされており、ルートアカウントではなく通常のユーザーアカウントで実行されていることを前提としています。

ビルドサーバーは、継続的インテグレーション専用の、OS X を実行するスタンドアロンコンピューターである必要があります。 理想的には、ビルドサーバーは、データベースサーバー、web サーバー、開発者ワークステーションなどの他のロールに対して責任を負いません。

> [!IMPORTANT]
> このガイドでは、Xamarin の "ヘッドレス" インストールについては説明しません。

[!include[](~/tools/ci/includes/firewall-information.md)]

## <a name="preparing-the-build-server"></a>ビルドサーバーの準備

ビルドサーバーを構成するための重要な手順は、モバイルアプリケーションをビルドするために必要なすべてのツール、ソフトウェア、および証明書をインストールすることです。 ビルドサーバーでモバイルソリューションをコンパイルし、テストを実行できることが重要です。 構成の問題を最小限に抑えるには、TeamCity をホストしているのと同じユーザーアカウントにソフトウェアとツールをインストールする必要があります。 必要なものの一覧を次に示します。

1. **Visual Studio for Mac** –これには、Xamarin と xamarin Android が含まれます。
2. **Xamarin コンポーネントストアにログインする**–これはオプションの手順であり、アプリケーションが Xamarin コンポーネントストアのコンポーネントを使用する場合にのみ必要です。 この時点でコンポーネントストアに事前にログインすると、TeamCity ビルドがアプリケーションのコンパイルを試行しても問題が発生しなくなります。
3. **Xcode** – Xcode は、iOS アプリケーションをコンパイルして署名するために必要です。
4. **Xcode コマンドラインツール**–「 [Ruby With rbenv の更新](https://github.com/calabash/calabash-ios/wiki)」ガイドの「インストール」セクションの手順 1. で説明されています。
5. **& プロビジョニングプロファイルの署名 id** : XCode を使用して証明書とプロビジョニングプロファイルをインポートします。 詳細については[、「署名 id とプロビジョニングプロファイルのエクスポート](https://developer.apple.com/library/ios/recipes/xcode_help-accounts_preferences/articles/export_signing_assets.html)に関する Apple のガイド」を参照してください。
6. **Android キーストア**–必要な android キーストアを teamcity ユーザーがアクセスできるディレクトリにコピーします。つまり、 `~/Documents/keystores/MyAndroidApp1`。
7. **Calabash** –アプリケーションに calabash を使用して記述されたテストがある場合は、これは省略可能な手順です。 詳細については、「 [OS X Mavericks に Calabash をインストールする](https://github.com/calabash/calabash-ios/wiki)」および「 [Ruby With rbenv を更新](https://github.com/calabash/calabash-ios/wiki)する」ガイドを参照してください。

次の図は、これらすべてのコンポーネントを示しています。

![](teamcity-images/image1.png "This diagram illustrates all of these components")

すべてのソフトウェアがインストールされたら、ユーザーアカウントにログインし、すべてのソフトウェアが正しくインストールされ、動作していることを確認します。 これには、ソリューションをコンパイルし、Test Cloud にアプリケーションを送信する必要があります。 これは、次のセクションで説明するように、ビルドスクリプトを実行することで大幅に簡素化できます。

## <a name="create-a-build-script"></a>ビルドスクリプトの作成

TeamCity では、コンパイルとモバイルアプリケーションの送信のすべての側面を処理して Test Cloud することができますが、ビルドスクリプトを作成することを強くお勧めします。 ビルドスクリプトには、次のような利点があります。

1. **ドキュメント**–ビルドスクリプトは、ソフトウェアの構築方法に関するドキュメントの形式として機能します。 これにより、アプリケーションの配置に関連する "マジック" の一部が削除され、開発者は機能に専念できます。
1. **再現性**: ビルドスクリプトを使用すると、アプリケーションをコンパイルして配置するたびに、どのユーザーがどのように動作するかに関係なく、まったく同じ方法で動作します。 この反復可能な一貫性によって、ビルドまたは人的エラーが不適切に実行されたことが原因で、でクリープする可能性のある問題またはエラーが除去されます。
1. **バージョン管理**-ビルドスクリプトをソース管理システムに含めることができます。 つまり、ビルドスクリプトへの変更を追跡、監視、修正して、エラーまたは誤りが見つかった場合には修正できます。
1. **環境を準備**する–ビルドスクリプトには、必要なサードパーティの依存関係をインストールするロジックを含めることができます。 これにより、アプリケーションが適切なコンポーネントでビルドされるようになります。

ビルドスクリプトは、Powershell ファイル (Windows の場合) または bash スクリプト (OS X の場合) のように簡単にできます。 ビルドスクリプトを作成する場合、スクリプト言語にはいくつかの選択肢があります。

- [**Rake**](https://github.com/jimweirich/rake) : Ruby に基づいてプロジェクトをビルドするためのドメイン固有言語 (DSL) です。 Rake には、人気と豊富なライブラリのエコシステムの利点があります。

- [**psake**](https://github.com/psake/psake) - ソフトウェアを構築するための Windows Powershell ライブラリです

- [**フェイク**](http://fsharp.github.io/FAKE/)–これは DSL ベースです。 F#これにより、必要に応じて既存の .net ライブラリを利用できるようになります。

どのスクリプト言語が使用されるかは、ユーザーの好みや要件によって異なります。 [Taskypro-Calabash](https://github.com/xamarin/test-cloud-samples/tree/master/TaskyPro/TaskyPro-Calabash)の例には、Rake を[ビルドスクリプト](https://github.com/xamarin/test-cloud-samples/blob/master/TaskyPro/TaskyPro-Calabash/Rakefile)として使用する例が含まれています。

> [!NOTE]
> MSBuild や NAnt などの XML ベースのビルドシステムを使用することもできますが、本ソフトウェアの構築専用の DSL の表現力や保守性は欠けています。

### <a name="parameterizing-the-build-script"></a>ビルドスクリプトのパラメーター化

ソフトウェアのビルドとテストのプロセスには、機密情報を保持する必要がある情報が必要です。 特に、APK の作成には、キーストアのパスワード、またはキーストアのキーエイリアスが必要になる場合があります。 同様に、Test Cloud には、開発者に固有の API キーが必要です。 これらの種類の値は、ビルドスクリプトでハードコーディングしないでください。 代わりに、変数としてビルドスクリプトに渡す必要があります。

機密性が低いは、iOS デバイス ID や、テストの実行に使用 Test Cloud デバイスを識別する Android デバイス ID などの値です。 これらは、保護する必要がある値ではなく、ビルドごとに変更される可能性があります。

これらの種類の変数をビルドスクリプトの外部に格納すると、開発者は、たとえば、組織内でビルドスクリプトを共有しやすくなります。 開発者は、ビルドサーバーとまったく同じスクリプトを使用できますが、独自のキーストアと API キーを使用できます。

これらの機密値を格納するには、次の2つのオプションを使用できます。

- **構成ファイル**– Test Cloud API キーを保護するには、この値をバージョン管理にチェックインしないようにする必要があります。 ファイルは、コンピューターごとに作成できます。 このファイルから値を読み取る方法は、使用するスクリプト言語によって異なります。

- **環境変数**–コンピューター単位で簡単に設定でき、基になるスクリプト言語に依存しません。

これらの選択肢には長所と短所があります。 TeamCity は環境変数で適切に動作するため、このガイドではビルドスクリプトを作成するときにこの手法をお勧めします。

### <a name="build-steps"></a>ビルドステップ

ビルドスクリプトは、次の手順を実行できる必要があります。

- **アプリケーションをコンパイルし**ます。これには、適切なプロビジョニングプロファイルを使用したアプリケーションへの署名も含まれます。

- **アプリケーションを Xamarin Test Cloud に送信**します。これには、適切なキーストアを使用して apk を署名し、zip に揃えることが含まれます。

これらの2つの手順については、以下で詳しく説明します。

#### <a name="compiling-a-xamarinios-application"></a>Xamarin. iOS アプリケーションのコンパイル

[!include[](~/tools/ci/includes/commandline-compile-of-xamarin-ios-ipa.md)]

#### <a name="compiling-a-xamarinandroid-application"></a>Xamarin Android アプリケーションのコンパイル

Android アプリケーションをコンパイルするには、 **xbuild** (または Windows 上の**msbuild** ) を使用します。

```bash
/Library/Frameworks/Mono.framework/Commands/xbuild /t:SignAndroidPackage /p:Configuration=Release /path/to/android.csproj
```

Xamarin Android アプリケーションをコンパイルすると、 **xbuild**によってプロジェクトが使用され、iOS アプリケーションをビルドするにはソリューション**が必要になります**。

#### <a name="submitting-xamarinuitests-to-test-cloud"></a>UITests を Test Cloud に送信しています

UITests は、次のスニペットに示すように、`test-cloud.exe` アプリケーションを使用して送信されます。

```bash
test-cloud.exe <path-to-apk-or-ipa-file> <test-cloud-team-api-key> --devices <device-selection-id> --assembly-dir <path-to-tests-containing-test-assemblies> --nunit-xml report.xml --user <email>
```

テストが実行されると、テスト結果は、**レポート xml**という NUnit 形式の xml ファイルの形式で返されます。 TeamCity によって、ビルドログに情報が表示されます。

UITests を Test Cloud に送信する方法の詳細については、「 [Xamarin Android アプリの準備](/appcenter/test-cloud/preparing-for-upload/xamarin-android-uitest)」または「 [Xamarin の IOS アプリの準備](/appcenter/test-cloud/preparing-for-upload/xamarin-ios-uitest)」を参照してください。

#### <a name="submitting-calabash-tests-to-test-cloud"></a>Test Cloud に対する Calabash テストの送信

Calabash テストは、次のスニペットに示すように `test-cloud` gem を使用して送信されます。

```bash
test-cloud submit /path/to/APK-or-IPA <test-cloud-team-api-key> --devices <device-id> --user <email>
```

Android アプリケーションを Test Cloud に送信するには、最初に calabash-android を使用して APK テストサーバーを再構築する必要があります。

```bash
$ calabash-android build </path/to/signed/APK>
$ test-cloud submit /path/to/APK <test-cloud-team-api-key> --devices <ANDROID_DEVICE_ID> --profile=android --config=config/cucumber.yml --pretty
```

Calabash テストの送信の詳細については、 [Test Cloud への Calabash テストの送信](https://github.com/calabash/calabash-ios/wiki)に関する Xamarin のガイドを参照してください。

## <a name="creating-a-teamcity-project"></a>TeamCity プロジェクトの作成

TeamCity がインストールされ Visual Studio for Mac、プロジェクトをビルドできるようになったら、TeamCity でプロジェクトを作成してプロジェクトをビルドし、Test Cloud に送信します。

1. Web ブラウザーを使用して TeamCity にログインすることで開始されます。 ルートプロジェクトに移動します。

    ![ルートプロジェクトに移動します](teamcity-images/image2.png "ルートプロジェクトに移動します。")。ルートプロジェクトの下に、新しいサブプロジェクトを作成します。

    ![ルートプロジェクトの下にあるルートプロジェクトに移動し、新しいサブプロジェクトを作成します。](teamcity-images/image3.png "ルートプロジェクトの下にあるルートプロジェクトに移動し、新しいサブプロジェクトを作成します。")
2. サブプロジェクトが作成されたら、新しいビルド構成を追加します。

    ![サブプロジェクトが作成されたら、新しいビルド構成を追加します。](teamcity-images/image5.png "サブプロジェクトが作成されたら、新しいビルド構成を追加します。")
3. ビルド構成に VCS プロジェクトをアタッチします。 これを行うには、[バージョンコントロールの設定] 画面を使用します。

    ![これは、[バージョンコントロールの設定] 画面で行います。](teamcity-images/image6.png "これは、[バージョンコントロールの設定] 画面で行います。")

    VCS プロジェクトが作成されていない場合は、次に示す新しい VCS ルートページから作成することもできます。

    ![VCS プロジェクトが作成されていない場合は、新しい VCS ルートページから作成することもできます。](teamcity-images/image7.png "VCS プロジェクトが作成されていない場合は、新しい VCS ルートページから作成することもできます。")

    VCS のルートがアタッチされると、TeamCity によってプロジェクトがチェックアウトされ、ビルドのステップが自動的に検出されます。 TeamCity を使い慣れている場合は、検出されたビルドステップのいずれかを選択できます。 現時点では、検出されたビルドの手順を無視しても安全です。

4. 次に、ビルドトリガーを構成します。 これにより、ユーザーがリポジトリにコードをコミットしたときなど、特定の条件が満たされたときにビルドがキューに入れられます。 次のスクリーンショットは、ビルドトリガーを追加する方法を示しています。

    ![このスクリーンショットは、ビルドトリガーを追加する方法を示し](teamcity-images/image8.png "このスクリーンショットは、ビルドトリガーを追加する方法を示しています。")ています。ビルドトリガーを構成する例を次のスクリーンショットに示します。

    ![ビルドトリガーを構成する例については、このスクリーンショットをご覧ください。](teamcity-images/image9.png "ビルドトリガーを構成する例については、このスクリーンショットをご覧ください。")

5. 前のセクション「ビルドスクリプトのパラメーター化」では、環境変数としていくつかの値を格納することを推奨していました。 これらの変数は、[パラメーター] 画面を使用してビルド構成に追加できます。 次のスクリーンショットに示すように、Test Cloud API キー、iOS デバイス ID、および Android デバイス ID の変数を追加します。

    ![Test Cloud API キー、iOS デバイス ID、および Android デバイス ID の変数を追加します。](teamcity-images/image11.png "Test Cloud API キー、iOS デバイス ID、および Android デバイス ID の変数を追加します。")

6. 最後の手順では、ビルドスクリプトを呼び出してアプリケーションをコンパイルし、アプリケーションを Test Cloud にエンキューするビルドステップを追加します。 次のスクリーンショットは、Rakefile を使用してアプリケーションをビルドするビルドステップの例です。

    ![このスクリーンショットは、Rakefile を使用してアプリケーションをビルドするビルドステップの例です。](teamcity-images/image12.png "このスクリーンショットは、Rakefile を使用してアプリケーションをビルドするビルドステップの例です。")

7. この時点で、ビルド構成が完了します。 ビルドをトリガーして、プロジェクトが適切に構成されていることを確認することをお勧めします。 これを行うには、小規模で重要ではない変更をリポジトリにコミットすることをお勧めします。 TeamCity はコミットを検出し、ビルドを開始する必要があります。

8. ビルドが完了したら、ビルドログを調べ、注意が必要なビルドに問題または警告があるかどうかを確認します。

## <a name="summary"></a>まとめ

このガイドでは、TeamCity を使用して Xamarin モバイルアプリケーションをビルドし、Test Cloud に送信する方法について説明します。 ビルドプロセスを自動化するためのビルドスクリプトの作成について説明しました。 ビルドスクリプトは、アプリケーションのコンパイル、Test Cloud への送信、および結果の待機を行います。

次に、開発者がコードをコミットしてビルドスクリプトを呼び出すたびにビルドをキューにする、TeamCity でプロジェクトを作成する方法について説明します。

## <a name="related-links"></a>関連リンク

- [Xamarin Android アプリを準備する](/appcenter/test-cloud/preparing-for-upload/xamarin-android-uitest)
- [Xamarin iOS アプリを準備しています](/appcenter/test-cloud/preparing-for-upload/xamarin-ios-uitest)
- [TeamCity のインストールと構成](http://confluence.jetbrains.com/display/TCD8/Installing+and+Configuring+the+TeamCity+Server)
