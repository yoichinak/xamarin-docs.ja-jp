---
title: Android の磨耗1.0 のウォッチ式の作成
description: このガイドでは、Android 用のカスタムウォッチフェイス1.0 を実装する方法について説明します。 デジタルウォッチフェイスサービスを削除した後、アナログスタイルのウォッチ式を作成するためのより多くのコードを作成する手順について説明します。
ms.prod: xamarin
ms.assetid: 4D3F9A40-A820-458D-A12A-D784BB11F643
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/23/2018
ms.openlocfilehash: 38123e2b1ef20144606bcc77ad33af572aa3707a
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030381"
---
# <a name="creating-a-watch-face"></a>ウォッチの文字盤を作成する

_このガイドでは、Android 用のカスタムウォッチフェイス1.0 を実装する方法について説明します。デジタルウォッチフェイスサービスを削除した後、アナログスタイルのウォッチ式を作成するためのより多くのコードを作成する手順について説明します。_

## <a name="overview"></a>概要

このチュートリアルでは、カスタムの Android 磨耗1.0 ウォッチ式の作成の要点を示すために、基本的な watch face service が作成されています。
最初の watch face サービスには、現在の時間を時間と分で表示するシンプルなデジタルウォッチが表示されます。

[![デジタルウォッチの顔](creating-a-watchface-images/01-initial-face.png "最初のデジタルウォッチ顔のスクリーンショットの例")](creating-a-watchface-images/01-initial-face.png#lightbox)

このデジタルウォッチフェイスを開発してテストした後、次の3人のより洗練されたアナログウォッチ式にアップグレードするためのコードを追加します。

[![アナログウォッチフェイス](creating-a-watchface-images/02-example-watchface.png "最後のアナログウォッチ式のスクリーンショットの例")](creating-a-watchface-images/02-example-watchface.png#lightbox)

Watch face services は、摩耗1.0 アプリの一部としてバンドルされ、インストールされます。 次の例では、`MainActivity` は、アプリケーションの一部として、watch face サービスをパッケージ化してスマートウォッチにデプロイできるように、摩耗1.0 アプリテンプレートのコードよりも多くのものを含んでいません。 実際には、このアプリは、デバッグとテストのために watch のサービスを磨耗1.0 デバイス (エミュレーター) に読み込むための手段として純粋に機能します。

## <a name="requirements"></a>［要件］

Watch face service を実装するには、次のものが必要です。

- Android 5.0 (API レベル 21) 以上 (磨耗デバイスまたはエミュレーターの場合)。

- Xamarin [android の磨耗サポートライブラリ](https://www.nuget.org/packages/Xamarin.Android.Wear)を xamarin android プロジェクトに追加する必要があります。

Android 5.0 は watch face service を実装するための最小 API レベルですが、Android 5.1 以降をお勧めします。 Android 5.1 (API 22) 以降を実行している android の磨耗デバイスでは、デバイスが低電力の*アンビエント*モードであるときに画面に表示される内容を、磨耗アプリで制御できます。 デバイスが低電力の*アンビエント*モードのままになっている場合は、*対話*モードになります。 これらのモードの詳細については、「[アプリの表示の維持](https://developer.android.com/training/wearables/apps/always-on.html)」を参照してください。

## <a name="start-an-app-project"></a>アプリプロジェクトを開始する

**WatchFace**という名前の新しい Android の磨耗1.0 プロジェクトを作成します (新しい Xamarin Android プロジェクトの作成の詳細については、「 [Hello, android](~/android/get-started/hello-android/hello-android-quickstart.md)」を参照してください)。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![[新しいプロジェクト] ダイアログ](creating-a-watchface-images/03-wear-project-vs-sml.png "[新しいプロジェクト] ダイアログボックスで [磨耗アプリ] を選択します。")](creating-a-watchface-images/03-wear-project-vs.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![[新しいプロジェクト] ダイアログ](creating-a-watchface-images/03-wear-project-xs-sml.png "[新しいプロジェクト] ダイアログボックスで [磨耗アプリ] を選択します。")](creating-a-watchface-images/03-wear-project-xs.png#lightbox)

-----

パッケージ名を `com.xamarin.watchface` に設定します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![パッケージ名の設定](creating-a-watchface-images/04-package-name-vs.png "パッケージ名を watchface に設定します。")](creating-a-watchface-images/04-package-name-vs.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![パッケージ名の設定](creating-a-watchface-images/04-package-name-xs.png "パッケージ名を watchface に設定します。")](creating-a-watchface-images/04-package-name-xs.png#lightbox)

-----

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

さらに、下にスクロールして、**インターネット**と**WAKE_LOCK**のアクセス許可を有効にします。

[![必要なアクセス許可](creating-a-watchface-images/05-required-permissions-vs.png "インターネットと WAKE_LOCK のアクセス許可を有効にする")](creating-a-watchface-images/05-required-permissions-vs.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Android の最小バージョンを**android 5.1 (API レベル 22)** に設定します。
また、**インターネット**と**WakeLock**のアクセス許可を有効にします。

[![必要なアクセス許可](creating-a-watchface-images/05-required-permissions-xs.png "インターネットと WakeLock のアクセス許可を有効にする")](creating-a-watchface-images/05-required-permissions-xs.png#lightbox)

-----

次に、[プレビュー &ndash; ダウンロードします。](creating-a-watchface-images/preview.png)このファイルは、このチュートリアルの後半で、 **drawables**実行可能フォルダーに追加されます。

## <a name="add-the-xamarinandroid-wear-package"></a>Xamarin. Android の磨耗パッケージを追加する

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

NuGet パッケージマネージャーを起動します (Visual Studio で、**ソリューションエクスプローラー**の **[参照]** を右クリックし、 **[nuget パッケージの管理]** を選択します)。プロジェクトを最新の安定したバージョンの**Xamarin. Android. Android**に更新します。

[![NuGet パッケージマネージャーの追加](creating-a-watchface-images/06-add-wear-pkg-vs-sml.png "Xamarin. Android パッケージを追加する")](creating-a-watchface-images/06-add-wear-pkg-vs.png#lightbox)

次に、 **v13**がインストールされている場合は、アンインストールします。

[![NuGet パッケージマネージャーの削除](creating-a-watchface-images/07-uninstall-v13-sml.png "V13 を削除します。")](creating-a-watchface-images/07-uninstall-v13.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

NuGet パッケージマネージャーを起動します (Visual Studio for Mac で、[**ソリューション] ウィンドウ**の **[パッケージ]** を右クリックし、 **[パッケージの追加...]** を選択します)。プロジェクトを最新の安定したバージョンの**Xamarin. Android. Android**に更新します。

[![NuGet パッケージマネージャーの追加](creating-a-watchface-images/06-add-wear-pkg-xs-sml.png "Xamarin. Android パッケージを追加する")](creating-a-watchface-images/06-add-wear-pkg-xs.png#lightbox)

-----

アプリをビルドして、磨耗デバイスまたはエミュレーターで実行します (詳細については、[はじめに](~/android/wear/get-started/index.md)ガイドを参照してください)。 次のアプリ画面が磨耗デバイスに表示されます。

[![アプリのスクリーンショット](creating-a-watchface-images/08-app-screen.png "磨耗デバイスでのアプリ画面")](creating-a-watchface-images/08-app-screen.png#lightbox)

この時点では、基本の摩耗アプリにはウォッチフェイス機能がありません。これは、まだ watch のサービス実装が提供されていないためです。 このサービスは次に追加されます。

## <a name="canvaswatchfaceservice"></a>CanvasWatchFaceService

Android 磨耗は、`CanvasWatchFaceService` クラスを使用してウォッチフェイスを実装します。 `CanvasWatchFaceService` は `WatchFaceService` から派生し、次の図に示すように `WallpaperService` から派生します。

[![継承ダイアグラム](creating-a-watchface-images/09-inheritance-diagram-sml.png "CanvasWatchFaceService 継承ダイアグラム")](creating-a-watchface-images/09-inheritance-diagram.png#lightbox)

`CanvasWatchFaceService` には、入れ子になった `CanvasWatchFaceService.Engine` が含まれます。これは、ウォッチの表面を描画する実際の作業を行う `CanvasWatchFaceService.Engine` オブジェクトをインスタンス化します。 `CanvasWatchFaceService.Engine` は、上の図に示すように `WallpaperService.Engine` から派生します。

この図に示されていないのは、次に示すように、この `Canvas` が `OnDraw` メソッドを介して渡される &ndash;、ウォッチ式の描画に使用 `CanvasWatchFaceService` `Canvas` です。

以下のセクションでは、次の手順に従ってカスタムウォッチフェイスサービスを作成します。

1. `CanvasWatchFaceService`から派生した `MyWatchFaceService` というクラスを定義します。

2. `MyWatchFaceService`内で、`CanvasWatchFaceService.Engine`から派生した `MyWatchFaceEngine` という名前の入れ子になったクラスを作成します。

3. `MyWatchFaceService`で、`MyWatchFaceEngine` をインスタンス化して返す `CreateEngine` メソッドを実装します。

4. `MyWatchFaceEngine`では、`OnCreate` メソッドを実装して、ウォッチフェイススタイルを作成し、その他の初期化タスクを実行します。

5. `MyWatchFaceEngine`の `OnDraw` メソッドを実装します。 このメソッドは、ウォッチフェイスを再描画する必要がある (つまり、*無効化*される) たびに呼び出されます。 `OnDraw` は、時間、分、2番目の針などのウォッチ式を描画 (および再描画) するメソッドです。

6. `MyWatchFaceEngine`の `OnTimeTick` メソッドを実装します。
    `OnTimeTick` は、(アンビエントモードと対話モードの両方で) 1 分間に1回以上呼び出されるか、日付/時刻が変更されたときに呼び出されます。

`CanvasWatchFaceService`の詳細については、Android [CanvasWatchFaceService](https://developer.android.com/reference/android/support/wearable/watchface/CanvasWatchFaceService.html) API のドキュメントを参照してください。
同様に、CanvasWatchFaceService は、ウォッチ式の実際の実装について説明し[ます](https://developer.android.com/reference/android/support/wearable/watchface/CanvasWatchFaceService.Engine.html)。

### <a name="add-the-canvaswatchfaceservice"></a>CanvasWatchFaceService を追加する

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

**MyWatchFaceService.cs**という名前の新しいファイルを追加します (Visual Studio の場合は、**ソリューションエクスプローラー**で**WatchFace**を右クリックし、 **[新しい項目の追加 >]** 、 **[クラス]** の選択 の順にクリックします)。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

**MyWatchFaceService.cs**という名前の新しいファイルを追加します (Visual Studio for Mac で**WatchFace**プロジェクトを右クリックし、 **[新しいファイルの > 追加]** をクリックして **[空のクラス]** を選択します)。

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

`MyWatchFaceService` (`CanvasWatchFaceService` から派生) は、ウォッチ式の "メインプログラム" です。 `MyWatchFaceService` は、`MyWatchFaceEngine` オブジェクトをインスタンス化して返すメソッド (`OnCreateEngine`) を1つだけ実装します (`MyWatchFaceEngine` は `CanvasWatchFaceService.Engine` から派生します)。 インスタンス化された `MyWatchFaceEngine` オブジェクトは `WallpaperService.Engine` として返される必要があります。 カプセル化 `MyWatchFaceService` オブジェクトがコンストラクターに渡されます。

`MyWatchFaceEngine` は、ウォッチフェイスを描画するコードが含まれている &ndash; 実際のウォッチフェイスの実装です。 また、画面の変更 (アンビエント/interactive モード、画面の電源オフなど) などのシステムイベントも処理します。

### <a name="implement-the-engine-oncreate-method"></a>Engine OnCreate メソッドを実装する

`OnCreate` メソッドは、ウォッチ式を初期化します。 `MyWatchFaceEngine`に次のフィールドを追加します。

```csharp
Paint hoursPaint;
```

この `Paint` オブジェクトは、ウォッチ式の現在の時刻を描画するために使用されます。 次に、次のメソッドを `MyWatchFaceEngine` に追加します。

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

`OnCreate` は `MyWatchFaceEngine` が開始された直後に呼び出されます。 `WatchFaceStyle` (磨耗デバイスがユーザーとどのように対話するかを制御する) を設定し、時刻を表示するために使用される `Paint` オブジェクトをインスタンス化します。

`SetWatchFaceStyle` を呼び出すと、次のことが行われます。

1. *ピークモード*を `PeekModeShort` に設定します。これにより、通知がディスプレイに小さな "ピーク" カードとして表示されます。

2. 背景の可視性を `Interruptive` に設定します。これにより、伴わ通知を表す場合、ピークカードの背景が一時的に表示されます。

3. 既定のシステム UI 時間をウォッチ画面に描画しないようにします。これにより、カスタムウォッチフェイスに時間を表示できるようになります。

これらおよびその他のウォッチフェイスのスタイルオプションの詳細については、Android [WatchFaceStyle](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceStyle.Builder.html) API のドキュメントを参照してください。

`SetWatchFaceStyle` が完了すると、`OnCreate` によって `Paint` オブジェクト (`hoursPaint`) がインスタンス化され、その色が白に、テキストサイズが48ピクセルに設定されます (高さはピクセル単位で指定[する必要が](https://developer.android.com/reference/android/graphics/Paint.html#setTextSize%28float%29)あります)。

### <a name="implement-the-engine-ondraw-method"></a>エンジン OnDraw メソッドを実装する

`OnDraw` メソッドは、数字や時計の顔などのウォッチ式を実際に描画するメソッド &ndash;、最も重要な `CanvasWatchFaceService.Engine` メソッドです。
次の例では、ウォッチの表面に時間の文字列を描画します。
次のメソッドを `MyWatchFaceEngine` に追加します。

```csharp
public override void OnDraw (Canvas canvas, Rect frame)
{
    var str = DateTime.Now.ToString ("h:mm tt");
    canvas.DrawText (str,
        (float)(frame.Left + 70),
        (float)(frame.Top  + 80), hoursPaint);
}
```

Android が `OnDraw` を呼び出すと、`Canvas` インスタンスと、その面を描画できる境界が渡されます。 上のコード例では、`DateTime` を使用して、現在の時刻を時間と分 (12 時間形式) で計算しています。 結果として得られる時間文字列は、`Canvas.DrawText` メソッドを使用してキャンバスに描画されます。 この文字列は、左端から70ピクセル、上端から80ピクセルが表示されます。

`OnDraw` 方法の詳細については、Android の[onDraw](https://developer.android.com/reference/android/support/wearable/watchface/CanvasWatchFaceService.Engine#ondraw) API のドキュメントを参照してください。

### <a name="implement-the-engine-ontimetick-method"></a>Engine OnTimeTick メソッドを実装する

Android では、`OnTimeTick` メソッドを定期的に呼び出して、ウォッチ式によって表示される時間を更新します。 1分に1回以上 (アンビエントモードと対話モードの両方)、または日付/時刻またはタイムゾーンが変更されたときに呼び出されます。 次のメソッドを `MyWatchFaceEngine` に追加します。

```csharp
public override void OnTimeTick()
{
    Invalidate();
}
```

この `OnTimeTick` の実装では、単に `Invalidate` を呼び出します。 `Invalidate` メソッドは、ウォッチの表面を再描画するための `OnDraw` をスケジュールします。

`OnTimeTick` 方法の詳細については、Android [Ontimetick](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onTimeTick()) API のドキュメントを参照してください。

## <a name="register-the-canvaswatchfaceservice"></a>CanvasWatchFaceService を登録する

`MyWatchFaceService` は、関連する磨耗アプリの**Androidmanifest .xml**に登録する必要があります。 これを行うには、次の XML を `<application>` セクションに追加します。

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

この XML は、次のことを行います。

1. `android.permission.BIND_WALLPAPER` のアクセス許可を設定します。 このアクセス許可は、デバイス上のシステムの壁紙を変更するアクセス許可を watch face サービスに与えます。 このアクセス許可は、外側の `<application>` セクションではなく、`<service>` セクションで設定する必要があることに注意してください。

2. `watch_face` リソースを定義します。 このリソースは、`wallpaper` リソースを宣言する短い XML ファイルです (このファイルは次のセクションで作成されます)。

3. [ウォッチピッカーの選択] 画面に表示される `preview` と呼ばれる描画可能なイメージを宣言します。

4. `MyWatchFaceService` にウォッチ式が表示されることを Android に知らせるための `intent-filter` が含まれています。

これで、基本的な `WatchFace` の例のコードを完成させることができます。 次の手順では、必要なリソースを追加します。

## <a name="add-resource-files"></a>リソースファイルの追加

Watch サービスを実行する前に、 **watch_face**リソースとプレビューイメージを追加する必要があります。 まず、 **Resources/xml/watch_face**で新しい xml ファイルを作成し、その内容を次の xml に置き換えます。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<wallpaper xmlns:android="http://schemas.android.com/apk/res/android" />
```

このファイルのビルドアクションを**Androidresource**に設定します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![ビルドアクション](creating-a-watchface-images/10-android-resource-vs.png "ビルドアクションを AndroidResource に設定する")](creating-a-watchface-images/10-android-resource-vs.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![ビルドアクション](creating-a-watchface-images/10-android-resource-xs.png "ビルドアクションを AndroidResource に設定する")](creating-a-watchface-images/10-android-resource-xs.png#lightbox)

-----

このリソースファイルは、ウォッチフェイスに使用される単純な `wallpaper` 要素を定義します。

まだ行っていない場合は[、ダウンロードしてください。](creating-a-watchface-images/preview.png)
**Resources/の描画/プレビュー**ファイルにインストールします。 必ずこのファイルを `WatchFace` プロジェクトに追加してください。 このプレビューイメージは、磨耗デバイスの ウォッチ盤ピッカーのユーザーに表示されます。 独自のウォッチ顔のプレビューイメージを作成するには、実行中にウォッチフェイスのスクリーンショットを撮影します。 (摩耗デバイスからスクリーンショットを取得する方法の詳細については、「[スクリーンショットの撮影](~/android/wear/deploy-test/debug-on-device.md#screenshots)」を参照してください)。

## <a name="try-it"></a>お試しください!

アプリをビルドして、磨耗デバイスにデプロイします。 [磨耗] アプリ画面が前と同じように表示されます。 新しいウォッチ式を有効にするには、次の手順を実行します。

1. ウォッチ画面の背景が表示されるまで右にスワイプします。

2. 画面の背景にある任意の場所に2秒間、タッチして保持します。

3. さまざまなウォッチフェイスを参照するには、左から右にスワイプします。

4. **Xamarin サンプル**ウォッチフェイス (右側に表示) を選択します。

    [![Watchface ピッカー](creating-a-watchface-images/11-watchface-picker.png "スワイプして Xamarin サンプルウォッチフェイスを見つける")](creating-a-watchface-images/11-watchface-picker.png#lightbox)

5. **Xamarin サンプル**ウォッチフェイスをタップして選択します。

これにより、これまでに実装されたカスタムウォッチフェイスサービスを使用するように、磨耗デバイスのウォッチ式が変更されます。

[![デジタルウォッチの顔](creating-a-watchface-images/12-digital-watchface.png "摩耗デバイスで実行されているカスタムデジタルウォッチ")](creating-a-watchface-images/12-digital-watchface.png#lightbox)

これは、アプリの実装が非常に少ないため (たとえば、ウォッチフェイスの背景が含まれておらず、外観を向上させるために `Paint` アンチエイリアスメソッドを呼び出さない)、比較的見やすい顔です。
ただし、カスタムウォッチフェイスを作成するために必要なベアボーン機能が実装されています。

次のセクションでは、このウォッチフェイスをより高度な実装にアップグレードします。

## <a name="upgrading-the-watch-face"></a>ウォッチ式のアップグレード

このチュートリアルの残りの部分では、`MyWatchFaceService` をアップグレードしてアナログスタイルのウォッチ式を表示し、さらに多くの機能をサポートするように拡張しています。 アップグレードしたウォッチ式を作成するには、次の機能が追加されます。

1. アナログ時間、分、および秒の針を使用して時刻を示します。

2. 可視性の変化に反応します。

3. アンビエントモードと対話モードの間の変更に応答します。

4. 基になる磨耗デバイスのプロパティを読み取ります。

5. タイムゾーンの変更が行われる時刻を自動的に更新します。

以下のコード変更を実装する前に、この .zip をダウンロードし、解凍して、[圧縮](https://github.com/xamarin/monodroid-samples/blob/master/wear/WatchFace/Resources/drawable.zip?raw=true)された .png ファイルを**リソース/ド**コードに移動します (前の**preview**を上書きします)。 新しい .png ファイルを `WatchFace` プロジェクトに追加します。

### <a name="update-engine-features"></a>エンジン機能の更新

次の手順では、 **MyWatchFaceService.cs**を、アナログウォッチ式を描画し、新機能をサポートする実装にアップグレードします。 **MyWatchFaceService.cs**の内容を[MyWatchFaceService.cs](https://github.com/xamarin/monodroid-samples/blob/master/wear/WatchFace/WatchFace/MyWatchFaceService.cs)のウォッチ盤コードのアナログバージョンに置き換えます (このソースを切り取って、既存の**MyWatchFaceService.cs**に貼り付けます)。

このバージョンの**MyWatchFaceService.cs**では、既存のメソッドにコードを追加し、追加の機能を追加するためのオーバーライドされたメソッドが追加されています。 以下のセクションでは、ソースコードのガイドツアーについて説明します。

#### <a name="oncreate"></a>OnCreate

更新された**OnCreate**メソッドは、前と同じようにウォッチフェイススタイルを構成しますが、いくつかの追加の手順が含まれています。

1. バックグラウンドイメージを、 **Resources/drawable-hdpi/xamarin_background**に存在する**xamarin_background**リソースに設定します。

2. 時間、分、および秒の針を描画するために `Paint` オブジェクトを初期化します。

3. `Paint` オブジェクトを初期化して、ウォッチフェイスの端を中心とした時間の目盛りを描画します。

4. 2番目のハンドが毎秒再描画されるように `Invalidate` (再描画) メソッドを呼び出すタイマーを作成します。 このタイマーが必要になるのは、`OnTimeTick` が1分ごとに1回だけ `Invalidate` を呼び出すことができるためです。

この例には、 **xamarin_background**イメージが1つだけ含まれています。ただし、カスタムウォッチの面でサポートされる画面の密度ごとに異なる背景画像を作成することもできます。

#### <a name="ondraw"></a>OnDraw

更新された**OnDraw**メソッドは、次の手順に従って、アナログスタイルのウォッチ式を描画します。

1. 現在の時刻を取得します。これは `time` オブジェクトで維持されます。

2. 描画サーフェイスとその中心の境界を決定します。

3. 背景を描画します。背景が描画されるときに、デバイスに合わせてスケーリングされます。

4. 時計の表面 (時計の表面の時間に対応) の周りに 12*ティック*を描画します。

5. 各ウォッチハンドの角度、回転、および長さを計算します。

6. 各ハンドをウォッチ画面に描画します。 ウォッチがアンビエントモードの場合、2番目のハンドは描画されないことに注意してください。

#### <a name="onpropertieschanged"></a>OnPropertiesChanged

このメソッドは、磨耗デバイス (低ビットのアンビエントモードや書き込み保護など) のプロパティについて `MyWatchFaceEngine` に通知するために呼び出されます。 `MyWatchFaceEngine`では、このメソッドは低いビットアンビエントモードのみをチェックします (低ビットのアンビエントモードでは、画面は各色に対してより少ないビットをサポートします)。

このメソッドの詳細については、Android [Onpropertieschanged](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onPropertiesChanged%28android.os.Bundle%29) API のドキュメントを参照してください。

#### <a name="onambientmodechanged"></a>OnAmbientModeChanged

このメソッドは、摩耗デバイスがアンビエントモードに入ったり終了したりしたときに呼び出されます。 `MyWatchFaceEngine` の実装では、モニターの顔がアンビエントモードのときにアンチエイリアシングを無効にします。

このメソッドの詳細については、Android [onAmbientModeChanged](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onAmbientModeChanged%28boolean%29) API のドキュメントを参照してください。

#### <a name="onvisibilitychanged"></a>OnVisibilityChanged

このメソッドは、ウォッチが表示または非表示になるたびに呼び出されます。 `MyWatchFaceEngine`では、このメソッドは、表示状態に応じて、タイムゾーン受信者 (以下で説明) の登録/登録解除を行います。

このメソッドの詳細については、Android [onVisibilityChanged](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onVisibilityChanged%28boolean%29) API のドキュメントを参照してください。

### <a name="time-zone-feature"></a>タイムゾーン機能

新しい**MyWatchFaceService.cs**には、タイムゾーンが変更されたとき (タイムゾーン間での移動中など) に現在の時刻を更新する機能も含まれています。 **MyWatchFaceService.cs**の末尾付近では、タイムゾーンの変更されたインテントオブジェクトを処理するタイムゾーンの変更 `BroadcastReceiver` が定義されています。

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

`RegisterTimezoneReceiver` メソッドと `UnregisterTimezoneReceiver` メソッドは `OnVisibilityChanged` メソッドによって呼び出されます。
`UnregisterTimezoneReceiver` は、ウォッチフェイスの表示状態が非表示に変更されると呼び出されます。 ウォッチフェイスが再び表示されると、`RegisterTimezoneReceiver` が呼び出されます (`OnVisibilityChanged` メソッドを参照してください)。

エンジン `RegisterTimezoneReceiver` メソッドは、このタイムゾーン受信者の `Receive` イベントのハンドラーを宣言します。このハンドラーは、タイムゾーンが交差するたびに新しい時間で `time` オブジェクトを更新します。

```csharp
timeZoneReceiver = new TimeZoneReceiver ();
timeZoneReceiver.Receive = (intent) => {
    time.Clear (intent.GetStringExtra ("time-zone"));
    time.SetToNow ();
};
```

インテントフィルターが作成され、タイムゾーン受信者に登録されます。

```csharp
IntentFilter filter = new IntentFilter(Intent.ActionTimezoneChanged);
Application.Context.RegisterReceiver (timeZoneReceiver, filter);
```

`UnregisterTimezoneReceiver` メソッドは、タイムゾーン受信者の登録を解除します。

```csharp
Application.Context.UnregisterReceiver (timeZoneReceiver);
```

### <a name="run-the-improved-watch-face"></a>強化されたウォッチフェイスを実行する

アプリをビルドして、もう一度アプリを磨耗デバイスに配置します。 前と同じように、[ウォッチ盤] ピッカーからウォッチフェイスを選択します。 ウォッチピッカーのプレビューが左側に表示され、新しいウォッチフェイスが右側に表示されます。

[![アナログウォッチフェイス](creating-a-watchface-images/13-analog-watchface.png "ピッカーおよびデバイスでのアナログフェイスの向上")](creating-a-watchface-images/13-analog-watchface.png#lightbox)

このスクリーンショットでは、2番目のハンドが1秒間に1回移動しています。 このコードを磨耗デバイスで実行すると、ウォッチがアンビエントモードになると2番目のハンドが消えます。

## <a name="summary"></a>まとめ

このチュートリアルでは、カスタム Android 磨耗 1.0 watchface を実装し、テストしました。 `CanvasWatchFaceService` クラスと `CanvasWatchFaceService.Engine` クラスが導入され、単純なデジタルウォッチ面を作成するためにエンジンクラスの重要なメソッドが実装されました。 この実装は、アナログウォッチ式を作成するためのより多くの機能で更新されました。また、デバイスプロパティの可視性、アンビエントモード、および違いの変化を処理するために、追加のメソッドが実装されています。 最後に、タイムゾーンブロードキャストレシーバーが実装されました。これにより、ウォッチがタイムゾーンを超えた時間が自動的に更新されます。

## <a name="related-links"></a>関連リンク

- [ウォッチフェイスの作成](https://developer.android.com/training/wearables/watch-faces/index.html)
- [WatchFace サンプル](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-watchface)
- [WatchFaceService](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html)
