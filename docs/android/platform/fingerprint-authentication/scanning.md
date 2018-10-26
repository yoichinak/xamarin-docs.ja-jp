---
title: 指紋のスキャン
ms.prod: xamarin
ms.assetid: 1CDDC096-77E0-47B3-BE0B-8953E2DDACD3
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/23/2016
ms.openlocfilehash: ed7f31b011c32b25f431801e47c570900589e131
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50106326"
---
# <a name="scanning-for-fingerprints"></a>指紋のスキャン

指紋認証を使用する Xamarin.Android アプリケーションを準備する方法を確認しましたに戻って、`FingerprintManager.Authenticate`メソッドでは、Android 6.0 指紋認証では、その場所について説明します。 この一覧に指紋認証のワークフローの概要を説明します。

1. 呼び出す`FingerprintManager.Authenticate`を渡して、`CryptoObject`と`FingerprintManager.AuthenticationCallback`インスタンス。 `CryptoObject`に指紋認証の結果が変更されていないことを確認するために使用します。 
2. サブクラス、 [FingerprintManager.AuthenticationCallback](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)クラス。 このクラスのインスタンスに提供される`FingerprintManager`指紋ときの認証を開始します。 指紋スキャナーが完了したら、このクラスでコールバック メソッドのいずれかが呼び出されます。
3. ユーザーにデバイスが指紋スキャナーが開始され、ユーザーの操作が待機していることを通知する UI を更新するコードを記述します。 
4. 指紋スキャナーを完了すると、Android は結果を返します、アプリケーションへのメソッドを呼び出すことによって、`FingerprintManager.AuthenticationCallback`前の手順で指定されたインスタンス。
5. アプリケーションが、指紋認証結果をユーザーに通知され、適切な結果に対応します。 

次のコード スニペットでは、指紋のスキャンを開始するアクティビティ内のメソッドの例を示します。

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

これらのパラメーターのそれぞれについて説明します、`Authenticate`メソッドで少し詳しく説明します。

* 最初のパラメーターは、 _crypto_指紋スキャンの結果を認証するが、指紋スキャナーを使用するオブジェクトします。 このオブジェクトは`null`、指紋結果場合、アプリケーションは、何を無条件に信頼する必要があります。 変更います。 推奨されます、`CryptoObject`インスタンスが作成されに提供される、 `FingerprintManager` null ではなく。 [CryptObject を作成する](~/android/platform/fingerprint-authentication/creating-a-cryptoobject.md)インスタンスを作成する方法の詳細を説明します、`CryptoObject`に基づいて、`Cipher`します。
* 2 番目のパラメーターは、常に 0 です。 Android ドキュメントでは、これをフラグのセットとして識別しは将来使用するため予約されている可能性があります。 
* 3 番目のパラメーターでは、`cancellationSignal`指紋スキャナーをオフにして、現在の要求をキャンセルするために使用するオブジェクトです。 これは、 [Android CancellationSignal](http://developer.android.com/reference/android/os/CancellationSignal.html)、および .NET framework からの型ではありません。
* 4 番目のパラメーターは必須であり、クラスをサブクラス化するは、`AuthenticationCallback`抽象クラス。 クライアントに通知するこのクラスのメソッドが呼び出されるときに、`FingerprintManager`が完了し、結果は。 実装する方法を理解するのには多く、`AuthenticationCallback`で取り上げる[独自のセクションは](~/android/platform/fingerprint-authentication/fingerprint-authentication-callbacks.md)します。
* 5 番目のパラメーターは省略可能な`Handler`インスタンス。 場合、`Handler`オブジェクトが提供される、`FingerprintManager`を使用して、`Looper`指紋ハードウェアからメッセージを処理するときに、そのオブジェクトから。 通常、1 つ指定する必要はありません、 `Handler`、FingerprintManager を使用して、`Looper`アプリケーションから。

## <a name="cancelling-a-fingerprint-scan"></a>指紋のスキャンを取り消しています

ユーザー (またはアプリケーション) が開始された後に、指紋のスキャンをキャンセルする必要がある場合があります。 このような状況で呼び出し、 [ `IsCancelled` ](http://developer.android.com/reference/android/os/CancellationSignal.html#isCanceled())メソッドを[ `CancellationSignal` ](http://developer.android.com/reference/android/os/CancellationSignal.html)に指定された`FingerprintManager.Authenticate`指紋のスキャンを開始するときに呼び出されました。

これで行ってきた、`Authenticate`メソッド、さらに重要なパラメーターの詳細についてのいくつか見ていきましょう。 見て最初に、[認証コールバックへの応答](~/android/platform/fingerprint-authentication/fingerprint-authentication-callbacks.md)はについて説明しますサブクラス化する方法、 [FingerprintManager.AuthenticationCallback](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)に対応する Android アプリケーションを有効にすると、指紋スキャナーによって提供される結果。




## <a name="related-links"></a>関連リンク

- [CancellationSignal](http://developer.android.com/reference/android/os/CancellationSignal.html)
- [FingerprintManager.AuthenticationCallback](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)
- [FingerprintManager.CryptoObject](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.CryptoObject.html)
- [FingerprintManagerCompat.CryptoObject](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.CryptoObject.html)
- [FingerprintManager](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html)
- [FingerprintManagerCompat](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)
