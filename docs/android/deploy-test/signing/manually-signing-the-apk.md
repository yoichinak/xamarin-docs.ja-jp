---
title: APK に手動で署名する
ms.prod: xamarin
ms.assetid: 08549E1C-7F04-4D20-9E7A-794B9D09FD12
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 9e1b168b7212f093b50a36c40550fba2e7d63e77
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="manually-signing-the-apk"></a>APK に手動で署名する


アプリケーションがリリースのためにビルドされたら、Android デバイスで実行できるように、配信に先立ち APK に署名する必要があります。 このプロセスは一般的に IDE で処理されます。ただし、コマンド ラインで、手動で APK に署名することが必要な状況があります。 APK 署名には次の手順が含まれます。

1.   **秘密キーを作成する** &ndash; この手順は 1 回だけ実行する必要があります。 秘密キーは APK のデジタル署名に必要です。
    秘密キーを用意したら、今後のリリース ビルドでは、この手順を省略できます。

2.   **APK に zipalign を実行する** &ndash; *zipalign* はアプリケーション上で実行される最適化プロセスです。 実行時に Android が APK とより効率的に対話できるようにします。 Xamarin.Android は実行時にチェックを実行します。APK に zipalign が実行されていない場合、アプリケーションの実行は許可されません。

3.  **APK に署名する** &ndash; この手順では、Android SDK の **apksigner** ユーティリティを利用し、前の手順で作成された秘密キーで APK に署名します。 v24.0.3 より前の古いバージョンの Android SDK ビルド ツールで開発されたアプリケーションは JDK の **jarsigner** アプリを使用します。 これらのツールについては、以下で詳しく説明します。 

順序は重要です。APK の署名に利用されるツールによって変わります。 **apksigner** を使用するときは、先にアプリケーションに **zipalign** を実行し、それから **apksigner** で署名することが重要です。  **jarsigner** を使用して APK に署名する必要がある場合、先に APK に署名し、それから **zipalign** を実行することが重要です。 



## <a name="prerequisites"></a>必須コンポーネント

このガイドでは、Android SDK ビルド ツール v24.0.3 以降の **apksigner** を利用します。 APK が既にビルドされていると想定しています。

