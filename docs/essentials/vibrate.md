---
title: 'Xamarin.Essentials: バイブレーション'
description: このドキュメントでは、バイブレーション機能を開始して一定時間後に停止することができる Xamarin.Essentials の Vibration クラスについて説明します。
ms.assetid: 7E8B24C4-2625-4DAE-A129-383542D34F1E
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
ms.openlocfilehash: d9bf7a1e5e0d15f1fdc909745cd439115b6f8463
ms.sourcegitcommit: 01f93a34b466f8d4043cef68fab9b35cd8decee6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/05/2018
ms.locfileid: "52898940"
---
# <a name="xamarinessentials-vibration"></a>Xamarin.Essentials: バイブレーション

**Vibration** クラスを使用すると、バイブレーション機能を開始して一定時間後に停止することができます。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

**Vibration** の機能にアクセスするには、次のプラットフォーム固有の設定が必要です。

# <a name="androidtabandroid"></a>[Android](#tab/android)

Vibrate アクセス許可が必要です。Android プロジェクト内で構成する必要があります。 これは次の方法で追加できます。

**[プロパティ]** フォルダーにある **AssemblyInfo.cs** ファイルを開き、以下を追加します。

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.Vibrate)]
```

または、Android マニフェストを追加します。

**[プロパティ]** フォルダーにある **AndroidManifest.xml** ファイルを開き、**manifest** ノードの内部に以下を追加します。

```xml
<uses-permission android:name="android.permission.VIBRATE" />
```

または、Android プロジェクトを右クリックし、プロジェクトのプロパティを開きます。 **[Android マニフェスト]** の下で **[必要なアクセス許可:]** 領域を探し、**VIBRATE** アクセス許可をオンにします。 これにより、**AndroidManifest.xml** ファイルが自動的に更新されます。

# <a name="iostabios"></a>[iOS](#tab/ios)

追加の設定は必要ありません。

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

プラットフォームによる違いはありません。

-----

## <a name="using-vibration"></a>Vibration の使用

自分のクラスに Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

Vibration 機能では、有効にする時間を要求でき、既定値は 500 ミリ秒です。

```csharp
try
{
    // Use default vibration length
    Vibration.Vibrate();

    // Or use specified time
    var duration = TimeSpan.FromSeconds(1);
    Vibration.Vibrate(duration);
}
catch (FeatureNotSupportedException ex)
{
    // Feature not supported on device
}
catch (Exception ex)
{
    // Other error has occurred.
}
```

デバイスのバイブレーションの取り消しは、`Cancel` メソッドで要求できます。

```csharp
try
{
    Vibration.Cancel();
}
catch (FeatureNotSupportedException ex)
{
    // Feature not supported on device
}
catch (Exception ex)
{
    // Other error has occurred.
}
```

## <a name="platform-differences"></a>プラットフォームによる違い

# <a name="androidtabandroid"></a>[Android](#tab/android)

プラットフォームによる違いはありません。

# <a name="iostabios"></a>[iOS](#tab/ios)

* デバイスが "着信時にバイブレーション" に設定されているときにのみバイブレーションします。
* 常に 500 ミリ秒だけバイブレーションします。
* バイブレーションを取り消すことはできません。

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

プラットフォームによる違いはありません。

-----

## <a name="api"></a>API

- [Vibration のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Vibration)
- [Vibration API のドキュメント](xref:Xamarin.Essentials.Vibration)
