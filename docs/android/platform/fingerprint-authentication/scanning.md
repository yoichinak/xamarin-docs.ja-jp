---
title: 指紋のスキャン
ms.prod: xamarin
ms.assetid: 1CDDC096-77E0-47B3-BE0B-8953E2DDACD3
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/23/2016
ms.openlocfilehash: 4ead912b55790caf3e2e1f22e149f5682e6bb697
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70761219"
---
# <a name="scanning-for-fingerprints"></a>指紋のスキャン

ここでは、指紋認証を使用するように Xamarin Android アプリケーションを準備する方法を説明しました`FingerprintManager.Authenticate` 。次に、メソッドに戻り、android 6.0 フィンガープリント認証の場所について説明します。 指紋認証のワークフローの概要については、次の一覧で説明します。

1. `CryptoObject`と`FingerprintManager.Authenticate` `FingerprintManager.AuthenticationCallback`インスタンスを渡して、を呼び出します。 は`CryptoObject` 、指紋認証の結果が改ざんされていないことを確認するために使用されます。 
2. [FingerprintManager](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)クラスをサブクラス化します。 フィンガープリント認証が開始されると、 `FingerprintManager`このクラスのインスタンスがに提供されます。 指紋スキャナーが終了すると、このクラスのコールバックメソッドの1つが呼び出されます。
3. デバイスが指紋スキャナーを開始し、ユーザーの操作を待機していることをユーザーに通知するように、UI を更新するコードを記述します。 
4. 指紋スキャナーが完了すると、Android は、前の手順で指定した`FingerprintManager.AuthenticationCallback`インスタンスでメソッドを呼び出すことによって、結果をアプリケーションに返します。
5. アプリケーションは、指紋認証の結果をユーザーに通知し、必要に応じて結果に反応します。 

次のコードスニペットは、指紋のスキャンを開始するアクティビティ内のメソッドの例です。

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

メソッド内のこれらのパラメーターについて`Authenticate`もう少し詳しく説明します。

- 最初のパラメーターは、指紋スキャンの結果を認証するために指紋スキャナーが使用する_crypto_オブジェクトです。 このオブジェクトは、 `null`である可能性があります。この場合、アプリケーションは、指紋結果が改ざんされていないことを無条件に信頼する必要があります。 を`CryptoObject`インスタンス化し、null `FingerprintManager`ではなくに指定することをお勧めします。 [Cryptobject の作成](~/android/platform/fingerprint-authentication/creating-a-cryptoobject.md)では、 `CryptoObject` `Cipher`に基づいてをインスタンス化する方法について詳しく説明します。
- 2番目のパラメーターは常に0です。 Android のドキュメントでは、これをフラグのセットとして識別し、将来使用するために予約されている可能性があります。 
- 3番目の`cancellationSignal`パラメーターは、指紋スキャナーをオフにして現在の要求をキャンセルするために使用されるオブジェクトです。 これは[Android CancellationSignal](https://developer.android.com/reference/android/os/CancellationSignal.html)であり、.net framework の型ではありません。
- 4番目のパラメーターは必須で、 `AuthenticationCallback`抽象クラスをサブクラス化するクラスです。 このクラスのメソッドは、 `FingerprintManager`が終了したことと結果がクライアントに通知するために呼び出されます。 の`AuthenticationCallback`実装について理解することはたくさんありますが、それについては[独自のセクション](~/android/platform/fingerprint-authentication/fingerprint-authentication-callbacks.md)で説明します。
- 5番目のパラメーターは`Handler` 、省略可能なインスタンスです。 オブジェクトが指定されている`FingerprintManager`場合、は`Looper`フィンガープリントハードウェアからのメッセージを処理するときに、そのオブジェクトのを使用します。 `Handler` 通常、を`Handler`提供する必要はありませんが、FingerprintManager はアプリケーション`Looper`からを使用します。

## <a name="cancelling-a-fingerprint-scan"></a>指紋スキャンを取り消しています

ユーザー (またはアプリケーション) が、開始後に指紋スキャンをキャンセルする必要がある場合があります。 このよう[`CancellationSignal`](https://developer.android.com/reference/android/os/CancellationSignal.html)な場合は、 [`IsCancelled`](https://developer.android.com/reference/android/os/CancellationSignal.html#isCanceled())に渡されたで、に`FingerprintManager.Authenticate`よって指定されたメソッドを呼び出して、指紋スキャンを開始します。

`Authenticate`メソッドについて説明したので、さらに重要なパラメーターをいくつか詳しく見ていきましょう。 まず、[認証コールバックへの対応](~/android/platform/fingerprint-authentication/fingerprint-authentication-callbacks.md)について説明します。ここでは、 [FingerprintManager コールバック](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)をサブクラス化して、Android アプリケーションが指紋スキャナーによって得られた結果に反応できるようにする方法について説明します。

## <a name="related-links"></a>関連リンク

- [CancellationSignal](https://developer.android.com/reference/android/os/CancellationSignal.html)
- [FingerprintManager](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)
- [FingerprintManager.CryptoObject](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.CryptoObject.html)
- [FingerprintManagerCompat.CryptoObject](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.CryptoObject.html)
- [FingerprintManager](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html)
- [FingerprintManagerCompat](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)
