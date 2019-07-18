---
title: スプラッシュ スクリーン
description: Android アプリを起動デバイスでアプリを最初に起動するときに特に時間がかかります。 スプラッシュスクリーンは起動の進捗状態やブランドを表示します。
ms.prod: xamarin
ms.assetid: 26480465-CE19-71CD-FC7D-69D0990D05DE
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 09/06/2018
ms.openlocfilehash: b28dba9031840b312868e2ebc45e348a390d3b12
ms.sourcegitcommit: 58d8bbc19ead3eb535fb8248710d93ba0892e05d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/09/2019
ms.locfileid: "67675048"
---
# <a name="splash-screen"></a>スプラッシュ スクリーン

_Android アプリを起動デバイスでアプリを最初に起動するときに特に時間がかかります。スプラッシュスクリーンは起動の進捗状態やブランドを表示します。_


## <a name="overview"></a>概要

Android アプリ開始する時間のかかる、特に最初の時間中に、アプリがデバイスで実行 (これとも呼ば、_コールド スタート_)。 スプラッシュ スクリーンが表示をユーザーに進行状況を起動したり、特定し、アプリケーションを昇格するブランド化の情報を表示可能性があります。

このガイドでは、Android アプリケーションのスプラッシュ スクリーンを実装する 1 つの方法について説明します。 これには、次の手順について説明します。

1.  スプラッシュ画面に描画可能なリソースを作成します。

2.  描画可能なリソースを表示する新しいテーマを定義します。

3.  前の手順で作成したテーマによって定義された、スプラッシュ スクリーンとして使用されるアプリケーションに新しいアクティビティを追加します。

