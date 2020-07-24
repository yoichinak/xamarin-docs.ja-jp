---
title: Xamarin. iOS のアプリケーションアイコン
description: 'このドキュメントでは、Xamarin でさまざまなアプリケーションアイコンを操作する方法について説明します。 iOS: アプリケーションアイコン自体、スポットライトアイコン、設定アイコン、iTunes アートワーク。'
ms.prod: xamarin
ms.assetid: B7791574-4A0F-4CB6-8C18-36D40B5C91EB
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 05/22/2017
ms.openlocfilehash: 65e42e824a888934fd0f0a01093a6549dcf3d99d
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/23/2020
ms.locfileid: "86997463"
---
# <a name="application-icons-in-xamarinios"></a>Xamarin. iOS のアプリケーションアイコン

次のトピックで、詳しく説明します。

- [[アプリケーション]、[スポットライト]、および [設定] アイコン](#icon-types)-iOS アプリに必要なさまざまな種類のアイコン。
- [資産カタログ](#managing)を使用したアイコンの管理-資産カタログを使用したアプリケーションアイコンの管理。
- [Itunes アートワーク](#itunes)-アプリケーションを提供するために必要な Itunes アートワークを提供します。

<a name="icon-types"></a>

## <a name="application-spotlight-and-settings-icons"></a>[アプリケーション]、[スポットライト]、および [設定] アイコン

Xamarin iOS アプリで UI コントロールやドキュメントアイコンとしてイメージアセットを使用するのと同じ方法で、イメージアセットを使用してアプリケーションアイコンを提供できます。 IPad の次のスクリーンショットは、iOS でのアイコンの3つの使用方法を示しています。

- **アプリケーションアイコン**-すべての iOS アプリで、アプリケーションアイコンを定義する必要があります。 これは、アプリを起動するためにユーザーが iOS ホーム画面からタップするアイコンです。 また、このアイコンは Game Center によって使用されます (該当する場合)。 例:

    [![アプリケーションアイコン](app-icons-images/000.png)](app-icons-images/000-full.png#lightbox)
- **スポットライトアイコン**-ユーザーがスポットライト検索でアプリの名前を入力するたびに、このアイコンが表示されます。 例:

    [![スポットライトアイコン](app-icons-images/000a.png)](app-icons-images/000a-full.png#lightbox)
- **設定アイコン**-ユーザーが iOS デバイスで**設定**アプリを入力すると、アプリの**設定**リストの最後にこのアイコンが表示されます。 例:

    [![設定アイコン](app-icons-images/000b.png)](app-icons-images/000b-full.png#lightbox)

次の画像のサイズと解像度は、ios 5 から iOS 9 (またはそれ以降) を対象とする Xamarin ios アプリで必要なすべてのアイコンの種類をサポートするために必要になります。

### <a name="iphone-icon-sizes"></a>iPhone アイコンのサイズ

- **iPhone: iOS 9 & 10 (iPhone 6 & 7 Plus)**

    |アイコン|3x|
    |---|---|
    |アプリケーション アイコン|180x180|
    |Spotlight|120 x 120|
    |設定|87x87|

- **iPhone: iOS 7 & 8**

    |アイコン|1x|2x|
    |---|---|---|
    |アプリケーション アイコン|いずれか<sup>1</sup>|120 x 120|
    |Spotlight|40 x 40<sup>2</sup>|80 x 80|
    |設定|-|-|

- **iPhone: iOS 5 & 6**

    |アイコン|1x|2x|
    |---|---|---|
    |アプリケーション アイコン|57 x 57|114x114|
    |Spotlight|29 x 29|58 x 58|
    |設定|29x29<sup>3、4</sup>|58 x 58<sup>3、4</sup>|

### <a name="ipad-icon-sizes"></a>iPad アイコンのサイズ

- **iPad: iOS 9 & 10**

    |アイコン|2x (iPad Pro)|
    |---|---|
    |アプリケーション アイコン|167x167<sup>6</sup>|
    |Spotlight|120x120<sup>6</sup>|
    |設定|58 x 58<sup>5</sup>|

- **iPad: iOS 7 & 8**

    |アイコン|1x|2x|
    |---|---|---|
    |アプリケーション アイコン|76 x 76|は152x152|
    |Spotlight|40 x 40|80 x 80|
    |設定|-|-|

- **iPad: iOS 5 & 6**

    |アイコン|1x|2x|
    |---|---|---|
    |アプリケーション アイコン|72 x 72|144x144|
    |Spotlight|50 x 50|100x100|
    |設定|29x29<sup>3、5</sup>|58 x 58<sup>3、5</sup>|

 1. Visual Studio for Mac と Xcode の両方で、iOS 7 の1x イメージの設定はサポートされなくなりました。
 2. 資産カタログを使用する場合、iOS 7 用の1x イメージの設定はサポートされていません。
 3. iOS 7 & 8 では、iOS 5 & 6 と同じイメージサイズを使用します。
 4. では、スポットライトアイコンと同じイメージとサイズが使用されます。
 5. では、iPhone と同じサイズのアイコンが使用されます。
 6. アセットカタログのイメージセットでのみサポートされます。

 アイコンの詳細については、Apple の[アイコンとイメージのサイズ](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG/IconMatrix.html#//apple_ref/doc/uid/TP40006556-CH27-SW1)に関するドキュメントを参照してください。

<a name="managing"></a>

## <a name="managing-icons-with-asset-catalogs"></a>資産カタログを使用したアイコンの管理

アイコンの場合は、 `AppIcon` アプリのプロジェクト内のファイルに特殊なイメージセットを追加でき `Assets.xcassets` ます。 すべての解像度をサポートするために必要なイメージのすべてのバージョンは、 _xcasset_に含まれ、グループ化されています。 Visual Studio for Mac の特別なエディターを使用すると、開発者はこれらのイメージをグラフィカルに追加して設定できます。

アセットカタログを使用するには、次の手順を実行します。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

1. ソリューションエクスプローラー内のファイルをダブルクリックし `Info.plist` て、編集用に**Solution Explorer**開きます。
2. [ **IPhone のアイコン**] セクションまで下にスクロールします。
3. [**資産カタログに移行する**] ボタンをクリックします。

    ![AppIcon が選択されていることを確認する](app-icons-images/migrate01.png)

4. **ソリューションエクスプローラー**で、ファイルをダブルクリックし `Assets.xcassets` て開き、編集します。

    ![ソリューションエクスプローラー内の Assets. xcassets ファイル](app-icons-images/asset01.png)

5. `AppIcon`アセットの一覧から選択すると、次のものが表示され `Icon Editor` ます。

    ![AppIcon エディター](app-icons-images/asset02.png)

6. [指定されたアイコン] をクリックして、必要な種類/サイズのイメージファイルを選択するか、フォルダーからイメージをドラッグして目的のサイズにドロップします。
7. [**開く**] ボタンをクリックして、プロジェクトに画像を含め、xcasset に設定します。
8. 必要なすべてのイメージについて、この手順を繰り返します。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. * * [情報] をダブルクリックします。  * ***ソリューションエクスプローラー**内のファイル:

    ![[インフォ] を選択します。](app-icons-images/icon01w.png)

2. [**ビジュアルアセット**] タブをクリックし、[**アプリアイコン**] の [**アセットカタログを使用**] ボタンをクリックします。

    ![[ビジュアルアセット] タブを選択します。](app-icons-images/icon02w.png)

    ボタンがなく、ドロップダウンリストが表示されている場合は、アセットカタログが既にこのプロジェクトに追加されています。

3. **ソリューションエクスプローラー**から、[**資産カタログ**] フォルダーを展開します。

    ![[資産カタログ] フォルダーを展開します。](app-icons-images/image009.png)

4. **メディア**ファイルをダブルクリックして、エディターで開きます。

    ![エディターでメディアファイルを開きます。](app-icons-images/image010.png)

5. 開発者は、**プロパティエクスプローラー**で、必要なさまざまな種類やサイズのアイコンを選択できます。
6. [指定されたアイコンの種類] をクリックし、必要な種類/サイズのイメージファイルを選択します。
7. [**開く**] ボタンをクリックして、プロジェクトに画像を含め、xcasset に設定します。
8. 必要なすべてのイメージについて、この手順を繰り返します。

-----

これは、アプリのアプリケーション、スポットライト、および設定のアイコンを提供するために使用されるイメージアセットを含めて管理するための推奨される方法です。

<a name="itunes"></a>

## <a name="itunes-artwork"></a>iTunes アートワーク

アプリを配信するアドホック方式 (企業ユーザー向けまたは実際のデバイスでのベータテスト用) を使用する場合、開発者は、iTunes でアプリを表すために使用される512x512 と1024x1024 イメージも含める必要があります。

iTunes アートワークは次の手順で指定します。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

1. ソリューションエクスプローラー内のファイルをダブルクリックし `Info.plist` て、編集用に**Solution Explorer**開きます。
2. エディターの**ITunes アートワーク**セクションまでスクロールします。

    ![エディターの iTunes アートワークセクションまでスクロールします。](app-icons-images/itunes01.png)
3. イメージが見つからない場合は、エディターでサムネイルをクリックし、[ファイルを開く] ダイアログボックスで目的の iTunes アートワークのイメージファイルを選択して、[ **OK** ] ボタンをクリックします。
4. アプリに必要なすべてのイメージが指定されるまで、この手順を繰り返します。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. ソリューションエクスプローラー内のファイルをダブルクリックし `Info.plist` て、編集用に**Solution Explorer**開きます。

2. [**ビジュアルアセット**] タブをクリックし、 **iTunes アートワーク**を展開します。

    ![Visual Studio での iTunes アートワークの編集](app-icons-images/itunes01w.png)
3. イメージが見つからない場合は、エディターでサムネイルをクリックし、[ファイルを開く] ダイアログボックスで目的の iTunes アートワークのイメージファイルを選択して、[**開く**] ボタンをクリックします。
4. アプリに必要なすべてのイメージが指定されるまで、この手順を繰り返します。

-----

## <a name="related-links"></a>関連リンク

- [イメージの操作 (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/workingwithimages)
- [Hello, iPhone](~/ios/get-started/hello-ios/index.md)
- [カスタムアイコンとイメージ作成のガイドライン](https://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html))
