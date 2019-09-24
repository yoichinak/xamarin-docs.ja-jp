---
title: ライブラリ インストール エラーの解決
description: 場合によっては、Android サポートライブラリのインストール中にエラーが発生することがあります。 このガイドでは、いくつかの一般的なエラーの回避策について説明します。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 2AE68ACE-8496-445D-BF17-5E4097D4AE35
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/14/2018
ms.openlocfilehash: 8107a26e090aa920d71146d5f2af8b8365697d6b
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70757213"
---
# <a name="resolving-library-installation-errors"></a>ライブラリ インストール エラーの解決

_場合によっては、Android サポートライブラリのインストール中にエラーが発生することがあります。このガイドでは、いくつかの一般的なエラーの回避策について説明します。_

## <a name="overview"></a>概要

Xamarin Android アプリプロジェクトをビルドするときに、Visual Studio または Visual Studio for Mac が依存関係ライブラリをダウンロードしてインストールしようとすると、ビルドエラーが発生することがあります。 これらのエラーの多くは、ネットワーク接続の問題、ファイルの破損、またはバージョン管理の問題によって発生します。 このガイドでは、サポートライブラリのインストールに関する最も一般的なエラーについて説明し、これらの問題を回避してアプリプロジェクトを再度ビルドするための手順を示します。 

## <a name="errors-while-downloading-m2repository"></a>M2Repository のダウンロード中にエラーが発生しました

Android サポートライブラリまたは Google Play サービスの NuGet パッケージを参照すると、 **m2repository**エラーが表示される場合があります。 エラー メッセージは次のようになります。

```shell
Download failed. Please download https://dl-ssl.google.com/android/repository/android_m2repository_r16.zip and extract it to the C:\Users\mgm\AppData\Local\Xamarin\Android.Support.v4\22.2.1\content directory.
```

この例は **\_android m2repository\_r16**を対象としていますが、 **\_android m2repository\_r18**や**android\_などの別のバージョンでも同じエラーメッセージが表示される場合があります。m2repository\_r25**。 

### <a name="automatic-recovery-from-m2repository-errors"></a>M2repository エラーからの自動復旧 

多くの場合、この問題は、次の手順に従って問題のあるライブラリを削除して再構築することで解決できます。 

1. コンピューターのサポートライブラリのディレクトリに移動します。

    - Windows では、サポートライブラリは **\\C: Users\\ユーザー_名_\\AppData\\ローカル\\Xamarin**にあります。 

    - Mac OS X では、サポートライブラリは **/_ユーザー/ユーザー名_/.local/share/Xamarin**にあります。 

