---
title: "Xamarin.Android で放送受信機"
description: "このセクションでは、放送受信機を使用する方法について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: B2727160-12F2-43EE-84B5-0B15C8FCF4BD
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 03/09/2018
ms.openlocfilehash: b2da136ddfa6aab4121ba21d0e6f83b2390ba10b
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2018
---
# <a name="broadcast-receivers-in-xamarinandroid"></a>Xamarin.Android で放送受信機

_このセクションでは、放送受信機を使用する方法について説明します。_


## <a name="broadcast-receiver-overview"></a>放送受信機の概要

A_放送受信機_Android コンポーネントにより、メッセージに応答するアプリケーションです (Android [ `Intent` ](https://developer.xamarin.com/api/type/Android.Content.Intent/))、Android オペレーティング システムまたはアプリケーションによっては、ブロードキャストをします。 次のブロードキャスト、_公開/定期受信_モデル&ndash;イベントの原因が発行され、イベントに関心のあるこれらのコンポーネントが受信したブロードキャストします。 

Android では、ブロードキャストの 2 つのカテゴリを識別します。

* **通常のブロードキャスト**&ndash;通常ブロードキャストは不定な順序で登録されているすべてのブロードキャスト受信者にルーティングされます。 各受信者は、任意の順序を目的に表示されます。 
* **順序付けにブロードキャスト**&ndash;順序付けられたブロードキャストは一度に 1 つを登録済みの受信側に配信されます。 目的が受信したときにブロードキャストの受信者が目的を変更できます。 またはブロードキャストを終了できます。

ブロードキャストの受信者のサブクラスは、`BroadcastReceiver`クラスおよびそれをオーバーライドする必要があります、 [ `OnReceive` ](https://developer.xamarin.com/api/member/Android.Content.BroadcastReceiver.OnReceive/p/Android.Content.Context/Android.Content.Intent/)メソッドです。 Android を実行`OnReceive`メイン スレッドでそのためこの方法を設計する高速に実行します。 内のスレッドを起動するときは注意する必要がある`OnReceive`のため、メソッドの終了時に、Android がプロセスを終了させる可能性があります。 放送受信機が長時間実行される作業を実行する必要があるかどうかは、スケジュールを設定することをお勧め、_ジョブ_を使用して、`JobScheduler`または_Firebase ジョブ ディスパッチャー_です。 ジョブに作業をスケジュール設定については別のガイドで説明します。

_インテント フィルター_ Android は、メッセージを正しくルーティングできるように放送受信機の登録に使用します。 実行時にインテントのフィルターを指定することができます (これとも呼ば、_受信者のコンテキストに登録された_または_動的登録_)、Android マニフェスト (、で静的に定義することができます_レシーバーのマニフェストに登録された_)。 Xamarin.Android 提供 c# 属性、 `IntentFilterAttribute`、意図的に (このガイドの後半で詳しくは、この説明は) フィルターを静的に登録します。 

マニフェストに登録された受信機とコンテキストに登録された受信機の主な違いにマニフェストに登録された受信者が応答できるときに、アプリケーションが実行中に、コンテキストに登録された受信者がブロードキャストに応答のみアプリが実行されていない場合でもをブロードキャストします。  

ブロードキャストの受信者を管理およびブロードキャストを送信するためには 2 セットの Api があります。

1. **`Context`** &ndash; `Android.Content.Context`システム全体のイベントに応答する放送受信機を登録するクラスを使用できます。 `Context`はシステム全体のブロードキャストの発行にも使用します。
2. **[`LocalBroadcastManager`](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html#sendBroadcast(android.content.Intent))** &ndash; これは、API を介して使用可能な[v4 NuGet パッケージの Xamarin サポート ライブラリ](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)です。 このクラスは、ブロードキャストおよびブロードキャストの受信側がそれらを使用しているアプリケーションのコンテキスト内に分離するために使用されます。 このクラスは、他のアプリケーションのアプリケーション専用のブロードキャストに応答しておよびプライベートの受信者にメッセージの送信を回避するために役立ちます。

放送受信機は、のダイアログ ボックスを表示できない可能性があり、ブロードキャスト レシーバー内からアクティビティを開始する強くお勧めします。 放送受信機は、ユーザーに通知する必要がありますは通知を発行する必要があります。

バインドまたはブロードキャスト レシーバー内からのサービスを開始することはできません。 

このガイドはブロードキャスト レシーバーを作成する方法とブロードキャストが生じるようにに登録する方法について説明します。

## <a name="creating-a-broadcast-receiver"></a>ブロードキャスト レシーバーを作成します。

Xamarin.Android でブロードキャスト レシーバーを作成するには、アプリケーションは、サブクラスを必要があります、`BroadcastReceiver`クラスを装飾することで、 `BroadcastReceiverAttribute`、オーバーライド、`OnReceive`メソッド。

```csharp
[BroadcastReceiver(Enabled = true, Exported = false)]
public class SampleReceiver : BroadcastReceiver
{
    public override void OnReceive(Context context, Intent intent)
    {
        // Do stuff here.
        
        String value = intent.GetStringExtra("key");
    }
}
```

Xamarin.Android は、クラスをコンパイルするときに、受信側を登録するために必要なメタ データと共に、AndroidManifest も更新されます。 静的に登録されているブロードキャストの受信者を表す、`Enabled`適切に設定する必要があります`true`、それ以外の場合、Android によって、受信側のインスタンスを作成できません。
 
`Exported`プロパティはブロードキャストの受信側がアプリケーションの外部からのメッセージを受信できるかどうかを制御します。 プロパティが明示的に設定されていない場合、プロパティの既定値はブロードキャストの受信者に関連付けられた任意の目的フィルターがある場合に基づく Android によって決まります。 放送受信機のには、少なくとも 1 つの目的としたフィルターがあるかどうかは、Android と見なされます、`Exported`プロパティは`true`します。 放送受信機に関連付けられたを目的としたフィルターがないかどうかは、Android では、値があると想定されます`false`です。 

`OnReceive`メソッドへの参照を受け取る、`Intent`放送受信機をディスパッチされました。 これによりは、ブロードキャストの受信者に値を渡すことを目的の送信者の可能性があります。

### <a name="statically-registering-a-broadcast-receiver-with-an-intent-filter"></a>インテント フィルターを使用して放送受信機を静的に登録します。

ときに、`BroadcastReceiver`で修飾された、 [ `IntentFilterAttribute` ](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/)、Xamarin.Android は、必要な追加`<intent-filter>`要素、Android をコンパイル時にマニフェスト。 次のスニペットは、デバイスの場合、ユーザーから、Android の適切なアクセス許可が付与された) の起動が完了したときに実行される放送受信機の例を示します。

```csharp
[BroadcastReceiver(Enabled = true)]
[IntentFilter(new[] { Android.Content.Intent.ActionBootCompleted })]
public class MyBootReceiver : BroadcastReceiver
{
    public override void OnReceive(Context context, Intent intent)
    {
        // Work that should be done when the device boots.     
    }
}
```

カスタムの目的に応答するインテント フィルターを作成することもできます。 次に例を示します。 

```csharp
[BroadcastReceiver(Enabled = true)]
[IntentFilter(new[] { "com.xamarin.example.TEST" })]
public class MySampleBroadcastReceiver : BroadcastReceiver
{
    public override void OnReceive(Context context, Intent intent)
    {
        // Do stuff here
    }
}
```

### <a name="context-registering-a-broadcast-receiver"></a>ブロードキャストの受信者のコンテキストの登録 

呼び出すことによって、受信者のコンテキストの登録が実行される、`RegisterReceiver`メソッド、およびブロードキャストの受信者がへの呼び出しに登録されている必要がある、`UnregisterReceiver`メソッドです。 リソースのリークを防ぐためには、コンテキストに関連がある不要になったときに、受信者の登録を解除する必要があります。 たとえば、サービスはユーザーに表示される更新プログラムが利用できるアクティビティに通知することを目的をブロードキャストする可能性があります。 アクティビティを開始するときは、それらの意図的の登録はします。 ときに、アクティビティは、バック グラウンドに移動されが表示されない、ユーザーに、それを登録解除する受信側、更新プログラムを表示するための UI が表示されないためです。 次のコード スニペットは、登録して、アクティビティのコンテキストで放送受信機の登録を解除する方法の例を示します。

```csharp
[Activity(Label = "MainActivity", MainLauncher = true, Icon = "@mipmap/icon")]
public class MainActivity: Activity 
{
    MySampleBroadcastReceiver receiver;
    
    protected override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);
        receiver = new MySampleBroadcastReceiver()
        
        // Code omitted for clarity
    }
    
    protected override OnResume() 
    {
        base.OnResume();
        RegisterReceiver(receiver, new IntentFilter("com.xamarin.example.TEST"));
        // Code omitted for clarity
    }
    
    protected override OnPause() 
    {
        UnregisterReceiver(receiver);
        // Code omitted for clarity
        base.OnPause();
    }
}
```

前の例で、前景色を入手したとき、アクティビティが登録されますを使用してカスタムの目的をリッスンする放送受信機、`OnResume`ライフ サイクル メソッドです。 アクティビティは、バック グラウンドに移動すると、`OnPause()`メソッドは、受信側に登録を解除します。

## <a name="publishing-a-broadcast"></a>ブロードキャストの発行

ブロードキャストがカプセル化することによって発行された、_アクション_目的、および 2 つの Api のいずれかのディスパッチで。 

1. **[`LocalBroadcastManager`](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html#sendBroadcast(android.content.Intent))** &ndash; 意図的に公開されている、`LocalBroadcastManager`のみです。 アプリケーションで受信される他のアプリケーションにルーティングされません。 現在のアプリケーション内でのインテントを保持することで、追加のセキュリティ レベルを提供され、プロセス間呼び出しに関連するオーバーヘッドがないものはすべてインプロセスのためので、優先される、この必要があります。 このコード スニペットがアクティビティからインテントを使用して、ディスパッチする方法を示しています、 `LocalBroadcastManager`:

   ```csharp
   Intent message = new Intent("com.xamarin.example.TEST");
   // If desired, pass some values to the broadcast receiver.
   intent.PutExtra("key", "value");
   Android.Support.V4.Content.LocalBroadcastManager.GetInstance(this).SendBroadcast(message);
   ```

2. **[`Context.SendBroadcast`](https://developer.xamarin.com/api/member/Android.Content.Context.SendBroadcast/p/Android.Content.Intent/) メソッド**&ndash;このメソッドの実装がいくつかがあります。
   これらのメソッドは、システム全体にインテントをブロードキャストします。 これにより、柔軟性が大幅に向上を提供が他のアプリケーションの登録可能性があります、アプリケーションを受信することを意味します。 これにより、潜在的なセキュリティ リスクがもたらされます。 アプリケーションは、目的への未承認のアクセスを確保する追加のセキュリティを実装する必要があります。 このコード スニペットは 1 つのいずれかを使用して、目的をディスパッチする方法の例、`SendBroadcast`メソッド。

   ```csharp
   Intent message = new Intent("com.xamarin.example.TEST");
   // If desired, pass some values to the broadcast receiver.
   intent.PutExtra("key", "value");
   SendBroadcast(intent);
   ```
        
> [!NOTE]
> LocalBroadcastManager はから利用できる、 [Xamarin サポート ライブラリ v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) NuGet パッケージです。 

このスニペットは、ブロードキャストを使用して、送信の別の例、`Intent.SetAction`アクションを識別する方法。

```csharp 
Intent intent = new Intent();
intent.SetAction("com.xamarin.example.TEST");
intent.PutExtra("key", "value");
SendBroadcast(intent);
```



## <a name="related-links"></a>関連リンク

- [BroadcastReceiver](https://developer.xamarin.com/api/type/Android.Content.BroadcastReceiver/)
- [Context.RegisterReceiver](https://developer.xamarin.com/api/member/Android.Content.Context.RegisterReceiver/p/Android.Content.BroadcastReceiver/Android.Content.IntentFilter/System.String/Android.OS.Handler/)
- [Context.SendBroadcast](https://developer.xamarin.com/api/member/Android.Content.Context.SendBroadcast/p/Android.Content.Intent/)
- [Context.UnregisterReceiver](https://developer.xamarin.com/api/member/Android.Content.Context.UnregisterReceiver/p/Android.Content.BroadcastReceiver/)
- [目的](https://developer.xamarin.com/api/type/Android.Content.Intent/)
- [IntentFilter](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/)
- [LocalBroadcastManager](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html#sendBroadcast(android.content.Intent))
- [Android でのローカルの通知](~/android/app-fundamentals/notifications/local-notifications.md)
- [Android ライブラリ v4 のサポート](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)
