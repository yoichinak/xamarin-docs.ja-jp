---
title: Xamarin のコア NFC
description: このドキュメントでは、iOS 11 で導入された Api を使用して、Xamarin iOS の近距離無線通信タグを読み取る方法について説明します。
ms.prod: xamarin
ms.technology: xamarin-ios
ms.assetid: 846B59D3-F66A-48F3-A78C-84217697194E
author: lobrien
ms.author: laobri
ms.date: 09/25/2017
ms.openlocfilehash: c82861a0b4ca00f7c664cd6a920f250ad937d306
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68656462"
---
# <a name="core-nfc-in-xamarinios"></a>Xamarin のコア NFC

_IOS 11 を使用した近距離無線通信 (NFC) タグの読み取り_

CoreNFC は iOS 11 の新しいフレームワークで、アプリ内からタグを読み取る_近距離無線通信_(NFC) ラジオへのアクセスを提供します。 IPhone 7、7 Plus、8、8 Plus、X で動作します。

IOS デバイスの NFC タグリーダーは、 _Nfc データ交換形式_(NDEF) 情報を含むすべての nfc タグの種類 1 ~ 5 をサポートしています。

注意すべきいくつかの制限があります。

- CoreNFC はタグ読み取りのみをサポートします (書き込みや書式設定はできません)。
- タグスキャンは、ユーザーが開始する必要があり、60秒後にタイムアウトになります。
- アプリはスキャンのために前景に表示される必要があります。
- CoreNFC は、(シミュレーターではなく) 実際のデバイスでのみテストできます。

このページでは、CoreNFC の使用に必要な構成について説明し、 ["Nfctagreader" サンプルコード](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-nfctagreader)を使用して API を使用する方法を示します。

## <a name="configuration"></a>構成

CoreNFC を有効にするには、プロジェクトで次の3つの項目を構成する必要があります。

- **情報 plist**プライバシーキー。
- **権利の plist**エントリ。
- **NFC タグ読み取り**機能を備えたプロビジョニングプロファイル。

### <a name="infoplist"></a>Info.plist

**NFCReaderUsageDescription**のプライバシーキーとテキストを追加します。これは、スキャンの実行中にユーザーに表示されます。 アプリケーションに適切なメッセージを使用します (たとえば、スキャンの目的を説明します)。

```xml
<key>NFCReaderUsageDescription</key>
<string>NFC tag to read NDEF messages into the application</string>
```

### <a name="entitlementsplist"></a>Entitlements.plist

アプリでは、次のキー/値ペアを使用して、**近距離無線通信タグ読み取り**機能を要求する必要があり**ます。 plist**:

```xml
<key>com.apple.developer.nfc.readersession.formats</key>
<array>
  <string>NDEF</string>
</array>
```

### <a name="provisioning-profile"></a>プロファイルのプロビジョニング

新しい**アプリ ID**を作成し、 **NFC タグ読み取り**サービスが実行されていることを確認します。

[![開発者ポータルでの NFC タグの読み取りが選択された新しいアプリ ID ページ](corenfc-images/app-services-nfc-sml.png)](corenfc-images/app-services-nfc.png#lightbox)

その後、このアプリ ID 用の新しいプロビジョニングプロファイルを作成し、それをダウンロードして開発用 Mac にインストールする必要があります。

## <a name="reading-a-tag"></a>タグの読み取り

プロジェクトが構成されたら、 `using CoreNFC;`ファイルの先頭にを追加し、次の3つの手順に従って NFC タグ読み取り機能を実装します。

### <a name="1-implement-infcndefreadersessiondelegate"></a>1.導入`INFCNdefReaderSessionDelegate`

インターフェイスには、次の2つのメソッドを実装できます。

- `DidDetect`–タグが正常に読み取られたときに呼び出されます。
- `DidInvalidate`–エラーが発生したとき、または60秒のタイムアウトに達したときに呼び出されます。

#### <a name="diddetect"></a>DidDetect

このサンプルコードでは、スキャンされた各メッセージをテーブルビューに追加します。

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

セッションで複数のタグ読み取りが許可されている場合、このメソッドは複数回 (およびメッセージの配列が渡される可能性があります) に呼び出されることがあります。 これは、 `Start` ([手順 2](#step2)で説明した) メソッドの3番目のパラメーターを使用して設定します。

#### <a name="didinvalidate"></a>DidInvalidate

無効化は、次のようなさまざまな理由で発生する可能性があります。

- スキャン中にエラーが発生しました。
- アプリはフォアグラウンドになったます。
- ユーザーがスキャンをキャンセルすることを選択しました。
- スキャンはアプリによって取り消されました。

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

セッションが無効になったら、再度スキャンするために新しいセッションオブジェクトを作成する必要があります。

<a name="step2" />

### <a name="2-start-an-nfcndefreadersession"></a>2.開始`NFCNdefReaderSession`

スキャンは、ボタンを押すなどのユーザー要求で開始する必要があります。
次のコードでは、スキャンセッションを作成して開始します。

```csharp
Session = new NFCNdefReaderSession(this, null, true);
Session?.BeginSession();
```

`NFCNdefReaderSession`コンストラクターのパラメーターは次のとおりです。

- `delegate`–の`INFCNdefReaderSessionDelegate`実装。 このサンプルコードでは、デリゲートはテーブルビューコントローラーに実装されて`this`いるため、デリゲートパラメーターとして使用されます。
- `queue`–コールバックが処理されるキュー。 この場合`null`、(サンプルに示されているように`DispatchQueue.MainQueue` ) ユーザーインターフェイスコントロールを更新するときに必ずを使用する必要があります。
- `invalidateAfterFirstRead`–の`true`場合、スキャンは最初に成功した後に`false`停止します。スキャンが続行され、複数の結果が返されるまで、スキャンが取り消されるか、60秒のタイムアウトに達します。


### <a name="3-cancel-the-scanning-session"></a>3.スキャンセッションをキャンセルする

ユーザーは、ユーザーインターフェイスのシステム指定のボタンを使用して、スキャンセッションを取り消すことができます。

![スキャン中の [キャンセル] ボタン](corenfc-images/scan-cancel-sml.png)

アプリでは、メソッドを`InvalidateSession`呼び出すことによって、プログラムでスキャンを取り消すことができます。

```csharp
Session.InvalidateSession();
```

どちらの場合も、デリゲートの`DidInvalidate`メソッドが呼び出されます。

## <a name="summary"></a>まとめ

CoreNFC を使用すると、アプリは NFC タグからデータを読み取ることができます。 さまざまなタグ形式 (NDEF types 1 ~ 5) の読み取りをサポートしますが、書き込みや書式設定はサポートしていません。


## <a name="related-links"></a>関連リンク

- [NFCTagReader (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-nfctagreader)
- [コア NFC (WWDC) の概要 (ビデオ)](https://developer.apple.com/videos/play/wwdc2017/718/)
