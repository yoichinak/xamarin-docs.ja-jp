---
title: "ウォッチの文字盤を作成します。"
description: "このガイドでは、Android を着用のカスタム ウォッチ フェイス サービスを実装する方法について説明します。 詳細な手順については、さらに、アナログ スタイル ウォッチの文字盤を作成するコードを続けて、デジタル ウォッチ フェイス サービスをストリップを構築するために提供されます。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 4D3F9A40-A820-458D-A12A-D784BB11F643
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: fb3a2a9e60bda2a99a719bf75d23c29d42a94bdb
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="creating-a-watch-face"></a>ウォッチの文字盤を作成します。

_このガイドでは、Android を着用のカスタム ウォッチ フェイス サービスを実装する方法について説明します。詳細な手順については、さらに、アナログ スタイル ウォッチの文字盤を作成するコードを続けて、デジタル ウォッチ フェイス サービスをストリップを構築するために提供されます。_

## <a name="overview"></a>概要 

このチュートリアルでは、[ウォッチ式の基本的な面サービスが、カスタム Android 着用ウォッチの文字盤の作成の基礎を説明するために作成されます。 初期ウォッチ フェイス サービスには、時間と分で、現在の時刻を表示する簡単なデジタル ウォッチが表示されます。 

[![デジタル ウォッチの文字盤](creating-a-watchface-images/01-initial-face.png "初期デジタル ウォッチの文字盤の例のスクリーン ショット")](creating-a-watchface-images/01-initial-face.png#lightbox)

このデジタル ウォッチの文字盤を開発およびテストされた後よりにアップグレードする、3 つの手でアナログ ウォッチの文字盤を高度なコードが追加されます。 

[![アナログ ウォッチの文字盤](creating-a-watchface-images/02-example-watchface.png "最終的なアナログ ウォッチの文字盤の例のスクリーン ショット")](creating-a-watchface-images/02-example-watchface.png#lightbox)

ウォッチ フェイス サービスは組み込まれ、消耗アプリの一部としてインストールされます。 次の例についてで`MainActivity`ウォッチ フェイス サービスをパッケージ化したり、アプリの一部としてスマート ウォッチに展開できるように消耗アプリ テンプレートからのコードよりも詳細 nothing が含まれます。 実際には、このアプリは、デバッグとテストの損傷、デバイス エミュレーター) に読み込まれるウォッチ フェイス サービスを取得するための手段として純粋使用されます。 

## <a name="requirements"></a>必要条件

ウォッチ フェイス サービスを実装するのには次が必要です。

-   Android 5.0 (API レベル 21) 以上消耗デバイスまたはエミュレーターにします。

-   [Xamarin Android 着用サポート ライブラリ](https://www.nuget.org/packages/Xamarin.Android.Wear)Xamarin.Android プロジェクトに追加する必要があります。 

Android 5.0 が、最小 API レベル ウォッチ フェイス サービス、Android 5.1 の実装を後ではお勧めします。 Android Android 5.1 (API 22) を実行しているデバイスを着用または以降消耗アプリをデバイスが低電力の中に画面に表示される内容を制御できるように*アンビエント*モード。 省電力から、デバイスが離れたとき*アンビエント*モードでは、*インタラクティブ*モード。 これらのモードの詳細についてを参照してください。 [Keeping Your App 表示](https://developer.android.com/training/wearables/apps/always-on.html)です。 


## <a name="start-an-app-project"></a>アプリ プロジェクトを開始します。

いう新しい Android 着用プロジェクトを作成**WatchFace** (新しい Xamarin.Android プロジェクトの作成に関する詳細については、次を参照してください。 [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md))。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![新しいプロジェクト] ダイアログ](creating-a-watchface-images/03-wear-project-vs-sml.png "着用アプリの [新しいプロジェクト] ダイアログ ボックスの選択")](creating-a-watchface-images/03-wear-project-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![新しいプロジェクト] ダイアログ](creating-a-watchface-images/03-wear-project-xs-sml.png "着用アプリの [新しいプロジェクト] ダイアログ ボックスの選択")](creating-a-watchface-images/03-wear-project-xs.png#lightbox)

-----


パッケージ名を設定します`com.xamarin.watchface`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![パッケージの名前の設定](creating-a-watchface-images/04-package-name-vs.png "com.xamarin.watchface にパッケージ名を設定")](creating-a-watchface-images/04-package-name-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![パッケージの名前の設定](creating-a-watchface-images/04-package-name-xs.png "com.xamarin.watchface にパッケージ名を設定")](creating-a-watchface-images/04-package-name-xs.png#lightbox)

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

さらに、下にスクロールし、有効にする、**インターネット**と**WAKE_LOCK**アクセス許可。 

[![アクセス許可を必要な](creating-a-watchface-images/05-required-permissions-vs.png "を有効にするインターネットおよび WAKE_LOCK のアクセス許可")](creating-a-watchface-images/05-required-permissions-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

最低限の Android バージョンを設定**Android 5.1 (API レベル 22)**です。 また、有効にする、**インターネット**と**WakeLock**アクセス許可。

[![アクセス許可を必要な](creating-a-watchface-images/05-required-permissions-xs.png "を有効にするインターネットおよび WakeLock のアクセス許可")](creating-a-watchface-images/05-required-permissions-xs.png#lightbox)

-----

次に、ダウンロード[preview.png](creating-a-watchface-images/preview.png) &ndash;これに追加されます、**ドロウアブル**このチュートリアルの後半でフォルダーです。


## <a name="add-the-xamarin-android-wear-package"></a>Xamarin Android 消耗パッケージを追加します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

NuGet パッケージ マネージャーを起動 (Visual Studio を右クリックし**参照**で、**ソリューション エクスプ ローラー**選択と**NuGet パッケージの管理.**).プロジェクトの最新の安定したバージョンを更新して**Xamarin.Android.Wear**: 

[![NuGet パッケージ マネージャーを追加](creating-a-watchface-images/06-add-wear-pkg-vs-sml.png "Xamarin.Android.Wear パッケージの追加")](creating-a-watchface-images/06-add-wear-pkg-vs.png#lightbox)

次に、 **Xamarin.Android.Support.v13**はこれをアンインストールして、インストールされています。

[![NuGet Package Manager 削除](creating-a-watchface-images/07-uninstall-v13-sml.png "Xamarin.Support.v13 の削除")](creating-a-watchface-images/07-uninstall-v13.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

NuGet パッケージ マネージャーを起動 (Mac を Visual Studio で、右クリック**パッケージ**で、**ソリューション ペイン**選択**パッケージの追加...]**).プロジェクトの最新の安定したバージョンを更新して**Xamarin.Android.Wear**: 

[![NuGet パッケージ マネージャーを追加](creating-a-watchface-images/06-add-wear-pkg-xs-sml.png "Xamarin.Android.Wear パッケージの追加")](creating-a-watchface-images/06-add-wear-pkg-xs.png#lightbox)

-----


ビルドし、ウェア デバイスまたはエミュレーターでアプリを実行 (これを行う方法の詳細については、次を参照してください。、[作業の開始](~/android/wear/get-started/index.md)ガイド)。 消耗デバイスで次のアプリ画面が表示されます。

[![アプリのスクリーン ショット](creating-a-watchface-images/08-app-screen.png "消耗デバイスでアプリの画面")](creating-a-watchface-images/08-app-screen.png#lightbox)

この時点では、ウォッチ フェイス サービス実装がまだ用意されていないために、基本の消耗アプリにはウォッチ フェイス機能はありません。 このサービスは、次に追加されます。 

 
## <a name="canvaswatchfaceservice"></a>CanvasWatchFaceService

Android の消耗を実装を介して面の視聴、`CanvasWatchFaceService`クラスです。 `CanvasWatchFaceService` 派生した`WatchFaceService`、自体はから派生`WallpaperService`次の図に示すようにします。 

[![継承図](creating-a-watchface-images/09-inheritance-diagram-sml.png "CanvasWatchFaceService 継承図")](creating-a-watchface-images/09-inheritance-diagram.png#lightbox)

`CanvasWatchFaceService` 入れ子になったが含まれています`CanvasWatchFaceService.Engine`; がインスタンス化、`CanvasWatchFaceService.Engine`ウォッチの文字盤の描画の実際の処理を行うオブジェクト。 `CanvasWatchFaceService.Engine` 派生した`WallpaperService.Engine`上の図に示すようにします。 

このダイアグラムに表示されませんが、`Canvas`を`CanvasWatchFaceService`ウォッチの文字盤を描画するために使用して&ndash;この`Canvas`経由で渡される、`OnDraw`以下に示すようにメソッドです。 

次のセクションで、次の手順に従ってカスタム ウォッチ フェイス サービスが作成されます。 

1.  クラスを定義`MyWatchFaceService`から派生する`CanvasWatchFaceService`です。 

2.  内で`MyWatchFaceService`と呼ばれる入れ子になったクラスを作成する`MyWatchFaceEngine`から派生する`CanvasWatchFaceService.Engine`です。 

3.  `MyWatchFaceService`、実装、`CreateEngine`インスタンス化するメソッド`MyWatchFaceEngine`しそれを取得します。

4.  `MyWatchFaceEngine`、実装、`OnCreate`ウォッチ面のスタイルを作成し、他の初期化タスクを実行します。 

5.  実装、`OnDraw`メソッドの`MyWatchFaceEngine`します。 ウォッチの文字盤を再描画する必要があるたびに、このメソッドが呼び出されます (つまり*無効*)。 `OnDraw` メソッドを描画 (および再描画) ウォッチ フェイス要素時間、分、および 2 番目の手などです。 

6.  実装、`OnTimeTick`メソッドの`MyWatchFaceEngine`します。 
    `OnTimeTick` ごと (アンビエントと対話型の両方のモード) の分、または日付/時刻が変更されたときに少なくとも 1 回呼び出されます。 

詳細については`CanvasWatchFaceService`、Android を参照してください[CanvasWatchFaceService](https://developer.android.com/reference/android/support/wearable/watchface/CanvasWatchFaceService.html) API のドキュメントです。
同様に、 [CanvasWatchFaceService.Engine](https://developer.android.com/reference/android/support/wearable/watchface/CanvasWatchFaceService.Engine.html)ウォッチの文字盤の実際の実装について説明します。


### <a name="add-the-canvaswatchfaceservice"></a>CanvasWatchFaceService を追加します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

という名前の新しいファイルを追加**MyWatchFaceService.cs** (Visual Studio を右クリックして**WatchFace**で、**ソリューション エクスプ ローラー**をクリックして**追加 > 新しい項目]**を選択して**クラス**)。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

という名前の新しいファイルを追加**MyWatchFaceService.cs** (Mac を Visual Studio で右クリックし、 **WatchFace**プロジェクトで、をクリックして**追加 > 新しいファイル]**を選択して**空のクラス**)。 

-----

このファイルの内容を次のコードに置き換えます。 

```csharp
using System;
using Android.Views;
using Android.Support.Wearable.Watchface;
using Android.Service.Wallpaper;
using Android.Graphics;

namespace WatchFace
{
    class MyWatchFaceService : CanvasWatchFaceService
    {
        public override WallpaperService.Engine OnCreateEngine()
        {
            return new MyWatchFaceEngine(this);
        }

        public class MyWatchFaceEngine : CanvasWatchFaceService.Engine
        {
            CanvasWatchFaceService owner;
            public MyWatchFaceEngine (CanvasWatchFaceService owner) : base(owner)
            {
                this.owner = owner;
            }
        }
    }
}
```

`MyWatchFaceService` (から派生した`CanvasWatchFaceService`) は、ウォッチの文字盤の「メイン プログラム」です。 `MyWatchFaceService` 1 つのメソッドを実装する`OnCreateEngine`、インスタンス化を返す、`MyWatchFaceEngine`オブジェクト (`MyWatchFaceEngine`から派生した`CanvasWatchFaceService.Engine`)。 インスタンス化された`MyWatchFaceEngine`としてオブジェクトを返す必要がある、`WallpaperService.Engine`です。 カプセル化する、`MyWatchFaceService`オブジェクトは、コンス トラクターに渡されます。 

`MyWatchFaceEngine` ウォッチ式の実際の顔実装&ndash;ウォッチの文字盤を描画するコードが含まれています。 また、画面の変更 (アンビエント/対話型モードの場合は、画面の電源を切るなどです。) などのシステム イベントを処理します。 

 
### <a name="implement-the-engine-oncreate-method"></a>エンジン OnCreate メソッドを実装します。

`OnCreate`メソッドがウォッチの文字盤を初期化します。 次のフィールドを追加`MyWatchFaceEngine`: 

```csharp
Paint hoursPaint;
```

これは、`Paint`ウォッチの文字盤上の現在の時刻の描画に使用されるオブジェクト。 次のメソッドを次に、追加`MyWatchFaceEngine`: 

```csharp
public override void OnCreate(ISurfaceHolder holder)
{
    base.OnCreate (holder);

    SetWatchFaceStyle (new WatchFaceStyle.Builder(owner)
        .SetCardPeekMode (WatchFaceStyle.PeekModeShort)
        .SetBackgroundVisibility (WatchFaceStyle.BackgroundVisibilityInterruptive)
        .SetShowSystemUiTime (false)
        .Build ());

    hoursPaint = new Paint();
    hoursPaint.Color = Color.White;
    hoursPaint.TextSize = 48f;
}
```

`OnCreate` 呼び出された後まもなく`MyWatchFaceEngine`が開始します。 設定する、 `WatchFaceStyle` (が制御消耗デバイスが、ユーザーと対話する方法) のインスタンスを作成し、`Paint`時刻を表示するために使用するオブジェクト。 

呼び出し`SetWatchFaceStyle`は次の実行します。

1.  セット*ピーク モード*に`PeekModeShort`、それが原因で通知を表示上の小さな「ピーク」カードとして表示されます。 

2.  バック グラウンドの可視性に設定`Interruptive`、それが原因で中断の通知を表すかどうかにのみについて簡単に表示されるピーク カードの背景。

3.  カスタム ウォッチの文字盤では、時間を代わりに表示できるように、ウォッチの文字盤で描画されないという既定のシステム UI 時間を無効にします。

これらおよび他のウォッチ フェイス スタイル オプションの詳細については、Android を参照してください。 [WatchFaceStyle.Builder](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceStyle.Builder.html) API のドキュメントです。

後に`SetWatchFaceStyle`が完了したら、`OnCreate`をインスタンス化、`Paint`オブジェクト (`hoursPaint`) を白と、テキスト サイズが 48 ピクセルにその色に設定し、([TextSize](https://developer.android.com/reference/android/graphics/Paint.html#setTextSize%28float%29)ピクセル単位で指定する必要があります)。 


### <a name="implement-the-engine-ondraw-method"></a>エンジンの OnDraw メソッドを実装します。

`OnDraw`メソッドは、最も重要なおそらく`CanvasWatchFaceService.Engine`メソッド&ndash;メソッドを実際に描画数字などの面要素を見るし、フェイス手のクロックであります。 次の例では、ウォッチの文字盤で時刻の文字列を描画します。
次のメソッドを追加`MyWatchFaceEngine`:

```csharp
public override void OnDraw (Canvas canvas, Rect frame)
{
    var str = DateTime.Now.ToString ("h:mm tt");
    canvas.DrawText (str, 
        (float)(frame.Left + 70), 
        (float)(frame.Top  + 80), hoursPaint);
}
```

Android を呼び出すと`OnDraw`、渡します、`Canvas`インスタンスと境界の表面を描画できます。 上記のコード例では`DateTime`時間の現在の時間と分 (12 時間形式) での計算に使用します。 結果として得られる時刻の文字列を使用して、キャンバスに描画、`Canvas.DrawText`メソッドです。 文字列上表示されます 70 ピクセル、左端 80 ピクセルからダウン上端から。 

詳細については、`OnDraw`メソッド、Android を参照してください[onDraw](https://developer.android.com/reference/android/support/wearable/watchface/CanvasWatchFaceService.Engine.html#onDraw(android.graphics.Canvas, android.graphics.Rect)) API のドキュメントです。


### <a name="implement-the-engine-ontimetick-method"></a>エンジン OnTimeTick メソッドを実装します。

Android が定期的に呼び出して、`OnTimeTick`ウォッチの文字盤で表示される時間を更新する方法です。 (アンビエントと対話型の両方のモードで)、1 分ごとに少なくとも 1 回または日付/時刻またはタイム ゾーンが変更されたときに呼び出されます。 次のメソッドを追加`MyWatchFaceEngine`: 

```csharp
public override void OnTimeTick()
{
    Invalidate();
}
```

この実装`OnTimeTick`を呼び出すだけ`Invalidate`です。 `Invalidate`メソッド スケジュール`OnDraw`ウォッチの文字盤を再描画します。 

詳細については、`OnTimeTick`メソッド、Android を参照してください[onTimeTick](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onTimeTick()) API のドキュメントです。

 
## <a name="register-the-canvaswatchfaceservice"></a>登録、CanvasWatchFaceService

`MyWatchFaceService` 登録する必要があります、 **AndroidManifest.xml**関連付けられた消耗アプリのです。 これを行うには、次の XML を追加、`<application>`セクション。 

```xml
<service 
    android:name="watchface.MyWatchFaceService" 
    android:label="Xamarin Sample" 
    android:allowEmbedded="true" 
    android:taskAffinity="" 
    android:permission="android.permission.BIND_WALLPAPER">
    <meta-data 
        android:name="android.service.wallpaper" 
        android:resource="@xml/watch_face" />
    <meta-data 
        android:name="com.google.android.wearable.watchface.preview" 
        android:resource="@drawable/preview" />
    <intent-filter>
        <action android:name="android.service.wallpaper.WallpaperService" />
        <category android:name="com.google.android.wearable.watchface.category.WATCH_FACE" />
    </intent-filter>
</service>
```

この XML は次のとおり

1.  セット、`android.permission.BIND_WALLPAPER`権限です。 このアクセス許可は、デバイス上のシステムの壁紙を変更するウォッチ フェイス サービスのアクセス許可を与えます。 このアクセス許可を設定する必要がありますに注意してください、`<service>`セクションで、外部ではなく`<application>`セクションです。 

2.  定義、`watch_face`リソース。 このリソースは短い XML ファイルを宣言する、`wallpaper`リソース (このファイルは、次のセクションで作成されます)。 

3.  宣言と呼ばれるドロウアブル イメージ`preview`ウォッチ ピッカーの選択] 画面で表示されます。 

4.  含まれています、`intent-filter`させることに注意して Android`MyWatchFaceSevice`ウォッチの文字盤を表示します。 

Basic の場合、コードを完了する`WatchFace`例です。 次の手順では、必要なリソースを追加します。

 
## <a name="add-resource-files"></a>リソース ファイルを追加します。

監視サービスを実行する前に追加する必要があります、 **watch_face**リソースとプレビュー イメージ。 新しい XML ファイルを最初に、作成**Resources/xml/watch_face.xml**しその内容を次の XML に置き換えます。 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<wallpaper xmlns:android="http://schemas.android.com/apk/res/android" />
```

このファイルのビルド アクション設定**AndroidResource**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![ビルド アクション](creating-a-watchface-images/10-android-resource-vs.png "セット ビルド AndroidResource するアクション")](creating-a-watchface-images/10-android-resource-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![ビルド アクション](creating-a-watchface-images/10-android-resource-xs.png "セット ビルド AndroidResource するアクション")](creating-a-watchface-images/10-android-resource-xs.png#lightbox)

-----

このリソース ファイルを定義、単純な`wallpaper`ウォッチの文字盤のために使用する要素。 

まだいない場合は、ダウンロード[preview.png](creating-a-watchface-images/preview.png)です。 インストール**Resources/drawable/preview.png**です。 このファイルを追加することを確認する、`WatchFace`プロジェクト。 このプレビュー イメージは、ウォッチ フェイス ピッカー消耗デバイス上でユーザーに表示されます。 独自ウォッチの文字盤のプレビュー イメージを作成するには、実行中にウォッチの文字盤のスクリーン ショットを実行できます。 (詳細消耗デバイスからのスクリーン ショットの取得については、次を参照してください。[スクリーン ショットを取得](~/android/wear/deploy-test/debug-on-device.md#screenshots))。 


## <a name="try-it"></a>手順を次に示します。

ビルドし、損傷、デバイスにアプリを配置します。 以前と同様に表示される消耗アプリ画面が表示されます。 新しいウォッチの文字盤を有効にするのには、次の操作を行います。 

1.  [ウォッチ] 画面の背景が表示されるまで右にスワイプします。 

2.  タッチして、2 秒の画面の背景で任意の場所を保持します。

3.  左から右を各種のウォッチ式の面で参照するにスワイプします。 

4.  選択、 **Xamarin サンプル**フェイス (右側に表示) を監視します。 

    [![Watchface ピッカー](creating-a-watchface-images/11-watchface-picker.png "スワイプ Xamarin サンプル ウォッチの文字盤を検索するには")](creating-a-watchface-images/11-watchface-picker.png#lightbox)

5.  タップして、 **Xamarin サンプル**視聴フェイスをオンにします。 

これは、これまでに実装されたカスタム ウォッチ フェイス サービスを使用する消耗デバイスのウォッチの文字盤を変更します。 

[![デジタル ウォッチの文字盤](creating-a-watchface-images/12-digital-watchface.png "消耗デバイスで実行されているカスタム デジタル ウォッチ")](creating-a-watchface-images/12-digital-watchface.png#lightbox)

これは、アプリの実装がように最小限に抑えるためにの比較的粗雑ウォッチの文字盤 (たとえば、ウォッチ表面の背景に含まれていないことを呼び出しません`Paint`を見やすくするために、アンチ エイリアス メソッド)。 ただし、カスタム ウォッチの文字盤の作成に必要な最低限機能が実装されます。 

次のセクションでは、このウォッチの文字盤をより高度な実装にアップグレードされます。 

 
## <a name="upgrading-the-watch-face"></a>ウォッチの文字盤のアップグレード

このチュートリアルの残りの部分で`MyWatchFaceService`は、アナログ スタイル ウォッチの文字盤の表示にアップグレードされより多くの機能をサポートするために拡張されています。 アップグレードされたウォッチの文字盤を作成する、次の機能が追加されます。 

1.  アナログの時間、分、および 2 番目の手で時間を示します。

2.  可視性の変更に対して動作します。

3.  アンビエント モードと対話型モードの変更に応答します。 

4.  基になる消耗デバイスのプロパティを読み取ります。

5.  タイム ゾーンの変更がいつ行わ時刻が自動的に更新します。 

次のコードの変更を実装する前にダウンロード[drawable.zip](https://github.com/xamarin/monodroid-samples/blob/master/wear/WatchFace/Resources/drawable.zip?raw=true)解凍し、および解凍の .png ファイルの移動**リソース/描画**(前の上書き**preview.png**). 新しい .png ファイルを追加、`WatchFace`プロジェクト。


### <a name="update-engine-features"></a>エンジンの機能を更新します。

次の手順はアップグレード**MyWatchFaceService.cs**アナログのウォッチの文字盤を描画し、新機能をサポートを実装します。 内容を置き換える**MyWatchFaceService.cs**ウォッチ フェイス コードのアナログのバージョンと[MyWatchFaceService.cs](https://github.com/xamarin/monodroid-samples/blob/master/wear/WatchFace/WatchFace/MyWatchFaceService.cs) (切り取りし、ソースはこの既存に貼り付けることができます**MyWatchFaceService.cs**)。 

このバージョンの**MyWatchFaceService.cs**既存のメソッドにより多くのコードを追加しより多くの機能を追加する追加のオーバーライドされたメソッドが含まれています。 次のセクションでは、ソース コードのガイド付きツアーを提供します。

#### <a name="oncreate"></a>OnCreate

更新された**OnCreate**メソッドが以前と同様、ウォッチ面のスタイルを構成しますが、追加の手順が含まれています。 

1.  背景画像の設定、 **xamarin_background**内にあるリソース**Resources/drawable-hdpi/xamarin_background.png**です。 

2.  初期化`Paint`手の時間、分手、および秒針を描画するためのオブジェクト。

3.  初期化、`Paint`ウォッチの文字盤の端に時間刻みを描画するためのオブジェクト。 

4.  タイマーを作成するには、その呼び出し、 `Invalidate` (再描画) メソッド秒針は 1 秒間に再描画されるようにします。 このタイマーが必要なことに注意してくださいため`OnTimeTick`呼び出し`Invalidate`毎分 1 回だけです。

この例には、1 つだけが含まれます**xamarin_background.png**イメージです。 ただし、の各画面密度カスタム ウォッチの文字盤をサポートする別の背景イメージを作成する場合があります。 

#### <a name="ondraw"></a>OnDraw

更新された**OnDraw**メソッドは、次の手順を使用して、アナログ スタイル ウォッチの表面を描画します。 

1.  現在管理されている現在の時刻を取得、`time`オブジェクト。 

2.  描画サーフェイスおよびそのセンターの境界を決定します。

3.  背景が描画されると、デバイスに合わせて拡大縮小、背景を描画します。

4.  12 個の描画*タイマー刻み*(盤の時間に対応する)、クロックの顔周りです。 

5.  角度、回転、および各ウォッチ手の形の長さを計算します。

6.  ウォッチ画面で、各手の形を描画します。 秒針では、ウォッチがアンビエント モードの場合を描画されていないことに注意してください。 

 
#### <a name="onpropertieschanged"></a>OnPropertiesChanged

このメソッドは、通知するために呼び出されます`MyWatchFaceEngine`(低いビット アンビエント モードと保護バーンイン) などの消耗デバイスのプロパティに関するします。 `MyWatchFaceEngine`、このメソッドは、低ビット アンビエント モード用にのみチェック (低いビット アンビエント モードでは、画面をサポートしているビット数が少ないの各色)。 

この方法の詳細については、Android を参照してください。 [onPropertiesChanged](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onPropertiesChanged%28android.os.Bundle%29) API のドキュメントです。


#### <a name="onambientmodechanged"></a>OnAmbientModeChanged

消耗デバイスに入るかアンビエント モードを終了すると、このメソッドが呼び出されます。 `MyWatchFaceEngine`実装では、ウォッチの文字盤を無効にアンチエイリアシング アンビエント モードにあるときです。 

この方法の詳細については、Android を参照してください。 [onAmbientModeChanged](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onAmbientModeChanged%28boolean%29) API のドキュメントです。


#### <a name="onvisibilitychanged"></a>OnVisibilityChanged

表示または非表示、ウォッチになるたびにこのメソッドが呼び出されます。 `MyWatchFaceEngine`、このメソッドのレジスタ/の登録を解除 (下記参照)、可視性の状態に従ってタイム ゾーンの受信者です。 

この方法の詳細については、Android を参照してください。 [onVisibilityChanged](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onVisibilityChanged%28boolean%29) API のドキュメントです。

 
### <a name="time-zone-feature"></a>タイム ゾーンの機能

新しい**MyWatchFaceService.cs**タイム ゾーンの変更 (タイム ゾーン間で出張中など) と現在の時間を更新する機能もあります。 末尾近く**MyWatchFaceService.cs**、ゾーンの変更時`BroadcastReceiver`が定義されているダイナミック オブジェクトのタイム ゾーンの変更を処理します。 

```csharp
public class TimeZoneReceiver: BroadcastReceiver
{
    public Action<Intent> Receive { get; set; }
    public override void OnReceive (Context context, Intent intent)
    {
        if (Receive != null)
            Receive (intent);
    }
}
```

`RegisterTimezoneReceiver`と`UnregisterTimezoneReceiver`メソッドが呼び出される、`OnVisibilityChanged`メソッドです。 
`UnregisterTimezoneReceiver` 呼ばれるをウォッチの文字盤の可視性の状態が変更されたときに非表示します。 ウォッチの文字盤が再度表示される`RegisterTimezoneReceiver`と呼びます (を参照してください、`OnVisibilityChanged`メソッド)。 

エンジン`RegisterTimezoneReceiver`メソッドは、このタイム ゾーン レシーバーのハンドラーを宣言`Receive`イベントです。 このハンドラーが更新、`time`タイム ゾーンを超えたときに、新しい時刻を持つオブジェクト。 

```csharp
timeZoneReceiver = new TimeZoneReceiver ();
timeZoneReceiver.Receive = (intent) => {
    time.Clear (intent.GetStringExtra ("time-zone"));
    time.SetToNow ();
};
```

目的のフィルターが作成され、タイム ゾーンの受信者の登録します。

```csharp
IntentFilter filter = new IntentFilter(Intent.ActionTimezoneChanged);
Application.Context.RegisterReceiver (timeZoneReceiver, filter);
```

`UnregisterTimezoneReceiver`メソッド タイム ゾーンの受信者の登録を解除します。

```csharp
Application.Context.UnregisterReceiver (timeZoneReceiver);
```

### <a name="run-the-improved-watch-face"></a>強化されたウォッチの文字盤を実行します。

ビルドし、もう一度消耗デバイスにアプリを展開します。 として前に、ウォッチ フェイス ピッカーからウォッチの文字盤を選択します。 ウォッチ ピッカーでプレビューは、左側に表示され、新しいウォッチの文字盤が右側に表示されます。

[![アナログ ウォッチの文字盤](creating-a-watchface-images/13-analog-watchface.png "アナログ フェイス ピッカー、およびデバイス上の向上")](creating-a-watchface-images/13-analog-watchface.png#lightbox)

このスクリーン ショットでは、秒針を毎秒 1 回移動です。 消耗デバイスでこのコードを実行すると、ウォッチ アンビエント モードに入る秒針表示されなくなります。

 
## <a name="summary"></a>まとめ

このチュートリアルでは、カスタムの Android 着用 watchface が実装およびテストします。 `CanvasWatchFaceService`と`CanvasWatchFaceService.Engine`クラスが導入された、およびエンジン クラスの重要なメソッドが実装され、単純なデジタル ウォッチの文字盤を作成します。 この実装は、アナログのウォッチの文字盤を作成する多くの機能と更新され、可視性、アンビエント モード、およびデバイスのプロパティの違いに変更を処理する追加のメソッドが実装されました。 最後に、タイム ゾーンの放送受信機は、ウォッチが自動的には、タイム ゾーンをいつ超える時間を更新するように実装されています。 


## <a name="related-links"></a>関連リンク

- [ウォッチ面を作成します。](https://developer.android.com/training/wearables/watch-faces/index.html)
- [WatchFace サンプル](https://developer.xamarin.com/samples/monodroid/wear/WatchFace)
- [WatchFaceService.Engine](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html)
