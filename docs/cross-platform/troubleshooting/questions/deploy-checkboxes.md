---
title: 構成マネージャーで無効になっているチェック ボックスを配置する
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: aaf675cd-d885-4dac-9754-77dbcaea3be9
author: davidortinau
ms.author: daortin
ms.date: 12/02/2016
ms.openlocfilehash: edf471f1d9a2ee4adc11f09e0c7b7ad3cf6f78f1
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73014255"
---
# <a name="deploy-checkboxes-disabled-in-configuration-manager"></a>構成マネージャーで無効になっているチェック ボックスを配置する

Xamarin 3.5 以降では、 **[スタート]** ツールバーボタンを押すか、デバッグ **[> 開始]** メニュー項目を選択するたびに、xamarin の iOS プロジェクトが自動的に配置されます。 これらのコマンドのいずれかを実行する前に、必要な Xamarin. iOS アプリプロジェクトを**スタートアッププロジェクト**として設定する必要があります。

このため、Visual Studio Configuration Manager for Xamarin. iOS プロジェクトでは、 **[配置]** チェックボックスは意図的に無効になっています。

![](deploy-checkboxes-images/configuration.png "Visual Studio Configuration Manager showing the 'Deploy' checkbox disabled for a Xamarin.iOS project in Xamarin 3.5")

この変更により、Xamarin iOS アプリプロジェクトが展開するように設定されていない場合に、Xamarin の以前のバージョン (バージョン3.3 以前) で表示されるエラーがなくなります。

![](deploy-checkboxes-images/error.png "Error dialog: The project iPhoneApp1 needs to be deployed before it can be started. Verify the project is selected to be deployed in the Solution Configuration Manager.")
