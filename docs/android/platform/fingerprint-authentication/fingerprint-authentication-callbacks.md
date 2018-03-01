---
title: "認証コールバックへの応答"
ms.topic: article
ms.prod: xamarin
ms.assetid: 6533AFC9-1A1C-4897-A154-4D4ECFE27761
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/06/2017
ms.openlocfilehash: 371ffae8e14a630cb548f4a9ee2bf0bd06f7284c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="responding-to-authentication-callbacks"></a>認証コールバックへの応答

指紋スキャナーが独自のスレッドでバック グラウンドで実行しが完了すると、スキャンの結果の 1 つのメソッドを呼び出すことによって報告されます`FingerprintManager.AuthenticationCallback`UI スレッドにします。 Android アプリケーションは、次のすべてのメソッドを実装してこの抽象クラスを拡張する独自のハンドラーを提供する必要があります。

* **`OnAuthenticationError(int errorCode, ICharSequence errString)`** &ndash; 回復不能なエラーがあるときに呼び出されます。 アプリケーションまたはユーザーをもう一度やり直してください可能性のある点を除いて、この状況を解決するのに実行できるものはありません。
* **`OnAuthenticationFailed()`** &ndash; 指紋が検出されましたが、デバイスによって認識されない場合は、このメソッドが呼び出されます。
* **`OnAuthenticationHelp(int helpMsgId, ICharSequence helpString)`** &ndash; スキャナーを高速に読み取られている指など、回復可能なエラーがあるときに呼び出されます。
* **`OnAuthenticationSucceeded(FingerprintManagerCompati.AuthenticationResult result)`** &ndash; これは、指紋が認識されているときに呼び出されます。

場合、`CryptoObject`を呼び出すときに使用された`Authenticate`を呼び出すことをお勧め`Cipher.DoFinal`で`OnAuthenticationSuccessful`です。
`DoFinal` 暗号が改ざんまたは不適切な初期化される場合、例外がスローされます、こと指紋スキャナーの結果が改ざんされて、アプリケーションの外部を示すです。


> [!NOTE]
> **注:**保持しに、コールバック クラス比較的軽量のないアプリケーション固有のロジックをお勧めします。 コールバックが、「ように、トラフィック」Android アプリケーションと、結果の間で指紋スキャナーからとして機能します。

## <a name="a-sample-authentication-callback-handler"></a>サンプル認証コールバック ハンドラー

次のクラスは、最小限の例`FingerprintManager.AuthenticationCallback`実装します。 

```csharp
class MyAuthCallbackSample : FingerprintManagerCompat.AuthenticationCallback
{
    // Can be any byte array, keep unique to application.
    static readonly byte[] SECRET_BYTES = {1, 2, 3, 4, 5, 6, 7, 8, 9};
    // The TAG can be any string, this one is for demonstration.
    static readonly string TAG = "X:" + typeof (SimpleAuthCallbacks).Name;

    public MyAuthCallbackSample()
    {
    }

    public override void OnAuthenticationSucceeded(FingerprintManagerCompat.AuthenticationResult result)
    {
        if (result.CryptoObject.Cipher != null) 
        {
            try
            {
                // Calling DoFinal on the Cipher ensures that the encryption worked.
                byte[] doFinalResult = result.CryptoObject.Cipher.DoFinal(SECRET_BYTES);
    
                // No errors occurred, trust the results.              
            }
            catch (BadPaddingException bpe)
            {
                // Can't really trust the results.
                Log.Error(TAG, "Failed to encrypt the data with the generated key." + bpe);
            }
            catch (IllegalBlockSizeException ibse)
            {
                // Can't really trust the results.
                Log.Error(TAG, "Failed to encrypt the data with the generated key." + ibse);
            }
        }
        else
        {
            // No cipher used, assume that everything went well and trust the results.
        }
    }

    public override void OnAuthenticationError(int errMsgId, ICharSequence errString)
    {
        // Report the error to the user. Note that if the user canceled the scan,
        // this method will be called and the errMsgId will be FingerprintState.ErrorCanceled.
    }

    public override void OnAuthenticationFailed()
    {
        // Tell the user that the fingerprint was not recognized.
    }

    public override void OnAuthenticationHelp(int helpMsgId, ICharSequence helpString)
    {
        // Notify the user that the scan failed and display the provided hint.
    }
}
```

`OnAuthenticationSucceeded` かどうかをチェック、`Cipher`に提供された`FingerprintManager`とき`Authentication`が呼び出されました。 場合は、`DoFinal`暗号にメソッドが呼び出されます。 終了、 `Cipher`、元の状態に復元します。 あったか、暗号の問題、`DoFinal`は例外をスローし、認証の試行は失敗したものと見なされます。

