---
title: "こんにちは, 損傷"
description: "最初の Android 着用アプリを作成し、損傷のエミュレーターまたはデバイスで実行します。 このチュートリアルでは、ボタンのクリックを処理し、損傷デバイスをクリックしてカウンターを表示する小規模な Android 着用プロジェクトを作成するための手順を説明します。 これには、損傷エミュレーターまたは Android フォンに Bluetooth 経由で接続されている消耗デバイスを使用してアプリをデバッグする方法について説明します。 また、Android を着用のデバッグに関するヒントのセットも提供します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 86BCD0E7-E9DC-40F1-9B44-887BC51BB48D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 8eed2d6b825a6e6dd7e956bf901246b9a630081a
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="hello-wear"></a>こんにちは, 損傷

_最初の Android 着用アプリを作成し、損傷のエミュレーターまたはデバイスで実行します。このチュートリアルでは、ボタンのクリックを処理し、損傷デバイスをクリックしてカウンターを表示する小規模な Android 着用プロジェクトを作成するための手順を説明します。これには、損傷エミュレーターまたは Android フォンに Bluetooth 経由で接続されている消耗デバイスを使用してアプリをデバッグする方法について説明します。また、Android を着用のデバッグに関するヒントのセットも提供します。_

![このチュートリアルを完了する消耗アプリのスクリーン ショット](hello-wear-images/example.png)

## <a name="your-first-wear-app"></a>最初の消耗アプリ

最初の Xamarin.Android 消耗アプリを作成する手順に従います。

### <a name="1-create-a-new-android-project"></a>1.新しい Android プロジェクトを作成します。

