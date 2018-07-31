---
title: 'Xamarin.Essentials: バージョン管理'
description: Xamarin.Essentials で VersionTracking クラスでは、アプリケーションのバージョンを確認することができ、ビルド番号が、最初にある場合、このような追加情報を表示すると共に、アプリケーションの起動をこれまでの時間または、現在のバージョンでは、前回のビルドを取得については、その他
ms.assetid: 670C7E8A-E882-4AC0-97D2-A53D90ADD6A3
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 81dc67fa5a4975f31d0fbf9f7219637596a827ce
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353660"
---
# <a name="xamarinessentials-version-tracking"></a>Xamarin.Essentials: バージョン管理

![NuGet にプレリリースします。](~/media/shared/pre-release.png)

**VersionTracking**クラスでは、アプリケーションのバージョンを確認することができ、これまでに起動するアプリケーションまたは現在のバージョンの取得前が最初にある場合、このような追加情報を表示すると共に、ビルド番号ビルド情報など。

## <a name="using-version-tracking"></a>バージョンの追跡を使用

クラスで Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

初めて使用する、 **VersionTracking**クラスの現在のバージョンの追跡を開始します。 呼び出す必要があります`Track`が読み込まれた現在のバージョン情報が追跡されるたびに、アプリケーションにのみ。

```csharp
VersionTracking.Track();
```

初期後`Track`と呼びますバージョン情報を読み取ることができます。

```csharp

// First time ever launched application
var firstLaunch = VersionTracking.IsFirstLaunchEver;

// First time launching current version
var firstLaunchCurrent = VersionTracking.IsFirstLaunchForCurrentVersion;

// First time launching current build
var firstLaunchBuild = VersionTracking.IsFirstLaunchForCurrentBuild;

// Current app version (2.0.0)
var currentVersion = VersionTracking.CurrentVersion;

// Current build (2)
var currentBuild = VersionTracking.CurrentBuild;

// Previous app version (1.0.0)
var previousVersion = VersionTracking.PreviousVersion;

// Previous app build (1)
var previousBuild = VersionTracking.PreviousBuild;

// First version of app installed (1.0.0)
var firstVersion = VersionTracking.FirstInstalledVersion;

// First build of app installed (1)
var firstBuild = VersionTracking.FirstInstalledBuild;

// List of versions installed (1.0.0, 2.0.0)
var versionHistory = VersionTracking.VersionHistory;

// List of builds installed (1, 2)
var buildHistory = VersionTracking.BuildHistory;
```

## <a name="platform-implementation-specifics"></a>プラットフォームの実装の詳細

使用してすべてのバージョン情報を格納、[設定](preferences.md)Xamarin.Essentials で API のファイル名では **[YOUR-アプリのパッケージの ID].xamarinessentials.versiontracking**に従って同じ記載されているデータの永続化、[設定](preferences.md#persistence)ドキュメント。

## <a name="api"></a>API

- [バージョンの追跡のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/VersionTracking)
- [バージョンの追跡の API ドキュメント](xref:Xamarin.Essentials.VersionTracking)
