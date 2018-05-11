---
title: Xamarin.Essentials アプリ情報
description: AppInfo クラスでは、アプリケーションに関する情報を提供します。
ms.assetid: 15924FCB-19E0-45B2-944E-E94FD7AE12FA
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 32e3eb8fab719540e4c9ffec4e57f5510c10e3f5
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/10/2018
---
# <a name="xamarinessentials-app-information"></a>Xamarin.Essentials アプリ情報

![プレリリース NuGet](~/media/shared/pre-release.png)

**AppInfo**クラスは、アプリケーションに関する情報を提供します。

## <a name="using-appinfo"></a>AppInfo の使用

クラスの Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

次の情報は、API を介して公開されています。

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

## <a name="api"></a>API

- [AppInfo のソース コード](https://github.com/xamarin/Essentials/tree/master/Essentials/AppInfo)
- [AppInfo API ドキュメント](xref:Xamarin.Essentials.AppInfo)
