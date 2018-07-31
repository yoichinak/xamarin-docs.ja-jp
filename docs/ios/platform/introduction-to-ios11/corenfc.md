---
title: Xamarin.iOS では、core NFC
description: このドキュメントでは、iOS 11 で導入された Api を使用して Xamarin.iOS でフィールド通信タグの近くを読み取る方法について説明します。
ms.prod: xamarin
ms.technology: xamarin-ios
ms.assetid: 846B59D3-F66A-48F3-A78C-84217697194E
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/25/2017
ms.openlocfilehash: 1381a4564f93fd091f181949454df3f06b31ae6b
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350834"
---
# <a name="core-nfc-in-xamarinios"></a>Xamarin.iOS では、core NFC

_IOS 11 を使用して読み取り近距離通信 (NFC) タグ_

CoreNFC は ios 11 へのアクセスを提供する新しいフレームワーク、_近距離通信_(NFC) オプションをアプリ内からのタグを読み取る。 IPhone 7、8、さらに、および X 8、7 Plus です。

IOS デバイスの NFC タグ リーダーがすべて NFC タグの種類 1 ~ 5 が含まれているをサポートしている_NFC データ交換形式_(NDEF) 情報。

これには注意すべきいくつかの制限があります。

- CoreNFC は、タグ (作成や書式設定) の読み取りのみをサポートします。
- タグのスキャンは、ユーザーが開始する必要があり、60 秒後にタイムアウトします。
- アプリは、スキャンの前景色に表示される必要があります。
- CoreNFC は、(シミュレーター) ではなく実際のデバイスでのみテストできます。

このページは CoreNFC を使用するために必要な構成について説明し、API を使用して、使用する方法を示しています、 ["TFCTagReader"サンプル コード](https://developer.xamarin.com/samples/monotouch/ios11/NFCTagReader/)します。

## <a name="configuration"></a>構成

CoreNFC を有効にするには、プロジェクトの 3 つの項目を構成する必要があります。

- **Info.plist**プライバシー キー。
- **Entitlements.plist**エントリ。
- 使用するプロビジョニング プロファイル**NFC タグの読み取り**機能します。

### <a name="infoplist"></a>Info.plist

追加、 **NFCReaderUsageDescription**プライバシー キーと、スキャンの実行中にユーザーに表示されるテキスト。 アプリケーションに適切なメッセージを使用して (たとえば、スキャンの目的を説明します)。

```xml
<key>NFCReaderUsageDescription</key>
<string>NFC tag to read NDEF messages into the application</string>
```

### <a name="entitlementsplist"></a>Entitlements.plist

アプリが要求する必要があります、**フィールド通信タグ読み取りの近く**で次のキー/値を使用して機能のペア、 **Entitlements.plist**:

```xml
<key>com.apple.developer.nfc.readersession.formats</key>
<array>
  <string>NDEF</string>
</array>
```

### <a name="provisioning-profile"></a>プロファイルのプロビジョニング

新規作成**アプリ ID**いることを確認し、 **NFC タグの読み取り**サービスがオンになっています。

[![NFC タグの読み取りが選択されていると開発者ポータルの新しいアプリ ID ページ](corenfc-images/app-services-nfc-sml.png)](corenfc-images/app-services-nfc.png#lightbox)

必要がありますし、この App ID の新しいプロビジョニング プロファイルを作成し、ダウンロードして、開発ファルダにインストールします。

## <a name="reading-a-tag"></a>タグの読み取り

プロジェクトを構成すると後で追加`using CoreNFC;`NFC を実装するためにこれら 3 つの手順に従って、ファイルの先頭に読み取り機能にタグ付けします。

### <a name="1-implement-infcndefreadersessiondelegate"></a>1.実装 `INFCNdefReaderSessionDelegate`

インターフェイスでは、2 つのメソッドを実装するには。

- `DidDetect` – タグが正常に読み取られたときに呼び出されます。
- `DidInvalidate` – エラーが発生したり、60 秒のタイムアウトに達したときに呼び出されます。

#### <a name="diddetect"></a>DidDetect

サンプル コードでは、スキャンされた各メッセージがテーブルのビューに追加されます。

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

このメソッドは、複数回呼び出すことができます (とのメッセージの配列を渡すことがあります) 場合は、セッションを複数のタグ読み取りデータを使用します。 3 番目のパラメーターを使用してこの設定は、`Start`メソッド (で説明されている[手順 2.](#step2))。

#### <a name="didinvalidate"></a>DidInvalidate

無効化は、さまざまな理由によって発生します。

- スキャン中にエラーが発生しました。
- アプリではなく、フォア グラウンドであります。
- ユーザーは、スキャンをキャンセルすることを選択します。
- スキャンは、アプリによって取り消されました。

次のコードでは、エラーを処理する方法を示します。

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

セッションが無効にするを再度スキャンする、新しいセッション オブジェクトを作成する必要があります。

<a name="step2" />

### <a name="2-start-an-nfcndefreadersession"></a>2.開始します。 `NFCNdefReaderSession`

スキャンは、ボタンを押すなどのユーザーの要求で始まらなければなりません。
次のコードは作成し、スキャンのセッションを開始します。

```csharp
Session = new NFCNdefReaderSession(this, null, true);
Session?.BeginSession();
```

パラメーター、`NFCNdefReaderSession`コンス トラクターは、次のとおり。

- `delegate` – の実装`INFCNdefReaderSessionDelegate`します。 サンプル コードで、デリゲートではテーブル ビュー コント ローラーで、したがって`this`デリゲート パラメーターとして使用されます。
- `queue` – でコールバックが処理されるキュー。 できます`null`、後者を使用して、必ず、 `DispatchQueue.MainQueue` (ように、サンプルに示された) ユーザー インターフェイス コントロールを更新するときにします。
- `invalidateAfterFirstRead` – とき`true`、最初の成功したスキャン後のスキャンの停止時に`false`スキャンは続行し、スキャンがキャンセルされたか、60 秒のタイムアウトに達するまで、複数の結果が返されます。


### <a name="3-cancel-the-scanning-session"></a>3.スキャンのセッションをキャンセルします。

ユーザーは、ユーザー インターフェイスでのシステム提供のボタンを使用してスキャンのセッションをキャンセルできます。

![スキャン中に [キャンセル] ボタン](corenfc-images/scan-cancel-sml.png)

アプリがスキャンをプログラムでキャンセル、`InvalidateSession`メソッド。

```csharp
Session.InvalidateSession();
```

どちらの場合も、デリゲートの`DidInvalidate`メソッドが呼び出されます。

## <a name="summary"></a>まとめ

CoreNFC では、NFC タグからデータを読み取る、アプリを使用できます。 さまざまなタグの形式 (NDEF 型 1 ~ 5) の読み取りをサポートしていますが、書き込み、または書式設定をサポートしません。


## <a name="related-links"></a>関連リンク

- [NFCTagReader (サンプル)](https://developer.xamarin.com/samples/monotouch/ios11/NFCTagReader/)
- [Core NFC (WWDC) (ビデオ) の概要](https://developer.apple.com/videos/play/wwdc2017/718/)
