---
title: CryptoObject の作成
ms.prod: xamarin
ms.assetid: 4D159B80-FFF4-4136-A7F0-F8A5C6B86838
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: 871058d1c128b37a0f2e77b43587139efb433de1
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/10/2020
ms.locfileid: "75487777"
---
# <a name="creating-a-cryptoobject"></a>CryptoObject の作成

指紋認証の結果の整合性は、アプリケーションにとって重要です &ndash; それは、アプリケーションがユーザーの ID を認識する方法です。 理論的には、サードパーティのマルウェアが、指紋スキャナーによって返された結果をインターセプトして改ざんすることが可能です。 このセクションでは、指紋結果の有効性を維持するための 1 つの手法について説明します。 

`FingerprintManager.CryptoObject` は、Java 暗号化 API のラッパーであり、認証要求の整合性を保護するために `FingerprintManager` によって使用されます。 通常、`Javax.Crypto.Cipher` オブジェクトは、指紋スキャナーの結果を暗号化するためのメカニズムです。 `Cipher` オブジェクト自体は、Android Keystore API を使用してアプリケーションによって作成されたキーを使用します。

これらのクラスすべてがどのように連携するかを理解するために、まず、`CryptoObject` を作成する方法を示す次のコードを見てから、さらに詳しく説明します。

```csharp
public class CryptoObjectHelper
{
    // This can be key name you want. Should be unique for the app.
    static readonly string KEY_NAME = "com.xamarin.android.sample.fingerprint_authentication_key";

    // We always use this keystore on Android.
    static readonly string KEYSTORE_NAME = "AndroidKeyStore";

    // Should be no need to change these values.
    static readonly string KEY_ALGORITHM = KeyProperties.KeyAlgorithmAes;
    static readonly string BLOCK_MODE = KeyProperties.BlockModeCbc;
    static readonly string ENCRYPTION_PADDING = KeyProperties.EncryptionPaddingPkcs7;
    static readonly string TRANSFORMATION = KEY_ALGORITHM + "/" +
                                            BLOCK_MODE + "/" +
                                            ENCRYPTION_PADDING;
    readonly KeyStore _keystore;

    public CryptoObjectHelper()
    {
        _keystore = KeyStore.GetInstance(KEYSTORE_NAME);
        _keystore.Load(null);
    }

    public FingerprintManagerCompat.CryptoObject BuildCryptoObject()
    {
        Cipher cipher = CreateCipher();
        return new FingerprintManagerCompat.CryptoObject(cipher);
    }

    Cipher CreateCipher(bool retry = true)
    {
        IKey key = GetKey();
        Cipher cipher = Cipher.GetInstance(TRANSFORMATION);
        try
        {
            cipher.Init(CipherMode.EncryptMode, key);
        } catch(KeyPermanentlyInvalidatedException e)
        {
            _keystore.DeleteEntry(KEY_NAME);
            if(retry)
            {
                CreateCipher(false);
            } else
            {
                throw new Exception("Could not create the cipher for fingerprint authentication.", e);
            }
        }
        return cipher;
    }

    IKey GetKey()
    {
        IKey secretKey;
        if(!_keystore.IsKeyEntry(KEY_NAME))
        {
            CreateKey();
        }

        secretKey = _keystore.GetKey(KEY_NAME, null);
        return secretKey;
    }

    void CreateKey()
    {
        KeyGenerator keyGen = KeyGenerator.GetInstance(KeyProperties.KeyAlgorithmAes, KEYSTORE_NAME);
        KeyGenParameterSpec keyGenSpec =
            new KeyGenParameterSpec.Builder(KEY_NAME, KeyStorePurpose.Encrypt | KeyStorePurpose.Decrypt)
                .SetBlockModes(BLOCK_MODE)
                .SetEncryptionPaddings(ENCRYPTION_PADDING)
                .SetUserAuthenticationRequired(true)
                .Build();
        keyGen.Init(keyGenSpec);
        keyGen.GenerateKey();
    }
}
```

このサンプル コードでは、アプリケーションによって作成されたキーを使用して、`CryptoObject` ごとに新しい `Cipher` を作成します。 このキーは、`CryptoObjectHelper` クラスの先頭に設定された `KEY_NAME` 変数によって特定されます。 メソッド `GetKey` は、Android Keystore API を使用してキーの取得を試みます。 キーが存在しない場合は、メソッド `CreateKey` によって、アプリケーションの新しいキーが作成されます。

暗号は `Cipher.GetInstance` の呼び出しでインスタンス化され、"_変換_" (データを暗号化および暗号化解除する方法を暗号に指示する文字列値) を取得します。 `Cipher.Init` の呼び出しでは、アプリケーションからキーを提供することによって暗号の初期化が完了します。 

次のような状況によって Android でキーが無効化される可能性があることを認識することが重要です。 

