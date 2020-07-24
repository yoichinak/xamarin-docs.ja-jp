---
title: 構成マネージャーで無効になっているチェック ボックスを配置する
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: aaf675cd-d885-4dac-9754-77dbcaea3be9
author: davidortinau
ms.author: daortin
ms.date: 12/02/2016
ms.openlocfilehash: c3d0b8f2da971238b98253405f3b8fe08699ab56
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/23/2020
ms.locfileid: "86997216"
---
# <a name="deploy-checkboxes-disabled-in-configuration-manager"></a>構成マネージャーで無効になっているチェック ボックスを配置する

Xamarin 3.5 以降では、[**スタート**] ツールバーボタンを押すか、[デバッグ **> 開始**] メニュー項目を選択するたびに、xamarin の iOS プロジェクトが自動的に配置されます。 これらのコマンドのいずれかを実行する前に、必要な Xamarin. iOS アプリプロジェクトを**スタートアッププロジェクト**として設定する必要があります。

このため、Visual Studio Configuration Manager for Xamarin. iOS プロジェクトでは、[**配置**] チェックボックスは意図的に無効になっています。

![Xamarin 3.5 で Xamarin. iOS プロジェクトの [配置] チェックボックスが無効になっている Configuration Manager Visual Studio](deploy-checkboxes-images/configuration.png)

この変更により、Xamarin iOS アプリプロジェクトが展開するように設定されていない場合に、Xamarin の以前のバージョン (バージョン3.3 以前) で表示されるエラーがなくなります。

![エラーダイアログ: プロジェクトを開始するには、iPhoneApp1 を配置する必要があります。 ソリューション Configuration Manager に配置するプロジェクトが選択されていることを確認します。](deploy-checkboxes-images/error.png)
