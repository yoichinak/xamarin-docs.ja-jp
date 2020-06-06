---
タイトル: "Xamarin.Essentials: 振動" の説明: "このドキュメントでは、バイブレーション機能を開始して一定時間後に停止することができる Xamarin.Essentials の Vibration クラスについて説明します。"
ms.assetid:7E8B24C4-2625-4DAE-A129-383542D34F1E author: jamesmontemagno ms.custom: video ms.author: jamont ms.date:11/04/2018 no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# <a name="xamarinessentials-vibration"></a>Xamarin.Essentials:振動

**Vibration** クラスを使用すると、バイブレーション機能を開始して一定時間後に停止することができます。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

**Vibration** の機能にアクセスするには、次のプラットフォーム固有の設定が必要です。

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

## <a name="using-vibration"></a>Vibration の使用

クラスの Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

Vibration 機能では、有効にする時間を要求でき、既定値は 500 ミリ秒です。

```csharp
try
{
    // Use default vibration length
    Vibration.Vibrate();

    // Or use specified time
    var duration = TimeSpan.FromSeconds(1);
    Vibration.Vibrate(duration);
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

デバイスのバイブレーションの取り消しは、`Cancel` メソッドで要求できます。

```csharp
try
{
    Vibration.Cancel();
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

## <a name="platform-differences"></a>プラットフォームによる違い

# <a name="android"></a>[Android](#tab/android)

プラットフォームによる違いはありません。

# <a name="ios"></a>[iOS](#tab/ios)

- デバイスが "着信時にバイブレーション" に設定されているときにのみバイブレーションします。
- 常に 500 ミリ秒だけバイブレーションします。
- バイブレーションを取り消すことはできません。

# <a name="uwp"></a>[UWP](#tab/uwp)

プラットフォームによる違いはありません。

-----

## <a name="api"></a>API

- [Vibration のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Vibration)
- [Vibration API のドキュメント](xref:Xamarin.Essentials.Vibration)

## <a name="related-video"></a>関連ビデオ

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Vibration-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