[![アプリの画面に続く例 Xamarin ロゴのスプラッシュ スクリーン](splash-screen-images/splashscreen-01-sml.png)](splash-screen-images/splashscreen-01.png#lightbox)


## <a name="requirements"></a>必要条件

このガイドでは、アプリケーションが Android API レベル 15 (Android 4.0.3 以降) を対象とするまたはそれ以降。 また、アプリケーションが必要、 **Xamarin.Android.Support.v4**と**Xamarin.Android.Support.v7.AppCompat** NuGet パッケージをプロジェクトに追加します。

すべてのコードと XML では、このガイドを参照して、 [SplashScreen](https://developer.xamarin.com/samples/monodroid/SplashScreen)このガイドのサンプル プロジェクト。


## <a name="implementing-a-splash-screen"></a>スプラッシュ スクリーンを実装します。

レンダリングし、スプラッシュ画面を表示する最も簡単な方法では、カスタム テーマを作成し、スプラッシュ スクリーンが発生しているアクティビティに適用します。 アクティビティが表示されると、テーマを読み込み、アクティビティの背景に描画可能なリソース (テーマによって参照される) を適用します。 このアプローチでは、レイアウト ファイルを作成する必要があります。

スプラッシュ スクリーンは、ブランドを表示するアクティビティとして実装されて、描画可能な任意の初期化を実行し、すべてのタスクを開始します。 アプリが新しくて現在起動中し、スプラッシュ スクリーン アクティビティ メイン アクティビティが開始、アプリケーションのバック スタックから自体を削除します。


### <a name="creating-a-drawable-for-the-splash-screen"></a>描画可能なスプラッシュ スクリーンを作成します。

スプラッシュ スクリーンは、アクティビティのスプラッシュ スクリーンの背景に描画可能な XML で表示されます。 イメージのビットマップ イメージ (PNG や JPG) を使用して表示する必要があります。

このガイドでは使用して、[レイヤーの一覧](https://developer.android.com/guide/topics/resources/drawable-resource.html#LayerList)アプリケーションのスプラッシュ スクリーン イメージを中央にします。 次のスニペットの例に示します、`drawable`を使用してリソースを`layer-list`:

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

これは、`layer-list`ロゴ イメージの中央は**splash.png**で指定されたバック グラウンドで、`@color/splash_background`リソース。 この XML ファイルを配置、**リソース/drawable**フォルダー (たとえば、 **Resources/drawable/splash_screen.xml**)。

描画可能なスプラッシュ スクリーンが作成されたら、次の手順では、スプラッシュ スクリーンのテーマを作成します。


### <a name="implementing-a-theme"></a>テーマを実装します。

スプラッシュ スクリーン アクティビティ用のカスタム テーマを作成、編集 (または追加) ファイル**values/styles.xml**され、新しい作成`style`スプラッシュ スクリーン要素。 サンプル**values/style.xml**でファイルを次に、`style`という**MyTheme.Splash**:

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

**MyTheme.Splash**は非常に spartan&ndash;ウィンドウの背景を宣言して、明示的に ウィンドウで、タイトル バーを削除しますおよび全画面表示であることを宣言します。 使用することができます、アクティビティが最初のレイアウトを拡張する前に、アプリの UI をエミュレートするスプラッシュ スクリーンを作成する場合は、`windowContentOverlay`なく`windowBackground`スタイル定義でします。 この場合、変更することする必要がありますも、 **splash_screen.xml** UI のエミュレーションを表示するように描画します。


### <a name="create-a-splash-activity"></a>スプラッシュ アクティビティを作成します。

これで、ロゴ イメージを備え、スタートアップ タスクが実行を起動する Android 用の新しいアクティビティが必要です。 次のコードでは、完全なスプラッシュ スクリーンの実装の例を示します。

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

`SplashActivity` アプリケーションの既定のテーマをオーバーライドする、前のセクションで作成されたテーマを明示的に使用します。
レイアウトをロードする必要はありません`OnCreate`ように、テーマを背景として描画可能な宣言します。

設定することが重要、`NoHistory=true`属性、アクティビティが戻るスタックから削除されるようにします。 [戻る] ボタンが、起動プロセスをキャンセルするを防ぐするには、オーバーライドの`OnBackPressed`してそれを何もしません。

```csharp
public override void OnBackPressed() { }
```

スタートアップの作業がで非同期的に実行される`OnResume`します。 これは、機能は、スタートアップ作業が低下したり、起動画面の外観を遅延していないように必要です。 作業が完了したら、`SplashActivity`が起動`MainActivity`し、ユーザーがアプリとの対話を開始します。

この新しい`SplashActivity`を設定して、アプリケーションの起動ツール アクティビティとして設定されて、`MainLauncher`属性を`true`します。 `SplashActivity`が編集する必要があります、ランチャー アクティビティ`MainActivity.cs`、および削除、`MainLauncher`から属性`MainActivity`:

```csharp
[Activity(Label = "@string/ApplicationName")]
public class MainActivity : AppCompatActivity
{
    // Code omitted for brevity
}
```

## <a name="landscape-mode"></a>横モード

前の手順で実装されるスプラッシュ画面が縦長と横長の両方のモードで正しく表示されます。 ただし、場合によっては (たとえば、ロゴ イメージが全画面表示である場合) の縦長と横長モード別のスプラッシュ スクリーンを用意する必要があります。

横モードのスプラッシュ スクリーンを追加するには、次の手順を使用します。

1. **リソース/drawable**フォルダー スプラッシュ スクリーン イメージを使用したいのランドス ケープのバージョンを追加します。 この例で**splash_logo_land.png** (白のアイコンの代わりに使用青) 上記の例で使用されていたロゴの横のバージョンします。

2. **リソース/drawable**フォルダー、ランドス ケープのバージョンの作成、 `layer-list` drawable を既に定義されて (たとえば、 **splash_screen_land.xml**)。 このファイルには、スプラッシュ スクリーンのイメージの横のバージョンにビットマップのパスを設定します。 次の例では、 **splash_screen_land.xml**使用**splash_logo_land.png**:

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

3.  作成、**リソース/値の land**フォルダーが存在しない場合。

4.  ファイルを追加**colors.xml**と**style.xml**に**値 land** (コピーされ、既存のものから変更するには、これら**values/colors.xml**と**values/style.xml**ファイル)。

5.  変更**値-land/style.xml**の drawable のランドス ケープのバージョンを使用するよう`windowBackground`します。 この例で**splash_screen_land.xml**使用されます。

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

6.  変更**値-land/colors.xml**スプラッシュ スクリーンのランドス ケープのバージョンを使用する色を構成します。 この例では、ロゴの背景色は横モードの青に変更されています。

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

7.  構築し、再度アプリを実行します。 スプラッシュ スクリーンがまだ表示されている間は横長表示モードにデバイスを回転します。 スプラッシュ スクリーン、ランドス ケープのバージョンを変更します。

    [![横モードにスプラッシュ画面の回転](splash-screen-images/landscape-splash-sml.png)](splash-screen-images/landscape-splash.png#lightbox)


横モードのスプラッシュ スクリーンの使用が、シームレスなエクスペリエンスを常に提供していないことに注意してください。 既定では、Android は縦モードでアプリを起動し、場合でも、デバイスが既に横モードでは横長表示モードに遷移します。 その結果、デバイスが横モードでは、アプリが起動され、デバイスは縦のスプラッシュ スクリーンを簡単に表示し、縦向きからランドス ケープのスプラッシュ画面の回転をアニメーション化します。 残念ながら、この最初の縦の横に遷移が行わ場合でも`ScreenOrientation = Android.Content.PM.ScreenOrientation.Landscape`スプラッシュ アクティビティのフラグで指定します。 この制限を回避する最善の方法では、縦長と横長の両方のモードで正しくレンダリングされる 1 つのスプラッシュ画面イメージを作成します。


## <a name="summary"></a>まとめ

このガイドでは、;、Xamarin.Android アプリケーションでスプラッシュ スクリーンを実装する方法を説明しました。つまり、起動アクティビティをカスタム テーマを適用します。


## <a name="related-links"></a>関連リンク

- [スプラッシュ スクリーン (サンプル)](https://developer.xamarin.com/samples/monodroid/SplashScreen)
- [レイヤーの一覧のディスプレイ](https://developer.android.com/guide/topics/resources/drawable-resource.html#LayerList)
- [素材のデザイン パターン - 起動画面](https://material.io/design/communication/launch-screen.html#usage)