新しい**Android 着用アプリケーション**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![新しいプロジェクト ダイアログで新しい Android 着用アプリケーションの作成](hello-wear-images/vs/new-solution-sml.png)](hello-wear-images/vs/new-solution.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![新しいソリューション ダイアログ ボックスで新しい Android 着用アプリケーションの作成](hello-wear-images/xs/new-solution-sml.png)](hello-wear-images/xs/new-solution.png#lightbox)

-----


このテンプレートは自動的に含まれる、 **Xamarin Android ウェアラブル ライブラリ**NuGet (と依存関係) 消耗固有ウィジェットにアクセスする必要があります。 消耗テンプレートが表示されない場合は、確認、[インストールとセットアップ](~/android/wear/get-started/installation.md)ガイドをサポートされている Android SDK がインストールされていることを再確認してください。 

### <a name="2-choose-the-correct-target-framework"></a>2.正しい選択**ターゲット フレームワーク**

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

いることを確認**ターゲットに最低限の Android**に設定されている**Android 5.0 (ロリポップ)**以降。 

[![Visual Studio での Android 5.0 にターゲット フレームワークを設定](hello-wear-images/vs/target-framework-sml.png)](hello-wear-images/vs/target-framework.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

ターゲット フレームワークに設定されていることを確認**Android 5.0 (ロリポップ)**以降。

[![Mac 用 Visual Studio での Android 5.0 にターゲット フレームワークを設定](hello-wear-images/xs/target-framework-sml.png)](hello-wear-images/xs/target-framework.png#lightbox)

-----

ターゲット フレームワークを設定する方法の詳細については、次を参照してください。 [Android API レベルの理解](~/android/app-fundamentals/android-api-levels.md)です。


### <a name="3-edit-the-mainaxml-layout"></a>3.編集、 **Main.axml**レイアウト

構成を格納するレイアウト、`TextView`と`Button`サンプル。 

```xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
android:layout_width="match_parent"
android:layout_height="match_parent">
  <ScrollView
    android:id="@+id/scroll"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:background="#000000"
    android:fillViewport="true">
    <LinearLayout
      android:layout_width="match_parent"
      android:layout_height="wrap_content"
      android:orientation="vertical">
      <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginBottom="2dp"
        android:text="Main Activity"
        android:textSize="36sp"
        android:textColor="#006600" />
      <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginBottom="2dp"
        android:textColor="#cccccc"
        android:id="@+id/result" />
      <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:onClick="showNotification"
        android:text="Click Me!"
        android:id="@+id/click_button" />
    </LinearLayout>
  </ScrollView>
</FrameLayout>
```

### <a name="4-edit-the-mainactivitycs-source"></a>4.編集、 **MainActivity.cs**ソース

カウンターをインクリメントし、ボタンがクリックされたときに表示するコードを追加します。 

```csharp
[Activity (Label = "WearTest", MainLauncher = true, Icon = "@drawable/icon")]
public class MainActivity : Activity
{
  int count = 1;

  protected override void OnCreate (Bundle bundle)
  {
    base.OnCreate (bundle);

    SetContentView (Resource.Layout.Main);

    Button button = FindViewById<Button> (Resource.Id.click_button);
    TextView text = FindViewById<TextView> (Resource.Id.result);

    button.Click += delegate {
      text.Text = string.Format ("{0} clicks!", count++);
    };
  }
}
```

### <a name="5-setup-an-emulator-or-device"></a>5.エミュレーターまたはデバイスのセットアップ

次の手順を展開し、アプリを実行するには、エミュレーターまたはデバイスを設定するとします。 まだ配置および実行の手順に慣れる Xamarin.Android アプリ一般を参照してください場合は、[こんにちは, Android のクイック スタート](~/android/get-started/hello-android/hello-android-quickstart.md)です。

Android の着用 Smartwatch など、Android を着用デバイスがあるない場合は、エミュレーターでアプリを実行できます。 エミュレーターでアプリの消耗のデバッグについては、次を参照してください。[エミュレーターで Android 着用をデバッグ](~/android/wear/deploy-test/debug-on-emulator.md)です。

Android の着用 Smartwatch など、Android を着用デバイスがある場合は場合、は、エミュレーターを使用する代わりに、デバイスでアプリを実行できます。 消耗デバイスでデバッグの詳細については、次を参照してください。[着用デバイス上でのデバッグ](~/android/wear/deploy-test/debug-on-device.md)です。


### <a name="6-run-the-android-wear-app"></a>6.Android の消耗アプリを実行します。

Android を着用デバイスは、デバイスのプルダウン メニューに表示されます。 デバッグを開始する前に、正しい着用して Android デバイスまたは AVD を選択することを確認します。 デバイスを選択すると、エミュレーターまたはデバイスにアプリを展開する [再生] ボタンをクリックします。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Visual Studio デバイス メニューに着け AVD を選択します。](hello-wear-images/vs/choose-wear-sim.png)](hello-wear-images/vs/choose-wear-sim.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Mac のデバイス メニューの Visual Studio で着用 AVD の選択](hello-wear-images/xs/choose-wear-sim.png)](hello-wear-images/xs/choose-wear-sim.png#lightbox)

-----

表示される、**すぐしています.**最初にメッセージ (またはその他のいくつかのスポット画面)。 

![エミュレーターは 1 分だけを表示を確認してください.](hello-wear-images/please-wait.png)

ウォッチ エミュレーターを使用している場合、アプリの起動にしばらくかかります。 Bluetooth を使用しているときに、アプリを展開して、USB 上よりも時間がかかります。 (たとえば、5 分程度にかかる Nexus 5 スマート フォンへの Bluetooth 接続である LG G ウォッチをこのアプリを展開します。)

アプリが正常に展開した後に、損傷、デバイスの画面は次のように画面を表示する必要があります。

[![消耗アプリの初期画面](hello-wear-images/mainactivity-screen.png)](hello-wear-images/mainactivity-screen.png#lightbox)

タップして、**します をクリックします。** ボタン表面消耗デバイスと各 tap カウントの増加を参照してください:

[![3 回のクリック後にアプリを着用のスクリーン ショット](hello-wear-images/mainactivity-counts.png)](hello-wear-images/mainactivity-counts.png#lightbox)


## <a name="next-steps"></a>次の手順

チェック アウト、[サンプルを着用](https://developer.xamarin.com/samples/android/Android%20Wear/)コンパニオン Phone アプリでアプリを Android 着用を含むです。

アプリを配布する準備ができたらを参照してください。[パッケージングの使用](~/android/wear/deploy-test/packaging.md)です。


## <a name="related-links"></a>関連リンク

- [(サンプル) のアプリ をクリックします。](https://developer.xamarin.com/samples/monodroid/wear/WearTest/)
