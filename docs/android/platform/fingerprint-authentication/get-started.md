---
title: 指紋認証の概要
ms.prod: xamarin
ms.assetid: 7BACCB36-8E3E-4E5D-B8EF-56A639839FD2
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/08/2018
ms.openlocfilehash: 70d27ef3d7518619a246c25aac128b2fd1ed70c5
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
ms.locfileid: "30764399"
---
# <a name="getting-started-with-fingerprint-authentication"></a>指紋認証の概要

最初に、最初に説明 Xamarin.Android プロジェクトを構成して、アプリケーションは、指紋認証を使用することができるようにする方法。

1. 更新**AndroidManifest.xml**指紋 Api を必要とするアクセス許可を宣言します。
2. 参照を取得、`FingerprintManager`です。
3. デバイスが指紋をスキャンできることを確認してください。

## <a name="requesting-permissions-in-the-application-manifest"></a>アプリケーションのアクセス許可の要求のマニフェストします。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Android アプリケーションを要求する必要があります、`USE_FINGERPRINT`マニフェストにアクセスを許可します。 次のスクリーン ショットは、Visual Studio 2015 のアプリケーションにこのアクセス許可を追加する方法を示します。

[![使用を有効にする\_Android マニフェスト 画面で指紋](get-started-images/fingerprint-01-vs.png)](get-started-images/fingerprint-01-vs.png#lightbox) 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Android アプリケーションを要求する必要があります、`USE_FINGERPRINT`マニフェストにアクセスを許可します。 次のスクリーン ショットは、Mac を Visual Studio でのアプリケーションにこのアクセス許可を追加する方法を示しています。

[![Android アプリケーションの画面で UseFingerprint を有効にします。](get-started-images/fingerprint-01-xs.png)](get-started-images/fingerprint-01-xs.png#lightbox) 

-----

## <a name="getting-an-instance-of-the-fingerprintmanager"></a>FingerprintManager のインスタンスを取得します。

次に、アプリケーションがのインスタンスを取得する必要があります、`FingerprintManager`または`FingerprintManagerCompat`クラスです。 Android アプリケーションが API との互換性を使用して Android の旧バージョンと互換性が Android サポート v4 NuGet パッケージに見つかりませんでした。 次のスニペットでは、オペレーティング システムから適切なオブジェクトを取得する方法を示しています。 

```csharp
// Using the Android Support Library v4
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);

// Using API level 23:
FingerprintManager fingerprintManager = context.GetSystemService(Context.FingerprintService) as FingerprintManager;
```  

前のスニペットに、`context`は任意の Android`Android.Content.Context`です。 通常これが、認証を実行するアクティビティです。

## <a name="checking-for-eligibility"></a>適格性のチェック

アプリケーションでは、指紋認証を使用することであることを確認するいくつかのチェックを実行する必要があります。 合計では、アプリケーションが適格性の確認に使用する 5 つの条件があります。  
 

**API レベル 23** &ndash;指紋 Api API level 23 以上を必要とします。 `FingerprintManagerCompat`クラスは、の API レベル チェックをラップします。 使用することをお勧めはこのため、 **Android のサポート ライブラリ v4**と`FingerprintManagerCompat`; これらのチェックのいずれかのアカウントがこれです。

**ハードウェア**&ndash;が最初に、アプリケーションの起動時、指紋スキャナーの存在を確認する必要があります。

```csharp
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);
if (!fingerprintManager.IsHardwareDetected)
{
    // Code omitted
}
```
    
**デバイスがセキュリティで保護された**&ndash;画面のロックで保護されているデバイスが必要です。 ユーザーが画面のロックを使用してデバイスをセキュリティで保護されません、セキュリティは、アプリケーションに重要な場合は、ユーザーに通知する画面のロックを構成する必要があります。 次のコード スニペットは、この前 requiste をチェックする方法を示します。

```csharp
KeyguardManager keyguardManager = (KeyguardManager) GetSystemService(KeyguardService);
if (!keyguardManager.IsKeyguardSecure)
{
}
```

**指紋を登録した**&ndash;オペレーティング システムに登録されている、少なくとも 1 つの指紋が必要です。 このアクセス許可のチェックは、各認証試行する前に発生する必要があります。

```csharp
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);
if (!fingerprintManager.HasEnrolledFingerprints)
{
    // Can't use fingerprint authentication - notify the user that they need to
    // enroll at least one fingerprint with the device.
}
```

**アクセス許可**&ndash;アプリケーションは、アプリケーションを使用する前に、ユーザーからのアクセス許可を要求する必要があります。 Android 5.0 および下は、ユーザーは、アプリをインストールするを条件としての権限を付与します。 Android 6.0 には、実行時に権限をチェックする新しいアクセス許可モデルが導入されました。 このコード スニペットは、Android 6.0 でのアクセス許可を確認する方法の例を示します。

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
    // http://developer.android.com/training/permissions/requesting.html
}
```

アプリケーションは、指紋認証を使用する必要があるたびに 3、4、および 5 の条件を確認する必要があります。 デバイスと結果の保存に初めてアプリケーションを実行、最初の 2 つの条件をチェックすることができます (共有の基本設定など)。

Android 6.0 でアクセス許可を要求する方法の詳細については、Android のガイドを参照してください。[実行時にアクセス許可の要求](http://developer.android.com/training/permissions/requesting.html)です。



## <a name="related-links"></a>関連リンク

- [コンテキスト](https://developer.xamarin.com/api/type/Android.Content.Context/)
- [ContextCompat](https://developer.xamarin.com/api/type/Android.Support.V4.Content.ContextCompat/)
- [KeyguardManager](https://developer.xamarin.com/api/type/Android.App.KeyguardManager/)
- [FingerprintManager](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html)
- [FingerprintManagerCompat](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)
- [実行時にアクセス許可を要求します。](http://developer.android.com/training/permissions/requesting.html)
