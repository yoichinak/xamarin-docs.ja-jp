---
title: Configuration Manager で無効になっているチェック ボックスを展開します。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: aaf675cd-d885-4dac-9754-77dbcaea3be9
author: asb3993
ms.author: amburns
ms.date: 12/02/2016
ms.openlocfilehash: ab825ba4d28ca8768e5c633fc3779828638a498d
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/09/2018
ms.locfileid: "33919515"
---
# <a name="deploy-checkboxes-disabled-in-configuration-manager"></a>Configuration Manager で無効になっているチェック ボックスを展開します。

Xamarin 3.5 以降 Xamarin.iOS プロジェクトは配置に自動的にキーを押すたびに、**開始**ツール バー ボタンまたは選択、**デバッグ > [デバッグ開始]** メニュー項目。 目的の Xamarin.iOS アプリ プロジェクトとして設定する必要があります、**スタートアップ プロジェクト**前に実行する前にこれらのいずれかのコマンドします。

このため、**展開**のチェック ボックスは Xamarin.iOS プロジェクトの Visual Studio 構成マネージャーで意図的に無効になります。

![](deploy-checkboxes-images/configuration.png "Visual Studio の Configuration Manager Xamarin 3.5 では、Xamarin.iOS プロジェクトに対して無効になって '配置' のチェック ボックスを表示")

この変更を展開する Xamarin.iOS アプリ プロジェクトが設定されていない場合に、Xamarin (3.3 およびそれ以前のバージョン) の旧バージョンで使用可能なエラーを排除できます。

![](deploy-checkboxes-images/error.png "エラー ダイアログ ボックス: プロジェクト iPhoneApp1 を起動する前に配置する必要があります。ソリューション構成マネージャーで配置するプロジェクトが選択されていることを確認します。")
