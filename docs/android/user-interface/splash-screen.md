---
title: スプラッシュ スクリーン
description: Android アプリでは、起動、特に、アプリが最初に起動した場合、デバイス上に時間がかかります。 スプラッシュスクリーンは起動の進捗状態やブランドを表示します。
ms.prod: xamarin
ms.assetid: 26480465-CE19-71CD-FC7D-69D0990D05DE
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/14/2018
ms.openlocfilehash: 6200a04bb4d82174d36a48beab7c63709ac39187
ms.sourcegitcommit: c5bb1045b2f4607dafe3101ad1ea6ade23e44342
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/14/2018
ms.locfileid: "34153198"
---
# <a name="splash-screen"></a>スプラッシュ スクリーン

_Android アプリでは、起動、特に、アプリが最初に起動した場合、デバイス上に時間がかかります。スプラッシュスクリーンは起動の進捗状態やブランドを表示します。_


## <a name="overview"></a>概要

Android アプリを起動する時間のかかる、特に最初の時間中にデバイスでアプリを実行 (これとも呼ば、_コールド スタート_)。 スプラッシュ画面が表示の進行状況をユーザーが起動またはブランド情報を特定し、アプリケーションの昇格を表示できます。

このガイドでは、Android アプリケーションのスプラッシュ スクリーンを実装する 1 つの方法について説明します。 次の手順がについて説明します。

1.  スプラッシュ スクリーンにドロウアブル リソースを作成します。

2.  ドロウアブル リソースを表示する新しいテーマを定義します。

3.  前の手順で作成されたテーマによって定義されるスプラッシュ スクリーンとして使用されるアプリケーションに新しいアクティビティを追加します。

