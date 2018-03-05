---
title: "コア NFC"
description: "IOS 11 を使用してタグを読み取り近距離通信 (NFC)"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/25/2016
ms.openlocfilehash: 4975b4008c635ad2355ca2806ba867636dd50201
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="core-nfc"></a>コア NFC

_IOS 11 を使用してタグを読み取り近距離通信 (NFC)_

CoreNFC は ios 11 へのアクセスを提供する新しいフレームワーク、_近距離通信_をアプリ内からはタグを読み取る (NFC) オプション。 IPhone 7 で動作プラスの 7、8、さらに、および 8 です。

IOS デバイスの NFC タグ リーダーは、すべて NFC タグの種類 1. ~ 5. を含むをサポートしている_NFC データ交換形式_(NDEF) 情報。

これには注意すべきいくつかの制限があります。

- CoreNFC には、(書き込みや書式設定) の読み取りタグのみがサポートされます。
- タグのスキャンは、ユーザーが開始する必要があります、60 秒後にタイムアウトします。
- アプリは、スキャンの前景に表示する必要があります。
- CoreNFC は、(シミュレーター) ではなく実際のデバイスでのみテストできます。

このページは CoreNFC を使用するために必要な構成方法について説明し、API を使用して、使用する方法を示しています、[のサンプル コードは"TFCTagReader"](https://developer.xamarin.com/samples/monotouch/ios11/NFCTagReader/)です。

## <a name="configuration"></a>構成

CoreNFC を有効にするには、プロジェクトに 3 つの項目を構成する必要があります。

- **Info.plist**プライバシー キー。
- **Entitlements.plist**エントリです。
- プロビジョニング プロファイルが**NFC タグの読み取り**機能します。

### <a name="infoplist"></a>Info.plist

追加、 **NFCReaderUsageDescription**プライバシー キーとスキャンが発生しているユーザーに表示されるテキストです。 アプリケーションの適切なメッセージを使用して (たとえば、スキャンの目的を説明する)。

```xml
<key>NFCReaderUsageDescription</key>
<string>NFC tag to read NDEF messages into the application</string>
```

### <a name="entitlementsplist"></a>Entitlements.plist

アプリを要求する必要があります、**フィールド通信タグ読み取りの近く**の機能を使用して、次のキー/値のペア、 **Entitlements.plist**:

```xml
<key>com.apple.developer.nfc.readersession.formats</key>
<array>
  <string>NDEF</string>
</array>
```

### <a name="provisioning-profile"></a>プロファイルのプロビジョニング

新しい**アプリ ID**ことを確認して、 **NFC タグの読み取り**サービスには、チェック マークします。

[ ![選択されている NFC タグの読み取りと開発者のアプリ ID の新しいポータル ページ](corenfc-images/app-services-nfc-sml.png)](corenfc-images/app-services-nfc.png)

必要がありますし、このアプリ ID の新しいプロビジョニング プロファイルを作成し、ダウンロードして、開発ファルダにインストールします。

## <a name="reading-a-tag"></a>タグの読み取り

プロジェクトを構成すると後で追加`using CoreNFC;`フォローして、ファイルの先頭にこれら 3 つの手順を NFC を実装するタグ読み取り機能。

### <a name="1-implement-infcndefreadersessiondelegate"></a>1.実装 `INFCNdefReaderSessionDelegate`

インターフェイスでは、次の 2 つのメソッドを実装するには。

- `DidDetect` – タグが正常に読み取られたときに呼び出されます。
- `DidInvalidate` – エラーが発生したり、60 秒のタイムアウトに達したときに呼び出されます。

#### <a name="diddetect"></a>DidDetect

サンプル コードでは、スキャンされた各メッセージがテーブル ビューに追加されます。

```csharp
public void DidDetect(NFCNdefReaderSession session, NFCNdefMessage[] messages)
{
    foreach (NFCNdefMessage msg in messages)
    {  // adds the messages to a list view
        DetectedMessages.Add(msg);
    }
    DispatchQueue.MainQueue.DispatchAsync(() =>
    {
        this.TableView.ReloadData();
    });
}
```

このメソッドは、複数回呼び出すことができます (およびでメッセージの配列を渡すことができます) 場合は、セッションを複数のタグ読み取りデータを使用します。 3 番目のパラメーターを使用してこの設定は、`Start`メソッド (で説明した[手順 2.](#step2))。

#### <a name="didinvalidate"></a>DidInvalidate

無効化は、さまざまな理由から発生します。

- スキャン中にエラーが発生しました。
- アプリがフォア グラウンドで中断されることです。
- ユーザーは、スキャンをキャンセルしてを選択します。
- スキャンは、アプリによって取り消されました。

次のコードは、エラーを処理する方法を示しています。

```csharp
public void DidInvalidate(NFCNdefReaderSession session, NSError error)
{
    var readerError = (NFCReaderError)(long)error.Code;
    if (readerError != NFCReaderError.ReaderSessionInvalidationErrorFirstNDEFTagRead &&
        readerError != NFCReaderError.ReaderSessionInvalidationErrorUserCanceled)
    {
      // some error handling
    }
}
```

セッションが、無効になった後をもう一度スキャンを新しいセッション オブジェクトを作成する必要があります。

<a name="step2" />

### <a name="2-start-an-nfcndefreadersession"></a>2.開始します。 `NFCNdefReaderSession`

スキャンは、ボタン押下などのユーザー要求に開始する必要があります。
次のコードでは、作成し、スキャン セッションを開始します。

```csharp
Session = new NFCNdefReaderSession(this, null, true);
Session?.BeginSession();
```

パラメーター、`NFCNdefReaderSession`コンス トラクターは、次のようにします。

- `delegate` – の実装`INFCNdefReaderSessionDelegate`です。 サンプル コードで、デリゲートは実装されて、テーブル ビュー コント ローラーで、したがって`this`デリゲートのパラメーターとして使用されます。
- `queue` – でコールバックが処理されるキューです。 できます`null`、その場合、必ず使用して、`DispatchQueue.MainQueue`に示すように、サンプル) は、ユーザー インターフェイス コントロールを更新するときにします。
- `invalidateAfterFirstRead` – とき`true`、; 最初の正常なスキャン後のスキャンの停止時に`false`スキャンは続行され、スキャンがキャンセルされたか、60 秒のタイムアウトに達するとなるまで、複数の結果が返されます。


### <a name="3-cancel-the-scanning-session"></a>3.スキャンのセッションを取り消す

ユーザーは、ユーザー インターフェイスでシステムによって提供されるボタンを使用してスキャン セッションを取り消すことができます。

![スキャン中に [キャンセル] ボタン](corenfc-images/scan-cancel-sml.png)

アプリが呼び出すことによって、スキャンをプログラムで取り消すことができます、`InvalidateSession`メソッド。

```csharp
Session.InvalidateSession();
```

どちらの場合、デリゲートの`DidInvalidate`メソッドが呼び出されます。

## <a name="summary"></a>まとめ

CoreNFC には、NFC タグからデータを読み取る、アプリができるようにします。 さまざまなタグの形式 (NDEF 型 1 ~ 5) の読み取りをサポートしていますが、書き込み、書式設定やサポートされていません。


## <a name="related-links"></a>関連リンク

- [NFCTagReader (サンプル)](https://developer.xamarin.com/samples/monotouch/ios11/NFCTagReader/)
- [コア NFC (WWDC) (ビデオ) の概要](https://developer.apple.com/videos/play/wwdc2017/718/)
