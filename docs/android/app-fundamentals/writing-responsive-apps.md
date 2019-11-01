---
title: 応答性の高いアプリケーションの作成
ms.prod: xamarin
ms.assetid: 452DF940-6331-55F0-D130-002822BBED55
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/15/2018
ms.openlocfilehash: 1900a4fc42778db07e78c41bbc0acfafbd594cdf
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73024188"
---
# <a name="writing-responsive-applications"></a>応答性の高いアプリケーションの作成

応答性の高い GUI を維持するための鍵の1つは、長時間実行されるタスクをバックグラウンドスレッドで実行して、GUI がブロックされないようにすることです。 たとえば、ユーザーに表示する値を計算するとしますが、その値は計算に5秒かかります。

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

これは機能しますが、値が計算されている間、アプリケーションは5秒間 "ハング" します。 この間、アプリはどのユーザーの操作にも応答しません。 この問題を回避するには、バックグラウンドスレッドで計算を実行します。

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

次に、バックグラウンドスレッドで値を計算します。これにより、計算中に GUI の応答性が維持されます。 ただし、計算が完了すると、アプリがクラッシュし、次のようにログに残ります。

```shell
E/mono    (11207): EXCEPTION handling: Android.Util.AndroidRuntimeException: Exception of type 'Android.Util.AndroidRuntimeException' was thrown.
E/mono    (11207):
E/mono    (11207): Unhandled Exception: Android.Util.AndroidRuntimeException: Exception of type 'Android.Util.AndroidRuntimeException' was thrown.
E/mono    (11207):   at Android.Runtime.JNIEnv.CallVoidMethod (IntPtr jobject, IntPtr jmethod, Android.Runtime.JValue[] parms)
E/mono    (11207):   at Android.Widget.TextView.set_Text (IEnumerable`1 value)
E/mono    (11207):   at MonoDroidDebugging.Activity1.SlowMethod ()
```

これは、gui スレッドから GUI を更新する必要があるためです。 このコードによって、ThreadPool スレッドから GUI が更新され、アプリがクラッシュします。 バックグラウンドスレッドで値を計算する必要がありますが、GUI スレッドで更新を行う必要があります。これは、 [Activity. RunOnUIThread](xref:Android.App.Activity.RunOnUiThread*)で処理されます。

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

このコードは想定どおりに動作します。 この GUI は応答性を維持し、計算が特殊されると適切に更新されます。

メモこの手法は、高価な値の計算には使用されません。 これは、web サービス呼び出しやインターネットデータのダウンロードなど、バックグラウンドで実行できる実行時間の長いタスクに使用できます。
