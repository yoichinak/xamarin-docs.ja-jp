---
title: "Android アプリケーション パッケージに署名する"
ms.topic: article
ms.prod: xamarin
ms.assetid: 8E3EFBB2-F8AD-C126-5F32-7FD140791E53
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/26/2018
ms.openlocfilehash: 56d6eea891de3aef8efbe1a971ce3af96ffd8fab
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2018
---
# <a name="signing-the-android-application-package"></a>Android アプリケーション パッケージに署名する

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

このセクションでは、Visual Studio によって提供される APK に署名するための統合された公開ワークフローについて説明します。 「[リリースに向けてアプリケーションを準備する](~/android/deploy-test/release-prep/index.md)」では、**アーカイブ マネージャー**を使用してアプリをビルドし、署名および公開するためにそれをアーカイブに配置しました。 このセクションでは、Android の署名 ID を作成する方法、Android アプリケーション用の新しい署名証明書を作成する方法、アーカイブしたアプリの*アドホック*をディスクに公開する方法について説明します。
結果として得られる APK は、アプリ ストアを経由せずに Android デバイスにサイドロードすることができます。

[公開のためのアーカイブ](~/android/deploy-test/release-prep/index.md#archive)では、**[配布チャネル]** ダイアログに 2 種類の配布方法が表示されていました。 **[アドホック]** を選択します。

[![[配布チャネル] ダイアログ](images/vs/01-distribution-channel-sml.png)](images/vs/01-distribution-channel.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

このセクションでは、Visual Studio for Mac の統合された公開ワークフローを使用して、APK に署名します。 「[リリースに向けてアプリケーションを準備する](~/android/deploy-test/release-prep/index.md)」では、**アーカイブ マネージャー**を使用してアプリをビルドし、署名および公開するためにそれをアーカイブに配置しました。 このセクションでは、Android の署名 ID を作成する方法、Android アプリケーション用の新しい署名証明書を作成する方法、アーカイブしたアプリの*アドホック*をディスクに公開する方法を学習します。 結果として得られる APK は、アプリ ストアを経由せずに Android デバイスにサイドロードすることができます。

[公開のためのアーカイブ](~/android/deploy-test/release-prep/index.md#archive)では、**[署名と配布...]** ダイアログに 2 種類の配布方法が表示されました。 **[アドホック]** を選択して **[次へ]** をクリックします。

[![[署名と配布] ダイアログ](images/xs/01-select-ad-hoc-sml.png)](images/xs/01-select-ad-hoc.png#lightbox)

-----

<a name="newcertvs" />
<a name="newcert" />
<a name="newcertxs" />

## <a name="create-a-new-certificate"></a>新しい証明書を作成する

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

**[アドホック]** を選択すると、次のスクリーンショットに示されているように、Visual Studio でダイアログの **[署名 ID]** ページが開きます。 .APK を公開するには、まず署名キー (証明書とも呼ばれます) で署名する必要があります。

既存の証明書を使用するには、**[インポート]** ボタンをクリックし、[[APK の署名]](#signapkvs) に進みます。 または、**[+]** ボタンをクリックして、新しい証明書を作成します。

[![アドホック署名 ID](images/vs/02-ad-hoc-signing-identity-vs-sml.png)](images/vs/02-ad-hoc-signing-identity-vs.png#lightbox)

**[Create Android Key Store]\(Android キー ストアの作成\)**  ダイアログが表示されます。このダイアログを使用して、Android アプリケーションの署名に使用できる新しい署名証明書を作成します。 このダイアログ ボックスに必要な情報 (赤で表示) を入力します。

[![[Create Android Key Store]\(Android キー ストアの作成\) ダイアログ](images/vs/03-create-android-key-store-vs-sml.png)](images/vs/03-create-android-key-store-vs.png#lightbox)

次の例は、指定する必要がある情報の種類を示しています。 **[作成]** をクリックして、新しい証明書を作成します。

[![新しい証明書の作成](images/vs/04-key-store-example-vs-sml.png)](images/vs/04-key-store-example-vs.png#lightbox)

結果として得られるキーストアは、次の場所に存在します。

**C:\\Users\\*ユーザー名*\\AppData\\Local\\Xamarin\\Mono for Android\\alias\\alias.keystore**

たとえば、上記の手順では、次の場所に新しい署名キーが作成される場合があります。

**C:\\Users\\*ユーザー名*\\AppData\\Local\\Xamarin\\Mono for Android\\chimp\\chimp.keystore**

> [!NOTE]
> 結果として得られるキーストア ファイルは、必ず安全な場所にバックアップを作成しておきます &ndash; これはソリューションには含まれません。 (たとえば、別のコンピューターに移動したり Windows を再インストールしたことが原因で) キーストア ファイルを紛失した場合、以前のバージョンと同じ証明書を使用してアプリに署名できなくなります。

キーストアの詳細については、「[キーストアの MD5 または SHA1 署名の検索](~/android/deploy-test/signing/keystore-signature.md)」を参照してください。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

**[アドホック]** をクリックすると、次のスクリーンショットに示すように、Visual Studio for Mac で **[Android の署名の識別情報]** ダイアログが開きます。 .APK を公開するには、まず署名キー (証明書とも呼ばれます) で署名する必要があります。 証明書が既に存在する場合は、**[既存のキーをインポート]** ボタンをクリックしてインポートし、[[APK の署名]](#signapkxs) に進みます。または、**[キーの新規作成]** ボタンをクリックして新しい証明書を作成します。 

[![[Android の署名の識別情報] ダイアログ](images/xs/02-android-signing-identity-sml.png)](images/xs/02-android-signing-identity.png#lightbox)

**[新しい証明書を作成]** ダイアログは、Android アプリケーションの署名に使用できる新しい署名証明書を作成するために使用されます。 必要な情報を入力したら、**[OK]** をクリックします。

[![[新しい証明書を作成] ダイアログ](images/xs/03-create-new-certificate-sml.png)](images/xs/03-create-new-certificate.png#lightbox)

結果として得られるキーストアは、次の場所に存在します。

**~/Library/Developer/Xamarin/Keystore/alias/alias.keystore**

たとえば、上記の手順では、次の場所に新しい署名キーが作成される場合があります。

**~/Library/Developer/Xamarin/Keystore/chimp/chimp.keystore**


> [!NOTE]
> 結果として得られるキーストア ファイルは、必ず安全な場所にバックアップを作成しておきます &ndash; これはソリューションには含まれません。 (たとえば、別のコンピューターに移動したり Mac を再インストールしたことが原因で) キーストア ファイルを紛失した場合、以前のバージョンと同じ証明書を使用してアプリに署名できなくなります。

キーストアの詳細については、「[キーストアの MD5 または SHA1 署名の検索](~/android/deploy-test/signing/keystore-signature.md)」を参照してください。

-----

<a name="signapkvs" />

## <a name="sign-the-apk"></a>APK に署名する

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

**[作成]** をクリックすると、新しいキー ストア (新しい証明書を含む) が保存され、次のスクリーンショットに示すように、**[署名 ID]** の下に一覧表示されます。 Google Play でアプリを公開するには、**[キャンセル]** をクリックして「[Google Play に公開する](~/android/deploy-test/publishing/publishing-to-google-play/index.md)」に進みます。
*アドホック*を公開するには、署名に使用する署名 ID を選択し、**[名前を付けて保存]** をクリックして独立したディストリビューション用にアプリを公開します。 たとえば、このスクリーンショットでは、(以前に作成した) **chimp** 署名 ID が選択されています。

[![署名 ID の例](images/vs/05-save-as-vs-sml.png)](images/vs/05-save-as-vs.png#lightbox)

次に、**[アーカイブ マネージャー]** に公開の進行状況が表示されます。 公開プロセスが完了すると、**[名前を付けて保存]** ダイアログが開き、生成された .APK ファイルを格納する場所を尋ねられます。

[![[名前を付けて保存] ダイアログ](images/vs/06-save-as-dialog-vs-sml.png)](images/vs/06-save-as-dialog-vs.png#lightbox)

目的の場所に移動し、**[保存]** をクリックします。 キー パスワードが不明の場合、**[署名パスワード]** ダイアログが表示され、選択した証明書のパスワードが求められます。

[![[署名パスワード] ダイアログ](images/vs/07-signing-password-vs-sml.png)](images/vs/07-signing-password-vs.png#lightbox)

署名プロセスが完了したら、**[フォルダーを開く]** をクリックします。

[![[フォルダーを開く] ボタン](images/vs/08-open-folder-vs-sml.png)](images/vs/08-open-folder-vs.png#lightbox)

これにより、Windows エクスプローラーで生成された APK ファイルを含むフォルダーが開きます。 この時点で、Visual Studio は Xamarin.Android アプリケーションを配布可能な APK にコンパイルしています。
次のスクリーンショットは、公開の準備ができたアプリ **MyApp.MyApp.apk** の例を示しています。

[![Windows エクスプローラーに表示された APK](images/vs/09-generated-app-vs-sml.png)](images/vs/09-generated-app-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)


既に説明したように、新しい証明書がキー ストアに追加されました。 Google Play でアプリを公開するには、**[キャンセル]** をクリックして「[Google Play に公開する](~/android/deploy-test/publishing/publishing-to-google-play/index.md)」に進みます。
または、**[次へ]** をクリックして、この例で示すように (独立したディストリビューション用に) アプリの*アドホック*を公開します。

[![[署名と配布] ダイアログ](images/xs/04-select-identity-sml.png)](images/xs/04-select-identity.png#lightbox)

**[アドホックとして公開します]** ダイアログでは、公開前の署名済みアプリの概要を提供します。 この情報が正しければ、**[公開]** をクリックします。

[![[アドホックとして公開します] ダイアログ](images/xs/05-publish-ad-hoc-sml.png)](images/xs/05-publish-ad-hoc.png#lightbox)

**[APK ファイルを出力]** ダイアログは、APK を指定されたパスに保存します。 **[保存]**をクリックします。

![[APK ファイルを出力] ダイアログ](images/xs/06-output-apk-file.png)

次に、証明書のパスワード (**[新しい証明書を作成]** ダイアログで使用したパスワード) を入力し、**[OK]** をクリックします。 

![証明書パスワードの入力](images/xs/07-signing-certificate.png)

APK が証明書で署名され、指定した場所に保存されます。 **[Finder に表示]** をクリックします。

[![[Publication Succeeded]\(公開に成功しました\) ダイアログ](images/xs/08-app-is-ready-sml.png)](images/xs/08-app-is-ready.png#lightbox)

これにより Finder が開き、署名済みの APK ファイルの場所が示されます。

[![Finder に表示された APK](images/xs/09-show-in-finder-sml.png)](images/xs/09-show-in-finder.png#lightbox)

Finder から APK をコピーして最終的な宛先に送信することができます。 配布する前に、Android デバイスに APK をインストールして試すことをお勧めします。 *アドホック* APK の公開に関する詳細は、「[個別公開](~/android/deploy-test/publishing/publishing-independently.md)」を参照してください。

-----



## <a name="next-steps"></a>次の手順

アプリケーション パッケージがリリースの署名が付いたら、それを公開します。 後続のセクションでは、アプリケーションを公開する方法をいくつか説明します。
