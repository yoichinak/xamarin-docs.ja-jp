---
title: 指紋のスキャン
ms.prod: xamarin
ms.assetid: 1CDDC096-77E0-47B3-BE0B-8953E2DDACD3
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/23/2016
ms.openlocfilehash: 61edd0e4b532f18a8fc28502e5bb990703068776
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027489"
---
# <a name="scanning-for-fingerprints"></a>指紋のスキャン

ここでは、指紋認証を使用するように Xamarin Android アプリケーションを準備する方法を説明しました。次に、`FingerprintManager.Authenticate` メソッドに戻り、Android 6.0 の指紋認証でのその場所について説明します。 指紋認証のワークフローの概要については、次の一覧で説明します。

1. `FingerprintManager.Authenticate`を呼び出し、`CryptoObject` と `FingerprintManager.AuthenticationCallback` インスタンスを渡します。 `CryptoObject` は、指紋認証の結果が改ざんされていないことを確認するために使用されます。 
2. [FingerprintManager](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)クラスをサブクラス化します。 フィンガープリント認証が開始されると、このクラスのインスタンスが `FingerprintManager` に提供されます。 指紋スキャナーが終了すると、このクラスのコールバックメソッドの1つが呼び出されます。
3. デバイスが指紋スキャナーを開始し、ユーザーの操作を待機していることをユーザーに通知するように、UI を更新するコードを記述します。 
4. 指紋スキャナーが完了すると、Android は、前の手順で指定した `FingerprintManager.AuthenticationCallback` インスタンスでメソッドを呼び出すことによって、結果をアプリケーションに返します。
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

ここでは、`Authenticate` メソッドの各パラメーターについてもう少し詳しく説明します。

- 最初のパラメーターは、指紋スキャンの結果を認証するために指紋スキャナーが使用する_crypto_オブジェクトです。 このオブジェクトは `null`である可能性があります。この場合、アプリケーションは、指紋の結果が改ざんされていないことを無条件に信頼する必要があります。 `CryptoObject` をインスタンス化し、null ではなく `FingerprintManager` に提供することをお勧めします。 [CryptObject の作成](~/android/platform/fingerprint-authentication/creating-a-cryptoobject.md)では、`Cipher`に基づいて `CryptoObject` をインスタンス化する方法について詳しく説明します。
- 2番目のパラメーターは常に0です。 Android のドキュメントでは、これをフラグのセットとして識別し、将来使用するために予約されている可能性があります。 
- 3番目のパラメーターである `cancellationSignal` は、指紋スキャナーをオフにして現在の要求をキャンセルするために使用されるオブジェクトです。 これは[Android CancellationSignal](https://developer.android.com/reference/android/os/CancellationSignal.html)であり、.net framework の型ではありません。
- 4番目のパラメーターは必須であり、`AuthenticationCallback` 抽象クラスをサブクラス化するクラスです。 このクラスのメソッドは、`FingerprintManager` が終了し、結果がどのようなものであるかをクライアントに通知するために呼び出されます。 `AuthenticationCallback`の実装について理解することはたくさんありますが、それについては[独自のセクション](~/android/platform/fingerprint-authentication/fingerprint-authentication-callbacks.md)で説明します。
- 5番目のパラメーターは、省略可能な `Handler` インスタンスです。 `Handler` オブジェクトが指定されている場合、`FingerprintManager` はフィンガープリントハードウェアからのメッセージを処理するときに、そのオブジェクトの `Looper` を使用します。 通常は、`Handler`を提供する必要はありません。そのため、FingerprintManager はアプリケーションの `Looper` を使用します。

## <a name="cancelling-a-fingerprint-scan"></a>指紋スキャンを取り消しています

ユーザー (またはアプリケーション) が、開始後に指紋スキャンをキャンセルする必要がある場合があります。 このような状況では、指紋スキャンを開始するために呼び出されたときに `FingerprintManager.Authenticate` に提供された[`CancellationSignal`](https://developer.android.com/reference/android/os/CancellationSignal.html)に対して、 [`IsCancelled`](https://developer.android.com/reference/android/os/CancellationSignal.html#isCanceled())メソッドを呼び出します。

`Authenticate` メソッドを見たので、さらに重要なパラメーターをいくつか詳しく見ていきましょう。 まず、[認証コールバックへの対応](~/android/platform/fingerprint-authentication/fingerprint-authentication-callbacks.md)について説明します。ここでは、 [FingerprintManager コールバック](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)をサブクラス化して、Android アプリケーションが指紋スキャナーによって得られた結果に反応できるようにする方法について説明します。

## <a name="related-links"></a>関連リンク

- [CancellationSignal](https://developer.android.com/reference/android/os/CancellationSignal.html)
- [FingerprintManager](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)
- [FingerprintManager オブジェクト](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.CryptoObject.html)
- [FingerprintManagerCompat オブジェクト](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.CryptoObject.html)
- [FingerprintManager](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html)
- [FingerprintManagerCompat](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)
