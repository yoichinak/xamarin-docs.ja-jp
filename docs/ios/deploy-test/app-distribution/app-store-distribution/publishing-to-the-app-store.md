---
title: App Store への Xamarin.iOS アプリの公開
description: このドキュメントでは、App Store で配布する Xamarin.iOS アプリケーションの構成、ビルド、発行の方法を示します。
ms.prod: xamarin
ms.assetid: DFBCC0BA-D233-4DC4-8545-AFBD3768C3B9
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/25/2018
ms.openlocfilehash: 3803d7e14b161a7c166bcae37e3d9f46b7637984
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73026639"
---
# <a name="publishing-xamarinios-apps-to-the-app-store"></a>App Store への Xamarin.iOS アプリの公開

[App Store](https://www.apple.com/ios/app-store/) にアプリを公開する場合、アプリ開発者は、まず、スクリーンショット、説明、アイコン、およびその他の情報と共に、そのアプリを審査のために Apple に提出する必要があります。 アプリの承認後、Apple ではそのアプリを App Store に配置します。そこでユーザーはアプリを購入し、自分の iOS デバイスから直接インストールすることができます。

このガイドでは、App Store 用のアプリを準備し、審査のために Apple に送信する場合に従う手順について説明します。 具体的には、以下の作業について説明します。

> [!div class="checklist"]
>
> - App Store 審査ガイドラインに従う
> - App ID と権利の設定
> - App Store アイコンとアプリ アイコンの提供
> - App Store プロビジョニング プロファイルの設定
> - **リリース** ビルド構成の更新
> - iTunes Connect でのアプリの構成
> - アプリのビルドと Apple への提出

> [!IMPORTANT]
> Apple は、2019 年 3 月以降に App Store に提出されるすべてのアプリおよび更新プログラムが iOS 12.1 SDK (Xcode 10.1 以降に含まれている) でビルドされる必要があることを[通知しました](https://developer.apple.com/ios/submit/)。
> アプリでは、iPhone XS および 12.9 インチ iPad Pro の画面サイズもサポートされる必要もあります。

## <a name="app-store-guidelines"></a>App Store のガイドライン

App Store で公開するためにアプリを提出する前に、そのアプリが Apple の「[App Store 審査ガイドライン](https://developer.apple.com/appstore/resources/approval/guidelines.html)」で定義されている基準を満たしていることを確認します。
App Store にアプリを提出すると、Apple ではそのアプリを審査し、要件を満たしていることを確認します。 要件を満たしていない場合、Apple はそのアプリを却下します。その場合、示された問題に対処し、再提出する必要があります。
したがって、開発プロセスでできるだけ早くガイドラインをよく理解することをお勧めします。

アプリを提出する際の注意事項をいくつか以下に示します。

1. アプリの説明がその機能と一致することを確認します。
2. アプリが通常の使用でクラッシュしないことをテストします。 これには、サポートされるすべての iOS デバイスでの使用が含まれます。

Apple で提供される [App Store に関連するリソース](https://developer.apple.com/app-store/resources/)も参照してください。

## <a name="set-up-an-app-id-and-entitlements"></a>アプリ ID と権利を設定する

iOS アプリにはそれぞれ一意のアプリ ID があり、*権利* と呼ばれる、一連のアプリケーション サービスが関連付けられています。 権利により、アプリでは、プッシュ通知の受信や、HealthKit などの iOS 機能へのアクセスなど、さまざまな作業を行うことができます。

アプリ ID を作成し、必要な権利を選択するには、[Apple Developer ポータル](https://developer.apple.com/account/)にアクセスして、次の手順に従います。

1. **[Certificates, IDs & Profiles]\(証明書、ID およびプロファイル\)** セクションで、 **[識別子]、[アプリ ID]** の順に選択します。
2. **+** ボタンをクリックし、新しいアプリケーションの**名前**と**バンドル ID** を指定します。
3. 画面の下部までスクロールし、Xamarin.iOS アプリケーションで必要になる **App Services** を選択します。 App Services の詳細については、「[Xamarin.iOS の機能の使用](~/ios/deploy-test/provisioning/capabilities/index.md)」ガイドを参照してください。
4. **[続行]** ボタンをクリックし、画面の指示に従って新しいアプリ ID を作成します。

アプリ ID を定義する際に必要なアプリケーション サービスを選択して構成するだけでなく、アプリ ID と権利を Xamarin.iOS プロジェクトで構成する必要があります。その場合は、**Info.plist** ファイルと **Entitlements.plist** ファイルを編集します。 詳細については、「[Xamarin.iOS での権利の使用](~/ios/deploy-test/provisioning/entitlements.md)」ガイドを参照してください。このガイドでは、**Entitlements.plist** ファイルの作成方法と、そのファイルに含まれるさまざまな権利設定の意味について説明しています。

## <a name="include-an-app-store-icon"></a>App Store アイコンを含める

Apple にアプリを提出する場合は、App Store アイコンを含むアセット カタログが含まれていることを確認してください。 これを行う方法については、「[App Store icons in Xamarin.iOS](~/ios/app-fundamentals/images-icons/app-store-icon.md)」 (Xamarin.iOS の App Store アイコン) ガイドを参照してください。

## <a name="set-the-apps-icons-and-launch-screens"></a>アプリ アイコンと起動画面を設定する

Apple が iOS アプリを App Store で利用できるようにするには、アプリを実行できるすべての iOS デバイスの適切なアイコンと起動画面が必要になります。 アプリ アイコンと起動画面の設定の詳細については、以下のガイドを参照してください。

- [Xamarin.iOS のアプリケーション アイコン](~/ios/app-fundamentals/images-icons/app-icons.md)
- [Xamarin.iOS アプリの起動画面](~/ios/app-fundamentals/images-icons/launch-screens.md)

## <a name="create-and-install-an-app-store-provisioning-profile"></a>App Store プロビジョニング プロファイルを作成してインストールする

iOS では*プロビジョニング プロファイル*を使用して、特定のアプリケーション ビルドの展開方法を制御します。 これらは、アプリの署名に使用される証明書、アプリ ID、およびアプリをインストールできる場所に関する情報を含むファイルです。 開発とアドホック配布の場合、プロビジョニング プロファイルには、アプリを展開できる許可デバイスのリストも含まれます。 ただし、App Store での配布の場合は、証明書とアプリ ID の情報のみが含まれます。これは、App Store でのみ一般に配布されるためです。

App Store プロビジョニング プロファイルを作成してインストールするには、次の手順に従います。

1. [Apple Developer ポータル](https://developer.apple.com/account/)にログインします。
2. **[Certificates, IDs & Profiles]\(証明書、ID およびプロファイル\)** で、 **[プロビジョニング プロファイル]、[配布]** の順に選択します。
3. **+** ボタンをクリックして **[App Store]** を選択します。次に **[続行]** をクリックします。
4. リストからご利用のアプリの **[アプリ ID]** を選択して、 **[続行]** をクリックします。
5. 署名証明書を選択して、 **[続行]** をクリックします。
6. **[プロファイル名]** を入力し、 **[続行]** をクリックしてプロファイルを生成します。
7. Xamarin の [Apple アカウント管理](~/cross-platform/macios/apple-account-management.md)ツールを使用して、新しく作成されたプロビジョニング プロファイルをご利用の Mac にダウンロードします。 Mac を使用している場合は、Apple Developer ポータルから直接プロビジョニング プロファイルをダウンロードし、ダブルクリックしてインストールすることもできます。

詳しい手順については、[配布プロファイルの作成](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioningprofile)に関するページと「[Xamarin.iOS プロジェクトでの配布プロファイルの選択](~/ios/deploy-test/app-distribution/app-store-distribution/index.md#selectprofile)」を参照してください。

## <a name="update-the-release-build-configuration"></a>リリース ビルド構成を更新する

新しい Xamarin.iOS プロジェクトでは、**デバッグ**および**リリース** _ビルド構成_が自動的に設定されます。 **リリース** ビルドを正しく構成するには、次の手順に従います。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. **Solution Pad** から、**Info.plist** を開きます。 **[手動プロビジョニング]** を選択します。 ファイルを保存して閉じます。
2. **Solution Pad** で **[プロジェクト名]** を右クリックし、 **[オプション]** を選択して **[iOS ビルド]** タブに移動します。
3. **[構成]** を **[リリース]** に、 **[プラットフォーム]** を **[iPhone]** に設定します。
4. 特定の iOS SDK でビルドするには、 **[SDK バージョン]** リストからそれを選択します。 それ以外の場合は、この値を**既定**のままにしておきます。
5. リンクを設定し、未使用のコードを削除することで、アプリケーションの全体のサイズが小さくなります。 ほとんどの場合、 **[リンカーの動作]** は既定値の **[フレームワーク SDK のみをリンクする]** に設定する必要があります。 いくつかのサード パーティ製ライブラリを使用するときなど、必要なコードが削除されないようにするため、この値を **[リンクしない]** に設定する必要がある場合もあります。 詳細については、「[Xamarin.iOS アプリをリンクする](~/ios/deploy-test/linker.md)」ガイドを参照してください。
6. **[PNG 画像を最適化する]** をオンにして、アプリケーションのサイズをさらに小さくします。
7. デバッグはビルド サイズが不必要に大きくなるため、有効に_しない_ でください。
8. iOS 11 の場合は、**ARM64** をサポートするデバイス アーキテクチャの 1 つを選択します。 64 ビット iOS デバイスのビルドの詳細については、「[32/64 ビット プラットフォームの考慮事項](~/cross-platform/macios/32-and-64/index.md)」ドキュメントの「**Enabling 64-Bit Builds of Xamarin.iOS Apps**」 (Xamarin.iOS アプリの 64 ビット ビルドの有効化) セクションを参照してください。
9. **LLVM** コンパイラを使用すれば、より小さく高速なコードをビルドすることができます。 しかし、このオプションではコンパイル時間が増加します。
10. アプリケーションのニーズに基づいて、**ガベージ コレクション**の種類を調整したり、**国際化**対応のための設定をすることもできます。

    上記のオプションを設定した後のビルド設定は次のようになります。

    ![iOS ビルド設定](publishing-to-the-app-store-images/build-m157.png "iOS ビルド設定")

    ビルド設定について詳しく説明されている、こちらの「[iOS ビルドのしくみ](~/ios/deploy-test/ios-build-mechanics.md)」ガイドも参照してください。

11. **[iOS バンドル署名]** タブに移動します。ここでオプションが編集できない場合は、**Info.plist** ファイルで **[手動プロビジョニング]** が選択されていることを確認します。
12. **[構成]** が **[リリース]** に、 **[プラットフォーム]** が **[iPhone]** に設定されていることを確認します。
13. **[署名 ID]** を **[配布 (自動)]** に設定します。
14. **[プロビジョニング プロファイル]** では、[前の手順で作成した](#create-and-install-an-app-store-provisioning-profile) App Store プロビジョニング プロファイルを選択します。

    これで、プロジェクトのバンドル署名オプションは次のようになります。

    ![iOS バンドル署名](publishing-to-the-app-store-images/bundleSigning-m157.png "iOS バンドル署名")

15. **[OK]** をクリックして、プロジェクト プロパティへの変更を保存します。

# <a name="visual-studio-2019tabwindows"></a>[Visual Studio 2019](#tab/windows)

1. Visual Studio 2019 が [Mac ビルド ホストとペアリング](~/ios/get-started/installation/windows/connecting-to-mac/index.md)されていることを確認します。
2. **ソリューション エクスプローラー**で **[プロジェクト名]** を右クリックし、 **[プロパティ]** を選択します。
3. **[iOS ビルド]** タブに移動して、 **[構成]** を **[リリース]** に、 **[プラットフォーム]** を **[iPhone]** に設定します。
4. 特定の iOS SDK でビルドするには、 **[SDK バージョン]** リストからそれを選択します。 それ以外の場合は、この値を**既定**のままにしておきます。
5. リンクを設定し、未使用のコードを削除することで、アプリケーションの全体のサイズが小さくなります。 ほとんどの場合、 **[リンカーの動作]** は既定値の **[フレームワーク SDK のみをリンクする]** に設定する必要があります。 いくつかのサード パーティ製ライブラリを使用するときなど、必要なコードが削除されないようにするため、この値を **[リンクしない]** に設定する必要がある場合もあります。 詳細については、「[Xamarin.iOS アプリをリンクする](~/ios/deploy-test/linker.md)」ガイドを参照してください。
6. **[PNG 画像を最適化する]** をオンにして、アプリケーションのサイズをさらに小さくします。
7. デバッグはビルド サイズが不必要に大きくなるため、有効にしないでください。
8. iOS 11 の場合は、**ARM64** をサポートするデバイス アーキテクチャの 1 つを選択します。 64 ビット iOS デバイスのビルドの詳細については、「[32/64 ビット プラットフォームの考慮事項](~/cross-platform/macios/32-and-64/index.md)」ドキュメントの「**Enabling 64-Bit Builds of Xamarin.iOS Apps**」 (Xamarin.iOS アプリの 64 ビット ビルドの有効化) セクションを参照してください。
9. **LLVM** コンパイラを使用すれば、より小さく高速なコードをビルドすることができます。 しかし、このオプションではコンパイル時間が増加します。
10. アプリケーションのニーズに基づいて、**ガベージ コレクション**の種類を調整したり、**国際化**対応のための設定をすることもできます。

    上記のオプションを設定した後のビルド設定は次のようになります。

    ![iOS ビルド設定](publishing-to-the-app-store-images/build-w157.png "iOS ビルド設定")

    ビルド設定について詳しく説明されている、こちらの「[iOS ビルドのしくみ](~/ios/deploy-test/ios-build-mechanics.md)」ガイドも参照してください。

11. **[iOS バンドル署名]** タブに移動します。 **[構成]** が **[リリース]** に、 **[プラットフォーム]** が **[iPhone]** に設定され、かつ **[手動プロビジョニング]** が選択されていることを確認します。
12. **[署名 ID]** を **[配布 (自動)]** に設定します。
13. **[プロビジョニング プロファイル]** では、[前の手順で作成した](#create-and-install-an-app-store-provisioning-profile) App Store プロビジョニング プロファイルを選択します。

    これで、プロジェクトのバンドル署名オプションは次のようになります。

    ![iOS バンドル署名の設定](publishing-to-the-app-store-images/bundleSigning-w157.png "iOS バンドル署名の設定")

14. ビルド構成を保存して閉じます。

# <a name="visual-studio-2017tabwin-vs2017"></a>[Visual Studio 2017](#tab/win-vs2017)

1. Visual Studio 2017 が [Mac ビルド ホストとペアリング](~/ios/get-started/installation/windows/connecting-to-mac/index.md)されていることを確認します。
2. **ソリューション エクスプローラー**で **[プロジェクト名]** を右クリックし、 **[プロパティ]** を選択します。
3. **[iOS ビルド]** タブに移動して、 **[構成]** を **[リリース]** に、 **[プラットフォーム]** を **[iPhone]** に設定します。
4. 特定の iOS SDK でビルドするには、 **[SDK バージョン]** リストからそれを選択します。 それ以外の場合は、この値を**既定**のままにしておきます。
5. リンクを設定し、未使用のコードを削除することで、アプリケーションの全体のサイズが小さくなります。 ほとんどの場合、 **[リンカーの動作]** は既定値の **[フレームワーク SDK のみをリンクする]** に設定する必要があります。 いくつかのサード パーティ製ライブラリを使用するときなど、必要なコードが削除されないようにするため、この値を **[リンクしない]** に設定する必要がある場合もあります。 詳細については、「[Xamarin.iOS アプリをリンクする](~/ios/deploy-test/linker.md)」ガイドを参照してください。
6. **[PNG 画像を最適化する]** をオンにして、アプリケーションのサイズをさらに小さくします。
7. デバッグはビルド サイズが不必要に大きくなるため、有効にしないでください。
8. iOS 11 の場合は、**ARM64** をサポートするデバイス アーキテクチャの 1 つを選択します。 64 ビット iOS デバイスのビルドの詳細については、「[32/64 ビット プラットフォームの考慮事項](~/cross-platform/macios/32-and-64/index.md)」ドキュメントの「**Enabling 64-Bit Builds of Xamarin.iOS Apps**」 (Xamarin.iOS アプリの 64 ビット ビルドの有効化) セクションを参照してください。
9. **LLVM** コンパイラを使用すれば、より小さく高速なコードをビルドすることができます。 しかし、このオプションではコンパイル時間が増加します。
10. アプリケーションのニーズに基づいて、**ガベージ コレクション**の種類を調整したり、**国際化**対応のための設定をすることもできます。

    上記のオプションを設定した後のビルド設定は次のようになります。

    ![iOS ビルド設定](publishing-to-the-app-store-images/build-w157.png "iOS ビルド設定")

    ビルド設定について詳しく説明されている、こちらの「[iOS ビルドのしくみ](~/ios/deploy-test/ios-build-mechanics.md)」ガイドも参照してください。

11. **[iOS バンドル署名]** タブに移動します。 **[構成]** が **[リリース]** に、 **[プラットフォーム]** が **[iPhone]** に設定され、かつ **[手動プロビジョニング]** が選択されていることを確認します。
12. **[署名 ID]** を **[配布 (自動)]** に設定します。
13. **[プロビジョニング プロファイル]** では、[前の手順で作成した](#create-and-install-an-app-store-provisioning-profile) App Store プロビジョニング プロファイルを選択します。

    これで、プロジェクトのバンドル署名オプションは次のようになります。

    ![iOS バンドル署名の設定](publishing-to-the-app-store-images/bundleSigning-w157.png "iOS バンドル署名の設定")

14. **[iOS IPA オプション]** タブに移動します。
15. **[構成]** が **[リリース]** に、 **[プラットフォーム]** が **[iPhone]** に設定されていることを確認します。
16. **[iTunes Package Archive (IPA) をビルドする]** チェック ボックスをオンにします。 この設定により、各**リリース** ビルド (これが選択された構成であるため) で .ipa ファイルが生成されるようになります。 App Store でのリリースのためにこのファイルを Apple に提出することができます。

    > [!NOTE]
    > App Store でのリリースには、**iTunes メタデータ**と **iTunesArtwork** は必要ありません。 詳細については、「[Xamarin.iOS アプリの iTunesMetadata.plist ファイル](~/ios/deploy-test/app-distribution/itunesmetadata.md)」と「[iTunes アートワーク](~/ios/app-fundamentals/images-icons/app-icons.md#itunes-artwork)」を参照してください。

17. Xamarin.iOS プロジェクト名とは異なる .ipa ファイル名を指定するには、 **[パッケージ名]** フィールドにその名前を入力します。

    ![iOS バンドル署名の設定](publishing-to-the-app-store-images/ipaOptions-w157.png "iOS バンドル署名の設定")

18. ビルド構成を保存して閉じます。

-----

## <a name="configure-your-app-in-itunes-connect"></a>iTunes Connect でアプリを構成する

[iTunes Connect](https://itunesconnect.apple.com) は、App Store で iOS アプリケーションを管理するための Web ベースのツール群です。 Xamarin.iOS アプリケーションを審査のために Apple に提出し、App Store でリリースする前に、iTunes Connect でアプリケーションを正しく構成する必要があります。

これを行う方法については、「[iTunes Connect でのアプリの構成](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)」ガイドを参照してください。

## <a name="build-and-submit-your-app"></a>アプリをビルドして提出する

ビルド設定が正しく構成され、iTunes Connect が提出待ちの状態になったら、アプリをビルドして Apple に提出することができます。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. Visual Studio for Mac で、 **[リリース]** ビルド構成とビルド対象のデバイス (シミュレーターではない) を選択します。

    ![ビルド構成とプラットフォームの選択](publishing-to-the-app-store-images/chooseConfig-m157.png "ビルド構成とプラットフォームの選択")

2. **[ビルド]** メニューから、 **[発行のためのアーカイブ]** を選択します。
3. アーカイブが作成されると、 **[アーカイブ]** ビューが表示されます。 **[署名と配布]** をクリックして、発行ウィザードを開きます。

    ![[アーカイブ] ビューの [署名と配布] ボタンの場所のスクリーンショット。](publishing-to-the-app-store-images/archives-mac.png "[アーカイブ] ビューの [署名と配布] ボタンの場所のスクリーンショット。")

    > [!NOTE]
    > 既定では、 **[アーカイブ]** ビューには開いているソリューションのアーカイブのみが表示されます。 アーカイブのあるソリューションをすべて表示するには、 **[アーカイブをすべて表示]** チェック ボックスをオンにします。 古いアーカイブを保持し、その中に含まれるデバッグ情報を、必要に応じてクラッシュ レポートにシンボル名を付加するために使用できるようにしておくことをお勧めします。

4. **[App Store]** 配布チャネルを選択します。 **[次へ]** をクリックします。

5. **[アップロード]** を宛先として選択します。 **[次へ]** をクリックします。

6. **[プロビジョニング プロファイル]** ウィンドウで、署名 ID、アプリ、およびプロビジョニング プロファイルを選択します。 **[次へ]** をクリックします。

    ![有効な署名 ID、アプリ、プロビジョニング プロファイルの選択を示すプロビジョニング プロファイル ウィザード ページのスクリーンショット。](publishing-to-the-app-store-images/provProfileSelect-mac.png "有効な署名 ID、アプリ、プロビジョニング プロファイルが選択されているプロビジョニング プロファイル ウィザード ページのスクリーンショット。")

7. **[App Store Connect information]\(App Store Connect の情報\)** ウィンドウで、メニューから Apple ID のユーザー名を選択し、[アプリ固有のパスワード](https://support.apple.com/ht204397)を入力します。 **[次へ]** をクリックします。

    ![Apple ID ユーザー名が選択されていることを示す、App Store の接続情報ウィザード ページのスクリーンショット。](publishing-to-the-app-store-images/connectInfo-mac.png "Apple ID ユーザー名が選択されていることを示す、App Store の接続情報ウィザード ページのスクリーンショット。")

8. パッケージの詳細を確認し、 **[発行]** をクリックします。 .ipa ファイルを保存する場所を選択すると、アプリが App Store Connect にアップロードされます。

    > [!NOTE]
    > Apple では、.ipa ファイルに **iTunesMetadata.plist** が含まれるアプリは、次のようなエラーが発生するため、却下する場合があります。
    >
    > `ERROR: ERROR ITMS-90047: "Disallowed paths ( "iTunesMetadata.plist" ) found at: Payload/iPhoneApp1.app"`
    >
    > このエラーの回避策については、[Xamarin フォーラムのこちらの投稿](https://forums.xamarin.com/discussion/40388/disallowed-paths-itunesmetadata-plist-found-at-when-submitting-to-app-store/p1)を参照してください。

# <a name="visual-studio-2019tabwindows"></a>[Visual Studio 2019](#tab/windows)

> [!NOTE]
> App Store への発行は、Visual Studio 2019 バージョン 16.3 以降でサポートされています。

1. Visual Studio 2019 が [Mac ビルド ホストとペアリング](~/ios/get-started/installation/windows/connecting-to-mac/index.md)されていることを確認します。
2. **[ソリューション構成]** ドロップダウンからは **[リリース]** を、 **[ソリューション プラットフォーム]** ドロップダウンからは **[iPhone]** を選択します。

    ![ソリューション構成がリリースに設定され、ソリューション プラットフォームが iPhone に設定され、ターゲットがデバイスに設定されていることを示す、Visual Studio ツール バーのスクリーンショット。](publishing-to-the-app-store-images/chooseConfig-w157.png "ソリューション構成がリリースに設定され、ソリューション プラットフォームが iPhone に設定され、ターゲットがデバイスに設定されていることを示す、Visual Studio ツール バーのスクリーンショット。")

3. **[ビルド]** メニューの **[アーカイブ]** を選択します。これにより、 **[アーカイブ マネージャー]** が開き、アーカイブの作成が開始されます。

4. アーカイブが作成されたら、 **[配布]** をクリックして、発行ウィザードを開きます。

    ![アーカイブ マネージャー ビューの [配布] ボタンの場所のスクリーンショット。](publishing-to-the-app-store-images/archives-win.png "アーカイブ マネージャー ビューの [配布] ボタンの場所のスクリーンショット。")

5. **[App Store]** 配布チャネルを選択します。

6. 署名 ID とプロビジョニング プロファイルを選択します。 **[Upload to Store]\(Store にアップロード\)** をクリックします。

    ![有効な署名 ID とプロビジョニング プロファイルの選択を示す発行ウィザードのスクリーンショット。](publishing-to-the-app-store-images/provProfileSelect-win.png "有効な署名 ID とプロビジョニング プロファイルの選択を示す発行ウィザードのスクリーンショット。")

7. Apple ID と[アプリ固有のパスワード](https://support.apple.com/ht204397)を入力します。 **[OK]** をクリックして、App Store Connect へのアプリのアップロードを開始します。

    ![Apple ID とアプリ固有のパスワードを入力するためのポップアップ ウィンドウのスクリーンショット。](publishing-to-the-app-store-images/connectInfo-win.png "Apple ID とアプリ固有のパスワードを入力するためのポップアップ ウィンドウのスクリーンショット。")

# <a name="visual-studio-2017tabwin-vs2017"></a>[Visual Studio 2017](#tab/win-vs2017)

> [!NOTE]
> Visual Studio 2017 では、Visual Studio for Mac と Visual Studio 2019 にある完全発行のワークフローはサポートしていません。
>
> 次のステップは、Xcode 10 を対象としています。
>
> 引き続き、以下のステップに従って .IPA ファイルを構築できますが、Xcode 11 (iOS 13 のサポートのために必要) を使用して App Store に展開するには、[Visual Studio for Mac を使用する](?tabs=macos#build-and-submit-your-app)必要があります。

1. Visual Studio 2017 が [Mac ビルド ホストとペアリング](~/ios/get-started/installation/windows/connecting-to-mac/index.md)されていることを確認します。
2. Visual Studio 2017 の **[ソリューション構成]** ドロップダウンからは **[リリース]** を、 **[ソリューション プラットフォーム]** ドロップダウンからは **[iPhone]** を選択します。

    ![ビルド構成とプラットフォームの選択](publishing-to-the-app-store-images/chooseConfig-w157.png "ビルド構成とプラットフォームの選択")

3. プロジェクトをビルドします。 これで .ipa ファイルが作成されます。

    > [!NOTE]
    > このドキュメントの「[リリース ビルド構成を更新する](#update-the-release-build-configuration)」セクションでは、各**リリース** ビルドの .ipa ファイルを作成するためのアプリのビルド設定を構成しました。

4. Windows コンピューター上の .ipa ファイルを検索するには、Visual Studio 2019 または Visual Studio 2017 の**ソリューション エクスプローラー**で Xamarin.iOS プロジェクト名を右クリックし、 **[エクスプローラーでフォルダーを開く]** を選択します。 次に、開いた Windows の**エクスプローラー**で、**bin/iPhone/Release** サブディレクトリに移動します。 [.ipa ファイルの出力場所をカスタマイズ](#customize-the-ipa-location)していない限り、このディレクトリにあります。
5. 代わりに Mac ビルド ホスト上の .ipa ファイルを表示するには、Visual Studio 2019 または Visual Studio 2017 の**ソリューション エクスプローラー** (Windows 上) で Xamarin.iOS プロジェクト名を右クリックし、 **[ビルド サーバーに IPA ファイルを表示]** を選択します。 これで、Mac ビルド ホストに、.ipa ファイルが選択された状態で **Finder** ウィンドウが開きます。

    > [!TIP]
    >
    > 次のステップは、Xcode 10 を使用していて、ビルドの対象が iOS 12 以前である場合にのみ有効です。
    >
    > Xcode 11 (iOS 13 用) を使用して App Store に展開するには、[Visual Studio for Mac を使用](?tabs=macos#build-and-submit-your-app)してアプリをビルドし、アップロードする必要があります。 Xcode 11 では**アプリケーション ローダー**を使用できません。

6. Mac ビルド ホストで、**Application Loader** を開きます。 Xcode で、 **[Xcode]、[開発者ツールを開く]、[Application Loader]** の順に選択します。

    > [!NOTE]
    > ツールの詳細については、[Application Loader に関する Apple のドキュメント](https://help.apple.com/itc/apploader/#/apdS673accdb)を参照してください。

7. Application Loader にログインします (Apple ID 用の[アプリ固有のパスワードを作成する](https://support.apple.com/ht204397)必要があることに注意してください)。
8. **[Deliver Your App]\(アプリの配信\)** を選択して、 **[選択]** ボタンをクリックします。

    ![[Deliver Your App]\(アプリの配信\) を選択する](publishing-to-the-app-store-images/publishvs01.png "[Deliver Your App]\(アプリの配信\) を選択する")

9. 前の手順で作成した .ipa ファイルを選択して、 **[OK]** をクリックします。
10. アプリケーション ローダーはファイルを検証します。

    ![検証画面](publishing-to-the-app-store-images/publishvs02.png "検証画面")

11. **[次へ]** ボタンをクリックすると、アプリケーションは App Store に対して検証されます。

    ![App Store に対する検証](publishing-to-the-app-store-images/publishvs03.png "App Store に対する検証")

12. **[送信]** ボタンをクリックして、審査のために Apple にアプリケーションを送信します。
13. アプリケーション ローダーは、ファイルが正常にアップロードされたときに通知します。

    > [!NOTE]
    > Apple では、.ipa ファイルに **iTunesMetadata.plist** が含まれるアプリは、次のようなエラーが発生するため、却下する場合があります。
    >
    > `ERROR: ERROR ITMS-90047: "Disallowed paths ( "iTunesMetadata.plist" ) found at: Payload/iPhoneApp1.app"`
    >
    > このエラーの回避策については、[Xamarin フォーラムのこちらの投稿](https://forums.xamarin.com/discussion/40388/disallowed-paths-itunesmetadata-plist-found-at-when-submitting-to-app-store/p1)を参照してください。

-----

## <a name="itunes-connect-status"></a>iTunes Connect のステータス

アプリの提出の状態を確認するには、iTunes Connect にログインし、ご利用のアプリを選択します。 初期状態は **[審査待ち]** となります。ただし、処理中は一時的に **[Upload Received]\(アップロード受信済み\)** になる場合があります。

![レビューを待機しています](publishing-to-the-app-store-images/image21.png "レビューを待機しています")

## <a name="tips-and-tricks"></a>ヒントとテクニック

### <a name="customize-the-ipa-location"></a>.ipa の場所をカスタマイズする

**MSBuild** プロパティの `IpaPackageDir` では、.ipa ファイルの出力場所をカスタマイズすることができます。 `IpaPackageDir` がカスタムの場所に設定されている場合、タイムスタンプが付いた既定のサブディレクトリではなく、その場所に .ipa ファイルが置かれます。 このような変更は、継続的インテグレーション (CI) ビルドに利用される自動化ビルドのように、正常に動作するために特定のディレクトリ パスに依存する自動化ビルドを作成する際に便利な場合があります。

新しいプロパティはいくつかの方法で使用される可能性があります。 たとえば、(Xamarin.iOS 9.6 以前の) 古い既定のディレクトリに .ipa ファイルを出力する場合は、次のいずれかの方法を使用して、`IpaPackageDir` プロパティを `$(OutputPath)` に設定できます。 いずれの方法も、IDE ビルド、および **msbuild** や **mdtool** を使用するコマンド ライン ビルドを含む、すべての Unified API Xamarin.iOS ビルドと互換性があります。

- 最初のオプションでは、**MSBuild** ファイルの `<PropertyGroup>` 要素内で `IpaPackageDir` プロパティを設定します。 たとえば、次の `<PropertyGroup>` を iOS アプリ プロジェクト .csproj ファイルの一番下 (終了タグ `</Project>` の直前) に追加できます。

    ```xml
    <PropertyGroup>
      <IpaPackageDir>$(OutputPath)</IpaPackageDir>
    </PropertyGroup>
    ```

- もっと良い手法は、.ipa ファイルのビルドに使用される構成に対応する既存の `<PropertyGroup>` の一番下に `<IpaPackageDir>` 要素を追加することです。 この方法が優れているのは、iOS IPA オプション プロジェクト プロパティ ページで予定されている設定との将来的な互換性がプロジェクトに与えられるためです。 現在、`Release|iPhone` 構成を使用して .ipa ファイルをビルドしている場合、更新された完全なプロパティ グループは次のようになります。

    ```xml
    <PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Release|iPhone'">
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
       <MtouchArch>ARMv7, ARM64</MtouchArch>
       <MtouchHttpClientHandler>HttpClientHandler</MtouchHttpClientHandler>
       <MtouchTlsProvider>Default</MtouchTlsProvider>
       <BuildIpa>true</BuildIpa>
       <IpaPackageDir>$(OutputPath)</IpaPackageDir>
    </PropertyGroup>
    ```

コマンド ライン ビルドの **msbuild** のための代替手法は、`/p:` コマンド ライン引数を追加し、`IpaPackageDir` プロパティを設定することです。 代替手法を使う場合、**msbuild** はコマンド ラインに渡される `$()` 式を展開しません。そのため、`$(OutputPath)` 構文は利用できません。 代わりに、完全パス名を指定する必要があります。

```bash
msbuild /p:Configuration="Release" /p:Platform="iPhone" /p:ServerAddress="192.168.1.3" /p:ServerUser="macuser" /p:IpaPackageDir="%USERPROFILE%\Builds" /t:Build SingleViewIphone1.sln
```

Mac の場合は次のようになります。

```bash
msbuild /p:Configuration="Release" /p:Platform="iPhone" /p:IpaPackageDir="$HOME/Builds" /t:Build SingleViewIphone1.sln
```

配布ビルドが作成され、アーカイブされたら、アプリケーションを iTunes Connect に提出できます。

## <a name="summary"></a>まとめ

この記事では、App Store でのリリースのために iOS アプリを構成、ビルド、および提出する方法について説明しました。

## <a name="related-links"></a>関連リンク

- [Apple Developer ポータル (Apple)](https://developer.apple.com/account/)
- [iTunes Connect (Apple)](https://itunesconnect.apple.com)
- [App Store 審査ガイドライン (Apple)](https://developer.apple.com/appstore/resources/approval/guidelines.html)
- [一般的なアプリケーションの却下理由 (Apple)](https://developer.apple.com/app-store/review/rejections/)
- [Xamarin.iOS の機能の使用](~/ios/deploy-test/provisioning/capabilities/index.md)
- [Xamarin.iOS での権利の使用](~/ios/deploy-test/provisioning/entitlements.md)
- [iTunes Connect でのアプリの構成](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [Xamarin.iOS のアプリケーション アイコン](~/ios/app-fundamentals/images-icons/app-icons.md)
- [Xamarin.iOS アプリの起動画面](~/ios/app-fundamentals/images-icons/launch-screens.md)
