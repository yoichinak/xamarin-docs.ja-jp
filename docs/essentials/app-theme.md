---
title: Xamarin.Essentials:アプリのテーマ
description: このドキュメントでは、Xamarin.Essentials の要求されたアプリ テーマに関する API について説明します。実行中のアプリに対して要求されているテーマ スタイルに関する情報が記載されています。
ms.assetid: F6F6D496-A8A9-4B9A-AF1A-370D937E5073
author: jamesmontemagno
ms.custom: video
ms.author: jamont
ms.date: 01/06/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: c68f00b77f0b9f88d014334dc56e1e58ed057986
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91436884"
---
# <a name="no-locxamarinessentials-app-theme"></a>Xamarin.Essentials:アプリのテーマ

**RequestedTheme** API は [`AppInfo`](app-information.md) クラスの一部であり、実行中のアプリに対してシステムから要求されているテーマに関する情報が提供されます。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-requestedtheme"></a>RequestedTheme の使用

クラスの Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

## <a name="obtaining-theme-information"></a>テーマ情報の取得

要求されたアプリケーションのテーマは、次の API を使用して検出できます。

```csharp
AppTheme appTheme = AppInfo.RequestedTheme;

```

これにより、システムによって現在要求されているアプリケーションのテーマが提供されます。 その戻り値は、次のいずれかになります。

* 指定されていません。
* 淡色
* 濃色

オペレーティング システムから要求される特定のユーザー インターフェイス スタイルがない場合は、Unspecified が返されます。 この例としては、13.0 より前のバージョンの iOS を稼働しているデバイスなどがあります。


## <a name="platform-implementation-specifics"></a>プラットフォームの実装の詳細

# <a name="android"></a>[Android](#tab/android)

Android では、要求するテーマの種類を指定するために、ユーザーからの構成モードが使用されます。 これは、Android のバージョンに基づき、ユーザーが変更したり、バッテリ節約モードが有効になっているときに変更されたりする場合があります。

詳細については、公式の[ダーク テーマに関する Android ドキュメント](https://developer.android.com/guide/topics/ui/look-and-feel/darktheme)を参照してください。


# <a name="ios"></a>[iOS](#tab/ios)

13.0 より前のバージョンの iOS では、常に Unspecified が返されます


# <a name="uwp"></a>[UWP](#tab/uwp)

`RequestedTheme` の呼び出しは UI スレッドで行う必要があります。そうしないと、例外がスローされます。

UWP アプリケーションでは、**RequestedTheme** で UWP App.xaml の設定が尊重されます。 特定のテーマに設定されている場合、Xamarin.Essentials からは常にこの設定が返されます。 OS の動的テーマを使用するには、アプリケーションからこのノードを削除します。その後、アプリが実行されると、Windows の設定でユーザーが設定したテーマが返されます ( **[設定] > [パーソナル化] > [色] > [Choose your default app mode]\(既定のアプリ モードを選択する\)** )。

詳細については、[UWP の要求されたテーマに関するドキュメント](/uwp/api/windows.ui.xaml.application.requestedtheme)を参照してください。

--------------

## <a name="api"></a>API

- [AppInfo のソース コード](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/AppInfo)
- [AppInfo API のドキュメント](xref:Xamarin.Essentials.AppInfo)

## <a name="related-video"></a>関連ビデオ

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Theme-Detection-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]