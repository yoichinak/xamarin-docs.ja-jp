---
title: Android ビーム
ms.prod: xamarin
ms.assetid: 4172A798-89EC-444D-BC0C-0A7DD67EF98C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/06/2017
ms.openlocfilehash: 89e668b8936db9a05fca2353b334b630b8363a74
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="android-beam"></a>Android ビーム

Android のビームは、情報を共有するときに、近接 NFC 経由でアプリケーションを使用する Android 4.0 で導入された近距離通信 (NFC) テクノロジです。

[![情報の共有の近くに 2 つのデバイスを示すダイアグラム](android-beam-images/androidbeam.png)](android-beam-images/androidbeam.png#lightbox)

Android のビームは、2 つのデバイスが範囲内にある場合は、NFC 経由でメッセージをプッシュして動作します。 デバイス互いから約 4 cm では、Android ビームを使用してデータを共有できます。 1 つのデバイス上でアクティビティがメッセージを作成し、アクティビティ (アクティビティ) を指定します、プッシュを処理することができます。 フォア グラウンドでは、指定されたアクティビティと、デバイスが範囲内では、Android ビームは、メッセージを 2 番目のデバイスにプッシュされます。 受信側のデバイスでは、メッセージ データを含むインテントが呼び出されます。

Android には、Android ビーム設定メッセージの 2 つの方法がサポートされています。

-   `SetNdefPushMessage` -Android ビームを開始する前に、アプリケーションは、NdefMessage NFC、およびプッシュは、アクティビティをプッシュするを指定する SetNdefPushMessage を呼び出すことができます。 アプリケーションの使用中にメッセージを変更しないと、このメカニズムは適しています。

-   `SetNdefPushMessageCallback` -Android ビームが開始されると、アプリケーションは、NdefMessage を作成するコールバックを処理できます。 このメカニズムは、デバイスが範囲になるまで遅延するメッセージを作成できます。 アプリケーションで何が起こっているかに基づいて、メッセージが異なる場合がありますシナリオをサポートします。


どちらの場合、Android のビームを使用してデータを送信するアプリケーションに送信される、 `NdefMessage`、いくつかのデータをパッケージ化`NdefRecords`です。 Android ビームをトリガーして前に対処する必要がありますのキー_ポイントで見てをみましょう。 最初に、操作を行います。 作成のコールバック スタイル、`NdefMessage`です。


## <a name="creating-a-message"></a>メッセージを作成します。

ここでコールバックを登録することができます、`NfcAdapter`アクティビティの`OnCreate`メソッドです。 たとえばと仮定した場合、`NfcAdapter`という`mNfcAdapter`アクティビティで、クラス変数として宣言は、メッセージを構築するコールバックを作成する次のコードを記述できます。

```csharp
mNfcAdapter = NfcAdapter.GetDefaultAdapter (this);
mNfcAdapter.SetNdefPushMessageCallback (this, this);
```

実装すると、アクティビティ`NfcAdapter.ICreateNdefMessageCallback`に渡される、`SetNdefPushMessageCallback`上記のメソッドです。 Android ビームが開始されると、システムが呼び出す`CreateNdefMessage`からアクティビティを構築できます、`NdefMessage`次のようにします。

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


## <a name="receiving-a-message"></a>メッセージの受信

システム側では、受信側で目的が呼び出される、`ActionNdefDiscovered`抽出元とすること、NdefMessage 次のように、アクション。

```csharp
IParcelable [] rawMsgs = intent.GetParcelableArrayExtra (NfcAdapter.ExtraNdefMessages);
NdefMessage msg = (NdefMessage) rawMsgs [0];
```

以下のスクリーン ショットで実行されている Android のビームを使用する完全なコード例については、 [Android ビーム デモ](https://developer.xamarin.com/samples/monodroid/AndroidBeamDemo/)サンプル ギャラリーにします。

[![Android ビーム デモの例のスクリーン ショット](android-beam-images/24.png)](android-beam-images/24.png#lightbox)



## <a name="related-links"></a>関連リンク

- [Android ビーム デモ (サンプル)](https://developer.xamarin.com/samples/monodroid/AndroidBeamDemo/)
- [アイスクリーム サンドイッチの概要](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 プラットフォーム](http://developer.android.com/sdk/android-4.0.html)
