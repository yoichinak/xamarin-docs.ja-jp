---
title: スプラッシュ スクリーン
description: Android アプリの起動には時間がかかります。特に、アプリがデバイスで最初に起動されるときです。 スプラッシュスクリーンで、[開始の進行状況] がユーザーに表示されるか、ブランド化が示されることがあります。
ms.prod: xamarin
ms.assetid: 26480465-CE19-71CD-FC7D-69D0990D05DE
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 10/02/2019
ms.openlocfilehash: bd24fbf6ce99e87c6a7f4ccd8cceef6cbe826f0f
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93370534"
---
# <a name="splash-screen"></a>スプラッシュ スクリーン

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/monodroid-samples/splashscreen)

_Android アプリの起動には時間がかかります。特に、アプリがデバイスで最初に起動されるときです。スプラッシュスクリーンで、[開始の進行状況] がユーザーに表示されるか、ブランド化が示されることがあります。_

## <a name="overview"></a>概要

Android アプリの起動には時間がかかります。特に、デバイスでアプリを初めて実行するとき ( _コールドスタート_ と呼ばれることもあります)、 スプラッシュスクリーンでは、ユーザーに [開始] の進行状況が表示されます。または、アプリケーションを識別して昇格させるためのブランド情報が表示される場合があります。

このガイドでは、Android アプリケーションでスプラッシュスクリーンを実装する方法の1つについて説明します。 次の手順について説明します。

1. スプラッシュスクリーンの描画されたリソースを作成します。

2. 作成されたリソースを表示する新しいテーマを定義します。

3. 前の手順で作成したテーマによって定義されたスプラッシュスクリーンとして使用される、新しいアクティビティをアプリケーションに追加します。

