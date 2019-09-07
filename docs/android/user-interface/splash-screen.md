---
title: スプラッシュ スクリーン
description: Android アプリの起動には時間がかかります。特に、アプリがデバイスで最初に起動されるときです。 スプラッシュスクリーンは起動の進捗状態やブランドを表示します。
ms.prod: xamarin
ms.assetid: 26480465-CE19-71CD-FC7D-69D0990D05DE
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 09/06/2018
ms.openlocfilehash: b05ab7ee835a97f13af618332baec7a5ebf404ec
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70764083"
---
# <a name="splash-screen"></a>スプラッシュ スクリーン

_Android アプリの起動には時間がかかります。特に、アプリがデバイスで最初に起動されるときです。スプラッシュスクリーンは起動の進捗状態やブランドを表示します。_

## <a name="overview"></a>概要

Android アプリの起動には時間がかかります。特に、デバイスでアプリを初めて実行するとき (_コールドスタート_と呼ばれることもあります)、 スプラッシュスクリーンでは、ユーザーに [開始] の進行状況が表示されます。または、アプリケーションを識別して昇格させるためのブランド情報が表示される場合があります。

このガイドでは、Android アプリケーションでスプラッシュスクリーンを実装する方法の1つについて説明します。 次の手順について説明します。

1. スプラッシュスクリーンの描画されたリソースを作成します。

2. 作成されたリソースを表示する新しいテーマを定義します。

3. 前の手順で作成したテーマによって定義されたスプラッシュスクリーンとして使用される、新しいアクティビティをアプリケーションに追加します。

