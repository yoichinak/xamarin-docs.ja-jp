---
title: 認証コールバックへの応答
ms.prod: xamarin
ms.assetid: 6533AFC9-1A1C-4897-A154-4D4ECFE27761
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/06/2017
ms.openlocfilehash: 8dc06740355bd95828e1a1bd8d9d15a2ef37e6b2
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/10/2020
ms.locfileid: "73027532"
---
# <a name="responding-to-authentication-callbacks"></a>認証コールバックへの応答

指紋スキャナーは、独自のスレッドでバックグラウンドで実行されます。完了すると、UI スレッドで `FingerprintManager.AuthenticationCallback` から 1 つのメソッドを呼び出すことによって、スキャンの結果を報告します。 Android アプリケーションでは、次のメソッドをすべて実装して、この抽象クラスを拡張する独自のハンドラーを提供する必要があります。

- **`OnAuthenticationError(int errorCode, ICharSequence errString)`** &ndash; 回復不可能なエラーがある場合に呼び出されます。 アプリケーションやユーザーは、もう一度試してみる以外に、このような状況を修正することはできません。
- **`OnAuthenticationFailed()`** &ndash; このメソッドは、指紋が検出されているが、デバイスで認識されない場合に呼び出されます。
- **`OnAuthenticationHelp(int helpMsgId, ICharSequence helpString)`** &ndash; スキャナー経由で指で速くスワイプしているなど、回復可能なエラーが発生したときに呼び出されます。
- **`OnAuthenticationSucceeded(FingerprintManagerCompati.AuthenticationResult result)`** &ndash; これは指紋が認識されたときに呼び出されます。

`Authenticate` を呼び出すときに `CryptoObject` が使用された場合は、`OnAuthenticationSuccessful` で `Cipher.DoFinal` を呼び出すことをお勧めします。
暗号が改ざんされたか、不適切に初期化された場合に、`DoFinal` によって例外がスローされます。これは、指紋スキャナーの結果がアプリケーションの外部で改ざんされた可能性があることを示します。

> [!NOTE]
> コールバック クラスは比較的軽量に保ち、アプリケーション固有のロジックを含めないようにすることをお勧めします。 コールバックは、Android アプリケーションと指紋スキャナーの結果との間で "交通警官" として機能します。

## <a name="a-sample-authentication-callback-handler"></a>認証コールバック ハンドラーのサンプル

次のクラスは、最小限の `FingerprintManager.AuthenticationCallback` の実装例を示しています。 

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

`OnAuthenticationSucceeded` は、`Authentication` が呼び出されたときに `FingerprintManager` に `Cipher` が提供されたかどうかを確認します。 もしそうであれば、`DoFinal` メソッドが暗号で呼び出されます。 これにより、`Cipher` が閉じられ、その元の状態に復元されます。 暗号に問題があった場合は、`DoFinal` によって例外がスローされ、認証の試行は失敗したと見なされます。

`OnAuthenticationError` と `OnAuthenticationHelp` のコールバックはそれぞれ、問題の原因を示す整数を受け取ります。 次のセクションでは、使用可能なヘルプまたはエラー コードについてそれぞれ説明します。 2 つのコールバックは同様の目的で機能し &ndash; 指紋認証が失敗したことをアプリケーションに通知します。 相違点は重要度によって異なります。 `OnAuthenticationHelp` は、指紋が高速にスワイプされるなど、ユーザーが回復可能なエラーです。`OnAuthenticationError` は、指紋スキャナーが破損しているなど、より重大なエラーです。

`CancellationSignal.Cancel()` メッセージを使用して指紋スキャンがキャンセルされると、`OnAuthenticationError` が呼び出されることにご注意ください。 `errMsgId` パラメーターの値は 5 (`FingerprintState.ErrorCanceled`) になります。 要件によっては、`AuthenticationCallbacks` の実装によって、この状況を他のエラーとは異なる方法で処理されることがあります。 

`OnAuthenticationFailed` は、指紋が正常にスキャンされたが、デバイスに登録されている指紋と一致しなかった場合に呼び出されます。 

## <a name="help-codes-and-error-message-ids"></a>ヘルプ コードとエラー メッセージ ID 

エラー コードとヘルプ コードの一覧と説明については、FingerprintManager クラスの [Android SDK ドキュメント](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html#FINGERPRINT_ACQUIRED_GOOD)で見つけることができます。 Xamarin.Android は、これらの値を `Android.Hardware.Fingerprints.FingerprintState` 列挙型で表します。

- **`AcquiredGood`** &ndash; (値 0) 取得したイメージは良好でした。

- **`AcquiredImagerDirty`** &ndash; (値 3) センサーが汚れている可能性があるか、汚れが検出されたために、指紋イメージのノイズが多すぎました。 たとえば、複数の `AcquiredInsufficient` の後、またはセンサーの汚れ (スタック ピクセル、帯状など) を実際に検出した後に、これを返すのは妥当です。 ユーザーは、これが返された場合、センサーをクリーニングする操作を行う必要があります。

- **`AcquiredInsufficient`** &ndash; (値 2) 検出された状態 (ドライ スキン) または汚れている可能性があるセンサー (`AcquiredImagerDirty` を参照) のために、指紋イメージを処理するには、ノイズが多すぎました。

- **`AcquiredPartial`** &ndash; (値 1) 部分的な指紋イメージのみが検出されました。 登録時には、この問題を解決する必要があることを、ユーザーに通知する必要があります (例: &ldquo;センサーをしっかりと押します&rdquo;)。

- **`AcquiredTooFast`** &ndash; (値 5) クイック モーションのため、指紋イメージは不完全でした。 ほとんどが線形配列センサーに適していますが、これは取得中に指が移動された場合にも発生する可能性があります。 ユーザーは、指をもっとゆっくり (線形) 移動するか、または指をもっと長くセンサーに置いたままにするように求められます。

- **`AcquiredToSlow`** &ndash; (値 4) 動作がないため、指紋イメージを読み取ることができませんでした。 これは、スワイプ モーションが必要な線形配列センサーに最適です。

- **`ErrorCanceled`** &ndash; (値 5) 指紋センサーが使用できないため、操作が取り消されました。 たとえば、ユーザーが切り替えられた場合、デバイスがロックされている場合、または別の保留中の操作によってブロックまたは無効化された場合に発生する可能性があります。

- **`ErrorHwUnavailable`** &ndash; (値 1) ハードウェアが使用できません。 後で再試行してください。

- **`ErrorLockout`** &ndash; (値 7) 試行回数が多すぎるため、API がロックアウトされたことが理由で、操作が取り消されました。

- **`ErrorNoSpace`** &ndash; (値 4) 登録などの操作に対してエラー状態が返されました。操作を完了するのに十分なストレージが残っていないため、操作を完了できません。

- **`ErrorTimeout`** &ndash; (値 3) 現在の要求の実行時間が長すぎると、エラー状態が返されます。 これは、プログラムが指紋センサーを無期限に待機するのを防ぐためのものです。 タイムアウトはプラットフォームとセンサーに固有ですが、通常は約 30 秒です。

- **`ErrorUnableToProcess`** &ndash; (値 2) センサーが現在のイメージを処理できなかったときに、エラー状態が返されました。

## <a name="related-links"></a>関連リンク

- [暗号化](https://docs.oracle.com/javase/7/docs/api/javax/crypto/Cipher.html)
- [AuthenticationCallback](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)
- [AuthenticationCallback](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.AuthenticationCallback.html)
