---
title: Xamarin.Essentials:ダイヤラー
description: Xamarin.Essentials の PhoneDialer クラスを使用すると、アプリケーションからダイヤラーで電話番号を開くことができます
ms.assetid: E7457942-4D7B-4195-A2FF-417919B9537F
author: jamesmontemagno
ms.custom: video
ms.author: jamont
ms.date: 07/02/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 9bd281a61fd53ef3f6d0d3d2307f78a218f33cf4
ms.sourcegitcommit: db423d51356cf5a2dfa1b3925204797b1baf3cd9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92734773"
---
# <a name="no-locxamarinessentials-phone-dialer"></a>Xamarin.Essentials:ダイヤラー

**PhoneDialer** クラスを使用すると、アプリケーションからダイヤラーで電話番号を開くことができます。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

# <a name="android"></a>[Android](#tab/android)

プロジェクトのターゲット Android バージョンが **Android 11 (R API 30)** に設定される場合、新しい [パッケージの可視性要件](https://developer.android.com/preview/privacy/package-visibility)で使用されるクエリで Android マニフェストを更新する必要があります。

**[プロパティ]** フォルダーにある **AndroidManifest.xml** ファイルを開き、 **manifest** ノードの内部に以下を追加します。

```xml
<queries>
  <intent>
    <action android:name="android.intent.action.DIAL" />
    <data android:scheme="tel"/>
  </intent>
</queries>
```

# <a name="ios"></a>[iOS](#tab/ios)

追加の設定は必要ありません。

# <a name="uwp"></a>[UWP](#tab/uwp)

プラットフォームによる違いはありません。

-----

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

- [PhoneDialer のソース コード](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/PhoneDialer)
- [PhoneDialer API のドキュメント](xref:Xamarin.Essentials.PhoneDialer)

## <a name="related-video"></a>関連ビデオ

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Phone-Dialer-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
