---
title: ライブラリのインストール エラーを解決します。
description: 場合によっては、Android のサポート ライブラリをインストール中にエラーが発生する可能性があります。 このガイドでは、一般的なエラーのいくつかの回避策を提供します。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 2AE68ACE-8496-445D-BF17-5E4097D4AE35
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/14/2018
ms.openlocfilehash: a54c69ff708ff7438ef1a8fd14c17e77b5375039
ms.sourcegitcommit: f52aa66de4d07bc00931ac8af791d4c33ee1ea04
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/19/2018
---
# <a name="resolving-library-installation-errors"></a>ライブラリのインストール エラーを解決します。

_場合によっては、Android のサポート ライブラリをインストール中にエラーが発生する可能性があります。このガイドでは、一般的なエラーのいくつかの回避策を提供します。_

## <a name="overview"></a>概要

Xamarin.Android アプリ プロジェクトをビルド中に Visual Studio または Visual Studio for Mac ダウンロードし、依存するライブラリのインストールを試みるときにビルド エラーが発生する可能性があります。 ネットワーク接続の問題、ファイルの破損、またはバージョン管理の問題のこれらのエラーの多くが原因です。 このガイドでは、最も一般的なサポート ライブラリのインストール エラーを説明し、これらの問題を回避し、アプリ プロジェクトをもう一度ビルドを取得する手順を示します。 

 
 
## <a name="errors-while-downloading-m2repository"></a>M2Repository のダウンロード中にエラー

表示される**m2repository** Android のサポート ライブラリや Google Play サービスの NuGet パッケージを参照しているときにエラーです。 エラー メッセージは次のようになります。

```shell
Download failed. Please download https://dl-ssl.google.com/android/repository/android_m2repository_r16.zip and extract it to the C:\Users\mgm\AppData\Local\Xamarin\Android.Support.v4\22.2.1\content directory.
```

この例は、 **android\_m2repository\_r16**など、別のバージョンは、この同じエラー メッセージを参照して可能性がありますが、 **android\_m2repository\_r18**または**android\_m2repository\_r25**です。 



### <a name="automatic-recovery-from-m2repository-errors"></a>M2repository エラーからの自動復旧 

多くの場合、問題のあるライブラリを削除して、次の手順に従って再構築でこの問題を解決することができます。 

1. コンピューターのサポート ライブラリのディレクトリに移動します。

    -   Windows では、サポート ライブラリの場所は**c:\\ユーザー\\_username_\\AppData\\ローカル\\Xamarin**です。 

    -   Mac OS x のサポート ライブラリの場所は**/users/_username_/.local/share/Xamarin**です。 

