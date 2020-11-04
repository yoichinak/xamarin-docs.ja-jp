---
title: Xcode を使用した Ui のデザイン
description: IOS ユーザーインターフェイスを構築するには、Mac で Xcode を使用することをお勧めします。
ms.prod: xamarin
ms.assetid: af9f95db-5cd6-475d-867d-f73e1574e8fc
ms.technology: xamarin-ios
author: joshspicer
ms.author: jospicer
ms.date: 10/27/2020
ms.openlocfilehash: b6fc5c89e9221bc84b364290a275fbd6d35c12c4
ms.sourcegitcommit: d2aa3a8bf9a60b6708db55b10b0c6893c06d3256
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/04/2020
ms.locfileid: "93343314"
---
# <a name="using-xcode-to-design-user-interfaces"></a>Xcode を使用したユーザーインターフェイスの設計

_Storyboard ファイルと nib ファイルを編集する場合は、Mac 上の Xcode Interface Builder で編集することをお勧めします。_

## <a name="how-to-open-a-storyboard"></a>ストーリーボードを開く方法 

ストーリーボードファイルを右クリックし、[ **Xcode Interface Builder** ] を選択して Visual Studio for Mac で iOS ユーザーインターフェイスファイルを開きます。

[![Interface Builder の選択](images/select-interface-builder.png)](images/select-interface-builder.png#lightbox)

Xcode ウィンドウが表示されます。 ここで保存した編集は、Visual Studio プロジェクトに反映されます。

[![Xcode ウィンドウ](images/xcode.png)](images/xcode.png#lightbox)

Xcode Interface Builder の詳細については、 [こちら](https://developer.apple.com/xcode/interface-builder/)を参照してください。

## <a name="known-problems"></a>既知の問題

このセクションでは、既知の問題について説明します。

### <a name="visual-studio-could-not-communicate-with-xcode"></a>"Visual Studio は Xcode と通信できませんでした"

MacOS Catalina.properties 以降では、以下のエラーが発生する場合があります。

[![エラーを通知しない](images/could-not-communicate.png)](images/could-not-communicate.png#lightbox)

まず、Visual Studio が表示され、 **Xcode** がオンになっていることを **> &** 確認します。

[![macOS のセキュリティ](images/macos-security.png)](images/macos-security.png#lightbox)

**Xcode** がオンになっていて、エラーメッセージがまだ表示されている場合は、Visual Studio for Mac プライバシーアクセス許可のリセットが必要になることがあります。

これは、ターミナルウィンドウを起動し、コマンドを発行することで実現できます。

```bash
sudo tccutil reset All "com.microsoft.visual-studio"
```

上記の変更を確実に適用するには、Mac の PRAM をリセットします。 その方法については、 [こちら](https://support.apple.com/HT204063)を参照してください。
