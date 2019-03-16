---
title: 指紋認証の概要
ms.prod: xamarin
ms.assetid: 7BACCB36-8E3E-4E5D-B8EF-56A639839FD2
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/17/2018
ms.openlocfilehash: 3082dfcd6d0ffbc6404a89a10819e60b57b9c61c
ms.sourcegitcommit: 650458de1d362cd7de174cacef7838f0e74426f3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2019
ms.locfileid: "58070710"
---
# <a name="getting-started-with-fingerprint-authentication"></a>指紋認証の概要

最初に、最初に説明、アプリケーションは、指紋認証を使用することができるように、Xamarin.Android プロジェクトを構成する方法。

1. Update **AndroidManifest.xml**指紋 Api が必要なアクセス許可を宣言します。
2. 参照を取得、`FingerprintManager`します。
3. デバイスが指紋をスキャンできることを確認します。

## <a name="requesting-permissions-in-the-application-manifest"></a>マニフェストのアプリケーションのアクセス許可の要求

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Android アプリケーションを要求する必要があります、`USE_FINGERPRINT`マニフェストにアクセスを許可します。 次のスクリーン ショットは、Visual Studio でアプリケーションにこのアクセス許可を追加する方法を示しています。

[![使用を有効にする\_[Android マニフェスト] 画面では、フィンガー プリント](get-started-images/fingerprint-01-vs.png)](get-started-images/fingerprint-01-vs.png#lightbox) 

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Android アプリケーションを要求する必要があります、`USE_FINGERPRINT`マニフェストにアクセスを許可します。 次のスクリーン ショットは、Visual studio for Mac のアプリケーションにこのアクセス許可を追加する方法を示しています。

[![Android アプリケーションの画面で UseFingerprint を有効にします。](get-started-images/fingerprint-01-xs.png)](get-started-images/fingerprint-01-xs.png#lightbox) 

-----

## <a name="getting-an-instance-of-the-fingerprintmanager"></a>FingerprintManager のインスタンスを取得します。

次に、アプリケーションがのインスタンスを取得する必要があります、`FingerprintManager`または`FingerprintManagerCompat`クラス。 Android の以前のバージョンと互換性がある、Android アプリケーションが互換性 API を使用する必要がありますが、Android サポート v4 の NuGet パッケージに見つかりませんでした。 次のスニペットでは、オペレーティング システムから適切なオブジェクトを取得する方法を示しています。 

```csharp
// Using the Android Support Library v4
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);

// Using API level 23:
FingerprintManager fingerprintManager = context.GetSystemService(Context.FingerprintService) as FingerprintManager;
```  

前のスニペットで、`context`は任意の Android`Android.Content.Context`します。 通常これは認証を実行するアクティビティです。

## <a name="checking-for-eligibility"></a>適格性のチェック

アプリケーションでは、指紋認証を使用することであることを確認するいくつかのチェックを実行する必要があります。 合計では、アプリケーションが適格性の確認に使用する 5 つの条件があります。  

**API レベル 23** &ndash;指紋 Api が API レベル 23 以上が必要です。 `FingerprintManagerCompat`クラスでの API レベルのチェックはラップします。 このためが使用することが推奨、 **Android サポート ライブラリ v4**と`FingerprintManagerCompat`; これらのチェックのいずれかのアカウントはこれです。

**ハードウェア**&ndash;最初に、アプリケーションの起動時、指紋スキャナーの存在を確認する必要があります。

```csharp
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);
if (!fingerprintManager.IsHardwareDetected)
{
    // Code omitted
}
```

**デバイスがセキュリティで保護された**&ndash;ユーザー デバイスの画面のロックで保護されている必要があります。 ユーザーが画面のロックを使用してデバイスをセキュリティで保護しないと、セキュリティは、アプリケーションに重要なし、ユーザーに通知する画面のロックを構成する必要があります。 次のコード スニペットでは、この前の前提条件としてをチェックする方法を示します。

```csharp
KeyguardManager keyguardManager = (KeyguardManager) GetSystemService(KeyguardService);
if (!keyguardManager.IsKeyguardSecure)
{
}
```

**指紋の登録**&ndash;ユーザーは、オペレーティング システムに登録されている少なくとも 1 つのフィンガー プリントをいる必要があります。 このアクセス許可のチェックは、各認証試行する前に発生する必要があります。

```csharp
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);
if (!fingerprintManager.HasEnrolledFingerprints)
{
    // Can't use fingerprint authentication - notify the user that they need to
    // enroll at least one fingerprint with the device.
}
```

**アクセス許可**&ndash;アプリケーションは、アプリケーションを使用する前に、ユーザーからのアクセス許可を要求する必要があります。 Android 5.0 と下は、ユーザーは、アプリのインストールの条件としての権限を付与します。 Android 6.0 には、実行時に権限をチェックしますが、新しいアクセス許可モデルが導入されました。 このコード スニペットでは、Android 6.0 でのアクセス許可を確認する方法の例を示します。

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

すべてのたびに、アプリケーションでは、認証オプションはにより、ユーザーは、最適なユーザー エクスペリエンスを取得します。 これらの条件を確認しています。 変更や、デバイスまたはオペレーティング システムへのアップグレードは、指紋認証の可用性に影響可能性があります。 これらのチェックのいずれかの結果をキャッシュすることを選択する場合は、アップグレード シナリオに対応するために確認します。

Android 6.0 でのアクセス許可を要求する方法の詳細については、Android」ガイドを参照してください。[実行時にアクセス許可の要求](https://developer.android.com/training/permissions/requesting.html)します。

## <a name="related-links"></a>関連リンク

- [コンテキスト](https://developer.xamarin.com/api/type/Android.Content.Context/)
- [ContextCompat](https://developer.xamarin.com/api/type/Android.Support.V4.Content.ContextCompat/)
- [KeyguardManager](https://developer.xamarin.com/api/type/Android.App.KeyguardManager/)
- [FingerprintManager](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html)
- [FingerprintManagerCompat](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)
- [実行時にアクセス許可の要求](https://developer.android.com/training/permissions/requesting.html)
