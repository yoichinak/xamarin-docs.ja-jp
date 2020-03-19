---
title: Android ビーム
ms.prod: xamarin
ms.assetid: 4172A798-89EC-444D-BC0C-0A7DD67EF98C
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/06/2017
ms.openlocfilehash: aab121ed5f811baf38eed48cf891ccdf076eaf44
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/10/2020
ms.locfileid: "76723813"
---
# <a name="android-beam"></a>Android ビーム

Android ビームは、Android 4.0 で導入された近距離無線通信 (NFC) テクノロジで、アプリケーションが近接している場合に NFC 経由で情報を共有できるようにします。

[![近接している 2 つのデバイスが情報を共有していることを示す図](android-beam-images/androidbeam.png)](android-beam-images/androidbeam.png#lightbox)

Android ビームは、2 つのデバイスが範囲内にある場合に、NFC 経由でメッセージをプッシュすることで機能します。 約 4 cm 離れているデバイス同士は、Android ビームを使用してデータを共有できます。 1 つのデバイスのアクティビティによってメッセージが作成され、それをプッシュできる 1 つ (または複数の) アクティビティが指定されます。 指定されたアクティビティがフォアグラウンドにあり、両方のデバイスが範囲内にある場合、Android ビームは 2 番目のデバイスにメッセージをプッシュします。 受信側のデバイスでは、メッセージ データを含む Intent が呼び出されます。

Android では、Android ビームでメッセージを設定する次の 2 つの方法がサポートされています。

- `SetNdefPushMessage` - Android ビームが開始される前に、アプリケーションは SetNdefPushMessage を呼び出して、NFC 経由でプッシュする NdefMessage とそれをプッシュするアクティビティを指定できます。 このメカニズムは、アプリケーションの使用中にメッセージが変更されない場合に使用するのが最適です。

- `SetNdefPushMessageCallback` - Android ビームが開始されると、アプリケーションは、NdefMessage を作成するためにコールバックを処理できます。 このメカニズムにより、デバイスが範囲内に入るまでメッセージの作成を遅らせることができます。 これは、アプリケーションで発生していることに応じてメッセージが変化する場合があるシナリオをサポートします。

いずれの場合もアプリケーションは、Android ビームでデータを送信するために、`NdefMessage` を送信し、データを複数の `NdefRecords` にパッケージ化します。 Android ビームをトリガーする前に対処する必要がある重要な点を確認していきます。 まず、`NdefMessage` を作成するコールバック スタイルを扱います。

## <a name="creating-a-message"></a>メッセージの作成

アクティビティの `OnCreate` メソッドの `NfcAdapter` にコールバックを登録できます。 たとえば、`mNfcAdapter` という名前の `NfcAdapter` がアクティビティでクラス変数として宣言されていると仮定すると、次のコードを記述して、メッセージを構築するコールバックを作成できます。

```csharp
mNfcAdapter = NfcAdapter.GetDefaultAdapter (this);
mNfcAdapter.SetNdefPushMessageCallback (this, this);
```

`NfcAdapter.ICreateNdefMessageCallback` を実装するアクティビティは、上記の `SetNdefPushMessageCallback` メソッドに渡されます。 Android ビームが開始されると、システムによって `CreateNdefMessage` が呼び出され、アクティビティはそこから、次に示すように `NdefMessage` を構築できます。

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

受信側では、システムによって `ActionNdefDiscovered` アクションを持つ Intent が呼び出され、そこから次のように NdefMessage を抽出できます。

```csharp
IParcelable [] rawMsgs = intent.GetParcelableArrayExtra (NfcAdapter.ExtraNdefMessages);
NdefMessage msg = (NdefMessage) rawMsgs [0];
```

次のスクリーンショットで実行されている Android ビームを使用する完全なコード例については、サンプル ギャラリーの [Android ビーム デモ](https://docs.microsoft.com/samples/xamarin/monodroid-samples/androidbeamdemo) を参照してください。

[![Android ビーム デモのスクリーンショットの例](android-beam-images/24.png)](android-beam-images/24.png#lightbox)

## <a name="related-links"></a>関連リンク

- [Android ビーム デモ (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/androidbeamdemo)
