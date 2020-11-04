---
title: Xamarin.Essentials:Preferences
description: このドキュメントでは、アプリケーションのユーザー設定をキーと値のストアに保存する、Xamarin.Essentials の Preferences クラスについて説明します。 クラスの使用方法と、格納できるデータの種類について説明します。
ms.assetid: AA81BCBD-79BA-448F-942B-BA4415CA50FF
author: jamesmontemagno
ms.author: jamont
ms.date: 01/15/2019
ms.custom: video
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 07bfcabc7ffef20bee43531bfab3e78155beb9a9
ms.sourcegitcommit: 58247fe066ad271ee43c8967ac3301fdab6ca2d1
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92629575"
---
# <a name="no-locxamarinessentials-preferences"></a>Xamarin.Essentials:Preferences

**Preferences** クラスを使用すると、アプリケーションのユーザー設定をキー/値ストアに保存できます。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-preferences"></a>Preferences の使用

クラスの Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

特定の " _キー_ " の値をユーザー設定に保存するには:

```csharp
Preferences.Set("my_key", "my_value");
```

ユーザー設定の値、または設定されていない場合は既定値を取得するには:

```csharp
var myValue = Preferences.Get("my_key", "default_value");
```

特定の _キー_ がユーザー設定に存在するかどうかを確認するには:

```csharp
bool hasKey = Preferences.ContainsKey("my_key");
```

ユーザー設定から " _キー_ " を削除するには:

```csharp
Preferences.Remove("my_key");
```

すべてのユーザー設定を削除するには:

```csharp
Preferences.Clear();
```

> [!TIP]
> 上のメソッドは、`sharedName` という名称のオプション `string` パラメーターを受け取ります。 このパラメーターは、一部のユース ケースで役に立つ基本設定用の追加コンテナーを作成するために使用されます。 ユース ケースの 1 つは、拡張機能全体で、あらゆる時計アプリケーションと基本設定を共有する必要がアプリケーションにあるときです。 下にあるプラットフォーム実装の詳細をご覧ください。

## <a name="supported-data-types"></a>サポートされているデータ型

**Preferences** では次のデータ型がサポートされています。

- **bool**
- **double**
- **int**
- **float**
- **long**
- **string**
- **DateTime**

## <a name="integrate-with-system-settings"></a>システム設定と統合する

ユーザー設定はネイティブで格納されるため、設定をネイティブ システム設定に統合できます。 プラットフォーム ドキュメントとサンプルに従って、プラットフォームと統合します。

* Apple: [iOS 設定バンドルの実装](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/UserDefaults/Preferences/Preferences.html)
* [iOS アプリケーションのユーザー設定のサンプル](/samples/xamarin/ios-samples/appprefs/)
* [watchOS の設定](https://developer.xamarin.com/guides/ios/watch/working-with/settings/)
* Android:[設定画面の概要](https://developer.android.com/guide/topics/ui/settings.html)

## <a name="implementation-details"></a>実装の詳細

`DateTime` の値は `DateTime` クラスによって定義された 2 つのメソッドを使用して 64 ビット バイナリ (long 型整数) 形式で格納されます。[`ToBinary`](xref:System.DateTime.ToBinary) メソッドを使用して `DateTime` の値がエンコードされ、[`FromBinary`](xref:System.DateTime.FromBinary(System.Int64)) メソッドが値をデコードします。 世界協定時刻 (UTC) 値ではない `DateTime` が格納されているときに値をデコードするために行われる可能性がある調整については、これらのメソッドのドキュメントをご覧ください。

## <a name="platform-implementation-specifics"></a>プラットフォームの実装の詳細

# <a name="android"></a>[Android](#tab/android)

すべてのデータは [Shared Preferences](https://developer.android.com/training/data-storage/shared-preferences.html)に格納されます。 `sharedName` が指定されていない場合は、既定の共有ユーザー設定が使用されます。指定されている場合は、その名前を使用して **プライベート** 共有ユーザー設定が取得されます。

# <a name="ios"></a>[iOS](#tab/ios)

iOS デバイスに値を格納するには、[NSUserDefaults](../ios/app-fundamentals/user-defaults.md) が使用されます。 `sharedName` が指定されていない場合は、`StandardUserDefaults` が使用されます。指定されている場合は、`NSUserDefaultsType.SuiteName` に対して使用される指定された名前で新しい `NSUserDefaults` が作成されます。

# <a name="uwp"></a>[UWP](#tab/uwp)

デバイスに値を格納するには [ApplicationDataContainer](/uwp/api/windows.storage.applicationdatacontainer) が使用されます。 `sharedName` が指定されていない場合は、`LocalSettings` が使用されます。指定されている場合は、その名前を使用して `LocalSettings` 内に新しいコンテナーが作成されます。

さらに、`LocalSettings` には、各設定の名前は最大で 255 文字という制限があります。 各設定のサイズは最大 8K バイトで、各コンポジット設定のサイズは最大 64K バイトです。

--------------

## <a name="persistence"></a>永続性

アプリケーションをアンインストールすると、すべての " _ユーザー設定_ " が削除されます。ただし、 [__自動バックアップ__](https://developer.android.com/guide/topics/data/autobackup)が使用されている Android 6.0 (API レベル 23) 以降を対象にして、そこで実行されるアプリは例外です。 この機能は既定で有効にされて、 __Shared Preferences__ などのアプリ データを保持し、 **Preferences** API はそれを利用します。 この機能は、Google の [ドキュメント](https://developer.android.com/guide/topics/data/autobackup)に従って無効にできます。

## <a name="limitations"></a>制限事項

文字列を格納するとき、この API では少量のテキストを格納することが想定されています。 大量のテキストを格納するためにこれを使用しようとすると、パフォーマンスが低下する可能性があります。

## <a name="api"></a>API

- [Preferences のソース コード](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/Preferences)
- [Preferences API のドキュメント](xref:Xamarin.Essentials.Preferences)

## <a name="related-video"></a>関連ビデオ

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Preferences-Essential-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
