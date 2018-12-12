---
title: 'Xamarin.Essentials: ダイヤラー'
description: Xamarin.Essentials の PhoneDialer クラスを使用すると、アプリケーションからダイヤラーで電話番号を開くことができます。
ms.assetid: E7457942-4D7B-4195-A2FF-417919B9537F
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
ms.openlocfilehash: 8d4b0cdcae5e33ac2c48baa0b7749597314eae8c
ms.sourcegitcommit: 01f93a34b466f8d4043cef68fab9b35cd8decee6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/05/2018
ms.locfileid: "52898272"
---
# <a name="xamarinessentials-phone-dialer"></a>Xamarin.Essentials: ダイヤラー

**PhoneDialer** クラスを使用すると、アプリケーションからダイヤラーで電話番号を開くことができます。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-phone-dialer"></a>PhoneDialer の使用

自分のクラスの Xamarin.Essentials に参照を追加します。

```csharp
using Xamarin.Essentials;
```

PhoneDialer 機能を使用するには、ダイヤラーで開く電話番号を指定して `Open` メソッドを呼び出します。 `Open` が要求されると、API は国番号が指定されている場合はそれに基づいて自動的に番号の書式設定を試みます。

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

- [PhoneDialer のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/PhoneDialer)
- [PhoneDialer API のドキュメント](xref:Xamarin.Essentials.PhoneDialer)
