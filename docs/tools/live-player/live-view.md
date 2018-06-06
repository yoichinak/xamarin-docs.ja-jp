---
redirect_url: /xamarin/tools/live-player/
title: XAML ライブ プレビュー
description: このドキュメントでは、Xamarin Live Player を使用してライブ プレビュー XAML ページは、XAML を変更してデバイスにすぐに表示、変更を反映する方法について説明します。
ms.prod: xamarin
ms.assetid: 86E9A179-21F8-4F3A-A9CE-36F0FC5DB4A8
author: topgenorth
ms.author: toopge
ms.date: 12/21/2017
ms.openlocfilehash: cc68044342fca84e62e3b17770170e1d7a23f677
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34793703"
---
# <a name="xaml-live-previewing"></a>XAML ライブ プレビュー

Xamarin Live Player の利点の 1 つは、XAML ページのプレビューを live、Visual Studio で、コードを変更およびがデバイスにすぐに表示、変更を反映する権限です。 IOS または Android のデバイスまたはシミュレーターやエミュレーターで、ライブのプレビューを作成できます。 このガイドでは、ライブのプレビュー機能を使用して、個々 の XAML 画面を表示する方法を示します。

## <a name="requirements"></a>必要条件

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Windows 7 以降を実行しているコンピューター。
2. Visual Studio 2017 15.4 以降のバージョン、 **.NET を使用したモバイル開発**ワークロードがインストールされています。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. OS X 10.11、macOS 10.12、またはそれ以上で Mac。
2. Mac 7.2 またはそれ以降の visual Studio です。 最新バージョンをお勧めします。

-----

<a name="deploydevice" />

## <a name="deploying-to-device"></a>デバイスに展開します。

Xamarin Live Player を使用するには、iOS または Android デバイスで、前に、Xamarin Player のライブ アプリをダウンロードして」の説明に従って Visual Studio をペアリングする必要があります、[インストール](~/tools/live-player/install.md)ガイドです。 Visual Studio にデバイスが正常に組み合わせるとき、XAML ページのリアルタイムのプレビューを開始できます。 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Visual Studio 2017 エディターでのライブのプレビューにする XAML ページを開きます。

    ![](live-view-images/vs-image1.png)

2. デバイスの構成設定**デバッグ | iPhone** iOS 用または**デバッグ**for Android、および一覧からライブ プレーヤー デバイスを選択。

    ![](live-view-images/vs-image2.png)

3. デバイスでライブのビューとしてこの XAML ページを実行するには選択**ツール > Xamarin Live Player > 現在のビューの実行をライブ**メニュー バーから。

    ![](live-view-images/vs-image3.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. Mac のエディター、Visual Studio でのライブのプレビューにする XAML ページを開きます。

    ![](live-view-images/image1.png)

2. デバイスの構成設定**デバッグ | iPhone** iOS 用または**デバッグ**for Android、および一覧からライブ プレーヤー デバイスを選択。

    ![](live-view-images/image2.png)

3. デバイスでライブのビューとしてこの XAML ページを実行するには選択**実行 > 現在のビューの実行をライブ**メニュー バーから。

    ![](live-view-images/image3.png)

-----

## <a name="deploying-to-android-emulator"></a>Android エミュレーターに展開します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Visual Studio 2017 エディターでのライブのプレビューにする XAML ページを開きます。

    ![](live-view-images/vs-image1.png)

2. デバイス構成を設定**デバッグ**for Android および、Live プレーヤーの一覧からデバイスを選択します。

    ![](live-view-images/vs-image4.png)

3. Android エミュレーター上のライブ ビューとしてこの XAML ページを実行するには選択**ツール > Xamarin Live Player > 現在のビューの実行をライブ**メニュー バーから。

    ![](live-view-images/vs-image3.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Mac のエディター、Visual Studio でのライブのプレビューにする XAML ページを開きます。

    ![](live-view-images/image7.png)

2. デバイス構成を設定**デバッグ**for Android および、Live プレーヤーの一覧からデバイスを選択します。

    ![](live-view-images/image6.png)

3. デバイスでライブのビューとしてこの XAML ページを実行するには、実行を選択 > ライブ実行現在のビュー、メニュー バーから。

    ![](live-view-images/image3.png)

-----

## <a name="deploying-to-ios-simulator"></a>IOS シミュレーターに展開します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

現在、Windows 上のリモート iOS シミュレーターでプレビューするライブ XAML を使用するためのサポートされていません。 代わりにする必要があります[はデバイスに展開](#deploydevice)です。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Mac のエディター、Visual Studio でのライブのプレビューにする XAML ページを開きます。

    ![](live-view-images/image1.png)

2. デバイス構成を設定**デバッグ | iPhoneSimulator** iOS デバイスと、一覧から iOS シミュレーターを選択します。

    ![](live-view-images/image2.png)

3. 選択**実行 > 現在のビューの実行をライブ**シミュレーターを起動し、XAML ページを表示するには、メニュー バーから。

    ![](live-view-images/image4.png)

4. シミュレーターが起動した後は、XAML を編集を開始し、ライブで表示されるプレビューを参照してください。

    ![](live-view-images/image5.png)  

-----

## <a name="related-links"></a>関連リンク

- [Xamarin Player のライブの概要](https://xamarin.com/live)
- [ブログの投稿](https://blog.xamarin.com/live-player/)
- [Xamarin Player のライブのサンプル](~/tools/live-player/samples.md)
