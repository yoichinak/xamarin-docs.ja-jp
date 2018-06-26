---
title: Xamarin.Android で放送受信機
description: このセクションでは、放送受信機を使用する方法について説明します。
ms.prod: xamarin
ms.assetid: B2727160-12F2-43EE-84B5-0B15C8FCF4BD
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 04/20/2018
ms.openlocfilehash: 6b2e316eaf67e51801be4fcd670e80ec81c8ff08
ms.sourcegitcommit: 26033c087f49873243751deded8037d2da701655
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/25/2018
ms.locfileid: "36935400"
---
# <a name="broadcast-receivers-in-xamarinandroid"></a>Xamarin.Android で放送受信機

_このセクションでは、放送受信機を使用する方法について説明します。_

## <a name="broadcast-receiver-overview"></a>放送受信機の概要

A_放送受信機_Android コンポーネントにより、メッセージに応答するアプリケーションです (Android [ `Intent` ](https://developer.xamarin.com/api/type/Android.Content.Intent/))、Android オペレーティング システムまたはアプリケーションによっては、ブロードキャストをします。 次のブロードキャスト、_公開/定期受信_モデル&ndash;イベントの原因が発行され、イベントに関心のあるこれらのコンポーネントが受信したブロードキャストします。 

Android では、2 種類のブロードキャストを識別します。

* **明示的なブロードキャスト**&ndash;これらの種類のブロードキャストの特定のアプリケーションを対象とします。 明示的なブロードキャストの最も一般的な用途は、アクティビティを開始します。 アプリが電話番号をダイヤルする必要がある場合は、明示的なブロードキャストの例ダイヤルするには、Android と電話番号に沿ったパスの Phone アプリを対象とするインテントをディスパッチ、されます。 Android は、電話アプリに、インテントがルーティングされます。
* **暗黙的なブロードキャスト**&ndash;これらのブロードキャストは、デバイス上のすべてのアプリケーションにディスパッチされます。 暗黙のブロードキャストの例は、`ACTION_POWER_CONNECTED`目的としました。 この目的は、Android デバイスのバッテリが充電ことを検出するたびに発行されます。 Android では、このイベントに対して登録されているすべてのアプリをこの目的をルーティングします。

ブロードキャストの受信者のサブクラスは、`BroadcastReceiver`型およびそれをオーバーライドする必要があります、 [ `OnReceive` ](https://developer.xamarin.com/api/member/Android.Content.BroadcastReceiver.OnReceive/p/Android.Content.Context/Android.Content.Intent/)メソッドです。 Android を実行`OnReceive`メイン スレッドでそのためこの方法を設計する高速に実行します。 内のスレッドを起動するときは注意する必要がある`OnReceive`のため、メソッドの終了時に、Android がプロセスを終了させる可能性があります。 放送受信機が長時間実行される作業を実行する必要があるかどうかは、スケジュールを設定することをお勧め、_ジョブ_を使用して、`JobScheduler`または_Firebase ジョブ ディスパッチャー_です。 ジョブに作業をスケジュール設定については別のガイドで説明します。

_インテント フィルター_ Android は、メッセージを正しくルーティングできるように放送受信機の登録に使用します。 実行時にインテントのフィルターを指定することができます (これとも呼ば、_受信者のコンテキストに登録された_または_動的登録_)、Android マニフェスト (、で静的に定義することができます_レシーバーのマニフェストに登録された_)。 Xamarin.Android 提供 c# 属性、 `IntentFilterAttribute`、意図的に (このガイドの後半で詳しくは、この説明は) フィルターを静的に登録します。 Android 8.0 以降、ことはできません、アプリケーションを暗黙のブロードキャストを静的に登録します。

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

### <a name="statically-registering-a-broadcast-receiver-with-an-intent-filter"></a>目的としたフィルターを使用して放送受信機を静的に登録します。

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

Android 8.0 (API レベル 26) を対象とするアプリまたは高い可能性がありますいない静的に登録暗黙のブロードキャストのです。 アプリは、明示的なブロードキャストの可能性があります登録静的のままにします。 この制限から除外される暗黙のブロードキャストの小さいリストがあります。 これらの例外に記載されて、[ブロードキャストの暗黙的な例外](https://developer.android.com/guide/components/broadcast-exceptions.html)Android ドキュメントのガイドです。 暗黙的なブロードキャストに関心のあるアプリを使用して動的に行う必要があります、`RegisterReceiver`メソッドです。 これは次に説明します。

### <a name="context-registering-a-broadcast-receiver"></a>ブロードキャストの受信者のコンテキストの登録

呼び出してコンテキスト (動的登録とも呼ばれる) の登録、受信側が実行される、`RegisterReceiver`メソッド、およびブロードキャストの受信者がへの呼び出しに登録されている必要がある、`UnregisterReceiver`メソッドです。 リソースのリークを防ぐためには、することが重要 (アクティビティまたはサービス) のコンテキストに関連が不要になったときに、受信者の登録を解除します。 たとえば、サービスはユーザーに表示される更新プログラムが利用できるアクティビティに通知することを目的をブロードキャストする可能性があります。 アクティビティを開始するときは、それらの意図的の登録はします。 ときに、アクティビティは、バック グラウンドに移動されが表示されない、ユーザーに、それを登録解除する受信側、更新プログラムを表示するための UI が表示されないためです。 次のコード スニペットは、登録して、アクティビティのコンテキストで放送受信機の登録を解除する方法の例を示します。

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

ブロードキャストはインテント オブジェクトを作成してでのディスパッチのデバイスにインストールされているすべてのアプリに発行できるようにする、`SendBroadcast`または`SendOrderedBroadcast`メソッドです。  

1. **Context.SendBroadcast メソッド**&ndash;このメソッドの実装がいくつかがあります。
   これらのメソッドは、システム全体にインテントをブロードキャストします。 放送受信機のためには、不定な順序で、目的が表示されます。 柔軟性が大幅に向上にはこれが他のアプリケーションを登録し、目的が表示されることを意味します。 これにより、潜在的なセキュリティ リスクがもたらされます。 アプリケーションは、未承認のアクセスを防ぐために追加のセキュリティを実装する必要があります。 1 つの考えられる解決方法は、使用する、`LocalBroadcastManager`のみ、アプリのプライベート空間内のメッセージのディスパッチをされます。 このコード スニペットは 1 つのいずれかを使用して、目的をディスパッチする方法の例、`SendBroadcast`メソッド。

   ```csharp
   Intent message = new Intent("com.xamarin.example.TEST");
   // If desired, pass some values to the broadcast receiver.
   intent.PutExtra("key", "value");
   SendBroadcast(intent);
   ```

    このスニペットは、ブロードキャストを使用して、送信の別の例、`Intent.SetAction`アクションを識別する方法。

    ```csharp 
    Intent intent = new Intent();
    intent.SetAction("com.xamarin.example.TEST");
    intent.PutExtra("key", "value");
    SendBroadcast(intent);
    ```

2. **Context.SendOrderedBroadcast** &ndash;メソッドは、これとよく似ています`Context.SendBroadcast`目的が、recievers が登録されている順序でレシーバーを一度に 1 つの公開済みになる、違いです。

### <a name="localbroadcastmanager"></a>LocalBroadcastManager

[Xamarin サポート ライブラリ v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)と呼ばれるヘルパー クラスを提供[ `LocalBroadcastManager`](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html)です。 `LocalBroadcastManager`を送信するか、デバイス上の他のアプリからブロードキャストを受信しないようにするアプリのためのものでは、します。 `LocalBroadcastManager`に登録されているこれらのブロードキャスト レシーバーにのみ、アプリケーションのコンテキスト内でメッセージを発行するのみ、`LocalBroadcastManager`です。 このコード スニペットはブロードキャストの受信者を登録する次の例`LocalBroadcastManager`:

```csharp
Android.Support.V4.Content.LocalBroadcastManager.GetInstance(this). RegisterReceiver(receiver, new IntentFilter("com.xamarin.example.TEST"));
```

その他のデバイスでアプリがで公開されているメッセージを受信できない、`LocalBroadcastManager`です。 このコード スニペットは、インテントを使用して、ディスパッチする方法を示しています、 `LocalBroadcastManager`:

```csharp
Intent message = new Intent("com.xamarin.example.TEST");
// If desired, pass some values to the broadcast receiver.
intent.PutExtra("key", "value");
Android.Support.V4.Content.LocalBroadcastManager.GetInstance(this).SendBroadcast(message);
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
