---
title: 指紋認証の使用を開始する
ms.prod: xamarin
ms.assetid: 7BACCB36-8E3E-4E5D-B8EF-56A639839FD2
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/17/2018
ms.openlocfilehash: 746a096f93036e63b29bc917826259f88426cead
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "73020278"
---
# <a name="getting-started-with-fingerprint-authentication"></a>指紋認証の使用を開始する

はじめに、アプリケーションで指紋認証を使用できるように Xamarin.Android プロジェクトを構成する方法について説明します。

1. **AndroidManifest.xml** を更新して、指紋 API で必要なアクセス許可を宣言します。
2. `FingerprintManager` への参照を取得します。
3. デバイスが指紋スキャンに対応していることを確認します。

## <a name="requesting-permissions-in-the-application-manifest"></a>アプリケーション マニフェストでアクセス許可を要求する

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Android アプリケーションでは、マニフェストで `USE_FINGERPRINT` アクセス許可を要求する必要があります。 次のスクリーンショットに、Visual Studio でアプリケーションにこのアクセス許可を追加する方法を示します。

[![[Android マニフェスト] 画面での [USE\_FINGERPRINT] の有効化](get-started-images/fingerprint-01-vs.png)](get-started-images/fingerprint-01-vs.png#lightbox) 

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

Android アプリケーションでは、マニフェストで `USE_FINGERPRINT` アクセス許可を要求する必要があります。 次のスクリーンショットに、Visual Studio for Mac でアプリケーションにこのアクセス許可を追加する方法を示します。

[![[Android アプリケーション] 画面での UseFingerprint の有効化](get-started-images/fingerprint-01-xs.png)](get-started-images/fingerprint-01-xs.png#lightbox) 

-----

## <a name="getting-an-instance-of-the-fingerprintmanager"></a>FingerprintManager のインスタンスを取得する

次に、アプリケーションで `FingerprintManager` または `FingerprintManagerCompat` クラスのインスタンスを取得する必要があります。 Android の以前のバージョンとの互換性を維持するため、Android アプリケーションでは、Android Support v4 NuGet パッケージに含まれている互換性 API を使用する必要があります。 次のスニペットで、オペレーティング システムから適切なオブジェクトを取得する方法を示します。 

```csharp
// Using the Android Support Library v4
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);

// Using API level 23:
FingerprintManager fingerprintManager = context.GetSystemService(Context.FingerprintService) as FingerprintManager;
```  

前のスニペットの `context` は任意の Android `Android.Content.Context` です。 通常、これは、認証を実行するアクティビティです。

## <a name="checking-for-eligibility"></a>資格の確認

アプリケーションでは、指紋認証を使用できることを確認するために、さまざまなチェックを実行する必要があります。 アプリケーションで資格を確認するために使用される条件は、全部で 5 つあります。  

**API レベル 23** &ndash; 指紋 API では、API レベル 23 以降が必要です。 `FingerprintManagerCompat` クラスによって、API レベルのチェックが自動的にラップされます。 このため、**Android Support Library v4** と `FingerprintManagerCompat` を使用することをお勧めします。これにより、これらのチェックの 1 つが構成されます。

**ハードウェア** &ndash; アプリケーションの初回の起動時に、指紋スキャナーの存在を確認する必要があります。

```csharp
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);
if (!fingerprintManager.IsHardwareDetected)
{
    // Code omitted
}
```

**デバイスがセキュリティで保護されている** &ndash; デバイスが画面ロックを使用してユーザーによって保護されている必要があります。 デバイスが画面ロックを使用してユーザーによって保護されていないときに、アプリケーションにとってセキュリティが重要である場合は、画面ロックを構成する必要があることをユーザーに通知する必要があります。 次のコード スニペットに、この前提条件を確認する方法を示します。

```csharp
KeyguardManager keyguardManager = (KeyguardManager) GetSystemService(KeyguardService);
if (!keyguardManager.IsKeyguardSecure)
{
}
```

**登録されている指紋** &ndash; 少なくとも 1 つの指紋がユーザーによってオペレーティング システムに登録されている必要があります。 このアクセス許可の確認は、認証が試行されるときに毎回実行される必要があります。

```csharp
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);
if (!fingerprintManager.HasEnrolledFingerprints)
{
    // Can't use fingerprint authentication - notify the user that they need to
    // enroll at least one fingerprint with the device.
}
```

**アクセス許可** &ndash; アプリケーションでは、アプリケーションが使用される前にユーザーにアクセス許可を要求する必要があります。 Android 5.0 以降では、ユーザーは、アプリをインストールする条件としてアクセス許可を付与します。 Android 6.0 では、実行時にアクセス許可をチェックする新しいアクセス許可モデルが導入されています。 次のコード スニペットは、Android 6.0 でアクセス許可を確認する方法の例です。

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

アプリケーションで認証オプションが提供されるたびにこれらの条件をすべて確認することで、ユーザーが確実に最適なユーザーエクスペリエンスを得ることができます。 デバイスまたはオペレーティング システムを変更またはアップグレードすると、指紋認証の可用性に影響を与える可能性があります。 これらのチェックの結果をキャッシュすることを選択した場合は、アップグレード シナリオに対応していることを確認してください。

Android 6.0 でアクセス許可を要求する方法の詳細については、Android のガイドの「[実行時にアプリの権限をリクエストする](https://developer.android.com/training/permissions/requesting.html)」を参照してください。

## <a name="related-links"></a>関連リンク

- [コンテキスト](xref:Android.Content.Context)
- [KeyguardManager](xref:Android.App.KeyguardManager)
- [KeyguardManager](https://developer.android.com/reference/android/support/v4/content/ContextCompat)
- [FingerprintManager](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html)
- [FingerprintManagerCompat](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)
- [実行時にアプリの権限をリクエストする](https://developer.android.com/training/permissions/requesting.html)