- 新しい指紋がデバイスに登録された。
- デバイスに登録されている指紋がない。
- ユーザーが画面のロックを無効にした。
- ユーザーが画面のロック (screenlock の種類または使用されている PIN/パターン) を変更した。

この場合、`Cipher.Init` は [`KeyPermanentlyInvalidatedException`](https://developer.android.com/reference/android/security/keystore/KeyPermanentlyInvalidatedException.html) をスローします。 上記のサンプル コードでは、その例外をトラップし、キーを削除して、新しいものを作成します。

次のセクションでは、キーを作成してデバイスに格納する方法について説明します。

## <a name="creating-a-secret-key"></a>秘密鍵の作成

`CryptoObjectHelper` クラスでは、Android [`KeyGenerator`](xref:Javax.Crypto.KeyGenerator) を使用してキーを作成し、デバイスに格納します。 `KeyGenerator` クラスでは、キーを作成できますが、作成するキーの種類に関するメタデータが必要です。 この情報は、[`KeyGenParameterSpec`](https://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.html) クラスのインスタンスによって提供されます。 

`KeyGenerator` は、`GetInstance` ファクトリ メソッドを使用してインスタンス化されます。 このサンプル コードでは、暗号化アルゴリズムとして [_Advanced Encryption Standard_](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) (_AES_) を使用しています。 AES では、データが固定サイズのブロックに分割され、その各ブロックが暗号化されます。

次に、`KeyGenParameterSpec.Builder` を使用して `KeyGenParameterSpec` が作成されます。 `KeyGenParameterSpec.Builder` によって、作成されるキーに関する次の情報がラップされます。

- キー名。
- キーは、暗号化と暗号化解除の両方に対して有効である必要があります。
- このサンプル コードでは、`BLOCK_MODE` は "_暗号ブロック チェーン_" (`KeyProperties.BlockModeCbc`) に設定されています。つまり、各ブロックは前のブロックと XOR されます (各ブロック間の依存関係が作成されます)。 
- `CryptoObjectHelper` は ["_公開キー暗号化標準 #7_"](https://tools.ietf.org/html/rfc2315) (_PKCS7_) を使用して、ブロックを埋めるバイトを生成し、それらがすべて同じサイズになるようにします。
- `SetUserAuthenticationRequired(true)` は、キーを使用する前に、ユーザー認証が必要であることを意味します。

`KeyGenParameterSpec` が作成されると、それを使用して `KeyGenerator` が初期化されます。これにより、キーが生成され、デバイスに安全に格納されます。 

## <a name="using-the-cryptoobjecthelper"></a>CryptoObjectHelper の使用

サンプル コードで、`CryptoWrapper` を作成するためのロジックの大部分を `CryptoObjectHelper` クラスにカプセル化したので、このガイドの最初のコードを見直し、`CryptoObjectHelper` を使用して暗号を作成し、指紋スキャナーを起動します。 

```csharp
protected void FingerPrintAuthenticationExample()
{
    const int flags = 0; /* always zero (0) */
    
    CryptoObjectHelper cryptoHelper = new CryptoObjectHelper();
    cancellationSignal = new Android.Support.V4.OS.CancellationSignal();
    
    // Using the Support Library classes for maximum reach
    FingerprintManagerCompat fingerPrintManager = FingerprintManagerCompat.From(this);
    
    // AuthCallbacks is a C# class defined elsewhere in code.
    FingerprintManagerCompat.AuthenticationCallback authenticationCallback = new MyAuthCallbackSample(this);

    // Here is where the CryptoObjectHelper builds the CryptoObject. 
    fingerprintManager.Authenticate(cryptohelper.BuildCryptoObject(), flags, cancellationSignal, authenticationCallback, null);
}
```

`CryptoObject` を作成する方法を確認したので、次に、`FingerprintManager.AuthenticationCallbacks` を使用して指紋スキャナー サービスの結果を Android アプリケーションに転送する方法を確認しましょう。

## <a name="related-links"></a>関連リンク

- [暗号化](xref:Javax.Crypto.Cipher)
- [FingerprintManager.CryptoObject](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.CryptoObject.html)
- [FingerprintManagerCompat.CryptoObject](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.CryptoObject.html)
- [KeyGenerator](xref:Javax.Crypto.KeyGenerator)
- [KeyGenParameterSpec](https://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.html)
- [KeyGenParameterSpec.Builder](https://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.Builder.html)
- [KeyPermanentlyInvalidatedException](https://developer.android.com/reference/android/security/keystore/KeyPermanentlyInvalidatedException.html)
- [KeyProperties](https://developer.android.com/reference/android/security/keystore/KeyProperties.html)
- [AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)
- [RFC 2315 - PCKS #7](https://tools.ietf.org/html/rfc2315)
