---
title: 'Xamarin.Essentials: ダイヤラー'
description: Xamarin.Essentials で PhoneDialer クラスは、ダイヤラーに電話番号を開くためのアプリケーションを使用できます。
ms.assetid: E7457942-4D7B-4195-A2FF-417919B9537F
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 34a6c80836d8cb42b1f8fd95718fe248d4701c0f
ms.sourcegitcommit: 7f2e44e6f628753e06a5fe2a3076fc2ec5baa081
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/18/2018
ms.locfileid: "39130794"
---
# <a name="xamarinessentials-phone-dialer"></a>Xamarin.Essentials: ダイヤラー

![NuGet にプレリリースします。](~/media/shared/pre-release.png)

**PhoneDialer**クラス ダイヤラーに電話番号を開くためのアプリケーションを使用できます。

## <a name="using-phone-dialer"></a>ダイヤラを使用します。

クラスで Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

ダイヤラー機能が呼び出すことによって、`Open`を開くとダイヤラーの電話番号を持つメソッド。 ときに`Open`API は、数値が指定されている場合は、国コードに基づいて自動的に試行を要求します。

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

- [ソース コードをダイヤラーします。](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/PhoneDialer)
- [電話のダイヤラー API ドキュメント](xref:Xamarin.Essentials.PhoneDialer)
