---
title: 認証コールバックへの応答
ms.prod: xamarin
ms.assetid: 6533AFC9-1A1C-4897-A154-4D4ECFE27761
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/06/2017
ms.openlocfilehash: f1fc484931ba7a574ac660b4856f20b1cb1e08a3
ms.sourcegitcommit: 1dd7d09b60fcb1bf15ba54831ed3dd46aa5240cb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/28/2019
ms.locfileid: "70119584"
---
# <a name="responding-to-authentication-callbacks"></a>認証コールバックへの応答

指紋スキャナーは、独自のスレッドでバックグラウンドで実行され、完了すると、UI スレッドでの1つの`FingerprintManager.AuthenticationCallback`メソッドを呼び出すことによってスキャンの結果を報告します。 Android アプリケーションは、次のメソッドをすべて実装して、この抽象クラスを拡張する独自のハンドラーを提供する必要があります。

- **`OnAuthenticationError(int errorCode, ICharSequence errString)`** &ndash;回復不可能なエラーが発生したときに呼び出されます。 アプリケーションやユーザーは、このような状況を修正することはできません。ただし、もう一度試してみてください。
- **`OnAuthenticationFailed()`** &ndash;このメソッドは、指紋が検出されたがデバイスで認識されない場合に呼び出されます。
- **`OnAuthenticationHelp(int helpMsgId, ICharSequence helpString)`** &ndash;スキャナー経由でスワイプする指など、回復可能なエラーが発生したときに呼び出されます。
- **`OnAuthenticationSucceeded(FingerprintManagerCompati.AuthenticationResult result)`** &ndash;これは、指紋が認識されたときに呼び出されます。

を呼び出す`CryptoObject` `Authenticate`ときにを使用した場合は、で`Cipher.DoFinal` `OnAuthenticationSuccessful`を呼び出すことをお勧めします。
`DoFinal`は、暗号が改ざんされたか、不適切に初期化された場合に例外をスローします。これは、指紋スキャナーの結果がアプリケーションの外部で改ざんされた可能性があることを示します。


> [!NOTE]
> コールバッククラスは比較的軽量で、アプリケーション固有のロジックを解放することをお勧めします。 コールバックは、Android アプリケーションと指紋スキャナーの結果との間で "traffic cop" として機能する必要があります。

## <a name="a-sample-authentication-callback-handler"></a>認証コールバックハンドラーのサンプル

次のクラスは、最小`FingerprintManager.AuthenticationCallback`実装の例です。 

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

`OnAuthenticationSucceeded`が呼び出されたとき`Cipher` `Authentication`にがに`FingerprintManager`指定されたかどうかを確認します。 その場合、 `DoFinal`メソッドは暗号で呼び出されます。 これにより`Cipher`、が閉じられ、元の状態に復元されます。 暗号に問題があった場合、 `DoFinal`は例外をスローし、認証の試行は失敗したと見なす必要があります。

`OnAuthenticationError` と`OnAuthenticationHelp`の各コールバックは、問題の原因を示す整数を受け取ります。 次のセクションでは、使用可能なヘルプまたはエラーコードについて説明します。 2つのコールバックは&ndash; 、指紋認証が失敗したことをアプリケーションに通知するために、同様の目的で機能します。 相違点は、重要度によって異なります。 `OnAuthenticationHelp`指紋が高速にスワイプされるなど、ユーザー回復可能なエラーです。`OnAuthenticationError`指紋スキャナーの破損など、重大なエラーです。

メッセージ`CancellationSignal.Cancel()`を`OnAuthenticationError`使用して指紋スキャンがキャンセルされると、が呼び出されることに注意してください。 パラメーターの値は 5 (`FingerprintState.ErrorCanceled`) です。 `errMsgId` の実装では`AuthenticationCallbacks` 、要件に応じて、この状況を他のエラーとは異なる方法で扱うことができます。 

`OnAuthenticationFailed`は、指紋が正常にスキャンされたが、デバイスに登録されている指紋と一致しなかった場合に呼び出されます。 

## <a name="help-codes-and-error-message-ids"></a>ヘルプコードとエラーメッセージ Id 

エラーコードとヘルプコードの一覧と説明については、FingerprintManager クラスの[Android SDK のドキュメント](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html#FINGERPRINT_ACQUIRED_GOOD)を参照してください。 Xamarin では、 `Android.Hardware.Fingerprints.FingerprintState`次の列挙値を使用してこれらの値を表します。


- **`AcquiredGood`** &ndash; (値 0) 取得したイメージが正常であった。


- **`AcquiredImagerDirty`** &ndash; (値 3) センサーの汚れが疑われるか、検出された汚れが原因で、指紋画像が雑音を超えました。 たとえば、センサーの汚れを複数`AcquiredInsufficient`または実際に検出した後に、これを返すことは妥当です (スタックピクセル、膨大など)。 ユーザーは、これが返されたときにセンサーをクリーニングするためのアクションを実行する必要があります。


- **`AcquiredInsufficient`** (値 2) 検出された状態 (つまりドライスキン) またはダーティセンサー (場合によっては、「」を参照し`AcquiredImagerDirty`てください。 &ndash;



- **`AcquiredPartial`** &ndash; (値 1) 部分的なフィンガープリントイメージのみが検出されました。 登録時には、この問題を解決するために何が必要なのかをユーザーに&ldquo;通知する必要があります。たとえば、センサーでしっかりと押します。&rdquo;



- **`AcquiredTooFast`** &ndash; (値 5) クイックモーションが原因で、フィンガープリントイメージが不完全でした。 線形配列センサーに適していますが、これは、取得中に指が移動された場合にも発生する可能性があります。 ユーザーは、指をゆっくり (線状) に移動するか、または指をもっと長くするように求められます。




- **`AcquiredToSlow`** &ndash; (値 4) 動作が不足しているために指紋イメージを読み取ることができませんでした。 これは、スワイプモーションが必要な線形配列センサーに最適です。



- **`ErrorCanceled`** &ndash; (値 5) 指紋センサーが使用できないため、操作が取り消されました。 たとえば、ユーザーが切り替えられた場合、デバイスがロックされている場合、または別の保留中の操作によってブロックまたは無効化された場合に発生する可能性があります。



- **`ErrorHwUnavailable`** &ndash; (値 1) ハードウェアは使用できません。 後でもう一度お試しください。




- **`ErrorLockout`** &ndash; (値 7) 試行回数が多すぎるために API がロックアウトされたため、操作が取り消されました。




- **`ErrorNoSpace`** &ndash; (値 4) 登録などの操作に対して返されたエラー状態。操作を完了するのに十分なストレージが残っていないため、操作を完了できません。



- **`ErrorTimeout`** &ndash; (値 3) 現在の要求の実行時間が長すぎる場合に返されるエラー状態。 これは、プログラムが指紋センサーを無期限に待機するのを防ぐためのものです。 タイムアウトはプラットフォームとセンサー固有ですが、通常は約30秒です。



- **`ErrorUnableToProcess`** &ndash; (値 2) センサーが現在のイメージを処理できなかった場合に返されるエラー状態。



## <a name="related-links"></a>関連リンク

- [暗号](https://docs.oracle.com/javase/7/docs/api/javax/crypto/Cipher.html)
- [AuthenticationCallback](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)
- [AuthenticationCallback](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.AuthenticationCallback.html)
