---
title: Xamarin.Essentials ダイヤラ
description: PhoneDialer クラスには、最適化されたシステム優先ブラウザーまたは外部のブラウザーで web リンクを開くためのアプリケーションができるようにします。
ms.assetid: E7457942-4D7B-4195-A2FF-417919B9537F
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 6bea1b26e43459be12272f9dd2ecc6fafba2b8b4
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/09/2018
---
# <a name="xamarinessentials-phone-dialer"></a>Xamarin.Essentials ダイヤラ

![プレリリース NuGet](~/media/shared/pre-release.png)

**PhoneDialer**クラスには、最適化されたシステム優先ブラウザーまたは外部のブラウザーで web リンクを開くためのアプリケーションができるようにします。

## <a name="using-phone-dialer"></a>ダイヤラを使用します。

クラスの Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

ダイヤラ機能が呼び出すことによって、`Open`を開くにはのダイヤラーの電話番号を持つメソッドです。 ときに`Open`API は、数値の国コードに基づいて指定された場合は自動的に試行を要求します。

```csharp
public class PhoneDialerTest
{
    public async Task PlacePhoneCall(string number)
    {
        try
        {
            PhoneDialer.Open(number);
        }
        catch (ArgumentNullException anEx)
        {
            // Number was null or white space
        }
        catch (FeatureNotSupportedException ex)
        {
            // Phone Dialer is not supported on this device.
        }
        catch (Exception ex)
        {
            // Other error has occurred.
        }
    }
}
```

## <a name="api"></a>API

- [ダイヤラーをソース コード](https://github.com/xamarin/Essentials/tree/master/Essentials/PhoneDialer)
- [Phone ダイヤラー API ドキュメント](xref:Xamarin.Essentials.PhoneDialer)
