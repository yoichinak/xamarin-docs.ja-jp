---
title: Xamarin.Essentials 設定
description: 設定クラスでは、キー/値のストアにアプリケーションの設定を保存します。
ms.assetid: AA81BCBD-79BA-448F-942B-BA4415CA50FF
ms.technology: xamarin-crossplatform
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 3886432f51125508349fd7815ac374cb0339c254
ms.sourcegitcommit: 46d3c9daa45350bdd536d9e105517f3c1c753c5b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/07/2018
---
# <a name="xamarinessentials-preferences"></a>Xamarin.Essentials 設定

![プレリリース NuGet](~/media/shared/pre-release.png)

**設定**クラスは、キー/値のストアにアプリケーションの設定を格納するのに役立ちます。

## <a name="using-secure-storage"></a>セキュリティで保護されたストレージを使用します。

クラスの Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

値を保存する、指定された_キー_設定 で。

```csharp
Preferences.Set("my_key", "my_value");
```

設定されていない場合、設定や、既定値から値を取得します。

```csharp
var myValue = Preferences.Get("my_key", "default_value");
```

削除する、_キー_設定から。

```csharp
Preferences.Remove("my_key");
```

すべての環境設定を削除します。

```csharp
Preferences.Clear();
```

これらのメソッドだけでなくそれぞれを受け取る省略可能なで`sharedName` 基本設定の追加のコンテナーの作成に使用できます。 以下のプラットフォームの実装詳細を参照してください。

## <a name="supported-data-types"></a>サポートされるデータ型

次のデータ型はサポート**設定**:

- **bool**
- **double**
- **int**
- **float**
- **long**
- **string**

## <a name="platform-implementation-specifics"></a>プラットフォームの実装の詳細

# <a name="androidtabandroid"></a>[Android](#tab/android)

すべてのデータが格納されている[共有設定](https://developer.android.com/training/data-storage/shared-preferences.html)です。 ない場合は`sharedName`が指定されているは、共有の既定の設定を使用する、それ以外の場合、名前の使用を取得、**プライベート**指定した名前と環境設定を共有します。

# <a name="iostabios"></a>[iOS](#tab/ios)

[NSUserDefaults](https://docs.microsoft.com/en-us/xamarin/ios/app-fundamentals/user-defaults) iOS デバイスで値を格納するために使用します。 いない場合`sharedName`が指定されている、`StandardUserDefaults`は、それ以外の場合、名前が使用を新規作成する`NSUserDefaults`ためのシステム指定の名前を持つ、`NSUserDefaultsType.SuiteName`です。

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

[ApplicationDataContainer](https://docs.microsoft.com/en-us/uwp/api/windows.storage.applicationdatacontainer)デバイス上の値を格納するために使用します。 ない場合は`sharedName`が指定されている、`LocalSettings`は、それ以外の場合、名前が使用中の新しいコンテナーを作成する`LocalSettings`です。

--------------

## <a name="limitations"></a>制限事項

文字列を格納するときにこの API は、少量のテキストを格納します。  パフォーマンスは、大量のテキストを保存するため使用しようとする場合は、subpar 可能性があります。

## <a name="api"></a>API

- [環境設定のソース コード](https://github.com/xamarin/Essentials/tree/master/Essentials/Preferences)
- [設定 API ドキュメント](xref:Xamarin.Essentials.Preferences)
