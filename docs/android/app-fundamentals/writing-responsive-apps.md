---
title: 応答性の高いアプリケーションの作成
ms.prod: xamarin
ms.assetid: 452DF940-6331-55F0-D130-002822BBED55
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/15/2018
ms.openlocfilehash: a1642c4cbb790cf09d2a31e629408afc61d5b7ab
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50121777"
---
# <a name="writing-responsive-applications"></a>応答性の高いアプリケーションの作成

GUI がブロックされるように、バック グラウンド スレッドで実行時間の長いタスクを実行すると応答性の高い GUI を維持するために、キーの 1 つです。 その値を計算する 5 秒間には、ユーザーに表示する値を計算したいとします。

```csharp
public class ThreadDemo : Activity
{
    TextView textview;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        // Create a new TextView and set it as our view
        textview = new TextView (this);
        textview.Text = "Working..";

        SetContentView (textview);

        SlowMethod ();
    }

    private void SlowMethod ()
    {
        Thread.Sleep (5000);
        textview.Text = "Method Complete";
    }
}
```

これは機能が、アプリケーションが「ハング」5 秒の値を計算中にします。 この期間中、アプリは、ユーザーの操作に応答しません。 この問題を回避するには、バック グラウンド スレッドで、計算を実行します。

```csharp
public class ThreadDemo : Activity
{
    TextView textview;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        // Create a new TextView and set it as our view
        textview = new TextView (this);
        textview.Text = "Working..";

        SetContentView (textview);

        ThreadPool.QueueUserWorkItem (o => SlowMethod ());
    }

    private void SlowMethod ()
    {
        Thread.Sleep (5000);
        textview.Text = "Method Complete";
    }
}
```

今すぐバック グラウンド スレッドで値を計算します、計算中に、GUI が応答性が維持するため。 ただし、計算が完了したら、アプリがクラッシュしたログのこのままになります。

```shell
E/mono    (11207): EXCEPTION handling: Android.Util.AndroidRuntimeException: Exception of type 'Android.Util.AndroidRuntimeException' was thrown.
E/mono    (11207):
E/mono    (11207): Unhandled Exception: Android.Util.AndroidRuntimeException: Exception of type 'Android.Util.AndroidRuntimeException' was thrown.
E/mono    (11207):   at Android.Runtime.JNIEnv.CallVoidMethod (IntPtr jobject, IntPtr jmethod, Android.Runtime.JValue[] parms)
E/mono    (11207):   at Android.Widget.TextView.set_Text (IEnumerable`1 value)
E/mono    (11207):   at MonoDroidDebugging.Activity1.SlowMethod ()
```

これは、GUI スレッドから GUI を更新する必要があります。 コードでは、スレッド プールのスレッドから GUI をアプリのクラッシュの原因を更新します。 バック グラウンド スレッドで、値を計算し、更新プログラムで処理されますが、GUI スレッドで操作を行いますたい[Activity.RunOnUIThread](https://developer.xamarin.com/api/member/Android.App.Activity.RunOnUiThread/(System.Action)):

```csharp
public class ThreadDemo : Activity
{
    TextView textview;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        // Create a new TextView and set it as our view
        textview = new TextView (this);
        textview.Text = "Working..";

        SetContentView (textview);

        ThreadPool.QueueUserWorkItem (o => SlowMethod ());
    }

    private void SlowMethod ()
    {
        Thread.Sleep (5000);
        RunOnUiThread (() => textview.Text = "Method Complete");
    }
}
```

このコードは、期待どおりに動作します。 この GUI では、応答性が維持し、計算が完了したら適切に更新を取得します。

コストの値を計算するのには、この手法は使用されませんだけに注意してください。 これは、web サービスの呼び出しまたはインターネットのデータをダウンロードするように、バック グラウンドで実行できる任意の実行時間の長いタスクを使用できます。
