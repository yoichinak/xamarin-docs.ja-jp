---
title: "CryptoObject を作成します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 4D159B80-FFF4-4136-A7F0-F8A5C6B86838
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 6d383bc6077dc0286fa5ee4ae7e88df2d62e4664
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="creating-a-cryptoobject"></a>CryptoObject を作成します。

指紋認証結果の整合性に問題がアプリケーションに重要な&ndash;アプリケーションがユーザーの本人性を確認する方法をお勧めします。 サード パーティ製マルウェアをインターセプトし、指紋スキャナーによって返される結果を改ざんすることは理論的には。 このセクションでは、指紋結果の有効性を保持するための 1 つの方法を説明します。 

`FingerprintManager.CryptoObject` Java cryptography Api をラップするラッパーで使用される、`FingerprintManager`認証要求の整合性を保護します。 通常、`Javax.Crypto.Cipher`オブジェクトは、指紋スキャナーの結果を暗号化するためのメカニズムです。 `Cipher`オブジェクト自体では、Android キーストア Api を使用するアプリケーションによって作成されるキーを使用します。

これらすべてのクラスの動作を理解するには、まずを見てみましょう、次のコードを作成する方法を示します、 `CryptoObject`、し、さらに詳しく説明します。

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
            cipher.Init(CipherMode.EncryptMode | CipherMode.DecryptMode, key);
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

新しいサンプル コードが作成されます`Cipher`各`CryptoObject`、アプリケーションによって作成されたキーを使用します。 キーが識別される、`KEY_NAME`の先頭で設定されている変数、`CryptoObjectHelper`クラスです。 メソッド`GetKey`Android キーストア Api を使用して、キーの取得し、なります。 キーが存在しないかどうか、メソッド`CreateKey`アプリケーションの新しいキーが作成されます。

暗号への呼び出しによってインスタンス化`Cipher.GetInstance`、取得、_変換_(暗号文の暗号化し、データの暗号化を解除する方法を示す文字列値)。 呼び出し`Cipher.Init`アプリケーションからのキーを入力して暗号の初期化を完了します。 

これは、状況によっては、Android が無効になるキーがあることに注意。 

* 新しい指紋がデバイスに登録されています。
* デバイスに登録されている指紋はありません。
* ユーザーは画面のロックを無効にします。
* ユーザーが画面のロック (、screenlock または使用する PIN/パターンの種類) を変更します。

この場合、`Cipher.Init`がスローされます、 [ `KeyPermanentlyInvalidatedException`](http://developer.android.com/reference/android/security/keystore/KeyPermanentlyInvalidatedException.html)です。 上記のサンプル コードは例外をトラップ、キーを削除し、新しく作成します。

次のセクションでは、キーを作成し、デバイスに保存する方法を説明します。

## <a name="creating-a-secret-key"></a>秘密キーを作成します。

`CryptoObjectHelper`クラスは、Android を使用して[ `KeyGenerator` ](https://developer.xamarin.com/api/type/Javax.Crypto.KeyGenerator/)にキーを作成し、デバイスに保存します。 `KeyGenerator`クラスを作成できる、キーが、一部に関するメタデータは、キーの種類を作成します。 インスタンスでこの情報が提供される、 [ `KeyGenParameterSpec` ](http://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.html)クラスです。 

A`KeyGenerator`を使用してインスタンス化される、`GetInstance`ファクトリ メソッド。 サンプル コードを使用して、 [ _Advanced Encryption Standard_ ](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) (_AES_)、暗号化アルゴリズムとして。 AES は、固定サイズのブロックにデータを分割し、それらのブロックのそれぞれを暗号化します。

次に、`KeyGenParameterSpec`を使用して作成されて、`KeyGenParameterSpec.Builder`です。 `KeyGenParameterSpec.Builder`が作成されるキーに関する次の情報をラップします。

* キー名。
* キーを暗号化および暗号化解除の両方に対して有効にする必要があります。
* サンプル コード、`BLOCK_MODE`に設定されている_暗号ブロック チェーン_(`KeyProperties.BlockModeCbc`)、各ブロックが (各ブロック間の依存関係を作成する)、前のブロックと Xor ことを意味します。 
* `CryptoObjectHelper`使用[ _Public Key Cryptography Standard #7_ ](https://tools.ietf.org/html/rfc2315) (_PKCS7_) は、それらがすべて同じサイズであることを確認するブロック埋めるされるバイト数を生成するには.
* `SetUserAuthenticationRequired(true)` キーを使用する前に、ユーザー認証が必要なことを意味します。

1 回、`KeyGenParameterSpec`が作成されると、初期化に使用されて、`KeyGenerator`は、キーを生成し、デバイス上に安全に保存します。 

## <a name="using-the-cryptoobjecthelper"></a>使用して、CryptoObjectHelper

これで、サンプル コードは、ロジックを作成するための多くをカプセル化されたが、`CryptoWrapper`に、`CryptoObjectHelper`クラス、みましょうこのガイドの先頭からコードを再確認しを使用して、`CryptoObjectHelper`暗号を作成して指紋スキャナーを開始します。 

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

作成する方法を説明しました、`CryptoObject`を参照してくださいに進むことができます、`FingerprintManager.AuthenticationCallbacks`指紋スキャナーのサービスの結果を Android アプリケーションに転送するために使用します。



## <a name="related-links"></a>関連リンク

- [Cipher](https://developer.xamarin.com/api/type/Javax.Crypto.Cipher/)
- [FingerprintManager.CryptoObject](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.CryptoObject.html)
- [FingerprintManagerCompat.CryptoObject](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.CryptoObject.html)
- [KeyGenerator](https://developer.xamarin.com/api/type/Javax.Crypto.KeyGenerator/)
- [KeyGenParameterSpec](http://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.html)
- [KeyGenParameterSpec.Builder](http://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.Builder.html)
- [KeyPermanentlyInvalidatedException](http://developer.android.com/reference/android/security/keystore/KeyPermanentlyInvalidatedException.html)
- [KeyProperties](http://developer.android.com/reference/android/security/keystore/KeyProperties.html)
- [AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)
- [RFC 2315 - PCKS #7](https://tools.ietf.org/html/rfc2315)
