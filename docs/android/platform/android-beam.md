---
title: Android ビーム
ms.prod: xamarin
ms.assetid: 4172A798-89EC-444D-BC0C-0A7DD67EF98C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/06/2017
ms.openlocfilehash: 13a0a0d9c6a9d1d5f49020b1a8096f5e054d415c
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50114926"
---
# <a name="android-beam"></a>Android ビーム

Android ビームは、NFC の近くにあるときに情報を共有するアプリケーションを使用する Android 4.0 で導入された近距離通信 (NFC) テクノロジです。

[![情報の共有の近くに 2 つのデバイスを示す図](android-beam-images/androidbeam.png)](android-beam-images/androidbeam.png#lightbox)

Android ビームは、2 つのデバイスが範囲内にあるときに、NFC 経由でメッセージをプッシュすることによって機能します。 デバイスから他の約 4 cm には、Android ビームを使用してデータを共有できます。 アクティビティを 1 つのデバイスがメッセージを作成し、アクティビティ (またはアクティビティ) を指定します。 プッシュを処理することができます。 指定されたアクティビティがフォア グラウンドと、デバイスが範囲内にある、Android ビームは 2 つ目のデバイスにメッセージをプッシュします。 受信側のデバイスでは、メッセージ データを含む、インテントが呼び出されます。

Android では、Android ビーム設定メッセージの 2 つの方法がサポートされています。

-   `SetNdefPushMessage` -Android ビームが開始されると、前に、アプリケーションは、NdefMessage NFC、およびそれをプッシュしているアクティビティをプッシュするを指定する SetNdefPushMessage を呼び出すことができます。 アプリケーションを使用中に、メッセージは変更されないときに、このメカニズムは適しています。

-   `SetNdefPushMessageCallback` -Android ビームが開始されるとき、アプリケーションは、NdefMessage を作成するコールバックを処理できます。 このメカニズムにより、デバイスが範囲内にあるまでが遅延するメッセージの作成。 アプリケーションで何が起こっているかに基づいて、メッセージが異なる場合がありますシナリオをサポートします。


いずれの場合も、Android ビームを使用してデータを送信するアプリケーションに送信される、 `NdefMessage`、いくつかのデータをパッケージ化`NdefRecords`します。 Android ビームをトリガーした前に対処しなければならない重要な点を見ていきましょう。 作成するコールバック スタイルを使用して操作を行います。 まず、、`NdefMessage`します。


## <a name="creating-a-message"></a>メッセージを作成します。

使用したコールバックを登録することができます、 `NfcAdapter` 、アクティビティの`OnCreate`メソッド。 たとえば、`NfcAdapter`という名前`mNfcAdapter`アクティビティでは、クラス変数として宣言は、メッセージを構築するコールバックを作成する次のコードを記述します。

```csharp
mNfcAdapter = NfcAdapter.GetDefaultAdapter (this);
mNfcAdapter.SetNdefPushMessageCallback (this, this);
```

実装すると、アクティビティ`NfcAdapter.ICreateNdefMessageCallback`に渡される、`SetNdefPushMessageCallback`上記のメソッド。 Android ビームが開始されると、システムが呼び出さ`CreateNdefMessage`、アクティビティを構築できますから、`NdefMessage`次に示します。

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

受信側では、システムには、インテントが呼び出される、`ActionNdefDiscovered`元を抽出できます、NdefMessage 次のように、アクション。

```csharp
IParcelable [] rawMsgs = intent.GetParcelableArrayExtra (NfcAdapter.ExtraNdefMessages);
NdefMessage msg = (NdefMessage) rawMsgs [0];
```

、次のスクリーン ショットで実行されている Android ビームを使用する完全なコード例については、 [Android ビーム デモ](https://developer.xamarin.com/samples/monodroid/AndroidBeamDemo/)サンプル ギャラリーにします。

[![Android ビーム デモの例のスクリーン ショット](android-beam-images/24.png)](android-beam-images/24.png#lightbox)



## <a name="related-links"></a>関連リンク

- [Android ビーム デモ (サンプル)](https://developer.xamarin.com/samples/monodroid/AndroidBeamDemo/)
- [Ice Cream Sandwich の概要](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 プラットフォーム](http://developer.android.com/sdk/android-4.0.html)
