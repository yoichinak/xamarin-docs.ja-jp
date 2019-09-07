---
title: CryptoObject の作成
ms.prod: xamarin
ms.assetid: 4D159B80-FFF4-4136-A7F0-F8A5C6B86838
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 7328792e0d921beb09389d9a0400ea97766b2246
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70756446"
---
# <a name="creating-a-cryptoobject"></a>CryptoObject の作成

指紋認証の結果の整合性は、アプリケーションに&ndash;とって重要です。これは、アプリケーションがユーザーの id を認識する方法です。 サードパーティのマルウェアが指紋スキャナーによって返された結果をインターセプトし、改ざんする可能性があります。 このセクションでは、指紋結果の有効性を維持するための1つの方法について説明します。 

は、Java 暗号化 api のラッパーであり、に`FingerprintManager`よって認証要求の整合性を保護するために使用されます。 `FingerprintManager.CryptoObject` 通常、 `Javax.Crypto.Cipher`オブジェクトは、指紋スキャナーの結果を暗号化するためのメカニズムです。 `Cipher`オブジェクト自体は、Android キーストア api を使用して、アプリケーションによって作成されたキーを使用します。

これらのクラスがどのように連携するかを理解するために、まずを作成する方法を示す`CryptoObject`次のコードを見ていきます。詳細については、「」を参照してください。

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

このサンプルコードでは、アプリケーション`Cipher`によっ`CryptoObject`て作成されたキーを使用して、それぞれに新しいを作成します。 キーは、 `CryptoObjectHelper`クラスの先頭`KEY_NAME`に設定された変数によって識別されます。 メソッド`GetKey`は、Android キーストア api を使用してキーを検索し、取得します。 キーが存在しない場合は、メソッド`CreateKey`によって、アプリケーションの新しいキーが作成されます。

暗号は、の呼び出し`Cipher.GetInstance`を使用してインスタンス化され、_変換_(データの暗号化と復号化の方法を暗号に示す文字列値) を取得します。 を`Cipher.Init`呼び出すと、アプリケーションからキーが提供され、暗号の初期化が完了します。 

Android でキーが無効になる場合があることに注意してください。 

- 新しい指紋がデバイスに登録されました。
- デバイスに登録されている指紋がありません。
- ユーザーが画面ロックを無効にしました。
- ユーザーが画面のロック (screenlock の種類または使用されている PIN/パターン) を変更しました。

これが発生する`Cipher.Init`と、は[`KeyPermanentlyInvalidatedException`](https://developer.android.com/reference/android/security/keystore/KeyPermanentlyInvalidatedException.html)をスローします。 上記のサンプルコードでは、その例外をトラップし、キーを削除して、新しいものを作成します。

次のセクションでは、キーを作成してデバイスに格納する方法について説明します。

## <a name="creating-a-secret-key"></a>秘密キーの作成

クラス`CryptoObjectHelper`は Android [`KeyGenerator`](xref:Javax.Crypto.KeyGenerator)を使用してキーを作成し、デバイスに格納します。 クラス`KeyGenerator`はキーを作成できますが、作成するキーの種類に関するメタデータがいくつか必要です。 この情報は、 [`KeyGenParameterSpec`](https://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.html)クラスのインスタンスによって提供されます。 

は、 `GetInstance`ファクトリメソッドを使用してインスタンス化されます。`KeyGenerator` このサンプルコードでは、暗号化アルゴリズムとして[_Advanced Encryption Standard_](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) (_AES_) を使用しています。 AES は、固定サイズのブロックにデータを分割し、それらの各ブロックを暗号化します。

次に`KeyGenParameterSpec` 、を`KeyGenParameterSpec.Builder`使用して、が作成されます。 は`KeyGenParameterSpec.Builder` 、作成されるキーに関する次の情報をラップします。

- キー名。
- キーは、暗号化と復号化の両方に対して有効である必要があります。
- サンプルコードでは、 `BLOCK_MODE`は_暗号ブロックチェーン_(`KeyProperties.BlockModeCbc`) に設定されています。つまり、各ブロックは前のブロックと xor されます (各ブロック間の依存関係が作成されます)。 
- は`CryptoObjectHelper` 、[_公開キー暗号化標準 #7_](https://tools.ietf.org/html/rfc2315) (_PKCS7_) を使用して、ブロックに埋め込まれるバイトを生成し、それらがすべて同じサイズであることを確認します。
- `SetUserAuthenticationRequired(true)`キーを使用する前に、ユーザー認証が必要であることを意味します。

を作成した後は、キーを生成し`KeyGenerator`てデバイスに安全に格納するを初期化するために使用されます。 `KeyGenParameterSpec` 

## <a name="using-the-cryptoobjecthelper"></a>CryptoObjectHelper を使用する

このサンプルコードでは、 `CryptoWrapper` `CryptoObjectHelper`クラスにを作成するためのロジックの多くをカプセル化しました。次に、 `CryptoObjectHelper`このガイドの最初のコードを再確認し、を使用して暗号を作成し、指紋スキャナーを起動します。 

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

を作成`CryptoObject`する方法について説明したので、ここでは、 `FingerprintManager.AuthenticationCallbacks`を使用して、指紋スキャナーサービスの結果を Android アプリケーションに転送する方法を確認します。

## <a name="related-links"></a>関連リンク

- [暗号](xref:Javax.Crypto.Cipher)
- [FingerprintManager.CryptoObject](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.CryptoObject.html)
- [FingerprintManagerCompat.CryptoObject](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.CryptoObject.html)
- [KeyGenerator](xref:Javax.Crypto.KeyGenerator)
- [KeyGenParameterSpec](https://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.html)
- [KeyGenParameterSpec.Builder](https://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.Builder.html)
- [KeyPermanentlyInvalidatedException](https://developer.android.com/reference/android/security/keystore/KeyPermanentlyInvalidatedException.html)
- [KeyProperties](https://developer.android.com/reference/android/security/keystore/KeyProperties.html)
- [AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)
- [RFC 2315 - PCKS #7](https://tools.ietf.org/html/rfc2315)
