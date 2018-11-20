---
title: Xamarin.iOS アプリの iTunesMetadata.plist ファイル
description: この記事では、テストまたはエンタープライズ展開のためのアドホックな配布を使って iOS アプリケーションに関する情報を iTunes に提供するために使われる iTunesMetadata.plist ファイルについて説明します。
ms.prod: xamarin
ms.assetid: 70676eba-6a99-4a3a-bccc-84359fe9c2c3
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
ms.openlocfilehash: c03815776921a61c1f54136e3f09c0996dff71d3
ms.sourcegitcommit: 849bf6d1c67df943482ebf3c80c456a48eda1e21
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2018
ms.locfileid: "51528417"
---
# <a name="the-itunesmetadataplist-file-in-xamarinios-apps"></a>Xamarin.iOS アプリの iTunesMetadata.plist ファイル

_この記事では、テストまたはエンタープライズ展開のためのアドホックな配布を使って iOS アプリケーションに関する情報を iTunes に提供するために使われる iTunesMetadata.plist ファイルについて説明します。_

iTunes App Store での販売または無料リリースのために iOS アプリケーションを iTunes Connect で作成するとき、開発者はアプリケーションのジャンル、サブジャンル、著作権の通知、サポートされている iOS デバイス、必要なデバイス機能などの情報を指定できます。 アドホック配布によってテスト担当者またはエンタープライズ ユーザーに配信される iOS アプリケーションの場合は、この情報はありません。

不足している情報をアドホック配布に提供するには、省略可能な `iTunesMetadata.plist` ファイルを作成して、アプリケーションの IPA ファイルに含めることができます。 この plist ファイルは特殊な形式の XML ファイルであり (詳しくは Apple の「[Property List Programming Guide](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/PropertyLists/Introduction/Introduction.html)」(プロパティ リスト プログラミング ガイド) をご覧ください)、特定の iOS アプリケーションに関する情報を定義するキー/値ペアが含まれます。

<a name="iTunesMetadata_contents" />

## <a name="the-itunesmetadataplist-contents"></a>iTunesMetadata.plist の内容

アドホック配布用の iTunes 情報の定義に使われる一般的な `iTunesMetadata.plist` ファイルの例を次に示します。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>UIRequiredDeviceCapabilities</key>
    <dict>
        <key>armv7</key>
        <true/>
        <key>front-facing-camera</key>
        <true/>
    </dict>
    <key>artistName</key>
    <string>Company, Inc.</string>
    <key>bundleDisplayName</key>
    <string>App Name</string>
    <key>bundleShortVersionString</key>
    <string>1.5.1</string>
    <key>bundleVersion</key>
    <string>1.5.1</string>
    <key>copyright</key>
    <string>© 2015 Company, Inc.</string>
    <key>drmVersionNumber</key>
    <integer>0</integer>
    <key>fileExtension</key>
    <string>.app</string>
    <key>gameCenterEnabled</key>
    <false/>
    <key>gameCenterEverEnabled</key>
    <false/>
    <key>genre</key>
    <string>Games</string>
    <key>genreId</key>
    <integer>6014</integer>
    <key>itemName</key>
    <string>App Name</string>
    <key>kind</key>
    <string>software</string>
    <key>playlistArtistName</key>
    <string>Company, Inc.</string>
    <key>playlistName</key>
    <string>App Name</string>
    <key>releaseDate</key>
    <string>2015-11-18T03:23:10Z</string>
    <key>s</key>
    <integer>143441</integer>
    <key>softwareIconNeedsShine</key>
    <false/>
    <key>softwareSupportedDeviceIds</key>
    <array>
        <integer>9</integer>
    </array>
    <key>softwareVersionBundleId</key>
    <string>com.company.appid</string>
    <key>subgenres</key>
    <array>
        <dict>
            <key>genre</key>
            <string>Puzzle</string>
            <key>genreId</key>
            <integer>7012</integer>
        </dict>
        <dict>
            <key>genre</key>
            <string>Word</string>
            <key>genreId</key>
            <integer>7019</integer>
        </dict>
    </array>
    <key>versionRestrictions</key>
    <integer>16843008</integer>
