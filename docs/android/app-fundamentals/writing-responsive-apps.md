---
title: "応答性の高いアプリケーションの作成"
ms.topic: article
ms.prod: xamarin
ms.assetid: 452DF940-6331-55F0-D130-002822BBED55
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: 67cad90f99614c3df90f483c9c6dbcec2df5113a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="writing-responsive-applications"></a>応答性の高いアプリケーションの作成

GUI がブロックされるため、バック グラウンド スレッドで時間のかかるタスクを行う応答性の GUI を維持するために、キーの 1 つです。 値は 5 秒間を計算することが、ユーザーに表示する値を計算したいとします。

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

これは、機能しますが、アプリケーションは「ハング」5 秒の値を計算中にします。 この期間中に、アプリは任意のユーザーとの対話に応答しません。 この問題を回避するため、バック グラウンド スレッドで、計算を実行します。

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

今すぐお値を計算しますバック グラウンド スレッドで GUI はまだ計算中に応答するようにします。 ただし、計算が完了すると、アプリのクラッシュ、ログのままにしておきます。

```shell
E/mono    (11207): EXCEPTION handling: Android.Util.AndroidRuntimeException: Exception of type 'Android.Util.AndroidRuntimeException' was thrown.
E/mono    (11207):
E/mono    (11207): Unhandled Exception: Android.Util.AndroidRuntimeException: Exception of type 'Android.Util.AndroidRuntimeException' was thrown.
E/mono    (11207):   at Android.Runtime.JNIEnv.CallVoidMethod (IntPtr jobject, IntPtr jmethod, Android.Runtime.JValue[] parms)
E/mono    (11207):   at Android.Widget.TextView.set_Text (IEnumerable`1 value)
E/mono    (11207):   at MonoDroidDebugging.Activity1.SlowMethod ()
```

これは、GUI のスレッドから GUI を更新する必要があるためです。 このコードは、スレッド プールのスレッドから、アプリのクラッシュの原因のフル インストールを更新します。 バック グラウンド スレッドで、値の計算がで処理されて GUI のスレッドで、更新を行う必要があります[Activity.RunOnUIThread](https://developer.xamarin.com/api/member/Android.App.Activity.RunOnUiThread/(System.Action)):

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

このコードは、期待どおりに動作します。 この GUI では、応答性の高いままになるし、計算が特殊したら適切に更新を取得します。

負荷の高い値を計算するのには、この手法は使用されませんだけに注意してください。 Web サービスの呼び出しまたはインターネットのデータをダウンロードするように、バック グラウンドで実行できる実行時間の長いタスクに対して使用できます。
