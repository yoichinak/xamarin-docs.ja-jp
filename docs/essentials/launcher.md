---
title: Xamarin.Essentials ランチャー
description: Xamarin.Essentials の Launcher クラスを使用すると、アプリケーションがシステムで URI を開くことができるようになります。
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.custom: video
ms.author: jamont
ms.date: 08/20/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 1e68755778522fa61d593d25e763fae1569724cf
ms.sourcegitcommit: 2a7bbe9cbee3727ba20ee755c1713bcfdb4d8ecb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2021
ms.locfileid: "98950977"
---
# <a name="xamarinessentials-launcher"></a>Xamarin.Essentials:ランチャー

**Launcher** クラスを使用すると、アプリケーションがシステムで URI を開くことができるようになります。 これは多くの場合、他のアプリケーションのカスタム URI スキームへのディープ リンクを設定するときに使用されます。 ブラウザーで Web サイトを開く場合は、 **[Browser](open-browser.md)** API を参照する必要があります。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-launcher"></a>ランチャーの使用

クラスの Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

ランチャーの機能を使用するには、`OpenAsync` メソッドを呼び出し、開く `string` または `Uri` を渡します。 必要に応じて、デバイス上のアプリケーションで URI スキーマを処理できるかどうかを確認するために `CanOpenAsync` メソッドを使用できます。

```csharp
public class LauncherTest
{
    public async Task OpenRideShareAsync()
    {
        var supportsUri = await Launcher.CanOpenAsync("lyft://");
        if (supportsUri)
            await Launcher.OpenAsync("lyft://ridetype?id=lyft_line");
    }
}
```

これは、`TryOpenAsync` を含む単一の呼び出しに結合することができます。これにより、パラメーターを開くことができるかどうかがチェックされ、可能な場合は開かれます。

```csharp
public class LauncherTest
{
    public async Task<bool> OpenRideShareAsync()
    {
        return await Launcher.TryOpenAsync("lyft://ridetype?id=lyft_line");
    }
}
```

### <a name="additional-platform-setup"></a>その他のプラットフォームのセットアップ

# <a name="android"></a>[Android](#tab/android)

追加のセットアップはありません。

# <a name="ios"></a>[iOS](#tab/ios)

iOS 9 以降、Apple は、アプリケーションが照会できるスキームを強制しています。 使用したいスキームを指定するには、`Info.plist` ファイルで `LSApplicationQueriesSchemes` を指定する必要があります。

```
<key>LSApplicationQueriesSchemes</key>
<array>
    <string>lyft</string>  
    <string>fb</string>
</array>
```

# <a name="uwp"></a>[UWP](#tab/uwp)

追加のセットアップはありません。

-----

## <a name="files"></a>ファイル

この機能により、アプリでは他のアプリにファイルを開いて表示するように要求できます。 Xamarin.Essentials では、ファイルの種類 (MIME) が自動的に検出され、ファイルを開くように要求されます。

次に示すのは、テキストをディスクに書き込んで、それを開くように要求するサンプルです。

```csharp
var fn = "File.txt";
var file = Path.Combine(FileSystem.CacheDirectory, fn);
File.WriteAllText(file, "Hello World");

await Launcher.OpenAsync(new OpenFileRequest
{
    File = new ReadOnlyFile(file)
});
```

## <a name="presentation-location-when-opening-files"></a>ファイルを開くときの表示の位置

[!include[](~/essentials/includes/ios-PresentationSourceBounds.md)]

## <a name="platform-differences"></a>プラットフォームによる違い

# <a name="android"></a>[Android](#tab/android)

`CanOpenAsync` から返されたタスクはすぐに完了します。

# <a name="ios"></a>[iOS](#tab/ios)

自分のアプリケーションから `OpenAsync` でこのデバイス上の目的のアプリケーションを開いたことがない場合、iOS によって、ユーザーは一度、自分のアプリがそれを開くことを許可するよう求められます。

`CanOpenAsync` から返されたタスクはすぐに完了します。

iOS の実装について詳しくは、[こちら](xref:UIKit.UIApplication.CanOpenUrl*)をご覧ください。

# <a name="uwp"></a>[UWP](#tab/uwp)

プラットフォームによる違いはありません。

-----

## <a name="api"></a>API

- [Launcher のソース コード](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/Launcher)
- [Launcher API ドキュメント](xref:Xamarin.Essentials.Launcher)

## <a name="related-video"></a>関連ビデオ

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Launcher-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
