---
title: "アプリケーションのアイコン"
description: "この資料について説明など、アプリ アイコンとして使用する Xamarin.iOS アプリでのイメージ資産を管理します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: B7791574-4A0F-4CB6-8C18-36D40B5C91EB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/22/2017
ms.openlocfilehash: 6de57e9523ff336c2e06e39903280db9c9ab95fa
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="application-icons"></a>アプリケーションのアイコン

_この資料について説明など、アプリ アイコンとして使用する Xamarin.iOS アプリでのイメージ資産を管理します。_

次のトピックで、詳しく説明します。

* [アプリケーションのスポット ライト、設定アイコン](#icon-types)-iOS アプリに必要なアイコンの種類。
* [アイコンと資産カタログを管理する](#managing)- 資産カタログを使用するアプリケーション アイコンを管理します。
* [iTunes アートワーク](#itunes)-アプリケーションの配信のアドホック メソッドの必須 iTunes アートワークを指定します。

<a name="icon-types" />

## <a name="application-spotlight-and-settings-icons"></a>アプリケーション、メディア、および設定アイコン

Xamarin.iOS アプリを UI コントロールとドキュメントのアイコンのイメージ アセットを使用できること、同じ方法では、アプリケーションのアイコンを提供するイメージの資産を使用できます。 IPad の次のスクリーン ショットは、iOS でのアイコンの 3 つの用途を示しています。

- **アプリケーション アイコン**-すべての iOS アプリは、アプリケーション アイコンを定義する必要があります。 これは、ユーザーがタップするアイコン iOS ホーム画面でアプリを起動します。 さらに、このアイコンがゲーム センターで使用されるは、該当する場合。 例: 

    [ ![](app-icons-images/000.png "アプリケーション アイコン")](app-icons-images/000-full.png)
- **アイコンのスポット ライト**- Spotlight 検索に、アプリの名前を入力するたびにこのアイコンが表示されます。 例: 

    [ ![](app-icons-images/000a.png "スポット ライト アイコン")](app-icons-images/000a-full.png)
- **設定アイコン**- ユーザーが入力した場合、**設定**の最後に、iOS デバイスを次のアイコン上のアプリが表示されます、**設定**アプリの一覧です。 例: 

    [ ![](app-icons-images/000b.png "設定アイコン")](app-icons-images/000b-full.png)

次のイメージ資産のサイズと解像度は、すべての iOS 9 (またはそれ以上) を介して iOS 5 を対象とする Xamarin.iOS アプリで必要なアイコンの種類をサポートするために必要なされます。

<table cellpadding="7" cellspacing="0" width="100%">
    <tr valign="top">
        <td width="200" style="border-width: 0px;"></td>
        <td colspan="5" align="center" bgcolor="#F0F0F0"><b>iPhone</b></td>
    </tr>
    <tr valign="center">
        <td width="200" style="border-width: 0px;"></td>
        <td colspan="2" align="center" bgcolor="#F9F9F9"><b>iOS 5 & 6</b></td>
        <td colspan="2" align="center" bgcolor="#F9F9F9"><b>iOS 7 & 8</b></td>
        <td align="center" bgcolor="#F9F9F9"><b>iOS 9 & 10<b><br/><i>(iPhone 6 および 7 Plus)</i></td>
    </tr>
    <tr valign="top" bgcolor="#F0F0F0">
        <td width="200" align="center"><b>アイコンの種類</b></td>
        <td align="center"><b>1x</b></td>
        <td align="center"><b>2x</b></td>
        <td align="center"><b>1x</b></td>
        <td align="center"><b>2x</b></td>
        <td align="center"><b>3x</b></td>
    </tr>
    <tr valign="top">
        <td width="200" bgcolor="#F9F9F9" align="right">アプリケーション アイコン</td>
        <td align="center">57 x 57</td>
        <td align="center">114x114</td>
        <td align="center" style="color:#BBBBBB;">60x60<sup>(1)</sup></td>
        <td align="center">120 x 120</td>
        <td align="center">180x180</td>
    </tr>
    <tr valign="top">
        <td width="200" bgcolor="#F9F9F9" align="right">スポット ライト</td>
        <td align="center">29 x 29</td>
        <td align="center">58 x 58</td>
        <td align="center" style="color:#BBBBBB;">40x40<sup>(2)</sup></td>
        <td align="center">80x80</td>
        <td align="center">120 x 120</td>
    </tr>
    <tr valign="top">
        <td width="200" bgcolor="#F9F9F9" align="right">設定</td>
        <td align="center" style="color:#BBBBBB;">29x29<sup>(3)(4)</sup></td>
        <td align="center" style="color:#BBBBBB;">58x58<sup>(3)(4)</sup></td>
        <td align="center">-</td>
        <td align="center">-</td>
        <td align="center">87x87</td>
    </tr>
</table>

<table cellpadding="7" cellspacing="0" width="100%">
    <tr valign="top">
        <td width="200" style="border-width: 0px;"></td>
        <td colspan="5" align="center" bgcolor="#F0F0F0"><b>iPad</b></td>
    </tr>
    <tr valign="center">
        <td width="200" style="border-width: 0px;"></td>
        <td colspan="2" align="center" bgcolor="#F9F9F9"><b>iOS 5 & 6</b></td>
        <td colspan="2" align="center" bgcolor="#F9F9F9"><b>iOS 7 & 8</b></td>
        <td colspan="1" align="center" bgcolor="#F9F9F9"><b>iOS&nbsp;9 & 10</b></td>
    </tr>
    <tr valign="top" bgcolor="#F0F0F0">
        <td width="200" align="center"><b>アイコンの種類</b></td>
        <td align="center"><b>1x</b></td>
        <td align="center"><b>2x</b></td>
        <td align="center"><b>1x</b></td>
        <td align="center"><b>2x</b></td>
        <td align="center"><b>2x<br/>iPad&nbsp;Pro</b></td>
    </tr>
    <tr valign="top">
        <td width="200" bgcolor="#F9F9F9" align="right">アプリケーション アイコン</td>
        <td align="center">72 x 72</td>
        <td align="center">144x144</td>
        <td align="center">76 x 76</td>
        <td align="center">152x152</td>
        <td align="center">167x167<sup>(6)</sup></td>
    </tr>
    <tr valign="top">
        <td width="200" bgcolor="#F9F9F9" align="right">スポット ライト</td>
        <td align="center">50 x 50</td>
        <td align="center">100x100</td>
        <td align="center">40 x 40</td>
        <td align="center">80x80</td>
        <td align="center" style="color:#BBBBBB;">120x120<sup>(5)</sup></td>
    </tr>
    <tr valign="top">
        <td width="200" bgcolor="#F9F9F9" align="right">設定</td>
        <td align="center" style="color:#BBBBBB;">29x29<sup>(3)(5)</sup></td>
        <td align="center" style="color:#BBBBBB;">58x58<sup>(3)(5)</sup></td>
        <td align="center">-</td>
        <td align="center">-</td>
        <td align="center" style="color:#BBBBBB;">58x58<sup>(5)</sup></td>
    </tr>
</table>

1. _Mac と Xcode の両方の Visual Studio は、iOS 7 の 1 の x 画像の設定をサポートしません。_
2. _資産カタログを使用する場合は、iOS 7 の 1 x 画像の設定はサポートされていません。_
3. _iOS 7 と 8 では、iOS 5 および 6 として同じイメージのサイズを使用します。_
4. _スポット ライト アイコンとして同じイメージとサイズを使用します。_
5. _IPhone として同じサイズのアイコンを使用します。_
6. _アセット カタログのイメージ セットでのみサポートされます。_

アイコンの詳細については、Apple を参照してください[アイコンおよび画像のサイズ](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG/IconMatrix.html#//apple_ref/doc/uid/TP40006556-CH27-SW1)ドキュメント。

<a name="managing" />

## <a name="managing-icons-with-asset-catalogs"></a>アイコンと資産カタログを管理します。

アイコン、特殊な用`AppIcons`イメージ セットに追加できる、`Assets.xcassets`アプリのプロジェクト内のファイルです。 解像度すべてをサポートするために必要なイメージのすべてのバージョンに含まれる、 _xcasset_され、一緒にグループ化します。 Mac 用の Visual Studio での特殊なエディターには、含めるし、グラフィカルにこれらのイメージをセットアップする開発者ができるようにします。

資産カタログを使用するには、次の操作を行います。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. ダブルクリックして、`Info.plist`ファイルで、**ソリューション エクスプ ローラー**編集用に開きます。
2. 下方向にスクロール、**アプリ アイコン**セクションです。
3. **ソース** ドロップダウン リストで、確認**AppIcons**が選択されています。 

    ![](app-icons-images/migrate01.png "AppIcons が選択されていることを確認します。")
4. **ソリューション エクスプ ローラー**をダブルクリックして、`Assets.xcassets`ファイルを開いて編集するファイル。 

    ![](app-icons-images/asset01.png "ソリューション エクスプ ローラーで Assets.xcassets ファイル")
5. 選択`AppIcons`を表示する資産の一覧から、 `Icon Editor`: 

    ![](app-icons-images/asset02.png "AppIcons エディター")
6. 指定されたアイコンの種類 をクリックし、必要な型/表示サイズのイメージ ファイルを選択、またはフォルダーからイメージのドラッグし、目的のサイズの上にドロップします。
7. をクリックして、**開く** をクリックして、プロジェクトにイメージを含める、xcasset で設定します。
8. 必要なすべてのイメージを繰り返します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. ダブルクリックして、 **Info.plist**ファイルで、**ソリューション エクスプ ローラー**:

    ![](app-icons-images/icon01w.png "Info.plist を選択します。")
2. をクリックして、**のビジュアル アセット** タブでをクリックし、**資産カタログを使用して**下にあるボタン**アプリ アイコン**: 

    ![](app-icons-images/icon02w.png "ビジュアル資産 タブを選択します。")
4. **ソリューション エクスプ ローラー**、展開、**アセット カタログ**フォルダー。 

    ![](app-icons-images/image009.png "アセット カタログのフォルダーを展開します。")
5. ダブルクリックして、**メディア**エディターで開くファイル。 

    ![](app-icons-images/image010.png "エディターで、メディア ファイルを開く")
6. 下にある、**プロパティ エクスプ ローラー**開発者は、さまざまな種類とサイズのために必要なアイコンを選択できます。
7. 指定されたアイコンの種類 をクリックし、必要な型/表示サイズのイメージ ファイルを選択します。
8. をクリックして、**開く** をクリックして、プロジェクトにイメージを含める、xcasset で設定します。
9. 必要なすべてのイメージを繰り返します。

-----

これは、アプリケーション、スポット ライト、設定アイコンをアプリに提供するために使用するイメージ資産の管理などの推奨される方法です。

### <a name="migrating-from-infoplist-to-asset-catalogs"></a>Info.plist から資産カタログへの移行

Xamarin.iOS アプリを使用して既存の`Info.plist`のアイコンを管理するファイルを開発者に切り替える使用を強くお勧め、`AppIcons`内のイメージ アセット、`Assets.xcassets`です。

次の手順で行います。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. ダブルクリックして、`Info.plist`ファイルで、**ソリューション エクスプ ローラー**編集用に開きます。
2. 下方向にスクロール、**アプリ アイコン**セクションです。
3. **ソース**ドロップダウン リストで、**資産カタログへの移行**: 

    ![](app-icons-images/migrate02.png "資産カタログへの移行を選択")
4. 定義されているすべての既存のアイコン、`Info.plist`ファイルに移行される、`AppIcons`イメージの選択に追加`Assets.xcassets`: 

     ![](app-icons-images/migrate03.png "Assets.xcassets AppIcons イメージ セット")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. ダブルクリックして、`Info.plist`ファイルで、**ソリューション エクスプ ローラー**編集用に開きます。
2. アイコンのセクションで、iPhone でをクリックします。 

    ![](app-icons-images/image007.png "された iPhone アイコン エディター")
3. 下方向にスクロール、**アイコン**セクションです。
4. **アセット カタログ**ドロップダウン リストで、**資産カタログを使用して**です。
5. 定義されているすべての既存のアイコン、`Info.plist`ファイルに移行される、`Images`セットに追加された`Assets.xcassets`です。
6. 変更内容を `Info.plist` ファイルに保存します。

-----

<a name="itunes" />

## <a name="itunes-artwork"></a>iTunes アートワーク

(か企業ユーザー向けの実際のデバイスでテスト ベータ版)、アプリの配信のアドホック メソッドを使用する場合、開発者も含める必要がある 512 x 512 と 1024 × 1024 イメージ iTunes でアプリを表すために使用されます。

iTunes アートワークは次の手順で指定します。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. ダブルクリックして、`Info.plist`ファイルで、**ソリューション エクスプ ローラー**編集用に開きます。
2. スクロールして、 **iTunes アートワーク**エディターのセクション。 

    ![](app-icons-images/itunes01.png "ITunes エディターのアートワーク セクションまでスクロールします。")
3. 不足している画像、サムネイル、エディターでをクリックし、ファイルを開く ダイアログ ボックスから目的 iTunes アートワークのイメージ ファイルを選択し、クリックして、 **OK**ボタンをクリックします。
4. 必要なイメージが指定されているアプリのすべてが、この手順を繰り返します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. ダブルクリックして、`Info.plist`ファイルで、**ソリューション エクスプ ローラー**編集用に開きます。

2. をクリックして、**のビジュアル アセット** タブでを展開し、 **iTunes アートワーク**: 

    ![](app-icons-images/itunes01w.png "ITunes アートワークを Visual Studio での編集")
4. 不足している画像、サムネイル、エディターでをクリックし、ファイルを開く ダイアログ ボックスから目的 iTunes アートワークのイメージ ファイルを選択し、クリックして、**開く**ボタンをクリックします。
5. 必要なイメージが指定されているアプリのすべてが、この手順を繰り返します。

-----

## <a name="related-links"></a>関連リンク

- [イメージ (サンプル) の操作](https://developer.xamarin.com/samples/WorkingWithImages/)
- [Hello, iPhone](~/ios/get-started/hello-ios/index.md)
- [カスタム アイコンをクリックしてイメージ作成のガイドライン](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html))
