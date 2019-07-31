---
title: Wear の基本
description: 初めての Android 用の磨耗アプリを作成し、それを磨耗エミュレーターまたはデバイスで実行します。 このチュートリアルでは、ボタンのクリックを処理し、磨耗デバイスに click カウンターを表示する小さな Android 用の磨耗プロジェクトを作成する手順について説明します。 この例では、アプリをデバッグする方法について説明します。これには、デバイスの摩耗エミュレーター、または Bluetooth 経由で Android フォンに接続されている磨耗デバイスを使用します。 また、Android の磨耗に関する一連のデバッグのヒントも提供します。
ms.prod: xamarin
ms.assetid: 86BCD0E7-E9DC-40F1-9B44-887BC51BB48D
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/10/2018
ms.openlocfilehash: 056ab7a9fe4bcb7f07a9a7cd7c841a3d9f7574b6
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68648022"
---
# <a name="hello-wear"></a>Wear の基本

_初めての Android 用の磨耗アプリを作成し、それを磨耗エミュレーターまたはデバイスで実行します。このチュートリアルでは、ボタンのクリックを処理し、磨耗デバイスに click カウンターを表示する小さな Android 用の磨耗プロジェクトを作成する手順について説明します。この例では、アプリをデバッグする方法について説明します。これには、デバイスの摩耗エミュレーター、または Bluetooth 経由で Android フォンに接続されている磨耗デバイスを使用します。また、Android の磨耗に関する一連のデバッグのヒントも提供します。_

![このチュートリアルで完了する必要がある磨耗アプリのスクリーンショット](hello-wear-images/example.png)

## <a name="your-first-wear-app"></a>最初の摩耗アプリ

次の手順に従って、最初の Xamarin. Android の磨耗アプリを作成します。

### <a name="1-create-a-new-android-project"></a>1.新しい Android プロジェクトを作成する

新しい Android 用の**磨耗アプリケーション**を作成します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![[新しいプロジェクト] ダイアログでの新しい Android の磨耗アプリケーションの作成](hello-wear-images/vs/new-solution-sml.w157.png)](hello-wear-images/vs/new-solution.w157.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![[新しいソリューション] ダイアログでの新しい Android の磨耗アプリケーションの作成](hello-wear-images/xs/new-solution-sml.png)](hello-wear-images/xs/new-solution.png#lightbox)

-----


このテンプレートには、 **Xamarin Android ウェアラブルライブラリ**NuGet (および依存関係) が自動的に含まれるため、摩耗固有のウィジェットにアクセスできます。 [磨耗] テンプレートが表示されない場合は、[インストールとセットアップ](~/android/wear/get-started/installation.md)のガイドを参照して、サポートされている Android SDK がインストールされていることを確認します。 

### <a name="2-choose-the-correct-target-framework"></a>2.適切な**ターゲットフレームワーク**を選択する

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

最小の**android ターゲット**が**Android 5.0 (ロリポップ)** 以降に設定されていることを確認します。 

[![Visual Studio でターゲットフレームワークを Android 5.0 に設定する](hello-wear-images/vs/target-framework-sml.png)](hello-wear-images/vs/target-framework.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

ターゲットフレームワークが**Android 5.0 (ロリポップ)** 以降に設定されていることを確認します。

[![Visual Studio for Mac でターゲットフレームワークを Android 5.0 に設定する](hello-wear-images/xs/target-framework-sml.png)](hello-wear-images/xs/target-framework.png#lightbox)

-----

ターゲットフレームワークの設定の詳細については、「 [ANDROID API レベルについ](~/android/app-fundamentals/android-api-levels.md)て」を参照してください。


### <a name="3-edit-the-mainaxml-layout"></a>3.メインの**axml**レイアウトを編集する

サンプル`TextView` の`Button`とを含むようにレイアウトを構成します。 

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

### <a name="4-edit-the-mainactivitycs-source"></a>4.**MainActivity.cs**ソースを編集する

カウンターをインクリメントし、ボタンがクリックされるたびに表示されるように、コードを追加します。 

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

### <a name="5-setup-an-emulator-or-device"></a>5.エミュレーターまたはデバイスを設定する

次の手順では、アプリをデプロイして実行するためのエミュレーターまたはデバイスを設定します。 Xamarin をデプロイして実行するプロセスにまだ慣れていない場合は、「 [Hello, Android クイックスタート](~/android/get-started/hello-android/hello-android-quickstart.md)」を参照してください。

Android の磨耗 Smartwatch などの Android 磨耗デバイスがない場合は、エミュレーターでアプリを実行できます。 エミュレーターでの磨耗アプリのデバッグの詳細については、「[エミュレーターでの Android の磨耗のデバッグ](~/android/wear/deploy-test/debug-on-emulator.md)」を参照してください。

Android の磨耗 Smartwatch などの Android の磨耗デバイスがある場合は、エミュレーターを使用する代わりに、デバイスでアプリを実行できます。 磨耗デバイスでのデバッグの詳細については、「[磨耗デバイスでのデバッグ](~/android/wear/deploy-test/debug-on-device.md)」を参照してください。


### <a name="6-run-the-android-wear-app"></a>6.Android の磨耗アプリを実行する

Android の磨耗デバイスが [デバイス] プルダウンメニューに表示されます。 デバッグを開始する前に、適切な Android の磨耗デバイスまたは AVD を選択してください。 デバイスを選択した後、[再生] ボタンをクリックして、エミュレーターまたはデバイスにアプリをデプロイします。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Visual Studio デバイスメニューでの磨耗 AVD の選択](hello-wear-images/vs/choose-wear-sim.png)](hello-wear-images/vs/choose-wear-sim.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![Visual Studio for Mac デバイスメニューでの磨耗 AVD の選択](hello-wear-images/xs/choose-wear-sim.png)](hello-wear-images/xs/choose-wear-sim.png#lightbox)

-----

最初に、1**分ほど**のメッセージ (または他のスポット画面) が表示される場合があります。 

![Watch emulator が1分で表示されます...](hello-wear-images/please-wait.png)

Watch emulator を使用している場合は、アプリの起動に時間がかかることがあります。 Bluetooth を使用している場合は、USB よりもアプリの展開に時間がかかります。 (たとえば、このアプリを LG G Watch にデプロイするには、5分ほどかかることがあります。これは、接続5の電話に Bluetooth で接続されています)。

アプリが正常に展開されると、磨耗デバイスの画面に次のような画面が表示されます。

[![磨耗アプリの最初の画面](hello-wear-images/mainactivity-screen.png)](hello-wear-images/mainactivity-screen.png#lightbox)

タップして、 **をクリックします** 。 ボタンを押して、次のように押します。

[![3回のクリック後の磨耗アプリのスクリーンショット](hello-wear-images/mainactivity-counts.png)](hello-wear-images/mainactivity-counts.png#lightbox)


## <a name="next-steps"></a>次の手順

関連する電話アプリを含む、Android の摩耗アプリを含む[磨耗のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Android+wear)を確認してください。

アプリを配布する準備ができたら、「[パッケージングの操作](~/android/wear/deploy-test/packaging.md)」を参照してください。


## <a name="related-links"></a>関連リンク

- [[Me App (サンプル)]](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-weartest)
