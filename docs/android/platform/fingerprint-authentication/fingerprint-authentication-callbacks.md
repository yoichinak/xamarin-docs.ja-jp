---
title: 認証コールバックへの応答
ms.prod: xamarin
ms.assetid: 6533AFC9-1A1C-4897-A154-4D4ECFE27761
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/06/2017
ms.openlocfilehash: 17a1c94ad3b9bde67537ea7113352f0fc10d2a08
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50110122"
---
# <a name="responding-to-authentication-callbacks"></a>認証コールバックへの応答

指紋スキャナーは、独自のスレッドでバック グラウンドで実行し、それが完了すると、スキャンの結果の 1 つのメソッドを呼び出すことによって報告されます`FingerprintManager.AuthenticationCallback`UI スレッド上でします。 Android アプリケーションでは、次のすべてのメソッドを実装して、この抽象クラスを拡張する独自のハンドラーを提供する必要があります。

* **`OnAuthenticationError(int errorCode, ICharSequence errString)`** &ndash; 回復不能なエラーがあるときに呼び出されます。 もう一度お試し可能性がある点を除いて、問題を解決するアプリケーションまたはユーザーが行うものがありません。
* **`OnAuthenticationFailed()`** &ndash; 指紋が検出されましたが、デバイスによって認識されない場合は、このメソッドが呼び出されます。
* **`OnAuthenticationHelp(int helpMsgId, ICharSequence helpString)`** &ndash; スキャナーの上に高速に読み取られている指など、回復可能なエラーがあるときに呼び出されます。
* **`OnAuthenticationSucceeded(FingerprintManagerCompati.AuthenticationResult result)`** &ndash; 指紋が認識されているときに呼び出されます。

場合、`CryptoObject`を呼び出すときに使用された`Authenticate`を呼び出すことをお勧め`Cipher.DoFinal`で`OnAuthenticationSuccessful`します。
`DoFinal` 指紋スキャナーの結果が改ざんされたアプリケーションの外部で、暗号が改ざんされたり、正しく初期化される場合、例外がスローされます。


> [!NOTE]
> コールバック クラスの比較的軽い重みを保持し、アプリケーション固有のロジックの解放することをお勧めします。 コールバックは、"traffic cop"Android アプリケーションと、結果の指紋スキャナーからとして機能する必要があります。

## <a name="a-sample-authentication-callback-handler"></a>認証コールバック ハンドラーをサンプル

次のクラスは、最小限の例を示します`FingerprintManager.AuthenticationCallback`実装。 

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

`OnAuthenticationSucceeded` かどうかをチェック、`Cipher`に提供された`FingerprintManager`とき`Authentication`が呼び出されました。 そうである場合、`DoFinal`暗号でメソッドが呼び出されます。 終了、 `Cipher`、元の状態に復元することです。 かどうかは問題があった、暗号でし`DoFinal`は例外をスローして失敗した認証の試行を考慮する必要があります。

`OnAuthenticationError`と`OnAuthenticationHelp`ごとのコールバックは、問題が発生するかを示す整数値を受信します。 次のセクションでは、それぞれの可能なヘルプまたはエラー コードについて説明します。 2 つのコールバックのような目的が&ndash;その指紋認証が失敗したアプリケーションに通知します。 重大度がどのように異なるか。 `OnAuthenticationHelp` 速すぎる; フィンガー プリントをスワイプなど、ユーザーの回復可能なエラーは、します。`OnAuthenticationError`が破損した指紋スキャナーより重大なエラー。

なお`OnAuthenticationError`経由で指紋のスキャンがキャンセルされたときに呼び出される、`CancellationSignal.Cancel()`メッセージ。 `errMsgId`パラメーター 5 の値になります (`FingerprintState.ErrorCanceled`)。 実装の要件に応じて、`AuthenticationCallbacks`他のエラーとは異なるこのような状況を扱うことがあります。 

`OnAuthenticationFailed` 指紋が正常にスキャンされたが、デバイスに登録されている任意の指紋が一致しませんでしたが呼び出されます。 

## <a name="help-codes-and-error-message-ids"></a>ヘルプのコードとエラー メッセージ Id 

一覧と、エラー コードとヘルプのコードの説明を参照して、 [Android SDK のドキュメント](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html#FINGERPRINT_ACQUIRED_GOOD)FingerprintManager クラス。 Xamarin.Android は、これらの値を表す、`Android.Hardware.Fingerprints.FingerprintState`列挙型。


-   **`AcquiredGood`** &ndash; (値 0)取得したイメージは、問題ありませんでした。


-   **`AcquiredImagerDirty`** &ndash; (値 3)指紋のイメージが、センサー、疑わしいまたは検出された未舗装によりノイズが多すぎます。 たとえば、複数の後にこれを返す適切なは`AcquiredInsufficient`やセンサー (抜け、範囲など) の土の実際の検出。 これは、返されるときに、センサーをクリーンアップするアクションを実行するには、ユーザーが必要です。


-   **`AcquiredInsufficient`** &ndash; (値 2)指紋のイメージが検出された状態 (つまりドライ スキン) または場合によってダーティ センサーにより処理する雑音が多すぎる (を参照してください`AcquiredImagerDirty`します。



-   **`AcquiredPartial`** &ndash; (値 1)部分的な指紋イメージのみが検出されました。 たとえば、この問題を解決する場合に発生する必要があるものの登録中にユーザーに通知する必要があります&ldquo;センサーをしっかりとします。&rdquo;



-   **`AcquiredTooFast`** &ndash; (値 5)指紋の画像は、クイック モーションにより完了しませんでした。 個のセンサーのほとんどの場合、必要に応じて、中には、指を取得中に移動された場合も発生これでした。 低速の指を移動するユーザーを求める必要があります (線形) か長いセンサーの指のままにします。




-   **`AcquiredToSlow`** &ndash; (値 4)指紋の画像をアニメーションがないのため読み取れませんでした。 これは、スワイプ動作を必要とする線形配列センサーに最も適した。



-   **`ErrorCanceled`** &ndash; (値 5)指紋センサーが利用できないために、操作が取り消されました。 たとえば、ユーザーの切り替え、デバイスがロックされている、またはにより別の操作の保留中、または無効にするときにこれ可能性があります。



-   **`ErrorHwUnavailable`** &ndash; (値 1)ハードウェアは、ご利用いただけません。 後でもう一度お試しください。




-   **`ErrorLockout`** &ndash; (値 7)API は、、回数が多すぎるためロックされているために、操作が取り消されました。




-   **`ErrorNoSpace`** &ndash; (値 4)登録などの操作に対して返されるエラーの状態残りの操作を完了するための十分な記憶域がないためにの操作を完了できません。



-   **`ErrorTimeout`** &ndash; (値 3)エラー状態が現在の要求が実行されている長すぎる場合に返されます。 これは、プログラムが、指紋センサーを無期限に待機していることを防止するものです。 タイムアウトはプラットフォームであり、センサー固有が、通常約 30 秒。



-   **`ErrorUnableToProcess`** &ndash; (値 2)エラー状態は、センサーで、現在のイメージを処理できなかった場合に返されます。



## <a name="related-links"></a>関連リンク

- [暗号](https://docs.oracle.com/javase/7/docs/api/javax/crypto/Cipher.html)
- [AuthenticationCallback](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)
- [AuthenticationCallback](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.AuthenticationCallback.html)
