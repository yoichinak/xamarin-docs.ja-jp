---
title: 'Xamarin.Essentials: アプリ情報'
description: このドキュメントでは、アプリケーションに関する情報を提供する Xamarin.Essentials の AppInfo クラスについて説明します。 たとえば、アプリの名前やバージョンが公開されます。
ms.assetid: 15924FCB-19E0-45B2-944E-E94FD7AE12FA
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 62ecf890ba6b3276db89e93c2699e0406dbe4d45
ms.sourcegitcommit: a635312ffec816ba357a92b66c8c5221c8d9044c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/02/2018
ms.locfileid: "50965618"
---
# <a name="xamarinessentials-app-information"></a>Xamarin.Essentials: アプリ情報

![プレリリースの NuGet](~/media/shared/pre-release.png)

**AppInfo** クラスでは、アプリケーションに関する情報が提供されます。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-appinfo"></a>AppInfo の使用

自分のクラスの Xamarin.Essentials に参照を追加します。

```csharp
using Xamarin.Essentials;
```

## <a name="obtaining-application-information"></a>アプリケーションの情報の取得:

API では次の情報が公開されます。

```csharp
// Application Name
var appName = AppInfo.Name;

// Package Name/Application Identifier (com.microsoft.testapp)
var packageName = AppInfo.PackageName;

// Application Version (1.0.0)
var version = AppInfo.VersionString;

// Application Build Number (1)
var build = AppInfo.BuildString;
```

## <a name="displaying-application-settings"></a>アプリケーションの設定の表示

**AppInfo** クラスでは、アプリケーションに関してオペレーティング システムで保持されている設定のページも表示できます。

```csharp
// Display settings page
AppInfo.ShowSettingsUI();
```

この設定ページでは、アプリケーションのアクセス許可を変更したり、プラットフォーム固有の他のタスクを実行したりできます。

## <a name="api"></a>API

- [AppInfo のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/AppInfo)
- [AppInfo API のドキュメント](xref:Xamarin.Essentials.AppInfo)
