---
title: Wear の基本
description: 最初の Android Wear アプリを作成し、Wear エミュレーターまたはデバイスで実行します。 このチュートリアルには、Android Wear ボタンのクリックを処理し、Wear デバイスでをクリックしてカウンターを表示します。 小規模なプロジェクトを作成するための手順が説明します。 Wear エミュレーターまたは Android フォンに Bluetooth 経由で接続されている Wear デバイスを使用してアプリをデバッグする方法について説明します。 Android Wear のデバッグに関するヒントのセットも提供します。
ms.prod: xamarin
ms.assetid: 86BCD0E7-E9DC-40F1-9B44-887BC51BB48D
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/10/2018
ms.openlocfilehash: a8e27063040ff91f72a1cbf932b1b277a5dee63d
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50104922"
---
# <a name="hello-wear"></a>Wear の基本

_最初の Android Wear アプリを作成し、Wear エミュレーターまたはデバイスで実行します。このチュートリアルには、Android Wear ボタンのクリックを処理し、Wear デバイスでをクリックしてカウンターを表示します。 小規模なプロジェクトを作成するための手順が説明します。Wear エミュレーターまたは Android フォンに Bluetooth 経由で接続されている Wear デバイスを使用してアプリをデバッグする方法について説明します。Android Wear のデバッグに関するヒントのセットも提供します。_

![このチュートリアルで完了する Wear アプリのスクリーン ショット](hello-wear-images/example.png)

## <a name="your-first-wear-app"></a>最初の Wear アプリ

最初の Xamarin.Android Wear アプリを作成する次の手順に従います。

### <a name="1-create-a-new-android-project"></a>1.新しい Android プロジェクトを作成します。

新規作成**Android Wear アプリケーション**:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![新しいプロジェクト ダイアログで新しい Android Wear アプリケーションの作成](hello-wear-images/vs/new-solution-sml.w157.png)](hello-wear-images/vs/new-solution.w157.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![新しいソリューション ダイアログ ボックスで新しい Android Wear アプリケーションの作成](hello-wear-images/xs/new-solution-sml.png)](hello-wear-images/xs/new-solution.png#lightbox)

-----


このテンプレートが自動的に含まれる、 **Xamarin Android ウェアラブル ライブラリ**NuGet (および依存関係) Wear 固有のウィジェットにアクセスする必要があります。 Wear テンプレートが表示されない場合は、確認、[インストールとセットアップ](~/android/wear/get-started/installation.md)ガイドは、サポートされている Android SDK がインストールされていることを再確認してください。 

### <a name="2-choose-the-correct-target-framework"></a>2.正しい選択**ターゲット フレームワーク**

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

いることを確認**最小 Android ターゲット**に設定されている**Android 5.0 (Lollipop)** またはそれ以降。 

[![Visual Studio での Android 5.0 にターゲット フレームワークを設定します。](hello-wear-images/vs/target-framework-sml.png)](hello-wear-images/vs/target-framework.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

ターゲット フレームワークに設定されていることを確認**Android 5.0 (Lollipop)** またはそれ以降。

[![Visual Studio での Android 5.0 を Mac にターゲット フレームワークを設定します。](hello-wear-images/xs/target-framework-sml.png)](hello-wear-images/xs/target-framework.png#lightbox)

-----

ターゲット フレームワークの設定の詳細については、次を参照してください。 [Understanding Android API Levels](~/android/app-fundamentals/android-api-levels.md)します。


### <a name="3-edit-the-mainaxml-layout"></a>3.編集、 **Main.axml**レイアウト

構成を格納するレイアウトを`TextView`と`Button`サンプル。 

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

カウンターをインクリメントして、ボタンがクリックされたときに表示するコードを追加します。 

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

[次へ] の手順をデプロイして、アプリを実行するには、エミュレーターまたはデバイスを設定するとします。 ない場合の展開および実行するプロセスに詳しく Xamarin.Android アプリ一般を参照してください、[こんにちは, Android クイック スタート](~/android/get-started/hello-android/hello-android-quickstart.md)します。

Android Wear デバイス、Android Wear スマートウォッチなどがいない場合は、エミュレーターで、アプリを実行できます。 Wear アプリをエミュレーターでのデバッグについては、次を参照してください。[エミュレーターで Android Wear をデバッグ](~/android/wear/deploy-test/debug-on-emulator.md)します。

Android Wear デバイス、Android Wear スマートウォッチなどを使っている場合は、エミュレーターを使用する代わりに、デバイスでアプリを実行できます。 Wear デバイスでのデバッグの詳細については、次を参照してください。 [Wear デバイスでのデバッグ](~/android/wear/deploy-test/debug-on-device.md)します。


### <a name="6-run-the-android-wear-app"></a>6.Android Wear アプリを実行します。

Android Wear デバイスは、デバイスのプルダウン メニューに表示されます。 デバッグを開始する前に、正しい Android Wear デバイスまたは AVD を選択してください。 デバイスを選択すると、エミュレーターまたはデバイスにアプリを展開する [再生] ボタンをクリックします。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Visual Studio メニューでデバイスを着用 AVD を選択します。](hello-wear-images/vs/choose-wear-sim.png)](hello-wear-images/vs/choose-wear-sim.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![Visual Studio での Mac デバイス メニュー Wear AVD を選択します。](hello-wear-images/xs/choose-wear-sim.png)](hello-wear-images/xs/choose-wear-sim.png#lightbox)

-----

表示される、**わずか 1 分.** 最初メッセージ (またはその他のいくつかのスポット画面)。 

![エミュレーターは、わずか 1 分を表示を見る.](hello-wear-images/please-wait.png)

ウォッチ エミュレーターを使用している場合は、アプリの起動にかかります。 Bluetooth を使用しているときに、USB 経由では、アプリのデプロイに時間がかかります。 (たとえば、約 5 分かかります Nexus 5 電話の Bluetooth 接続である LG G ウォッチにこのアプリをデプロイする。)

アプリが正常に展開した後 Wear デバイスの画面には、次のような画面が表示されます。

[![Wear アプリの最初の画面](hello-wear-images/mainactivity-screen.png)](hello-wear-images/mainactivity-screen.png#lightbox)

タップして、 **をクリックします** 。 ボタン表面 Wear デバイスと各をタップしてカウントの増加を参照してください。

[![3 回のクリック後のスクリーン ショットの Wear アプリ](hello-wear-images/mainactivity-counts.png)](hello-wear-images/mainactivity-counts.png#lightbox)


## <a name="next-steps"></a>次の手順

チェック アウト、 [Wear サンプル](https://developer.xamarin.com/samples/android/Android%20Wear/)コンパニオン Phone アプリで Android Wear アプリなどです。

アプリを配布する準備ができたらを参照してください。[パッケージ化操作](~/android/wear/deploy-test/packaging.md)します。


## <a name="related-links"></a>関連リンク

- [Me アプリ (サンプル) をクリックします。](https://developer.xamarin.com/samples/monodroid/wear/WearTest/)
