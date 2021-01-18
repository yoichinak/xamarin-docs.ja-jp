---
title: Xamarin での TeamCity の使用
description: このガイドでは、TeamCity を使用してモバイルアプリケーションをコンパイルし、App Center テストに送信する手順について説明します。
ms.prod: xamarin
ms.assetid: AC2626CB-28A7-4808-B2A9-789D67899546
author: davidortinau
ms.author: daortin
ms.date: 04/01/2020
ms.openlocfilehash: 553f40a407fa8003a6545214a545f00b01fb0ed6
ms.sourcegitcommit: b75c369adb8e02a429b6c0fed8ba4a855099bf01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/18/2021
ms.locfileid: "98555036"
---
# <a name="using-teamcity-with-xamarin"></a>Xamarin での TeamCity の使用

_このガイドでは、TeamCity を使用してモバイルアプリケーションをコンパイルし、App Center テストに送信する手順について説明します。_

「 [継続的インテグレーションの概要](~/tools/ci/intro-to-ci.md) 」で説明したように、継続的インテグレーション (CI) は、品質の高いモバイルアプリケーションを開発するときに便利な方法です。 継続的インテグレーションサーバーソフトウェアには、さまざまなオプションが用意されています。このガイドでは、JetBrains からの [Teamcity](https://www.jetbrains.com/teamcity/) に注目します。

TeamCity のインストールには、いくつかの異なる順列があります。 次の一覧では、これらの順列のいくつかについて説明します。

- **Windows サービス** –このシナリオでは、Windows が windows サービスとして起動すると teamcity が開始されます。 IOS アプリケーションをコンパイルするには、Mac ビルドホストとペアリングする必要があります。

- **OS X でデーモンを起動** する–概念的には、これは前の手順で説明した Windows サービスとして実行するのと似ています。 既定では、ビルドはルートアカウントで実行されます。

- **OS X のユーザーアカウント** –ユーザーがログインするたびに起動するユーザーアカウントで teamcity を実行できます。

前のシナリオでは、OS X のユーザーアカウントで TeamCity を実行するのが最も簡単で、簡単に設定できます。

TeamCity の設定には、次のようないくつかの手順が含まれます。

- **Teamcity** のインストール– teamcity のインストールについては、このガイドでは説明しません。 このガイドでは、TeamCity がインストールされ、ユーザーアカウントで実行されていることを前提としています。 [TeamCity のインストール](https://confluence.jetbrains.com/display/TCD8/Installation)手順については、JetBrains による[teamcity 8 のドキュメント](https://confluence.jetbrains.com/display/TCD8/TeamCity+Documentation)を参照してください。

- **ビルドサーバーの準備** : この手順では、モバイルアプリケーションを構築し、配布の準備を行うために必要なソフトウェア、ツール、および証明書をインストールします。

- **ビルドスクリプトを作成** する-この手順は厳密には必要ありませんが、ビルドスクリプトは、アプリケーションを無人でビルドするのに役立ちます。 ビルドスクリプトを使用すると、発生する可能性のあるビルドの問題のトラブルシューティングに役立ち、継続的インテグレーションが実行されない場合でも、一貫性のある反復可能な方法で配布用のバイナリを作成できます。

- **TeamCity プロジェクトの作成** –前の3つの手順が完了したら、ソースコードを取得し、プロジェクトをコンパイルして、テストを App Center テストに送信するために必要なすべてのメタデータを含む teamcity プロジェクトを作成する必要があります。

## <a name="requirements"></a>要件

[App Center テスト](/appcenter/test-cloud/)の経験が必要です。

TeamCity 8.1 に関する知識が必要です。 TeamCity のインストールについては、このドキュメントでは説明しません。 TeamCity が OS X Mavericks にインストールされており、ルートアカウントではなく通常のユーザーアカウントで実行されていることを前提としています。

ビルドサーバーは、継続的インテグレーション専用の、OS X を実行するスタンドアロンコンピューターである必要があります。 理想的には、ビルドサーバーは、データベースサーバー、web サーバー、開発者ワークステーションなどの他のロールに対して責任を負いません。

> [!IMPORTANT]
> このガイドでは、Xamarin の "ヘッドレス" インストールについては説明しません。

[!include[](~/tools/ci/includes/firewall-information.md)]

## <a name="preparing-the-build-server"></a>ビルドサーバーの準備

ビルドサーバーを構成するための重要な手順は、モバイルアプリケーションをビルドするために必要なすべてのツール、ソフトウェア、および証明書をインストールすることです。 ビルドサーバーでモバイルソリューションをコンパイルし、テストを実行できることが重要です。 構成の問題を最小限に抑えるには、TeamCity をホストしているのと同じユーザーアカウントにソフトウェアとツールをインストールする必要があります。 次の一覧に、必要な情報の詳細を示します。

1. **Visual Studio for Mac** –これには、Xamarin と xamarin Android が含まれます。
2. **Xamarin コンポーネントストアにログインする** –この手順は省略可能で、アプリケーションが Xamarin コンポーネントストアのコンポーネントを使用する場合にのみ必要です。 この時点でコンポーネントストアに事前にログインすると、TeamCity ビルドがアプリケーションのコンパイルを試行しても問題が発生しなくなります。
3. **Xcode** – Xcode は、iOS アプリケーションをコンパイルして署名するために必要です。
4. **Xcode Command-Line ツール** –「 [Ruby With rbenv の更新](https://github.com/calabash/calabash-ios/wiki) 」ガイドの「インストール」セクションの手順 1. で説明されています。
5. **& プロビジョニングプロファイルの署名 id** : XCode を使用して証明書とプロビジョニングプロファイルをインポートします。 詳細については [、「署名 id とプロビジョニングプロファイルのエクスポート](https://developer.apple.com/library/ios/recipes/xcode_help-accounts_preferences/articles/export_signing_assets.html) に関する Apple のガイド」を参照してください。
6. **Android キーストア** –必要な android キーストアを、teamcity ユーザーがアクセスできるディレクトリにコピーし `~/Documents/keystores/MyAndroidApp1` ます。
7. **Calabash** –アプリケーションに calabash を使用して記述されたテストがある場合は、これは省略可能な手順です。 詳細については、「 [OS X Mavericks に Calabash をインストールする](https://github.com/calabash/calabash-ios/wiki) 」および「 [Ruby With rbenv を更新](https://github.com/calabash/calabash-ios/wiki) する」ガイドを参照してください。

次の図は、これらすべてのコンポーネントを示しています。

![この図は、これらすべてのコンポーネントを示しています。](teamcity-images/image1.png)

すべてのソフトウェアがインストールされたら、ユーザーアカウントにログインし、すべてのソフトウェアが正しくインストールされ、動作していることを確認します。 これには、ソリューションのコンパイルと App Center テストへのアプリケーションの送信が含まれます。 これは、次のセクションで説明するように、ビルドスクリプトを実行することで簡略化できます。

## <a name="create-a-build-script"></a>ビルドスクリプトの作成

TeamCity は、モバイルアプリケーションのコンパイルと送信のすべての側面を処理して、App Center テストを単独で行うことができます。ビルドスクリプトを作成することをお勧めします。 ビルドスクリプトには、次のような利点があります。

1. **ドキュメント** –ビルドスクリプトは、ソフトウェアの構築方法に関するドキュメントの形式として機能します。 これにより、アプリケーションの配置に関連する "マジック" の一部が削除され、開発者は機能に専念できます。
1. **再現性** : ビルドスクリプトを使用すると、アプリケーションがコンパイルされて配置されるたびに、どのユーザーがどのように動作するかに関係なく、同じように動作します。 この反復可能な一貫性によって、ビルドまたは人的エラーが不適切に実行されたことが原因で発生する可能性がある問題やエラーが解消されます。
1. **バージョン管理** -ビルドスクリプトをソース管理システムに含めることができます。 つまり、ビルドスクリプトへの変更を追跡、監視、修正して、エラーまたは誤りが見つかった場合には修正できます。
1. **環境を準備** する–ビルドスクリプトには、必要なサードパーティの依存関係をインストールするロジックを含めることができます。 これにより、アプリケーションが適切なコンポーネントでビルドされるようになります。

ビルドスクリプトは、PowerShell ファイル (Windows の場合) または bash スクリプト (OS X の場合) のように簡単にできます。 ビルドスクリプトを作成する場合、スクリプト言語にはいくつかの選択肢があります。

- [**Rake**](https://github.com/jimweirich/rake) : Ruby に基づいてプロジェクトをビルドするための Domain-Specific 言語 (DSL) です。 Rake には、人気と豊富なライブラリのエコシステムの利点があります。

- [](https://github.com/psake/psake)ソフトウェアを構築するための Windows PowerShell ライブラリです。

- [**フェイク**](https://fsharp.github.io/FAKE/) –これは F # に基づく DSL で、必要に応じて既存の .net ライブラリを使用できます。

どのスクリプト言語が使用されるかは、ユーザーの好みや要件によって異なります。

> [!NOTE]
> MSBuild や NAnt などの XML ベースのビルドシステムを使用することはできますが、ソフトウェアの構築専用の DSL の表現力と保守性が欠けています。

### <a name="parameterizing-the-build-script"></a>ビルドスクリプトのパラメーター化

ソフトウェアのビルドとテストのプロセスには、機密情報を保持する必要がある情報が必要です。 APK を作成する場合、キーストアのパスワード、またはキーストアのキーエイリアスが必要になることがあります。 同様に、App Center テストには、開発者に固有の [API キー](/appcenter/api-docs/) が必要です。 これらの種類の値は、ビルドスクリプト内でハードコーディングすることはできません。 代わりに、変数としてビルドスクリプトに渡す必要があります。

機密性が低いは、iOS デバイス ID や、テストの実行に使用 App Center デバイスを識別する Android デバイス ID などの値です。 これらの値は保護する必要がありますが、ビルドごとに変更される可能性があります。

これらの種類の変数をビルドスクリプトの外部に格納すると、開発者は、たとえば、組織内でビルドスクリプトを共有しやすくなります。 開発者は、ビルドサーバーとまったく同じスクリプトを使用できますが、独自のキーストアと [API キー](/appcenter/api-docs/)を使用できます。

これらの機密値を格納するには、次の2つのオプションを使用できます。

- **構成ファイル** – [API キー](/appcenter/api-docs/) を保護するために、この値をバージョン管理にチェックインすることはできません。 ファイルは、コンピューターごとに作成できます。 このファイルから値を読み取る方法は、使用するスクリプト言語によって異なります。

- **環境変数** –コンピューター単位で簡単に設定でき、基になるスクリプト言語に依存しません。

これらの選択肢には長所と短所があります。 TeamCity は環境変数で適切に動作するため、このガイドではビルドスクリプトを作成するときにこの手法をお勧めします。

### <a name="build-steps"></a>ビルドステップ

ビルドスクリプトでは、次の手順を実行する必要があります。

- **アプリケーションをコンパイルし** ます。これには、適切なプロビジョニングプロファイルを使用したアプリケーションへの署名も含まれます。

- **アプリケーションを Xamarin Test Cloud に送信** します。これには、適切なキーストアを使用して apk を署名し、zip に揃えることが含まれます。

これらの2つの手順については、以下で詳しく説明します。

#### <a name="compiling-a-xamarinios-application"></a>Xamarin. iOS アプリケーションのコンパイル

[!include[](~/tools/ci/includes/commandline-compile-of-xamarin-ios-ipa.md)]

#### <a name="compiling-a-xamarinandroid-application"></a>Xamarin Android アプリケーションのコンパイル

Android アプリケーションをコンパイルするには、 **xbuild** (または Windows 上の **msbuild** ) を使用します。

```bash
/Library/Frameworks/Mono.framework/Commands/xbuild /t:SignAndroidPackage /p:Configuration=Release /path/to/android.csproj
```
Android アプリケーションをコンパイルする **xbuild** はプロジェクトを使用し、iOS アプリケーション **xbuild** はソリューションを使用します。

#### <a name="submitting-xamarinuitests-to-app-center"></a>UITests を App Center に送信しています

UITests は、次のスニペットに示すように [APP CENTER CLI](https://github.com/microsoft/appcenter-cli)を使用して送信されます。

```bash
appcenter test run uitest --app <TEAM-NAME/APP-NAME> --devices <DEVICE_SET> --token <API_KEY> --app-path <appname.APK-or-appname.IPA> --merge-nunit-xml report.xml --build-dir pathToUITestBuildDir
```

テストを実行すると、 **report.xml** という名前の NUnit スタイルの XML ファイルの形式でテスト結果が返されます。 TeamCity によって、ビルドログに情報が表示されます。

App Center に UITests を送信する方法の詳細については、「 [Xamarin Android アプリを準備](/appcenter/test-cloud/uitest/preparing-for-upload-android) する」または「 [xamarin IOS アプリを準備](/appcenter/test-cloud/uitest/preparing-for-upload-ios)する」を参照してください。

#### <a name="submitting-calabash-tests-to-app-center"></a>App Center に対する Calabash テストの送信

Calabash テストは、次のスニペットに示すように [APP CENTER CLI](https://github.com/microsoft/appcenter-cli)を使用して送信されます。

```bash
appcenter test run calabash --app <TEAM-NAME/APP-NAME> --devices <DEVICE_SET> --token <API_KEY> --app-path <appname.APK-or-appname.IPA> --project-dir pathToProjectDir
```

Android アプリケーションを App Center テストに送信するには、最初に calabash-android を使用して APK テストサーバーを再構築する必要があります。

```bash
$ calabash-android build </path/to/signed/APK>
$ appcenter test run calabash --app <TEAM-NAME/APP-NAME> --devices <DEVICE_SET> --token <API_KEY> --app-path <appname.APK> --project-dir pathToProjectDir
```

Calabash テストの送信の詳細については、 [Test Cloud への Calabash テストの送信](https://github.com/calabash/calabash-ios/wiki)に関する Xamarin のガイドを参照してください。

## <a name="creating-a-teamcity-project"></a>TeamCity プロジェクトの作成

TeamCity がインストールされ、Visual Studio for Mac プロジェクトをビルドできるようになったら、TeamCity でプロジェクトを作成してプロジェクトをビルドし、App Center に送信します。

1. Web ブラウザーを使用して TeamCity にログインすることで開始されます。 ルートプロジェクトに移動します。

    ![ルートプロジェクトに移動します](teamcity-images/image2.png "ルートプロジェクトに移動します。") 。ルートプロジェクトの下に、新しいサブプロジェクトを作成します。

    ![ルートプロジェクトの下にあるルートプロジェクトに移動し、新しいサブプロジェクトを作成します。](teamcity-images/image3.png "ルートプロジェクトの下にあるルートプロジェクトに移動し、新しいサブプロジェクトを作成します。")
2. サブプロジェクトが作成されたら、新しいビルド構成を追加します。

    ![サブプロジェクトが作成されたら、新しいビルド構成を追加します。](teamcity-images/image5.png "サブプロジェクトが作成されたら、新しいビルド構成を追加します。")
3. ビルド構成に VCS プロジェクトをアタッチします。 これを行うには、[バージョンコントロールの設定] 画面を使用します。

    ![これは、[バージョンコントロールの設定] 画面で行います。](teamcity-images/image6.png "これは、[バージョンコントロールの設定] 画面で行います。")

    VCS プロジェクトが作成されていない場合は、次に示す新しい VCS ルートページから作成できます。

    ![VCS プロジェクトが作成されていない場合は、新しい VCS ルートページから作成できます。](teamcity-images/image7.png "VCS プロジェクトが作成されていない場合は、新しい VCS ルートページから作成することもできます。")

    VCS のルートがアタッチされると、TeamCity によってプロジェクトがチェックアウトされ、ビルドのステップが自動的に検出されます。 TeamCity を使い慣れている場合は、検出されたビルド手順のいずれかを選択できます。 ここでは、検出されたビルドの手順を無視しても安全です。

4. 次に、ビルドトリガーを構成します。 これにより、ユーザーがリポジトリにコードをコミットしたときなど、特定の条件が満たされたときにビルドがキューに入れられます。 次のスクリーンショットは、ビルドトリガーを追加する方法を示しています。

    ![このスクリーンショットは、ビルドトリガーを追加する方法を示し](teamcity-images/image8.png "このスクリーンショットは、ビルドトリガーを追加する方法を示しています。") ています。ビルドトリガーを構成する例を次のスクリーンショットに示します。

    ![ビルドトリガーを構成する例については、このスクリーンショットをご覧ください。](teamcity-images/image9.png "ビルドトリガーを構成する例については、このスクリーンショットをご覧ください。")

5. 前のセクション「ビルドスクリプトのパラメーター化」では、環境変数としていくつかの値を格納することを推奨していました。 これらの変数は、[パラメーター] 画面を使用してビルド構成に追加できます。 次のスクリーンショットに示すように、App Center [API キー](/appcenter/api-docs/)、IOS デバイス id、および ANDROID デバイス id の変数を追加します。

    ![App Center テスト API キー、iOS デバイス ID、および Android デバイス ID の変数を追加します。](teamcity-images/image11.png "Test Cloud API キー、iOS デバイス ID、および Android デバイス ID の変数を追加します。")

6. 最後の手順では、ビルドスクリプトを呼び出してアプリケーションをコンパイルし App Center テストにアプリケーションをエンキューするビルドステップを追加します。 次のスクリーンショットは、Rakefile を使用してアプリケーションをビルドするビルドステップの例です。

    ![このスクリーンショットは、Rakefile を使用してアプリケーションをビルドするビルドステップの例です。](teamcity-images/image12.png "このスクリーンショットは、Rakefile を使用してアプリケーションをビルドするビルドステップの例です。")

7. この時点で、ビルド構成が完了します。 ビルドをトリガーして、プロジェクトが適切に構成されていることを確認することをお勧めします。 これを行うには、小規模で重要ではない変更をリポジトリにコミットすることをお勧めします。 TeamCity はコミットを検出し、ビルドを開始する必要があります。

8. ビルドが完了したら、ビルドログを調べ、注意が必要なビルドに問題または警告があるかどうかを確認します。

## <a name="summary"></a>まとめ

このガイドでは、TeamCity を使用して Xamarin モバイルアプリケーションをビルドし、App Center テストに送信する方法について説明します。 ビルドプロセスを自動化するためのビルドスクリプトの作成について説明しました。 ビルドスクリプトは、アプリケーションのコンパイル、App Center テストへの送信、および結果の待機を行います。

次に、開発者がコードをコミットしてビルドスクリプトを呼び出すたびにビルドをキューにする、TeamCity でプロジェクトを作成する方法について説明します。

## <a name="related-links"></a>関連リンク

- [Xamarin Android アプリを準備する](/appcenter/test-cloud/uitest/preparing-for-upload-android)
- [Xamarin iOS アプリを準備しています](/appcenter/test-cloud/uitest/preparing-for-upload-ios)
- [TeamCity のインストールと構成](https://confluence.jetbrains.com/display/TCD8/Installing+and+Configuring+the+TeamCity+Server)
