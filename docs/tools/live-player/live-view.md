---
redirect_url: /xamarin/tools/live-player/
title: XAML Live プレビュー
description: このドキュメントでは、Xamarin Live Player を使用して XAML ページのプレビューを live、XAML に変更を加えるし、デバイスにすぐに表示されます。 変更を確認する方法について説明します。
ms.prod: xamarin
ms.assetid: 86E9A179-21F8-4F3A-A9CE-36F0FC5DB4A8
author: lobrien
ms.author: laobri
ms.date: 08/08/2018
ms.openlocfilehash: 1602c98eceaff607c79400a37c4ace60d5bf8807
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50110597"
---
# <a name="xaml-live-previewing"></a>XAML Live プレビュー

Xamarin Live Player の利点の 1 つは、XAML ページのプレビューを live、Visual Studio でコードを変更し、デバイスにすぐに表示、変更を参照してくださいする機能です。 Android デバイスまたはシミュレーターやエミュレーター、ライブ プレビューを作成できます。 このガイドでは、ライブ プレビュー機能を使用して、XAML の個々 の画面を表示する方法を示します。

## <a name="requirements"></a>必要条件

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Windows 7 以降を実行しているコンピューター。
2. Visual Studio 2017 バージョン 15.4 以降で、 **.NET によるモバイル開発**ワークロードをインストールします。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. OS X 10.11、macOS 10.12、またはそれ以上での Mac。
2. Visual Studio for Mac 7.2 以降。 最新バージョンをお勧めします。

-----

<a name="deploydevice" />

## <a name="deploying-to-device"></a>デバイスに展開します。

Xamarin Live Player を使用するには、Android デバイスに、前に、Xamarin Live Player アプリをダウンロードして」の説明に従って Visual Studio をペアリングする必要があります、[インストール](~/tools/live-player/install.md)ガイド。 Visual Studio は、デバイスを正常にペアリングする XAML ページのライブ プレビューを開始できます。 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Visual Studio 2017 のエディターでのライブ プレビューする XAML ページを開きます。

    ![](live-view-images/vs-image1.png)

2. デバイスの構成設定**デバッグ**Live Player デバイスを一覧から選択します。

    ![](live-view-images/vs-image2.png)

3. デバイスでのライブ ビューとしてこの XAML ページを実行する選択**ツール > Xamarin Live Player > 現在のビューの実行を Live**メニュー バーから。

    ![](live-view-images/vs-image3.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. エディターの Mac に Visual Studio でのライブ プレビューする XAML ページを開きます。

    ![](live-view-images/image1.png)

2. デバイスの構成設定**デバッグ**Live Player デバイスを一覧から選択します。

    ![](live-view-images/image2.png)

3. デバイスでのライブ ビューとしてこの XAML ページを実行する選択**実行 > 現在のビューの実行を Live**メニュー バーから。

    ![](live-view-images/image3.png)

-----

## <a name="deploying-to-android-emulator"></a>Android エミュレーターに展開します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Visual Studio 2017 のエディターでのライブ プレビューする XAML ページを開きます。

    ![](live-view-images/vs-image1.png)

2. デバイスの構成設定**デバッグ**Android 向け Live Player デバイスの一覧からを選択します。

    ![](live-view-images/vs-image4.png)

3. Android エミュレーターでのライブ ビューとしてこの XAML ページを実行する選択**ツール > Xamarin Live Player > 現在のビューの実行を Live**メニュー バーから。

    ![](live-view-images/vs-image3.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. エディターの Mac に Visual Studio でのライブ プレビューする XAML ページを開きます。

    ![](live-view-images/image7.png)

2. デバイスの構成設定**デバッグ**Android 向け Live Player デバイスの一覧からを選択します。

    ![](live-view-images/image6.png)

3. デバイスでのライブ ビューとしてこの XAML ページを実行するには、実行を選択 > ライブ実行現在のビュー、メニュー バーから。

    ![](live-view-images/image3.png)

-----

## <a name="related-links"></a>関連リンク

- [Xamarin Live Player の概要](https://xamarin.com/live)
- [ブログの投稿](https://blog.xamarin.com/live-player/)
- [Xamarin Live Player のサンプル](~/tools/live-player/samples.md)