2. エラーメッセージに対応するライブラリとバージョンフォルダーを見つけます。 たとえば、上記のエラーメッセージのライブラリとバージョンフォルダーは、 **\\22.2.1**にあります。

    [![22.2.1 サポートライブラリのフォルダーの場所の例](resolving-library-installation-errors-images/01-example-location.png)](resolving-library-installation-errors-images/01-example-location.png#lightbox)

3. バージョンフォルダーの内容を削除します。 このフォルダー内の**コンテンツ**と**埋め込み**サブディレクトリだけでなく、 **.zip**ファイルも必ず削除してください。 上記のエラーメッセージの例では、このスクリーンショットに示されているファイルとサブディレクトリ (**コンテンツ**、**埋め込み**、および**android_m2repository_r16**) は削除されます。

    [![22.2.1 サポートライブラリフォルダーの内容の例](resolving-library-installation-errors-images/02-example-folder-vs.png)](resolving-library-installation-errors-images/02-example-folder-vs.png#lightbox)

   このフォルダーの内容*全体*を削除することが重要です。 このフォルダーには、最初に "見つかりませ**ん\_"\_android m2repository r16**ファイルが含まれている場合がありますが、このファイルは部分的にダウンロードされているか、破損している可能性があります。

4. プロジェクト&ndash;をリビルドすると、ビルドプロセスによって欠落しているライブラリが再ダウンロードされます。

ほとんどの場合、これらの手順でビルドエラーが解決され、続行することができます。 このライブラリを削除してもビルドエラーが解決しない場合は、次のセクションで説明されているように、 **\_android m2repository\_r_nn_** ファイルを手動でダウンロードしてインストールする必要があります。 

### <a name="manually-downloading-m2repository"></a>M2repository を手動でダウンロードする

上記の自動回復手順を試してもビルドエラーが発生する場合は、(web ブラウザーを使用して) **\_android m2repository\_r_nn_** ファイルを手動でダウンロードし、次の手順に従ってインストールすることができます. この手順は、開発用コンピューターでインターネットにアクセスできないが、別のコンピューターを使用してアーカイブをダウンロードできる場合にも役立ちます。 

1. 次の一覧に、エラーメッセージ&ndash;のリンクに対応する**android\_m2repository\_r_nn_** ファイル (各リンクの URL の対応する MD5 ハッシュ) をダウンロードします。

    - [android\_m2repository\_r33](https://dl-ssl.google.com/android/repository/android_m2repository_r33.zip) 5FB756A25962361D17BBE99C3B3FCC44&ndash;

    - [android\_m2repository\_r32](https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip) F16A3455987DBAE5783F058F19F7FCDF&ndash;

    - [android\_m2repository\_r31](https://dl-ssl.google.com/android/repository/android_m2repository_r31.zip) 99A8907CE2324316E754A95E4C2D786E&ndash;

    - [android\_m2repository\_r30](https://dl-ssl.google.com/android/repository/android_m2repository_r30.zip) 05AD180B8BDC7C21D6BCB94DDE7F2C8F&ndash;

    - [android\_m2repository\_r29](https://dl-ssl.google.com/android/repository/android_m2repository_r29.zip) 2A3A8A6D6826EF6CC653030E7D695C41&ndash;

    - [android\_m2repository\_r28.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r28.zip) &ndash; 17BE247580748F1EDB72E9F374AA0223

    - [android\_m2repository\_r27](https://dl-ssl.google.com/android/repository/android_m2repository_r27.zip) C9FD4FCD69D7D12B1D9DF076B7BE4E1C&ndash;

    - [android\_m2repository\_](https://dl-ssl.google.com/android/repository/android_m2repository_r26.zip) r26&ndash; 8157fc1c311bb36420c1d8992af54a4d

    - [android\_m2repository\_r25](https://dl-ssl.google.com/android/repository/android_m2repository_r25.zip) 0B3F1796C97C707339FB13AE8507AF50&ndash;

    - [android\_m2repository\_](https://dl-ssl.google.com/android/repository/android_m2repository_r24.zip) 8E3C9EC713781EDFE1EFBC5974136BEA&ndash;

    - [android\_m2repository\_r23](https://dl-ssl.google.com/android/repository/android_m2repository_r23.zip) D5BB66B3640FD9B9C6362C9DB5AB0FE7&ndash;

    - [android\_m2repository\_](https://dl-ssl.google.com/android/repository/android_m2repository_r22.zip) 96659D653BDE0FAEDB818170891F2BB0&ndash;

    - [android\_m2repository\_r21](https://dl-ssl.google.com/android/repository/android_m2repository_r21.zip) CD3223F2EFE068A26682B9E9C4B6FBB5&ndash;

    - [android\_m2repository\_r20](https://dl-ssl.google.com/android/repository/android_m2repository_r20.zip) 650E58DF02DB1A832386FA4A2DE46B1A&ndash;

    - [android\_m2repository\_r19.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r19.zip) &ndash; 263B062D6EFAA8AEE39E9460B8A5851A

    - [android\_m2repository\_r18](https://dl-ssl.google.com/android/repository/android_m2repository_r18.zip) 25947AD38DCB4865ABEB61522FAFDA0E&ndash;

    - [android\_m2repository\_r17](https://dl-ssl.google.com/android/repository/android_m2repository_r17.zip) 49054774F44AE5F35A6BA9D3C117EFD8&ndash;

    - [android\_m2repository\_](https://dl-ssl.google.com/android/repository/android_m2repository_r16.zip) r16&ndash; 0595e577d19d31708195a8308788 1 ee6

    **M2repository**アーカイブがこの表に示されていない場合は、ダウンロードする**m2repository**の名前 **https://dl-ssl.google.com/android/repository/** を前に付加してダウンロード URL を作成できます。 たとえば、使用して **https://dl-ssl.google.com/android/repository/android\_m2repository\_r10.zip** をダウンロードする **android\_ m2repository\_ r10.zip** します。

2. 上記の表に示されているように、ファイルの名前をダウンロード URL の対応する MD5 ハッシュに変更します。 たとえば、 **android\_m2repository\_r25**をダウンロードした場合は、名前を**0B3F1796C97C707339FB13AE8507AF50**に変更します。 ダウンロードしたファイルのダウンロード URL の MD5 ハッシュが表に示されていない場合は、[オンライン md5 ジェネレーター](http://www.webconfs.com/online-md5-generator.php)を使用して URL を md5 ハッシュ文字列に変換できます。 

3. ファイルを Xamarin **zip**フォルダーにコピーします。 

    - Windows では、このフォルダーは **\\C: Users\\***ユーザー 名***\\AppData\\ローカル\\Xamarin\\zip**にあります。 

    - Mac OS X では、このフォルダーは **//.local/share/Xamarin/zips**にあります。 

    たとえば、次のスクリーンショットは、 **\_android m2repository\_r16**がダウンロードされ、Windows のダウンロード URL の MD5 ハッシュに名前が変更された場合の結果を示しています。

    [![R16 リポジトリの名前を 0595E577D19D31708195A8308788 1EE6 .zip に変更する例](resolving-library-installation-errors-images/03-md5-rename-vs.png)](resolving-library-installation-errors-images/03-md5-rename-vs.png#lightbox)

この手順でビルドエラーが解決されない場合は、次のセクションで説明するように、 **\_android m2repository\_r_nn_** ファイルを手動でダウンロードして解凍し、その内容をインストールする必要があります。 

### <a name="manually-downloading-and-installing-m2repository-files"></a>M2repository ファイルを手動でダウンロードしてインストールする

**M2repository**エラーから復旧するための完全な手動プロセスでは、(web ブラウザーを使用して) **\_android m2repository\_r_nn_** ファイルをダウンロードし、解凍して、サポートにコンテンツをコピーする必要があります。コンピューター上のライブラリディレクトリ。 次の例では、このエラーメッセージから回復します。 

```shell
Unzipping failed. Please download https://dl-ssl.google.com/android/repository/android_m2repository_r25.zip and extract it to the C:\Users\mgm\AppData\Local\Xamarin\Android.Support.v4\23.1.1\content directory.
```

**M2repository**をダウンロードし、その内容をインストールするには、次の手順を実行します。

1. エラーメッセージに対応するライブラリフォルダーの内容を削除します。 たとえば、上記のエラーメッセージでは、 **C:\\Users\\***ユーザー 名***\\AppData\\ローカル\\Xamarin\\Android. Support. v4の内容を削除します。23.1.1.0\\** 。 
    前述のように、このディレクトリの内容全体を削除する必要があります。

    [![23.1.1.0 フォルダーからのコンテンツ、埋め込み、および android_m2repository フォルダーの削除](resolving-library-installation-errors-images/04-delete-contents-vs.png)](resolving-library-installation-errors-images/04-delete-contents-vs.png#lightbox)

2. エラーメッセージに対応する Google から **\_android m2repository\_r_nn_** ファイルをダウンロードします (リンクについては、前のセクションの表を参照してください)。

3. この **.zip**アーカイブを任意の場所 (デスクトップなど) に抽出します。 これにより、 **.zip**アーカイブの名前に対応するディレクトリが作成されます。 このディレクトリ内に、 **m2repository**という名前のサブディレクトリがあります。 

    [![抽出された zip アーカイブに m2repository フォルダーが見つかりました](resolving-library-installation-errors-images/05-m2repository-vs.png)](resolving-library-installation-errors-images/05-m2repository-vs.png#lightbox)

4. 手順 1. で削除したバージョン管理されたライブラリディレクトリで、**コンテンツ**と**埋め込ま**れたサブディレクトリを再作成します。 たとえば、次のスクリーンショットは、 **android\_m2repository\_r25**の**23.1.1.0**フォルダーに作成される**コンテンツ**と**埋め込み**サブディレクトリを示しています。 

    [![23.1.1.0 フォルダーにコンテンツと埋め込みフォルダーを作成する](resolving-library-installation-errors-images/06-recreate-folders-vs.png)](resolving-library-installation-errors-images/06-recreate-folders-vs.png#lightbox)

5. 抽出した **.zip**から、前の手順で作成した**コンテンツ**ディレクトリに**m2repository**をコピーします。 

    [![23.1.1.0/content フォルダーにコピーされた m2repository のスクリーンショット](resolving-library-installation-errors-images/07-copied-m2repository-vs.png)](resolving-library-installation-errors-images/07-copied-m2repository-vs.png#lightbox)

6. 抽出した **.zip**ディレクトリで**m2repository\\com\\android\\support\\support-v4**を参照し、上で作成したバージョン番号に対応するフォルダーを開きます (この例では、**23.1.1**):

    [![サポート-v4/23.1.1 フォルダーに格納されているファイルの一覧の例](resolving-library-installation-errors-images/08-zip-contents-vs.png)](resolving-library-installation-errors-images/08-zip-contents-vs.png#lightbox)

7. このフォルダー内のすべてのファイルを、手順4で作成した**埋め込み**ディレクトリにコピーします。

    [![23.1.1.0/embedded フォルダーにコピーされたファイルの例](resolving-library-installation-errors-images/09-copied-vs.png)](resolving-library-installation-errors-images/09-copied-vs.png#lightbox)

8. すべてのファイルがコピーされていることを確認します。 **埋め込み**ディレクトリには、 **.jar**、 **aar**、および**pom**などのファイルが含まれるようになります。

9. **Aar**ファイルの内容を**埋め込み**ディレクトリに解凍します。 Windows の場合は、 **aar**ファイルに **.zip**拡張子を追加して開き、内容を**埋め込み**ディレクトリにコピーします。
    MacOS で、ターミナルの**unzip**コマンドを使用して**aar**ファイルを解凍します (たとえば、 **aar を解凍**します)。

この時点で、不足しているコンポーネントが手動でインストールされ、プロジェクトはエラーなしでビルドされます。 そうでない場合は、エラーメッセージのバージョンと正確に一致する**m2repository** **アーカイブバージョン**をダウンロードしたことを確認し、上記の手順の説明に従って、内容が正しい場所にインストールされていることを確認します。 

## <a name="summary"></a>Summary 

この記事では、依存関係ライブラリの自動ダウンロードおよびインストール中に発生する可能性のある一般的なエラーから回復する方法について説明しました。 この記事では、問題のあるライブラリを削除し、ライブラリを再ダウンロードして再インストールする方法として、プロジェクトをリビルドする方法を説明しています。 この記事では、ライブラリをダウンロードし、 **zip**フォルダーにインストールする方法について説明しています。 また、自動的な方法で解決できない問題に対処する方法として、必要なファイルを手動でダウンロードしてインストールする手順についても説明しました。 
