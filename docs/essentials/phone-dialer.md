---
title: Xamarin.Essentials:ダイヤラー
description: Xamarin.Essentials の PhoneDialer クラスを使用すると、アプリケーションからダイヤラーで電話番号を開くことができます。
ms.assetid: E7457942-4D7B-4195-A2FF-417919B9537F
author: jamesmontemagno
ms.author: jamont
ms.date: 07/02/2019
ms.openlocfilehash: 0b00307759d95b8e2efe27bd9a17d2c6efd3d5c0
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "67538683"
---
# <a name="xamarinessentials-phone-dialer"></a>Xamarin.Essentials:ダイヤラー

**PhoneDialer** クラスを使用すると、アプリケーションからダイヤラーで電話番号を開くことができます。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-phone-dialer"></a>PhoneDialer の使用

自分のクラスに Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

PhoneDialer 機能を使用するには、ダイヤラーで開く電話番号を指定して `Open` メソッドを呼び出します。 `Open` が要求されると、API は国番号が指定されている場合はそれに基づいて自動的に番号の書式設定を試みます。

```csharp
public class PhoneDialerTest
{
    public void PlacePhoneCall(string number)
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

- [PhoneDialer のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/PhoneDialer)
- [PhoneDialer API のドキュメント](xref:Xamarin.Essentials.PhoneDialer)
