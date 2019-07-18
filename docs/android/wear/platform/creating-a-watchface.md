---
title: Android Wear 1.0 のウォッチの文字盤を作成します。
description: このガイドでは、Android Wear 1.0 用のカスタム ウォッチ face サービスを実装する方法について説明します。 詳細な手順については、アナログ スタイル ウォッチの文字盤を作成するためのコードを続けて、デジタル ウォッチ face サービスを停止、削除を構築するために提供されます。
ms.prod: xamarin
ms.assetid: 4D3F9A40-A820-458D-A12A-D784BB11F643
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/23/2018
ms.openlocfilehash: 067a39838fbfe3f1b33ac0d30b5069366b11e407
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61287219"
---
# <a name="creating-a-watch-face"></a>ウォッチの文字盤を作成する

_このガイドでは、Android Wear 1.0 用のカスタム ウォッチ face サービスを実装する方法について説明します。詳細な手順については、アナログ スタイル ウォッチの文字盤を作成するためのコードを続けて、デジタル ウォッチ face サービスを停止、削除を構築するために提供されます。_

## <a name="overview"></a>概要

このチュートリアルでは、ウォッチの基本的な面のサービスは、カスタムの Android Wear 1.0 ウォッチの文字盤を作成する基礎を説明するために作成されます。
初期ウォッチ顔のサービスでは、時間と分で、現在の時刻を表示する単純なデジタル時計が表示されます。

