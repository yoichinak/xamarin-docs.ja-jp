---
title: Xamarin.iOS アプリケーションのアイコン
description: このドキュメントは、Xamarin.iOS のさまざまなアプリケーションのアイコンを操作する方法を説明します。 アプリケーション アイコンでは、スポット ライトのアイコン、アイコンの設定、および iTunes アートワークです。
ms.prod: xamarin
ms.assetid: B7791574-4A0F-4CB6-8C18-36D40B5C91EB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/22/2017
ms.openlocfilehash: cd67c564461721ade6f3eb269b461ddea5e2d2c4
ms.sourcegitcommit: ffb0f3dbf77b5f244b195618316bbd8964541e42
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/26/2018
ms.locfileid: "39276003"
---
# <a name="application-icons-in-xamarinios"></a>Xamarin.iOS アプリケーションのアイコン

次のトピックで、詳しく説明します。

* [アプリケーションのスポット ライト、設定アイコン](#icon-types)-iOS アプリに必要なアイコンの種類。
* [資産カタログを使用したアイコンを管理する](#managing)- 資産カタログを使用して管理するアプリケーションのアイコン。
* [iTunes アートワーク](#itunes)-アプリケーションを提供する、アドホック メソッドの必要な iTunes アートワークを指定します。

<a name="icon-types" />

## <a name="application-spotlight-and-settings-icons"></a>アプリケーション、スポット ライト、および設定のアイコン

Xamarin.iOS アプリも、UI コントロールとドキュメント アイコンには、イメージの資産を使用できます、同じ方法では、アプリケーション アイコンを提供するイメージの資産を使用できます。 IPad から次のスクリーン ショットは、iOS でのアイコンの 3 つの使用を示しています。

- **アプリケーション アイコン**-すべての iOS アプリは、アプリケーション アイコンを定義する必要があります。 これは、ユーザーがタップするアイコンのアプリを起動する iOS ホーム画面です。 さらに、このアイコンは、該当する場合、Game Center をによって使用されます。 例: 

    [![](app-icons-images/000.png "アプリケーション アイコン")](app-icons-images/000-full.png#lightbox)
- **アイコンのスポット ライト**- ユーザーが、Spotlight 検索で、アプリの名前を入力したときにこのアイコンが表示されます。 例: 

    [![](app-icons-images/000a.png "スポット ライト アイコン")](app-icons-images/000a-full.png#lightbox)
- **設定アイコン**- ユーザーが入力した場合、**設定**の最後にこのアイコン、iOS デバイスでアプリが表示されます、**設定**アプリの一覧。 例: 

    [![](app-icons-images/000b.png "設定アイコン")](app-icons-images/000b-full.png#lightbox)

次のイメージ資産のサイズと解像度は、すべての iOS 9 (またはそれ以上) を通じて iOS 5 を対象とする Xamarin.iOS アプリに必要なアイコンの種類をサポートするために必要になります。

### <a name="iphone-icon-sizes"></a>iPhone アイコンのサイズ

- **iPhone: 9 および 10、iOS (iPhone 6 と 7 に加えて)**

    ||3 x|
    |---|---|
    |アプリケーション アイコン|180x180|
    |スポット ライト|120 x 120|
    |設定|87 x 87|

- **iPhone: iOS 7 および 8**

    ||1 x|2x|
    |---|---|---|
    |アプリケーション アイコン|60 x 60<sup>1</sup>|120 x 120|
    |スポット ライト|40x40<sup>2</sup>|80 x 80|
    |設定|-|-|

- **iPhone: iOS 5、6**

    ||1 x|2x|
    |---|---|---|
    |アプリケーション アイコン|57 x 57|114 x 114|
    |スポット ライト|29 x 29|58 x 58|
    |設定|29 x 29<sup>3、4</sup>|58 x 58<sup>3、4</sup>|

### <a name="ipad-icon-sizes"></a>iPad アイコンのサイズ

- **iPad: iOS 9 および 10**

    ||2 x (iPad Pro)|
    |---|---|
    |アプリケーション アイコン|167x167<sup>6</sup>|
    |スポット ライト|120x120<sup>6</sup>|
    |設定|58x58<sup>5</sup>|

- **iPad: iOS 7 および 8**

    ||1 x|2x|
    |---|---|---|
    |アプリケーション アイコン|76 x 76|152x152|
    |スポット ライト|40 x 40|80 x 80|
    |設定|-|-|

- **iPad: iOS 5、6**

    ||1 x|2x|
    |---|---|---|
    |アプリケーション アイコン|72 x 72|144 x 144|
    |スポット ライト|50 x 50|100x100|
    |設定|29 x 29<sup>3、5</sup>|58 x 58<sup>3、5</sup>|

 1. 両方の Visual Studio for Mac と Xcode は、ios 7 1 x イメージの設定をサポートしません。
 2. 資産カタログを使用する場合、1 x iOS 7 のイメージの設定はサポートされていません。
 3. iOS 7 および 8 では、iOS 5、6 として同じイメージのサイズを使用します。
 4. スポット ライトのアイコンと同じイメージとサイズを使用します。
 5. IPhone と同じサイズのアイコンを使用します。
 6. 資産カタログの画像セットでのみサポートされます。
 
 アイコンの詳細については、Apple を参照してください[アイコンおよび画像のサイズ](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG/IconMatrix.html#//apple_ref/doc/uid/TP40006556-CH27-SW1)ドキュメント。

<a name="managing" />

## <a name="managing-icons-with-asset-catalogs"></a>資産カタログを使用した管理アイコン

アイコン、特別な用`AppIcon`画像セットに追加できる、`Assets.xcassets`アプリのプロジェクト内のファイル。 すべてのバージョンのすべての解像度をサポートするために必要なイメージが含まれている、 _xcasset_グループ化します。 Visual Studio for Mac で特別なエディターは、含めるし、グラフィカルにこれらのイメージをセットアップする開発者を使用できます。

アセット カタログを使用するには、次の操作を行います。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. ダブルクリックして、`Info.plist`ファイル、**ソリューション エクスプ ローラー**編集用に開きます。
2. 下へスクロールして、**アプリ アイコン**セクション。
3. **ソース**ドロップダウン ボックスの一覧を確認してください**AppIcon**が選択されています。 

    ![](app-icons-images/migrate01.png "AppIcon が選択されていることを確認します。")
4. **ソリューション エクスプ ローラー**、ダブルクリック、`Assets.xcassets`ファイルを開き、編集します。 

    ![](app-icons-images/asset01.png "ソリューション エクスプ ローラーで Assets.xcassets ファイル")
5. 選択`AppIcon`を表示する資産の一覧から、 `Icon Editor`:

    ![](app-icons-images/asset02.png "AppIcon エディター")
6. いずれかの指定されたアイコンの種類 をクリックと必要な型/サイズのイメージ ファイルを選択またはイメージのフォルダーからにドラッグ アンド ドロップ、目的のサイズ。
7. をクリックして、**オープン** をクリックして、プロジェクトにイメージを含める、xcasset で設定します。
8. 必要なすべてのイメージを繰り返します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. ダブルクリックして、 **Info.plist**ファイル、**ソリューション エクスプ ローラー**:

    ![](app-icons-images/icon01w.png "Info.plist を選択します。")
2. をクリックして、**のビジュアル アセット** タブでをクリックし、**アセット カタログを使用**下ボタン**アプリ アイコン**: 

    ![](app-icons-images/icon02w.png "ビジュアル資産 タブを選択します。")
4. **ソリューション エクスプ ローラー**、展開、**資産カタログ**フォルダー。 

    ![](app-icons-images/image009.png "資産カタログのフォルダーを展開します。")
5. ダブルクリック、**メディア**ファイルをエディターで開きます。 

    ![](app-icons-images/image010.png "エディターで、メディア ファイルを開く")
6. で、**プロパティ エクスプ ローラー**開発者は、さまざまな種類と必要なアイコンのサイズを選択できます。
7. 指定されたアイコンの種類 をクリックし、必要な型/サイズのイメージ ファイルを選択します。
8. をクリックして、**オープン** をクリックして、プロジェクトにイメージを含める、xcasset で設定します。
9. 必要なすべてのイメージを繰り返します。

-----

これは、アプリのアプリケーション、スポット ライト、設定アイコンを提供するために使用するイメージのアセットの管理などの推奨される方法です。

### <a name="migrating-from-infoplist-to-asset-catalogs"></a>Info.plist から資産カタログへの移行

Xamarin.iOS アプリ使用して、既存の`Info.plist`のアイコンを管理するファイルを開発者に切り替えることが経由で使用を強くお勧め、`AppIcons`内のイメージ資産、`Assets.xcassets`します。

次の手順で行います。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. ダブルクリックして、`Info.plist`ファイル、**ソリューション エクスプ ローラー**編集用に開きます。
2. 下へスクロールして、**アプリ アイコン**セクション。
3. **ソース**ドロップダウン リストで、**資産カタログへの移行**: 

    ![](app-icons-images/migrate02.png "資産カタログへの移行を選択します")
4. 定義されているすべての既存のアイコン、`Info.plist`ファイルに移行されます、`AppIcons`セットのイメージに追加された`Assets.xcassets`: 

     ![](app-icons-images/migrate03.png "Assets.xcassets AppIcons イメージ セット")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. ダブルクリックして、`Info.plist`ファイル、**ソリューション エクスプ ローラー**編集用に開きます。
2. Iphone の場合、「アイコン」セクションををクリックしてします。 

    ![](app-icons-images/image007.png "された iPhone アイコン エディター")
3. 下へスクロールして、**アイコン**セクション。
4. **資産カタログ**ドロップダウン リストで、**資産カタログを使用して**します。
5. 定義されているすべての既存のアイコン、`Info.plist`ファイルに移行されます、`Images`セットに追加`Assets.xcassets`します。
6. 変更内容を `Info.plist` ファイルに保存します。

-----

<a name="itunes" />

## <a name="itunes-artwork"></a>iTunes アートワーク

(または企業のユーザーのため実際のデバイスでベータ テスト) アプリを配信するアドホック メソッドを使用して場合、開発者も含める必要があります、512 x 512 と 1024 x 1024 イメージを iTunes でアプリを表すために使用されます。

iTunes アートワークは次の手順で指定します。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. ダブルクリックして、`Info.plist`ファイル、**ソリューション エクスプ ローラー**編集用に開きます。
2. スクロールして、 **iTunes アートワーク**エディターのセクション。 

    ![](app-icons-images/itunes01.png "ITunes アートワーク エディターの セクションまでスクロールします。")
3. 、不足している任意のイメージ エディターでサムネイルをクリックして、ファイルを開く ダイアログ ボックスから iTunes アートワークのイメージ ファイルを選択およびクリックして、 **OK**ボタンをクリックします。
4. 必要なイメージは、アプリに指定されているすべてまでこの手順を繰り返します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. ダブルクリックして、`Info.plist`ファイル、**ソリューション エクスプ ローラー**編集用に開きます。

2. をクリックして、**のビジュアル アセット**タブし、展開、 **iTunes アートワーク**: 

    ![](app-icons-images/itunes01w.png "ITunes アートワークが Visual Studio での編集")
4. 、不足している任意のイメージ エディターでサムネイルをクリックして、ファイルを開く ダイアログ ボックスから iTunes アートワークのイメージ ファイルを選択およびクリックして、**オープン**ボタンをクリックします。
5. 必要なイメージは、アプリに指定されているすべてまでこの手順を繰り返します。

-----

## <a name="related-links"></a>関連リンク

- [画像 (サンプル) の操作](https://developer.xamarin.com/samples/WorkingWithImages/)
- [Hello, iPhone](~/ios/get-started/hello-ios/index.md)
- [カスタム アイコンとイメージの作成ガイドライン](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html))
