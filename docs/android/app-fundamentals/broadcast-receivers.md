---
title: Xamarin.Android でブロードキャスト レシーバー
description: このセクションでは、ブロードキャスト レシーバーを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: B2727160-12F2-43EE-84B5-0B15C8FCF4BD
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/20/2018
ms.openlocfilehash: 51bd3dd4c27dce19344f7660c31a0d4e741e1ad4
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50121140"
---
# <a name="broadcast-receivers-in-xamarinandroid"></a>Xamarin.Android でブロードキャスト レシーバー

_このセクションでは、ブロードキャスト レシーバーを使用する方法について説明します。_

## <a name="broadcast-receiver-overview"></a>ブロードキャスト レシーバーの概要

A_ブロードキャスト レシーバー_ Android コンポーネントにより、メッセージに応答するアプリケーションです (Android [ `Intent` ](https://developer.xamarin.com/api/type/Android.Content.Intent/)) Android オペレーティング システムまたはアプリケーションをブロードキャストします。 以下のブロードキャストを_パブリッシュ/サブスクライブ_モデル&ndash;イベントが発行され、受信したイベントに関心のあるこれらのコンポーネントにブロードキャストします。

Android では、ブロードキャストの 2 つの種類を識別します。

* **明示的なブロードキャスト**&ndash;ブロードキャストのこれらの型は、特定のアプリケーションを対象します。 明示的なブロードキャストの最も一般的な用途では、アクティビティを開始します。 アプリが電話番号をダイヤルする必要がある場合は、明示的なブロードキャストの例インテントをダイヤルするには、Android とパスに沿って、電話番号、電話アプリを対象とすることはディスパッチします。 Android は、Phone アプリに意図がルーティングされます。
* **暗黙的なブロードキャスト**&ndash;ブロードキャストは、デバイス上のすべてのアプリにディスパッチされます。 暗黙のブロードキャストの例は、`ACTION_POWER_CONNECTED`インテントです。 この目的は、Android デバイスのバッテリが充電ことを検出するたびに発行されます。 Android では、この目的をこのイベントに対して登録されているすべてのアプリにルーティングします。

ブロードキャスト レシーバーのサブクラスは、`BroadcastReceiver`型とそれをオーバーライドする必要があります、 [ `OnReceive` ](https://developer.xamarin.com/api/member/Android.Content.BroadcastReceiver.OnReceive/p/Android.Content.Context/Android.Content.Intent/)メソッド。 Android を実行`OnReceive`のメイン スレッドでそのためこのメソッドを設計する高速に実行します。 内のスレッドを起動するときは注意する必要がある`OnReceive`のため、メソッドが完了すると、Android は、プロセスを終了可能性があります。 かどうかには、ブロードキャスト レシーバーは、実行時間の長い作業を実行する必要がありますし、スケジュールを設定することを推奨、_ジョブ_を使用して、`JobScheduler`または_Firebase ジョブ ディスパッチャー_します。 ジョブでの作業のスケジュール設定については、別のガイドで説明します。

_インテント フィルター_ブロードキャスト レシーバーを登録し、Android は、メッセージを正しくルーティングできるようにするために使用します。 実行時にインテント フィルターを指定できます (これとも呼ば、_受信者のコンテキストに登録された_または_動的登録_)、Android マニフェスト (、で静的に定義することができます_レシーバーのマニフェストに登録された_)。 Xamarin.Android の提供、C#属性、 `IntentFilterAttribute`、(このガイドの後半で詳しくは、この説明は)、インテント フィルターは静的に登録します。 Android 8.0 以降、ことはできませんの暗黙のブロードキャストを静的に登録するアプリケーションです。

マニフェストに登録された受信機とコンテキスト登録の受信者の間の主な違いは、マニフェストに登録された受信者が応答できるときに、アプリケーションが実行中に、コンテキスト登録の受信者がブロードキャストに応答のみアプリが実行されていない場合でもをブロードキャストします。  

ブロードキャスト レシーバーを管理およびブロードキャストを送信するためには 2 つの Api のセットがあります。

1. **`Context`** &ndash; `Android.Content.Context`システム全体のイベントに応答するブロードキャスト レシーバーを登録するクラスを使用できます。 `Context`システム全体のブロードキャストの発行にも使用されます。
2. **[`LocalBroadcastManager`](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html#sendBroadcast(android.content.Intent))** &ndash; これは、API で使用可能な[v4 NuGet パッケージの Xamarin サポート ライブラリ](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)します。 このクラスは、ブロードキャスト、ブロードキャスト レシーバーがそれらを使用しているアプリケーションのコンテキスト内で分離の保持に使用されます。 このクラスは、他のアプリケーションがアプリケーション専用のブロードキャストに応答してまたはプライベートの受信者にメッセージの送信を回避するために役立ちます。

ブロードキャスト レシーバーは、ダイアログを表示できない可能性があり、ブロードキャスト レシーバー内からアクティビティの開始を強くお勧めします。 ブロードキャスト レシーバーは、ユーザーに通知する必要がありますは通知を発行する必要があります。

場合によっては、バインドまたはブロードキャスト レシーバー内からサービスを開始することはできません。 

このガイドは、ブロードキャスト レシーバーを作成する方法とブロードキャストを受け取ることができるように登録する方法について説明します。

## <a name="creating-a-broadcast-receiver"></a>ブロードキャスト レシーバーを作成します。

アプリケーション Xamarin.Android では、ブロードキャスト レシーバーを作成するにはサブクラス化する必要があります、`BroadcastReceiver`クラスを装飾することで、`BroadcastReceiverAttribute`をオーバーライドし、`OnReceive`メソッド。

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

Xamarin.Android では、クラスをコンパイルするときに、受信側を登録するために必要なメタ データと共に、AndroidManifest も更新されます。 静的に登録されているブロードキャスト レシーバーの`Enabled`適切に設定する必要があります`true`、それ以外の場合、Android によって、受信側のインスタンスを作成できません。
 
`Exported`プロパティは、ブロードキャスト レシーバーがアプリケーションの外部からのメッセージを受信できるかどうかを制御します。 プロパティが明示的に設定されていない場合、プロパティの既定値は Android ブロードキャスト レシーバーに関連付けられているそのインテント フィルターがない場合に基づくによって決まります。 ブロードキャスト レシーバーに少なくとも 1 つのインテント フィルターがあるかどうかは、Android はいると仮定して、`Exported`プロパティは`true`します。 インテント フィルターに関連付けられた、ブロードキャスト レシーバーがないかどうかは、Android では、値があると想定されます`false`します。 

`OnReceive`メソッドへの参照を受け取る、`Intent`ブロードキャスト レシーバーにディスパッチするでした。 これにより、ブロードキャスト レシーバーに値を渡す目的の送信者に関することができます。

### <a name="statically-registering-a-broadcast-receiver-with-an-intent-filter"></a>インテント フィルターを使用してブロードキャスト レシーバーを静的に登録します。

ときに、`BroadcastReceiver`で修飾されたが、 [ `IntentFilterAttribute` ](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/)、Xamarin.Android は、必要に応じて、追加`<intent-filter>`要素、Android をコンパイル時にマニフェスト。 次のスニペットでは、デバイスの (ユーザーが Android の適切な権限が付与された) 場合の起動が完了したときに実行するブロードキャスト レシーバーの例を示します。

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

カスタムのインテントに応答するインテント フィルターを作成することもできます。 次に例を示します。 

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

Android 8.0 (API レベル 26) を対象とするアプリ以降に静的に登録できません暗黙のブロードキャストまたはします。 アプリは、明示的なブロードキャストも静的を登録することがあります。 この制限から除外される暗黙のブロードキャストの小規模なリストがあります。 これらの例外が記載されて、[ブロードキャストの暗黙的な例外](https://developer.android.com/guide/components/broadcast-exceptions.html)Android ドキュメントのガイド。 暗黙的なブロードキャストに関心のあるアプリを使用して動的に行う必要があります、`RegisterReceiver`メソッド。 これは次に説明します。

### <a name="context-registering-a-broadcast-receiver"></a>ブロードキャスト レシーバーのコンテキストの登録

コンテキスト (動的な登録とも呼ばれます) の登録、受信側が呼び出すことによって実行、`RegisterReceiver`メソッドをおよびブロードキャスト レシーバーがへの呼び出しに登録する必要がありますが、`UnregisterReceiver`メソッド。 リソースのリークを防ぐためが (アクティビティまたはサービス) のコンテキストに関連がなくなったとき、受信側の登録を解除する重要です。 たとえば、サービスをユーザーに表示される更新プログラムが利用できるアクティビティを通知するためにインテントをブロードキャストする可能性があります。 アクティビティの開始時に、インテントを登録します。 ときに、アクティビティがバック グラウンドに移動し、ユーザーには表示されなく、それを登録解除する、受信側、更新プログラムを表示するための UI が表示されないため、します。 次のコード スニペットでは、登録およびアクティビティのコンテキストでブロードキャスト レシーバーの登録を解除する方法の例を示します。

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

前の例では、アクティビティが、フォア グラウンドには登録を使用してカスタムのインテントをリッスンするブロードキャスト レシーバー、`OnResume`ライフ サイクル メソッド。 アクティビティは、バック グラウンドに移動すると、`OnPause()`メソッドは、受信側の登録を解除します。

## <a name="publishing-a-broadcast"></a>ブロードキャストの発行

インテント オブジェクトを作成して、ディスパッチでデバイスにインストールされているすべてのアプリに発行できるブロードキャスト、`SendBroadcast`または`SendOrderedBroadcast`メソッド。  

1. **Context.SendBroadcast メソッド**&ndash;はこのメソッドのいくつかの実装があります。
   これらのメソッドは、システム全体にインテントをブロードキャストします。 目的が、不定な順序で受信するブロードキャスト レシーバー。 これにより、幅広い柔軟性を提供しますは、他のアプリケーションを登録し、目的の受信を意味します。 潜在的なセキュリティ リスクが生じます。 アプリケーションは、不正アクセスを防止する追加のセキュリティを実装する必要があります。 ソリューションの 1 つは、使用する、`LocalBroadcastManager`これはのみ、アプリのプライベート領域内のメッセージをディスパッチします。 このコード スニペットは、のいずれかを使用して、インテントをディスパッチする方法の 1 つの例、`SendBroadcast`メソッド。

   ```csharp
   Intent message = new Intent("com.xamarin.example.TEST");
   // If desired, pass some values to the broadcast receiver.
   message.PutExtra("key", "value");
   SendBroadcast(message);
   ```

    このスニペットを使用してブロードキャストを送信する別の例とは、`Intent.SetAction`アクションを識別するメソッド。

    ```csharp
    Intent intent = new Intent();
    intent.SetAction("com.xamarin.example.TEST");
    intent.PutExtra("key", "value");
    SendBroadcast(intent);
    ```

2. **Context.SendOrderedBroadcast** &ndash;メソッドは、これとよく似ています`Context.SendBroadcast`目的が時、受信側が登録されている順序での受信側に公開された 1 つになるに違いです。

### <a name="localbroadcastmanager"></a>LocalBroadcastManager

[Xamarin サポート ライブラリ v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)というヘルパー クラスを提供します。 [ `LocalBroadcastManager`](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html)します。 `LocalBroadcastManager`送信またはデバイス上の他のアプリからのブロードキャストを受信したくないアプリが対象としています。 `LocalBroadcastManager`に登録されているこれらのブロードキャスト レシーバーにのみ、アプリケーションのコンテキスト内でのメッセージを発行するのみ、`LocalBroadcastManager`します。 このコード スニペットでブロードキャスト レシーバーを登録する例は、 `LocalBroadcastManager`:

```csharp
Android.Support.V4.Content.LocalBroadcastManager.GetInstance(this). RegisterReceiver(receiver, new IntentFilter("com.xamarin.example.TEST"));
```

デバイス上の他のアプリにで公開されているメッセージが表示されることはできません、`LocalBroadcastManager`します。 このコード スニペットは、インテントを使用してディスパッチする方法を示しています、 `LocalBroadcastManager`:

```csharp
Intent message = new Intent("com.xamarin.example.TEST");
// If desired, pass some values to the broadcast receiver.
message.PutExtra("key", "value");
Android.Support.V4.Content.LocalBroadcastManager.GetInstance(this).SendBroadcast(message);
```

## <a name="related-links"></a>関連リンク

- [BroadcastReceiver API](https://developer.xamarin.com/api/type/Android.Content.BroadcastReceiver/)
- [Context.RegisterReceiver API](https://developer.xamarin.com/api/member/Android.Content.Context.RegisterReceiver/p/Android.Content.BroadcastReceiver/Android.Content.IntentFilter/System.String/Android.OS.Handler/)
- [Context.SendBroadcast API](https://developer.xamarin.com/api/member/Android.Content.Context.SendBroadcast/p/Android.Content.Intent/)
- [Context.UnregisterReceiver API](https://developer.xamarin.com/api/member/Android.Content.Context.UnregisterReceiver/p/Android.Content.BroadcastReceiver/)
- [インテント API](https://developer.xamarin.com/api/type/Android.Content.Intent/)
- [IntentFilter API](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/)
- [LocalBroadcastManager (Android docs)](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html#sendBroadcast(android.content.Intent))
- [Android でのローカル通知](~/android/app-fundamentals/notifications/local-notifications.md)ガイド
- [Android サポート ライブラリ v4 (NuGet)](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)
