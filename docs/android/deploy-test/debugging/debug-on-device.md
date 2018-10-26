---
title: デバイスでのデバッグ
description: この記事では、Android の物理デバイス上で Xamarin.Android アプリケーションをデバッグする方法について説明します。
ms.prod: xamarin
ms.assetid: 153D3746-A27F-198B-48FE-D219C0133A79
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 3c30beadf7c2de618952bdf0cd5c03e7ceee2845
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50109706"
---
# <a name="debug-on-device"></a>デバイスでのデバッグ

_この記事では、Android の物理デバイス上で Xamarin.Android アプリケーションをデバッグする方法について説明します。_

## <a name="debug-on-device-overview"></a>デバイスでのデバッグの概要

Visual Studio for Mac または Visual Studio のいずれかを使用して、Android デバイスで Xamarin.Android アプリをデバッグできます。 デバイスでデバッグできるようにするには、デバイスが[開発用にセットアップ](~/android/get-started/installation/set-up-device-for-development.md)され、PC または Mac に接続されている必要があります。


## <a name="debug-application"></a>アプリケーションのデバッグ

デバイスがコンピューターに接続されると、Xamarin.Android アプリケーションのデバッグは、他の Xamarin 製品や .NET アプリケーションと同じように完了します。 **デバッグ**構成と外部デバイスが、IDE で選択されていることを確認します。これで、必要なデバッグ シンボルが使用可能になり、IDE が実行中のアプリケーションに接続できるようになります。 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![選択されたデバッグ構成](debug-on-device-images/image1-vs.png)

次に、コード内にブレークポイントを設定します。

![コードの行にブレークポイントを設定](debug-on-device-images/image2-vs.png)

デバイスが選択されると、Xamarin.Android はそのデバイスに接続され、アプリケーションを展開して実行します。 ブレークポイントに達すると、デバッガーはそのアプリケーションを停止し、他の C# アプリケーションと同様の方法でアプリケーションをデバッグできるようになります。 

![ブレークポイントに到達](debug-on-device-images/image3-vs.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![選択されたデバッグ構成](debug-on-device-images/image1-xs.png)

次に、コード内にブレークポイントを設定します。

![コードの行にブレークポイントを設定](debug-on-device-images/image2-xs.png)

デバイスが選択されると、Xamarin.Android はそのデバイスに接続され、アプリケーションを展開して実行します。 ブレークポイントに達すると、デバッガーはそのアプリケーションを停止し、他の C# アプリケーションと同様の方法でアプリケーションをデバッグできるようになります。 

![ブレークポイントに到達](debug-on-device-images/image3-xs.png)

-----



## <a name="summary"></a>まとめ

このドキュメントでは、ブレークポイントを設定し、対象のデバイスを選択して、Xamarin.Android アプリケーションをデバッグする方法について説明します。


## <a name="related-links"></a>関連リンク

- [開発用のデバイスの設定](~/android/get-started/installation/set-up-device-for-development.md)
- [デバッグできる属性の設定](~/android/deploy-test/debuggable-attribute.md)
