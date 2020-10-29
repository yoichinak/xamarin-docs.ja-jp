---
title: Xamarin. Android で受信者をブロードキャストする
description: ここでは、ブロードキャストレシーバーの使用方法について説明します。
ms.prod: xamarin
ms.assetid: B2727160-12F2-43EE-84B5-0B15C8FCF4BD
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/20/2018
ms.openlocfilehash: 9e702bdb31563588d20b9cd70e35b67aefe67e0c
ms.sourcegitcommit: d1980b2251999224e71c1289e4b4097595b7e261
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2020
ms.locfileid: "92928374"
---
# <a name="broadcast-receivers-in-xamarinandroid"></a>Xamarin. Android で受信者をブロードキャストする

_ここでは、ブロードキャストレシーバーの使用方法について説明します。_

## <a name="broadcast-receiver-overview"></a>ブロードキャストレシーバーの概要

_ブロードキャストレシーバー_ は、android [`Intent`](xref:Android.Content.Intent) オペレーティングシステムまたはアプリケーションによってブロードキャストされるメッセージ (android) にアプリケーションが応答できるようにする android コンポーネントです。 ブロードキャストは _パブリッシュ/サブスクライブ_ モデルに従います。イベントが発生すると、イベント &ndash; に関連するコンポーネントによってブロードキャストが公開され、受信されます。

Android は、次の2種類のブロードキャストを識別します。

- **明示的なブロードキャスト** &ndash; これらの種類のブロードキャストは、特定のアプリケーションを対象とします。 明示的なブロードキャストを使用する最も一般的な方法は、アクティビティを開始することです。 アプリで電話番号をダイヤルする必要がある場合の明示的なブロードキャストの例を次に示します。Android で電話アプリを対象とするインテントをディスパッチし、ダイヤルする電話番号に沿って渡すことができます。 その後、Android は目的を Phone アプリにルーティングします。
- **暗黙のブロードキャスト** &ndash; これらのブロードキャストは、デバイス上のすべてのアプリにディスパッチされます。 暗黙のブロードキャストの例としては、 `ACTION_POWER_CONNECTED` 意図があります。 このインテントは、デバイスのバッテリが充電されていることを Android が検出するたびに公開されます。 Android は、このイベントに登録されているすべてのアプリにこのインテントをルーティングします。

ブロードキャストレシーバーは型のサブクラスで `BroadcastReceiver` あり、メソッドをオーバーライドする必要があり [`OnReceive`](xref:Android.Content.BroadcastReceiver.OnReceive*) ます。 Android は `OnReceive` メインスレッドで実行されるため、このメソッドは迅速に実行するように設計する必要があります。 でスレッドを生成するときは、 `OnReceive` メソッドの終了時に Android がプロセスを終了する可能性があるため、注意が必要です。 ブロードキャストレシーバーで長時間実行される作業を実行する必要がある場合は _job_ 、 `JobScheduler` または _焼討 base ジョブディスパッチャー_ を使用してジョブのスケジュールを設定することをお勧めします。 ジョブでの作業のスケジュール設定については、別のガイドで説明します。

_インテントフィルター_ は、Android がメッセージを適切にルーティングできるように、ブロードキャストレシーバーを登録するために使用されます。 インテントフィルターは、実行時に指定できます (これは、 _コンテキスト登録の受信_ 者または _動的な登録_ と呼ばれることもあります)。また、Android マニフェスト (マニフェストに登録された _受信側_ ) で静的に定義することもできます。 Xamarin Android には、インテントフィルターを静的に登録する C# の属性が用意されています (この `IntentFilterAttribute` 詳細については、このガイドで後述します)。 Android 8.0 以降では、アプリケーションが暗黙的なブロードキャストに対して静的に登録することはできません。

マニフェスト登録受信側とコンテキスト登録受信側の主な違いは、コンテキスト登録された受信側はアプリケーションの実行中にブロードキャストに応答するだけで、マニフェスト登録済み受信者は、アプリが実行されていない場合でもブロードキャストに応答できることです。  

ブロードキャストレシーバーを管理し、ブロードキャストを送信するための Api には、次の2つのセットがあります。

