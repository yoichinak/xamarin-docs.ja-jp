---
title: Xamarin.Essentials 懐中
description: 懐中電灯クラスをオンまたはオフ、デバイスのカメラにする場合は、懐中電灯をフラッシュする権限を持ちます。
ms.assetid: 06A03553-D212-43A2-9E6E-C2D2D93EB136
ms.technology: xamarin-crossplatform
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 3d867d742314f2677eeab35bbc354f696c99bf75
ms.sourcegitcommit: 46d3c9daa45350bdd536d9e105517f3c1c753c5b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/07/2018
---
# <a name="xamarinessentials-flashlight"></a>Xamarin.Essentials 懐中

![プレリリース NuGet](~/media/shared/pre-release.png)

**懐中電灯**クラスをオンまたはオフ、デバイスのカメラにする場合は、懐中電灯をフラッシュする権限を持ちます。

## <a name="getting-started"></a>作業の開始

アクセスする、**懐中電灯**次のプラットフォーム固有のセットアップの機能が必要です。

# <a name="androidtabandroid"></a>[Android](#tab/android)

懐中電灯とカメラのアクセス許可は、必要な Android プロジェクトで構成する必要があります。 これは、次の方法で追加できます。

開く、 **AssemblyInfo.cs**下にあるファイル、**プロパティ**フォルダーを追加。

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.Flashlight)]
[assembly: UsesPermission(Android.Manifest.Permission.Camera)]
```

または、Android のマニフェストを更新します。

開く、 **AndroidManifest.xml**下にあるファイル、**プロパティ**フォルダー内の次の追加と、**マニフェスト**ノード。

```xml
<uses-permission android:name="android.permission.FLASHLIGHT" />
<uses-permission android:name="android.permission.CAMERA" />
```

または、Anroid プロジェクトを右クリックし、プロジェクトのプロパティを開きます。 **Android マニフェスト**検索、**権限が必要:** 領域とチェック、**懐中電灯**と**カメラ**アクセス許可。 これは自動的に更新、 **AndroidManifest.xml**ファイル。

これらのアクセス許可を追加することによって[Google Play が自動的にフィルタで除外デバイス](http://developer.android.com/guide/topics/manifest/uses-feature-element.html#permissions-features)特定のハードウェアなし。 この問題を回避、Android プロジェクトの AssemblyInfo.cs ファイルに、次を追加することによって取得できます。

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

クラスの Xamarin.Essentials への参照を追加します。

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

### <a name="androidtabandroid-specifics"></a>[Android](#tab/android-specifics)

懐中電灯クラスは、デバイスのオペレーティング システムに基づく optmized されました。

#### <a name="api-level-23-and-higher"></a>API レベル 23 以上

新しい API レベルで[たいまつモード](https://developer.android.com/reference/android/hardware/camera2/CameraManager.html#setTorchMode)オンまたはオフ、デバイスのフラッシュ単位に使用されます。

#### <a name="api-level-22-and-lower"></a>API レベル 22 と下限

オンまたはオフにするカメラ表面のテクスチャが作成、`FlashMode`カメラ単位のです。 

### <a name="iostabios-specifics"></a>[iOS](#tab/ios-specifics)

[AVCaptureDevice](https://developer.xamarin.com/api/type/AVFoundation.AVCaptureDevice/)オンまたはたいまつとデバイスのフラッシュのモードをオフにするために使用します。

### <a name="uwptabuwp-specifics"></a>[UWP](#tab/uwp-specifics)

[ランプ](https://docs.microsoft.com/en-us/uwp/api/windows.devices.lights.lamp)オンまたはオフにするデバイスの背面の最初のランプを検出するために使用します。

-----

## <a name="api"></a>API

- [懐中電灯ソース コード](https://github.com/xamarin/Essentials/tree/master/Essentials/Flashlight)
- [懐中 API ドキュメント](xref:Xamarin.Essentials.Flashlight)
