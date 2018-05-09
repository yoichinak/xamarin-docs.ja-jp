---
title: Xamarin.Essentials 振動
description: 振動クラスを起動し、必要な時間を vibrate 機能を停止することができます。
ms.assetid: 7E8B24C4-2625-4DAE-A129-383542D34F1E
ms.technology: xamarin-crossplatform
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: e0358b95dbed5dc2bc6cb087a458034845cb8487
ms.sourcegitcommit: 46d3c9daa45350bdd536d9e105517f3c1c753c5b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/07/2018
---
# <a name="xamarinessentials-vibration"></a>Xamarin.Essentials 振動

![プレリリース NuGet](~/media/shared/pre-release.png)

**振動**クラスを起動し、必要な時間を vibrate 機能を停止することができます。

## <a name="getting-started"></a>作業の開始

アクセスする、**振動**次のプラットフォーム固有のセットアップの機能が必要です。

# <a name="androidtabandroid"></a>[Android](#tab/android)

Vibrate 権限は、必要なし、Android プロジェクトで構成する必要があります。 これは、次の方法で追加できます。

開く、 **AssemblyInfo.cs**下にあるファイル、**プロパティ**フォルダーを追加。

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.Vibrate)]
```

または、Android のマニフェストを更新します。

開く、 **AndroidManifest.xml**下にあるファイル、**プロパティ**フォルダー内の次の追加と、**マニフェスト**ノード。

```xml
<uses-permission android:name="android.permission.VIBRATE" />
```

または、Anroid プロジェクトを右クリックし、プロジェクトのプロパティを開きます。 **Android マニフェスト**検索、**権限が必要:** 領域とチェック、 **VIBRATE**権限です。 これは自動的に更新、 **AndroidManifest.xml**ファイル。

# <a name="iostabios"></a>[iOS](#tab/ios)

追加の設定が必要です。

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

追加の設定が必要です。

-----

## <a name="using-vibration"></a>振動を使用します。

クラスの Xamarin.Essentials への参照を追加します。

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

デバイス振動の取り消しを要求することができます、`Cancel`メソッド。

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

| プラットフォーム | 相違点 |
| --- | --- |
| iOS | 常に、500 ミリ秒の振動したりします。 |
| iOS | 振動をキャンセルすることはできません。 |

## <a name="api"></a>API

- [振動ソース コード](https://github.com/xamarin/Essentials/tree/master/Essentials/Vibration)
- [振動 API ドキュメント](xref:Xamarin.Essentials.Vibration)
