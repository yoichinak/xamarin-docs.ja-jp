---
title: 'Xamarin.Essentials: 振動'
description: このドキュメントを起動し、必要な時間のバイブレーション機能を停止することができる Xamarin.Essentials の振動クラスについて説明します。
ms.assetid: 7E8B24C4-2625-4DAE-A129-383542D34F1E
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 622689342dd961a63318a88f098dea4d1a60e277
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353868"
---
# <a name="xamarinessentials-vibration"></a>Xamarin.Essentials: 振動

![NuGet にプレリリースします。](~/media/shared/pre-release.png)

**振動**クラスを起動し、必要な時間のバイブレーション機能を停止することができます。

## <a name="getting-started"></a>作業の開始

アクセスする、**振動**次のプラットフォーム固有設定の機能が必要です。

# <a name="androidtabandroid"></a>[Android](#tab/android)

バイブレーション アクセス許可が必要ですし、Android プロジェクトで構成する必要があります。 これは、次の方法で追加できます。

開く、 **AssemblyInfo.cs**ファイル、**プロパティ**フォルダーを追加。

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.Vibrate)]
```

または、Android マニフェストを更新します。

開く、 **AndroidManifest.xml**ファイル、**プロパティ**フォルダー内の次の追加と、**マニフェスト**ノード。

```xml
<uses-permission android:name="android.permission.VIBRATE" />
```

または、Android プロジェクトを右クリックし、プロジェクトのプロパティを開きます。 **Android マニフェスト**検索、**ために必要なアクセス許可:** 領域とチェック、**バイブレーション**権限。 これは自動的に更新、 **AndroidManifest.xml**ファイル。

# <a name="iostabios"></a>[iOS](#tab/ios)

追加の設定が必要です。

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

プラットフォームの違いはありません。

-----

## <a name="using-vibration"></a>振動を使用します。

クラスで Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

振動機能は、一定の時間または 500 ミリ秒の既定値を要求できます。

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

指定するとデバイスの振動の取り消し、`Cancel`メソッド。

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

## <a name="platform-differences"></a>プラットフォームの違い

# <a name="androidtabandroid"></a>[Android](#tab/android)

プラットフォームの違いはありません。

# <a name="iostabios"></a>[iOS](#tab/ios)

* 「バイブレーション リング」を設定すると、デバイスを振動のみです。
* 常に 500 ミリ秒の振動します。
* 振動をキャンセルすることはできません。

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

プラットフォームの違いはありません。

-----

## <a name="api"></a>API

- [振動のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Vibration)
- [振動 API ドキュメント](xref:Xamarin.Essentials.Vibration)
