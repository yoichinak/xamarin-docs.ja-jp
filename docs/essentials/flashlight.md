---
title: 'Xamarin.Essentials: 懐中電灯'
description: このドキュメントでは、懐中電灯化フラッシュ オンまたはオフ、デバイスのカメラを有効にすることのできる、Xamarin.Essentials で懐中電灯クラスについて説明します。
ms.assetid: 06A03553-D212-43A2-9E6E-C2D2D93EB136
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 8c471f64c14a2e41693c450e02f89e7ac845d060
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353361"
---
# <a name="xamarinessentials-flashlight"></a>Xamarin.Essentials: 懐中電灯

![NuGet にプレリリースします。](~/media/shared/pre-release.png)

**懐中電灯**クラスには、懐中電灯化フラッシュ オンまたはオフ、デバイスのカメラを有効にする機能。

## <a name="getting-started"></a>作業の開始

アクセスする、**懐中電灯**次のプラットフォーム固有設定の機能が必要です。

# <a name="androidtabandroid"></a>[Android](#tab/android)

フラッシュ ライトとカメラのアクセス許可は、必要な Android プロジェクトで構成する必要があります。 これは、次の方法で追加できます。

開く、 **AssemblyInfo.cs**ファイル、**プロパティ**フォルダーを追加。

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.Flashlight)]
[assembly: UsesPermission(Android.Manifest.Permission.Camera)]
```

または、Android マニフェストを更新します。

開く、 **AndroidManifest.xml**ファイル、**プロパティ**フォルダー内の次の追加と、**マニフェスト**ノード。

```xml
<uses-permission android:name="android.permission.FLASHLIGHT" />
<uses-permission android:name="android.permission.CAMERA" />
```

または、Android プロジェクトを右クリックし、プロジェクトのプロパティを開きます。 **Android マニフェスト**検索、**ために必要なアクセス許可:** 領域とチェック、**懐中電灯**と**カメラ**アクセス許可。 これは自動的に更新、 **AndroidManifest.xml**ファイル。

これらのアクセス許可を追加することで[Google Play がデバイスを自動的にフィルター処理](http://developer.android.com/guide/topics/manifest/uses-feature-element.html#permissions-features)特定のハードウェアなし。 この問題を回避を Android プロジェクトで AssemblyInfo.cs ファイルに、次を追加することで取得できます。

```csharp
[assembly: UsesFeature("android.hardware.camera", Required = false)]
[assembly: UsesFeature("android.hardware.camera.autofocus", Required = false)]
```

# <a name="iostabios"></a>[iOS](#tab/ios)

追加の設定が必要です。

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

追加の設定が必要です。

-----

## <a name="using-flashlight"></a>懐中電灯を使用します。

クラスで Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

懐中電灯オンとオフを経由にする、`TurnOnAsync`と`TurnOffAsync`メソッド。

```csharp
try
{
    // Turn On
    await Flashlight.TurnOnAsync();

    // Turn Off
    await Flashlight.TurnOffAsync();
}
catch (FeatureNotSupportedException fnsEx)
{
    // Handle not supported on device exception
}
catch (PermissionException pEx)
{
    // Handle permission exception
}
catch (Exception ex)
{
    // Unable to turn on/off flashlight
}
```

## <a name="platform-implementation-specifics"></a>プラットフォームの実装の詳細

### <a name="androidtabandroid"></a>[Android](#tab/android)

懐中電灯クラスは、デバイスのオペレーティング システムに基づく最適化されています。

#### <a name="api-level-23-and-higher"></a>API レベル 23 以上

新しい API レベルで[Torch モード](https://developer.android.com/reference/android/hardware/camera2/CameraManager.html#setTorchMode)はオン/オフ、デバイスのフラッシュ単位に使用されます。

#### <a name="api-level-22-and-lower"></a>API レベル 22 と下限

オンまたはオフにするカメラの表面のテクスチャが作成、`FlashMode`カメラ単位。 

### <a name="iostabios"></a>[iOS](#tab/ios)

[AVCaptureDevice](https://developer.xamarin.com/api/type/AVFoundation.AVCaptureDevice/)オンとオフ、トーチとデバイスのモードをフラッシュするために使用します。

### <a name="uwptabuwp"></a>[UWP](#tab/uwp)

[Lamp](https://docs.microsoft.com/en-us/uwp/api/windows.devices.lights.lamp)最初に lamp をオンまたはオフにするデバイスの背面を検出するために使用します。

-----

## <a name="api"></a>API

- [懐中電灯ソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Flashlight)
- [懐中電灯 API ドキュメント](xref:Xamarin.Essentials.Flashlight)