`OnAuthenticationError`と`OnAuthenticationHelp`各コールバックは、問題が何を示す整数を受信します。 次のセクションでは、個々 の可能なヘルプまたはエラー コードについて説明します。 2 つのコールバックは、同様の目的を果たします&ndash;指紋認証が失敗したことをアプリケーションに通知するためにします。 動作の違いは重大度です。 `OnAuthenticationHelp` 速すぎます。 指紋をスワイプなど、ユーザーの回復可能なエラーは、します。`OnAuthenticationError`が破損している指紋スキャナーより重大なエラーです。

なお`OnAuthenticationError`指紋スキャンが使用して取り消されたときに呼び出される、`CancellationSignal.Cancel()`メッセージ。 `errMsgId`パラメーター 5 の値になります (`FingerprintState.ErrorCanceled`)。 実装の要件に応じて、`AuthenticationCallbacks`他のエラーとは異なるこのような状況を扱うことがあります。 

`OnAuthenticationFailed` 指紋が正常にスキャンされたが、デバイスに登録されている任意の指紋が一致しませんでしたが呼び出されます。 

## <a name="help-codes-and-error-message-ids"></a>ヘルプのコードとエラー メッセージ Id 

一覧と、エラー コードとコードのヘルプの説明は、「、 [Android SDK のドキュメント](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html#FINGERPRINT_ACQUIRED_GOOD)FingerprintManager クラスです。 Xamarin.Android がこれらの値を表す、`Android.Hardware.Fingerprints.FingerprintState`列挙型。


-   **`AcquiredGood`** &ndash; (値 0)取得したイメージは問題ありませんでした。


-   **`AcquiredImagerDirty`** &ndash; (値 3)指紋イメージが、センサーの土の疑いがあるか、検出されたためノイズが多すぎます。 たとえば、これを複数の後に返される妥当なは`AcquiredInsufficient`またはセンサー (抜け、範囲など) の土の実際の検出。 ユーザーは、これは、返されるときに、センサーをクリーンアップするアクションを実行すると想定されます。


-   **`AcquiredInsufficient`** &ndash; (値 2)指紋イメージが検出された状態 (つまりドライ スキン) または可能性のあるダーティ センサーのために処理にノイズが多すぎる (を参照してください`AcquiredImagerDirty`です。



-   **`AcquiredPartial`** &ndash; (値 1)部分的な指紋イメージのみが検出されました。 たとえば、この問題を解決する場合に発生する必要がある登録中に、ユーザーに通知する必要があります&ldquo;センサーをしっかりとします。&rdquo;



-   **`AcquiredTooFast`** &ndash; (値 5)指紋イメージは、クイック移行中のため完了しませんでした。 線形配列センサーのほとんどの場合、必要に応じて中に、起こる場合も取得中に、指が移動された場合。 ユーザーは、低速の本の指の移動を求める必要があります (線形) か長いセンサーの指のままにします。




-   **`AcquiredToSlow`** &ndash; (値は 4)指紋イメージは、動きがないのため読み取れませんでした。 これはスワイプ動きを必要とする線形配列センサーに最適です。



-   **`ErrorCanceled`** &ndash; (値 5)指紋センサーを使用できないために、操作が取り消されました。 たとえば、ユーザーが切り替えられる、デバイスがロックされている、または別の保留中の操作により、または無効にするときに可能性があります。



-   **`ErrorHwUnavailable`** &ndash; (値 1)ハードウェアでは使用できません。 後でもう一度やり直してください。




-   **`ErrorLockout`** &ndash; (値 7)API は、、回数が多すぎるためロックされているために、操作が取り消されました。




-   **`ErrorNoSpace`** &ndash; (値は 4)登録; などの操作に対して返されるエラー状態残りの操作を完了するための十分な記憶域がないため、操作を完了できません。



-   **`ErrorTimeout`** &ndash; (値 3)エラー状態が現在の要求が長時間実行されている場合に返されます。 指紋センサーを無制限に待つことからプログラムを防ぐためにこのものです。 タイムアウトはプラットフォームとセンサーに固有では一般に約 30 秒です。



-   **`ErrorUnableToProcess`** &ndash; (値 2)エラー状態が、センサーで現在のイメージを処理できなかった場合に返されます。



## <a name="related-links"></a>関連リンク

- [Cipher](https://docs.oracle.com/javase/7/docshttps://developer.xamarin.com/api/javax/crypto/Cipher.html)
- [AuthenticationCallback](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)
- [AuthenticationCallback](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.AuthenticationCallback.html)
