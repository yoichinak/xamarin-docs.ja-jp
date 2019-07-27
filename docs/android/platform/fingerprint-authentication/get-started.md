---
title: 指紋認証を使用したはじめに
ms.prod: xamarin
ms.assetid: 7BACCB36-8E3E-4E5D-B8EF-56A639839FD2
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/17/2018
ms.openlocfilehash: c433d4d7920b024795e2e8344b452e25d8f58cf4
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510644"
---
# <a name="getting-started-with-fingerprint-authentication"></a>指紋認証を使用したはじめに

まず、アプリケーションが指紋認証を使用できるように Xamarin Android プロジェクトを構成する方法について説明します。

1. 指紋 Api が必要とするアクセス許可を**宣言するよう**に、を更新します。
2. への`FingerprintManager`参照を取得します。
3. デバイスが指紋スキャンに対応していることを確認します。

## <a name="requesting-permissions-in-the-application-manifest"></a>アプリケーションマニフェストでアクセス許可を要求する

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Android アプリケーションは、マニフェストに`USE_FINGERPRINT`アクセス許可を要求する必要があります。 次のスクリーンショットは、Visual Studio でアプリケーションにこのアクセス許可を追加する方法を示しています。

[![Android マニフェスト\_画面での使用指紋の有効化](get-started-images/fingerprint-01-vs.png)](get-started-images/fingerprint-01-vs.png#lightbox) 

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Android アプリケーションは、マニフェストに`USE_FINGERPRINT`アクセス許可を要求する必要があります。 次のスクリーンショットは、Visual Studio for Mac でこのアクセス許可をアプリケーションに追加する方法を示しています。

[![Android アプリケーション画面での UseFingerprint の有効化](get-started-images/fingerprint-01-xs.png)](get-started-images/fingerprint-01-xs.png#lightbox) 

-----

## <a name="getting-an-instance-of-the-fingerprintmanager"></a>FingerprintManager のインスタンスを取得する

次に、アプリケーションは`FingerprintManager` `FingerprintManagerCompat`またはクラスのインスタンスを取得する必要があります。 Android の旧バージョンとの互換性を維持するには、android アプリケーションで、Android サポート v4 NuGet パッケージに含まれている互換性 API を使用する必要があります。 次のスニペットは、オペレーティングシステムから適切なオブジェクトを取得する方法を示しています。 

```csharp
// Using the Android Support Library v4
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);

// Using API level 23:
FingerprintManager fingerprintManager = context.GetSystemService(Context.FingerprintService) as FingerprintManager;
```  

前のスニペット`context`では、は Android `Android.Content.Context`です。 通常、これは認証を実行するアクティビティです。

## <a name="checking-for-eligibility"></a>適格性を確認しています

アプリケーションでは、指紋認証を使用できることを確認するために、いくつかのチェックを実行する必要があります。 アプリケーションが資格を確認するために使用する条件は合計で5つあります。  

**API レベル 23**&ndash;指紋 api では、api レベルが23以上である必要があります。 クラス`FingerprintManagerCompat`は、API レベルのチェックをラップします。 このため、 **Android サポートライブラリ v4**と`FingerprintManagerCompat`を使用することをお勧めします。これにより、これらのチェックのいずれかが考慮されます。

**ハードウェア**&ndash;アプリケーションを初めて起動するときは、指紋スキャナーが存在するかどうかを確認する必要があります。

```csharp
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);
if (!fingerprintManager.IsHardwareDetected)
{
    // Code omitted
}
```

**デバイスはセキュリティで保護されてい**ます&ndash;ユーザーは、デバイスが画面ロックで保護されている必要があります。 ユーザーが画面ロックを使用してデバイスをセキュリティで保護していない場合、セキュリティがアプリケーションにとって重要である場合は、画面ロックを構成する必要があることをユーザーに通知する必要があります。 次のコードスニペットは、この事前 requiste を確認する方法を示しています。

```csharp
KeyguardManager keyguardManager = (KeyguardManager) GetSystemService(KeyguardService);
if (!keyguardManager.IsKeyguardSecure)
{
}
```

**登録**された指紋&ndash;ユーザーには、オペレーティングシステムに少なくとも1つの指紋が登録されている必要があります。 このアクセス許可の確認は、各認証を試行する前に行われる必要があります。

```csharp
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);
if (!fingerprintManager.HasEnrolledFingerprints)
{
    // Can't use fingerprint authentication - notify the user that they need to
    // enroll at least one fingerprint with the device.
}
```

**アクセス許可**&ndash;アプリケーションを使用する前に、ユーザーにアクセス許可を要求する必要があります。 Android 5.0 以降では、ユーザーはアプリをインストールする条件としてアクセス許可を付与します。 Android 6.0 では、実行時にアクセス許可をチェックする新しいアクセス許可モデルが導入されました。 次のコードスニペットは、Android 6.0 でアクセス許可を確認する方法の例です。

```csharp
// The context is typically a reference to the current activity.
Android.Content.PM.Permission permissionResult = ContextCompat.CheckSelfPermission(context, Manifest.Permission.UseFingerprint);
if (permissionResult == Android.Content.PM.Permission.Granted)
{
    // Permission granted - go ahead and start the fingerprint scanner.
}
else
{
    // No permission. Go and ask for permissions and don't start the scanner. See
    // https://developer.android.com/training/permissions/requesting.html
}
```

アプリケーションが認証オプションを提供するたびにこれらの条件をすべて確認することで、ユーザーが最適なユーザーエクスペリエンスを得られるようになります。 デバイスまたはオペレーティングシステムへの変更またはアップグレードは、指紋認証の可用性に影響を与える可能性があります。 これらのチェックの結果をキャッシュすることを選択した場合は、アップグレードのシナリオに対応していることを確認してください。

Android 6.0 でアクセス許可を要求する方法の詳細については、[実行時にアクセス許可](https://developer.android.com/training/permissions/requesting.html)を要求する android ガイドを参照してください。

## <a name="related-links"></a>関連リンク

- [関連](xref:Android.Content.Context)
- [KeyguardManager](xref:Android.App.KeyguardManager)
- [ContextCompat](https://developer.android.com/reference/android/support/v4/content/ContextCompat)
- [FingerprintManager](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html)
- [FingerprintManagerCompat](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)
- [実行時にアクセス許可を要求する](https://developer.android.com/training/permissions/requesting.html)
