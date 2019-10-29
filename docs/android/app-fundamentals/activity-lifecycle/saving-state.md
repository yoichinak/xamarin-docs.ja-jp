---
title: チュートリアル - アクティビティの状態を保存する
description: アクティビティライフサイクルガイドでは、状態の保存の背後にある理論について説明しました。では、例を見てみましょう。
ms.prod: xamarin
ms.assetid: A6090101-67C6-4BDD-9416-F2FB74805A87
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: e449e6a62d0c8ca283f20c689477c1f1482611c5
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73017000"
---
# <a name="walkthrough---saving-the-activity-state"></a>チュートリアル - アクティビティの状態を保存する

_アクティビティライフサイクルガイドでは、状態の保存の背後にある理論について説明しました。では、例を見てみましょう。_

## <a name="activity-state-walkthrough"></a>アクティビティの状態のチュートリアル

( [Activitylifecycle サイクル](https://docs.microsoft.com/samples/xamarin/monodroid-samples/activitylifecycle)サンプルで) **ActivityLifecycle_Start**プロジェクトを開き、ビルドして実行します。 これは、アクティビティのライフサイクルを示す2つのアクティビティと、さまざまなライフサイクルメソッドの呼び出し方法を示す、非常に単純なプロジェクトです。 アプリケーションを起動すると、`MainActivity` の画面が表示されます。

[画面![アクティビティ](saving-state-images/01-activity-a-sml.png)](saving-state-images/01-activity-a.png#lightbox)

### <a name="viewing-state-transitions"></a>状態遷移の表示

このサンプルの各メソッドは、アクティビティの状態を示すために IDE アプリケーションの出力ウィンドウに書き込みます。 (Visual Studio で 出力 ウィンドウを開くには、 **CTRL + ALT + O キーを押し**ます。 Visual Studio for Mac で 出力 ウィンドウを開くには、**表示 > パッド > アプリケーション出力** をクリックします)。

アプリが初めて起動すると、[出力] ウィンドウに*アクティビティ A*の状態の変化が表示されます。 

```shell
[ActivityLifecycle.MainActivity] Activity A - OnCreate
[ActivityLifecycle.MainActivity] Activity A - OnStart
[ActivityLifecycle.MainActivity] Activity A - OnResume
```

**[アクティビティ b の開始]** ボタンをクリックすると、アクティビティ*b*が状態の変化を経ている間、*アクティビティ a が*一時停止して停止します。 

```shell
[ActivityLifecycle.MainActivity] Activity A - OnPause
[ActivityLifecycle.SecondActivity] Activity B - OnCreate
[ActivityLifecycle.SecondActivity] Activity B - OnStart
[ActivityLifecycle.SecondActivity] Activity B - OnResume
[ActivityLifecycle.MainActivity] Activity A - OnStop
```

その結果、アクティビティ*B*が開始され、*アクティビティ a*の代わりに表示されます。 

[![アクティビティ B 画面](saving-state-images/02-activity-b-sml.png)](saving-state-images/02-activity-b.png#lightbox)

**[戻る]** ボタンをクリックすると、*アクティビティ B*が破棄され、*アクティビティ a*が再開されます。 

```shell
[ActivityLifecycle.SecondActivity] Activity B - OnPause
[ActivityLifecycle.MainActivity] Activity A - OnRestart
[ActivityLifecycle.MainActivity] Activity A - OnStart
[ActivityLifecycle.MainActivity] Activity A - OnResume
[ActivityLifecycle.SecondActivity] Activity B - OnStop
[ActivityLifecycle.SecondActivity] Activity B - OnDestroy
```

### <a name="adding-a-click-counter"></a>Click カウンターを追加する

次に、アプリケーションを変更して、クリックされた回数をカウントして表示するボタンを作成します。 まず、`MainActivity`に `_counter` インスタンス変数を追加してみましょう。

```csharp
int _counter = 0;
```

次に、 **Resource/layout/Main axml**レイアウトファイルを編集し、ユーザーがボタンをクリックした回数を表示する新しい `clickButton` を追加します。 結果の**Main**は次のようになります。 

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

`MainActivity` の[OnCreate](xref:Android.App.Activity.OnCreate*)メソッドの末尾に次のコードを追加します。このコードは `clickButton`からのクリックイベントを処理 &ndash; ます。

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

アプリを再度ビルドして実行すると、新しいボタンが表示されます。このボタンをクリックするたびに `_counter` の値が表示されます。

[タッチカウントの追加![](saving-state-images/03-touched-sml.png)](saving-state-images/03-touched.png#lightbox)

ただし、デバイスを横モードに回転させると、この数は失われます。

[横方向への回転![、カウントをゼロに戻します](saving-state-images/05-rotate-nosave-sml.png)](saving-state-images/05-rotate-nosave.png#lightbox)

アプリケーションの出力を調べると、[縦] モードから [横] モードへの回転中に、*アクティビティ A*が一時停止、停止、破棄、再作成、再起動、再開されたことがわかります。 

```shell
[ActivityLifecycle.MainActivity] Activity A - OnPause
[ActivityLifecycle.MainActivity] Activity A - OnStop
[ActivityLifecycle.MainActivity] Activity A - On Destroy

[ActivityLifecycle.MainActivity] Activity A - OnCreate
[ActivityLifecycle.MainActivity] Activity A - OnStart
[ActivityLifecycle.MainActivity] Activity A - OnResume
```

*アクティビティ A*はデバイスのローテーション時に破棄されて再作成されるので、インスタンスの状態は失われます。 次に、インスタンスの状態を保存および復元するためのコードを追加します。

### <a name="adding-code-to-preserve-instance-state"></a>インスタンスの状態を保持するコードの追加

インスタンスの状態を保存する `MainActivity` するメソッドを追加してみましょう。 *アクティビティ A*が破棄される前に、Android は自動的に[OnSaveInstanceState](xref:Android.App.Activity.OnSaveInstanceState*)を呼び出し、インスタンスの状態を格納するために使用できる[バンドル](xref:Android.OS.Bundle)を渡します。 それを使用して、クリック数を整数値として保存してみましょう。

```csharp
protected override void OnSaveInstanceState (Bundle outState)
{
    outState.PutInt ("click_count", _counter);
    Log.Debug(GetType().FullName, "Activity A - Saving instance state");

    // always call the base implementation!
    base.OnSaveInstanceState (outState);    
}
```

*アクティビティ A*を再作成して再開すると、Android はこの `Bundle` を `OnCreate` 方法に戻します。 `OnCreate` にコードを追加して、渡された `Bundle`から `_counter` 値を復元してみましょう。 `clickbutton` が定義されている行の直前に、次のコードを追加します。 

```csharp
if (bundle != null)
{
    _counter = bundle.GetInt ("click_count", 0);
    Log.Debug(GetType().FullName, "Activity A - Recovered instance state");
}
```

もう一度アプリをビルドして実行し、2番目のボタンを数回クリックします。 デバイスを横モードに回転させると、カウントは保持されます。

[画面の回転![は、保持されている4の数を示します。](saving-state-images/06-rotate-save-sml.png)](saving-state-images/06-rotate-save.png#lightbox)

出力ウィンドウを見て、何が起こったかを見てみましょう。

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

[OnStop](xref:Android.App.Activity.OnStop)メソッドが呼び出される前に、新しい `OnSaveInstanceState` メソッドが呼び出され、`_counter` の値が `Bundle`に保存されました。 Android では、`OnCreate` 方法を呼び出したときにこの `Bundle` が米国に戻されました。これを使用して、中断した場所に `_counter` 値を復元できました。

## <a name="summary"></a>まとめ

この記事では、アクティビティのライフサイクルに関する知識を使用して、状態データを保持しました。

## <a name="related-links"></a>関連リンク

- [ActivityLifecycle サイクル (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/activitylifecycle)
- [アクティビティのライフサイクル](~/android/app-fundamentals/activity-lifecycle/index.md)
- [Android アクティビティ](xref:Android.App.Activity)
