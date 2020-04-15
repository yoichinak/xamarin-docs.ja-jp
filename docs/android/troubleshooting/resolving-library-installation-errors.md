---
title: ライブラリ インストール エラーの解決
description: 場合によっては、Android サポート ライブラリのインストール中にエラーが発生することがあります。 このガイドでは、いくつかの一般的なエラーの回避策について説明します。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 2AE68ACE-8496-445D-BF17-5E4097D4AE35
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/14/2018
ms.openlocfilehash: e99e12aa73374cc8145fad0eb5aad7f3259e0ca2
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "76724300"
---
# <a name="resolving-library-installation-errors"></a>ライブラリ インストール エラーの解決

"_場合によっては、Android サポート ライブラリのインストール中にエラーが発生することがあります。このガイドでは、いくつかの一般的なエラーの回避策について説明します。_ "

## <a name="overview"></a>概要

Xamarin.Android アプリ プロジェクトをビルドするときに、Visual Studio または Visual Studio for Mac が依存関係ライブラリをダウンロードしてインストールしようとすると、ビルド エラーが発生することがあります。 これらのエラーの多くは、ネットワーク接続の問題、ファイルの破損、またはバージョン管理の問題が原因です。 このガイドでは、サポート ライブラリのインストールに関する最も一般的なエラーについて説明し、これらの問題を回避してアプリ プロジェクトを再度ビルドするための手順を示します。

## <a name="errors-while-downloading-m2repository"></a>m2Repository のダウンロード中のエラー

Android サポート ライブラリまたは Google Play サービスの NuGet パッケージを参照すると、**m2repository** エラーが表示される場合があります。 エラー メッセージは次のようになります。

```shell
Download failed. Please download https://dl-ssl.google.com/android/repository/android_m2repository_r16.zip and extract it to the C:\Users\mgm\AppData\Local\Xamarin\Android.Support.v4\22.2.1\content directory.
```

この例は **android\_m2repository\_r16** を対象としていますが、**android\_m2repository\_r18** や **android\_m2repository\_r25** などの別のバージョンでも同じエラー メッセージが表示される場合があります。

### <a name="automatic-recovery-from-m2repository-errors"></a>m2repository エラーからの自動復旧

多くの場合、この問題は、次の手順に従って問題のあるライブラリを削除してリビルドすることで解決できます。

1. お使いのコンピューターのサポート ライブラリ ディレクトリに移動します。

    - Windows では、サポート ライブラリは **C:\\Users\\_ユーザー名_\\AppData\\Local\\Xamarin** にあります。

    - Mac OS X では、サポート ライブラリは、 **/Users/_ユーザー名_/.local/share/Xamarin** にあります。

