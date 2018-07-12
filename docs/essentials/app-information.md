---
title: 'Xamarin.Essentials: アプリ情報'
description: このドキュメントでは、アプリケーションに関する情報を提供する、Xamarin.Essentials で AppInfo クラスについて説明します。 たとえば、アプリの名前とバージョンを公開します。
ms.assetid: 15924FCB-19E0-45B2-944E-E94FD7AE12FA
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 7e79b3003f41b8de22950624e44e8c9e0e7e7e31
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2018
ms.locfileid: "38831508"
---
# <a name="xamarinessentials-app-information"></a>Xamarin.Essentials: アプリ情報

![NuGet にプレリリースします。](~/media/shared/pre-release.png)

**AppInfo**クラスは、アプリケーションに関する情報を提供します。

## <a name="using-appinfo"></a>AppInfo を使用します。

クラスで Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

## <a name="obtaining-application-information"></a>アプリケーションの情報を取得するには。

次の情報は、API を介して公開されます。

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

## <a name="displaying-application-settings"></a>アプリケーションの設定を表示します。

**AppInfo**クラスは、アプリケーション用のオペレーティング システムによって保持されている設定のページにも表示できます。

```csharp
// Display settings page
AppInfo.OpenSettings();
```

この設定のページは、アプリケーションのアクセス許可を変更し、その他のプラットフォームに固有のタスクを実行できます。

## <a name="api"></a>API

- [AppInfo のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/AppInfo)
- [AppInfo API ドキュメント](xref:Xamarin.Essentials.AppInfo)
