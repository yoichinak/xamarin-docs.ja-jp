---
title: 指紋をスキャン
ms.prod: xamarin
ms.assetid: 1CDDC096-77E0-47B3-BE0B-8953E2DDACD3
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/23/2016
ms.openlocfilehash: 2b20402fd2cd6782247fb2c62513d7aff43a95a6
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
ms.locfileid: "30774162"
---
# <a name="scanning-for-fingerprints"></a>指紋をスキャン

指紋認証を使用する Xamarin.Android アプリケーションを準備する方法を確認して、もう一度、`FingerprintManager.Authenticate`メソッド、し、そのため、Android 6.0 指紋認証をについて説明します。 この一覧に指紋認証ワークフローの概要のとおりです。

1. 呼び出す`FingerprintManager.Authenticate`渡す、`CryptoObject`と`FingerprintManager.AuthenticationCallback`インスタンス。 `CryptoObject`に指紋認証の結果が変更されていないことを確認するために使用します。 
2. サブクラス、 [FingerprintManager.AuthenticationCallback](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)クラスです。 このクラスのインスタンスに提供される`FingerprintManager`とき指紋認証を開始します。 指紋スキャナーが完了したら、このクラスでコールバック メソッドのいずれかが起動されます。
3. ユーザーにデバイスが指紋スキャナーが開始され、ユーザーの操作が待機していることを知らせるに UI を更新するコードを記述します。 
4. 指紋スキャナーを完了すると、Android は結果を返します、アプリケーションへのメソッドを呼び出すことによって、`FingerprintManager.AuthenticationCallback`前の手順で指定されたインスタンスです。
5. アプリケーションは、指紋認証結果をユーザーに通知し、適切な結果に対応するため。 

次のコード スニペットは、指紋のスキャンを開始するアクティビティのメソッドの例を示します。

```csharp
protected void FingerPrintAuthenticationExample()
{
    const int flags = 0; /* always zero (0) */

    // CryptoObjectHelper is described in the previous section.
    CryptoObjectHelper cryptoHelper = new CryptoObjectHelper();    
    
    // cancellationSignal can be used to manually stop the fingerprint scanner. 
    cancellationSignal = new Android.Support.V4.OS.CancellationSignal();
    
    FingerprintManagerCompat fingerPrintManager = FingerprintManagerCompat.From(this);
    
    // AuthenticationCallback is a base class that will be covered later on in this guide.
    FingerprintManagerCompat.AuthenticationCallback authenticationCallback = new MyAuthCallbackSample(this);

    // Start the fingerprint scanner.
    fingerprintManager.Authenticate(cryptoHelper.BuildCryptoObject(), flags, cancellationSignal, authenticationCallback, null);
}
```

これらの各パラメーターの説明、`Authenticate`もう少し詳しくメソッド。

* 最初のパラメーターは、 _crypto_オブジェクトの指紋スキャナーを使用することの指紋スキャン結果を認証します。 このオブジェクトはあります`null`、その場合、アプリケーションが何を無条件に信頼するが改ざん指紋結果。 推奨、`CryptoObject`インスタンス化されに提供される、 `FingerprintManager` null ではなくです。 [作成、CryptObject](~/android/platform/fingerprint-authentication/creating-a-cryptoobject.md)はインスタンス化する方法の詳細に説明、`CryptoObject`に基づいて、 `Cipher`。
* 2 番目のパラメーターは、常に 0 です。 Android のドキュメントを識別するこのフラグのセットとして将来使用するために予約された可能性があります。 
* 3 番目のパラメーターでは、`cancellationSignal`指紋スキャナーをオフにして、現在の要求をキャンセルするために使用するオブジェクトです。 これは、 [Android CancellationSignal](http://developer.android.com/reference/android/os/CancellationSignal.html)、および .NET framework の型ではありません。
* 4 番目のパラメーターは必須では、クラスをサブクラス化する、`AuthenticationCallback`抽象クラスです。 クライアントに通知をこのクラスのメソッドが呼び出されるときに、`FingerprintManager`が終了され、どの結果。 実装について理解しておくの多くがあるため、`AuthenticationCallback`で取り上げる[独自のセクションでは](~/android/platform/fingerprint-authentication/fingerprint-authentication-callbacks.md)します。
* 5 番目のパラメーターは省略可能な`Handler`インスタンス。 場合、`Handler`オブジェクトを指定すると、`FingerprintManager`を使用して、`Looper`指紋ハードウェアからのメッセージを処理するときにそのオブジェクトからです。 通常、1 つは必要はありませんを提供する、 `Handler`、FingerprintManager を使用して、`Looper`アプリケーションからです。

## <a name="cancelling-a-fingerprint-scan"></a>指紋によるスキャンをキャンセルします。

ユーザー (またはアプリケーション) が開始された後に、指紋スキャンをキャンセルするために必要な場合があります。 このような状況で呼び出し、 [ `IsCancelled` ](http://developer.android.com/reference/android/os/CancellationSignal.html#isCanceled())メソッドを[ `CancellationSignal` ](http://developer.android.com/reference/android/os/CancellationSignal.html)に指定された`FingerprintManager.Authenticate`指紋スキャンを開始するために呼び出されたとき。

これで、これまで見てきた、`Authenticate`メソッド、さらに詳しくより重要なパラメーターを調べてみましょう。 最初に、見ていきます[認証コールバックへの応答](~/android/platform/fingerprint-authentication/fingerprint-authentication-callbacks.md)はについて説明しますサブクラス化する方法、 [FingerprintManager.AuthenticationCallback](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)に対応するために Android アプリケーションを有効にすると、指紋スキャナーによって提供される結果。




## <a name="related-links"></a>関連リンク

- [CancellationSignal](http://developer.android.com/reference/android/os/CancellationSignal.html)
- [FingerprintManager.AuthenticationCallback](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)
- [FingerprintManager.CryptoObject](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.CryptoObject.html)
- [FingerprintManagerCompat.CryptoObject](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.CryptoObject.html)
- [FingerprintManager](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html)
- [FingerprintManagerCompat](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)
