---
title: Xamarin.iOS の IPA サポート
description: この記事では、IPA ファイルの作成方法を紹介します。IPA ファイルは、テスト目的か社内用のアプリケーションを社内で配布する目的のために、アドホック配布でアプリケーションを配置するときに利用できます。
ms.prod: xamarin
ms.assetid: D253C2DB-852E-6FC6-C9FD-574730B8DB19
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
ms.openlocfilehash: 70d6b908beb0d04788365b104b5e4a2679b0ebe1
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50113249"
---
# <a name="ipa-support-in-xamarinios"></a>Xamarin.iOS の IPA サポート

_この記事では、IPA ファイルの作成方法を紹介します。IPA ファイルは、テスト目的か社内用のアプリケーションを社内で配布する目的のために、アドホック配布でアプリケーションを配置するときに利用できます。_

アプリケーションは iTunes App Store を通じて公開し、販売するだけでなく、次の用途でも展開できます。

- **アドホック テスト** — iOS アプリケーションは最大 100 ユーザー (固有の iOS デバイス UUID) にアルファ版かベータ版をテストする目的で展開できます。 Apple の開発者アカウントにテスト iOS デバイスを追加する方法については、「[開発用の iOS デバイスのプロビジョニング](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioning)」を参照してください。[アドホック](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md) ガイドにもこの配布方法に関する詳細が記載されています。
- **社内/企業展開** — iOS アプリケーションは内部で、つまり、企業内で展開できます。これには、Apple Developer Enterprise プログラムに加入する必要があります。 社内配布の詳細は[社内配布](~/ios/deploy-test/app-distribution/in-house-distribution.md)ガイドにあります。

いずれの場合でも、IPA パッケージを作成し、適切な配布プロビジョニング プロファイルでデジタル署名する必要があります。 この記事では、Mac または Windows PC の iTunes で、IPA パッケージをビルドし、iOS デバイスにそのパッケージをインストールするための手順について説明します。

<a name="iTunesMetadata" />

## <a name="the-itunesmetadataplist-file"></a>iTunesMetadata.plist ファイル

iTunes App Store での販売または無料リリースのために iOS アプリケーションを iTunes Connect で作成するとき、開発者はアプリケーションのジャンル、サブジャンル、著作権の通知、サポートされている iOS デバイス、必要なデバイス機能などの情報を指定できます。

アドホック配布か社内配布で配信される iOS アプリケーションは、そのような情報を iTunes やユーザーのデバイスで表示するための何らかの方法を備えている必要があります。 既定は、プロジェクトをビルドするたびに小容量の iTunesMetadata.plist ファイルが作成され、プロジェクト ディレクトリに保存されます。