[![Xamarin ロゴのスプラッシュスクリーンの後にアプリの画面を表示する例](splash-screen-images/splashscreen-01-sml.png)](splash-screen-images/splashscreen-01.png#lightbox)

## <a name="requirements"></a>必要条件

このガイドでは、アプリケーションが Android API レベル 15 (Android 4.0.3) 以降を対象としていることを前提としています。 また、アプリケーションに**は、プロジェクト**に追加された v7 パッケージと**xamarin. android** .......

このガイドのすべてのコードと XML は、このガイドの[SplashScreen](https://docs.microsoft.com/samples/xamarin/monodroid-samples/splashscreen)サンプルプロジェクトに記載されています。

## <a name="implementing-a-splash-screen"></a>スプラッシュスクリーンの実装

スプラッシュスクリーンをレンダリングして表示する最も簡単な方法は、カスタムテーマを作成し、スプラッシュスクリーンを示すアクティビティにそれを適用することです。 アクティビティがレンダリングされると、テーマが読み込まれ、(テーマによって参照される) 描画されたリソースがアクティビティの背景に適用されます。 この方法を使用すると、レイアウトファイルを作成する必要がなくなります。

スプラッシュスクリーンは、ブランド化された描画を表示し、すべての初期化を実行し、任意のタスクを開始するアクティビティとして実装されます。 アプリがブートストラップされると、スプラッシュスクリーンアクティビティはメインアクティビティを開始し、それ自体をアプリケーションのバックスタックから削除します。

### <a name="creating-a-drawable-for-the-splash-screen"></a>スプラッシュスクリーン用の描画用の作成

スプラッシュスクリーンでは、スプラッシュスクリーンアクティビティの背景に XML を描画できます。 画像を表示するには、ビットマップイメージ (PNG、JPG など) を使用する必要があります。

このガイドでは、[レイヤーリスト](https://developer.android.com/guide/topics/resources/drawable-resource.html#LayerList)を使用して、アプリケーションでスプラッシュスクリーンイメージを中央揃えにします。 `drawable` を`layer-list`使用したリソースの例を次のスニペットに示します。

```xml
<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">
  <item>
    <color android:color="@color/splash_background"/>
  </item>
  <item>
    <bitmap
        android:src="@drawable/splash"
        android:tileMode="disabled"
        android:gravity="center"/>
  </item>
</layer-list>
```

これ`layer-list`により、 `@color/splash_background`リソースによって指定された背景にスプラッシュイメージの**スプラッシュ**が中央に表示されます。 この XML ファイルは **、resources/splash_screen フォルダーに**配置します (たとえば、 **resources/アブル/** )。

スプラッシュスクリーンの描画を作成した後、次の手順ではスプラッシュスクリーンのテーマを作成します。

### <a name="implementing-a-theme"></a>テーマを実装する

スプラッシュスクリーン活動用のカスタムテーマを作成するには、ファイル**値/スタイル .xml**を編集 (または追加) し、 `style`スプラッシュスクリーン用の新しい要素を作成します。 サンプル**値/スタイルの .xml**ファイルを、 `style` **mytheme**という名前の付いた次に示します。

```xml
<resources>
  <style name="MyTheme.Base" parent="Theme.AppCompat.Light">
  </style>

  <style name="MyTheme" parent="MyTheme.Base">
  </style>

  <style name="MyTheme.Splash" parent ="Theme.AppCompat.Light.NoActionBar">
    <item name="android:windowBackground">@drawable/splash_screen</item>
    <item name="android:windowNoTitle">true</item>
    <item name="android:windowFullscreen">true</item>
  </style>
</resources>
```

Spartan&ndash;は、ウィンドウの背景を宣言し、ウィンドウからタイトルバーを明示的に削除して、それが全画面表示であることを宣言することを示してい**ます。** アクティビティが最初のレイアウトを増えする前に、アプリの UI をエミュレートするスプラッシュスクリーンを作成する場合は、スタイル`windowContentOverlay`定義`windowBackground`ではなくを使用できます。 この場合は、UI のエミュレーションを表示するように**splash_screen**の作成されたファイルを変更する必要もあります。

### <a name="create-a-splash-activity"></a>スプラッシュアクティビティを作成する

ここで、スプラッシュイメージを含む Android を起動し、スタートアップタスクを実行する新しいアクティビティが必要です。 次のコードは、スプラッシュスクリーンの完全実装の例です。

```csharp
[Activity(Theme = "@style/MyTheme.Splash", MainLauncher = true, NoHistory = true)]
public class SplashActivity : AppCompatActivity
{
    static readonly string TAG = "X:" + typeof(SplashActivity).Name;

    public override void OnCreate(Bundle savedInstanceState, PersistableBundle persistentState)
    {
        base.OnCreate(savedInstanceState, persistentState);
        Log.Debug(TAG, "SplashActivity.OnCreate");
    }

    // Launches the startup task
    protected override void OnResume()
    {
        base.OnResume();
        Task startupWork = new Task(() => { SimulateStartup(); });
        startupWork.Start();
    }

    // Simulates background work that happens behind the splash screen
    async void SimulateStartup ()
    {
        Log.Debug(TAG, "Performing some startup work that takes a bit of time.");
        await Task.Delay (8000); // Simulate a bit of startup work.
        Log.Debug(TAG, "Startup work is finished - starting MainActivity.");
        StartActivity(new Intent(Application.Context, typeof (MainActivity)));
    }
}
```

`SplashActivity`前のセクションで作成したテーマを明示的に使用して、アプリケーションの既定のテーマをオーバーライドします。
テーマが描画をバックグラウンドとして宣言`OnCreate`するため、でレイアウトを読み込む必要はありません。

`NoHistory=true`属性を設定して、アクティビティがバックスタックから削除されるようにすることが重要です。 [戻る] ボタンによってスタートアッププロセスがキャンセルされないよう`OnBackPressed`にするには、をオーバーライドして何も実行しないようにすることもできます。

```csharp
public override void OnBackPressed() { }
```

スタートアップ作業は、で非同期に`OnResume`実行されます。 これは、起動作業の速度が低下したり、起動画面が表示されなくなったりしないようにするために必要です。 作業が完了すると、 `SplashActivity`が起動`MainActivity`し、ユーザーがアプリとの対話を開始する可能性があります。

この新しい`SplashActivity`は、 `MainLauncher`属性をに設定する`true`ことによって、アプリケーションのランチャーアクティビティとして設定されます。 がランチャーアクティビティになったので、次`MainActivity.cs`のようにを`MainLauncher`編集し`MainActivity`てから属性を削除する必要があります。 `SplashActivity`

```csharp
[Activity(Label = "@string/ApplicationName")]
public class MainActivity : AppCompatActivity
{
    // Code omitted for brevity
}
```

## <a name="landscape-mode"></a>横モード

前の手順で実装したスプラッシュスクリーンは、縦モードと横モードの両方で正しく表示されます。 ただし、場合によっては、縦モードと横モードで別々のスプラッシュスクリーンを使用する必要があります (スプラッシュイメージが全画面表示の場合など)。

横モードのスプラッシュスクリーンを追加するには、次の手順に従います。

1. [**リソース/** 作成] フォルダーで、使用するスプラッシュスクリーンイメージの横バージョンを追加します。 この例では、splash_logo_land は、上の例で使用したロゴの横バージョンです (青ではなくホワイト文字を使用し**ます**)。

2. **Resources/** splash_screen_land フォルダーで、前に定義した、( `layer-list`たとえば、) の前に定義した描画用の横バージョンを作成します。 このファイルで、ビットマップパスをスプラッシュスクリーンイメージの横バージョンに設定します。 次の例では、 **splash_screen_land**は**splash_logo_land**を使用します。

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <layer-list xmlns:android="http://schemas.android.com/apk/res/android">
      <item>
        <color android:color="@color/splash_background"/>
      </item>
      <item>
        <bitmap
            android:src="@drawable/splash_logo_land"
            android:tileMode="disabled"
            android:gravity="center"/>
      </item>
    </layer-list>
    ```

3. **Resources/values-陸**フォルダーがまだ存在しない場合は作成します。

4. ファイルの**色 .xml**と**スタイル .xml**を**値-土地**に追加します (これらは、既存の**値/色 .xml**と**値/スタイルの .xml**ファイルからコピーおよび変更できます)。

5. **Values-land/style .xml**を変更して、用の描画用`windowBackground`のランドスケープバージョンを使用するようにします。 この例では、 **splash_screen_land**が使用されます。

    ```xml
    <resources>
      <style name="MyTheme.Base" parent="Theme.AppCompat.Light">
      </style>
        <style name="MyTheme" parent="MyTheme.Base">
      </style>
      <style name="MyTheme.Splash" parent ="Theme.AppCompat.Light.NoActionBar">
        <item name="android:windowBackground">@drawable/splash_screen_land</item>
        <item name="android:windowNoTitle">true</item>  
        <item name="android:windowFullscreen">true</item>  
        <item name="android:windowContentOverlay">@null</item>  
        <item name="android:windowActionBar">true</item>  
      </style>
    </resources>
    ```

6. **Values-land/colors**を変更して、スプラッシュスクリーンの横バージョンで使用する色を構成します。 この例では、横モードの場合、スプラッシュの背景色が青に変更されます。

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <resources>
      <color name="primary">#2196F3</color>
      <color name="primaryDark">#1976D2</color>
      <color name="accent">#FFC107</color>
      <color name="window_background">#F5F5F5</color>
      <color name="splash_background">#3498DB</color>
    </resources>
    ```

7. アプリをもう一度ビルドして実行します。 スプラッシュスクリーンがまだ表示されている状態で、デバイスを横モードに切り替えます。 スプラッシュスクリーンが横向きバージョンに変わります。

    [![スプラッシュスクリーンから横モードへの回転](splash-screen-images/landscape-splash-sml.png)](splash-screen-images/landscape-splash.png#lightbox)

横モードのスプラッシュスクリーンを使用しても、常にシームレスなエクスペリエンスが提供されるわけではないことに注意してください。 既定では、Android は縦モードでアプリを起動し、デバイスが既に横モードになっている場合でも横モードに切り替えます。 その結果、デバイスが横モードになっているときにアプリを起動すると、デバイスは簡単に縦向きのスプラッシュスクリーンを表示し、縦から横方向のスプラッシュスクリーンへの回転をアニメーション化します。 残念ながら、スプラッシュアクティビティのフラグでが指定されている`ScreenOrientation = Android.Content.PM.ScreenOrientation.Landscape`場合でも、この初期の縦から横への切り替えが行われます。 この制限を回避する最善の方法は、縦モードと横モードの両方で正しくレンダリングされるスプラッシュスクリーンイメージを1つ作成することです。

## <a name="summary"></a>Summary

このガイドでは、Xamarin Android アプリケーションでスプラッシュスクリーンを実装する方法の1つを説明しました。つまり、起動アクティビティにカスタムテーマを適用します。

## <a name="related-links"></a>関連リンク

- [SplashScreen (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/splashscreen)
- [レイヤー一覧の描画](https://developer.android.com/guide/topics/resources/drawable-resource.html#LayerList)
- [素材のデザインパターン-起動画面](https://material.io/design/communication/launch-screen.html#usage)
