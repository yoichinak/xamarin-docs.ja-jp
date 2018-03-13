---
title: "チュートリアル - アクティビティの状態の保存"
description: "アクティビティのライフ サイクル番組ガイドの状態の保存の背後にある原理を説明します。ここで、例を見てみましょう。"
ms.topic: article
ms.prod: xamarin
ms.assetid: A6090101-67C6-4BDD-9416-F2FB74805A87
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: d8b44fb7f0e60db407271fd84899489bf8e65694
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="walkthrough---saving-the-activity-state"></a>チュートリアル - アクティビティの状態の保存

_アクティビティのライフ サイクル番組ガイドの状態の保存の背後にある原理を説明します。ここで、例を見てみましょう。_

## <a name="activity-state-walkthrough"></a>アクティビティの状態のチュートリアル

開いてみましょう、 **ActivityLifecycle_Start**プロジェクト (で、 [ActivityLifecycle](https://developer.xamarin.com/samples/monodroid/ActivityLifecycle)サンプル)、ビルド、およびそれを実行します。 これは、アクティビティのライフ サイクルとライフ サイクルのさまざまなメソッドを呼び出す方法を説明する 2 つの活動を非常に単純なプロジェクトです。 アプリケーションの画面を起動するときに`MainActivity`が表示されます。 

[![アクティビティ A 画面](saving-state-images/01-activity-a-sml.png)](saving-state-images/01-activity-a.png#lightbox)

### <a name="viewing-state-transitions"></a>表示状態遷移

このサンプル内の各メソッドは、活動状態を示すため、IDE のアプリケーションの出力ウィンドウに書き込みます。 (Visual Studio では、出力ウィンドウを開くには入力**CTRL ALT O**Mac の Visual Studio では、出力ウィンドウを開き、をクリックする;**ビュー > パッド > アプリケーションの出力**)。

出力ウィンドウに表示の状態の変更が、アプリが初めて起動したとき*アクティビティ A*: 

```shell
[ActivityLifecycle.MainActivity] Activity A - OnCreate
[ActivityLifecycle.MainActivity] Activity A - OnStart
[ActivityLifecycle.MainActivity] Activity A - OnResume
```

クリックしたとき、**アクティビティ B の開始**ボタン、ここを参照してください*アクティビティ A*一時停止、停止中に*アクティビティ B*その状態の変更を通過します。 

```shell
[ActivityLifecycle.MainActivity] Activity A - OnPause
[ActivityLifecycle.SecondActivity] Activity B - OnCreate
[ActivityLifecycle.SecondActivity] Activity B - OnStart
[ActivityLifecycle.SecondActivity] Activity B - OnResume
[ActivityLifecycle.MainActivity] Activity A - OnStop
```

その結果、*アクティビティ B*が開始およびの代わりに表示される*アクティビティ A*: 

[![アクティビティ B 画面](saving-state-images/02-activity-b-sml.png)](saving-state-images/02-activity-b.png#lightbox)

クリックしたとき、**戻る**ボタン、*アクティビティ B*が破棄されると*アクティビティ A*が再開します。 

```shell
[ActivityLifecycle.SecondActivity] Activity B - OnPause
[ActivityLifecycle.MainActivity] Activity A - OnRestart
[ActivityLifecycle.MainActivity] Activity A - OnStart
[ActivityLifecycle.MainActivity] Activity A - OnResume
[ActivityLifecycle.SecondActivity] Activity B - OnStop
[ActivityLifecycle.SecondActivity] Activity B - OnDestroy
```
### <a name="adding-a-click-counter"></a>クリックしてカウンターの追加

次に、ここでアプリケーションを変更する、カウントがクリックされた回数を表示するボタンがあるようにします。 最初に、追加してみましょう、`_counter`インスタンス変数を`MainActivity`:。

```csharp
int _counter = 0;
```

次に、編集しましょう、 **Resource/layout/Main.axml**レイアウト ファイルし、新しい`clickButton`ユーザーには、ボタンがクリックされた回数を表示します。 その結果**Main.axml**次のようになります。 

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

末尾に次のコードを追加してみましょう、 [OnCreate](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/)メソッド`MainActivity`&ndash;このコード クリックからイベントを処理、 `clickButton`:

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

構築して、アプリケーションを再実行するには、新しいボタンが表示されますをインクリメントし、値を表示`_counter`でそれぞれをクリックします。

[![タッチ カウントを追加します。](saving-state-images/03-touched-sml.png)](saving-state-images/03-touched.png#lightbox)

このカウント横向きモードにデバイスを回転させることが失われます。

[![0 に、カウントを設定する横に回転させる](saving-state-images/05-rotate-nosave-sml.png)](saving-state-images/05-rotate-nosave.png#lightbox)

アプリケーションの出力を確認するには、それが表示されて*アクティビティ A*が一時停止、停止、破棄、再作成、再起動、し、縦から横モードの回転時に再開します。 

```shell
[ActivityLifecycle.MainActivity] Activity A - OnPause
[ActivityLifecycle.MainActivity] Activity A - OnStop
[ActivityLifecycle.MainActivity] Activity A - On Destroy

[ActivityLifecycle.MainActivity] Activity A - OnCreate
[ActivityLifecycle.MainActivity] Activity A - OnStart
[ActivityLifecycle.MainActivity] Activity A - OnResume
```

*アクティビティ A*破棄され、デバイスを回転したときに、もう一度再作成がそのインスタンスの状態は失われます。 次に、保存し、インスタンスの状態を復元するためのコードを追加します。

### <a name="adding-code-to-preserve-instance-state"></a>インスタンスの状態を保持するコードを追加します。

メソッドを追加してみましょう。`MainActivity`インスタンスの状態を保存します。 前に*アクティビティ A*が破棄されると、Android を自動的に呼び出します[OnSaveInstanceState](https://developer.xamarin.com/api/member/Android.App.Activity.OnSaveInstanceState/p/Android.OS.Bundle/)で渡すと、[バンドル](https://developer.xamarin.com/api/type/Android.OS.Bundle/)インスタンスの状態の格納を使用できます。 使用してみましょう、クリック数を整数値として保存します。

```csharp
protected override void OnSaveInstanceState (Bundle outState)
{
    outState.PutInt ("click_count", _counter);
    Log.Debug(GetType().FullName, "Activity A - Saving instance state");

    // always call the base implementation!
    base.OnSaveInstanceState (outState);    
}
```

ときに*アクティビティ A*が再作成され、再開されると、Android を通過`Bundle`に戻す、`OnCreate`メソッドです。 コードを追加してみましょう。`OnCreate`を復元する、`_counter`から渡された値`Bundle`です。 行の前に、次のコードを追加、`clickbutton`が定義されています。 

```csharp
if (bundle != null)
{
    _counter = bundle.GetInt ("click_count", 0);
    Log.Debug(GetType().FullName, "Activity A - Recovered instance state");
}
```

ビルドし、アプリを再実行し、何回か 2 番目のボタンをクリックします。 横向きモードにデバイスを回転お場合カウントが保持されます。

[![保持 4 つの数を示す、画面の回転](saving-state-images/06-rotate-save-sml.png)](saving-state-images/06-rotate-save.png#lightbox)


何が起こった確認には、出力ウィンドウを見てをみましょう。
    
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

前に、 [OnStop](https://developer.xamarin.com/api/member/Android.App.Activity.OnStop/)メソッドが呼び出された新しい`OnSaveInstanceState`を保存するメソッドが呼び出されました、`_counter`値で、`Bundle`です。 Android で渡されるこの`Bundle`お知らせいただくことが呼び出されたとき、`OnCreate`メソッド、およびおできたの復元に使用する、`_counter`中断した場所に値。


## <a name="summary"></a>まとめ

このチュートリアルでは、状態データを保持するのにアクティビティのライフ サイクルの知識を使用しています。 



## <a name="related-links"></a>関連リンク

- [ActivityLifecycle (サンプル)](https://developer.xamarin.com/samples/monodroid/ActivityLifecycle)
- [アクティビティのライフサイクル](~/android/app-fundamentals/activity-lifecycle/index.md)
- [Android のアクティビティ](https://developer.xamarin.com/api/type/Android.App.Activity/)