カスタム **iTunesMetadata.plist** を作成し、配布情報を追加することもできます。 このファイルのコンテンツに関する詳細やファイルの作成方法については、「[iTunesMetadata.plist の内容](~/ios/deploy-test/app-distribution/itunesmetadata.md#iTunesMetadata_contents)」と「[iTunesMetadata.plist ファイルの作成](~/ios/deploy-test/app-distribution/itunesmetadata.md#iTunesMetadata_creating)」を参照してください。

<a name="iTunesArtwork" />

## <a name="itunes-artwork"></a>iTunes アートワーク

App Store 以外の方法でアプリを配信するとき、512 x 512 と 1024 x 1024 の画像も含める必要があります。画像は iTunes でそのアプリを表示するときに利用されます。

iTunes アートワークは次の手順で指定します。

1. **ソリューション エクスプローラー**で **Info.plist** ファイルをダブルクリックし、編集用に開きます。
2. エディターの **iTunes アートワーク** セクションまでスクロールします。
3. 画像が表示されていない場合、エディターでサムネイルをクリックし、**[ファイルを開く]** ダイアログ ボックスから iTunes アートワークにする画像ファイルを選択し、**[OK]** または **[開く]** ボタンをクリックします。
4. アプリケーションに必要な画像がすべて指定されるまでこの手順を繰り返します。

詳細については、「[iTunes アートワーク](~/ios/app-fundamentals/images-icons/app-icons.md)」を参照してください。

<a name="createipa" />

## <a name="creating-an-ipa"></a>IPA を作成する

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

IPA 作成は新しい公開ワークフローに組み込まれました。 IPA を作成するには、以下の指示に従い、アプリをアーカイブに収め、署名し、IPA を保存します。

プラットフォームに依存しないソリューションのための IPA を作成する前に、iOS プロジェクトがスタートアップ プロジェクトとして選択されていることを確認してください。

![](ipa-support-images/setasstartup.png "スタートアップ プロジェクトとして選択されている iOS プロジェクト")

### <a name="build-your-archive"></a>アーカイブをビルドする

IPA をビルドするには、アプリケーションのリリース ビルドの_アーカイブ_を作成する必要があります。 このアーカイブには、アプリとアプリを特定する情報が入っています。


1. Visual Studio for Mac で**リリースとデバイス**の構成を選択します。

    ![](ipa-support-images/buildxs01new.png "リリースの選択 | デバイスの構成")

1. **[ビルド]** メニューから **[発行のためのアーカイブ]** を選択します。

    ![](ipa-support-images/buildxs02new.png "[発行のためのアーカイブ] を選択します")

1. アーカイブが作成されると、**[アーカイブ]** ビューが表示されます。

    ![](ipa-support-images/buildxs03new.png "[アーカイブ] ビューが表示されます")


### <a name="sign-and-distribute-your-app"></a>アプリに署名して配布する

アーカイブのためにアプリケーションをビルドするたびに、**アーカイブ ビュー**が自動的に開き、アーカイブされているすべてのプロジェクトがソリューション別にグループ化されて表示されます。 既定では、このビューには現在開いているソリューションのみが表示されます。 アーカイブのあるソリューションをすべて表示するには、**[アーカイブをすべて表示]** オプションをクリックします。

(アドホック展開または社内展開で) 顧客に展開したアーカイブは保存しておくことが推奨されます。デバッグ情報が生成された場合、後でそれを記号で表すことができます。

App Store 以外のビルドで、**iTunesMetadata.plist** ファイルと iTunes アートワーク セットがアーカイブに入っている場合、これらは自動的に IPA に自動的に含まれます。

アプリに署名し、配布の準備をするには、次のようにします。


1. 下の画像のように、**[署名と配布...]** ボタンを選択します。

    ![](ipa-support-images/buildxs04new.png "[署名と配布] を選択します")

1. これにより、発行ウィザードが開きます。 配布チャネルとして **[アドホック]** か **[エンタープライズ]** (社内) を選択し、パッケージを作成します。

    ![](ipa-support-images/distribute01.png "[アドホック] か [エンタープライズ] (社内) を選択します")

1. [プロビジョニング プロファイル] 画面で、署名 ID と対応するプロビジョニング プロファイルを選択するか、別の ID で再署名します。

    ![](ipa-support-images/distribute02.png "署名 ID と対応するプロビジョニング プロファイルを選択します")

1. パッケージの詳細を確認し、**[発行]** をクリックします。

    ![](ipa-support-images/distribute03.png "パッケージの詳細を確認します")

1. 最後に、コンピューターに IPA を保存します。

    ![](ipa-support-images/distribute04.png "コンピューターに IPA を保存します")


### <a name="building-via-the-command-line-on-mac"></a>コマンドラインからビルドする (Mac)

CI 環境などでは、コマンド ラインから IPA をビルドしなければならないことがあります。 以下の手順でビルドします。


1. [プロジェクト オプション] の [iOS IPA オプション] で、**[Include iTunesArtwork images]\(iTunesArtwork 画像を含める\)** と **[アドホック/エンタープライズ パッケージ (IPA) をビルドする]** が選択されていることを確認します。

    ![](ipa-support-images/imagexs04.png "[Include iTunesArtwork images]\(iTunesArtwork 画像を含める\) と [アドホック/エンタープライズ パッケージ (IPA) をビルドする] が選択されています")

    代わりに、テキスト エディターで **.csproj** ファイルを編集し、この 2 つに相当するプロパティを `PropertyGroup` に手動で追加できます。この構成がアプリのビルドに利用されます。

    ```xml
    <BuildIpa>true</BuildIpa>
    <IpaIncludeArtwork>false</IpaIncludeArtwork>
    ```

1. 任意の **iTunesMetadata.plist** ファイルを含める場合、**[...]** ボタンをクリックして一覧から選択し、**[OK]** ボタンをクリックします。

     ![](ipa-support-images/imagexs03.png "一覧から iTunesMetadata.plist を選択します")

1. **msbuild** を直接呼び出し、コマンドラインで次のプロパティを渡します。

    ```bash
    /Library/Frameworks/Mono.framework/Commands/msbuild YourSolution.sln /p:Configuration=Ad-Hoc /p:Platform=iPhone /p:BuildIpa=true
    ```

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

プロビジョニング プロファイルが作成され、選択され、任意の **iTunesMetadata.plist** ファイルが作成され、iTunes アートワークが Visual Studio に設定されていれば、配布のために IPA をビルドできます。 次に、プロジェクトを構成します。 次の手順で行います。

1. **ソリューション エクスプローラー**で、Xamarin.iOS プロジェクト名を右クリックし、**[プロパティ]** を選択して編集用に開きます。

    ![](ipa-support-images/imagevs01.png "[プロパティ] を選択します")

2. **[iOS IPA オプション]** を選択し、**[構成]** ドロップダウン リストから **[アドホック]** を選択します。

    ![](ipa-support-images/imagevs02.png "[構成] ドロップダウン リストから [アドホック] を選択します")

    > [!NOTE]
    > 新しい Xamarin.iOS プロジェクトでは、アドホック構成を選択できない場合があります。 選択できない場合、**[リリース]** 構成を選択します。

3. 任意の **iTunesMetadata.plist** ファイルを含める場合、**[...]** ボタンをクリックして一覧から選択し、**[開く]** ボタンをクリックします。

    ![](ipa-support-images/imagevs03.png "一覧から iTunesMetadata.plist を選択します")

4. 必要に応じて、IPA に **[パッケージ名]** を指定できます。指定しない場合、Xamarin.iOS プロジェクトと同じ名前が付けられます。
5. プロジェクト プロパティに変更を保存します。
6. **[ビルド構成]** に **[アドホック]** があれば、それを選択します。 ない場合、**[リリース]** を選択します。

    ![](ipa-support-images/imagevs05.png "[ビルド構成] ドロップダウン リストから [アドホック] を選択します")

7. プロジェクトをビルドし、IPA パッケージを作成します。
8. IPA は **[Bin] の [iOS デバイス] にある [アドホック] (または[リリース])** フォルダーでビルドされます。

    ![](ipa-support-images/imagevs06.png "エクスプローラーの IPA")

-----

<a name="Customizing-the-IPA-Location" />

## <a name="customizing-the-ipa-location"></a>IPA の場所のカスタマイズ

新しい **MSBuild** プロパティ `IpaPackageDir` が追加され、**.ipa** ファイルの出力場所を簡単にカスタマイズできるようになりました。 `IpaPackageDir` がカスタムの場所に設定されている場合、タイムスタンプが与えられた既定の下位ディレクトリではなく、その場所に **.ipa** ファイルが置かれます。 このような変更は、継続的インテグレーション (CI) ビルドに利用される自動化ビルドのように、正常に動作するために特定のディレクトリ パスに依存する自動化ビルドを作成する際に便利な場合があります。

新しいプロパティの利用はいくつかの方法で利用される可能性があります。

たとえば、(Xamarin.iOS 9.6 以前の) 既定のディレクトリに **.ipa** ファイルを出力するには、次のいずれかの方法で `IpaPackageDir` プロパティを `$(OutputPath)` に設定します。 いずれの方法も、IDE ビルドや **msbuild**、**xbuild**、**mdtool** を利用するコマンド ライン ビルドなど、あらゆる Unified API Xamarin.iOS ビルドに対応しています。

- 最初のオプションでは、**MSBuild** ファイルの `<PropertyGroup>` 要素内で `IpaPackageDir` プロパティを設定します。 たとえば、次の `<PropertyGroup>` を iOS アプリ プロジェクト **.csproj** ファイルの一番下に追加できます (結びの `</Project>` タグの直前)。

    ```xml
    <PropertyGroup>
        <IpaPackageDir>$(OutputPath)</IpaPackageDir>
    </PropertyGroup>
    ```

- もっと良い手法は、**.ipa** ファイルのビルドに利用される構成に相当する既存の `<PropertyGroup>` の一番下に `<IpaPackageDir>` 要素を追加することです。 この方法が優れているのは、iOS IPA オプション プロジェクト プロパティ ページで予定されている設定との将来的な互換性がプロジェクトに与えられるためです。 現在、`Release|iPhone` 構成を利用して **.ipa** ファイルをビルドしている場合、更新されたプロパティ グループは次のようになります。

    ```xml
    <PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Release|iPhone' ">
        <Optimize>true</Optimize>
        <OutputPath>bin\iPhone\Release</OutputPath>
        <ErrorReport>prompt</ErrorReport>
        <WarningLevel>4</WarningLevel>
        <ConsolePause>false</ConsolePause>
        <CodesignKey>iPhone Developer</CodesignKey>
        <MtouchUseSGen>true</MtouchUseSGen>
        <MtouchUseRefCounting>true</MtouchUseRefCounting>
        <MtouchFloat32>true</MtouchFloat32>
        <CodesignEntitlements>Entitlements.plist</CodesignEntitlements>
        <MtouchLink>SdkOnly</MtouchLink>
        <MtouchArch>;ARMv7, ARM64</MtouchArch>
        <MtouchHttpClientHandler>HttpClientHandler</MtouchHttpClientHandler>
        <MtouchTlsProvider>Default</MtouchTlsProvider>
        <PlatformTarget>x86&</PlatformTarget>
        <BuildIpa>true</BuildIpa>
        <IpaPackageDir>$(OutputPath</IpaPackageDir>
    </PropertyGroup>
    ```

コマンド ライン ビルドの **msbuild** または **xbuild** のための代替手法は、`/p:` 引数を追加し、`IpaPackageDir` プロパティを設定することです。 代替手法を使う場合、**msbuild** はコマンド ラインに渡される `$()` 式を展開しません。そのため、`$(OutputPath)` 構文は利用できません。 代わりに、完全パス名を指定する必要があります。 Mono の **xbuild** コマンドは `$()` 式を展開しますが、それでも完全パス名の指定が推奨されます。**xbuild** は非推奨とされ、[クロスプラットフォーム バージョンの **msbuild**](https://www.mono-project.com/docs/about-mono/releases/5.0.0/#msbuild) に取って代わられたためです。

この手法を Windows で行うと次のようになります。

```bash
msbuild /p:Configuration="Release" /p:Platform="iPhone" /p:ServerAddress="192.168.1.3" /p:ServerUser="macuser" /p:IpaPackageDir="%USERPROFILE%\Builds" /t:Build SingleViewIphone1.sln
```
Mac の場合は次のようになります。

```bash
msbuild /p:Configuration="Release" /p:Platform="iPhone" /p:IpaPackageDir="$HOME/Builds" /t:Build SingleViewIphone1.sln
```

<a name="installipa" />

## <a name="installing-an-ipa-using-itunes"></a>iTunes で IPA をインストールする

生成された IPA パッケージはテスト ユーザーのもとに届け、iOS デバイスにインストールしてもらったり、エンタープライズ展開のために出荷したりできます。 選択された方法に関係なく、エンド ユーザーは Mac または Windows PC で IPA ファイルをダブルクリックするか、開いている iTunes ウィンドウにドラッグし、iTunes にパッケージをインストールします。

新しい iOS アプリケーションが **[マイ アプリ]** セクションに表示されます。アプリケーションを右クリックすると、アプリケーションに関する情報が表示されます。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

 ![](ipa-support-images/installxs01.png "新しい iOS アプリケーションが [マイ アプリ] セクションに表示されます")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

 ![](ipa-support-images/installvs01.png "新しい iOS アプリケーションが [マイ アプリ] セクションに表示されます")

-----

ユーザーは iTunes とデバイスを同期し、新しい iOS アプリケーションをインストールできます。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、App Store 以外のビルドのために Xamarin.iOS アプリケーションを用意するための手順について説明しました。 IPA パッケージを作成する方法と、テストや社内配布のために iOS アプリケーションをエンド ユーザーの iOS デバイスにインストールする方法を紹介しました。


## <a name="related-links"></a>関連リンク

- [App Store の配布](~/ios/deploy-test/app-distribution/app-store-distribution/index.md)
- [iTunes Connect でのアプリの構成](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [App Store への発行](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)
- [社内配布](~/ios/deploy-test/app-distribution/in-house-distribution.md)
- [アドホック配布](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)
- [iTunesMetadata.plist ファイル](~/ios/deploy-test/app-distribution/itunesmetadata.md)
- [トラブルシューティング](~/ios/deploy-test/troubleshooting.md)
- [iTunes アートワーク](~/ios/app-fundamentals/images-icons/app-icons.md#itunes)
- [iOS デバイス向けエンタープライズ アプリの配布](http://developer.apple.com/library/ios/#featuredarticles/FA_Wireless_Enterprise_App_Distribution/Introduction/Introduction.html)