</dict>
</plist>

```

個々のキーの値については後で詳しく説明します。

### <a name="uirequireddevicecapabilities"></a>UIRequiredDeviceCapabilities

`UIRequiredDeviceCapabilities` キーにより、iTunes は、特定の iOS デバイスにインストールする前に、iOS アプリケーションに必要なデバイス固有機能を認識できます。 機能 (`<key>...</key>`) の辞書 (`<dict>...</dict>`) と各機能のブール値として提供されます。 機能の値が `true` の場合、その機能は存在する必要があります。 `false` の場合、その機能はデバイスに存在することはできません。 例:

```xml
<key>UIRequiredDeviceCapabilities</key>
<dict>
    <key>armv7</key>
    <true/>
    <key>front-facing-camera</key>
    <true/>
</dict>
```
このアプリケーションをデバイスにインストールする前に、iOS デバイスが ARM7 命令をサポートし、前面カメラを持っている必要があることを指定します。 指定できる値の完全な一覧については、Apple のドキュメントで [UIRequiredDeviceCapabilities](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW3) をご覧ください。

### <a name="artistname-and-playlistartistname"></a>artistName、playlistArtistName

`artistName` および `playlistArtistName` キーは、iTunes に表示される iOS アプリケーション作成会社の名前を定義するために使います。 例:

```xml
<key>artistName</key>
<string>Company, Inc.</string>
...
<key>playlistArtistName</key>
<string>Company, Inc.</string>
```

### <a name="bundledisplayname-itemname-and-playlistname"></a>bundleDisplayName、itemName、playlistName

`bundleDisplayName`、`itemName`、`playlistName` キーは、iTunes 内に表示される iOS アプリケーションの名前を定義するために使います。 例:

```xml
<key>bundleDisplayName</key>
<string>App Name</string>
...
<key>itemName</key>
<string>App Name</string>
...
<key>playlistName</key>
<string>App Name</string>
```

### <a name="bundleshortversionstring-and-bundleversion"></a>bundleShortVersionString、bundleVersion

`bundleShortVersionString` および `bundleVersion` キーは、iTunes に表示される iOS アプリケーションのバージョン番号を定義するために使います。 例:

```xml
<key>bundleShortVersionString</key>
<string>1.5.1</string>
<key>bundleVersion</key>
<string>1.5.1</string>
```

### <a name="softwareversionbundleid"></a>softwareVersionBundleId

`softwareVersionBundleId` キーは、iOS アプリケーションのバンドル ID を指定するために使います。 例:

```xml
<key>softwareVersionBundleId</key>
<string>com.company.appid</string>
```

### <a name="copyright"></a>著作権

`copyright` キーは、iTunes に表示される著作権の告知を定義するために使います。 例:

```xml
<key>copyright</key>
<string>© 2015 Company, Inc.</string>
```

### <a name="releasedate"></a>releaseDate

`releaseDate` キーは、iTunes に表示される iOS アプリケーションのリリース日を指定するために使います。 例:

```xml
<key>releaseDate</key>
<string>2015-11-18T03:23:10Z</string>
```

### <a name="softwareiconneedsshine"></a>softwareIconNeedsShine

`softwareIconNeedsShine` キーは、iOS 6 (およびそれより前) の場合に iOS アプリケーションのアイコンを "_明るく強調する_" 必要があるかどうかを iTunes に伝えるために使います。 例:

```xml
<key>softwareIconNeedsShine</key>
<false/>
```

### <a name="gamecenterenabled-and-gamecentereverenabled"></a>gameCenterEnabled、gameCenterEverEnabled

`gameCenterEnabled` および `gameCenterEverEnabled` キーは、この iOS アプリケーションが Apple の Game Center をサポートしているかどうかを iTunes に伝えるために使います。 例:

```xml
<key>gameCenterEnabled</key>
<false/>
<key>gameCenterEverEnabled</key>
<false/>
```

### <a name="genre-genreid-and-subgenres"></a>genre、genreId、subgenres

`genre` および `genreId` キーは、iOS アプリケーションのジャンルを iTunes に伝えるために使います。 例:

```xml
<key>genre</key>
<string>Games</string>
<key>genreId</key>
<integer>6014</integer>
```

必要に応じて、`subgenres` キーを使って iOS アプリケーションのサブジャンルを最大 2 個まで追加定義できます。 例:

```xml
<key>subgenres</key>
<array>
    <dict>
        <key>genre</key>
        <string>Puzzle</string>
        <key>genreId</key>
        <integer>7012</integer>
    </dict>
    <dict>
        <key>genre</key>
        <string>Word</string>
        <key>genreId</key>
        <integer>7019</integer>
    </dict>
