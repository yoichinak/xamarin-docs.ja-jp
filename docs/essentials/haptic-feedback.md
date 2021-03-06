---
title: 'Xamarin.Essentials: Haptic フィードバック'
description: このドキュメントでは、デバイスに対する haptic フィードバックを制御できる Xamarin.Essentials の HapticFeedback クラスについて説明します。
ms.assetid: 4462936c-4018-443b-906d-d63da6d0ed7d
author: dimonovdd
ms.author: jamont
ms.date: 01/04/2021
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 0b039a8ba7db7b98d30a49b74454b8c0c101f040
ms.sourcegitcommit: 995ee23d93e08dceb8754cc6c682cd2f4594345b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2021
ms.locfileid: "97972306"
---
# <a name="no-locxamarinessentials-haptic-feedback"></a>Xamarin.Essentials: Haptic フィードバック

**HapticFeedback** クラスを使用すると、デバイスに対して haptic フィードバックを制御できます。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

**HapticFeedback** の機能にアクセスするには、次のプラットフォーム固有の設定が必要です。

# <a name="android"></a>[Android](#tab/android)

Vibrate アクセス許可が必要です。Android プロジェクト内で構成する必要があります。 これは次の方法で追加できます。

**[プロパティ]** フォルダーにある **AssemblyInfo.cs** ファイルを開き、以下を追加します。

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.Vibrate)]
```

または、Android マニフェストを追加します。

**[プロパティ]** フォルダーにある **AndroidManifest.xml** ファイルを開き、**manifest** ノードの内部に以下を追加します。

```xml
<uses-permission android:name="android.permission.VIBRATE" />
```

または、Android プロジェクトを右クリックし、プロジェクトのプロパティを開きます。 **[Android マニフェスト]** の下で **[必要なアクセス許可:]** 領域を探し、**VIBRATE** アクセス許可をオンにします。 これにより、**AndroidManifest.xml** ファイルが自動的に更新されます。

# <a name="ios"></a>[iOS](#tab/ios)

追加の設定は必要ありません。

# <a name="uwp"></a>[UWP](#tab/uwp)

プラットフォームによる違いはありません。

-----

## <a name="using-haptic-feedback"></a>Haptic フィードバックの使用

クラスの Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

Haptic フィードバックの機能は、フィードバックの種類 `Click` または `LongPress` を使用して実行できます。

```csharp
try
{
    // Perform click feedback
    HapticFeedback.Perform(HapticFeedbackType.Click);

    // Or use long press    
    HapticFeedback.Perform(HapticFeedbackType.LongPress);
}
catch (FeatureNotSupportedException ex)
{
    // Feature not supported on device
}
catch (Exception ex)
{
    // Other error has occurred.
}
```

## <a name="api"></a>API

- [HapticFeedback ソース コード](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/HapticFeedback)
- [HapticFeedback API のドキュメント](xref:Xamarin.Essentials.HapticFeedback)