2. エラー メッセージに対応するライブラリとバージョンのフォルダーを見つけます。 たとえば、上記のエラー メッセージのライブラリとバージョンのフォルダーにある**Android.Support.v4\\22.2.1**:

    [![22.2.1 の例のフォルダーの場所がライブラリをサポートします。](resolving-library-installation-errors-images/01-example-location.png)](resolving-library-installation-errors-images/01-example-location.png#lightbox)

3. バージョンのフォルダーの内容を削除します。 削除してください、 **.zip**ファイルだけでなく**コンテンツ**と**埋め込み**このフォルダー内のサブディレクトリ。 ファイルおよびサブディレクトリこのスクリーン ショットに示すように前に示した例のエラー メッセージを (**コンテンツ**、**埋め込み**、および**android_m2repository_r16.zip**) には削除されます。

    [![ライブラリ フォルダーをサポートして 22.2.1 の例の内容](resolving-library-installation-errors-images/02-example-folder-vs.png)](resolving-library-installation-errors-images/02-example-folder-vs.png#lightbox)

   削除するのには重要であることに注意してください、*全体*このフォルダーの内容。 このフォルダーを含めることがあります最初に、「が見つかりません」 **android\_m2repository\_r16.zip**ファイルでは、このファイルされた可能性がある部分的にダウンロードされたか破損しています。

4. プロジェクトをリビルドします&ndash;欠落したライブラリを再度ダウンロードするビルド プロセスが発生するがします。

ほとんどの場合は、次の手順はビルド エラーを解決して続行を許可します。 このライブラリを削除しても、ビルド エラーが解決しない場合必要があります手動でダウンロードしてインストールする、 **android\_m2repository\_r_nn_.zip**ファイルの次のセクションで説明します。 



### <a name="manually-downloading-m2repository"></a>M2repository を手動でダウンロードします。

手動でダウンロードでく上記の自動復旧手順を使用しようとしましたが、まだビルド エラーがある場合、 **android\_m2repository\_r_nn_.zip** (web ブラウザーを使用して) ファイルおよびに従ってインストール次の手順ではします。 この手順は、開発用コンピューター上のインターネット アクセスがありませんが、別のコンピューターを使用して、アーカイブをダウンロードすることができる場合にも役立ちます。 

1.  ダウンロード、 **android\_m2repository\_r_nn_.zip**エラー メッセージに対応するファイル&ndash;(リンクごとの対応する MD5 ハッシュと共に次の一覧に記載されたリンクURL):

    -   [android\_m2repository\_r33.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r33.zip) &ndash; 5FB756A25962361D17BBE99C3B3FCC44

    -   [android\_m2repository\_r32.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip) &ndash; F16A3455987DBAE5783F058F19F7FCDF

    -   [android\_m2repository\_r31.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r31.zip) &ndash; 99A8907CE2324316E754A95E4C2D786E

    -   [android\_m2repository\_r30.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r30.zip) &ndash; 05AD180B8BDC7C21D6BCB94DDE7F2C8F

    -   [android\_m2repository\_r29.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r29.zip) &ndash; 2A3A8A6D6826EF6CC653030E7D695C41

    -   [android\_m2repository\_r28.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r28.zip) &ndash; 17BE247580748F1EDB72E9F374AA0223

    -   [android\_m2repository\_r27.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r27.zip) &ndash; C9FD4FCD69D7D12B1D9DF076B7BE4E1C

    -   [android\_m2repository\_r26.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r26.zip) &ndash; 8157FC1C311BB36420C1D8992AF54A4D

    -   [android\_m2repository\_r25.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r25.zip) &ndash; 0B3F1796C97C707339FB13AE8507AF50

    -   [android\_m2repository\_r24.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r24.zip) &ndash; 8E3C9EC713781EDFE1EFBC5974136BEA

    -   [android\_m2repository\_r23.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r23.zip) &ndash; D5BB66B3640FD9B9C6362C9DB5AB0FE7

    -   [android\_m2repository\_r22.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r22.zip) &ndash; 96659D653BDE0FAEDB818170891F2BB0

    -   [android\_m2repository\_r21.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r21.zip) &ndash; CD3223F2EFE068A26682B9E9C4B6FBB5

    -   [android\_m2repository\_r20.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r20.zip) &ndash; 650E58DF02DB1A832386FA4A2DE46B1A

    -   [android\_m2repository\_r19.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r19.zip) &ndash; 263B062D6EFAA8AEE39E9460B8A5851A

    -   [android\_m2repository\_r18.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r18.zip) &ndash; 25947AD38DCB4865ABEB61522FAFDA0E

    -   [android\_m2repository\_r17.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r17.zip) &ndash; 49054774F44AE5F35A6BA9D3C117EFD8

    -   [android\_m2repository\_r16.zip](https://dl-ssl.google.com/android/repository/android_m2repository_r16.zip) &ndash; 0595E577D19D31708195A83087881EE6

    場合、 **m2repository**ダウンロード URL を作成するには付加することによってこのテーブルにアーカイブが表示されていない**https://dl-ssl.google.com/android/repository/**の名前に、 **m2repository**をダウンロードします。 たとえば、使用して **https://dl-ssl.google.com/android/repository/android \_m2repository\_r10.zip**をダウンロードする**android\_m2repository\_r10.zip**です。

2.  上記の表に示すように、ダウンロード URL の対応する MD5 ハッシュをファイルの名前を変更します。 たとえば、ダウンロードした**android\_m2repository\_r25.zip**に名前を変更**0B3F1796C97C707339FB13AE8507AF50.zip**です。 使用することができます、ダウンロードしたファイルのダウンロード URL の MD5 ハッシュがテーブルに表示されない場合、[オンライン MD5 ジェネレーター](http://www.webconfs.com/online-md5-generator.php) MD5 ハッシュ文字列を URL に変換します。 

3.  ファイルに、Xamarin コピー**圧縮**フォルダー。 

    -   Windows では、このフォルダーにある**c:\\ユーザー\\***username***\\AppData\\ローカル\\Xamarin\\圧縮**です。 

    -   Mac OS x では、このフォルダーにある**/users/***username***/.local/share/Xamarin/zips**です。 

    たとえば、次のスクリーン ショットは、結果を示しています。 ときに**android\_m2repository\_r16.zip**がダウンロードされ、Windows 上のダウンロード URL の MD5 ハッシュを名前を変更します。

    [![0595E577D19D31708195A83087881EE6.zip に名前が変更される r16.zip リポジトリの例](resolving-library-installation-errors-images/03-md5-rename-vs.png)](resolving-library-installation-errors-images/03-md5-rename-vs.png#lightbox)


手動でダウンロードする必要があるこの手順で、ビルド エラーが解決しない場合、 **android\_m2repository\_r_nn_.zip**ファイル、解凍し、次のセクションで説明するように、その内容をインストールします。 


### <a name="manually-downloading-and-installing-m2repository-files"></a>手動でダウンロードおよび m2repository ファイルをインストールします。

完全に手動のプロセスからの回復を**m2repository**ダウンロード、エラーが必要です、 **android\_m2repository\_r_nn_.zip**ファイル (web ブラウザーを使用)、解凍コンピューターのサポート ライブラリのディレクトリにその内容をコピーします。 次の例では、このエラー メッセージから復旧おします。 

```shell
Unzipping failed. Please download https://dl-ssl.google.com/android/repository/android_m2repository_r25.zip and extract it to the C:\Users\mgm\AppData\Local\Xamarin\Android.Support.v4\23.1.1\content directory.
```

ダウンロードを次の手順を使用して**m2repository**し、その内容をインストールします。

1.  エラー メッセージに対応するライブラリ フォルダーの内容を削除します。 たとえば、上記のエラー メッセージでは内容を削除するの**c:\\ユーザー\\***username***\\AppData\\ローカル\\Xamarin\\Android.Support.v4\\23.1.1.0**です。 
    前述のように、このディレクトリの内容全体を削除する必要があります。

    [![コンテンツを削除するには、埋め込まれていると、23.1.1.0 から android_m2repository フォルダー フォルダー](resolving-library-installation-errors-images/04-delete-contents-vs.png)](resolving-library-installation-errors-images/04-delete-contents-vs.png#lightbox)

2.  ダウンロード、 **android\_m2repository\_r_nn_.zip**エラーに対応する、Google からファイルをメッセージ (リンク前のセクションでテーブルを参照してください)。

3.  この抽出**.zip** (デスクトップ) などの任意の場所にアーカイブします。 これの名前に対応するディレクトリを作成する必要があります、 **.zip**アーカイブします。 このディレクトリ内には、という名前のサブディレクトリを検索する必要があります**m2repository**: 

    [![抽出した zip アーカイブ内に見つかった m2repository フォルダー](resolving-library-installation-errors-images/05-m2repository-vs.png)](resolving-library-installation-errors-images/05-m2repository-vs.png#lightbox)

4.  手順 1. でパージするバージョン管理されたライブラリ ディレクトリに再作成、**コンテンツ**と**埋め込み**サブディレクトリです。 たとえば、次のスクリーン ショットを示しています**コンテンツ**と**埋め込み**サブディレクトリで作成されている、 **23.1.1.0**フォルダー **android\_m2repository\_r25.zip**: 

    [![23.1.1.0 でコンテンツと埋め込みのフォルダーを作成するフォルダー](resolving-library-installation-errors-images/06-recreate-folders-vs.png)](resolving-library-installation-errors-images/06-recreate-folders-vs.png#lightbox)

5.  コピー **m2repository**から抽出した**.zip**に、**コンテンツ**前の手順で作成したディレクトリ。 

    [![23.1.1.0/content フォルダーにコピー m2repository のスクリーン ショット](resolving-library-installation-errors-images/07-copied-m2repository-vs.png)](resolving-library-installation-errors-images/07-copied-m2repository-vs.png#lightbox)

6.  抽出したで**.zip**ディレクトリを参照**m2repository\\com\\android\\サポート\\サポート v4**対応するフォルダーを開きます上記で作成したバージョン番号 (この例では**23.1.1**)。

    [![Support-v4/23.1.1 フォルダーに含まれているファイルの例](resolving-library-installation-errors-images/08-zip-contents-vs.png)](resolving-library-installation-errors-images/08-zip-contents-vs.png#lightbox)

7.  このフォルダー内のすべてのファイルをコピー、**埋め込み**手順 4. で作成されるディレクトリ。

    [![23.1.1.0/embedded フォルダーにコピーされたファイルの例](resolving-library-installation-errors-images/09-copied-vs.png)](resolving-library-installation-errors-images/09-copied-vs.png#lightbox)

8.  上のすべてのファイルがコピーされることを確認します。 **埋め込み**を今すぐファイルを含むディレクトリなど**.jar**、 **.aar**、および**.pom**です。

9.  抽出されたすべてのコンテンツを解凍**.aar**ファイルを**埋め込み**ディレクトリ。 Windows では、追加、 **.zip**拡張機能を**.aar**ファイルを開き、コンテンツのコピー、**埋め込み**ディレクトリ。
    Macos、解凍、 **.aar**ファイルを使用して、**解凍**端末のコマンド (たとえば、 **file.aar を解凍**)。

この時点では、不足しているコンポーネントを手動でインストールしたし、エラーせず、プロジェクトをビルドする必要があります。 以外の場合は、ダウンロードしたことを確認してください、 **m2repository** **.zip**エラー メッセージのバージョンに正確に対応するバージョンをアーカイブし、内容にがインストールされていることを確認してください、。上記の手順」の説明に従って、場所を修正します。 



## <a name="summary"></a>まとめ 

この記事では、自動的にダウンロードおよび依存するライブラリのインストール中に発生する一般的なエラーから回復する方法について説明します。 問題のあるライブラリを削除し、再度ダウンロードし、ライブラリを再インストールする方法として、プロジェクトを再構築する方法を説明します。 ライブラリをダウンロードしにインストールする方法が説明されている、**圧縮**フォルダーです。 手動でダウンロードし、自動の方法で解決できない問題を回避する手段として、必要なファイルをインストールするの複雑な手順についても説明します。 