</array>
```

iOS アプリケーションの場合、現在定義されているジャンルとジャンル ID は次のとおりです。

[!include[](~/ios/includes/table-appstore.md)]

詳しくは、Apple の「[Genre IDs Appendix](http://www.apple.com/itunes/affiliates/resources/documentation/genre-mapping.html)」(ジャンル ID 付録) ドキュメントをご覧ください。

### <a name="softwaresupporteddeviceids"></a>softwareSupportedDeviceIds

`softwareSupportedDeviceIds` キーは、この iOS アプリケーションがサポートしている iOS デバイスを iTunes に伝えるために使います。 例:

```xml
<key>softwareSupportedDeviceIds</key>
<array>
    <integer>9</integer>
</array>
```

有効な値は次のとおりです。

- 1 – クラシック iPhone
- 2 – iPod Touch
- 4 – iPad
- 9 – 最新の iPhone

### <a name="standard-keys"></a>標準キー

以下のキーは、iOS アプリケーションのすべての `iTunesMetadata.plist` ファイルに含まれ、常に同じ値に設定されます。

```xml
<key>drmVersionNumber</key>
<integer>0</integer>
<key>fileExtension</key>
<string>.app</string>
...
<key>kind</key>
<string>software</string>
...
<key>s</key>
<integer>143441</integer>
...
<key>versionRestrictions</key>
<integer>16843008</integer>
```

<a name="iTunesMetadata_creating" />

## <a name="creating-an-itunesmetadataplist-file"></a>iTunesMetadata.plist ファイルの作成

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

 Visual Studio for Mac で `iTunesMetadata.plist` ファイルを操作する場合には、次の 2 つのオプションがあります。

- Visual Studio for Mac のビジュアル plist エディターを使ってファイルを作成および保守します。
- プレーン テキスト エディターでファイルを作成および保守します。

 以下では両方のオプションについて詳しく説明します。

### <a name="using-the-visual-plist-editor"></a>ビジュアル plist エディターを使う

次の手順で行います。

1. **ソリューション エクスプローラー**で Xamarin.iOS プロジェクト ファイルを右クリックし、**[追加]** > **[新しいファイル...]** の順に選びます。
2. [新しいファイル] ダイアログで、**[iOS]** > **[プロパティ一覧]** の順に選びます。

    ![](itunesmetadata-images/image01.png "[IOS] の [プロパティ一覧] を選択します")
3. **[名前]** に「`iTunesMetadata`」と入力し、**[新規]** ボタンをクリックします。
4. **ソリューション エクスプローラー**で `iTunesMetadata.plist` ファイルをダブルクリックして、編集用に開きます。

    ![](itunesmetadata-images/image02.png "iTunesMetadata.plist エディター")
5. 緑色の **[+]** をクリックして新しいエントリを作成し、キー名として「`UIRequiredDeviceCapabilities`」と入力します。

    ![](itunesmetadata-images/image03.png "新しいエントリを作成し、キー名として UIRequiredDeviceCapabilities を入力します")
6. 値の種類として **[文字列]** をクリックし、ポップアップ リストから **[辞書]** を選びます。

    ![](itunesmetadata-images/image04.png "ポップアップ リストから [辞書] を選択します")
7. プロパティの名前の左側にある折り返しをクリックして、辞書のエントリを表示します。

    ![](itunesmetadata-images/image05.png "辞書のエントリを表示します")
8. **[新しいエントリの追加]** をクリックし、緑色の **[+]** をクリックして辞書にエントリを追加します。

    ![](itunesmetadata-images/image06.png "辞書にエントリを追加します")
9. キーの名前に「`armv7`」と入力し、種類として **[ブール値]** を選び、値として「**Yes**」を入力します。

    ![](itunesmetadata-images/image07.png "キーの名前に「armv7」と入力し、種類として [ブール値] を選び、値として「Yes」を入力します")
10. 上記の手順を繰り返して、必要なすべてのキー/値ペアを `iTunesMetadata.plist` ファイルに設定します (詳しくは、前の「[iTunesMetadata.plist の内容](#iTunesMetadata_contents)」をご覧ください)。

11. 変更内容を plist ファイルに保存します。

### <a name="using-a-plain-text-editor"></a>プレーン テキスト エディターを使う

次の手順で行います。

1. プレーン テキスト エディターで新しいテキスト ファイルを作成し、`iTunesMetadata.plist` という名前にします。
2. 前の「[iTunesMetadata.plist の内容](#iTunesMetadata_contents)」セクションから内容の例をコピーします。
3. ファイルに内容を貼り付け、必要に応じて編集します。
4. ファイルを保存し、Visual Studio for Mac に戻ります。
5. **ソリューション エクスプローラー**で Xamarin.iOS プロジェクト ファイルを右クリックし、**[追加]** > **[既存のファイル...]** の順に選びます。
6. [ファイルを開く] ダイアログで、上で作成した `iTunesMetadata.plist` ファイルを選び、**[OK]** ボタンをクリックします。
7. このファイルの **[ビルド アクション]** は **[なし]** のままにします。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio の Xamarin プラグインは `Info.plist` および `Entitlement.plist` ファイルのビジュアル エディターのみをサポートするので、`iTunesMetadata.plist` ファイルは標準のテキスト エディターで作成し、Xamarin.iOS プロジェクトに手動で含める必要があります。

次の手順で行います。

1. プレーン テキスト エディターで新しいテキスト ファイルを作成し、`iTunesMetadata.plist` という名前にします。
2. 前の「[iTunesMetadata.plist の内容](#iTunesMetadata_contents)」セクションから内容の例をコピーします。
3. ファイルに内容を貼り付け、必要に応じて編集します。
4. ファイルを保存し、Visual Studio に戻ります。
5. **ソリューション エクスプローラー**で Xamarin.iOS プロジェクト ファイルを右クリックし、**[追加]** > **[既存のファイル...]** の順に選びます。
6. [ファイルを開く] ダイアログで、上で作成した `iTunesMetadata.plist` ファイルを選び、**[開く]** ボタンをクリックします。
7. このファイルの **[ビルド アクション]** は **[なし]** のままにします。

-----

その後、IDE で IPA のビルドを準備するときに、この `iTunesMetadata.plist` ファイルを選びます。

## <a name="summary"></a>まとめ

この記事では、アドホックに配信される iOS アプリケーションについて iTunes に伝えるために使うことができる `iTunesMetadata.plist` ファイルについて説明しました。 plist ファイルの標準キーについて説明し、Visual Studio と Visual Studio for Mac. でファイルを作成して保守する方法について説明しました。

## <a name="related-links"></a>関連リンク

- [App Store の配布](~/ios/deploy-test/app-distribution/app-store-distribution/index.md)
- [iTunes Connect でのアプリの構成](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [App Store への発行](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)
- [社内配布](~/ios/deploy-test/app-distribution/in-house-distribution.md)
- [アドホック配布](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)
- [IPA のサポート](~/ios/deploy-test/app-distribution/ipa-support.md)
- [トラブルシューティング](~/ios/deploy-test/troubleshooting.md)
