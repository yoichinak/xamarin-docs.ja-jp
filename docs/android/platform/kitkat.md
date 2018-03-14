---
title: "KitKat 機能"
description: "Android 4.4 (KitKat) は、ユーザーおよび開発者向けの機能の cornucopia で読み込まれたものです。 このガイドでは、これらの機能のいくつかが強調表示され、コード例と KitKat を最大限に活用するための実装の詳細を提供します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: D3FDEA1C-F076-406F-BCC3-2A55D2C6ADEE
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 8fbb3f73fdc09f953ad5f7134020c1555d000d28
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2018
---
# <a name="kitkat-features"></a>KitKat 機能

_Android 4.4 (KitKat) は、ユーザーおよび開発者向けの機能の cornucopia で読み込まれたものです。このガイドでは、これらの機能のいくつかが強調表示され、コード例と KitKat を最大限に活用するための実装の詳細を提供します。_

## <a name="overview"></a>概要

Android 4.4 (API レベル 19) とも呼ばれる"KitKat"は、遅延 2013 でリリースされました。 KitKat には、さまざまな新機能となどの機能強化が提供しています。

-  [ユーザー エクスペリエンス](#user_experience)&ndash;遷移 framework、半透明状態とナビゲーション バー、および全画面表示イマーシブ モードでの簡単なアニメーションのユーザー エクスペリエンスを向上させる作成に役立ちます。

-  [ユーザー コンテンツ](#user_content)&ndash;記憶域アクセス フレームワークとユーザー ファイルの管理が簡素化されます。 画像の印刷、web サイト、およびその他のコンテンツが強化された印刷 Api に容易になります。

-  [ハードウェア](#hardware) &ndash; NFC カード エミュレーションのホスト ベースの NFC カードにすべてのアプリを有効にする; 低電力センサーとの実行、`SensorManager`です。

-  [開発者ツール](#developer_tools)&ndash;スクリーン キャスト アプリケーション、Android Debug Bridge クライアントは、使用可能な操作において、Android SDK の一部として。


このガイドでは、Xamarin.Android 開発者向けを KitKat の概要をできるだけでなく、KitKat 既存 Xamarin.Android アプリケーションへの移行に関するガイダンスを提供します。

## <a name="requirements"></a>必要条件

Xamarin.Android を KitKat を使用してアプリケーションを開発する必要があります*Xamarin.Android 4.11.0*または高い値と Android 4.4 (API レベル 19) で次のスクリーン ショットに示すように、Android SDK Manager を使用してをインストールします。

[![Android SDK Manager で Android 4.4 を選択します。](kitkat-images/api19.png)](kitkat-images/api19.png#lightbox)

<a name="Migrating_Your_App_to_KitKat" />

## <a name="migrating-your-app-to-kitkat"></a>アプリは、KitKat への移行

このセクションでは、Android 4.4 に既存のアプリケーションを移行するには、いくつか最初応答項目を示します。

### <a name="check-system-version"></a>システム バージョンを確認します。

アプリケーションは、Android の古いバージョンと互換性がある必要がある場合、は、次のコード例に示すように、システムのバージョン チェックでは、すべて KitKat 固有のコードをラップすることを確認します。

```csharp
if (Build.VERSION.SdkInt >= BuildVersionCodes.Kitkat) {
    //KitKat only code here
}
```

### <a name="alarm-batching"></a>アラームがバッチ処理

Android では、バック グラウンドでアプリを指定された時間にスリープ解除するのにアラーム services を使用します。 KitKat はこの手順をさらに電源を保持するためにアラームをバッチ処理しています。 これは、代わりにアプリごとの正確な時にウェイク アップ、KitKat 優先的に同じ期間にスリープ解除し、同時にスリープ解除して登録されているいくつかのアプリケーションをグループ化を意味します。
Android アプリを指定した時間間隔中にスリープ状態を確認する呼び出し`SetWindow`上、 [ `AlarmManager`](https://developer.xamarin.com/api/type/Android.App.AlarmManager/)と、最小、最大時間 (ミリ秒単位) が経過したら、アプリはウェイク状態が、および実行する操作を渡して、でウェイク アップします。
次のコードでは、30 分とウィンドウが設定されている時間から 1 時間の間で、起動状態にする必要があるアプリケーションの例を示します。

```csharp
AlarmManager alarmManager = (AlarmManager)GetSystemService(AlarmService);
alarmManager.SetWindow (AlarmType.Rtc, AlarmManager.IntervalHalfHour, AlarmManager.IntervalHour, pendingIntent);
```

アプリの正確な時間でウェイク アップを続行するには使用`SetExact`アプリが起動状態にする必要があります、正確な時間および実行する操作を渡して。

```csharp
alarmManager.SetExact (AlarmType.Rtc, AlarmManager.IntervalDay, pendingIntent);
```

不要になった KitKat では正確な繰り返しアラームを設定することができます。 使用するアプリケーション[ `SetRepeating` ](https://developer.xamarin.com/api/member/Android.App.AlarmManager.SetRepeating/p/Android.App.AlarmType/System.Int64/System.Int64/Android.App.PendingIntent/)各アラームを手動でトリガーを今すぐ必要な作業を正確なアラームを必要とします。

### <a name="external-storage"></a>外部ストレージ

外部記憶域は、2 種類のアプリケーション、および複数のアプリケーションで共有データに固有の記憶域に分かれていますようになりました。 読み取りと書き込み外部ストレージにアプリの特定の場所は、特殊なアクセス許可を不要です。 ここでの共有記憶域上のデータと対話する必要があります、`READ_EXTERNAL_STORAGE`または`WRITE_EXTERNAL_STORAGE`権限です。 そのため 2 つの型に分類できます。

-  メソッドを呼び出すことにより、ファイルまたはディレクトリのパスを取得するかどうかは`Context`- たとえば、 [ `GetExternalFilesDir` ](https://developer.xamarin.com/api/member/Android.Content.Context.GetExternalFilesDir/p/System.String/)または [`GetExternalCacheDirs`](https://developer.xamarin.com/api/member/Android.Content.Context.GetExternalCacheDirs%28%29/)
   - アプリには、追加のアクセス許可は不要です。

-  プロパティへのアクセスまたはでメソッドを呼び出すことによってファイルまたはディレクトリのパスが示されている場合`Environment`など[ `GetExternalStorageDirectory` ](https://developer.xamarin.com/api/property/Android.OS.Environment.ExternalStorageDirectory/)または[ `GetExternalStoragePublicDirectory` ](https://developer.xamarin.com/api/member/Android.OS.Environment.GetExternalStoragePublicDirectory/p/System.String/) 、アプリに必要で、`READ_EXTERNAL_STORAGE`または`WRITE_EXTERNAL_STORAGE`権限です。

> [!NOTE]
> `WRITE_EXTERNAL_STORAGE` 意味、`READ_EXTERNAL_STORAGE`のみ必要になった権限を 1 つを設定するためのアクセス許可。

### <a name="sms-consolidation"></a>SMS の統合

ユーザーが選択されている 1 つの既定のアプリケーション内のすべての SMS コンテンツを集計することによって、ユーザーのメッセージング KitKat が簡略化します。 開発者は、アプリを既定のメッセージング アプリケーションとして選択できるようにし、コードと有効期間にアプリケーションが選択されていない場合は、適切に動作します。 KitKat に SMS のアプリの移行の詳細についてを参照してください、 [Your SMS アプリの準備 KitKat](http://android-developers.blogspot.com/2013/10/getting-your-sms-apps-ready-for-kitkat.html) Google からのガイドです。

### <a name="webview-apps"></a>WebView アプリ

[WebView](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) KitKat」で、作り直しを取得します。 最も大きな変化がへのコンテンツを読み込むためのセキュリティを追加、`WebView`です。 古い API バージョンを対象とするほとんどのアプリケーションは、期待どおりに動作する必要があります、テストを使用するアプリケーション、`WebView`クラスを強くお勧めします。 影響を受ける WebView Api の詳細については、Android を参照してください[Android 4.4 で WebView への移行](http://developer.android.com/guide/webapps/migrating.html)ドキュメント。

<a name="user_experience" />

## <a name="user-experience"></a>ユーザー体験

KitKat は、プロパティ アニメーションおよびテーマの UI オプションの半透明を処理するための新しい遷移 framework などのユーザー エクスペリエンスを強化するためにいくつかの新しい Api が付属します。 以降では、これらの変更について説明します。

### <a name="transition-framework"></a>遷移フレームワーク

遷移フレームワークは、簡単に実装するアニメーションを使用します。 KitKat では、1 つの行のコードでは、単純なプロパティのアニメーションを実行したりを使用して遷移をカスタマイズできます。*シーン*です。

#### <a name="simple-property-animation"></a>単純なプロパティのアニメーション

新しい Android 遷移ライブラリには、プロパティのアニメーションの背後にあるコードが簡略化されます。 フレームワークでは、最小限のコードでの単純なアニメーションを実行することができます。 たとえば、次のコード サンプルでは[ `TransitionManager.BeginDelayedTransition` ](https://developer.xamarin.com/api/member/Android.Transitions.TransitionManager.BeginDelayedTransition/p/Android.Views.ViewGroup/Android.Transitions.Transition/)表示と非表示をアニメーション化する、 `TextView`:

```csharp
using Android.Transitions;

public class MainActivity : Activity
{
    LinearLayout linear;
    Button button;
    TextView text;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);
        SetContentView (Resource.Layout.Main);

        linear = FindViewById<LinearLayout> (Resource.Id.linearLayout);
        button = FindViewById<Button> (Resource.Id.button);
        text = FindViewById<TextView> (Resource.Id.textView);

        button.Click += (o, e) => {

            TransitionManager.BeginDelayedTransition (linear);

            if(text.Visibility != ViewStates.Visible)
            {
                text.Visibility = ViewStates.Visible;
            }
            else
            {
                text.Visibility = ViewStates.Invisible;
            }
        };
    }
}
```

上記の例では、遷移フレームワークを使用して、自動、プロパティ値の変更間の遷移の既定値を作成します。 アニメーションが処理されるため、1 つの行のコードによって、簡単にで、この Android の古いバージョンと互換性のあるラップすることによって、`BeginDelayedTransition`システムのバージョン チェックで呼び出します。 参照してください、 [Your KitKat アプリに移行する](#Migrating_Your_App_to_KitKat)詳細セクションです。

次のスクリーン ショットは、アニメーションの前に、アプリを示しています。

[![アニメーションの開始前に、アプリのスクリーン ショット](kitkat-images/trans-before.png)](kitkat-images/trans-before.png#lightbox)

次のスクリーン ショットは、アニメーションの終了後、アプリを示しています。

[![アニメーションの終了後のアプリのスクリーン ショット](kitkat-images/trans-after.png)](kitkat-images/trans-after.png#lightbox)

次のセクションで説明するシーンを使用した移行より詳細に制御を取得できます。

#### <a name="android-scenes"></a>Android のシーン

[シーン](https://developer.xamarin.com/api/type/Android.Transitions.Scene/)アニメーションより詳細に制御を開発者に提供するために遷移フレームワークの一部として導入されました。 シーンでは、UI で動的な領域を作成します。 コンテナーといくつかのバージョンでは、または"では"、コンテナーの内部 XML の内容を指定すると、Android は、シーンの間の遷移をアニメーション化する作業の残りの部分です。 Android のシーンでは、開発側で最小限の作業が複雑なアニメーションを作成できます。

動的なコンテンツを格納している静的の UI 要素は、呼び出された、*コンテナー*または*シーン基本*です。 次の例では、Android デザイナーを使用して、作成、`RelativeLayout`と呼ばれる`container`:

[![Android デザイナーを使用して、[相対レイアウト] コンテナーを作成するには](kitkat-images/container.png)](kitkat-images/container.png#lightbox)

サンプルのレイアウトでは、という名前のボタンも定義されて`sceneButton`下、`container`です。 このボタンには、遷移が発生します。

コンテナー内の動的なコンテンツには、2 つの新しい Android レイアウトが必要です。 これらのレイアウトを指定、コードだけで*内*コンテナーです。
次のコード例と呼ばれる、レイアウトを定義する*Scene1*を格納している 2 つのテキスト フィールドの読み取り「キット」と"Kat"それぞれ、および 2 番目のレイアウトと呼ばれる*Scene2*逆に、同じテキスト フィールドを格納しています。 XML は次のとおりです。

 **Scene1.axml**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<merge xmlns:android="http://schemas.android.com/apk/res/android">
    <TextView
        android:id="@+id/textA"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Kit"
        android:textSize="35sp" />
    <TextView
        android:id="@+id/textB"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_toRightOf="@id/textA"
        android:text="Kat"
        android:textSize="35sp" />
</merge>
```

 **Scene2.axml**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<merge xmlns:android="http://schemas.android.com/apk/res/android">
    <TextView
        android:id="@+id/textB"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Kat"
        android:textSize="35sp" />
    <TextView
        android:id="@+id/textA"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_toRightOf="@id/textB"
        android:text="Kit"
        android:textSize="35sp" />
</merge>
```

使用上の例の`merge`短いコードの表示を行うとビューの階層を簡素化します。 に関する詳細を読み取ることができます`merge`レイアウト[ここ](http://android-developers.blogspot.com/2009/03/android-layout-tricks-3-optimize-by.html)です。

呼び出して、シーンが作成された[ `Scene.GetSceneForLayout`](https://developer.xamarin.com/api/member/Android.Transitions.Scene.GetSceneForLayout/p/Android.Views.ViewGroup/System.Int32/Android.Content.Context/)シーンのレイアウト ファイル、および現在のリソース ID、コンテナー オブジェクトを渡して、`Context`次のコード例に示すように、します。

```csharp
RelativeLayout container = FindViewById<RelativeLayout> (Resource.Id.container);

Scene scene1 = Scene.GetSceneForLayout(container, Resource.Layout.Scene1, this);
Scene scene2 = Scene.GetSceneForLayout(container, Resource.Layout.Scene2, this);

scene1.Enter();
```

2 つのシーンは、Android の遷移の既定値をアニメーション化の間で反転ボタンをクリックします。

```csharp
sceneButton.Click += (o, e) => {
    Scene temp = scene2;
    scene2 = scene1;
    scene1 = temp;

    TransitionManager.Go (scene1);
};
```

次のスクリーン ショットは、アニメーションの前にシーンを示しています。

[![アニメーションの開始前に、のアプリのスクリーン ショット](kitkat-images/trans-after.png)](kitkat-images/trans-after.png#lightbox)

次のスクリーン ショットは、アニメーションの終了後、シーンを示しています。

[![アニメーションの完了後のアプリのスクリーン ショット](kitkat-images/scene.png)](kitkat-images/scene.png#lightbox)


> [!NOTE]
> [既知のバグ](https://code.google.com/p/android/issues/detail?id=62450)で Android の遷移のシーンの原因となるライブラリ使用して作成`GetSceneForLayout`をユーザーが移動すると、アクティビティを 2 番目の時間を中断します。 Java の回避策が記載されている[ここ](http://www.doubleencore.com/2013/11/new-transitions-framework/)です。


##### <a name="custom-transitions-in-scenes"></a>シーン内のカスタムの移行

Xml リソース ファイルにカスタム遷移を定義することができます、`transition`下にあるディレクトリ`Resources`次のスクリーン ショットに示すように、します。

[![リソース/遷移ディレクトリの下の transition.xml ファイルの場所](kitkat-images/resources.png)](kitkat-images/resources.png#lightbox)

次のコード サンプルを使用してアニメーションを 5 秒間の遷移を定義する、[補間を超えて](http://developer.android.com/reference/android/views/animation/OvershootInterpolator.html):

```xml
<changeBounds
  xmlns:android="http://schemas.android.com/apk/res/android"
  android:duration="5000"
  android:interpolator="@android:anim/overshoot_interpolator" />
```

アクティビティで、遷移が作成を使用して、 [TransitionInflater](https://developer.xamarin.com/api/type/Android.Transitions.TransitionInflater/)次のコードに示すように、します。

```csharp
Transition transition = TransitionInflater.From(this).InflateTransition(Resource.Transition.transition);
```

新しい移行に追加し、`Go`アニメーションが開始される呼び出し。

```csharp
TransitionManager.Go (scene1, transition);
```

### <a name="translucent-ui"></a>半透明の UI

KitKat 制御しやすくなりますテーマ経由で省略可能な transclucent ステータスとナビゲーション バーでのアプリです。 使用して、Android のテーマを定義する、同じ XML ファイル内のシステム UI 要素の透明度を変更することができます。 KitKat には、次のプロパティが導入されています。

-  `windowTranslucentStatus` -設定した場合に true の場合、最上位のステータス バー半透明。

-  `windowTranslucentNavigation` -設定した場合に true の場合、一番下のナビゲーション バー半透明。

-  `fitsSystemWindows` -Transcluent を上部または下部のバーを設定すると、既定で透過的な UI 要素の下でコンテンツがシフトします。 このプロパティを設定`true`コンテンツが重複半透明のシステム UI 要素にすることを防止する簡単な方法です。


次のコードでは、半透明のステータスとナビゲーション バーでテーマを定義します。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<resources>
    <style name="KitKatTheme" parent="android:Theme.Holo.Light">
        <item name="android:windowBackground">@color/xamgray</item>
        <item name="android:windowTranslucentStatus">true</item>
        <item name="android:windowTranslucentNavigation">true</item>
        <item name="android:fitsSystemWindows">true</item>
        <item name="android:actionBarStyle">@style/ActionBar.Solid.KitKat</item>
    </style>

    <style name="ActionBar.Solid.KitKat" parent="@android:style/Widget.Holo.Light.ActionBar.Solid">
        <item name="android:background">@color/xampurple</item>
    </style>
</resources>
```

次のスクリーン ショットは、状態: 半透明、ナビゲーション バーの上テーマを示しています。

[![半透明の状態とナビゲーション バーのあるアプリのサンプルのスクリーン ショット](kitkat-images/theme.png)](kitkat-images/theme.png#lightbox)

<a name="user_content" />

## <a name="user-content"></a>ユーザー コンテンツ

### <a name="storage-access-framework"></a>記憶域アクセス フレームワーク

記憶域アクセス フレームワーク (SAF) は、イメージ、ビデオ、およびドキュメントなどの格納されているコンテンツと対話するユーザーの新しい方法です。 コンテンツを処理するアプリケーションを選択するダイアログ ボックスでユーザーのプレゼンテーションのではなく KitKat 集計の 1 つの場所にデータにアクセスできる新しい UI を表示します。 コンテンツを選択すると、ユーザーは、コンテンツを要求したアプリケーションに返し、アプリ エクスペリエンスを通常どおり続行されます。

この変更には、開発者にある 2 つのアクションが必要です。 プロバイダーからのコンテンツを必要とするアプリケーションが最初、reqesting コンテンツの新しい方法に更新する必要があります。 データを書き込む、2 つ目のアプリケーション、`ContentProvider`新しいフレームワークを使用するように変更する必要があります。 両方のシナリオによって異なります、新しい[ `DocumentsProvider` ](https://developer.xamarin.com/api/type/Android.Provider.DocumentsProvider/) API です。

#### <a name="documentsprovider"></a>DocumentsProvider

KitKat とのやり取りで`ContentProviders`で抽象化、`DocumentsProvider`クラスです。 つまり、ある SAF いなし、データが物理的には、を介してアクセス可能である限り、 `DocumentsProvider` API です。 ローカル プロバイダーは、クラウド サービス、および外部記憶域デバイス インターフェイス、同一であり、同じ方法で処理がすべて使用する、ユーザーのコンテンツと対話する 1 つの場所に、ユーザーおよび開発者を提供します。

このセクションでは、読み込み、記憶域アクセス フレームワークでコンテンツを保存する方法について説明します。

#### <a name="request-content-from-a-provider"></a>プロバイダーからコンテンツを要求します。

いることがわかります KitKat で SAF UI を使用してコンテンツを選択すること、`ActionOpenDocument`目的で、デバイスに使用可能なすべてのコンテンツ プロバイダーに接続することを示します。 いくつかのフィルター処理にこのインテントを指定して追加することができます`CategoryOpenable`、つまり、(つまりアクセスで使用可能なコンテンツ) を開くことができるコンテンツのみが返されます。 KitKat が、コンテンツをフィルター処理を許可しても、`MimeType`です。 イメージを指定することによってイメージの結果のフィルターの下のコード例を`MimeType`:

```csharp
Intent intent = new Intent (Intent.ActionOpenDocument);
intent.AddCategory (Intent.CategoryOpenable);
intent.SetType ("image/*");
StartActivityForResult (intent, save_request_code);
```

呼び出す`StartActivityForResult`SAF UI では、ユーザーが画像を選択し、閲覧できるが起動します。

[![参照イメージする際の記憶域アクセス フレームワークを使用して、アプリのサンプルのスクリーン ショット](kitkat-images/saf-ui.png)](kitkat-images/saf-ui.png#lightbox)

ユーザーが、イメージを選択後`OnActivityResult`を返します、`Android.Net.Uri`の選択したファイル。 次のコード例では、ユーザーのイメージの選択が表示されます。

```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
    base.OnActivityResult(requestCode, resultCode, data);

    if (resultCode == Result.Ok && data != null && requestCode == save_request_code) {
        imageView = FindViewById<ImageView> (Resource.Id.imageView);
        imageView.SetImageURI (data.Data);
    }
}
```

#### <a name="write-content-to-a-provider"></a>コンテンツ プロバイダーを書き込む

SAF UI からのコンテンツの読み込み、に加えて KitKat することもできますいずれかにコンテンツを保存`ContentProvider`を実装する、 `DocumentProvider` API です。 コンテンツの使用を保存、`Intent`で`ActionCreateDocument`:

```csharp
Intent intentCreate = new Intent (Intent.ActionCreateDocument);
intentCreate.AddCategory (Intent.CategoryOpenable);
intentCreate.SetType ("text/plain");
intentCreate.PutExtra (Intent.ExtraTitle, "NewDoc");
StartActivityForResult (intentCreate, write_request_code);
```

上記のコード サンプルでは、ユーザー ファイル名を変更し、新しいファイルを格納するディレクトリを選択できるようにすること、SAF UI をロードします。

[![ダウンロード ディレクトリ NewDoc にファイル名を変更するユーザーのスクリーン ショット](kitkat-images/saf-save.png)](kitkat-images/saf-save.png#lightbox)

押されたとき**保存**、`OnActivityResult`が渡される、`Android.Net.Uri`アクセスするのには、新しく作成されたファイルの`data.Data`します。 Uri は、新しいファイルに、ストリーム データを使用できます。

```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
    base.OnActivityResult(requestCode, resultCode, data);

    if (resultCode == Result.Ok && data != null && requestCode == write_request_code) {
        using (Stream stream = ContentResolver.OpenOutputStream(data.Data)) {
            Encoding u8 = Encoding.UTF8;
            string content = "Hello, world!";
            stream.Write (u8.GetBytes(content), 0, content.Length);
        }
    }
}
```

なお[ `ContentResolver.OpenOutputStream(Android.Net.Uri)` ](https://developer.xamarin.com/api/member/Android.Content.ContentResolver.OpenOutputStream/(Android.Net.Uri))を返します、`System.IO.Stream`ストリーミング プロセス全体を .NET で記述できるように、します。

読み込みの詳細については、作成、および記憶域アクセス フレームワークとコンテンツの編集を参照してください、[記憶域アクセス Framework 用の Android ドキュメント](http://developer.android.com/guide/topics/providers/document-provider.html)です。

### <a name="printing"></a>印刷

導入に伴い KitKat で印刷内容が簡略化されて、[印刷サービス](https://developer.xamarin.com/api/namespace/Android.PrintServices/)と`PrintManager`です。 KitKat がフル活用するための最初の API バージョンではまた、 [Google のクラウド印刷サービス Api](https://developers.google.com/cloud-print/)を使用して、 [Google Cloud 印刷アプリケーション](https://play.google.com/store/apps/details?id=com.google.android.apps.cloudprint)です。
KitKat を自動的に付属するほとんどのデバイスは、Google Cloud 印刷アプリをダウンロードし、 [HP 印刷サービス プラグイン](https://play.google.com/store/apps/details?id=com.hp.android.printservice)WiFi に初めて接続するとき。 移動して、ユーザーがデバイスの印刷設定を確認できます**設定 > システム > 印刷**:

[![印刷設定 画面の例のスクリーン ショット](kitkat-images/printing.png)](kitkat-images/printing.png#lightbox)


> [!NOTE]
> 印刷の Api は、既定で Google Cloud の印刷を使用する設定は、Android 依然として、開発者は、新しい Api を使用してコンテンツを印刷の準備および印刷処理を行うには、他のアプリケーションに送信します。



#### <a name="printing-html-content"></a>HTML コンテンツの印刷

KitKat が自動的に作成、 [ `PrintDocumentAdapter` ](https://developer.xamarin.com/api/type/Android.Print.PrintDocumentAdapter/) web ビューに対して`WebView.CreatePrintDocumentAdapter`です。 Web コンテンツを印刷は間の連携作業、 [ `WebViewClient` ](https://developer.xamarin.com/api/type/Android.Webkit.WebViewClient/)をロードする HTML コンテンツを待機し、印刷オプションを [オプション] メニューで使用できるようにするアクティビティや、Actvity は、ユーザーが待機することができます呼び出しと印刷 オプションを選択`Print`上、`PrintManager`です。 ここでは、画面上を印刷するために必要な基本的なセットアップの HTML コンテンツ。

読み込みと web コンテンツの印刷がインターネット アクセス許可が必要であることを確認してください。

[![アプリケーションのオプションのインターネット アクセス許可を設定します。](kitkat-images/internet.png)](kitkat-images/internet.png#lightbox)

##### <a name="print-menu-item"></a>印刷メニュー項目

アクティビティの通常表示される印刷オプション[オプション メニュー](http://developer.android.com/guide/topics/ui/menus.html#options-menu)です。
[オプション] メニューを使用して、アクティビティのアクションを実行できます。 画面の右上隅し、次のような。

[![印刷メニュー項目いないことを示す画面の右上隅でのサンプルのスクリーン ショット](kitkat-images/menu.png)](kitkat-images/menu.png#lightbox)


追加のメニュー項目を定義することができます、*メニュー*下にあるディレクトリ*リソース*です。 次のコードを定義、サンプルというメニュー項目[印刷](https://developer.xamarin.com/api/type/Android.Print.PrintManager/):

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:id="@+id/menu_print"
        android:title="Print"
        android:showAsAction="never" />
</menu>
```

を介して、活動の [オプション] メニューとの対話が行われ、`OnCreateOptionsMenu`と`OnOptionsItemSelected`メソッドです。
`OnCreateOptionsMenu` [印刷] オプションと同様に、新しいメニュー項目を追加する場所、*メニュー*リソース ディレクトリ。
`OnOptionsItemSelected` メニューから、[印刷] オプションを選択すると、ユーザーをリッスンし、印刷を開始します。

```csharp
bool dataLoaded;

public override bool OnCreateOptionsMenu (IMenu menu)
{
    base.OnCreateOptionsMenu (menu);
    if (dataLoaded) {
        MenuInflater.Inflate (Resource.Menu.print, menu);
    }
    return true;
}

public override bool OnOptionsItemSelected (IMenuItem item)
{
    if (item.ItemId == Resource.Id.menu_print) {
        PrintPage ();
        return true;
    }
    return base.OnOptionsItemSelected (item);
}
```

上記のコードもという名前の変数を定義`dataLoaded`HTML コンテンツのステータスを追跡します。 `WebViewClient`オプション] メニューに [印刷メニュー項目を追加するアクティビティにわかるように、すべてのコンテンツが読み込まれるときに true には、この変数を設定します。

##### <a name="webviewclient"></a>WebViewClient

ジョブ、`WebViewClient`内のデータを確認することです、`WebView`メニューには、印刷オプションが表示される前に完全に読み込まれるが、`OnPageFinished`メソッドです。 `OnPageFinished` web コンテンツを読み込み、[完了] をリッスンし、そのオプション メニューを再作成するアクティビティを示す`InvalidateOptionsMenu`:

```csharp
class MyWebViewClient : WebViewClient
{
    PrintHtmlActivity caller;

    public MyWebViewClient (PrintHtmlActivity caller)
    {
        this.caller = caller;
    }

    public override void OnPageFinished (WebView view, string url)
    {
        caller.dataLoaded = true;
        caller.InvalidateOptionsMenu ();
    }
}
```

`OnPageFinished` また、設定、`dataLoaded`値を`true`ため、`OnCreateOptionsMenu`インプレース印刷オプションを使用してメニューを再作成できます。

##### <a name="printmanager"></a>PrintManager

次のコード例の内容を出力する、 `WebView`:

```csharp
void PrintPage ()
{
    PrintManager printManager = (PrintManager)GetSystemService (Context.PrintService);
    PrintDocumentAdapter printDocumentAdapter = myWebView.CreatePrintDocumentAdapter ();
    printManager.Print ("MyWebPage", printDocumentAdapter, null);
}
```

`Print` 引数として受け取り: ("MyWebPage"この例では)、印刷ジョブの名前、 [ `PrintDocumentAdapter` ](https://developer.xamarin.com/api/type/Android.Print.PrintDocumentAdapter/) 、コンテンツから印刷ドキュメントを生成して[ `PrintAttributes` ](https://developer.xamarin.com/api/type/Android.Print.PrintAttributes/) (`null`で、上記の例)。 指定できます`PrintAttributes`既定の属性は、ほとんど scenarions を処理する必要がありますが、印刷されるページのコンテンツのレイアウトは有効にします。

呼び出す`Print`印刷の UI では、印刷ジョブのオプションの一覧を読み込みます。 UI によって、次のスクリーン ショットに示すように、ユーザーの印刷または HTML コンテンツを保存して、PDF へのオプション。

[![[印刷] メニューを表示する PrintHtmlActivity のスクリーン ショット](kitkat-images/print1.png)](kitkat-images/print1.png#lightbox)

[![PrintHtmlActivity のスクリーン ショットは、保存を PDF メニューとして表示します。](kitkat-images/print2.png)](kitkat-images/print2.png#lightbox)

<a name="hardware" />

## <a name="hardware"></a>ハードウェア

KitKat は、新しいデバイスの機能に対応するいくつかの Api を追加します。 これらの最も重要なのホストに基づいたカードのエミュレーションと、新しい`SensorManager`です。

### <a name="host-based-card-emulation-in-nfc"></a>NFC エミュレーション ホスト ベースのカード

ホスト ベース カード エミュレーション (HCE) には、配送業者の独自のセキュリティで保護された要素に依存せず、NFC カードや NFC カード リーダーのように動作するアプリケーションができます。 HCE をセットアップする前に HCE を利用できるようにデバイスに`PackageManager.HasSystemFeature`:

```csharp
bool hceSupport = PackageManager.HasSystemFeature(PackageManager.FeatureNfcHostCardEmulation);
```

HCE では、する必要があります HCE 機能および`Nfc`アクセス許可が、アプリケーションに登録されている`AndroidManifest.xml`:

```xml
<uses-feature android:name="android.hardware.nfc.hce" />
```

[![アプリケーションのオプションの NFC アクセス許可の設定](kitkat-images/nfc.png)](kitkat-images/nfc.png#lightbox)

を使用する、HCE がバック グラウンドで実行できる必要があり、HCE を使用して、アプリケーションが実行されていない場合でも、ユーザーが、NFC トランザクションを開始する必要があります。 これを行うとして HCE コードを記述して、`Service`です。 HCE サービスを実装、`HostApduService`インターフェイスで、次のメソッドを実装します。

-  *ProcessCommandApdu* -、アプリケーション プロトコル データ ユニット (APDU) は、NFC リーダーと HCE サービス間で送信内容を取得します。 このメソッドは、リーダーから、ADPU を消費し、応答データ ユニットを返します。

-  *OnDeactivated* - `HostAdpuService` NFC リーダーに HCE サービスが通信していないときに無効になります。


HCE サービスも、アプリケーションのマニフェストに登録され、適切なアクセス権、インテント フィルター、およびメタデータで装飾する必要があります。 次のコードの例は、 `HostApduService` Android マニフェストを使用して、登録されている、`Service`属性 (属性の詳細については、Xamarin を参照してください[Android マニフェストの使用](~/android/platform/android-manifest.md)ガイド)。

```csharp
[Service(Exported=true, Permission="android.permissions.BIND_NFC_SERVICE"),
    IntentFilter(new[] {"android.nfc.cardemulation.HOST_APDU_SERVICE"}),
    MetaData("andorid.nfc.cardemulation.host.apdu_service",
    Resource="@xml/hceservice")]

class HceService : HostApduService
{
    public override byte[] ProcessCommandApdu(byte[] apdu, Bundle extras)
    {
        ...
    }

    public override void OnDeactivated (DeactivationReason reason)
    {
        ...
    }
}
```

上記のサービス、アプリケーションと対話する NFC リーダーの手段を提供、NFC リーダーはこのサービスがスキャンする必要がある NFC カードをエミュレートする場合を知る方法がありませんが。 NFC リーダー サービスを識別するためは割り当てることができます、サービスを一意*アプリケーション ID (補助)*です。 お HCE サービスに関するその他のメタデータと共に、参考情報を指定して、xml リソース ファイル内に登録されている、`MetaData`属性 (上記のコード例を参照してください)。 このリソース ファイルは、1 つまたは複数補助フィルターの 1 つ以上の NFC リーダー デバイスの補助に対応する 16 進形式で一意の識別子の文字列を指定します。

```xml
<host-apdu-service xmlns:android="http://schemas.android.com/apk/res/android"
    android:description="@string/hce_service_description"
    android:requireDeviceUnlock="false"
    android:apduServiceBanner="@drawable/service_banner">
    <aid-group android:description="@string/aid_group_description"
                android:category="payment">
        <aid-filter android:name="1111111111111111"/>
        <aid-filter android:name="0123456789012345"/>
    </aid-group>
</host-apdu-service>
```

補助フィルターだけでなく xml リソース ファイルは、HCE サービスのユーザー向けの説明も用意されています。 補助グループ ("その他"と支払アプリケーション) を指定し、アプリケーションの場合、支払い、260 x 96 dp バナーをユーザーに表示します。

上記で説明したセットアップは、NFC カードをエミュレートするアプリケーションの基本的なビルド ブロックを提供します。 NFC 自体には、さらにいくつかの手順と構成をさらにテストが必要です。 カード エミュレーションのホスト ベースの詳細についてを参照してください、 [Android ドキュメント ポータル](https://developer.android.com/guide/topics/connectivity/nfc/hce.html)です。
NFC を使用して、Xamarin を使用する方法については、チェック アウト、 [Xamarin NFC サンプル](https://github.com/xamarin/monodroid-samples/tree/master/NfcSample)です。

### <a name="sensors"></a>センサー

KitKat を通じて、デバイスのセンサーへのアクセスを提供する、 [ `SensorManager`](https://developer.xamarin.com/api/type/Android.Hardware.SensorManager/)です。
`SensorManager`センサー情報をバッチでアプリケーションの配信をスケジュールする OS は、バッテリの寿命を保持します。

KitKat には、また、ユーザーの手順を追跡するため 2 の新しい種類のセンサーが付属しています。 これらは、加速度計に基づいておりが含まれます。

-  *StepDetector* -アプリは通知を受け取る/ウェイク状態が、ユーザーは、ステップと探知機は、手順が発生したときの時刻値を提供します。

-  *StepCounter* -のセンサーが登録されているため、ユーザーが実行される手順の数を追跡*[次へ]、デバイスが再起動されるまで*です。

次のスクリーン ショットは、アクションで、手順カウンタを示しています。

[![手順のカウンターを表示する SensorsActivity アプリのスクリーン ショット](kitkat-images/stepcounter.png)](kitkat-images/stepcounter.png#lightbox)

作成することができます、`SensorManager`を呼び出して`GetSystemService(SensorService)`として結果をキャストし、`SensorManager`です。 手順カウンターを使用するのには、呼び出す`GetDeafultSensor`上、`SensorManager`です。 センサーを登録およびを利用してカウントのステップで変更をリッスンする、 [ `ISensorEventListener` ](https://developer.xamarin.com/api/type/Android.Hardware.ISensorEventListener/)インターフェイス、次のコード例に示すようにします。

```csharp
public class MainActivity : Activity, ISensorEventListener
{
    float count = 0;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);
        SetContentView (Resource.Layout.Main);

        SensorManager senMgr = (SensorManager) GetSystemService (SensorService);
        Sensor counter = senMgr.GetDefaultSensor (SensorType.StepCounter);
        if (counter != null) {
            senMgr.RegisterListener(this, counter, SensorDelay.Normal);
        }
    }

    public void OnAccuracyChanged (Sensor sensor, SensorStatus accuracy)
    {
        Log.Info ("SensorManager", "Sensor accuracy changed");
    }

    public void OnSensorChanged (SensorEvent e)
    {
        count = e.Values [0];
    }
}
```

`OnSensorChanged` カウントのステップは、アプリケーションがフォア グラウンドで間を更新する場合と呼びます。 アプリケーションが、バック グラウンドに入るか、デバイスがスリープ状態の場合`OnSensorChanged`は呼び出されません。 ただし、手順は引き続きまでカウントする`UnregisterListener`と呼びます。

ただし、*センサーを登録するすべてのアプリケーション間でステップ数の値が累積的な*します。 つまり、アンインストールし、アプリケーションを再インストールして初期化する場合でも、`count`アプリケーションの起動時に 0 で環境変数、センサーで感知された値は残りますセンサーが登録されているときに実行される手順の合計数によってかどうか、アプリケーションまたは別にします。 アプリから呼び出すことによって手順カウンターを追加することを防ぐことができます`UnregisterListener`上、`SensorManager`次のコードに示すように、します。

```csharp
protected override void OnPause()
{
    base.OnPause ();
    senMgr.UnregisterListener(this);
}
```

0 にステップごとのカウントをリセットするデバイスを再起動します。 アプリには、センサーまたはデバイスの状態を使用して他のアプリケーションに関係なく、アプリケーションの正確なカウントを報告していることを確認する追加コードが必要です。


> [!NOTE]
> 手順の検出と KitKat に付属のカウントの API、中に、センサーいないすべてのスマート フォンであり、します。 センサーがあるかを実行して確認できます`PackageManager.HasSystemFeature(PackageManager.FeatureSensorStepCounter);`、戻り値のことを確認するか、または`GetDefaultSensor`いない`null`です。


<a name="developer_tools" />

## <a name="developer-tools"></a>開発者用ツール

### <a name="screen-recording"></a>画面記録

KitKat には、新しい画面開発者がアクションでアプリケーションを記録できるように、録音機能にはが含まれています。 画面記録はから利用できる、 [Android デバッグ Bridge (ADB)](http://developer.android.com/tools/help/adb.html)クライアントで、Android SDK の一部としてダウンロードすることができます。

録音するには、画面; デバイスを接続します。次に、検索、Android SDK のインストールに移動、**プラット フォーム ツール**ディレクトリと実行、 **adb**クライアント。

```shell
adb shell screenrecord /sdcard/screencast.mp4
```

上記のコマンドは、既定の解像度で 4 mbps 既定 3 分間のビデオを記録します。 長さを編集するには、追加、 *--制限時間*フラグ。
解像度を変更するには追加、 *--ビット レート*フラグ。 次のコマンドは 8Mbps で 1 分間のビデオを記録します。

```shell
adb shell screenrecord --bit-rate 8000000 --time-limit 60 /sdcard/screencast.mp4
```

デバイスでビデオを見つけることができます - 記録が完了すると、ギャラリーに表示されます。


## <a name="other-kitkat-additions"></a>その他の KitKat 追加

上記の変更だけでなく KitKat にすることができます。

-  *全画面表示を使用して*-KitKat が導入されていますが、新しい[イマーシブ モード](http://developer.android.com/reference/android/view/View.html#setSystemUiVisibility(int))のコンテンツを参照する、ゲーム、および全画面表示の経験から利点がもたらされる他のアプリケーションを実行します。

-  *通知をカスタマイズする*-システム通知に関する追加情報を取得、 [ `NotificationListenerService` ](https://developer.xamarin.com/api/type/Android.Service.Notification.NotificationListenerService/)です。 これにより、アプリ内で別の方法で情報を表示できます。

-  *ドロウアブル リソースをミラー化*-ドロウアブル リソースがある新しい[ `autoMirrored` ](http://developer.android.com/reference/android/R.attr.html#autoMirrored)システムに通知する属性が左から右のレイアウトを切り替えることを必要とするイメージのミラー化されたバージョンを作成します。

-  *アニメーションを一時停止*-一時停止と再開のアニメーションで作成された、 [ `Animator` ](https://developer.xamarin.com/api/type/Android.Animation.Animator/)クラスです。

-  *動的に変更するテキストの読み取り*-UI の中の動的更新を新しいテキストに「ライブ領域」としてに新しい部分を表す[ `accesibilityLiveRegion` ](http://developer.android.com/reference/android/R.attr.html#accessibilityLiveRegion)属性の新しいテキストはアクセシビリティ モードで自動的に読み込まれるようにします。

-  *オーディオのエクスペリエンスの向上*-Make トラック音、 [ `LoudnessEnhancer` ](https://developer.xamarin.com/api/type/Android.Media.Audiofx.LoudnessEnhancer/) 、ピーク時と RMS を含むオーディオのストリームの検索、 [ `Visualizer` ](https://developer.xamarin.com/api/field/Android.Media.Audiofx.Visualizer.MeasurementModePeakRms/)クラス、および、から情報を取得[オーディオ タイムスタンプ](https://developer.xamarin.com/api/type/Android.Media.AudioTimestamp/)オーディオ ビデオ同期を支援します。

-  *カスタムの間隔で ContentResolver を同期*-KitKat が同期要求が実行された時刻にいくつかのばらつきを追加します。 同期、`ContentResolver`ユーザー定義の時刻または間隔を呼び出してで`ContentResolver.RequestSync`およびを渡して、`SyncRequest`です。

-  *コント ローラー間で識別*-KitKat では、コント ローラーは、デバイスの経由でアクセスできる一意の整数識別子を割り当てられる`ControllerNumber`プロパティです。 これにより、ゲームの間隔のプレーヤーに伝えますやすくします。

-  *リモート制御*-ハードウェアとソフトウェアの両方の側でいくつかの変更、KitKat でリモート_コントロールを使用して、、赤外線送信機能であり、デバイスを無効にする、 `ConsumerIrService`、および新しい周辺機器対話[ `RemoteController` ](https://developer.xamarin.com/api/type/Android.Media.RemoteController/) Api です。

上記の API の変更の詳細についてを参照してください、Google [Android 4.4 Api](http://developer.android.com/about/versions/android-4.4.html)の概要です。


## <a name="summary"></a>まとめ

ここでは、Android 4.4 (API レベル 19) で利用可能な新しい Api の一部を導入し、KitKat にアプリケーションを移行する際に、ベスト プラクティスを説明します。 It ユーザーに影響する Api をアウトライン表示の変更が発生するなど、*遷移 framework*と用の新しいオプション*テーマ*です。 導入された次に、*記憶域アクセス フレームワーク*と`DocumentsProvider`クラスだけでなく、新しい*Api の印刷*です。 ため、探索、 *NFC ホスト ベースのカード エミュレーション*を使用する方法と*低電力センサー*ユーザーの手順を追跡するための 2 つの新しいセンサーなど。 キャプチャ リアルタイムのデモを持つアプリケーションの最後に、説明*画面記録*、KitKat API の変更と追加の詳細な一覧を提供します。


## <a name="related-links"></a>関連リンク

- [KitKat サンプル](https://developer.xamarin.com/samples/KitKat/)
- [Android 4.4 Api](http://developer.android.com/about/versions/android-4.4.html)
- [Android KitKat](http://developer.android.com/about/versions/kitkat.html)
