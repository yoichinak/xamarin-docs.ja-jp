---
title: CryptoObject の作成
ms.prod: xamarin
ms.assetid: 4D159B80-FFF4-4136-A7F0-F8A5C6B86838
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 1658934bedce11a42701eb023a42fc9e617b654d
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50113769"
---
# <a name="creating-a-cryptoobject"></a>CryptoObject の作成

指紋認証の結果の整合性がアプリケーションに重要な&ndash;アプリケーションがユーザーの本人性を確認する方法をお勧めします。 理論的には、サード パーティ製マルウェアをインターセプトし、指紋スキャナーによって返される結果を改ざんすることができます。 このセクションでは、指紋結果の有効性を保持する機能の 1 つの方法について説明します。 

`FingerprintManager.CryptoObject` Java cryptography Api のラッパーを使って、`FingerprintManager`認証要求の整合性を保護します。 通常、`Javax.Crypto.Cipher`オブジェクトは、指紋スキャナーの結果を暗号化するためのメカニズムです。 `Cipher`オブジェクト自体は、Android キーストアの Api を使用して、アプリケーションによって作成されるキーを使用します。

これらのクラスはすべて連携させる方法については、まずを見てみましょうを作成する方法については、次のコードを`CryptoObject`、し、さらに詳しく説明します。

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

新しいサンプル コードが作成されます`Cipher`各`CryptoObject`、アプリケーションによって作成されたキーを使用します。 キーがで識別される、`KEY_NAME`の先頭で設定された変数、`CryptoObjectHelper`クラス。 メソッド`GetKey`Android キーストアの Api を使用してキーの取得し、なります。 キーが存在しないかどうか、メソッド`CreateKey`アプリケーション用の新しいキーが作成されます。

呼び出しがインスタンス化する暗号文`Cipher.GetInstance`を受け入れて、_変換_(暗号の暗号化し、データの暗号化を解除する方法を指示する文字列値)。 呼び出し`Cipher.Init`アプリケーションからキーを提供することで、暗号の初期化を完了します。 

状況によっては、Android が無効になるキーがあることを理解する必要があります。 

* 新しいフィンガー プリントは、デバイスに登録されています。
* デバイスに登録されている指紋はありません。
* ユーザーは画面のロックを無効にします。
* ユーザーが画面のロック (、screenlock または使用する PIN/パターンの種類) を変更します。

この場合、`Cipher.Init`がスローされます、 [ `KeyPermanentlyInvalidatedException`](http://developer.android.com/reference/android/security/keystore/KeyPermanentlyInvalidatedException.html)します。 上記のサンプル コードは例外をトラップ、キーを削除し、新しいものを作成します。

次のセクションには、キーを作成し、デバイスに保存する方法について説明します。

## <a name="creating-a-secret-key"></a>秘密キーを作成します。

`CryptoObjectHelper`クラスは、Android を使用して[ `KeyGenerator` ](https://developer.xamarin.com/api/type/Javax.Crypto.KeyGenerator/)キーを作成し、デバイスに保存します。 `KeyGenerator`クラス キーを作成、必要があるいくつかに関するメタデータを作成するキーの種類。 この情報がのインスタンスによって提供される、 [ `KeyGenParameterSpec` ](http://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.html)クラス。 

A`KeyGenerator`を使用してインスタンス化される、`GetInstance`ファクトリ メソッド。 サンプル コードを使用して、 [ _Advanced Encryption Standard_ ](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) (_AES_) 暗号化アルゴリズムとして。 AES は固定サイズのブロックにデータを分割し、それらのブロックの暗号化します。

次に、`KeyGenParameterSpec`を使用して作成されて、`KeyGenParameterSpec.Builder`します。 `KeyGenParameterSpec.Builder`が作成されるキーの詳細については、次の情報をラップします。

* キー名。
* キーは、暗号化と復号化の両方に対して有効である必要があります。
* サンプル コードで、`BLOCK_MODE`に設定されている_暗号ブロック連鎖_(`KeyProperties.BlockModeCbc`)、各ブロックは、前のブロック (各ブロック間の依存関係を作成する) で、xor を実行したことを意味します。 
* `CryptoObjectHelper`使用[_公開キー暗号化標準 #7_ ](https://tools.ietf.org/html/rfc2315) (_PKCS7_) 保つにはすべて同じサイズのブロックが埋め込まれますされるバイト数を生成するには.
* `SetUserAuthenticationRequired(true)` キーを使用する前に、そのユーザーの認証が必要です。

1 回、`KeyGenParameterSpec`が作成されると、初期化に使用されます、`KeyGenerator`キーを生成し、デバイスに安全に保存するされます。 

## <a name="using-the-cryptoobjecthelper"></a>CryptoObjectHelper を使用します。

これでサンプル コードは、ロジックを作成するための多くをカプセル化されたが、`CryptoWrapper`に、`CryptoObjectHelper`クラスこのガイドの先頭からコードを再確認してみましょう使用、`CryptoObjectHelper`暗号を作成し、指紋スキャナーを起動します。 

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

作成する方法を学習しました、`CryptoObject`を参照してくださいに進むことができます、`FingerprintManager.AuthenticationCallbacks`指紋スキャナーのサービスの結果を Android アプリケーションに転送するために使用します。



## <a name="related-links"></a>関連リンク

- [暗号](https://developer.xamarin.com/api/type/Javax.Crypto.Cipher/)
- [FingerprintManager.CryptoObject](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.CryptoObject.html)
- [FingerprintManagerCompat.CryptoObject](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.CryptoObject.html)
- [KeyGenerator](https://developer.xamarin.com/api/type/Javax.Crypto.KeyGenerator/)
- [KeyGenParameterSpec](http://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.html)
- [KeyGenParameterSpec.Builder](http://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.Builder.html)
- [KeyPermanentlyInvalidatedException](http://developer.android.com/reference/android/security/keystore/KeyPermanentlyInvalidatedException.html)
- [KeyProperties](http://developer.android.com/reference/android/security/keystore/KeyProperties.html)
- [AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)
- [RFC 2315 - PCKS #7](https://tools.ietf.org/html/rfc2315)