[![アプリの画面に続く例 Xamarin ロゴ スプラッシュ スクリーン](splash-screen-images/splashscreen-01-sml.png)](splash-screen-images/splashscreen-01.png#lightbox)


## <a name="requirements"></a>要件

このガイドでは、アプリケーションが Android API レベル 15 (Android 4.0.3 以降) を対象と仮定またはそれ以降。 また、アプリケーションが必要、 **Xamarin.Android.Support.v4**と**Xamarin.Android.Support.v7.AppCompat** NuGet パッケージのプロジェクトに追加します。

すべてのコードと XML このガイドでは、「、 [SplashScreen](https://developer.xamarin.com/samples/monodroid/SplashScreen)このガイド用のサンプル プロジェクト。


## <a name="implementing-a-splash-screen"></a>スプラッシュ スクリーンを実装します。

表示したり、スプラッシュ画面を表示する最も簡単な方法では、カスタム テーマを作成し、スプラッシュ スクリーンが発生しているアクティビティに適用します。 アクティビティが表示されると、テーマを読み込み、アクティビティの背景にドロウアブル リソース (テーマによって参照される) を適用します。 この方法では、レイアウト ファイルを作成する必要があります。

スプラッシュ スクリーンは、ブランドを表示するアクティビティとして実装ドロウアブル、任意の初期化を実行し、すべてのタスクを開始します。 アプリがブートス トラップで使う、アクティビティのスプラッシュ スクリーン、メインのアクティビティを開始し、アプリケーション バック スタックからそれ自体を削除します。


### <a name="creating-a-drawable-for-the-splash-screen"></a>スプラッシュ スクリーンのかもしれないの作成

スプラッシュ スクリーンは、アクティビティのスプラッシュ スクリーンの背景にドロウアブル XML で表示されます。 表示するイメージの (JPG や PNG) などのビットマップ イメージを使用する必要があります。

このガイドでは使用して、[レイヤー リスト](http://developer.android.com/guide/topics/resources/drawable-resource.html#LayerList)アプリケーションのスプラッシュ スクリーンのイメージの中央にします。 次のスニペットの例に示します、`drawable`リソースを使用して、 `layer-list`:

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

これは、`layer-list`スプラッシュ スクリーンのイメージが中央**splash.png**で指定されたバック グラウンドで、`@color/splash_background`リソース。
このファイルを配置、**リソース/描画**フォルダー (たとえば、 **Resources/drawable/splash_screen.xml**)。

ドロウアブル スプラッシュ画面が作成された後、次の手順は、スプラッシュ スクリーンのテーマを作成するは。


### <a name="implementing-a-theme"></a>テーマを実装します。

スプラッシュ画面のアクティビティ用のカスタム テーマを作成、編集 (または追加) ファイル**values/styles.xml**され、新しい作成`style`スプラッシュ スクリーンの要素。 サンプル**values/style.xml**でファイルを次に、`style`という**MyTheme.Splash**:

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

**MyTheme.Splash**は非常にスパルタ&ndash;ウィンドウの背景色を宣言して、明示的にウィンドウで、タイトル バーを削除および全画面表示であることを宣言します。 使用することができます、アクティビティが最初のレイアウトを拡張する前に、アプリの UI をエミュレートするスプラッシュ スクリーンを作成する場合は、`windowContentOverlay`なく`windowBackground`スタイル定義にします。 この場合も変更があります、 **splash_screen.xml** UI のエミュレーションを表示するようドロウアブルです。


### <a name="create-a-splash-activity"></a>スプラッシュ アクティビティを作成します。

今すぐに、ロゴ イメージがあるし、スタートアップ タスクを実行するを起動する Android 用の新しいアクティビティが必要です。 次のコードは、完全なスプラッシュ画面の実装の例です。

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
レイアウトをロードする必要はありません`OnCreate`ように、テーマがかもしれない背景としてを宣言します。

設定することが重要、`NoHistory=true`属性のアクティビティがバック スタックから削除されるようにします。 [戻る] ボタンが、起動プロセスをキャンセルするを防ぐするには、オーバーライドの`OnBackPressed`してそれを何もしません。

```csharp
public override void OnBackPressed() { }
```

非同期的に実行されるスタートアップ処理`OnResume`です。 これは、機能は、スタートアップ作業が低下したり起動画面の外観を遅延していないように必要です。 作業が完了したら、`SplashActivity`が起動`MainActivity`し、ユーザーがアプリとの対話を開始することがあります。

この新しい`SplashActivity`を設定して、アプリケーションの起動ツール アクティビティとして設定されて、`MainLauncher`属性を`true`です。 `SplashActivity`はこれで、編集する必要があります起動ツール アクティビティ`MainActivity.cs`、および削除、`MainLauncher`属性から`MainActivity`:

```csharp
[Activity(Label = "@string/ApplicationName")]
public class MainActivity : AppCompatActivity
{
    // Code omitted for brevity
}
```

## <a name="landscape-mode"></a>横モード

前の手順で実装されるスプラッシュ画面は、縦長と横長の両方のモードで正しく表示されます。 ただし、場合によっては (たとえば、ロゴ イメージが全画面表示である場合)、別のスプラッシュ スクリーンの縦方向と横モードを設定する必要です。

横向きモードのスプラッシュ スクリーンを追加するには、次の手順を使用します。

1. **リソース/描画**フォルダー、スプラッシュ スクリーンのイメージを使用する場合のランドス ケープ バージョンを追加します。 この例では**splash_logo_land.png** (使用して白レタリング青ではなく) 上記の例で使用されていたロゴの横のバージョンです。

2. **リソース/描画**フォルダーのランドス ケープ バージョンを作成、`layer-list`ドロウアブルを以前に定義した (たとえば、 **splash_screen_land.xml**)。 このファイルには、スプラッシュ スクリーンのイメージのランドス ケープ バージョンにビットマップのパスを設定します。 次の例では、 **splash_screen_land.xml**使用**splash_logo_land.png**:

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

3.  作成、**リソース/値の土地**フォルダーが存在しない場合。

4.  ファイルを追加**colors.xml**と**style.xml**に**値土地**(コピーされ、既存のものから変更するには、これら**values/colors.xml**と**values/style.xml**ファイル)。

5.  変更**値-土地/style.xml**ドロウアブル用のランドス ケープ バージョンを使用するように`windowBackground`です。 この例では**splash_screen_land.xml**を使用します。

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

6.  変更**値-土地/colors.xml**スプラッシュ スクリーンのランドス ケープ バージョンを使用する色を構成します。 この例では、ロゴの背景色が青の横長に変更されます。

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

7.  ビルドして、アプリケーションを再実行します。 スプラッシュ スクリーンがまだ表示されている間は、モードを横向きにデバイスを回転します。 スプラッシュ スクリーンは、ランドス ケープのバージョンに変更します。

    [![横向きモードにスプラッシュ画面の回転](splash-screen-images/landscape-splash-sml.png)](splash-screen-images/landscape-splash.png#lightbox)


横モードのスプラッシュ スクリーンの使用を提供しないこと、シームレスなエクスペリエンスに注意してください。 既定は、Android は、縦向きモードでアプリを起動し、デバイスが横モードで既に場合でも、モードを横向きに遷移します。 その結果、デバイスが横モードでは、アプリを起動、デバイスは縦スプラッシュ スクリーンを簡単に表示し、し、回転、縦から横スプラッシュ スクリーンをアニメーション化します。 残念ながら、この最初の縦の横の遷移が行わ場合でも、`ScreenOrientation = Android.Content.PM.ScreenOrientation.Landscape`スプラッシュ アクティビティのフラグで指定します。 この制限を回避する最善の方法では、縦長と横長の両方のモードで正しくレンダリングする 1 つのスプラッシュ スクリーンのイメージを作成します。


## <a name="summary"></a>まとめ

このガイドには、アプリケーションに実装するスプラッシュ スクリーン、Xamarin.Android; の 1 つの方法が説明されています。つまり、起動のアクティビティにカスタム テーマを適用しています。


## <a name="related-links"></a>関連リンク

- [スプラッシュ スクリーン (サンプル)](https://developer.xamarin.com/samples/monodroid/SplashScreen)
- [レイヤーの一覧のディスプレイ](http://developer.android.com/guide/topics/resources/drawable-resource.html#LayerList)
- [ 素材のデザイン パターン - 起動画面](https://www.google.com/design/spec/patterns/launch-screens.html)
