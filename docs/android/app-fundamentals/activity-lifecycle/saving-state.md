---
title: チュートリアル - アクティビティの状態を保存する
description: アクティビティのライフ サイクル ガイド; の状態を保存する理論をについて説明しました次に例を見てみましょう。
ms.prod: xamarin
ms.assetid: A6090101-67C6-4BDD-9416-F2FB74805A87
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: c8f92e55648dff469227cc3bad981ad5f6e6d0ac
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61019157"
---
# <a name="walkthrough---saving-the-activity-state"></a>チュートリアル - アクティビティの状態を保存する

_アクティビティのライフ サイクル ガイド; の状態を保存する理論をについて説明しました次に例を見てみましょう。_

## <a name="activity-state-walkthrough"></a>アクティビティの状態のチュートリアル

開いてみましょう、 **ActivityLifecycle_Start**プロジェクト (で、 [ActivityLifecycle](https://developer.xamarin.com/samples/monodroid/ActivityLifecycle)サンプル) をビルドして実行します。 これは、アクティビティのライフ サイクルと、さまざまなライフ サイクル メソッドを呼び出す方法を示す 2 つのアクティビティがある非常に単純なプロジェクトです。 アプリケーションの画面を起動すると`MainActivity`が表示されます。 

[![アクティビティ A 画面](saving-state-images/01-activity-a-sml.png)](saving-state-images/01-activity-a.png#lightbox)

### <a name="viewing-state-transitions"></a>表示状態遷移

このサンプル内の各メソッドは、アクティビティの状態を示すためには、IDE アプリケーション出力ウィンドウに書き込みます。 (Visual Studio で、出力ウィンドウを開くには入力**CTRL-o ALT**を Visual studio for Mac、出力ウィンドウを開き、**ビュー > パッド > アプリケーションの出力**)。

出力ウィンドウに表示の状態の変更、アプリが起動時に*アクティビティ A*: 

```shell
[ActivityLifecycle.MainActivity] Activity A - OnCreate
[ActivityLifecycle.MainActivity] Activity A - OnStart
[ActivityLifecycle.MainActivity] Activity A - OnResume
```

クリックしたとき、**開始アクティビティ B**ボタンを表示*アクティビティ A*一時停止、停止中に*アクティビティ B*その状態の変化を経由します。 

```shell
[ActivityLifecycle.MainActivity] Activity A - OnPause
[ActivityLifecycle.SecondActivity] Activity B - OnCreate
[ActivityLifecycle.SecondActivity] Activity B - OnStart
[ActivityLifecycle.SecondActivity] Activity B - OnResume
[ActivityLifecycle.MainActivity] Activity A - OnStop
```

その結果、*アクティビティ B*が開始およびの代わりに表示される*アクティビティ A*: 

[![B [アクティビティ] 画面](saving-state-images/02-activity-b-sml.png)](saving-state-images/02-activity-b.png#lightbox)

をクリックして、**戻る**ボタン、*アクティビティ B*が破棄されると*アクティビティ A*が再開されます。 

```shell
[ActivityLifecycle.SecondActivity] Activity B - OnPause
[ActivityLifecycle.MainActivity] Activity A - OnRestart
[ActivityLifecycle.MainActivity] Activity A - OnStart
[ActivityLifecycle.MainActivity] Activity A - OnResume
[ActivityLifecycle.SecondActivity] Activity B - OnStop
[ActivityLifecycle.SecondActivity] Activity B - OnDestroy
```
### <a name="adding-a-click-counter"></a>クリックしてカウンターの追加

次に、アプリケーションを変更するボタンがクリックされた回数の合計を表示してカウントする必要がよういきます。 最初に、追加、`_counter`インスタンス変数を`MainActivity`:

```csharp
int _counter = 0;
```

次に、編集、 **Resource/layout/Main.axml**レイアウト ファイルを開き、新しい追加`clickButton`をユーザーには、ボタンがクリックされた回数の合計が表示されます。 その結果、 **Main.axml**次のようになります。 

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent">
    <Button
        android:id="@+id/myButton"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:text="@string/mybutton_text" />
    <Button
        android:id="@+id/clickButton"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:text="@string/counterbutton_text" />
</LinearLayout>
```

末尾に次のコードを追加、 [OnCreate](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/)メソッド`MainActivity`&ndash;このコード クリックからのイベントを処理、 `clickButton`:

```csharp
var clickbutton = FindViewById<Button> (Resource.Id.clickButton);
clickbutton.Text = Resources.GetString (
    Resource.String.counterbutton_text, _counter);
clickbutton.Click += (object sender, System.EventArgs e) =>
{
    _counter++;
    clickbutton.Text = Resources.GetString (
        Resource.String.counterbutton_text, _counter);
} ;
```

インクリメントを構築して、アプリをもう一度実行するには、新しいボタンを表示しの値を表示します`_counter`をクリックするたびに。

[![タッチの数を追加します。](saving-state-images/03-touched-sml.png)](saving-state-images/03-touched.png#lightbox)

横モードにデバイスを回転させることと、この数が失われます。

[![0 に、カウントを設定する横長に回転](saving-state-images/05-rotate-nosave-sml.png)](saving-state-images/05-rotate-nosave.png#lightbox)

アプリケーションの出力を調べることがわかります*アクティビティ A*が一時停止、停止、破棄、再作成、再起動し、縦向きから横向きモードの回転時に再開します。 

```shell
[ActivityLifecycle.MainActivity] Activity A - OnPause
[ActivityLifecycle.MainActivity] Activity A - OnStop
[ActivityLifecycle.MainActivity] Activity A - On Destroy

[ActivityLifecycle.MainActivity] Activity A - OnCreate
[ActivityLifecycle.MainActivity] Activity A - OnStart
[ActivityLifecycle.MainActivity] Activity A - OnResume
```

*アクティビティ A*破棄され、デバイスの回転、もう一度再作成は、インスタンスの状態は失われます。 次に、保存し、インスタンスの状態を復元するためのコードを追加します。

### <a name="adding-code-to-preserve-instance-state"></a>インスタンスの状態を保持するコードを追加します。

メソッドを追加してみましょう`MainActivity`インスタンスの状態を保存します。 前に*アクティビティ A*が破棄されると、Android が自動的に呼び出す[OnSaveInstanceState](https://developer.xamarin.com/api/member/Android.App.Activity.OnSaveInstanceState/p/Android.OS.Bundle/)を渡します、[バンドル](https://developer.xamarin.com/api/type/Android.OS.Bundle/)インスタンスの状態の格納に使用することができます。 整数値として、クリック数を保存する使ってみましょう。

```csharp
protected override void OnSaveInstanceState (Bundle outState)
{
    outState.PutInt ("click_count", _counter);
    Log.Debug(GetType().FullName, "Activity A - Saving instance state");

    // always call the base implementation!
    base.OnSaveInstanceState (outState);    
}
```

ときに*アクティビティ A*が再作成され、再開されると、Android 渡します`Bundle`に戻す、`OnCreate`メソッド。 コードを追加してみましょう`OnCreate`を復元する、`_counter`値から渡されるで`Bundle`します。 行の直前に次のコードを追加、`clickbutton`が定義されています。 

```csharp
if (bundle != null)
{
    _counter = bundle.GetInt ("click_count", 0);
    Log.Debug(GetType().FullName, "Activity A - Recovered instance state");
}
```

ビルド アプリをもう一度実行し、何回か 2 番目のボタンをクリックします。 横向きモードにデバイスを回転しますと、カウントが保持されます。

[![4 つの保持数を示しています、画面の回転](saving-state-images/06-rotate-save-sml.png)](saving-state-images/06-rotate-save.png#lightbox)


変更点を表示する出力ウィンドウを見てみましょう。
    
```shell
[ActivityLifecycle.MainActivity] Activity A - OnPause
[ActivityLifecycle.MainActivity] Activity A - Saving instance state
[ActivityLifecycle.MainActivity] Activity A - OnStop
[ActivityLifecycle.MainActivity] Activity A - On Destroy

[ActivityLifecycle.MainActivity] Activity A - OnCreate
[ActivityLifecycle.MainActivity] Activity A - Recovered instance state
[ActivityLifecycle.MainActivity] Activity A - OnStart
[ActivityLifecycle.MainActivity] Activity A - OnResume
``` 

前に、 [OnStop](https://developer.xamarin.com/api/member/Android.App.Activity.OnStop/)メソッドが呼び出されると、新しい`OnSaveInstanceState`を保存するメソッドが呼び出された、`_counter`値、`Bundle`します。 これに渡される android`Bundle`お知らせいただくことが呼び出されたときに、`OnCreate`メソッド、および私たちが復元するために使用すること、`_counter`再開する値。


## <a name="summary"></a>まとめ

このチュートリアルでは、状態データを保持するためにアクティビティのライフ サイクルの知識を使用しましたがあります。 



## <a name="related-links"></a>関連リンク

- [ActivityLifecycle (サンプル)](https://developer.xamarin.com/samples/monodroid/ActivityLifecycle)
- [アクティビティのライフサイクル](~/android/app-fundamentals/activity-lifecycle/index.md)
- [Android アクティビティ](https://developer.xamarin.com/api/type/Android.App.Activity/)
