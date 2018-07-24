---
title: 'Xamarin.Essentials: 基本設定'
description: このドキュメントでは、Xamarin.Essentials、キー/値ストアでアプリケーションの設定を保存します。 これで設定クラスについて説明します。 これには、クラスと、格納できるデータの種類を使用する方法について説明します。
ms.assetid: AA81BCBD-79BA-448F-942B-BA4415CA50FF
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: ca6d4f1ec60a80b483c79dd75267144e67d80c0b
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2018
ms.locfileid: "38831765"
---
# <a name="xamarinessentials-preferences"></a>Xamarin.Essentials: 基本設定

![NuGet にプレリリースします。](~/media/shared/pre-release.png)

**設定**キー/値ストアでアプリケーションの設定を格納するクラスを使用します。

## <a name="using-preferences"></a>基本設定の使用

クラスで Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

値を保存する、指定された_キー_の基本設定。

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

すべての環境設定を削除するには。

```csharp
Preferences.Clear();
```

これらのメソッドだけでなく、省略可能な各実行`sharedName` 基本設定の追加のコンテナーの作成に使用できます。 以下のプラットフォームの実装の詳細を参照してください。

## <a name="supported-data-types"></a>サポートされるデータ型

次のデータ型はサポート**設定**:

- **bool**
- **double**
- **int**
- **float**
- **long**
- **string**
- **DateTime**

## <a name="implementation-details"></a>実装の詳細

値`DateTime`によって定義された 2 つのメソッドを使用して 64 ビットのバイナリ (長整数) 形式で格納されます、`DateTime`クラス: [ `ToBinary` ](xref:System.DateTime.ToBinary)メソッドのエンコードが使用される、`DateTime`値、および、 [ `FromBinary` ](xref:System.DateTime.FromBinary(System.Int64))メソッドが値をデコードします。 値がデコードに行われる可能性の調整でこれらのメソッドのドキュメントを参照してください、`DateTime`は世界協定時刻 (UTC) の値ではなくで格納されています。

## <a name="platform-implementation-specifics"></a>プラットフォームの実装の詳細

# <a name="androidtabandroid"></a>[Android](#tab/android)

すべてのデータが格納されている[共有設定](https://developer.android.com/training/data-storage/shared-preferences.html)します。 ない場合は`sharedName`が、共有の既定の設定が使用される、取得する名前を使用するそれ以外の場合に指定されて、**プライベート**指定した名前の基本設定を共有します。

# <a name="iostabios"></a>[iOS](#tab/ios)

[NSUserDefaults](https://docs.microsoft.com/en-us/xamarin/ios/app-fundamentals/user-defaults) iOS デバイスの値を格納するために使用します。 ない場合は`sharedName`が指定されて、`StandardUserDefaults`は、それ以外の場合、名前が使用を新しく作成する`NSUserDefaults`に使用される指定した名前で、`NSUserDefaultsType.SuiteName`します。

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

[ApplicationDataContainer](https://docs.microsoft.com/en-us/uwp/api/windows.storage.applicationdatacontainer)デバイス上の値を格納するために使用します。 ない場合は`sharedName`が指定されて、`LocalSettings`は、それ以外の場合、名前が使用中の新しいコンテナーを作成する`LocalSettings`します。

--------------

## <a name="limitations"></a>制限事項

文字列を格納するときに、この API は、少量のテキストを格納するものです。  パフォーマンスは、大量のテキストの格納に使用しようとする場合は程遠いものでした可能性があります。

## <a name="api"></a>API

- [基本設定のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Preferences)
- [API の基本設定のドキュメント](xref:Xamarin.Essentials.Preferences)
