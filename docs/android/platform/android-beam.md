---
title: Android ビーム
ms.prod: xamarin
ms.assetid: 4172A798-89EC-444D-BC0C-0A7DD67EF98C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/06/2017
ms.openlocfilehash: 0c4f7303d3620dcc2c829d732fe7a5f97f0e3883
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68643756"
---
# <a name="android-beam"></a>Android ビーム

Android ビームは Android 4.0 で導入された近距離無線通信 (NFC) テクノロジで、近接しているときにアプリケーションが NFC 経由で情報を共有できるようにします。

[![近接共有情報のある2つのデバイスを示す図](android-beam-images/androidbeam.png)](android-beam-images/androidbeam.png#lightbox)

Android ビームは、2つのデバイスが範囲内にある場合に、NFC 経由でメッセージをプッシュすることで機能します。 約4cm のデバイスは、Android ビームを使用してデータを共有できます。 1つのデバイスのアクティビティによってメッセージが作成され、プッシュを処理できるアクティビティ (またはアクティビティ) が指定されます。 指定されたアクティビティがフォアグラウンドにあり、デバイスが範囲内にある場合、Android ビームは2番目のデバイスにメッセージをプッシュします。 受信側のデバイスでは、メッセージデータを含むインテントが呼び出されます。

Android では、Android ビームでメッセージを設定する2つの方法がサポートされています。

-   `SetNdefPushMessage`-Android ビームが開始される前に、アプリケーションは SetNdefPushMessage を呼び出して、NFC 経由でプッシュする NdefMessage とそれをプッシュするアクティビティを指定できます。 このメカニズムは、アプリケーションの使用中にメッセージが変更されない場合に最適です。

-   `SetNdefPushMessageCallback`-Android ビームが開始されると、アプリケーションは、NdefMessage を作成するためのコールバックを処理できます。 このメカニズムにより、デバイスが範囲内になるまでメッセージの作成を遅延させることができます。 アプリケーションで発生している内容に応じてメッセージが変化するシナリオをサポートします。


どちらの場合も、Android ビームでデータを送信するために、 `NdefMessage`アプリケーションはを送信し、 `NdefRecords`データをいくつかの方法でパッケージ化します。 Android ビームをトリガーする前に対処する必要がある重要な点を見てみましょう。 まず、を作成するため`NdefMessage`のコールバックスタイルを使用します。


## <a name="creating-a-message"></a>メッセージの作成

アクティビティの`NfcAdapter` `OnCreate`メソッドでコールバックをに登録できます。 たとえば、 `NfcAdapter`という名前`mNfcAdapter`のがアクティビティでクラス変数として宣言されているとします。次のコードを記述して、メッセージを構築するコールバックを作成できます。

```csharp
mNfcAdapter = NfcAdapter.GetDefaultAdapter (this);
mNfcAdapter.SetNdefPushMessageCallback (this, this);
```

を実装`NfcAdapter.ICreateNdefMessageCallback`するアクティビティは、上記の`SetNdefPushMessageCallback`メソッドに渡されます。 Android ビームが開始されると、システムは`CreateNdefMessage`を呼び出します。このとき、アクティビティ`NdefMessage`は次のようにを構築できます。

```csharp
public NdefMessage CreateNdefMessage (NfcEvent evt)
{
    DateTime time = DateTime.Now;
    var text = ("Beam me up!\n\n" + "Beam Time: " +
        time.ToString ("HH:mm:ss"));
    NdefMessage msg = new NdefMessage (
        new NdefRecord[]{ CreateMimeRecord (
            "application/com.example.android.beam",
            Encoding.UTF8.GetBytes (text)) });
        } };
    return msg;
}

public NdefRecord CreateMimeRecord (String mimeType, byte [] payload)
{
    byte [] mimeBytes = Encoding.UTF8.GetBytes (mimeType);
    NdefRecord mimeRecord = new NdefRecord (
        NdefRecord.TnfMimeMedia, mimeBytes, new byte [0], payload);
    return mimeRecord;
}
```


## <a name="receiving-a-message"></a>メッセージを受信する

受信側では、システムは`ActionNdefDiscovered`アクションを使用してインテントを呼び出します。この場合、次のように ndefmessage を抽出できます。

```csharp
IParcelable [] rawMsgs = intent.GetParcelableArrayExtra (NfcAdapter.ExtraNdefMessages);
NdefMessage msg = (NdefMessage) rawMsgs [0];
```

以下のスクリーンショットのように、Android ビームを使用する完全なコード例については、サンプルギャラリーの[Android ビームデモ](https://docs.microsoft.com/samples/xamarin/monodroid-samples/androidbeamdemo)を参照してください。

[![Android ビームデモのスクリーンショットの例](android-beam-images/24.png)](android-beam-images/24.png#lightbox)



## <a name="related-links"></a>関連リンク

- [Android ビームデモ (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/androidbeamdemo)
- [アイスクリームサンドイッチの導入](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 プラットフォーム](https://developer.android.com/sdk/android-4.0.html)
