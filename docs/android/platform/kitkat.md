---
title: KitKat 機能
description: Android 4.4 (KitKat) は、ユーザーおよび開発者向け機能学ばなければで読み込まれています。 このガイドでは、これらの機能のいくつかを強調表示し、コード例と KitKat を最大限に活用するための実装の詳細を提供します。
ms.prod: xamarin
ms.assetid: D3FDEA1C-F076-406F-BCC3-2A55D2C6ADEE
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: 9572a08a2403bf13d74fc5fda7c62ec1fb2d1537
ms.sourcegitcommit: 58d8bbc19ead3eb535fb8248710d93ba0892e05d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/09/2019
ms.locfileid: "67674703"
---
# <a name="kitkat-features"></a>KitKat 機能

_Android 4.4 (KitKat) は、ユーザーおよび開発者向け機能学ばなければで読み込まれています。このガイドでは、これらの機能のいくつかを強調表示し、コード例と KitKat を最大限に活用するための実装の詳細を提供します。_

## <a name="overview"></a>概要

Android 4.4 (API レベル 19) とも呼ばれます"KitKat"は、遅延 2013 でリリースされました。 KitKat は、さまざまな新機能および強化などを提供します。

-  [ユーザー エクスペリエンス](#user_experience)&ndash;簡単なアニメーション遷移フレームワーク、半透明の状態とナビゲーション バー、全画面表示のイマーシブ モードとユーザーのエクスペリエンスを向上させるを作成するのに役立ちます。

-  [ユーザーがコンテンツ](#user_content)&ndash;ストレージ アクセス フレームワークを簡単にユーザー ファイルの管理は、画像の印刷、web サイト、およびその他のコンテンツが強化された印刷 Api で、簡単にします。

-  [ハードウェア](#hardware) &ndash; NFC カード エミュレーションのホスト ベースの NFC カードにすべてのアプリを有効にする; を低電力センサーの実行、`SensorManager`します。

-  [開発者ツール](#developer_tools)&ndash;スクリーン キャストのアプリケーションで利用できる Android Debug Bridge クライアント、Android SDK の一部として。


このガイドでは、Xamarin.Android 開発者向け KitKat の大まかな概要に加えて、KitKat に既存の Xamarin.Android アプリケーションを移行するためのガイダンスを提供します。

## <a name="requirements"></a>必要条件

KitKat を使用して Xamarin.Android アプリケーションを開発する必要があります*Xamarin.Android 4.11.0*または高い、Android 4.4 (API レベル 19) 次のスクリーン ショットに示すように、Android SDK Manager を使用してをインストールします。

[![Android SDK Manager で Android 4.4 を選択します。](kitkat-images/api19.png)](kitkat-images/api19.png#lightbox)

<a name="Migrating_Your_App_to_KitKat" />

## <a name="migrating-your-app-to-kitkat"></a>アプリは、KitKat への移行

このセクションでは、Android 4.4 に既存のアプリケーションを移行するには、いくつか最初応答項目を提供します。

### <a name="check-system-version"></a>システムのバージョンを確認してください。

Android の以前のバージョンと互換性があるアプリケーションに必要な場合は、次のコード例に示すように、システムのバージョン チェックですべて KitKat 固有のコードをラップしてください。

```csharp
if (Build.VERSION.SdkInt >= BuildVersionCodes.Kitkat) {
    //KitKat only code here
}
```

### <a name="alarm-batching"></a>アラームがバッチ処理

Android では、アラームのサービスを使用して、バック グラウンドでアプリを指定された時間にスリープ解除します。 KitKat は、この手順さらに電源を保持するためにアラームをバッチ処理します。 つまり、KitKat はアプリごとの正確な時刻にウェイク、代わりに、同じ期間にスリープ解除し、それらを同時にスリープ解除する登録されているいくつかのアプリケーションをグループ化するが優先されます。
指定した時間間隔中にアプリをウェイクするための Android を指示するには、呼び出す`SetWindow`上、 [ `AlarmManager`](https://developer.xamarin.com/api/type/Android.App.AlarmManager/)最小長、最大時間 (ミリ秒)、アプリがウェイク アップする前に経過時間、および実行する操作を渡して、でウェイク アップします。
次のコードでは、30 分と、ウィンドウの設定時間から 1 時間の間ウェイクする必要があるアプリケーションの例を示します。

```csharp
AlarmManager alarmManager = (AlarmManager)GetSystemService(AlarmService);
alarmManager.SetWindow (AlarmType.Rtc, AlarmManager.IntervalHalfHour, AlarmManager.IntervalHour, pendingIntent);
```

アプリを正確な時間にスリープ解除を続行するには使用`SetExact`では、アプリは、ウェイク アップする必要があります、正確な時間および実行する操作を渡して。

```csharp
alarmManager.SetExact (AlarmType.Rtc, AlarmManager.IntervalDay, pendingIntent);
```

不要になった KitKat では、正確な繰り返しのアラームを設定することができます。 使用するアプリケーション [`SetRepeating`](https://developer.xamarin.com/api/member/Android.App.AlarmManager.SetRepeating/p/Android.App.AlarmType/System.Int64/System.Int64/Android.App.PendingIntent/)
正確なアラームを手動で各アラームをトリガーする必要はの機能を必要とします。

### <a name="external-storage"></a>外部ストレージ

外部ストレージは、2 種類のアプリケーション、および複数のアプリケーションで共有データに固有の記憶域に分けられますようになりました。 読み取りと書き込みの外部ストレージにアプリの特定の場所には、特殊なアクセス許可必要ありません。 これで共有記憶域上のデータと対話する必要があります、`READ_EXTERNAL_STORAGE`または`WRITE_EXTERNAL_STORAGE`権限。 そのため、2 つの種類に分類できます。

-  メソッドを呼び出して、ファイルまたはディレクトリのパスを取得している場合`Context`- たとえば、 [`GetExternalFilesDir`](https://developer.xamarin.com/api/member/Android.Content.Context.GetExternalFilesDir/p/System.String/)
   または [`GetExternalCacheDirs`](https://developer.xamarin.com/api/member/Android.Content.Context.GetExternalCacheDirs%28%29/)
   - アプリには、追加のアクセス許可は不要です。

-  プロパティへのアクセスまたはでメソッドを呼び出すことによってファイルまたはディレクトリのパスを取得している場合`Environment`など [`GetExternalStorageDirectory`](https://developer.xamarin.com/api/property/Android.OS.Environment.ExternalStorageDirectory/)
   または [`GetExternalStoragePublicDirectory`](https://developer.xamarin.com/api/member/Android.OS.Environment.GetExternalStoragePublicDirectory/p/System.String/)
   、アプリが必要、`READ_EXTERNAL_STORAGE`または`WRITE_EXTERNAL_STORAGE`権限。

> [!NOTE]
> `WRITE_EXTERNAL_STORAGE` 意味、`READ_EXTERNAL_STORAGE`しかを 1 つのアクセス許可を設定する必要があるため、アクセス許可。

### <a name="sms-consolidation"></a>SMS の統合

KitKat は、ユーザーが選択した 1 つの既定のアプリケーションで SMS コンテンツをすべて集約することによって、ユーザーのメッセージ処理を簡略化します。 開発者は、既定のメッセージング アプリケーションとして選択可能なアプリできるようにし、コードと有効期間に、アプリケーションが選択されていない場合は、適切に動作します。 KitKat に SMS アプリの移行に関する詳細についてを参照してください、 [Your SMS アプリの準備 KitKat](http://android-developers.blogspot.com/2013/10/getting-your-sms-apps-ready-for-kitkat.html) Google からのガイド。

### <a name="webview-apps"></a>WebView アプリ

[WebView](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) KitKat で、作り直しを取得します。 最大の変更にコンテンツを読み込むためのセキュリティを追加、`WebView`します。 以前の API バージョンを対象とするほとんどのアプリケーションが期待どおりに機能、中にテストを使用するアプリケーション、`WebView`クラスは強くお勧めします。 WebView の影響を受ける Api の詳細については、Android を参照してください[Android 4.4 内の web ビューへの移行](https://developer.android.com/guide/webapps/migrating.html)ドキュメント。

<a name="user_experience" />

## <a name="user-experience"></a>ユーザー体験

KitKat は、プロパティのアニメーションとテーマの半透明の UI オプションを処理するための新しい移行フレームワークを含む、ユーザー エクスペリエンスを強化するためにいくつかの新しい Api が付属します。 これらの変更について取り上げます。

### <a name="transition-framework"></a>移行フレームワーク

移行フレームワークは、実装が簡単にアニメーションを使用します。 KitKat では、1 つの行のコードでは、シンプル プロパティのアニメーションを実行したりを使用して遷移をカスタマイズできます。*シーン*します。

#### <a name="simple-property-animation"></a>シンプル プロパティのアニメーション

新しい Android 遷移ライブラリには、プロパティのアニメーションの背後にあるコードが簡略化します。 フレームワークでは、最小限のコードで単純なアニメーションを実行できます。 たとえば、次のコード サンプルを使用します。 [`TransitionManager.BeginDelayedTransition`](https://developer.xamarin.com/api/member/Android.Transitions.TransitionManager.BeginDelayedTransition/p/Android.Views.ViewGroup/Android.Transitions.Transition/)
表示と非表示をアニメーション化する、 `TextView`:

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

上記の例では、移行フレームワークを使用して、自動プロパティの値が変更の間の既定の切り替え効果を作成します。 アニメーションが 1 行のコードによって処理されるため、行うことができます簡単にこれ以前のバージョンの Android との互換性をラップして、`BeginDelayedTransition`システムのバージョン チェックで呼び出します。 参照してください、 [Your KitKat アプリに移行する](#Migrating_Your_App_to_KitKat)詳細セクション。

次のスクリーン ショットは、アニメーションの前にアプリを示しています。

[![アニメーションの開始前に、アプリのスクリーン ショット](kitkat-images/trans-before.png)](kitkat-images/trans-before.png#lightbox)

次のスクリーン ショットは、アニメーションの終了後、アプリを示しています。

[![アニメーションの完了後にアプリのスクリーン ショット](kitkat-images/trans-after.png)](kitkat-images/trans-after.png#lightbox)

詳細については、次のセクションのシーンを使用した移行に制御を取得できます。

#### <a name="android-scenes"></a>Android のシーン

[シーン](https://developer.xamarin.com/api/type/Android.Transitions.Scene/)アニメーションの詳細に制御を開発者に提供するために移行フレームワークの一部として導入されました。 シーンでは、UI の動的な領域を作成します。 コンテナーといくつかのバージョンでは、またはコンテナーの内部 XML コンテンツを"シーン"を指定すると、Android は、残りのシーン間の遷移をアニメーション化する作業の。 Android のシーンでは、開発側で最低限の作業を複雑なアニメーションを作成できます。

動的なコンテンツを格納している静的の UI 要素は、呼び出された、*コンテナー*または*基本シーン*します。 次の例では、Android Designer を使用して、作成、`RelativeLayout`と呼ばれる`container`:

[![Android Designer を使用して、[相対レイアウト] コンテナーを作成するには](kitkat-images/container.png)](kitkat-images/container.png#lightbox)

サンプル レイアウトは、という名前のボタンも定義されています。`sceneButton`下、`container`します。 このボタンは、遷移をトリガーします。

コンテナー内で動的なコンテンツには、2 つの新しい Android レイアウトが必要です。 これらのレイアウトがコードのみを指定*内*コンテナー。
次のサンプル コードと呼ばれるレイアウトを定義します。 *Scene1* 2 つの"キット"を読み取り、テキスト フィールドと"Kat"をそれぞれ含むと 2 番目のレイアウトと呼ばれる*Scene2*逆に、同じテキスト フィールドを格納しています。 XML は次のとおりです。

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

使用して上記の例`merge`を短いコードの表示を作成し、階層の表示を簡略化します。 詳細をご覧ください`merge`レイアウト[ここ](http://android-developers.blogspot.com/2009/03/android-layout-tricks-3-optimize-by.html)します。

シーンが呼び出すことによって作成された[ `Scene.GetSceneForLayout`](https://developer.xamarin.com/api/member/Android.Transitions.Scene.GetSceneForLayout/p/Android.Views.ViewGroup/System.Int32/Android.Content.Context/)コンテナー オブジェクト、シーンのレイアウトのファイルと現在のリソース ID を渡して、`Context`次のコード例に示しますように。

```csharp
RelativeLayout container = FindViewById<RelativeLayout> (Resource.Id.container);

Scene scene1 = Scene.GetSceneForLayout(container, Resource.Layout.Scene1, this);
Scene scene2 = Scene.GetSceneForLayout(container, Resource.Layout.Scene2, this);

scene1.Enter();
```

既定の切り替えの値を持つ Android をアニメーション化の 2 つのシーン間反転 ボタンをクリックすると。

```csharp
sceneButton.Click += (o, e) => {
    Scene temp = scene2;
    scene2 = scene1;
    scene1 = temp;

    TransitionManager.Go (scene1);
};
```

次のスクリーン ショットは、アニメーション前にシーンを示しています。

[![アニメーションの開始前に、アプリのスクリーン ショット](kitkat-images/trans-after.png)](kitkat-images/trans-after.png#lightbox)

次のスクリーン ショットでは、アニメーションの終了後、シーンを示しています。

[![アニメーションの終了後、アプリのスクリーン ショット](kitkat-images/scene.png)](kitkat-images/scene.png#lightbox)


> [!NOTE]
> [既知のバグ](https://code.google.com/p/android/issues/detail?id=62450)Android の遷移のシーンを原因となるライブラリを使用して作成`GetSceneForLayout`2 回目、ユーザーのアクティビティ間を移動するときに中断します。 Java の回避策が記載されている[ここ](http://www.doubleencore.com/2013/11/new-transitions-framework/)します。


##### <a name="custom-transitions-in-scenes"></a>シーン内のカスタムの移行

内の xml リソース ファイルでカスタム遷移を定義することができます、`transition`ディレクトリ`Resources`次のスクリーン ショットに示すようにします。

[![リソースの移行/ディレクトリの下の transition.xml ファイルの場所](kitkat-images/resources.png)](kitkat-images/resources.png#lightbox)

次のコード サンプルは、5 秒間アニメーション化を使用して切り替えを定義、[インターポレーターをしまった](https://developer.android.com/reference/android/views/animation/OvershootInterpolator.html):

```xml
<changeBounds
  xmlns:android="http://schemas.android.com/apk/res/android"
  android:duration="5000"
  android:interpolator="@android:anim/overshoot_interpolator" />
```

アクティビティの遷移が作成されたを使用して、 [TransitionInflater](https://developer.xamarin.com/api/type/Android.Transitions.TransitionInflater/)次のコードに示しますように。

```csharp
Transition transition = TransitionInflater.From(this).InflateTransition(Resource.Transition.transition);
```

新しい移行に追加し、`Go`アニメーションが開始される呼び出し。

```csharp
TransitionManager.Go (scene1, transition);
```

### <a name="translucent-ui"></a>半透明の UI

KitKat をより細かく制御テーマ オプションの半透明の状態とナビゲーション バーを使用してアプリを示しています。 使用して、Android のテーマを定義する、同じ XML ファイル内のシステム UI 要素の透明度を変更することができます。 KitKat には、次のプロパティが導入されています。

-  `windowTranslucentStatus` -に設定すると true は、上部のステータス バー半透明です。

-  `windowTranslucentNavigation` -に設定すると true は、下部にあるナビゲーション バー半透明です。

-  `fitsSystemWindows` -上部または下部のバーを transcluent に設定すると、既定で透過的な UI 要素の下にコンテンツがシフトします。 このプロパティを設定`true`コンテンツが半透明のシステムの UI 要素を持つ重複するを防ぐために簡単な方法です。


次のコードでは、半透明の状態とナビゲーション バーでテーマを定義します。

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

次のスクリーン ショットは、半透明の状態、ナビゲーション バーの上のテーマを示しています。

[![半透明の状態とナビゲーション バーを使用したアプリのスクリーン ショットの例](kitkat-images/theme.png)](kitkat-images/theme.png#lightbox)

<a name="user_content" />

## <a name="user-content"></a>ユーザー コンテンツ

### <a name="storage-access-framework"></a>ストレージ アクセス フレームワーク

ストレージ アクセス フレームワーク (SAF) は、イメージ、ビデオ、ドキュメントなどの格納されているコンテンツと対話するユーザーの新しい方法です。 コンテンツを処理するためにアプリケーションを選択するダイアログ ボックスでユーザーを表示するのではなく KitKat は、データ集計の 1 つの場所にアクセスするユーザーを許可する新しい UI が開きます。 コンテンツが選択されているし、ユーザーは、コンテンツを要求したアプリケーションに戻りますアプリ エクスペリエンスを通常どおりに続行されます。

この変更には、開発者側で 2 つのアクションが必要です。 プロバイダーからのコンテンツを必要とするアプリが最初に、要求元のコンテンツの新しい方法を更新する必要があります。 データを書き込む、2 つ目のアプリケーション、`ContentProvider`新しいフレームワークを使用するように変更する必要があります。 どちらのシナリオは、新しいに依存します。 [`DocumentsProvider`](https://developer.xamarin.com/api/type/Android.Provider.DocumentsProvider/)
[API]。

#### <a name="documentsprovider"></a>DocumentsProvider

KitKat との対話で`ContentProviders`で抽象化は、`DocumentsProvider`クラス。 つまり、ある SAF は関係ありませんデータが物理的には、どこからアクセスできる限り、 `DocumentsProvider` API。 ローカル プロバイダーは、クラウド サービス、および外部記憶装置インターフェイスは同じで、同じ方法で処理がすべての使用、ユーザーのコンテンツと対話する 1 つの場所で、ユーザーと開発者を提供することです。

このセクションでは、読み込み、記憶域アクセス フレームワークでコンテンツを保存する方法について説明します。

#### <a name="request-content-from-a-provider"></a>コンテンツ プロバイダーからの要求します。

わかります KitKat SAF で UI を使用してコンテンツを選択すること、`ActionOpenDocument`目的で、デバイスで使用できるすべてのコンテンツ プロバイダーに接続することを示します。 いくつかフィルター処理をこの目的を指定して追加できます`CategoryOpenable`、つまり、(つまりアクセス、利用可能なコンテンツ) を開くことができる唯一のコンテンツが返されます。 KitKat が、コンテンツをフィルター処理を許可しても、`MimeType`します。 イメージを指定することによってイメージの結果をフィルター次のコード例では、 `MimeType`:

```csharp
Intent intent = new Intent (Intent.ActionOpenDocument);
intent.AddCategory (Intent.CategoryOpenable);
intent.SetType ("image/*");
StartActivityForResult (intent, save_request_code);
```

呼び出す`StartActivityForResult`ユーザー イメージの選択を参照できます、SAF UI を起動します。

[![ストレージ アクセス フレームワーク イメージへの参照を使用するアプリのスクリーン ショットの例](kitkat-images/saf-ui.png)](kitkat-images/saf-ui.png#lightbox)

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

SAF UI からのコンテンツの読み込み、だけでなく KitKat することもできますいずれかにコンテンツを保存`ContentProvider`を実装する、 `DocumentProvider` API。 コンテンツの使用を保存、`Intent`で`ActionCreateDocument`:

```csharp
Intent intentCreate = new Intent (Intent.ActionCreateDocument);
intentCreate.AddCategory (Intent.CategoryOpenable);
intentCreate.SetType ("text/plain");
intentCreate.PutExtra (Intent.ExtraTitle, "NewDoc");
StartActivityForResult (intentCreate, write_request_code);
```

上記のコード サンプルでは、ユーザーがファイル名を変更し、新しいファイルを格納するディレクトリを選択できるようにすること、SAF UI を読み込みます。

[![NewDoc をダウンロード ディレクトリにファイル名を変更するユーザーのスクリーン ショット](kitkat-images/saf-save.png)](kitkat-images/saf-save.png#lightbox)

押されたとき**保存**、`OnActivityResult`が渡される、`Android.Net.Uri`アクセスするのには、新しく作成されたファイルの`data.Data`します。 新しいファイルにデータをストリーミングする uri を使用できます。

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

注意してください。 [`ContentResolver.OpenOutputStream(Android.Net.Uri)`](https://developer.xamarin.com/api/member/Android.Content.ContentResolver.OpenOutputStream/(Android.Net.Uri))
返します、`System.IO.Stream`ので、ストリーミング プロセス全体を .NET で記述できます。

読み込みの詳細については、作成、およびストレージ アクセス フレームワークでのコンテンツの編集を参照してください、[ストレージ アクセス フレームワークの Android ドキュメント](https://developer.android.com/guide/topics/providers/document-provider.html)します。

### <a name="printing"></a>印刷

印刷コンテンツはの導入に伴い KitKat で簡素化され、[印刷サービス](https://developer.xamarin.com/api/namespace/Android.PrintServices/)と`PrintManager`します。 KitKat がフル活用するための最初の API バージョンではまた、 [Google のクラウドの印刷サービス Api](https://developers.google.com/cloud-print/)を使用して、 [Google Cloud Print アプリケーション](https://play.google.com/store/apps/details?id=com.google.android.apps.cloudprint)します。
KitKat を自動的に付属するほとんどのデバイスが Google クラウド印刷のアプリをダウンロードし、 [HP 印刷サービス プラグイン](https://play.google.com/store/apps/details?id=com.hp.android.printservice)WiFi に最初に接続するとき。 移動して、ユーザーがデバイスの印刷設定を確認できます**設定 > システム > 印刷**:

[![印刷設定 画面のスクリーン ショットの例](kitkat-images/printing.png)](kitkat-images/printing.png#lightbox)


> [!NOTE]
> 印刷 Api は、既定で Google Cloud の印刷を使用する設定は、Android は、開発者は新しい Api を使用して印刷コンテンツを準備し、印刷処理を行うには、他のアプリケーションに送信をもできます。



#### <a name="printing-html-content"></a>HTML コンテンツの印刷

KitKat は自動的に作成、 [ `PrintDocumentAdapter` ](https://developer.xamarin.com/api/type/Android.Print.PrintDocumentAdapter/) web ビューを`WebView.CreatePrintDocumentAdapter`します。 Web コンテンツを印刷が連携して業務を[ `WebViewClient` ](https://developer.xamarin.com/api/type/Android.Webkit.WebViewClient/)する HTML コンテンツを読み込むを待機し、印刷オプションを [オプション] メニューで使用できるようにする、アクティビティと、アクティビティは、ユーザーが待機することができます[印刷] オプションと呼び出しを選択`Print`上、`PrintManager`します。 ここでは、画面上を印刷するために必要な基本的なセットアップの HTML コンテンツ。

読み込みと web コンテンツの印刷がインターネット アクセス許可が必要であることを確認してください。

[![アプリケーションのオプションでのインターネット アクセス許可の設定](kitkat-images/internet.png)](kitkat-images/internet.png#lightbox)

##### <a name="print-menu-item"></a>印刷メニュー項目

アクティビティの通常表示される印刷オプション[オプション メニュー](https://developer.android.com/guide/topics/ui/menus.html#options-menu)します。
[オプション] メニューを使用して、アクティビティに対して操作を実行できます。 画面の右上隅にあると、次のような。

[![画面の右上隅に表示される印刷メニュー項目の例のスクリーン ショット](kitkat-images/menu.png)](kitkat-images/menu.png#lightbox)


追加のメニュー項目を定義することができます、*メニュー*ディレクトリ*リソース*します。 次のコード サンプル メニュー項目を定義する[印刷](https://developer.xamarin.com/api/type/Android.Print.PrintManager/):

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:id="@+id/menu_print"
        android:title="Print"
        android:showAsAction="never" />
</menu>
```

を介して行われ、アクティビティの [オプション] メニューとの対話、`OnCreateOptionsMenu`と`OnOptionsItemSelected`メソッド。
`OnCreateOptionsMenu` 印刷オプションのように、新しいメニュー項目を追加する場所、*メニュー* resources ディレクトリ。
`OnOptionsItemSelected` メニューから、[印刷] オプションを選択すると、ユーザーをリッスンしの印刷を開始します。

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

上記のコードでは、という名前の変数も定義します`dataLoaded`HTML コンテンツのステータスを追跡します。 `WebViewClient`オプション] メニューに [印刷メニュー項目を追加するアクティビティが認識できるようにすべてのコンテンツが読み込まれると、true には、この変数を設定します。

##### <a name="webviewclient"></a>WebViewClient

ジョブ、 `WebViewClient` 、内のデータを確認すること、`WebView`では、メニューに表示される印刷オプションを選択する前に完全に読み込まれるが、`OnPageFinished`メソッド。 `OnPageFinished` web コンテンツの読み込み、[完了] をリッスンし、そのオプション メニューを再作成するアクティビティに指示`InvalidateOptionsMenu`:

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

`Print` 引数として受け取ります ("MyWebPage"この例では)、印刷ジョブの名前を。 [`PrintDocumentAdapter`](https://developer.xamarin.com/api/type/Android.Print.PrintDocumentAdapter/)
コンテンツから印刷ドキュメントを生成して [`PrintAttributes`](https://developer.xamarin.com/api/type/Android.Print.PrintAttributes/)
(`null`上記の例では)。 指定できる`PrintAttributes`に印刷 ページで、コンテンツのレイアウトの役立つわけでは、既定の属性は、ほとんどのシナリオを処理する必要があります。

呼び出す`Print`印刷の UI は、印刷ジョブのオプションの一覧を読み込みます。 UI によって、次のスクリーン ショットに示すように、ユーザーの印刷または pdf、HTML コンテンツを保存のオプション。

[![[印刷] メニューを表示する PrintHtmlActivity のスクリーン ショット](kitkat-images/print1.png)](kitkat-images/print1.png#lightbox)

[![PrintHtmlActivity のスクリーン ショットは、保存を PDF メニューとして表示します。](kitkat-images/print2.png)](kitkat-images/print2.png#lightbox)

<a name="hardware" />

## <a name="hardware"></a>ハードウェア

KitKat は、新しいデバイス機能に対応するいくつかの Api を追加します。 これらの最も重要なのホスト ベースのカードのエミュレーションと新しい`SensorManager`します。

### <a name="host-based-card-emulation-in-nfc"></a>NFC のホスト ベースのカードのエミュレーション

ホスト ベース カード エミュレーション (HCE) には、通信事業者の専用のセキュリティで保護された要素に頼ることがなく NFC カードまたはカード リーダーの NFC ように動作するアプリケーションができます。 HCE をセットアップする前に HCE がデバイスで使用できることを確認します`PackageManager.HasSystemFeature`:

```csharp
bool hceSupport = PackageManager.HasSystemFeature(PackageManager.FeatureNfcHostCardEmulation);
```

HCE である必要があります HCE 機能と`Nfc`アプリケーションのアクセス許可に登録する`AndroidManifest.xml`:

```xml
<uses-feature android:name="android.hardware.nfc.hce" />
```

[![アプリケーションのオプションの NFC アクセス許可の設定](kitkat-images/nfc.png)](kitkat-images/nfc.png#lightbox)

作業をする、HCE が、バック グラウンドで実行できる必要があり、HCE を使用して、アプリケーションが実行されていない場合でも、ユーザーが、NFC トランザクションを開始する必要があります。 として HCE コードを記述してためここには、`Service`します。 HCE サービスを実装、`HostApduService`インターフェイスで、次のメソッドを実装します。

-  *ProcessCommandApdu* -An アプリケーション プロトコル データ ユニット (APDU) は、NFC のリーダーと HCE サービス間で送信内容を取得します。 このメソッドは、リーダーから、ADPU を消費し、応答内のデータ ユニットを返します。

-  *OnDeactivated* - `HostAdpuService` HCE サービスが NFC リーダーと通信する不要になったときは、非アクティブ化します。


HCE サービスも、アプリケーションのマニフェストを登録し、適切なアクセス許可、インテント フィルター、およびメタデータで装飾する必要があります。 次のコードの例は、 `HostApduService` Android マニフェストを使用して登録されている、`Service`属性 (属性の詳細については、Xamarin を参照してください[Android マニフェスト操作](~/android/platform/android-manifest.md)ガイド)。

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

上記のサービス、アプリケーションと対話する NFC リーダーの手段を提供が NFC リーダーはこのサービスをエミュレートして、NFC のカードをスキャンする必要がある場合を知る方法はありませんが。 NFC リーダーがサービスを識別するために、割り当てることができます、サービスを一意*アプリケーション ID (AID)* します。 HCE サービスに関するその他のメタデータと共に、参考情報を指定する xml リソース ファイル内で登録、`MetaData`属性 (上記のコード例を参照してください)。 このリソース ファイルは、1 つまたは複数 AID フィルターの 1 つ以上の NFC リーダー デバイスの補助に対応する 16 進形式で一意の識別子の文字列を指定します。

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

ヘルパーのフィルターだけでなく補助グループ ("other"と支払いアプリケーション) を指定 xml リソース ファイルも HCE サービスのユーザーに表示される説明を提供し、支払いのアプリケーションでは、260 x 96 dp バナーをユーザーに表示します。

上記で説明したセットアップは、NFC のカードをエミュレートするアプリケーションの基本的な構成要素を提供します。 NFC 自体では、さらにいくつかの手順とさらにテストを構成する必要があります。 カード エミュレーションのホスト ベースの詳細についてを参照してください、 [Android ドキュメント ポータル](https://developer.android.com/guide/topics/connectivity/nfc/hce.html)します。
NFC を使用して、Xamarin での詳細については、チェック アウト、 [Xamarin NFC サンプル](https://github.com/xamarin/monodroid-samples/tree/master/NfcSample)します。

### <a name="sensors"></a>センサー

KitKat を通じて、デバイスのセンサーへのアクセスを提供する、 [ `SensorManager`](https://developer.xamarin.com/api/type/Android.Hardware.SensorManager/)します。
`SensorManager`バッテリの寿命を維持し、バッチ内のアプリケーションにセンサーの情報の配信をスケジュールする OS を使用します。

KitKat も付属して 2 つの新しい種類のセンサーのユーザーの手順を追跡します。 これらは、加速度計に基づいておりが含まれます。

-  *StepDetector* -アプリは通知/ウェイク ユーザーが、ステップと、検出機能は、手順が発生したときの時刻の値を提供します。

-  *StepCounter* -のセンサーが登録されているために、ユーザーが行われたステップの数を追跡*次のデバイスの再起動まで*します。

次のスクリーン ショットは、アクションの手順のカウンターを示しています。

[![手順のカウンターを表示する SensorsActivity アプリのスクリーン ショット](kitkat-images/stepcounter.png)](kitkat-images/stepcounter.png#lightbox)

作成することができます、`SensorManager`呼び出して`GetSystemService(SensorService)`として結果をキャストし、 `SensorManager`。 手順のカウンターを使用する呼び出し`GetDefaultSensor`上、`SensorManager`します。 センサーを登録およびのカウントのステップで変更をリッスンする、 [`ISensorEventListener`](https://developer.xamarin.com/api/type/Android.Hardware.ISensorEventListener/)
次のコード例に示すようには、インターフェイス:

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

`OnSensorChanged` アプリケーションがフォア グラウンドでは、ステップ カウントが更新される場合と呼びます。 アプリケーションがバック グラウンドを入力またはデバイスがスリープ状態の場合`OnSensorChanged`は呼び出されません。 ただし、手順は引き続きまでカウントする`UnregisterListener`が呼び出されます。

注意*センサーを登録するすべてのアプリケーション間でステップ数の値は累積的な*します。 つまりをアンインストールし、アプリケーションを再インストールして初期化する場合でも、`count`アプリケーションの起動時に 0 にある変数をセンサーで感知された値は、センサーが登録されているときに行われたステップの総数のままはによってかどうか、アプリケーションまたは別にします。 アプリケーションから呼び出すことによって、手順カウンターへの追加を防ぐことができます`UnregisterListener`上、`SensorManager`次のコードに示しますように。

```csharp
protected override void OnPause()
{
    base.OnPause ();
    senMgr.UnregisterListener(this);
}
```

デバイスの再起動カウントのステップを 0 にリセットします。 アプリには、センサーやデバイスの状態を使用して他のアプリケーションに関係なく、アプリケーションの正確なカウントを報告していることを確認する追加コードが必要です。


> [!NOTE]
> 手順検出および KitKat 付属のカウントについて、API では、中にすべてのスマート フォンで、センサーはあり。 実行して、センサーが使用可能なかどうかをチェックする`PackageManager.HasSystemFeature(PackageManager.FeatureSensorStepCounter);`の戻り値のことを確認するか、または`GetDefaultSensor`いない`null`します。


<a name="developer_tools" />

## <a name="developer-tools"></a>開発者用ツール

### <a name="screen-recording"></a>画面記録

KitKat には、録音機能を開発者がアクションでアプリケーションを記録できるように新しい画面が含まれています。 画面記録を利用、 [Android Debug Bridge (ADB)](https://developer.android.com/tools/help/adb.html)クライアントで、Android SDK の一部としてダウンロードできます。

画面を記録するにはデバイスを接続します。次に、Android SDK のインストールを検索に移動し、**プラットフォーム ツール**ディレクトリと実行、 **adb**クライアント。

```shell
adb shell screenrecord /sdcard/screencast.mp4
```

上記のコマンドでは、4 mbps の既定の解像度で既定値 3 分間のビデオを記録します。 長さを編集するには、追加、 *-時間制限*フラグ。
解像度を変更するには、追加、 *--ビット レート*フラグ。 次のコマンドが 8Mbps で 1 分間のビデオを記録されます。

```shell
adb shell screenrecord --bit-rate 8000000 --time-limit 60 /sdcard/screencast.mp4
```

デバイスでビデオを見つけることができます - 記録が完了すると、ギャラリーに表示されます。


## <a name="other-kitkat-additions"></a>その他の KitKat の追加

上記の変更だけでなく KitKat ことができます。

-  *全画面表示を使用して、* -KitKat が導入されていますが、新しい[イマーシブ モード](https://developer.android.com/reference/android/view/View.html#setSystemUiVisibility(int))のコンテンツを参照する、ゲーム、および全画面エクスペリエンスから利点がもたらされる他のアプリケーションを実行します。

-  *通知をカスタマイズする*-を使用したシステム通知に関する追加情報を取得します [`NotificationListenerService`](https://developer.xamarin.com/api/type/Android.Service.Notification.NotificationListenerService/)
   . これにより、アプリ内の別の方法で情報を提供できます。

-  *描画可能なリソースをミラー化*-描画可能なリソースが新しいがあります。 [`autoMirrored`](https://developer.android.com/reference/android/R.attr.html#autoMirrored)
   システムに通知する属性は、左から右のレイアウトの反転を必要とするイメージのミラー化されたバージョンを作成します。

-  *アニメーションを一時停止*-一時停止と再開のアニメーションを使用して作成します [`Animator`](https://developer.xamarin.com/api/type/Android.Animation.Animator/)
   クラスの新しいインスタンスを初期化します。

-  *動的に変更するテキストの読み取り*-新しい「ライブ リージョン」として、新しいテキストに動的に更新する UI の部分を示す [`accessibilityLiveRegion`](https://developer.android.com/reference/android/R.attr.html#accessibilityLiveRegion)
   新しいテキストはアクセシビリティ モードで自動的に読み取るための属性です。

-  *オーディオのエクスペリエンスの向上*-Make トラック音は、 [`LoudnessEnhancer`](https://developer.xamarin.com/api/type/Android.Media.Audiofx.LoudnessEnhancer/)
   、ピーク時およびオーディオのストリームの RMS を検索します [`Visualizer`](https://developer.xamarin.com/api/field/Android.Media.Audiofx.Visualizer.MeasurementModePeakRms/)
   クラス、および情報の取得、[オーディオ タイムスタンプ](https://developer.xamarin.com/api/type/Android.Media.AudioTimestamp/)オーディオ ビデオ同期を支援します。

-  *カスタムの間隔で ContentResolver を同期*-KitKat 同期要求が実行された時刻にいくつかの可変性を追加します。 同期、`ContentResolver`でカスタムの時刻または間隔を呼び出して`ContentResolver.RequestSync`を渡して、`SyncRequest`します。

-  *コント ローラーの間で識別*-KitKat では、コント ローラーは、デバイスの経由でアクセスできる一意の整数識別子を割り当てられた`ControllerNumber`プロパティ。 これにより、簡単にゲームに離れているプレーヤーに伝えます。

-  *リモート コントロール*-ハードウェアとソフトウェアの両方の側でのいくつかの変更により、KitKat を使用すると、リモート_コントロールを使用してに IR の送信機能であり、デバイスを有効にする、 `ConsumerIrService`、および新しい周辺機器の対話 [`RemoteController`](https://developer.xamarin.com/api/type/Android.Media.RemoteController/)
   Api。

上記の API の変更点の詳細については、Google を参照してください[Android 4.4 Api](https://developer.android.com/about/versions/android-4.4.html)の概要。


## <a name="summary"></a>まとめ

この記事では、一部の Android 4.4 (API レベル 19) で使用できる新しい Api を導入し、KitKat へのアプリケーションを移行する際に、ベスト プラクティスを説明します。 ユーザーに影響を与える Api への枠で囲まれた変更が発生するなどの it、*遷移 framework*と用の新しいオプション*テーマ*します。 導入された次に、*ストレージ アクセス フレームワーク*と`DocumentsProvider`クラスだけでなく、新しい*印刷 Api*します。 探索が*NFC ホスト ベースのカード エミュレーション*を操作する方法と*低電力センサー*ユーザーの手順を追跡するための 2 つの新しいセンサーを含むです。 アプリケーションとのリアルタイムのデモをキャプチャ最後に、説明*画面記録*、KitKat API の変更点と追加の詳細な一覧を提供します。


## <a name="related-links"></a>関連リンク

- [KitKat サンプル](https://developer.xamarin.com/samples/monodroid/KitKat/)
- [Android 4.4 Api](https://developer.android.com/about/versions/android-4.4.html)
- [Android KitKat](https://developer.android.com/about/versions/kitkat.html)
