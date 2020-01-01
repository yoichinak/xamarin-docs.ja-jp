---
title: Xamarin.Essentials:Preferences
description: このドキュメントで説明する Xamarin.Essentials の Preferences クラスでは、アプリケーションのユーザー設定がキー/値ストアに保存されます。 クラスの使用方法と、格納できるデータの種類について説明します。
ms.assetid: AA81BCBD-79BA-448F-942B-BA4415CA50FF
author: jamesmontemagno
ms.author: jamont
ms.date: 01/15/2019
ms.custom: video
ms.openlocfilehash: 6d57b5ce9cb61363eef24b230f6cf71894f66198
ms.sourcegitcommit: 53f5e83f4e246be703917d7cc719c8cc959517ab
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/15/2019
ms.locfileid: "54317883"
---
# <a name="xamarinessentials-preferences"></a>Xamarin.Essentials:Preferences

**Preferences** クラスを使用すると、アプリケーションのユーザー設定をキー/値ストアに保存できます。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-preferences"></a>Preferences の使用

自分のクラスに Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

特定の "_キー_" の値をユーザー設定に保存するには:

```csharp
Preferences.Set("my_key", "my_value");
```

ユーザー設定の値、または設定されていない場合は既定値を取得するには:

```csharp
var myValue = Preferences.Get("my_key", "default_value");
```

ユーザー設定から "_キー_" を削除するには:

```csharp
Preferences.Remove("my_key");
```

すべてのユーザー設定を削除するには:

```csharp
Preferences.Clear();
```

これらのメソッドだけでなく、省略可能な `sharedName` を使用してユーザー設定用の追加コンテナーを作成できます。 以下のプラットフォームの実装の詳細をご覧ください。

## <a name="supported-data-types"></a>サポートされているデータ型

**Preferences** では次のデータ型がサポートされています。

- **bool**
- **double**
- **int**
- **float**
- **long**
- **string**
- **DateTime**

## <a name="implementation-details"></a>実装の詳細

`DateTime` の値は `DateTime` クラスによって定義された 2 つのメソッドを使用して 64 ビット バイナリ (long 型整数) 形式で格納されます。[`ToBinary`](xref:System.DateTime.ToBinary) メソッドを使用して `DateTime` の値がエンコードされ、[`FromBinary`](xref:System.DateTime.FromBinary(System.Int64)) メソッドが値をデコードします。 世界協定時刻 (UTC) 値ではない `DateTime` が格納されているときに値をデコードするために行われる可能性がある調整については、これらのメソッドのドキュメントをご覧ください。

## <a name="platform-implementation-specifics"></a>プラットフォームの実装の詳細

# <a name="androidtabandroid"></a>[Android](#tab/android)

すべてのデータは[Shared Preferences](https://developer.android.com/training/data-storage/shared-preferences.html)に格納されます。 `sharedName` が指定されていない場合は既定の共有ユーザー設定が使用され、指定されている場合はその名前を使用して**プライベート**共有ユーザー設定が取得されます。

# <a name="iostabios"></a>[iOS](#tab/ios)

iOS デバイスに値を格納するには、[NSUserDefaults](https://docs.microsoft.com/xamarin/ios/app-fundamentals/user-defaults) が使用されます。 `sharedName` が指定されていない場合は、`StandardUserDefaults` が使用されます。指定されている場合は、`NSUserDefaultsType.SuiteName` に対して使用される指定された名前で新しい `NSUserDefaults` が作成されます。

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

デバイスに値を格納するには [ApplicationDataContainer](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer) が使用されます。 `sharedName` が指定されていない場合は、`LocalSettings` が使用されます。指定されている場合は、その名前を使用して `LocalSettings` 内に新しいコンテナーが作成されます。

--------------

## <a name="persistence"></a>永続性

アプリケーションをアンインストールすると、すべての "_ユーザー設定_" が削除されます。 これには例外が 1 つあり、Android 6.0 (API レベル 23) 以降で実行されていて[__自動バックアップ__](https://developer.android.com/guide/topics/data/autobackup)を使用しているアプリの場合です。 この機能は既定で有効にされて、**Shared Preferences**などのアプリ データを保持し、**Preferences** API はそれを利用します。 この機能は、Google の[ドキュメント](https://developer.android.com/guide/topics/data/autobackup)に従って無効にできます。

## <a name="limitations"></a>制限事項

文字列を格納するとき、この API では少量のテキストを格納することが想定されています。  大量のテキストを格納するためにこれを使用しようとすると、パフォーマンスが低下する可能性があります。

## <a name="api"></a>API

- [Preferences のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Preferences)
- [Preferences API のドキュメント](xref:Xamarin.Essentials.Preferences)

## <a name="related-video"></a>関連ビデオ

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Preferences-Essential-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
