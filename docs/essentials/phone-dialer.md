---
タイトル: "Xamarin.Essentials:ダイヤラー'' description:"Xamarin.Essentials の PhoneDialer クラスを使用すると、アプリケーションからダイヤラーで電話番号を開くことができます" ms.assetid:E7457942-4D7B-4195-A2FF-417919B9537F author: jamesmontemagno ms.custom: video ms.author: jamont ms.date:07/02/2019 no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# <a name="xamarinessentials-phone-dialer"></a>Xamarin.Essentials:ダイヤラー

**PhoneDialer** クラスを使用すると、アプリケーションからダイヤラーで電話番号を開くことができます。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-phone-dialer"></a>PhoneDialer の使用

クラスの Xamarin.Essentials への参照を追加します。

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

## <a name="related-video"></a>関連ビデオ

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Phone-Dialer-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
