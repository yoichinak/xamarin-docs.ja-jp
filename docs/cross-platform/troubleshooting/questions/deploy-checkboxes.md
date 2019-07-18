---
title: 構成マネージャーで無効になっているチェック ボックスを配置する
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: aaf675cd-d885-4dac-9754-77dbcaea3be9
author: asb3993
ms.author: amburns
ms.date: 12/02/2016
ms.openlocfilehash: 35efb00a721062ad3217300f7e3a5430b1bd1560
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61357765"
---
# <a name="deploy-checkboxes-disabled-in-configuration-manager"></a>構成マネージャーで無効になっているチェック ボックスを配置する

Xamarin の 3.5 から Xamarin.iOS プロジェクトを展開に自動的にキーを押すたびに、**開始**ツール バー ボタンや選択、**デバッグ > [デバッグ開始]** メニュー項目。 目的の Xamarin.iOS アプリ プロジェクトとして設定する必要があります、**スタートアップ プロジェクト**これらのコマンドのいずれかを実行する前にします。

このため、**デプロイ**のチェック ボックスが Xamarin.iOS プロジェクト用 Visual Studio 構成マネージャーで意図的に無効になります。

![](deploy-checkboxes-images/configuration.png "Visual Studio 構成マネージャーが Xamarin 3.5 で Xamarin.iOS プロジェクトに対して無効です「デプロイ」のチェック ボックスを表示")

この変更により、エラーを展開する Xamarin.iOS アプリ プロジェクトが設定されていない場合、Xamarin (バージョン 3.3 以前) の旧バージョンで表示される可能性があります。

![](deploy-checkboxes-images/error.png "エラー ダイアログ ボックス:プロジェクト iPhoneApp1 を開始する前に配置する必要があります。ソリューションの Configuration Manager でデプロイするプロジェクトが選択されていることを確認します。")
