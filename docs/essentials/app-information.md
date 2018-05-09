---
title: Xamarin.Essentials アプリ情報
description: AppInfo クラスでは、アプリケーションに関する情報を提供します。
ms.assetid: 15924FCB-19E0-45B2-944E-E94FD7AE12FA
ms.technology: xamarin-crossplatform
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 5b235fdeb666c3f8f2c1b52a9691c931724ce2b8
ms.sourcegitcommit: 46d3c9daa45350bdd536d9e105517f3c1c753c5b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/07/2018
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
