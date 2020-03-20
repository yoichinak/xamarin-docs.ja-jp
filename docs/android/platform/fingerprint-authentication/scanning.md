---
title: 指紋のスキャン
ms.prod: xamarin
ms.assetid: 1CDDC096-77E0-47B3-BE0B-8953E2DDACD3
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/23/2016
ms.openlocfilehash: 61edd0e4b532f18a8fc28502e5bb990703068776
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/10/2020
ms.locfileid: "73027489"
---
# <a name="scanning-for-fingerprints"></a>指紋のスキャン

指紋認証を使用するための Xamarin.Android アプリケーションの準備方法がわかったので、次に、`FingerprintManager.Authenticate` メソッドに戻り、Android 6.0 の指紋認証でのその場所について説明します。 次の一覧では、指紋認証のワークフローの概要を説明します。

1. `FingerprintManager.Authenticate` を呼び出し、`CryptoObject` と `FingerprintManager.AuthenticationCallback` のインスタンスを渡します。 `CryptoObject` を使用して、指紋認証の結果が改ざんされていないことを確認します。 
2. [FingerprintManager.AuthenticationCallback](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html) クラスをサブクラス化します。 指紋認証を開始するときに、このクラスのインスタンスを `FingerprintManager` に提供します。 指紋スキャナーが終了すると、このクラスのコールバック メソッドの 1 つが呼び出されます。
3. デバイスで指紋スキャナーが開始され、ユーザーの操作を待機していることをユーザーに通知するように、UI を更新するコードを記述します。 
4. 指紋スキャナーが完了すると、前のステップで提供した `FingerprintManager.AuthenticationCallback` インスタンスのメソッドを呼び出すことにより、結果がアプリケーションに返されます。
5. アプリケーションで、指紋認証の結果をユーザーに通知し、必要に応じて結果に対応します。 

次のコード スニペットは、指紋のスキャンを開始するアクティビティ内のメソッドの例です。

```csharp
protected void FingerPrintAuthenticationExample()
{
    const int flags = 0; /* always zero (0) */

    // CryptoObjectHelper is described in the previous section.
    CryptoObjectHelper cryptoHelper = new CryptoObjectHelper();    
    
    // cancellationSignal can be used to manually stop the fingerprint scanner. 
    cancellationSignal = new Android.Support.V4.OS.CancellationSignal();
    
    FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(this);
    
    // AuthenticationCallback is a base class that will be covered later on in this guide.
    FingerprintManagerCompat.AuthenticationCallback authenticationCallback = new MyAuthCallbackSample(this);

    // Start the fingerprint scanner.
    fingerprintManager.Authenticate(cryptoHelper.BuildCryptoObject(), flags, cancellationSignal, authenticationCallback, null);
}
```

`Authenticate` メソッドの各パラメーターについてもう少し詳しく説明します。

- 最初のパラメーターは、指紋スキャンの結果を認証するために指紋スキャナーで使用される _crypto_ オブジェクトです。 このオブジェクトが `null` の場合、アプリケーションでは、指紋の結果が改ざんされていないことを無条件に信頼する必要があります。 null ではなく、`CryptoObject` をインスタンス化して `FingerprintManager` に提供することをお勧めします。 「[CryptObject の作成](~/android/platform/fingerprint-authentication/creating-a-cryptoobject.md)」では、`Cipher` に基づいて `CryptoObject` をインスタンス化する方法について詳しく説明します。
- 2 番目のパラメーターは常に 0 です。 Android のドキュメントでは、これはフラグのセットと説明されており、おそらくは将来使用するために予約されています。 
- 3 番目のパラメーター `cancellationSignal` は、指紋スキャナーをオフにして現在の要求をキャンセルするために使用されるオブジェクトです。 これは [Android の CancellationSignal](https://developer.android.com/reference/android/os/CancellationSignal.html) であり、.NET Framework の型ではありません。
- 4 番目のパラメーターは必須であり、`AuthenticationCallback` 抽象クラスをサブクラス化するクラスです。 このクラスのメソッドは、`FingerprintManager` が終了したこと、および結果をクライアントに通知するために呼び出されます。 `AuthenticationCallback` の実装については理解することがたくさんあるので、[独立したセクション](~/android/platform/fingerprint-authentication/fingerprint-authentication-callbacks.md)で説明します。
- 5 番目のパラメーターは、省略可能な `Handler` インスタンスです。 `Handler` オブジェクトを提供すると、`FingerprintManager` では、指紋ハードウェアからのメッセージを処理するときに、そのオブジェクトの `Looper` が使用されます。 通常は、`Handler` を提供する必要はなく、FingerprintManager ではアプリケーションの `Looper` が使用されます。

## <a name="cancelling-a-fingerprint-scan"></a>指紋のスキャンを取り消す

ユーザー (またはアプリケーション) が、開始後に指紋スキャンを取り消すことが必要になる場合があります。 このような場合は、指紋スキャンを開始するための呼び出し時に `FingerprintManager.Authenticate` に提供された [`CancellationSignal`](https://developer.android.com/reference/android/os/CancellationSignal.html) で [`IsCancelled`](https://developer.android.com/reference/android/os/CancellationSignal.html#isCanceled()) メソッドを呼び出します。

`Authenticate` メソッドの重要なパラメーターについてさらに詳しく説明します。 最初の「[認証コールバックへの応答](~/android/platform/fingerprint-authentication/fingerprint-authentication-callbacks.md)」では、[FingerprintManager.AuthenticationCallback](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html) をサブクラス化し、指紋スキャナーから提供された結果に Android アプリケーションで対応できるようにする方法を説明します。

## <a name="related-links"></a>関連リンク

- [CancellationSignal](https://developer.android.com/reference/android/os/CancellationSignal.html)
- [FingerprintManager.AuthenticationCallback](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)
- [FingerprintManager.CryptoObject](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.CryptoObject.html)
- [FingerprintManagerCompat.CryptoObject](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.CryptoObject.html)
- [FingerprintManager](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html)
- [FingerprintManagerCompat](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)