以前のバージョンの Android SDK ビルド ツールでビルドされたアプリケーションの場合、以下の「[jarsigner で APK に署名する](#Sign_the_APK_with_jarsigner)」の説明どおりに **jarsigner** を使用する必要があります。



## <a name="create-a-private-keystore"></a>プライベート キーストアを作成する

*キーストア*は、Java SDK のプログラム [keytool](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/keytool.html) を利用して作成されるセキュリティ証明書のデータベースです。 キーストアは Xamarin.Android アプリケーションを公開するために非常に重要です。Android では、デジタル署名のないアプリケーションは実行されません。

開発中、Xamarin.Android はデバッグ キーストアを利用し、アプリケーションに署名します。アプリケーションをエミュレーターまたはデバッグ可能アプリケーションを使用するように構成されたデバイスに直接、配布できます。
ただし、このキーストアは、アプリケーションの配信目的では有効なキーストアとして認識されません。

そのような理由から、プライベート キーストアを作成し、アプリケーションの署名に利用する必要があります。 これは 1 回のみ実行する手順です。同じキーが更新プログラムの公開に利用されます。また、他のアプリケーションの署名に利用できます。

このキーストアを保護することが重要です。 失った場合、Google Play でアプリケーションの更新プログラムを公開できなくなります。
失われたキーストアで引き起こされた問題を解決する唯一の策は、新しいキーストアを作成し、新しいキーで APK にもう一度署名し、新しいアプリケーションを提出することです。 古いアプリケーションは Google Play から削除する必要があります。 同様に、新しいキーストアが危険にさらされたり、公に配信されたりした場合、アプリケーションの非公式バージョンや悪意のあるバージョンが配布される可能性があります。



### <a name="create-a-new-keystore"></a>キーストアの新規作成

新しいキーストアを作成するには、Java SDK のコマンド ラインツール [keytool](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/keytool.html) が必要です。 次のスニペットは、**keytool** の使用方法例です (`<my-filename>` をキーストアのファイル名に、`<key-name>` をキーストア内のキーの名前に変更します)。

```shell
$ keytool -genkeypair -v -keystore <filename>.keystore -alias <key-name> -keyalg RSA \
          -keysize 2048 -validity 10000
```

**keytool** は最初にキーストアのパスワードを尋ねます。 次に、キーの作成に使われるいくつかの情報について尋ねます。 次のスニペットは、ファイル `xample.keystore` に保存される `publishingdoc` という名前の新しいキーを作成した例です。

```shell
$ keytool -genkeypair -v -keystore xample.keystore -alias publishingdoc -keyalg RSA -keysize 2048 -validity 10000
Enter keystore password:
Re-enter new password:
What is your first and last name?
  [Unknown]:  Ham Chimpanze
What is the name of your organizational unit?
  [Unknown]:  NASA
What is the name of your organization?
  [Unknown]:  NASA
What is the name of your City or Locality?
  [Unknown]:  Cape Canaveral
What is the name of your State or Province?
  [Unknown]:  Florida
What is the two-letter country code for this unit?
  [Unknown]:  US
Is CN=Ham Chimpanze, OU=NASA, O=NASA, L=Cape Canaveral, ST=Florida, C=US correct?
  [no]:  yes

Generating 2,048 bit RSA key pair and self-signed certificate (SHA1withRSA) with a validity of 10,000 days
        for: CN=Ham Chimpanze, OU=NASA, O=NASA, L=Cape Canaveral, ST=Florida, C=US
Enter key password for <publishingdoc>
        (RETURN if same as keystore password):
Re-enter new password:
[Storing xample.keystore]
```

キーストアに保存されているキーを一覧表示するには、&ndash; `list` オプションを指定して **keytool** を使用します。

```shell
$ keytool -list -keystore xample.keystore
```


## <a name="zipalign-the-apk"></a>APK に zipalign を実行する

**apksigner** で APK に署名する前に、Android SDK の **zipalign** ツールでファイルを最適化することが重要です。 **zipalign** は 4 バイト境界に沿って APK でリソースを再構築します。 この配置では、Android は APK からリソースをすばやくロードできます。アプリケーションのパフォーマンスが上がるほか、メモリの使用率が減る可能性もあります。 Xamarin.Android はランタイム チェックを実行し、APK に zipalign が実行されているかどうかを判断します。 APK に zipalign が実行されていない場合、アプリケーションは実行されません。

次のコマンドでは、署名された APK を利用し、署名され、zipalign が実行された **helloworld.apk** という名前の APK を生成します。この APK はすぐに配信できます。

```shell
$ zipalign -f -v 4 mono.samples.helloworld-unsigned.apk helloworld.apk
```


## <a name="sign-the-apk"></a>APK に署名する

APK に zipalign を実行したら、キーストアで署名する必要があります。 これは、このバージョンの SDK ビルド ツールの **build-tools** ディレクトリにある **apksigner** ツールで行われます。  たとえば、Android SDK ビルド ツール v25.0.3 がインストールされている場合、**apksigner** はこのディレクトリにあります。

```bash
$ ls $ANDROID_HOME/build-tools/25.0.3/apksigner
/Users/tom/android-sdk-macosx/build-tools/25.0.3/apksigner*
```

次のスニペットでは、**apksigner** に `PATH` 環境変数でアクセスできるものと想定しています。 ファイル **xample.keystore** に含まれるキー エイリアス `publishingdoc` で APK に署名されます。

```shell
$ apksigner sign --ks xample.keystore --ks-key-alias publishingdoc mono.samples.helloworld.apk
```

このコマンドが実行されると、必要な場合、**apksigner** はキーストアのパスワードを尋ねます。

**apksigner** の使用方法については、[Google の文書](https://developer.android.com/studio/command-line/apksigner.html)を参照してください。

> [!NOTE]
> [Google 問題 62696222](https://issuetracker.google.com/issues/62696222) によると、**apksigner** は Android SDK から "消えています"。 これを回避する方法は、Android SDK ビルド ツール v25.0.3 をインストールし、そのバージョンの **apksigner** を使用することです。  


<a name="Sign_the_APK_with_jarsigner" />

### <a name="sign-the-apk-with-jarsigner"></a>jarsigner で APK に署名する

> [!WARNING]
> このセクションは、**jarsigner** ユーティリティで APK に署名する必要がある場合にのみ適用されます。 開発者には、**apksigner** を利用して APK に署名することが推奨されています。

この手法では、Java SDK の **[jarsigner](https://docs.oracle.com/javase/8/docs/technotes/tools/windows/jarsigner.html)** コマンドを利用して APK ファイルに署名します。  **jarsigner** ツールは Java SDK が提供します。 

以下は、**jarsigner** と、「**xample.keystore**」という名前のキーストア ファイルに含まれているキー `publishingdoc` を利用して APK に署名する方法です。

```shell
$ jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore xample.keystore mono.samples.helloworld.apk publishingdoc
```

> [!NOTE]
> **jarsigner** を利用するとき、_先に_ APK に署名し、それから **zipalign** を使用することが重要です。  



## <a name="related-links"></a>関連リンク

- [アプリケーション署名](https://source.android.com/security/apksigning/)
- [Java JAR 署名](https://docs.oracle.com/javase/8/docs/technotes~/jar/jar.html#Signed_JAR_File)
- [jarsigner](https://docs.oracle.com/javase/8/docs/technotes/tools/windows/jarsigner.html)
- [keytool](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/keytool.html)
- [zipalign](https://developer.android.com/studio/command-line/zipalign.html)
- [ビルド ツール 26.0.0 - apksigner はどこに行った?](https://issuetracker.google.com/issues/62696222)