[![Xamarin ロゴのスプラッシュスクリーンの後にアプリの画面を表示する例](splash-screen-images/splashscreen-01-sml.png)](splash-screen-images/splashscreen-01.png#lightbox)

## <a name="requirements"></a>必要条件

このガイドでは、アプリケーションが Android API レベル21以上を対象としていることを前提としています。 また、アプリケーションに **は、プロジェクト** に追加された v7 パッケージと **xamarin. android** .......

このガイドのすべてのコードと XML は、このガイドの [SplashScreen](/samples/xamarin/monodroid-samples/splashscreen) サンプルプロジェクトに記載されています。

## <a name="implementing-a-splash-screen"></a>スプラッシュスクリーンの実装

スプラッシュスクリーンをレンダリングして表示する最も簡単な方法は、カスタムテーマを作成し、スプラッシュスクリーンを示すアクティビティにそれを適用することです。 アクティビティがレンダリングされると、テーマが読み込まれ、(テーマによって参照される) 描画されたリソースがアクティビティの背景に適用されます。 この方法を使用すると、レイアウトファイルを作成する必要がなくなります。

スプラッシュスクリーンは、ブランド化された描画を表示し、すべての初期化を実行し、任意のタスクを開始するアクティビティとして実装されます。 アプリがブートストラップされると、スプラッシュスクリーンアクティビティはメインアクティビティを開始し、それ自体をアプリケーションのバックスタックから削除します。

### <a name="creating-a-drawable-for-the-splash-screen"></a>スプラッシュスクリーン用の描画用の作成

スプラッシュスクリーンでは、スプラッシュスクリーンアクティビティの背景に XML を描画できます。 画像を表示するには、ビットマップイメージ (PNG、JPG など) を使用する必要があります。

このサンプルアプリケーションでは、 **splash_screen.xml** という名前の描画を定義しています。 この描画は、 [レイヤーリスト](https://developer.android.com/guide/topics/resources/drawable-resource.html#LayerList) を使用して、次の xml に示すように、アプリケーションのスプラッシュスクリーンイメージを中心にしています。

```xml
<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">
  <item>
    <color android:color="@color/splash_background"/>
  </item>
  <item>
    <bitmap
        android:src="@drawable/splash_logo"
        android:tileMode="disabled"
        android:gravity="center"/>
  </item>
</layer-list>
```

これに `layer-list` より、リソースによって指定された背景色でスプラッシュイメージが中心に `@color/splash_background` なります。 このサンプルアプリケーションでは、 **Resources/values/colors.xml** ファイルにこの色を定義しています。

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
  ...
  <color name="splash_background">#FFFFFF</color>
</resources>
```

オブジェクトの詳細について `Drawable` は、Android の描画用の [Google ドキュメント](https://developer.android.com/reference/android/graphics/drawable/Drawable)を参照してください。

### <a name="implementing-a-theme"></a>テーマを実装する

スプラッシュスクリーンアクティビティのカスタムテーマを作成するには、ファイルの **値/styles.xml** を編集 (または追加) し、 `style` スプラッシュスクリーン用の新しい要素を作成します。 サンプル **値/style.xml** ファイルは、次に示す mytheme という名前の付いてい `style` **ます。スプラッシュ** :

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
    <item name="android:windowContentOverlay">@null</item>  
    <item name="android:windowActionBar">true</item>  
  </style>
</resources>
```

**MyTheme.Splash** Spartan は、ウィンドウの背景を &ndash; 宣言し、ウィンドウからタイトルバーを明示的に削除して、それが全画面表示であることを宣言することを示しています。 アクティビティが最初のレイアウトを増えする前に、アプリの UI をエミュレートするスプラッシュスクリーンを作成する場合は、 `windowContentOverlay` スタイル定義ではなくを使用でき `windowBackground` ます。 この場合は、UI のエミュレーションを表示するように、 **splash_screen.xml** の組み込みを変更する必要もあります。

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

`SplashActivity` 前のセクションで作成したテーマを明示的に使用して、アプリケーションの既定のテーマをオーバーライドします。
`OnCreate`テーマが描画をバックグラウンドとして宣言するため、でレイアウトを読み込む必要はありません。

属性を設定して、 `NoHistory=true` アクティビティがバックスタックから削除されるようにすることが重要です。 [戻る] ボタンによってスタートアッププロセスがキャンセルされないようにするには、をオーバーライド `OnBackPressed` して何も実行しないようにすることもできます。

```csharp
public override void OnBackPressed() { }
```

スタートアップ作業は、で非同期に実行され `OnResume` ます。 これは、起動作業の速度が低下したり、起動画面が表示されなくなったりしないようにするために必要です。 作業が完了すると、 `SplashActivity` が起動 `MainActivity` し、ユーザーがアプリとの対話を開始する可能性があります。

この新しい `SplashActivity` は、属性をに設定することによって、アプリケーションのランチャーアクティビティとして設定され `MainLauncher` `true` ます。 がランチャーアクティビティになったので、次のようにを `SplashActivity` 編集 `MainActivity.cs` してから属性を削除する必要があり `MainLauncher` `MainActivity` ます。

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

1. [ **リソース/** 作成] フォルダーで、使用するスプラッシュスクリーンイメージの横バージョンを追加します。 この例では、 **splash_logo_land.png** が、上の例で使用したロゴの横バージョンです (青ではなくホワイト文字が使用されています)。

2. **Resources/** 作成されたフォルダーで、前に定義した `layer-list` (たとえば、 **splash_screen_land.xml** ) 前に定義した描画用の横バージョンを作成します。 このファイルで、ビットマップパスをスプラッシュスクリーンイメージの横バージョンに設定します。 次の例では **splash_screen_land.xml** 、splash_screen_land.xml **splash_logo_land.png** を使用します。

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

3. **Resources/values-陸** フォルダーがまだ存在しない場合は作成します。

4. ファイル **colors.xml** と **style.xml** を [ **値** ] に追加します (これらのファイルは、既存の  **値/colors.xml** と **値/style.xml** ファイルからコピーして変更できます)。

5. 値を変更して、用の描画用のランドスケープバージョンを使用するように **style.xml** `windowBackground` します。 この例では、 **splash_screen_land.xml** が使用されています。

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

6. 値を変更して、スプラッシュスクリーンの横バージョンで使用する色を構成します。 [ **colors.xml** ] を選択します。 この例では、横モードの場合、スプラッシュの背景色が青に変更されます。

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

横モードのスプラッシュスクリーンを使用しても、常にシームレスなエクスペリエンスが提供されるわけではないことに注意してください。 既定では、Android は縦モードでアプリを起動し、デバイスが既に横モードになっている場合でも横モードに切り替えます。 その結果、デバイスが横モードになっているときにアプリを起動すると、デバイスは簡単に縦向きのスプラッシュスクリーンを表示し、縦から横方向のスプラッシュスクリーンへの回転をアニメーション化します。 残念ながら、 `ScreenOrientation = Android.Content.PM.ScreenOrientation.Landscape` スプラッシュアクティビティのフラグでが指定されている場合でも、この初期の縦から横への切り替えが行われます。 この制限を回避する最善の方法は、縦モードと横モードの両方で正しくレンダリングされるスプラッシュスクリーンイメージを1つ作成することです。

## <a name="summary"></a>まとめ

このガイドでは、Xamarin Android アプリケーションでスプラッシュスクリーンを実装する方法の1つを説明しました。つまり、起動アクティビティにカスタムテーマを適用します。

## <a name="related-links"></a>関連リンク

- [SplashScreen (サンプル)](/samples/xamarin/monodroid-samples/splashscreen)
- [レイヤー一覧の描画](https://developer.android.com/guide/topics/resources/drawable-resource.html#LayerList)
- [素材のデザインパターン-起動画面](https://material.io/design/communication/launch-screen.html#usage)