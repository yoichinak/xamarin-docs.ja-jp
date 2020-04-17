---
title: Amazon App Store に公開する
ms.prod: xamarin
ms.assetid: A3E9EAC7-2968-8891-CDF2-B73FC0013EC9
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 288ff00b35c369581e50b8e8777f85b7b6119590
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "73021301"
---
# <a name="publishing-to-the-amazon-app-store"></a>Amazon App Store に公開する

Amazon のモバイル アプリ配信プログラムでは、モバイル アプリ開発者は Amazon でアプリケーションを公開できます。 このセクションでは、Amazon App Store for Android について簡単に取り上げます。 

[![Amazon App Store の画面](publishing-to-amazon-images/amazon-app-store.png)](publishing-to-amazon-images/amazon-app-store.png#lightbox)

Amazon では、APK のサイズに上限を課していません。 ただし、APK が 30MB より大きいと、Amazon Mobile App Distribution Portal ではなく、FTP が配信に利用されます。

## <a name="submitting-apps-binary-info"></a>アプリの送信:バイナリ情報

Amazon App Store にアプリケーションを提出する作業は、Google Play にアプリケーションを提出するプロセスと似ています。 Amazon が配信するアプリケーションには次のアセットが必要です。 

- **アイコン** &ndash;   114 x 114 の .png ファイルで、背景が透明のものです。 これは必須です。
- **サムネイル** &ndash;   上記のアイコンを大きくしたものです。 512 x 512 ピクセルであり、背景は透明にします。 このアイコンも必須です。
- **スクリーンショット** &ndash;   Amazon では、3 枚から 10 枚までのスクリーンショットが必要になります。 スクリーンショットは 1024w x 600h ピクセルまたは 800w x 480h ピクセルにします。 .png 形式と .jpg 形式を利用できます。
- **宣伝画像** &ndash;   ホーム ページなど、宣伝できる場所でアプリケーションを取り上げてもらうために、宣伝画像を任意で提出できます。 これは 1024w x 500h ピクセルの .png または .jpg ファイルにします。方向は横向きにします。 アニメーションを入れることはできません。
- 5 つの動画の更新を指定できます。

## <a name="approval-process"></a>承認プロセス

提出されたアプリケーションは承認プロセスに進みます。
Amazon はアプリケーションを検閲し、製品説明どおりに動くこと、顧客のデータを危険にさらさないこと、デバイスの動作を邪魔しないことを確認します。 承認プロセスが完了すると、Amazon は通知を送信し、アプリケーションを配信します。
