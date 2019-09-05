---
title: 構成マネージャーで無効になっているチェック ボックスを配置する
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: aaf675cd-d885-4dac-9754-77dbcaea3be9
author: conceptdev
ms.author: crdun
ms.date: 12/02/2016
ms.openlocfilehash: 82ff1a684ffad75a301f0db6b0f8e3116be6746d
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70285063"
---
# <a name="deploy-checkboxes-disabled-in-configuration-manager"></a>構成マネージャーで無効になっているチェック ボックスを配置する

Xamarin 3.5 以降では、 **[スタート]** ツールバーボタンを押すか、デバッグ **[> 開始]** メニュー項目を選択するたびに、xamarin の iOS プロジェクトが自動的に配置されます。 これらのコマンドのいずれかを実行する前に、必要な Xamarin. iOS アプリプロジェクトを**スタートアッププロジェクト**として設定する必要があります。

このため、Visual Studio Configuration Manager for Xamarin. iOS プロジェクトでは、 **[配置]** チェックボックスは意図的に無効になっています。

![](deploy-checkboxes-images/configuration.png "Xamarin. iOS プロジェクトの [配置] チェックボックスが Xamarin 3.5 で無効になっていることを示す Configuration Manager Visual Studio")

この変更により、Xamarin iOS アプリプロジェクトが展開するように設定されていない場合に、Xamarin の以前のバージョン (バージョン3.3 以前) で表示されるエラーがなくなります。

![](deploy-checkboxes-images/error.png "エラーダイアログ:プロジェクトを開始する前に、iPhoneApp1 プロジェクトを配置する必要があります。ソリューション Configuration Manager に配置するプロジェクトが選択されていることを確認します。")