2. エラー メッセージに対応するライブラリとバージョン フォルダーを見つけます。 たとえば、上記のエラー メッセージのライブラリとバージョン フォルダーは **Android.Support.v4\\22.2.1** にあります。

    [![22.2.1 サポート ライブラリ フォルダーの場所の例](resolving-library-installation-errors-images/01-example-location.png)](resolving-library-installation-errors-images/01-example-location.png#lightbox)

3. そのバージョン フォルダーの内容を削除します。 このフォルダー内の **.zip** ファイル、および **content** と **embedded** サブディレクトリを必ず削除してください。 上に示したエラー メッセージの例では、このスクリーンショットに示されているファイルとサブディレクトリ (**content**、**embedded**、および **android_m2repository_r16.zip**) を削除します。

    [![22.2.1 サポート ライブラリ フォルダーの内容の例](resolving-library-installation-errors-images/02-example-folder-vs.png)](resolving-library-installation-errors-images/02-example-folder-vs.png#lightbox)

   このフォルダーのコンテンツ*全体*を削除することが重要です。 このフォルダーには、"欠落した" **android\_m2repository\_r16.zip** ファイルが最初に含まれる場合がありますが、このファイルは部分的にダウンロードされているか、破損している可能性があります。

4. プロジェクトをリビルドします。&ndash;これにより、ビルド プロセスによって、欠落しているライブラリが再ダウンロードされます。

ほとんどの場合、これらの手順でビルド エラーが解決され、続行できるようになります。 このライブラリを削除してもビルド エラーが解決しない場合は、次のセクションの説明に従って、**android\_m2repository\_r_nn_.zip** ファイルを手動でダウンロードしてインストールする必要があります。

### <a name="manually-downloading-m2repository"></a>m2repository の手動ダウンロード

上記の自動復旧手順を試してもビルド エラーが発生する場合は、次の手順に従って、(Web ブラウザーを使用して) **android\_m2repository\_r_nn_.zip** ファイル を手動でダウンロードしてインストールできます。 この手順は、開発用コンピューターでインターネットにアクセスできないが、別のコンピューターを使用してアーカイブをダウンロードできる場合にも役立ちます。

1. エラー メッセージに対応する **android\_m2repository\_r_nn_.zip** ファイルをダウンロードします  &ndash;  リンクは、次の一覧に (各リンクの URL の対応する MD5 ハッシュとともに) 示されています。

    - [android\_m2repository\_r33.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r33.zip) &ndash; 5FB756A25962361D17BBE99C3B3FCC44

    - [android\_m2repository\_r32.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip) &ndash; F16A3455987DBAE5783F058F19F7FCDF

    - [android\_m2repository\_r31.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r31.zip) &ndash; 99A8907CE2324316E754A95E4C2D786E

    - [android\_m2repository\_r30.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r30.zip) &ndash; 05AD180B8BDC7C21D6BCB94DDE7F2C8F

    - [android\_m2repository\_r29.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r29.zip) &ndash; 2A3A8A6D6826EF6CC653030E7D695C41

    - [android\_m2repository\_r28.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r28.zip) &ndash; 17BE247580748F1EDB72E9F374AA0223

    - [android\_m2repository\_r27.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r27.zip) &ndash; C9FD4FCD69D7D12B1D9DF076B7BE4E1C

    - [android\_m2repository\_r26.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r26.zip) &ndash; 8157FC1C311BB36420C1D8992AF54A4D

    - [android\_m2repository\_r25.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r25.zip) &ndash; 0B3F1796C97C707339FB13AE8507AF50

    - [android\_m2repository\_r24.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r24.zip) &ndash; 8E3C9EC713781EDFE1EFBC5974136BEA

    - [android\_m2repository\_r23.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r23.zip) &ndash; D5BB66B3640FD9B9C6362C9DB5AB0FE7

    - [android\_m2repository\_r22.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r22.zip) &ndash; 96659D653BDE0FAEDB818170891F2BB0

    - [android\_m2repository\_r21.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r21.zip) &ndash; CD3223F2EFE068A26682B9E9C4B6FBB5

    - [android\_m2repository\_r20.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r20.zip) &ndash; 650E58DF02DB1A832386FA4A2DE46B1A

    - [android\_m2repository\_r19.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r19.zip) &ndash; 263B062D6EFAA8AEE39E9460B8A5851A

    - [android\_m2repository\_r18.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r18.zip) &ndash; 25947AD38DCB4865ABEB61522FAFDA0E

    - [android\_m2repository\_r17.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r17.zip) &ndash; 49054774F44AE5F35A6BA9D3C117EFD8

    - [android\_m2repository\_r16.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r16.zip) &ndash; 0595E577D19D31708195A83087881EE6

    **m2repository** アーカイブがこの表に示されていない場合は、ダウンロードする **m2repository** の名前の先頭に `https://dl-ssl.google.com/android/repository/` を付けることで、ダウンロード URL を作成できます。 たとえば、 **https://dl-ssl.google.com/android/repository/android\_m2repository\_r10.zip** を使用して **android\_ m2repository\_ r10.zip** をダウンロードします。

2. 上記の表に示されているように、ファイルの名前をダウンロード URL の対応する MD5 ハッシュに変更します。 たとえば、**android\_m2repository\_r25.zip** をダウンロードした場合は、その名前を **0B3F1796C97C707339FB13AE8507AF50.zip** に変更します。 ダウンロードしたファイルのダウンロード URL の MD5 ハッシュが表に示されていない場合は、[オンライン MD5 ジェネレーター](http://www.webconfs.com/online-md5-generator.php)を使用して、URL を MD5 ハッシュ文字列に変換できます。

3. そのファイルを Xamarin **zips** フォルダーにコピーします。

    - Windows では、このフォルダーは **C:\\Users\\***ユーザー名***\\AppData\\Local\\Xamarin\\zips** にあります。

    - Mac OS X では、このフォルダーは **/Users/***ユーザー名***/.local/share/Xamarin/zips** にあります。

    たとえば、次のスクリーンショットは、**android\_m2repository\_r16.zip** がダウンロードされ、Windows 上でそのダウンロード URL の MD5 ハッシュに名前が変更された場合の結果を示しています。

    [![r16.zip リポジトリの名前を 0595E577D19D31708195A83087881EE6.zip に変更する例](resolving-library-installation-errors-images/03-md5-rename-vs.png)](resolving-library-installation-errors-images/03-md5-rename-vs.png#lightbox)

この手順でビルド エラーが解決しない場合は、次のセクションの説明に従って、**android\_m2repository\_r_nn_.zip** ファイルを手動でダウンロードして解凍し、その内容をインストールする必要があります。

### <a name="manually-downloading-and-installing-m2repository-files"></a>m2repository ファイルの手動ダウンロードとインストール

**m2repository** エラーから復旧するための完全な手動プロセスでは、(Web ブラウザーを使用して) **android\_m2repository\_r_nn_.zip** ファイルをダウンロードして解凍し、その内容をお使いのコンピューターのサポート ライブラリ ディレクトリにコピーする必要があります。 次の例では、このエラー メッセージから復旧します。

```shell
Unzipping failed. Please download https://dl-ssl.google.com/android/repository/android_m2repository_r25.zip and extract it to the C:\Users\mgm\AppData\Local\Xamarin\Android.Support.v4\23.1.1\content directory.
```

次の手順に従って **m2repository** をダウンロードし、その内容をインストールします。

1. エラー メッセージに対応するライブラリ フォルダーの内容を削除します。 たとえば、上記のエラー メッセージでは、**C:\\Users\\***ユーザー名***\\AppData\\Local\\Xamarin\\Android.Support.v4\\23.1.1.0** の内容を削除します。
    前述のように、このディレクトリの内容全体を削除する必要があります。

    [![23.1.1.0 フォルダーからの content、embedded、および android_m2repository フォルダーの削除](resolving-library-installation-errors-images/04-delete-contents-vs.png)](resolving-library-installation-errors-images/04-delete-contents-vs.png#lightbox)

2. エラー メッセージに対応する **android\_m2repository\_r_nn_.zip** ファイルを Google からダウンロードします (リンクについては、前のセクションの表を参照してください)。

3. この **.zip** アーカイブを任意の場所 (デスクトップなど) に展開します。 これにより、 **.zip** アーカイブの名前に対応するディレクトリが作成されます。 このディレクトリ内に、**m2repository** という名前のサブディレクトリがあります。

    [![展開された zip アーカイブで見つかった m2repository フォルダー](resolving-library-installation-errors-images/05-m2repository-vs.png)](resolving-library-installation-errors-images/05-m2repository-vs.png#lightbox)

4. 手順 1 で消去した、バージョン管理されたライブラリ ディレクトリで、**content** および **embedded** サブディレクトリを再作成します。 たとえば、次のスクリーンショットは、**android\_m2repository\_r25.zip** の **23.1.1.0** フォルダーに **content** および **embedded** サブディレクトリが作成されていることを示しています。

    [![23.1.1.0 フォルダーに content および embedded フォルダーを作成する](resolving-library-installation-errors-images/06-recreate-folders-vs.png)](resolving-library-installation-errors-images/06-recreate-folders-vs.png#lightbox)

5. **m2repository** を、展開した **.zip** から、前の手順で作成した **content** ディレクトリにコピーします。

    [![23.1.1.0/content フォルダーにコピーされた m2repository のスクリーンショット](resolving-library-installation-errors-images/07-copied-m2repository-vs.png)](resolving-library-installation-errors-images/07-copied-m2repository-vs.png#lightbox)

6. 展開した **.zip** ディレクトリで、**m2repository\\com\\android\\support\\support-v4** を参照し、上で作成したバージョン番号に対応するフォルダーを開きます (この例では、**23.1.1**)。

    [![support-v4/23.1.1 フォルダーに含まれるファイルの一覧の例](resolving-library-installation-errors-images/08-zip-contents-vs.png)](resolving-library-installation-errors-images/08-zip-contents-vs.png#lightbox)

7. このフォルダー内のすべてのファイルを、手順 4 で作成した **embedded** ディレクトリにコピーします。

    [![23.1.1.0/embedded フォルダーにコピーされたファイルの例](resolving-library-installation-errors-images/09-copied-vs.png)](resolving-library-installation-errors-images/09-copied-vs.png#lightbox)

8. すべてのファイルがコピーされていることを確認します。 **embedded** ディレクトリには、 **.jar**、 **. aar**、 **. pom** などのファイルが含まれているはずです。

9. 展開した **.aar** ファイルの内容を **embedded** ディレクトリに解凍します。 Windows の場合は、 **.aar** ファイルに **.zip** 拡張子を追加して開き、その内容を **embedded** ディレクトリにコピーします。
    macOS の場合は、ターミナルで **unzip** コマンドを使用して **.aar** ファイルを解凍します (たとえば、**unzip file.aar**)。

この時点で、欠落しているコンポーネントが手動でインストールされているので、プロジェクトはエラーなしでビルドされます。 そうでない場合は、エラー メッセージ内のバージョンと正確に対応する **m2repository** **.zip** アーカイブ バージョンをダウンロードしていることと、その内容を、上記の手順で説明されている正しい場所にインストールしていることを確認してください。

## <a name="summary"></a>まとめ

この記事では、依存関係ライブラリの自動ダウンロードおよびインストール中に発生する可能性のある一般的なエラーから復旧する方法を説明しました。 ライブラリを再ダウンロードして再インストールする方法として、問題のあるライブラリを削除し、プロジェクトをリビルドする方法について説明しました。
ライブラリをダウンロードし、それを **zips** フォルダーにインストールする方法について説明しました。 また、自動的な方法で解決できない問題に対処する方法として、必要なファイルを手動でダウンロードしてインストールする、より複雑な手順についても説明しました。