[![デジタル ウォッチの文字盤](creating-a-watchface-images/01-initial-face.png "初期のデジタル ウォッチの文字盤の例のスクリーン ショット")](creating-a-watchface-images/01-initial-face.png#lightbox)

このデジタル ウォッチの文字盤を開発およびテストされた後よりにアップグレードして、次の 3 つの手でアナログ ウォッチの文字盤高度なコードが追加されます。

[![アナログ ウォッチの文字盤](creating-a-watchface-images/02-example-watchface.png "最終的なアナログ ウォッチの文字盤の例のスクリーン ショット")](creating-a-watchface-images/02-example-watchface.png#lightbox)

顔のサービスがバンドルされているし、1.0 の Wear アプリの一部としてインストールをご覧ください。 次の例で`MainActivity`ウォッチ顔のサービスをパッケージ化し、アプリの一部としてスマート ウォッチに展開されているように 1.0 の Wear アプリ テンプレートからコードにすぎませんが含まれます。 実際には、このアプリは、デバッグとテスト Wear 1.0 デバイス (またはエミュレーター) に読み込まれるウォッチ face サービスを取得するための手段としてのみ使用されます。

## <a name="requirements"></a>必要条件

ウォッチ顔のサービスを実装するために、次が必要です。

-   Android 5.0 (API レベル 21) または Wear デバイスまたはエミュレーターにそれ以降。

-   [Xamarin Android Wear サポート ライブラリ](https://www.nuget.org/packages/Xamarin.Android.Wear)Xamarin.Android プロジェクトに追加する必要があります。

Android 5.0 は、最小の API を Android 5.1、ウォッチ face サービスの実装のレベルがまたは後でお勧めします。 Android Wear Android 5.1 (API 22) を実行しているデバイスまたはデバイスが低電力の間に、画面に表示される内容を制御する Wear アプリを許可する以上*アンビエント*モード。 デバイスから離したときに低電力*アンビエント*では、モード、*対話型*モード。 詳細については、これらのモードは、次を参照してください。[維持 Your App 表示](https://developer.android.com/training/wearables/apps/always-on.html)します。


## <a name="start-an-app-project"></a>アプリ プロジェクトを開始します。

という新しい Android Wear 1.0 プロジェクト作成**WatchFace** (新しい Xamarin.Android プロジェクトの作成の詳細については、次を参照してください。 [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md))。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![新しいプロジェクト ダイアログ](creating-a-watchface-images/03-wear-project-vs-sml.png "Wear アプリの [新しいプロジェクト] ダイアログ ボックスの選択")](creating-a-watchface-images/03-wear-project-vs.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![新しいプロジェクト ダイアログ](creating-a-watchface-images/03-wear-project-xs-sml.png "Wear アプリの [新しいプロジェクト] ダイアログ ボックスの選択")](creating-a-watchface-images/03-wear-project-xs.png#lightbox)

-----


パッケージ名を設定`com.xamarin.watchface`:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![パッケージの名前の設定](creating-a-watchface-images/04-package-name-vs.png "com.xamarin.watchface にパッケージ名を設定")](creating-a-watchface-images/04-package-name-vs.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![パッケージの名前の設定](creating-a-watchface-images/04-package-name-xs.png "com.xamarin.watchface にパッケージ名を設定")](creating-a-watchface-images/04-package-name-xs.png#lightbox)

-----

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

さらに、下にスクロールし、有効にする、**インターネット**と**WAKE_LOCK**アクセス許可。

[![アクセス許可が必要な](creating-a-watchface-images/05-required-permissions-vs.png "を有効にするインターネットと WAKE_LOCK のアクセス許可")](creating-a-watchface-images/05-required-permissions-vs.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

最小 Android バージョンを設定**Android 5.1 (API レベル 22)** します。
また、有効にする、**インターネット**と**WakeLock**アクセス許可。

[![アクセス許可が必要な](creating-a-watchface-images/05-required-permissions-xs.png "を有効にするインターネットと WakeLock のアクセス許可")](creating-a-watchface-images/05-required-permissions-xs.png#lightbox)

-----

次に、ダウンロード[preview.png](creating-a-watchface-images/preview.png) &ndash;これに追加、**ドローアブル**このチュートリアルの後半でフォルダー。


## <a name="add-the-xamarinandroid-wear-package"></a>Xamarin.Android Wear パッケージを追加します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

NuGet パッケージ マネージャーを起動 (Visual Studio で、右クリックして**参照**で、**ソリューション エクスプ ローラー**選択**NuGet パッケージの管理**).最新の安定したバージョンにプロジェクトを更新**Xamarin.Android.Wear**:

[![NuGet パッケージ マネージャーの追加](creating-a-watchface-images/06-add-wear-pkg-vs-sml.png "Xamarin.Android.Wear パッケージの追加")](creating-a-watchface-images/06-add-wear-pkg-vs.png#lightbox)

次に、if **Xamarin.Android.Support.v13**は、インストール、アンインストールします。

[![NuGet パッケージ マネージャーの削除](creating-a-watchface-images/07-uninstall-v13-sml.png "Xamarin.Support.v13 の削除")](creating-a-watchface-images/07-uninstall-v13.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

NuGet パッケージ マネージャーを起動 (Visual Studio for Mac では、右クリック**パッケージ**で、**ソリューション ウィンドウ**選択**パッケージの追加...**).最新の安定したバージョンにプロジェクトを更新**Xamarin.Android.Wear**:

[![NuGet パッケージ マネージャーの追加](creating-a-watchface-images/06-add-wear-pkg-xs-sml.png "Xamarin.Android.Wear パッケージの追加")](creating-a-watchface-images/06-add-wear-pkg-xs.png#lightbox)

-----


ビルド、Wear デバイスまたはエミュレーターでアプリを実行して (これを行う方法の詳細については、次を参照してください。、 [Getting Started](~/android/wear/get-started/index.md)ガイド)。 Wear デバイスでは、次のアプリ画面を表示する必要があります。

[![アプリのスクリーン ショット](creating-a-watchface-images/08-app-screen.png "Wear デバイスでアプリの画面")](creating-a-watchface-images/08-app-screen.png#lightbox)

この時点では、ウォッチ face サービスの実装がまだ用意されていないために、基本的な Wear アプリにはウォッチ顔の機能はありません。 このサービスは、次に追加されます。


## <a name="canvaswatchfaceservice"></a>CanvasWatchFaceService

Android Wear 実装見る顔を使って、`CanvasWatchFaceService`クラス。 `CanvasWatchFaceService` 派生`WatchFaceService`、自体に由来`WallpaperService`次の図に示すようにします。

[![継承ダイアグラム](creating-a-watchface-images/09-inheritance-diagram-sml.png "CanvasWatchFaceService 継承ダイアグラム")](creating-a-watchface-images/09-inheritance-diagram.png#lightbox)

`CanvasWatchFaceService` 入れ子になったが含まれています`CanvasWatchFaceService.Engine`; をインスタンス化、`CanvasWatchFaceService.Engine`ウォッチの文字盤の描画の実際の処理を行うオブジェクト。 `CanvasWatchFaceService.Engine` 派生`WallpaperService.Engine`上の図に示すようにします。

この図で示されていませんが、`Canvas`を`CanvasWatchFaceService`ウォッチの文字盤を描画するために使用して&ndash;この`Canvas`によって渡された、`OnDraw`メソッドを以下に示すよう。

次のセクションで次の手順に従って、カスタム ウォッチ face サービスが作成されます。

1.  クラスを定義`MyWatchFaceService`から派生する`CanvasWatchFaceService`します。

2.  内で`MyWatchFaceService`、という入れ子になったクラスを作成`MyWatchFaceEngine`から派生する`CanvasWatchFaceService.Engine`します。

3.  `MyWatchFaceService`、実装、`CreateEngine`メソッドをインスタンス化する`MyWatchFaceEngine`され返されます。

4.  `MyWatchFaceEngine`、実装、`OnCreate`メソッドをウォッチ面のスタイルを作成し、他の初期化タスクを実行します。

5.  実装、`OnDraw`メソッドの`MyWatchFaceEngine`します。 ウォッチの文字盤を再描画する必要があるたびに、このメソッドが呼び出されます (つまり*無効*)。 `OnDraw` hour、minute、および 2 つ目のハンドなど、ウォッチ face 要素を描画します (および再描画) メソッドです。

6.  実装、`OnTimeTick`メソッドの`MyWatchFaceEngine`します。
    `OnTimeTick` ごと (アンビエントと対話型の両方のモード) で分単位または日付/時刻が変更されたときが少なくとも 1 回呼び出されます。

詳細については`CanvasWatchFaceService`、Android を参照してください。 [CanvasWatchFaceService](https://developer.android.com/reference/android/support/wearable/watchface/CanvasWatchFaceService.html) API のドキュメント。
同様に、 [CanvasWatchFaceService.Engine](https://developer.android.com/reference/android/support/wearable/watchface/CanvasWatchFaceService.Engine.html)ウォッチの文字盤の実際の実装について説明します。


### <a name="add-the-canvaswatchfaceservice"></a>追加、CanvasWatchFaceService

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

という新しいファイルを追加**MyWatchFaceService.cs** (Visual Studio で、右クリックして**WatchFace**で、**ソリューション エクスプ ローラー**、 をクリックして**追加 > 新しい項目...**、選び**クラス**)。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

という新しいファイルを追加**MyWatchFaceService.cs** (Visual studio for Mac を右クリックし、 **WatchFace**プロジェクトで、をクリックして**追加 > 新しいファイル...**、選び**空のクラス**)。

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

`MyWatchFaceService` (から派生した`CanvasWatchFaceService`) はウォッチの文字盤の「メイン プログラム」です。 `MyWatchFaceService` 1 つのメソッドを実装する`OnCreateEngine`、インスタンス化して返しますが、`MyWatchFaceEngine`オブジェクト (`MyWatchFaceEngine`から派生`CanvasWatchFaceService.Engine`)。 インスタンス化された`MyWatchFaceEngine`としてオブジェクトを返す必要がある、`WallpaperService.Engine`します。 カプセル化する、`MyWatchFaceService`オブジェクトは、コンス トラクターに渡されます。

`MyWatchFaceEngine` 実際のウォッチ face 実装&ndash;ウォッチの文字盤を描画するコードが含まれます。 画面の変更 (アンビエント/対話型モードでは、画面をオフにするなど。) などのシステム イベントも処理します。


### <a name="implement-the-engine-oncreate-method"></a>エンジンの OnCreate メソッドを実装します。

`OnCreate`メソッドがウォッチの文字盤を初期化します。 次のフィールドを追加`MyWatchFaceEngine`:

```csharp
Paint hoursPaint;
```

これは、`Paint`オブジェクトは、ウォッチの文字盤を現在の時刻を描画するために使用されます。 次のメソッドを次に、追加`MyWatchFaceEngine`:

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

`OnCreate` すぐ後と呼ばれる`MyWatchFaceEngine`が開始します。 設定、 `WatchFaceStyle` (コントロール Wear デバイスが、ユーザーと対話する方法) をインスタンス化し、`Paint`時刻を表示するために使用するオブジェクト。

呼び出し`SetWatchFaceStyle`は次の処理します。

1.  セット*ピーク モード*に`PeekModeShort`、それが原因で通知を表示上の小さな「ピーク」カードとして表示されます。

2.  バック グラウンドの可視性を設定`Interruptive`、それが原因で、通知を持とうとするものを表すかどうかにのみについて簡単に表示される、ピーク カードの背景。

3.  カスタムの腕時計が代わりに、時間を表示できるように、ウォッチの文字盤で描画する既定のシステム UI 時間を無効にします。

これらおよびその他の watch face スタイルのオプションの詳細については、Android を参照してください。 [WatchFaceStyle.Builder](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceStyle.Builder.html) API のドキュメント。

後`SetWatchFaceStyle`が完了したら、`OnCreate`をインスタンス化、`Paint`オブジェクト (`hoursPaint`) の色を白と 48 ピクセルにそのテキストのサイズに設定し、([TextSize](https://developer.android.com/reference/android/graphics/Paint.html#setTextSize%28float%29)ピクセル単位で指定する必要があります)。


### <a name="implement-the-engine-ondraw-method"></a>エンジンの OnDraw メソッドを実装します。

`OnDraw`メソッドは、最も重要な`CanvasWatchFaceService.Engine`メソッド&ndash;が、実際に描画桁の数字などの要素の顔を見るとメソッド face 両手のクロックします。
次の例では、ウォッチの文字盤で時刻の文字列を描画します。
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

Android を呼び出すと`OnDraw`、渡します、`Canvas`インスタンスと表面を描画する境界。 上記のコード例で`DateTime`時間の現在の時間と分 (12 時間形式) での計算に使用されます。 結果として得られる時刻の文字列を使用して、キャンバスに描画されます、`Canvas.DrawText`メソッド。 文字列上表示されます 70 ピクセル、左端と 80 ピクセルからダウン上端から。

詳細については、`OnDraw`メソッドでは、Android を参照してください。 [onDraw](https://developer.android.com/reference/android/support/wearable/watchface/CanvasWatchFaceService.Engine#ondraw) API のドキュメント。


### <a name="implement-the-engine-ontimetick-method"></a>実装エンジン OnTimeTick メソッド

Android が定期的に呼び出して、`OnTimeTick`ウォッチの文字盤での時間を更新するメソッド。 (アンビエントと対話型の両方のモードで)、1 分ごとに少なくとも 1 回または日付/時刻またはタイム ゾーンが変更されたときに呼び出されます。 次のメソッドを追加`MyWatchFaceEngine`:

```csharp
public override void OnTimeTick()
{
    Invalidate();
}
```

この実装の`OnTimeTick`を呼び出すだけです`Invalidate`します。 `Invalidate`メソッド スケジュール`OnDraw`ウォッチの文字盤を再描画します。

詳細については、`OnTimeTick`メソッドでは、Android を参照してください。 [onTimeTick](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onTimeTick()) API のドキュメント。


## <a name="register-the-canvaswatchfaceservice"></a>登録、CanvasWatchFaceService

`MyWatchFaceService` 登録する必要があります、 **AndroidManifest.xml**の関連付けられている Wear アプリ。 これを行うには、次の XML を追加、`<application>`セクション。

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

この XML は、次を行います。

1.  セット、`android.permission.BIND_WALLPAPER`権限。 このアクセス許可は、デバイスでシステムの壁紙を変更するのには、ウォッチ顔サービスのアクセス許可を提供します。 このアクセス許可を設定する必要がありますに注意してください、`<service>`セクションではなく、外部`<application>`セクション。

2.  定義、`watch_face`リソース。 このリソースは、宣言する簡単な XML ファイル、`wallpaper`リソース (このファイルは、次のセクションで作成されます)。

3.  宣言と呼ばれる描画可能なイメージ`preview`ウォッチ ピッカーの選択 画面で表示されます。

4.  含まれています、`intent-filter`を知っています。 Android`MyWatchFaceService`ウォッチの文字盤を表示します。

Basic の場合、コードは完成`WatchFace`例。 次の手順では、必要なリソースを追加します。


## <a name="add-resource-files"></a>リソース ファイルを追加します。

追加する必要があります、監視サービスを実行する前に、 **watch_face**リソースとプレビュー イメージ。 新しい XML ファイルを最初に、作成**Resources/xml/watch_face.xml**内容を次の XML に置き換えます。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<wallpaper xmlns:android="http://schemas.android.com/apk/res/android" />
```

このファイルのビルド アクション設定**AndroidResource**:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![ビルド アクション](creating-a-watchface-images/10-android-resource-vs.png "AndroidResource するアクションをビルドする設定")](creating-a-watchface-images/10-android-resource-vs.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![ビルド アクション](creating-a-watchface-images/10-android-resource-xs.png "AndroidResource するアクションをビルドする設定")](creating-a-watchface-images/10-android-resource-xs.png#lightbox)

-----

このリソース ファイルの定義、単純な`wallpaper`ウォッチの文字盤に使用される要素。

まだいない場合は、ダウンロード[preview.png](creating-a-watchface-images/preview.png)します。
インストールで**Resources/drawable/preview.png**します。 このファイルを追加することを確認する、`WatchFace`プロジェクト。 このプレビュー イメージが Wear デバイスでウォッチ face ピッカーでユーザーに表示されます。 ウォッチの文字盤独自のプレビュー イメージを作成するには、実行中は、ウォッチの文字盤のスクリーン ショットを実行できます。 (詳細 Wear デバイスからのスクリーン ショットの取得については、次を参照してください。[スクリーン ショットを作成](~/android/wear/deploy-test/debug-on-device.md#screenshots))。


## <a name="try-it"></a>これをお試しください。

ビルドして Wear デバイスにアプリを展開します。 以前と同様に表示される Wear アプリ画面が表示されます。 新しいウォッチの文字盤を有効にするのには、次の操作を行います。

1.  ウォッチ画面の背景が表示されるまで右にスワイプします。

2.  タッチして、2 秒間、画面の背景で任意の場所を保持します。

3.  さまざまな腕時計型インターフェイスを参照する権利を左からスワイプします。

4.  選択、 **Xamarin サンプル**ウォッチの文字盤 (右側に表示されます)。

    [![Watchface ピッカー](creating-a-watchface-images/11-watchface-picker.png "スワイプ Xamarin サンプル ウォッチの文字盤を検索するには")](creating-a-watchface-images/11-watchface-picker.png#lightbox)

5.  タップして、 **Xamarin サンプル**ウォッチの文字盤をオンにします。

これは、これまでに実装されたカスタム ウォッチ顔のサービスを使用するデバイス Wear の腕時計を変更します。

[![デジタル ウォッチの文字盤](creating-a-watchface-images/12-digital-watchface.png "Wear デバイスで実行されているカスタム デジタル ウォッチ")](creating-a-watchface-images/12-digital-watchface.png#lightbox)

これは、アプリの実装が最小限に抑えるためのための比較的洗練ウォッチの文字盤 (たとえば、ウォッチ顔の背景は含まれない、呼び出されない`Paint`外観を向上させるために、アンチ エイリアス メソッド)。
ただし、カスタム ウォッチの文字盤を作成するために必要である必要最低限の機能が実装します。

次のセクションでは、このウォッチの文字盤をより高度な実装にアップグレードされます。


## <a name="upgrading-the-watch-face"></a>ウォッチの文字盤をアップグレードします。

このチュートリアルの残りの部分で`MyWatchFaceService`はアナログ スタイルの腕時計の表示にアップグレードしより多くの機能をサポートするために拡張されています。 次の機能は、アップグレードされたウォッチの文字盤を作成する追加されます。

1.  アナログの 1 時間、分、および 2 つ目の手で時間を示します。

2.  可視性での変更に反応します。

3.  アンビエント モードと対話型モードの変更に応答します。

4.  基になる Wear デバイスのプロパティを読み取ります。

5.  時間のタイム ゾーンの変更が行われる時期を自動的に更新します。

以下のコード変更を実装する前にダウンロード[drawable.zip](https://github.com/xamarin/monodroid-samples/blob/master/wear/WatchFace/Resources/drawable.zip?raw=true)それを解凍し、解凍の .png ファイルの移動、**リソース/drawable** (上書き前**preview.png**). 新しい .png ファイルを追加、`WatchFace`プロジェクト。


### <a name="update-engine-features"></a>エンジンの機能を更新します。

次の手順はアップグレード**MyWatchFaceService.cs**アナログ腕時計を描画し、新しい機能をサポートしている実装にします。 内容を置き換える**MyWatchFaceService.cs**ウォッチ顔のコードのアナログのバージョンで[MyWatchFaceService.cs](https://github.com/xamarin/monodroid-samples/blob/master/wear/WatchFace/WatchFace/MyWatchFaceService.cs) (カットしてこのソースを既存貼り付ける**MyWatchFaceService.cs**)。

このバージョンの**MyWatchFaceService.cs**既存のメソッドにより多くのコードを追加し、多くの機能を追加する追加のオーバーライドされたメソッドが含まれています。 次のセクションでは、ソース コードのガイド付きツアーを提供します。

#### <a name="oncreate"></a>OnCreate

更新された**OnCreate**メソッドが以前と同様、ウォッチ面のスタイルを構成しますが、追加の手順が含まれています。

1.  背景画像の設定、 **xamarin_background**内にあるリソース**Resources/drawable-hdpi/xamarin_background.png**します。

2.  初期化します`Paint`手の 1 時間、分手、および 2 番目の手札を描画するためのオブジェクト。

3.  初期化します、`Paint`ウォッチの文字盤の端に 1 時間の目盛りを描画するためのオブジェクト。

4.  タイマーを作成するには、その呼び出し、 `Invalidate` (再描画) メソッドを 1 秒ごと、2 番目の手札を再描画されます。 このタイマーが必要なことに注意してください。 ため`OnTimeTick`呼び出し`Invalidate`毎分 1 回だけです。

この例には、1 つだけが含まれます**xamarin_background.png**イメージ。 ただし、カスタム ウォッチの文字盤をサポートする各画面密度の異なる背景イメージを作成したい場合があります。

#### <a name="ondraw"></a>OnDraw

更新された**OnDraw**メソッドは、次の手順を使用して、アナログ スタイル腕時計を描画します。

1.  現在管理されている現在の時刻を取得、`time`オブジェクト。

2.  描画サーフェイスおよびその中心の境界を決定します。

3.  背景が描画されるときに、デバイスに合わせて拡大縮小、背景を描画します。

4.  描画 12*タイマー刻み*(盤の時間に相当)、クロックの表面をします。

5.  角度、回転、および各ウォッチ手の形の長さを計算します。

6.  ウォッチ画面で、各手の形を描画します。 ウォッチがアンビエント モードの場合に 2 番目の手札が描画しないことに注意してください。


#### <a name="onpropertieschanged"></a>OnPropertiesChanged

このメソッドを呼び出して通知を`MyWatchFaceEngine`(低いビット アンビエント モードや保護バーンイン) Wear デバイスのプロパティの詳細について。 `MyWatchFaceEngine`、このメソッドは、低ビット アンビエント モード用にのみチェックします (下位ビットのアンビエント モードで画面ではビット数が少ないの各色)。

この方法の詳細については、Android を参照してください。 [onPropertiesChanged](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onPropertiesChanged%28android.os.Bundle%29) API のドキュメント。


#### <a name="onambientmodechanged"></a>OnAmbientModeChanged

Wear デバイスかアンビエント モードを終了すると、このメソッドが呼び出されます。 `MyWatchFaceEngine`実装、ウォッチの文字盤を無効にアンチエイリアシング アンビエント モードにあるときにします。

この方法の詳細については、Android を参照してください。 [onAmbientModeChanged](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onAmbientModeChanged%28boolean%29) API のドキュメント。


#### <a name="onvisibilitychanged"></a>OnVisibilityChanged

表示または非表示にウォッチなったときにこのメソッドが呼び出されます。 `MyWatchFaceEngine`、このメソッドのレジスタ/登録を解除、可視性の状態に応じて (後述) のタイム ゾーンの受信者。

この方法の詳細については、Android を参照してください。 [onVisibilityChanged](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onVisibilityChanged%28boolean%29) API のドキュメント。


### <a name="time-zone-feature"></a>タイム ゾーンの機能

新しい**MyWatchFaceService.cs**タイム ゾーンの変更 (タイム ゾーン間で出張中など) ときに、現在の時刻を更新する機能もあります。 末尾近く**MyWatchFaceService.cs**、ゾーンの変更時の`BroadcastReceiver`が定義されているタイム ゾーンの変更のインテント オブジェクトを処理します。

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

`RegisterTimezoneReceiver`と`UnregisterTimezoneReceiver`メソッドは、`OnVisibilityChanged`メソッド。
`UnregisterTimezoneReceiver` 呼び出されますをウォッチの文字盤の可視性の状態が変更されたときに非表示。 ウォッチの文字盤が再度表示される`RegisterTimezoneReceiver`が呼び出されます (を参照してください、`OnVisibilityChanged`メソッド)。

エンジン`RegisterTimezoneReceiver`メソッドは、このタイム ゾーンの受信者のハンドラーを宣言します`Receive`イベント。 このハンドラーを更新、`time`タイム ゾーンを超えたときに、新しい時刻を持つオブジェクト。

```csharp
timeZoneReceiver = new TimeZoneReceiver ();
timeZoneReceiver.Receive = (intent) => {
    time.Clear (intent.GetStringExtra ("time-zone"));
    time.SetToNow ();
};
```

インテントのフィルターが作成され、タイム ゾーンの受信側の登録します。

```csharp
IntentFilter filter = new IntentFilter(Intent.ActionTimezoneChanged);
Application.Context.RegisterReceiver (timeZoneReceiver, filter);
```

`UnregisterTimezoneReceiver`メソッドには、タイム ゾーンの受信側が登録解除します。

```csharp
Application.Context.UnregisterReceiver (timeZoneReceiver);
```

### <a name="run-the-improved-watch-face"></a>強化されたウォッチの文字盤を実行します。

ビルドして、もう一度 Wear デバイスにアプリをデプロイします。 前としてウォッチ face ピッカーからウォッチの文字盤を選択します。 ウォッチ ピッカーのプレビューは、左側に表示され、右側に表示する新しいウォッチの文字盤を。

[![アナログ ウォッチの文字盤](creating-a-watchface-images/13-analog-watchface.png "アナログ face ピッカーと、デバイス上の改善")](creating-a-watchface-images/13-analog-watchface.png#lightbox)

このスクリーン ショットでは、2 番目の手札を毎秒 1 回移動です。 Wear デバイスでこのコードを実行すると、ウォッチがアンビエント モードに入ったとき、2 番目の手札表示されなくなります。


## <a name="summary"></a>まとめ

このチュートリアルでは、カスタムの Android Wear 1.0 watchface が実装され、テストされました。 `CanvasWatchFaceService`と`CanvasWatchFaceService.Engine`クラスが導入された、およびエンジン クラスの重要なメソッドが実装され、単純なデジタル腕時計を作成します。 この実装は、アナログ腕時計を作成する多くの機能と更新され、可視性、アンビエント モード、およびデバイスのプロパティの相違で変更を処理する追加のメソッドが実装されました。 最後に、タイム ゾーンのブロードキャスト レシーバーは、ウォッチがタイム ゾーンを超えたときに時間を自動的に更新するように実装されていました。


## <a name="related-links"></a>関連リンク

- [腕時計型インターフェイスを作成します。](https://developer.android.com/training/wearables/watch-faces/index.html)
- [WatchFace サンプル](https://developer.xamarin.com/samples/monodroid/wear/WatchFace)
- [WatchFaceService.Engine](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html)