1. **`Context`**&ndash;クラスは、 `Android.Content.Context` システム全体のイベントに応答するブロードキャストレシーバーを登録するために使用できます。 は、 `Context` システム全体のブロードキャストを発行するためにも使用されます。
2. **[`LocalBroadcastManager`](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html#sendBroadcast(android.content.Intent))**&ndash;これは、 [Xamarin サポートライブラリ v4 NuGet パッケージ](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)を通じて提供される API です。 このクラスは、ブロードキャストとブロードキャストレシーバーを、それらを使用しているアプリケーションのコンテキストで分離された状態に保つために使用されます。 このクラスは、他のアプリケーションがアプリケーションのみのブロードキャストに応答したり、メッセージをプライベート受信者に送信したりするのを防ぐために役立ちます。

ブロードキャストレシーバーではダイアログが表示されない場合があり、ブロードキャストレシーバー内からアクティビティを開始しないことを強くお勧めします。 ブロードキャストレシーバーがユーザーに通知する必要がある場合は、通知を発行する必要があります。

ブロードキャストレシーバー内からサービスにバインドしたり、サービスを開始したりすることはできません。

このガイドでは、ブロードキャストレシーバーを作成する方法と、ブロードキャストを受信できるように登録する方法について説明します。

## <a name="creating-a-broadcast-receiver"></a>ブロードキャストレシーバーの作成

Xamarin Android でブロードキャストレシーバーを作成するには、アプリケーションでクラスをサブクラス化し、を使用して装飾し、メソッドをオーバーライドする必要があり `BroadcastReceiver` `BroadcastReceiverAttribute` `OnReceive` ます。

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

Xamarin Android はクラスをコンパイルするときに、必要なメタデータを使用して AndroidManifest を更新し、受信者を登録します。 静的に登録されたブロードキャストレシーバーの場合、 `Enabled` をに設定する必要があります。そうしないと、 `true` Android は受信側のインスタンスを作成できません。

プロパティは、 `Exported` ブロードキャスト受信者がアプリケーションの外部からメッセージを受信できるかどうかを制御します。 プロパティが明示的に設定されていない場合、ブロードキャストレシーバーに関連付けられているインテントフィルターがあるかどうかに基づいて、プロパティの既定値が Android によって決定されます。 ブロードキャストレシーバーに対して少なくとも1つのインテントフィルターがある場合、Android ではプロパティがであると想定され `Exported` `true` ます。 ブロードキャストレシーバーに関連付けられているインテントフィルターがない場合、Android は値がであると想定し `false` ます。

メソッドは、 `OnReceive` `Intent` ブロードキャストレシーバーにディスパッチされたへの参照を受け取ります。 これにより、送信側がブロードキャストレシーバーに値を渡すことができるようになります。

### <a name="statically-registering-a-broadcast-receiver-with-an-intent-filter"></a>インテントフィルターを使用してブロードキャストレシーバーを静的に登録する

`BroadcastReceiver`がによって修飾されると [`IntentFilterAttribute`](xref:Android.App.IntentFilterAttribute) 、 `<intent-filter>` コンパイル時に必要な要素が android マニフェストに追加されます。 次のスニペットは、デバイスの起動が完了したときに実行されるブロードキャストレシーバーの例です (適切な Android アクセス許可がユーザーに付与されている場合)。

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

> [!NOTE]
> Android 8.0 (API 26 以降) では、ユーザーが直接対話していないときにアプリが実行できる操作に [制限が設け](https://developer.android.com/about/versions/oreo/background) られていました。 これらの制限は、バックグラウンドサービスと、などの暗黙的なブロードキャストレシーバーに影響し `Android.Content.Intent.ActionBootCompleted` ます。 これらの制限により、 `Boot Completed` 新しいバージョンの Android にブロードキャストレシーバーを登録する際に問題が発生する可能性があります。 この場合、これらの制限は、ブロードキャストレシーバーから呼び出すことができるフォアグラウンドサービスには適用されないことに注意してください。

カスタムインテントに応答するインテントフィルターを作成することもできます。 次の例を確認してください。

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

Android 8.0 (API レベル 26) 以降を対象とするアプリでは、暗黙的なブロードキャストに対して静的に登録できない場合があります。 アプリは、明示的なブロードキャストに対して静的に登録できます。 この制限から除外される暗黙のブロードキャストの小さな一覧があります。 これらの例外については、Android ドキュメントの「 [暗黙のブロードキャスト例外](https://developer.android.com/guide/components/broadcast-exceptions.html) 」ガイドで説明されています。 暗黙のブロードキャストに関心があるアプリは、メソッドを使用して動的に行う必要があり `RegisterReceiver` ます。 これについては次に説明します。

### <a name="context-registering-a-broadcast-receiver"></a>ブロードキャストレシーバーの Context-Registering

受信側のコンテキスト登録 (動的登録とも呼ばれます) は、メソッドを呼び出すことによって実行され `RegisterReceiver` ます。また、ブロードキャストレシーバーは、メソッドの呼び出しによって登録解除される必要があり `UnregisterReceiver` ます。 リソースのリークを防ぐために、コンテキスト (アクティビティまたはサービス) に関連しなくなった受信側の登録を解除することが重要です。 たとえば、サービスは、更新プログラムがユーザーに表示可能であることをアクティビティに通知するために、インテントをブロードキャストする場合があります。 アクティビティが開始されると、そのインテントに登録されます。 アクティビティがバックグラウンドに移動され、ユーザーに表示されなくなると、更新を表示するための UI が表示されなくなるため、受信者の登録を解除する必要があります。 次のコードスニペットは、アクティビティのコンテキストでブロードキャストレシーバーの登録と登録解除を行う方法の例です。

```csharp
[Activity(Label = "MainActivity", MainLauncher = true, Icon = "@mipmap/icon")]
public class MainActivity: Activity
{
    MySampleBroadcastReceiver receiver;

    protected override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);
        receiver = new MySampleBroadcastReceiver();

        // Code omitted for clarity
    }

    protected override void OnResume()
    {
        base.OnResume();
        RegisterReceiver(receiver, new IntentFilter("com.xamarin.example.TEST"));
        // Code omitted for clarity
    }

    protected override void OnPause()
    {
        UnregisterReceiver(receiver);
        // Code omitted for clarity
        base.OnPause();
    }
}
```

前の例では、アクティビティがフォアグラウンドに到着すると、ライフサイクルメソッドを使用してカスタムインテントをリッスンするブロードキャストレシーバーが登録され `OnResume` ます。 アクティビティがバックグラウンドに移行すると、 `OnPause()` メソッドは受信側の登録を解除します。

## <a name="publishing-a-broadcast"></a>ブロードキャストを公開する

デバイスにインストールされているすべてのアプリにブロードキャストが発行される可能性があります。インテントオブジェクトを作成し、メソッドまたはメソッドを使用してディスパッチし `SendBroadcast` `SendOrderedBroadcast` ます。  

1. **コンテキストの SendBroadcast メソッド** &ndash; このメソッドにはいくつかの実装があります。
   これらのメソッドは、システム全体にインテントをブロードキャストします。 インテントを不定な順序で受信するブロードキャストレシーバー。 これにより、多くの柔軟性が得られますが、他のアプリケーションが目的を登録して受信できるようになります。 これにより、セキュリティ上のリスクが生じる可能性があります。 アプリケーションでは、不正なアクセスを防ぐために追加のセキュリティを実装する必要がある場合があります。 考えられる解決策の1つは、 `LocalBroadcastManager` アプリのプライベート空間内でのみメッセージをディスパッチするを使用することです。 次のコードスニペットは、メソッドのいずれかを使用してインテントをディスパッチする方法の一例です `SendBroadcast` 。

   ```csharp
   Intent message = new Intent("com.xamarin.example.TEST");
   // If desired, pass some values to the broadcast receiver.
   message.PutExtra("key", "value");
   SendBroadcast(message);
   ```

    このスニペットは、メソッドを使用してアクションを識別することによってブロードキャストを送信するもう1つの例です  `Intent.SetAction` 。

    ```csharp
    Intent intent = new Intent();
    intent.SetAction("com.xamarin.example.TEST");
    intent.PutExtra("key", "value");
    SendBroadcast(intent);
    ```

2. **SendOrderedBroadcast** &ndash; This is メソッドはと非常によく似ていますが、その `Context.SendBroadcast` 目的は、レシーバーが登録された順序で、レシーバーに一度に1つずつ発行される点が異なります。

### <a name="localbroadcastmanager"></a>LocalBroadcastManager

[Xamarin サポートライブラリ v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)には、というヘルパークラスが用意されて [`LocalBroadcastManager`](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html) います。 は、 `LocalBroadcastManager` デバイス上の他のアプリからのブロードキャストを送信または受信したくないアプリを対象としています。 は、 `LocalBroadcastManager` アプリケーションのコンテキスト内でのみメッセージを公開し、に登録されているブロードキャストレシーバーにのみメッセージを公開し `LocalBroadcastManager` ます。 次のコードスニペットは、ブロードキャストレシーバーをに登録する例です `LocalBroadcastManager` 。

```csharp
Android.Support.V4.Content.LocalBroadcastManager.GetInstance(this). RegisterReceiver(receiver, new IntentFilter("com.xamarin.example.TEST"));
```

デバイス上の他のアプリは、で公開されたメッセージを受信できません `LocalBroadcastManager` 。 次のコードスニペットは、を使用してインテントをディスパッチする方法を示してい `LocalBroadcastManager` ます。

```csharp
Intent message = new Intent("com.xamarin.example.TEST");
// If desired, pass some values to the broadcast receiver.
message.PutExtra("key", "value");
Android.Support.V4.Content.LocalBroadcastManager.GetInstance(this).SendBroadcast(message);
```

## <a name="related-links"></a>関連リンク

- [BroadcastReceiver API](xref:Android.Content.BroadcastReceiver)
- [コンテキスト. RegisterReceiver API](xref:Android.Content.Context.RegisterReceiver*)
- [コンテキスト。 SendBroadcast API](xref:Android.Content.Context.SendBroadcast*)
- [コンテキスト. UnregisterReceiver API](xref:Android.Content.Context.UnregisterReceiver*)
- [インテント API](xref:Android.Content.Intent)
- [IntentFilter API](xref:Android.App.IntentFilterAttribute)
- [LocalBroadcastManager (Android ドキュメント)](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html#sendBroadcast(android.content.Intent))
- [Android 用のローカル通知](~/android/app-fundamentals/notifications/local-notifications.md) ガイド
- [Android サポートライブラリ v4 (NuGet)](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)
