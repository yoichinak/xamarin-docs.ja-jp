---
title: KitKat の機能
description: Android 4.4 (KitKat) には、ユーザーと開発者向けのさまざまな機能が搭載されています。 このガイドでは、これらの機能のいくつかを取り上げ、KitKat を最大限に活用するために役立つコード例と実装の詳細について説明します。
ms.prod: xamarin
ms.assetid: D3FDEA1C-F076-406F-BCC3-2A55D2C6ADEE
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: 7a3fd9e22bcf037ec669c77ac919035b0d04b942
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/09/2020
ms.locfileid: "84567932"
---
# <a name="kitkat-features"></a>KitKat の機能

_Android 4.4 (KitKat) には、ユーザーと開発者向けのさまざまな機能が搭載されています。このガイドでは、これらの機能のいくつかを取り上げ、KitKat を最大限に活用するために役立つコード例と実装の詳細について説明します。_

## <a name="overview"></a>概要

"KitKat" とも呼ばれる Android 4.4 (API レベル 19) は、2013 年後半にリリースされました。 KitKat では、次のようなさまざまな新機能と機能強化が提供されています。

- [ユーザー エクスペリエンス](#user_experience) &ndash; 遷移フレームワーク、半透明の状態とナビゲーション バー、全画面表示のイマーシブ モードを備えた簡単なアニメーションを使うと、ユーザーのエクスペリエンスを向上させることができます。

- [ユーザー コンテンツ](#user_content) &ndash; ストレージ アクセス フレームワークによるユーザー ファイル管理の簡素化。印刷 API が改善され、写真、Web サイト、その他のコンテンツの印刷が簡単になりました。

- [ハードウェア](#hardware) &ndash; アプリを NFC ホストベースのカード エミュレーションを備えた NFC カードに変換します。`SensorManager` により、低電力センサーを実行します。

- [デベロッパー ツール](#developer_tools) &ndash; Android SDK の一部として使用できる Android Debug Bridge クライアントで動作するスクリーンキャスト アプリケーション。

このガイドでは、既存の Xamarin.Android アプリケーションを KitKat に移行するためのガイダンスと、Xamarin.Android 開発者向けに KitKat の概要について説明します。

## <a name="requirements"></a>必要条件

KitKat を使用して Xamarin.Android アプリケーションを開発するには、次のスクリーンショットに示すように、*Xamarin.Android 4.11.0* 以降と Android 4.4 (API レベル 19) を Android SDK マネージャーを介してインストールする必要があります。

[![Android SDK マネージャーで Android 4.4 を選択](kitkat-images/api19.png)](kitkat-images/api19.png#lightbox)

<a name="Migrating_Your_App_to_KitKat"></a>

## <a name="migrating-your-app-to-kitkat"></a>KitKat へのアプリの移行

このセクションでは、既存のアプリケーションを Android 4.4 に移行するための初期対応項目について説明します。

### <a name="check-system-version"></a>システム バージョンの確認

以前のバージョンの Android との互換性を持つ必要があるアプリケーションの場合は、以下のコード サンプルに示すように、システム バージョン チェックで KitKat 固有のコードをラップします。

```csharp
if (Build.VERSION.SdkInt >= BuildVersionCodes.Kitkat) {
    //KitKat only code here
}
```

### <a name="alarm-batching"></a>アラームのバッチ処理

Android では、指定された時間にアプリをバックグラウンドで起動するためにアラーム サービスが使用されます。 KitKat では、この処理がさらに一歩進化し、アラームのバッチ処理によって電力が節約されます。 つまり、KitKat では、個々のアプリが正確な時刻に起動されるのではなく、同じ時間間隔で起動するように登録されている複数のアプリケーションがグループにまとめられ、同時に起動される処理が優先されます。
指定された時間間隔でアプリを起動するように Android に指示するには、[`AlarmManager`](xref:Android.App.AlarmManager) 上で `SetWindow` を呼び出し、アプリを起動するまでの経過時間として許容できる最短時間と最長時間をミリ秒単位で渡し、起動時に実行する操作を渡します。
次のコードは、ウィンドウが設定されてから 30 分から 1 時間の間に起動する必要があるアプリケーションの例です。

```csharp
AlarmManager alarmManager = (AlarmManager)GetSystemService(AlarmService);
alarmManager.SetWindow (AlarmType.Rtc, AlarmManager.IntervalHalfHour, AlarmManager.IntervalHour, pendingIntent);
```

正確な時間でアプリを起動し続けるには、`SetExact` を使用し、アプリを起動する正確な時間と実行する操作を渡します。

```csharp
alarmManager.SetExact (AlarmType.Rtc, AlarmManager.IntervalDay, pendingIntent);
```

KitKat では、正確な繰り返しアラームを設定できなくなりました。 [`SetRepeating`](xref:Android.App.AlarmManager.SetRepeating*) を使用し、
正確なアラームを動作させる必要があるアプリケーションの場合、各アラームを手動でトリガーする必要があります。

### <a name="external-storage"></a>外部ストレージ

外部ストレージは 2 種類に分割されました。アプリケーションに固有のストレージと、複数のアプリケーションで共有されるデータに固有のものです。 外部ストレージ上のアプリに固有の場所に対する読み取りと書き込みには、特別なアクセス許可は必要ありません。 共有ストレージ上のデータを操作するには、`READ_EXTERNAL_STORAGE` または `WRITE_EXTERNAL_STORAGE` アクセス許可が必要になりました。 この 2 種類は、次のように分類できます。

- ファイルまたはディレクトリのパスを取得するために `Context` に対してメソッドを呼び出す場合 (たとえば [`GetExternalFilesDir`](xref:Android.Content.Context.GetExternalFilesDir*)
  または [`GetExternalCacheDirs`](xref:Android.Content.Context.GetExternalCacheDirs))
  - アプリに追加のアクセス許可は必要ありません。

- ファイルまたはディレクトリのパスを取得するために、プロパティにアクセスするか、`Environment` に対してメソッドを呼び出す場合 (たとえば [`GetExternalStorageDirectory`](xref:Android.OS.Environment.ExternalStorageDirectory)
  または [`GetExternalStoragePublicDirectory`](xref:Android.OS.Environment.GetExternalStoragePublicDirectory*))
  、`READ_EXTERNAL_STORAGE` または `WRITE_EXTERNAL_STORAGE` のアクセス許可がアプリに必要です。

> [!NOTE]
> `WRITE_EXTERNAL_STORAGE` には `READ_EXTERNAL_STORAGE` アクセス許可も含まれているため、いずれかのアクセス許可のみを設定する必要があります。

### <a name="sms-consolidation"></a>SMS の統合

KitKat では、ユーザーが選択した 1 つの既定のアプリケーションにすべての SMS コンテンツを集計することにより、ユーザーのメッセージングを簡略化しています。 開発者は、既定のメッセージング アプリケーションとしてアプリケーションを選択できるようにして、アプリケーションが選択されていない場合はコード内とアクティブなときに適切に動作するようにします。 SMS アプリを KitKat に移行する方法の詳細については、Google の「[Getting Your SMS Apps Ready for KitKat (SMS アプリを KitKat 用に準備する)](https://android-developers.blogspot.com/2013/10/getting-your-sms-apps-ready-for-kitkat.html)」ガイドを参照してください。

### <a name="webview-apps"></a>WebView アプリ

KitKat では [WebView](xref:Android.Webkit.WebView) が一新されました。 最大の変更点は、コンテンツを `WebView` に読み込む際のセキュリティが強化されたことです。 以前の API バージョンをターゲットとするほとんどのアプリケーションは想定どおりに動作するはずですが、`WebView` クラスを使用するアプリケーションのテストを強くお勧めします。 影響を受ける WebView API の詳細については、Android の「[Android 4.4 での WebView への移行](https://developer.android.com/guide/webapps/migrating.html)」ドキュメントを参照してください。

<a name="user_experience"></a>

## <a name="user-experience"></a>ユーザー体験

KitKat には、ユーザー エクスペリエンスを強化する新しい API がいくつか追加されました。たとえば、プロパティ アニメーションを処理するための新しい移行フレームワークや、テーマ用の半透明の UI オプションなどです。 ここでは、これらの変更について説明します。

### <a name="transition-framework"></a>遷移フレームワーク

遷移フレームワークを使用すると、アニメーションの実装が容易になります。 KitKat を使用すると、1 行のコードで簡単なプロパティ アニメーションを実行することや、"*シーン*" を使用して遷移をカスタマイズすることができます。

#### <a name="simple-property-animation"></a>単純なプロパティ アニメーション

新しい Android Transitions ライブラリを使用すると、プロパティ アニメーションの背後にあるコードが簡単になります。 フレームワークを使用すると、最小限のコードで単純なアニメーションを実行できます。 たとえば、次のコード サンプルでは [`TransitionManager.BeginDelayedTransition`](xref:Android.Transitions.TransitionManager.BeginDelayedTransition*) を使用して
`TextView` の表示と非表示をアニメーション化しています。

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

上の例では、遷移フレームワークを使用して、変化するプロパティ値間の自動的な既定の遷移を作成します。 アニメーションは 1 行のコードで処理されるため、システム バージョン チェックで `BeginDelayedTransition` の呼び出しをラップすることで、以前のバージョンの Android と簡単に互換性を持たせることができます。 詳細については、「[KitKat へのアプリの移行](#Migrating_Your_App_to_KitKat)」を参照してください。

次のスクリーンショットは、アニメーションの前のアプリを示しています。

[![アニメーションが開始される前のアプリのスクリーンショット](kitkat-images/trans-before.png)](kitkat-images/trans-before.png#lightbox)

次のスクリーンショットは、アニメーションの後のアプリを示しています。

[![アニメーションが完了した後のアプリのスクリーンショット](kitkat-images/trans-after.png)](kitkat-images/trans-after.png#lightbox)

次のセクションで説明するように、バックグラウンドで遷移をより細かく制御できます。

#### <a name="android-scenes"></a>Android のシーン

[シーン](xref:Android.Transitions.Scene)は、開発者がアニメーションをより細かく制御できるように、遷移フレームワークの一部として導入されました。 シーンを使用すると、UI に動的な領域を作成できます。コンテナー内の XML コンテンツに対してコンテナーといくつかのバージョン (つまり "シーン") を指定すると、シーン間の遷移をアニメーション化する残りの処理は Android によって自動的に行われます。 Android のシーンを使用すると、開発側での作業を最小限に抑えて複雑なアニメーションを作成できます。

動的コンテンツを格納する静的 UI 要素は、"*コンテナー*" または "*シーン ベース*" と呼ばれます。 次の例では、Android Designer を使用して `container` という `RelativeLayout` を作成します。

[![Android Designer を使用した RelativeLayout コンテナーの作成](kitkat-images/container.png)](kitkat-images/container.png#lightbox)

サンプル レイアウトでは、`container` の下に `sceneButton` というボタンも定義されています。 このボタンをクリックすると、遷移がトリガーされます。

コンテナー内の動的コンテンツには、2 つの新しい Android レイアウトが必要です。 これらのレイアウトには、コンテナー "*内*" のコードのみが指定されています。
次のコード例では、それぞれ "Kit" と "Kat" を読み取る 2 つのテキスト フィールドを含む *Scene1* というレイアウトと、反転した同じテキスト フィールドを含む *Scene2* という 2 つ目のレイアウトを定義しています。 XML は次のとおりです。

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

上の例では、`merge` を使用してビュー コードを短くし、ビュー階層を簡略化しています。 `merge` レイアウトの詳細については、[こちら](https://android-developers.blogspot.com/2009/03/android-layout-tricks-3-optimize-by.html)を参照してください。

シーンを作成するには、次のコード例に示すように、[`Scene.GetSceneForLayout`](xref:Android.Transitions.Scene.GetSceneForLayout*) を呼び出し、コンテナー オブジェクト、シーンのレイアウト ファイルのリソース ID、および現在の `Context` を渡します。

```csharp
RelativeLayout container = FindViewById<RelativeLayout> (Resource.Id.container);

Scene scene1 = Scene.GetSceneForLayout(container, Resource.Layout.Scene1, this);
Scene scene2 = Scene.GetSceneForLayout(container, Resource.Layout.Scene2, this);

scene1.Enter();
```

ボタンをクリックすると、2 つのシーンが切り替わります。これは、Android の既定の遷移値でアニメーション化されたものです。

```csharp
sceneButton.Click += (o, e) => {
    Scene temp = scene2;
    scene2 = scene1;
    scene1 = temp;

    TransitionManager.Go (scene1);
};
```

次のスクリーンショットは、アニメーションの前のシーンを示しています。

[![アニメーションを開始する前のアプリのスクリーンショット](kitkat-images/trans-after.png)](kitkat-images/trans-after.png#lightbox)

次のスクリーンショットは、アニメーションの後のシーンを示しています。

[![アニメーションが完了した後のアプリのスクリーンショット](kitkat-images/scene.png)](kitkat-images/scene.png#lightbox)

> [!NOTE]
> Android Transitions ライブラリには[既知のバグ](https://code.google.com/p/android/issues/detail?id=62450)があり、ユーザーが 2 回目に Activity を操作すると `GetSceneForLayout` を使用して作成されたシーンが動作しなくなります。 Java の回避策については、[こちら](http://www.doubleencore.com/2013/11/new-transitions-framework/)を参照してください。

##### <a name="custom-transitions-in-scenes"></a>シーンのカスタムの遷移

カスタムの遷移は、次のスクリーンショットに示すように、`Resources` 以下の `transition` ディレクトリにある xml リソース ファイルで定義できます。

[![Resources/transition ディレクトリにある transition.xml ファイルの場所](kitkat-images/resources.png)](kitkat-images/resources.png#lightbox)

次のコード サンプルでは、アニメーションが 5 秒間動作し、[オーバーシュート インターポレーター](https://developer.android.com/reference/android/views/animation/OvershootInterpolator.html)を使用する遷移を定義しています。

```xml
<changeBounds
  xmlns:android="http://schemas.android.com/apk/res/android"
  android:duration="5000"
  android:interpolator="@android:anim/overshoot_interpolator" />
```

遷移は、次のコードに示すように、[TransitionInflater](xref:Android.Transitions.TransitionInflater) を使用して Activity 内に作成されます。

```csharp
Transition transition = TransitionInflater.From(this).InflateTransition(Resource.Transition.transition);
```

次に、`Go` の呼び出しに、アニメーションを開始する新しい遷移が追加されます。

```csharp
TransitionManager.Go (scene1, transition);
```

### <a name="translucent-ui"></a>半透明の UI

KitKat では、オプションの半透明のステータス バーとナビゲーション バーを使ってアプリのテーマをより細かく制御できます。 Android テーマの定義に使用するものと同じ XML ファイル内で、システム UI 要素の透明度を変更できます。 KitKat では、次のプロパティが導入されました。

- `windowTranslucentStatus` - true に設定すると、上部のステータス バーが半透明になります。

- `windowTranslucentNavigation` - true に設定すると、下部のナビゲーション バーが半透明になります。

- `fitsSystemWindows` - 上部または下部のバーを半透明に設定すると、既定で透明な UI 要素の下にコンテンツが移動されます。 このプロパティを `true` に設定すると、簡単にコンテンツが半透明のシステム UI 要素と重ならないようにすることができます。

次のコードでは、半透明のステータス バーとナビゲーション バーがあるテーマを定義します。

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

次のスクリーンショットは、半透明のステータス バーとナビゲーション バーがある上のテーマを示しています。

[![半透明のステータス バーとナビゲーション バーがあるアプリのスクリーンショットの例](kitkat-images/theme.png)](kitkat-images/theme.png#lightbox)

<a name="user_content"></a>

## <a name="user-content"></a>ユーザー コンテンツ

### <a name="storage-access-framework"></a>Storage-Access フレームワーク

ストレージ アクセス フレームワーク (SAF) は、画像、ビデオ、ドキュメントなどの格納されているコンテンツをユーザーが操作するための新しい方法です。 KitKat では、コンテンツを処理するアプリケーションを選択するダイアログをユーザーに表示するのではなく、ユーザーが 1 つの集約された場所でデータにアクセスできる新しい UI が開かれます。 コンテンツが選択されると、ユーザーはコンテンツを要求したアプリケーションに戻り、アプリのエクスペリエンスは通常どおり続行されます。

この変更には、開発者側で 2 つのアクションが必要です。まず、プロバイダーからのコンテンツを必要とするアプリを、コンテンツを要求する新しい方法に更新する必要があります。 次に、`ContentProvider` にデータを書き込むアプリケーションを、新しいフレームワークを使用するように変更する必要があります。 どちらのシナリオも、新しい [`DocumentsProvider`](xref:Android.Provider.DocumentsProvider) に依存しています
[API]。

#### <a name="documentsprovider"></a>DocumentsProvider

KitKat では、`ContentProviders` との相互作用は `DocumentsProvider` クラスで抽象化されています。 つまり、`DocumentsProvider` API を介してアクセスできる限り、データの物理的な場所に関係なく SAF は機能します。 ローカル プロバイダー、クラウド サービス、および外部ストレージ デバイスのすべてには同じインターフェイスが使用され、同じ方法で処理され、ユーザーと開発者がユーザーのコンテンツを 1 か所で操作できるようになります。

このセクションでは、ストレージ アクセス フレームワークを使用してコンテンツを読み込み、保存する方法について説明します。

#### <a name="request-content-from-a-provider"></a>プロバイダーのコンテンツを要求する

SAF UI を `ActionOpenDocument` インテント (デバイスから使用できるすべてのコンテンツ プロバイダーに接続することを意味します) と共に使用してコンテンツを選択するように KitKat に指示することができます。 このインテントにフィルターを追加するには、`CategoryOpenable` を指定します。これは、開くことができるコンテンツ (つまり、アクセス可能で使用可能なコンテンツ) のみが返されることを意味します。 KitKat では、`MimeType` を使用してコンテンツをフィルター処理することもできます。 たとえば、次のコードでは、画像 `MimeType` を指定して、画像の結果をフィルター処理しています。

```csharp
Intent intent = new Intent (Intent.ActionOpenDocument);
intent.AddCategory (Intent.CategoryOpenable);
intent.SetType ("image/*");
StartActivityForResult (intent, save_request_code);
```

`StartActivityForResult` を呼び出すと SAF UI が起動し、ユーザーはそれを参照して画像を選択できます。

[![画像を参照するためにストレージ アクセス フレームワークを使用するアプリのスクリーンショットの例](kitkat-images/saf-ui.png)](kitkat-images/saf-ui.png#lightbox)

ユーザーが画像を選択すると、選択したファイルの `Android.Net.Uri` が `OnActivityResult` から返されます。 次のコード サンプルを実行すると、ユーザーの画像選択が表示されます。

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

#### <a name="write-content-to-a-provider"></a>プロバイダーへのコンテンツの書き込み

KitKat では、SAF UI からコンテンツを読み込むだけでなく、`DocumentProvider` API を実装する `ContentProvider` にコンテンツを保存することもできます。 コンテンツの保存には、`Intent` と `ActionCreateDocument` を使用します。

```csharp
Intent intentCreate = new Intent (Intent.ActionCreateDocument);
intentCreate.AddCategory (Intent.CategoryOpenable);
intentCreate.SetType ("text/plain");
intentCreate.PutExtra (Intent.ExtraTitle, "NewDoc");
StartActivityForResult (intentCreate, write_request_code);
```

上のコード サンプルでは、SAF UI を読み込み、ユーザーがファイル名を変更し、新しいファイルを格納するディレクトリを選択できるようにします。

[![Downloads ディレクトリにあるファイル名をユーザーが NewDoc に変更するスクリーンショット](kitkat-images/saf-save.png)](kitkat-images/saf-save.png#lightbox)

ユーザーが **[Save]\(保存\)** を押すと、`OnActivityResult`には新しく作成されたファイルの `Android.Net.Uri` が渡されます。これには `data.Data` を使用してアクセスできます。 URI を使用して、データを新しいファイルにストリーミングできます。

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

注意が必要な点は、[`ContentResolver.OpenOutputStream(Android.Net.Uri)`](xref:Android.Content.ContentResolver.OpenOutputStream*)
から `System.IO.Stream` が返されるので、ストリーミング プロセス全体を .NET で記述できることです。

ストレージ アクセス フレームワークを使用したコンテンツの読み込み、作成、編集の詳細については、[ストレージ アクセス フレームワークに関する Android ドキュメント](https://developer.android.com/guide/topics/providers/document-provider.html)を参照してください。

### <a name="printing"></a>印刷

KitKat では、[印刷サービス](xref:Android.PrintServices)と `PrintManager` が導入されたことで、コンテンツの印刷コンテンツが簡単になりました。 また、KitKat は、[Google クラウド プリント アプリケーション](https://play.google.com/store/apps/details?id=com.google.android.apps.cloudprint)を使用して [Google のクラウド プリント サービス API](https://developers.google.com/cloud-print/) を完全に活用する最初の API バージョンでもあります。
KitKat が搭載されているほとんどのデバイスでは、初めて WiFi に接続するときに、Google クラウド プリント アプリと [HP プリント サービス プラグイン](https://play.google.com/store/apps/details?id=com.hp.android.printservice)が自動的にダウンロードされます。 ユーザーは、 **[設定] > [システム] > [印刷]** に移動して、デバイスの印刷設定を確認できます。

[![[印刷設定] 画面のスクリーンショットの例](kitkat-images/printing.png)](kitkat-images/printing.png#lightbox)

> [!NOTE]
> 印刷 API は既定で Google クラウド プリントと連携するように設定されていますが、Android では、開発者が新しい API を使用して印刷コンテンツを準備し、印刷を処理する他のアプリケーションに送信することもできます。

#### <a name="printing-html-content"></a>HTML コンテンツの印刷

KitKat では、`WebView.CreatePrintDocumentAdapter` を使用して Web ビューに対して [`PrintDocumentAdapter`](xref:Android.Print.PrintDocumentAdapter) が自動的に作成されます。 Web コンテンツの印刷は、HTML コンテンツの読み込みを待機し、オプション メニューで印刷オプションを使用できるようになったことを Activity に通知する [`WebViewClient`](xref:Android.Webkit.WebViewClient) と、ユーザーが印刷オプションを選択して `PrintManager` に対して `Print` を呼び出すまで待機する Activity との間の連携作業です。 このセクションでは、画面上の HTML コンテンツを印刷するために必要な基本設定について説明します。

Web コンテンツの読み込みと印刷には、インターネット アクセス許可が必要です。

[![アプリ オプションでのインターネット アクセス許可の設定](kitkat-images/internet.png)](kitkat-images/internet.png#lightbox)

##### <a name="print-menu-item"></a>印刷メニュー項目

印刷オプションは、通常、Activity の[オプション メニュー](https://developer.android.com/guide/topics/ui/menus.html#options-menu)に表示されます。
オプション メニューを使用すると、ユーザーは Activity に対してアクションを実行できます。 画面の右上隅にあり、次のような外観です。

[![画面の右上隅に表示される印刷メニュー項目のスクリーンショットの例](kitkat-images/menu.png)](kitkat-images/menu.png#lightbox)

*Resources* 以下の *menu* ディレクトリで追加のメニュー項目を定義できます。 次のコードでは、[Print](xref:Android.Print.PrintManager) というサンプル メニュー項目を定義しています。

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:id="@+id/menu_print"
        android:title="Print"
        android:showAsAction="never" />
</menu>
```

Activity のオプション メニューの操作は、`OnCreateOptionsMenu` および `OnOptionsItemSelected` メソッドを使用して行われます。
`OnCreateOptionsMenu` は、*menu* リソース ディレクトリから、印刷オプションなどの新しいメニュー項目を追加する場所です。
`OnOptionsItemSelected` では、メニューから印刷オプションを選択するユーザーをリッスンし、印刷を開始します。

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

上のコードでは、HTML コンテンツの状態を追跡する `dataLoaded` という変数も定義しています。 すべてのコンテンツが読み込まれると、`WebViewClient` によってこの変数が true に設定されるため、印刷メニュー項目をオプション メニューに追加することが Activity に認識されます。

##### <a name="webviewclient"></a>WebViewClient

`WebViewClient` の仕事は、メニューに印刷オプションが表示される前に `WebView` のデータが完全に読み込まれるようにすることです。この処理は `OnPageFinished` メソッドを使用して実行されます。 `OnPageFinished` を使用すると、Web コンテンツでの読み込みの完了をリッスンし、`InvalidateOptionsMenu` でオプション メニューを再作成するように Activity に指示することができます。

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

また、`OnPageFinished` を使用して `dataLoaded` の値を `true` に設定して、`OnCreateOptionsMenu` で印刷オプションを使用してメニューを再作成することもできます。

##### <a name="printmanager"></a>PrintManager

次のコード例では、`WebView` のコンテンツが印刷されます。

```csharp
void PrintPage ()
{
    PrintManager printManager = (PrintManager)GetSystemService (Context.PrintService);
    PrintDocumentAdapter printDocumentAdapter = myWebView.CreatePrintDocumentAdapter ();
    printManager.Print ("MyWebPage", printDocumentAdapter, null);
}
```

`Print` は、引数として、印刷ジョブの名前 (この例では "MyWebPage")、[`PrintDocumentAdapter`](xref:Android.Print.PrintDocumentAdapter)
(コンテンツから印刷ドキュメントを生成します)、[`PrintAttributes`](xref:Android.Print.PrintAttributes)
(上の例では `null`) を受け取ります。 既定の属性でほとんどのシナリオに対応できるはずですが、`PrintAttributes` を指定すると、印刷ページにコンテンツをレイアウトする際に役立ちます。

`Print` を呼び出すと、印刷 UI が読み込まれ、印刷ジョブのオプションが一覧表示されます。 次のスクリーンショットに示すように、この UI にはユーザーが HTML コンテンツを印刷または PDF に保存するオプションが用意されています。

[![[印刷] メニューが表示された PrintHtmlActivity のスクリーンショット](kitkat-images/print1.png)](kitkat-images/print1.png#lightbox)

[![[PDF として保存] メニューが表示された PrintHtmlActivity のスクリーンショット](kitkat-images/print2.png)](kitkat-images/print2.png#lightbox)

<a name="hardware"></a>

## <a name="hardware"></a>ハードウェア

KitKat では、新しいデバイス機能に対応するためにいくつかの API が追加されました。 その中でも最も注目すべき点は、ホストベースのカード エミュレーションと新しい `SensorManager` です。

### <a name="host-based-card-emulation-in-nfc"></a>NFC のホストベースのカード エミュレーション

ホストベースのカード エミュレーション (HCE) を使用すると、通信事業者固有の Secure Element に依存することなく、NFC カードまたは NFC カード リーダーのようにアプリケーションを動作させることができます。 HCE を設定する前に、デバイス上で `PackageManager.HasSystemFeature` を使用して HCE を使用できることを確認します。

```csharp
bool hceSupport = PackageManager.HasSystemFeature(PackageManager.FeatureNfcHostCardEmulation);
```

HCE を使用するには、HCE 機能と、`Nfc` アクセス許可をアプリケーションの `AndroidManifest.xml` に登録する必要があります。

```xml
<uses-feature android:name="android.hardware.nfc.hce" />
```

[![アプリ オプションでの NFC アクセス許可の設定](kitkat-images/nfc.png)](kitkat-images/nfc.png#lightbox)

正常に機能するには、HCE をバックグラウンドで実行できる必要があります。また、HCE を使用するアプリケーションが実行されていなくても、ユーザーが NFC トランザクションを作成したときに起動する必要があります。 これを実現するには、HCE コードを `Service` として記述します。 HCE サービスには、次のメソッドを実装する `HostApduService` インターフェイスが実装されています。

- *ProcessCommandApdu* - アプリケーション プロトコル データ ユニット (APDU) は、NFC リーダーと HCE サービスの間で送信されるものです。 このメソッドは、リーダーからの ADPU を消費し、応答としてデータ ユニットを返します。

- *OnDeactivated* - HCE サービスが NFC リーダーと通信しなくなると、`HostAdpuService` は非アクティブになります。

また、HCE サービスをアプリケーションのマニフェストに登録し、適切なアクセス許可、インテント フィルター、およびメタデータで修飾する必要があります。 次のコードでは、`Service` 属性を使用して Android マニフェストに登録された `HostApduService` の例です (属性の詳細については、Xamarin の [Android マニフェストの使用](~/android/platform/android-manifest.md)に関するガイドを参照してください)。

```csharp
[Service(Exported=true, Permission="android.permissions.BIND_NFC_SERVICE"),
    IntentFilter(new[] {"android.nfc.cardemulation.HOST_APDU_SERVICE"}),
    MetaData("android.nfc.cardemulation.host.apdu_service",
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

前述のサービスには、NFC リーダーがアプリケーションと対話する方法が用意されていますが、このサービスでスキャンに必要な NFC カードがエミュレートされているかどうかを知る手段が NFC リーダーにはまだありません。 NFC リーダーでサービスを識別できるように、サービスに一意の "*アプリケーション ID (AID)* " を割り当てることができます。 `MetaData` 属性を使用して登録された xml リソース ファイル (上のコード例を参照してください) で、HCE サービスに関する他のメタデータと共に、AID を指定します。 このリソース ファイルには、1 つ以上の AID フィルター (1 台以上の NFC リーダー デバイスの AID に対応する 16 進形式の一意の識別子文字列) を指定します。

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

xml リソース ファイルには、AID フィルターに加えて、HCE サービスのユーザー向けの説明も指定します。また、AID グループ (支払いアプリケーションと "その他") と、支払いアプリケーションの場合、ユーザーに表示する 260x96 dp のバナーを指定しています。

前述の設定には、NFC カードをエミュレートするアプリケーションの基本的な構成要素を指定します。 NFC には、構成が必要な手順とテストが他にもあります。 ホストベースのカード エミュレーションの詳細については、[Android ドキュメント ポータル](https://developer.android.com/guide/topics/connectivity/nfc/hce.html)を参照してください。
Xamarin で NFC を使用する方法の詳細については、[Xamarin の NFC サンプル](https://github.com/xamarin/monodroid-samples/tree/master/NfcSample)に関するページを参照してください。

### <a name="sensors"></a>Sensors

KitKat では、[`SensorManager`](xref:Android.Hardware.SensorManager) を介してデバイスのセンサーにアクセスできます。
`SensorManager` を使用すると、バッテリー寿命を維持しながら、センサー情報のアプリケーションへの一括配信を OS でスケジュールすることができます。

KitKat には、ユーザーの歩数を追跡するための 2 種類の新しいセンサーも追加されました。 これらは加速度計に基づいており、以下が含まれます。

- *StepDetector* - ユーザーが 1 歩進むと、アプリが通知を受けて起動し、検出機能によってその 1 歩が発生した時刻値が提供されます。

- *StepCounter* - センサーが登録されてから "*次のデバイスの再起動まで*" ユーザーの歩数が追跡されます。

次のスクリーンショットは、動作中の歩数カウンターを示しています。

[![歩数カウンターが表示された SensorsActivity アプリのスクリーンショット](kitkat-images/stepcounter.png)](kitkat-images/stepcounter.png#lightbox)

`GetSystemService(SensorService)` を呼び出し、結果を `SensorManager` としてキャストすることで、`SensorManager` を作成できます。 歩数カウンターを使用するには、`SensorManager` に対して `GetDefaultSensor` を呼び出します。 センサーを登録し、歩数の変化をリッスンするするには、[`ISensorEventListener`](xref:Android.Hardware.ISensorEventListener)
インターフェイスを使用します。その例を次のコード サンプルに示します。

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

アプリケーションがフォアグラウンドにあるときに歩数が更新されると、`OnSensorChanged` が呼び出されます。 アプリケーションがバックグラウンドに切り替わるか、デバイスがスリープ状態の場合は、`OnSensorChanged` は呼び出されません。ただし、`UnregisterListener` が呼び出されるまで歩数は引き続きカウントされます。

"*歩数値は、センサーを登録するすべてのアプリケーションで蓄積される*" ことに注意してください。 つまり、アプリケーションをアンインストールして再インストールし、アプリケーションの起動時に `count` 変数を 0 に初期化したとしても、センサーから報告される値は、そのアプリケーションまたは他のアプリケーションでセンサーの登録中にカウントされた歩数の合計数のままです。 次のコードに示すように、`SensorManager` に対して `UnregisterListener` を呼び出すことにより、アプリケーションで歩数カウンターに追加されないようにすることができます。

```csharp
protected override void OnPause()
{
    base.OnPause ();
    senMgr.UnregisterListener(this);
}
```

デバイスを再起動すると、歩数は 0 にリセットされます。 センサーやデバイスの状態を使用している他のアプリケーションに関係なく、アプリケーションの正確な歩数が報告されるようにするには、そのアプリに追加のコードが必要です。

> [!NOTE]
> KitKat には歩数の検出とカウント用の API が付属していますが、すべての携帯電話にセンサーが搭載されているわけではありません。 センサーを使用できるかどうかを確認するには、`PackageManager.HasSystemFeature(PackageManager.FeatureSensorStepCounter);` を実行しするか、`GetDefaultSensor` の戻り値が `null` でないことを確認します。

<a name="developer_tools"></a>

## <a name="developer-tools"></a>開発者用ツール

### <a name="screen-recording"></a>画面記録

KitKat には新しい画面記録機能が追加されたので、開発者が動作中のアプリケーションを記録できます。 画面記録は、Android SDK の一部としてダウンロードできる [Android Debug Bridge (ADB)](https://developer.android.com/tools/help/adb.html) クライアントを通じて利用できます。

画面を記録するには、デバイスを接続します。次に、Android SDK インストールを見つけて、**platform-tools** ディレクトリに移動し、**adb** クライアントを実行します。

```shell
adb shell screenrecord /sdcard/screencast.mp4
```

上のコマンドを実行すると、既定の 4 Mbps の解像度で既定の 3 分間のビデオが記録されます。 長さを編集するには、 *--time-limit* フラグを追加します。
解像度を変更するには、 *--bit-rate* フラグを追加します。 次のコマンドを実行すると、8 Mbps で 1 分間のビデオが記録されます。

```shell
adb shell screenrecord --bit-rate 8000000 --time-limit 60 /sdcard/screencast.mp4
```

ビデオはデバイス上にあります。記録が完了すると、ギャラリーに表示されます。

## <a name="other-kitkat-additions"></a>その他の KitKat の追加

前述の変更に加えて、KitKat では次のことができるようになりました。

- *全画面表示の使用* - KitKat では、全画面表示エクスペリエンスに対応するコンテンツの閲覧、ゲームのプレイ、およびその他のアプリケーションの実行のための新しい[イマーシブ モード](https://developer.android.com/reference/android/view/View.html#setSystemUiVisibility(int))が導入されました。

- *通知のカスタマイズ* - [`NotificationListenerService`](xref:Android.Service.Notification.NotificationListenerService) を使用してシステム通知に関する追加の詳細を取得できます。
  . これにより、アプリ内で別の方法で情報を表示できます。

- *ドローアブル リソースの反転* - ドローアブル リソースには新しい [`autoMirrored`](https://developer.android.com/reference/android/R.attr.html#autoMirrored)
  属性があり、これを使用して、左から右へのレイアウトの場合に、反転が必要な画像の反転バージョンを作成するようにシステムに指示できます。

- *アニメーションの一時停止* - [`Animator`](xref:Android.Animation.Animator) を使用して作成されたアニメーションの一時停止と再開を行います。
  クラスの新しいインスタンスを初期化します。

- *動的に変化するテキストの読み取り* - 新しいテキストを "ライブ領域" として動的に更新する UI の部分を示すために、新しい [`accessibilityLiveRegion`](https://developer.android.com/reference/android/R.attr.html#accessibilityLiveRegion)
  属性を使用し、アクセシビリティ モードで新しいテキストが自動的に読み込まれるようにすることができます。

- *オーディオ エクスペリエンスの強化* - [`LoudnessEnhancer`](xref:Android.Media.Audiofx.LoudnessEnhancer) を使用してトラックの音量を上げ
  、オーディオ ストリームのピークと RMS を [`Visualizer`](xref:Android.Media.Audiofx.Visualizer.MeasurementModePeakRms)
  クラスを使用して検出し、[オーディオ タイムスタンプ](xref:Android.Media.AudioTimestamp)から情報を取得し、オーディオ ビデオの同期に利用できます。

- *カスタム間隔での ContentResolver の同期* - KitKat では、同期要求が実行される時間にある程度の変動性が追加されました。 `ContentResolver.RequestSync` を呼び出して `SyncRequest` を渡すことにより、カスタムの時間または間隔で `ContentResolver` が同期されます。

- *コントローラー間の区別* - KitKat では、コントローラーにはデバイスの `ControllerNumber` プロパティからアクセスできる一意の整数識別子が割り当てられます。 これにより、ゲームのプレーヤーを区別しやすくなります。

- *リモート コントロール* - KitKat では、ハードウェア側とソフトウェア側の両方にいくつかの変更が加えられ、`ConsumerIrService` を使用して IR トランスミッターが搭載されたデバイスをリモート コントロールに変え、新しい [`RemoteController`](xref:Android.Media.RemoteController)
  API を使用して周辺機器を操作できるようになりました。

前述の API の変更の詳細については、Google の [Android 4.4 API](https://developer.android.com/about/versions/android-4.4.html) の概要を参照してください。

## <a name="summary"></a>まとめ

この記事では、Android 4.4 (API レベル 19) で使用できる新しい API の一部を紹介し、アプリケーションを KitKat に移行する際のベスト プラクティスについて説明しました。 "*遷移フレームワーク*" や "*テーマ設定*" の新しいオプションなど、ユーザー エクスペリエンスに影響する API の変更の概要を説明しました。 次に、"*ストレージ アクセス フレームワーク*"、`DocumentsProvider` クラス、新しい*印刷 API* について紹介しました。 "*NFC ホストベースのカード エミュレーション*" と、ユーザーの歩数を追跡するための 2 つの新しいセンサーを含む "*低電力センサー*" の操作方法について説明しました。 最後に、*画面記録*を使用してアプリケーションのリアルタイム デモをキャプチャする例と、KitKat API の変更点と追加の詳細な一覧を示しました。

## <a name="related-links"></a>関連リンク

- [KitKat サンプル](https://docs.microsoft.com/samples/xamarin/monodroid-samples/kitkat)
- [Android 4.4 API](https://developer.android.com/about/versions/android-4.4.html)
- [Android KitKat](https://developer.android.com/about/versions/kitkat.html)
