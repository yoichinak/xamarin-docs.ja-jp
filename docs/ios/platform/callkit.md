---
title: Xamarin の CallKit
description: この記事では、Apple が iOS 10 でリリースした新しい CallKit API と、Xamarin iOS VOIP アプリでの実装方法について説明します。
ms.prod: xamarin
ms.assetid: 738A142D-FFD2-4738-B3ED-57C273179848
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/15/2017
ms.openlocfilehash: 791ab82e0e5f47929eff561ac836ec87e6d6c134
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/23/2020
ms.locfileid: "86997320"
---
# <a name="callkit-in-xamarinios"></a>Xamarin の CallKit

IOS 10 の新しい CallKit API は、VOIP アプリを iPhone UI と統合し、使い慣れたインターフェイスとエクスペリエンスをエンドユーザーに提供するための手段を提供します。 この API を使用すると、ユーザーは iOS デバイスのロック画面から VOIP 通話を表示して操作したり、電話アプリの [**お気に入り**] ビューと [**受信者**] ビューを使用して連絡先を管理したりできます。

## <a name="about-callkit"></a>CallKit について

Apple によれば、CallKit は、サードパーティのボイスオーバー IP (VOIP) アプリを iOS 10 のファーストパーティエクスペリエンスに昇格させる新しいフレームワークです。 CallKit API を使用すると、VOIP アプリを iPhone UI と統合し、使い慣れたインターフェイスとエクスペリエンスをエンドユーザーに提供できます。 組み込みの Phone アプリと同様に、ユーザーは iOS デバイスのロック画面から VOIP 通話を表示および操作したり、電話アプリの [**お気に入り**] ビューと [**受信者**] ビューを使用して連絡先を管理したりすることができます。

また、CallKit API では、電話番号を名前 (発信者 ID) に関連付けたり、番号をブロックする必要がある場合にシステムに通知したりできるアプリ拡張機能を作成することができます (呼び出しブロック)。

### <a name="the-existing-voip-app-experience"></a>既存の VOIP アプリのエクスペリエンス

新しい CallKit API とその機能について説明する前に、「MonkeyCall」という架空の VOIP アプリを使用して、iOS 9 でサードパーティ製の VOIP アプリを使用した現在のユーザーエクスペリエンスを確認してください。 MonkeyCall は、ユーザーが既存の iOS Api を使用して VOIP 通話を送受信できるようにする単純なアプリです。

現在、ユーザーが MonkeyCall で着信呼び出しを受信していて、iPhone がロックされている場合、ロック画面で受信した通知は、他の種類の通知 (メッセージやメールアプリなど) と区別できません。

ユーザーが呼び出しに応答する場合は、MonkeyCall 通知をスライドしてアプリを開き、電話のパスコード (またはユーザータッチ ID) を入力して、通話を受け入れてメッセージ交換を開始する前に電話のロックを解除する必要があります。

電話のロックが解除されても、エクスペリエンスは煩雑になります。 ここでも、入力した MonkeyCall 呼び出しは、画面の上部からスライドする標準通知バナーとして表示されます。 通知は一時的なものであるため、ユーザーが通知センターを開いて、応答する特定の通知を見つけてから、手動で MonkeyCall アプリを呼び出したり、起動したりする必要があります。

### <a name="the-callkit-voip-app-experience"></a>CallKit VOIP アプリのエクスペリエンス

MonkeyCall アプリで新しい CallKit Api を実装することにより、着信 VOIP 通話でのユーザーエクスペリエンスが、iOS 10 で大幅に改善される可能性があります。 電話が上からロックされているときに、VOIP 通話を受信するユーザーの例を見てください。 CallKit を実装することによって、呼び出しは iPhone のロック画面に表示されます。これは、組み込みの Phone アプリから電話を受け取った場合と同様に、全画面表示、ネイティブ UI、および標準的なスワイプ操作機能を使用して行われます。

ここでも、MonkeyCall VOIP 呼び出しを受信したときに iPhone のロックが解除された場合は、組み込みの Phone アプリの全画面表示、ネイティブ UI、および標準的なスワイプと応答の両方の機能が表示され、MonkeyCall にはカスタムの着信音を再生するオプションがあります。

CallKit は、MonkeyCall に追加機能を提供し、その VOIP 呼び出しを他の種類の呼び出しと対話し、組み込みのメーリングリストとお気に入りの一覧に表示できるようにします。また、組み込みの "応答なし" および "ブロック" 機能を使用して、ユーザーが MonkeyCall 呼び出しを Contacts アプリのユーザーに割り当てることができるようにし

次のセクションでは、CallKit アーキテクチャ、受信および送信呼び出しフロー、および CallKit API について詳しく説明します。

## <a name="the-callkit-architecture"></a>CallKit のアーキテクチャ

IOS 10 では、すべてのシステムサービスで、たとえば、CarPlay で行われた呼び出しが CallKit を介してシステム UI で認識されるように、Apple が CallKit を採用していました。 次の例では、MonkeyCall は CallKit を採用しているため、システムに対してこれらの組み込みシステムサービスと同じように認識され、同じ機能をすべて取得します。

[![CallKit サービススタック](callkit-images/callkit01.png)](callkit-images/callkit01.png#lightbox)

上の図の MonkeyCall アプリを詳しく見てみましょう。 アプリには、独自のネットワークと通信するためのすべてのコードが含まれており、独自のユーザーインターフェイスが含まれています。 システムと通信するための CallKit のリンクは次のとおりです。

[![MonkeyCall アプリのアーキテクチャ](callkit-images/callkit02.png)](callkit-images/callkit02.png#lightbox)

アプリが使用する CallKit には、次の2つの主要なインターフェイスがあります。

- `CXProvider`-MonkeyCall アプリが、発生する可能性のある帯域外通知をシステムに通知することができます。
- `CXCallController`-MonkeyCall アプリがローカルユーザーの操作をシステムに通知できるようにします。

### <a name="the-cxprovider"></a>CXProvider

前述のように、は、 `CXProvider` 発生する可能性がある帯域外通知をアプリケーションがシステムに通知することを許可します。 これらの通知は、ローカルユーザーの操作によって発生することはありませんが、着信呼び出しなどの外部イベントによって発生します。

アプリでは、次のためにを使用する必要があり `CXProvider` ます。

- システムへの着信呼び出しを報告します。
- 送信呼び出しがシステムに接続されたことを報告します。
- システムの呼び出しを終了するリモートユーザーを報告します。

アプリケーションがシステムと通信する場合は、クラスを使用 `CXCallUpdate` し、システムがアプリと通信する必要があるときに、クラスを使用し `CXAction` ます。

[![CXProvider を使用したシステムとの通信](callkit-images/callkit03.png)](callkit-images/callkit03.png#lightbox)

### <a name="the-cxcallcontroller"></a>CXCallController

を `CXCallController` 使用すると、アプリケーションは、ユーザーが VOIP 通話を開始するなど、ローカルユーザーの操作をシステムに通知することができます。 を実装することによって、 `CXCallController` アプリケーションはシステム内の他の種類の呼び出しと対話することができます。 たとえば、アクティブなテレフォニー通話が既に進行中の場合、では、 `CXCallController` voip アプリがその呼び出しを保留中に配置し、voip 通話を開始または回答できるようにすることができます。

アプリでは、次のためにを使用する必要があり `CXCallController` ます。

- ユーザーがシステムへの発信呼び出しを開始したことを報告します。
- ユーザーがシステムへの着信呼び出しに応答したときに報告します。
- ユーザーがシステムの呼び出しを終了したときに報告します。

アプリケーションがローカルユーザーの操作をシステムに通知する場合、次のようにクラスを使用し `CXTransaction` ます。

[![CXCallController を使用したシステムへの報告](callkit-images/callkit04.png)](callkit-images/callkit04.png#lightbox)

## <a name="implementing-callkit"></a>CallKit の実装

次のセクションでは、Xamarin. iOS VOIP アプリで CallKit を実装する方法について説明します。 例として、このドキュメントでは、架空の MonkeyCall VOIP アプリのコードを使用します。 ここで紹介するコードは、いくつかのサポートクラスを示しています。 CallKit 固有の部分については、以降のセクションで詳しく説明します。

### <a name="the-activecall-class"></a>ActiveCall クラス

クラスは、 `ActiveCall` 次のように現在アクティブな VOIP 通話に関するすべての情報を保持するために、MonkeyCall アプリによって使用されます。

```csharp
using System;
using CoreFoundation;
using Foundation;

namespace MonkeyCall
{
    public class ActiveCall
    {
        #region Private Variables
        private bool isConnecting;
        private bool isConnected;
        private bool isOnhold;
        #endregion

        #region Computed Properties
        public NSUuid UUID { get; set; }
        public bool isOutgoing { get; set; }
        public string Handle { get; set; }
        public DateTime StartedConnectingOn { get; set;}
        public DateTime ConnectedOn { get; set;}
        public DateTime EndedOn { get; set; }

        public bool IsConnecting {
            get { return isConnecting; }
            set {
                isConnecting = value;
                if (isConnecting) StartedConnectingOn = DateTime.Now;
                RaiseStartingConnectionChanged ();
            }
        }

        public bool IsConnected {
            get { return isConnected; }
            set {
                isConnected = value;
                if (isConnected) {
                    ConnectedOn = DateTime.Now;
                } else {
                    EndedOn = DateTime.Now;
                }
                RaiseConnectedChanged ();
            }
        }

        public bool IsOnHold {
            get { return isOnhold; }
            set {
                isOnhold = value;
            }
        }
        #endregion

        #region Constructors
        public ActiveCall ()
        {
        }

        public ActiveCall (NSUuid uuid, string handle, bool outgoing)
        {
            // Initialize
            this.UUID = uuid;
            this.Handle = handle;
            this.isOutgoing = outgoing;
        }
        #endregion

        #region Public Methods
        public void StartCall (ActiveCallbackDelegate completionHandler)
        {
            // Simulate the call starting successfully
            completionHandler (true);

            // Simulate making a starting and completing a connection
            DispatchQueue.MainQueue.DispatchAfter (new DispatchTime(DispatchTime.Now, 3000), () => {
                // Note that the call is starting
                IsConnecting = true;

                // Simulate pause before connecting
                DispatchQueue.MainQueue.DispatchAfter (new DispatchTime (DispatchTime.Now, 1500), () => {
                    // Note that the call has connected
                    IsConnecting = false;
                    IsConnected = true;
                });
            });
        }

        public void AnswerCall (ActiveCallbackDelegate completionHandler)
        {
            // Simulate the call being answered
            IsConnected = true;
            completionHandler (true);
        }

        public void EndCall (ActiveCallbackDelegate completionHandler)
        {
            // Simulate the call ending
            IsConnected = false;
            completionHandler (true);
        }
        #endregion

        #region Events
        public delegate void ActiveCallbackDelegate (bool successful);
        public delegate void ActiveCallStateChangedDelegate (ActiveCall call);

        public event ActiveCallStateChangedDelegate StartingConnectionChanged;
        internal void RaiseStartingConnectionChanged ()
        {
            if (this.StartingConnectionChanged != null) this.StartingConnectionChanged (this);
        }

        public event ActiveCallStateChangedDelegate ConnectedChanged;
        internal void RaiseConnectedChanged ()
        {
            if (this.ConnectedChanged != null) this.ConnectedChanged (this);
        }
        #endregion
    }
}
```

`ActiveCall`呼び出しの状態を定義するいくつかのプロパティと、呼び出しの状態が変化したときに発生する可能性がある2つのイベントを保持します。 これは単なる例であるため、呼び出しの開始、応答、終了をシミュレートするために3つのメソッドが使用されています。

### <a name="the-startcallrequest-class"></a>StartCallRequest クラス

静的クラスには、 `StartCallRequest` 発信呼び出しを操作するときに使用されるいくつかのヘルパーメソッドが用意されています。

```csharp
using System;
using Foundation;
using Intents;

namespace MonkeyCall
{
    public static class StartCallRequest
    {
        public static string URLScheme {
            get { return "monkeycall"; }
        }

        public static string ActivityType {
            get { return INIntentIdentifier.StartAudioCall.GetConstant ().ToString (); }
        }

        public static string CallHandleFromURL (NSUrl url)
        {
            // Is this a MonkeyCall handle?
            if (url.Scheme == URLScheme) {
                // Yes, return host
                return url.Host;
            } else {
                // Not handled
                return null;
            }
        }

        public static string CallHandleFromActivity (NSUserActivity activity)
        {
            // Is this a start call activity?
            if (activity.ActivityType == ActivityType) {
                // Yes, trap any errors
                try {
                    // Get first contact
                    var interaction = activity.GetInteraction ();
                    var startAudioCallIntent = interaction.Intent as INStartAudioCallIntent;
                    var contact = startAudioCallIntent.Contacts [0];

                    // Get the person handle
                    return contact.PersonHandle.Value;
                } catch {
                    // Error, report null
                    return null;
                }
            } else {
                // Not handled
                return null;
            }
        }
    }
}
```

`CallHandleFromURL`クラスと `CallHandleFromActivity` クラスは、発信呼び出しで呼び出されている人物の連絡先ハンドルを取得するために appdelegate で使用されます。 詳細については、以下の「[発信通話の処理](#handling-outgoing-calls)」セクションを参照してください。

### <a name="the-activecallmanager-class"></a>ActiveCallManager クラス

クラスは、 `ActiveCallManager` MonkeyCall アプリ内のすべての開いている呼び出しを処理します。

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using CallKit;

namespace MonkeyCall
{
    public class ActiveCallManager
    {
        #region Private Variables
        private CXCallController CallController = new CXCallController ();
        #endregion

        #region Computed Properties
        public List<ActiveCall> Calls { get; set; }
        #endregion

        #region Constructors
        public ActiveCallManager ()
        {
            // Initialize
            this.Calls = new List<ActiveCall> ();
        }
        #endregion

        #region Private Methods
        private void SendTransactionRequest (CXTransaction transaction)
        {
            // Send request to call controller
            CallController.RequestTransaction (transaction, (error) => {
                // Was there an error?
                if (error == null) {
                    // No, report success
                    Console.WriteLine ("Transaction request sent successfully.");
                } else {
                    // Yes, report error
                    Console.WriteLine ("Error requesting transaction: {0}", error);
                }
            });
        }
        #endregion

        #region Public Methods
        public ActiveCall FindCall (NSUuid uuid)
        {
            // Scan for requested call
            foreach (ActiveCall call in Calls) {
                if (call.UUID.Equals(uuid)) return call;
            }

            // Not found
            return null;
        }

        public void StartCall (string contact)
        {
            // Build call action
            var handle = new CXHandle (CXHandleType.Generic, contact);
            var startCallAction = new CXStartCallAction (new NSUuid (), handle);

            // Create transaction
            var transaction = new CXTransaction (startCallAction);

            // Inform system of call request
            SendTransactionRequest (transaction);
        }

        public void EndCall (ActiveCall call)
        {
            // Build action
            var endCallAction = new CXEndCallAction (call.UUID);

            // Create transaction
            var transaction = new CXTransaction (endCallAction);

            // Inform system of call request
            SendTransactionRequest (transaction);
        }

        public void PlaceCallOnHold (ActiveCall call)
        {
            // Build action
            var holdCallAction = new CXSetHeldCallAction (call.UUID, true);

            // Create transaction
            var transaction = new CXTransaction (holdCallAction);

            // Inform system of call request
            SendTransactionRequest (transaction);
        }

        public void RemoveCallFromOnHold (ActiveCall call)
        {
            // Build action
            var holdCallAction = new CXSetHeldCallAction (call.UUID, false);

            // Create transaction
            var transaction = new CXTransaction (holdCallAction);

            // Inform system of call request
            SendTransactionRequest (transaction);
        }
        #endregion
    }
}
```

ここでも、はシミュレーションのみであるため、は `ActiveCallManager` オブジェクトのコレクションのみを保持し、その `ActiveCall` プロパティによって特定の呼び出しを検索するためのルーチンを備えてい `UUID` ます。 また、発信呼び出しの保留状態を開始、終了、変更するメソッドも含まれています。 詳細については、以下の「[発信通話の処理](#handling-outgoing-calls)」セクションを参照してください。

### <a name="the-providerdelegate-class"></a>ProviderDelegate クラス

前に説明したように、は、 `CXProvider` アプリケーションと帯域外通知用システム間の双方向の通信を提供します。 開発者は、カスタムを指定 `CXProviderDelegate` し、それをにアタッチして、 `CXProvider` アウトオブバンド callkit イベントを処理する必要があります。 MonkeyCall は次のものを使用し `CXProviderDelegate` ます。

```csharp
using System;
using Foundation;
using CallKit;
using UIKit;

namespace MonkeyCall
{
    public class ProviderDelegate : CXProviderDelegate
    {
        #region Computed Properties
        public ActiveCallManager CallManager { get; set;}
        public CXProviderConfiguration Configuration { get; set; }
        public CXProvider Provider { get; set; }
        #endregion

        #region Constructors
        public ProviderDelegate (ActiveCallManager callManager)
        {
            // Save connection to call manager
            CallManager = callManager;

            // Define handle types
            var handleTypes = new [] { (NSNumber)(int)CXHandleType.PhoneNumber };

            // Get Image Template
            var templateImage = UIImage.FromFile ("telephone_receiver.png");

            // Setup the initial configurations
            Configuration = new CXProviderConfiguration ("MonkeyCall") {
                MaximumCallsPerCallGroup = 1,
                SupportedHandleTypes = new NSSet<NSNumber> (handleTypes),
                IconTemplateImageData = templateImage.AsPNG(),
                RingtoneSound = "musicloop01.wav"
            };

            // Create a new provider
            Provider = new CXProvider (Configuration);

            // Attach this delegate
            Provider.SetDelegate (this, null);

        }
        #endregion

        #region Override Methods
        public override void DidReset (CXProvider provider)
        {
            // Remove all calls
            CallManager.Calls.Clear ();
        }

        public override void PerformStartCallAction (CXProvider provider, CXStartCallAction action)
        {
            // Create new call record
            var activeCall = new ActiveCall (action.CallUuid, action.CallHandle.Value, true);

            // Monitor state changes
            activeCall.StartingConnectionChanged += (call) => {
                if (call.isConnecting) {
                    // Inform system that the call is starting
                    Provider.ReportConnectingOutgoingCall (call.UUID, call.StartedConnectingOn.ToNSDate());
                }
            };

            activeCall.ConnectedChanged += (call) => {
                if (call.isConnected) {
                    // Inform system that the call has connected
                    provider.ReportConnectedOutgoingCall (call.UUID, call.ConnectedOn.ToNSDate ());
                }
            };

            // Start call
            activeCall.StartCall ((successful) => {
                // Was the call able to be started?
                if (successful) {
                    // Yes, inform the system
                    action.Fulfill ();

                    // Add call to manager
                    CallManager.Calls.Add (activeCall);
                } else {
                    // No, inform system
                    action.Fail ();
                }
            });
        }

        public override void PerformAnswerCallAction (CXProvider provider, CXAnswerCallAction action)
        {
            // Find requested call
            var call = CallManager.FindCall (action.CallUuid);

            // Found?
            if (call == null) {
                // No, inform system and exit
                action.Fail ();
                return;
            }

            // Attempt to answer call
            call.AnswerCall ((successful) => {
                // Was the call successfully answered?
                if (successful) {
                    // Yes, inform system
                    action.Fulfill ();
                } else {
                    // No, inform system
                    action.Fail ();
                }
            });
        }

        public override void PerformEndCallAction (CXProvider provider, CXEndCallAction action)
        {
            // Find requested call
            var call = CallManager.FindCall (action.CallUuid);

            // Found?
            if (call == null) {
                // No, inform system and exit
                action.Fail ();
                return;
            }

            // Attempt to answer call
            call.EndCall ((successful) => {
                // Was the call successfully answered?
                if (successful) {
                    // Remove call from manager's queue
                    CallManager.Calls.Remove (call);

                    // Yes, inform system
                    action.Fulfill ();
                } else {
                    // No, inform system
                    action.Fail ();
                }
            });
        }

        public override void PerformSetHeldCallAction (CXProvider provider, CXSetHeldCallAction action)
        {
            // Find requested call
            var call = CallManager.FindCall (action.CallUuid);

            // Found?
            if (call == null) {
                // No, inform system and exit
                action.Fail ();
                return;
            }

            // Update hold status
            call.isOnHold = action.OnHold;

            // Inform system of success
            action.Fulfill ();
        }

        public override void TimedOutPerformingAction (CXProvider provider, CXAction action)
        {
            // Inform user that the action has timed out
        }

        public override void DidActivateAudioSession (CXProvider provider, AVFoundation.AVAudioSession audioSession)
        {
            // Start the calls audio session here
        }

        public override void DidDeactivateAudioSession (CXProvider provider, AVFoundation.AVAudioSession audioSession)
        {
            // End the calls audio session and restart any non-call
            // related audio
        }
        #endregion

        #region Public Methods
        public void ReportIncomingCall (NSUuid uuid, string handle)
        {
            // Create update to describe the incoming call and caller
            var update = new CXCallUpdate ();
            update.RemoteHandle = new CXHandle (CXHandleType.Generic, handle);

            // Report incoming call to system
            Provider.ReportNewIncomingCall (uuid, update, (error) => {
                // Was the call accepted
                if (error == null) {
                    // Yes, report to call manager
                    CallManager.Calls.Add (new ActiveCall (uuid, handle, false));
                } else {
                    // Report error to user here
                    Console.WriteLine ("Error: {0}", error);
                }
            });
        }
        #endregion
    }
}
```

このデリゲートのインスタンスが作成されると、 `ActiveCallManager` 呼び出しアクティビティを処理するために使用するが渡されます。 次に、が応答するハンドル型 () を定義し `CXHandleType` `CXProvider` ます。

```csharp
// Define handle types
var handleTypes = new [] { (NSNumber)(int)CXHandleType.PhoneNumber };
```

また、呼び出しの進行中にアプリのアイコンに適用されるテンプレートイメージを取得します。

```csharp
// Get Image Template
var templateImage = UIImage.FromFile ("telephone_receiver.png");
```

これらの値は、を `CXProviderConfiguration` 構成するために使用されるにバンドルされ `CXProvider` ます。

```csharp
// Setup the initial configurations
Configuration = new CXProviderConfiguration ("MonkeyCall") {
    MaximumCallsPerCallGroup = 1,
    SupportedHandleTypes = new NSSet<NSNumber> (handleTypes),
    IconTemplateImageData = templateImage.AsPNG(),
    RingtoneSound = "musicloop01.wav"
};
```

次に、デリゲートは、これらの構成を使用して新しいを作成 `CXProvider` し、それ自体をアタッチします。

```csharp
// Create a new provider
Provider = new CXProvider (Configuration);

// Attach this delegate
Provider.SetDelegate (this, null);
```

CallKit を使用すると、アプリは独自のオーディオセッションを作成して処理しなくなります。代わりに、システムによって作成および処理されるオーディオセッションを構成して使用する必要があります。

実際のアプリの場合、メソッドを使用して、システムによって `DidActivateAudioSession` 提供される構成済みのを使用して呼び出しを開始し `AVAudioSession` ます。

```csharp
public override void DidActivateAudioSession (CXProvider provider, AVFoundation.AVAudioSession audioSession)
{
    // Start the call's audio session here...
}
```

また、メソッドを使用して、 `DidDeactivateAudioSession` システムが提供するオーディオセッションへの接続を最終処理し、解放します。

```csharp
public override void DidDeactivateAudioSession (CXProvider provider, AVFoundation.AVAudioSession audioSession)
{
    // End the calls audio session and restart any non-call
    // releated audio
}
```

残りのコードについては、後のセクションで詳しく説明します。

### <a name="the-appdelegate-class"></a>AppDelegate クラス

MonkeyCall は、AppDelegate を使用してのインスタンスを保持し、 `ActiveCallManager` `CXProviderDelegate` アプリ全体で使用されます。

```csharp
using Foundation;
using UIKit;
using Intents;
using System;

namespace MonkeyCall
{
    [Register ("AppDelegate")]
    public class AppDelegate : UIApplicationDelegate
    {
        #region Constructors
        public override UIWindow Window { get; set; }
        public ActiveCallManager CallManager { get; set; }
        public ProviderDelegate CallProviderDelegate { get; set; }
        #endregion

        #region Override Methods
        public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
        {
            // Initialize the call handlers
            CallManager = new ActiveCallManager ();
            CallProviderDelegate = new ProviderDelegate (CallManager);

            return true;
        }

        public override bool OpenUrl (UIApplication app, NSUrl url, NSDictionary options)
        {
            // Get handle from url
            var handle = StartCallRequest.CallHandleFromURL (url);

            // Found?
            if (handle == null) {
                // No, report to system
                Console.WriteLine ("Unable to get call handle from URL: {0}", url);
                return false;
            } else {
                // Yes, start call and inform system
                CallManager.StartCall (handle);
                return true;
            }
        }

        public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
        {
            var handle = StartCallRequest.CallHandleFromActivity (userActivity);

            // Found?
            if (handle == null) {
                // No, report to system
                Console.WriteLine ("Unable to get call handle from User Activity: {0}", userActivity);
                return false;
            } else {
                // Yes, start call and inform system
                CallManager.StartCall (handle);
                return true;
            }
        }

        ...
        #endregion
    }
}
```

`OpenUrl`および `ContinueUserActivity` オーバーライドメソッドは、アプリが発信呼び出しを処理するときに使用されます。 詳細については、以下の「[発信通話の処理](#handling-outgoing-calls)」セクションを参照してください。

## <a name="handling-incoming-calls"></a>着信呼び出しの処理

着信 VOIP 呼び出しでは、次のような一般的な着信呼び出しワークフローを通過することができます。

- 着信呼び出しが存在することをユーザー (およびシステム) に通知します。
- ユーザーが呼び出しに応答し、他のユーザーとの呼び出しを初期化する必要があるときに通知を受信します。
- ユーザーが現在の呼び出しを終了するときに、システムと通信ネットワークに通知します。

次のセクションでは、例として MonkeyCall VOIP アプリを使用して、アプリが CallKit を使用して着信呼び出しワークフローを処理する方法について詳しく見ていきます。

### <a name="informing-user-of-incoming-call"></a>着信呼び出しをユーザーに通知する

リモートユーザーがローカルユーザーとの VOIP メッセージ交換を開始すると、次の処理が行われます。

[![リモートユーザーが VOIP メッセージ交換を開始しました](callkit-images/callkit05.png)](callkit-images/callkit05.png#lightbox)

1. アプリは、着信 VOIP 通話があることを示す通知を通信ネットワークから取得します。
2. アプリはを使用して、 `CXProvider` `CXCallUpdate` 呼び出しの通知をシステムに送信します。
3. システム UI、システムサービス、およびその他の VOIP アプリへの呼び出しを、CallKit を使用して発行します。

たとえば、 `CXProviderDelegate` 次のようになります。

```csharp
public void ReportIncomingCall (NSUuid uuid, string handle)
{
    // Create update to describe the incoming call and caller
    var update = new CXCallUpdate ();
    update.RemoteHandle = new CXHandle (CXHandleType.Generic, handle);

    // Report incoming call to system
    Provider.ReportNewIncomingCall (uuid, update, (error) => {
        // Was the call accepted
        if (error == null) {
            // Yes, report to call manager
            CallManager.Calls.Add (new ActiveCall (uuid, handle, false));
        } else {
            // Report error to user here
            Console.WriteLine ("Error: {0}", error);
        }
    });
}
```

このコードは、新しい `CXCallUpdate` インスタンスを作成し、呼び出し元を識別するハンドルをアタッチします。 次に、クラスのメソッドを使用して、 `ReportNewIncomingCall` `CXProvider` 呼び出しのシステムを通知します。 成功した場合、呼び出しはアプリのアクティブな呼び出しのコレクションに追加されます。そうでない場合は、エラーをユーザーに報告する必要があります。

### <a name="user-answering-incoming-call"></a>ユーザーに応答する着信呼び出し

ユーザーが着信 VOIP 通話に応答する場合、次の処理が行われます。

[![ユーザーは、着信 VOIP 通話に応答します。](callkit-images/callkit06.png)](callkit-images/callkit06.png#lightbox)

1. システム UI によって、ユーザーが VOIP 通話に応答する必要があることがシステムに通知されます。
2. システムは、 `CXAnswerCallAction` 回答インテントの通知をアプリに送信し `CXProvider` ます。
3. アプリは、ユーザーが通話に応答していることと、VOIP の呼び出しが通常どおり続行されることを通信ネットワークに通知します。

たとえば、 `CXProviderDelegate` 次のようになります。

```csharp
public override void PerformAnswerCallAction (CXProvider provider, CXAnswerCallAction action)
{
    // Find requested call
    var call = CallManager.FindCall (action.CallUuid);

    // Found?
    if (call == null) {
        // No, inform system and exit
        action.Fail ();
        return;
    }

    // Attempt to answer call
    call.AnswerCall ((successful) => {
        // Was the call successfully answered?
        if (successful) {
            // Yes, inform system
            action.Fulfill ();
        } else {
            // No, inform system
            action.Fail ();
        }
    });
}
```

このコードでは、まず、アクティブな呼び出しの一覧で、指定された呼び出しを検索します。 呼び出しが見つからない場合は、システムに通知され、メソッドは終了します。 見つかった場合は、 `AnswerCall` `ActiveCall` 呼び出しを開始するためにクラスのメソッドが呼び出され、成功または失敗した場合はシステムによって情報が格納されます。

### <a name="user-ending-incoming-call"></a>ユーザーが着信呼び出しを終了します

ユーザーがアプリの UI 内から呼び出しを終了する場合、次のようになります。

[![ユーザーは、アプリの UI 内から呼び出しを終了します。](callkit-images/callkit07.png)](callkit-images/callkit07.png#lightbox)

1. アプリは `CXEndCallAction` 、 `CXTransaction` 呼び出しが終了したことを通知するためにシステムに送信されるにバンドルされるを作成します。
2. システムは、エンド呼び出しの目的を確認し、を `CXEndCallAction` 介してアプリにを送り返し `CXProvider` ます。
3. その後、アプリは、呼び出しが終了したことを通信ネットワークに通知します。

たとえば、 `CXProviderDelegate` 次のようになります。

```csharp
public override void PerformEndCallAction (CXProvider provider, CXEndCallAction action)
{
    // Find requested call
    var call = CallManager.FindCall (action.CallUuid);

    // Found?
    if (call == null) {
        // No, inform system and exit
        action.Fail ();
        return;
    }

    // Attempt to answer call
    call.EndCall ((successful) => {
        // Was the call successfully answered?
        if (successful) {
            // Remove call from manager's queue
            CallManager.Calls.Remove (call);

            // Yes, inform system
            action.Fulfill ();
        } else {
            // No, inform system
            action.Fail ();
        }
    });
}
```

このコードでは、まず、アクティブな呼び出しの一覧で、指定された呼び出しを検索します。 呼び出しが見つからない場合は、システムに通知され、メソッドは終了します。 見つかった場合は、 `EndCall` `ActiveCall` 呼び出しを終了するためにクラスのメソッドが呼び出され、成功または失敗した場合はシステムが情報になります。 成功した場合は、アクティブな呼び出しのコレクションから呼び出しが削除されます。

## <a name="managing-multiple-calls"></a>複数の呼び出しの管理

ほとんどの VOIP アプリは、一度に複数の呼び出しを処理できます。 たとえば、現在アクティブな VOIP 通話が存在し、アプリが新しい着信呼び出しがあるという通知を受け取った場合、ユーザーは最初の呼び出しを一時停止または切断して、2つ目の呼び出しに応答できます。

上記の状況では、 `CXTransaction` 複数のアクション (やなど) のリストを含むがアプリに送信され `CXEndCallAction` `CXAnswerCallAction` ます。 これらの操作はすべて、システムが UI を適切に更新できるように、個別に実行する必要があります。

## <a name="handling-outgoing-calls"></a>送信呼び出しの処理

たとえば、ユーザーがアプリに属している通話の一覧 (Phone アプリの場合) からエントリをタップすると、システムによって_開始呼び出しの目的_が送信されます。

[![開始呼び出しの目的を受信する](callkit-images/callkit08.png)](callkit-images/callkit08.png#lightbox)

1. アプリは、システムから受信した開始呼び出しのインテントに基づいて、 _Start 呼び出しアクション_を作成します。
2. アプリはを使用して、 `CXCallController` システムから開始呼び出しアクションを要求します。
3. システムがアクションを受け入れると、デリゲートを介してアプリに返され `XCProvider` ます。
4. アプリは、通信ネットワークを使用して発信呼び出しを開始します。

インテントの詳細については、[インテントとインテントの UI 拡張機能](~/ios/platform/sirikit/understanding-sirikit.md)に関するドキュメントを参照してください。

### <a name="the-outgoing-call-lifecycle"></a>発信呼び出しのライフサイクル

CallKit と発信呼び出しを使用する場合、アプリは次のライフサイクルイベントをシステムに通知する必要があります。

1. **開始**-発信呼び出しが開始されようとしていることをシステムに通知します。
2. [**開始**済み-発信呼び出しが開始されたことをシステムに通知します。
3. **接続**-発信呼び出しが接続していることをシステムに通知します。
4. **Connected** -発信呼び出しが接続されたこと、および両方のパーティが通信できることを通知します。

たとえば、次のコードでは発信呼び出しが開始されます。

```csharp
private CXCallController CallController = new CXCallController ();
...

private void SendTransactionRequest (CXTransaction transaction)
{
    // Send request to call controller
    CallController.RequestTransaction (transaction, (error) => {
        // Was there an error?
        if (error == null) {
            // No, report success
            Console.WriteLine ("Transaction request sent successfully.");
        } else {
            // Yes, report error
            Console.WriteLine ("Error requesting transaction: {0}", error);
        }
    });
}

public void StartCall (string contact)
{
    // Build call action
    var handle = new CXHandle (CXHandleType.Generic, contact);
    var startCallAction = new CXStartCallAction (new NSUuid (), handle);

    // Create transaction
    var transaction = new CXTransaction (startCallAction);

    // Inform system of call request
    SendTransactionRequest (transaction);
}
```

を作成し、それを使用して、 `CXHandle` `CXStartCallAction` `CXTransaction` クラスのメソッドを使用してシステムに送信されるにバンドルされているを構成し `RequestTransaction` `CXCallController` ます。 メソッドを呼び出すことにより、 `RequestTransaction` 新しい呼び出しが開始される前に、ソース (Phone アプリ、FaceTime、VOIP など) に関係なく、既存の呼び出しを保留にすることができます。

発信 VOIP 通話を開始する要求は、Siri、連絡先カード (連絡先アプリの場合)、または (Phone アプリ内の) リストからのエントリなど、いくつかの異なるソースから取得できます。 このような状況では、アプリはの内部で開始呼び出しのインテントを送信 `NSUserActivity` し、AppDelegate はそれを処理する必要があります。

```csharp
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{
    var handle = StartCallRequest.CallHandleFromActivity (userActivity);

    // Found?
    if (handle == null) {
        // No, report to system
        Console.WriteLine ("Unable to get call handle from User Activity: {0}", userActivity);
        return false;
    } else {
        // Yes, start call and inform system
        CallManager.StartCall (handle);
        return true;
    }
}
```

ここで `CallHandleFromActivity` は、ヘルパークラスのメソッドを使用して、 `StartCallRequest` 呼び出されているユーザーへのハンドルを取得しています (上記[の Startcallrequest クラス](#the-startcallrequest-class)を参照してください)。

`PerformStartCallAction` [Providerdelegate クラス](#the-providerdelegate-class)のメソッドは、最終的に実際の発信呼び出しを開始し、そのライフサイクルをシステムに通知するために使用されます。

```csharp
public override void PerformStartCallAction (CXProvider provider, CXStartCallAction action)
{
    // Create new call record
    var activeCall = new ActiveCall (action.CallUuid, action.CallHandle.Value, true);

    // Monitor state changes
    activeCall.StartingConnectionChanged += (call) => {
        if (call.IsConnecting) {
            // Inform system that the call is starting
            Provider.ReportConnectingOutgoingCall (call.UUID, call.StartedConnectingOn.ToNSDate());
        }
    };

    activeCall.ConnectedChanged += (call) => {
        if (call.IsConnected) {
            // Inform system that the call has connected
            Provider.ReportConnectedOutgoingCall (call.UUID, call.ConnectedOn.ToNSDate ());
        }
    };

    // Start call
    activeCall.StartCall ((successful) => {
        // Was the call able to be started?
        if (successful) {
            // Yes, inform the system
            action.Fulfill ();

            // Add call to manager
            CallManager.Calls.Add (activeCall);
        } else {
            // No, inform system
            action.Fail ();
        }
    });
}
```

このクラスは、 `ActiveCall` (進行中の呼び出しに関する情報を保持するために) クラスのインスタンスを作成し、呼び出されたユーザーを設定します。 `StartingConnectionChanged`イベントと `ConnectedChanged` イベントは、発信呼び出しのライフサイクルを監視して報告するために使用されます。 呼び出しが開始され、アクションが満たされたことがシステムに通知されます。

### <a name="ending-an-outgoing-call"></a>発信呼び出しの終了

ユーザーが発信呼び出しを終了し、終了する場合は、次のコードを使用できます。

```csharp
private CXCallController CallController = new CXCallController ();
...

private void SendTransactionRequest (CXTransaction transaction)
{
    // Send request to call controller
    CallController.RequestTransaction (transaction, (error) => {
        // Was there an error?
        if (error == null) {
            // No, report success
            Console.WriteLine ("Transaction request sent successfully.");
        } else {
            // Yes, report error
            Console.WriteLine ("Error requesting transaction: {0}", error);
        }
    });
}

public void EndCall (ActiveCall call)
{
    // Build action
    var endCallAction = new CXEndCallAction (call.UUID);

    // Create transaction
    var transaction = new CXTransaction (endCallAction);

    // Inform system of call request
    SendTransactionRequest (transaction);
}
```

が end の呼び出しの UUID を使用してを作成した場合、は、 `CXEndCallAction` `CXTransaction` クラスのメソッドを使用してシステムに送信されるにバンドルし `RequestTransaction` `CXCallController` ます。

## <a name="additional-callkit-details"></a>その他の CallKit の詳細

このセクションでは、開発者が次のような CallKit を使用する際に考慮する必要がある追加の詳細について説明します。

- プロバイダーの構成
- アクション エラー
- システムの制限
- VOIP オーディオ

### <a name="provider-configuration"></a>プロバイダーの構成

プロバイダーの構成により、iOS 10 の VOIP アプリは、CallKit を使用するときに (ネイティブの呼び出し元の UI 内で) ユーザーエクスペリエンスをカスタマイズできます。

アプリでは、次の種類のカスタマイズを行うことができます。

- ローカライズされた名前を表示します。
- ビデオ通話のサポートを有効にします。
- 独自のテンプレートイメージアイコンを表示して、呼び出し中の UI のボタンをカスタマイズします。 カスタムボタンを使用したユーザー操作は、処理するアプリに直接送信されます。

### <a name="action-errors"></a>アクションエラー

CallKit を使用する iOS 10 VOIP アプリは、正常に失敗したアクションを処理し、常にユーザーにアクション状態を通知する必要があります。

次の例を考慮してください。

1. アプリは Start 呼び出しアクションを受け取り、通信ネットワークを使用して新しい VOIP 通話を初期化するプロセスを開始しました。
2. ネットワーク通信機能が制限されているかまったくないため、この接続は失敗します。
3. アプリは、**失敗**の原因をシステムに通知するために、[呼び出しの開始] アクション () にエラーメッセージを送信する*必要があり* `Action.Fail()` ます。
4. これにより、システムは呼び出しの状態をユーザーに通知できます。 たとえば、呼び出しエラーの UI を表示します。

さらに、iOS 10 VOIP アプリは、所定の時間内に予想されるアクションを処理できない場合に発生する可能性がある_タイムアウトエラー_に応答する必要があります。 CallKit によって提供される各アクションの種類には、最大タイムアウト値が関連付けられています。 これらのタイムアウト値によって、ユーザーによって要求された CallKit アクションが応答的な方法で処理されるため、OS の流体と応答性も維持されます。

`CXProviderDelegate`このタイムアウト状況を適切に処理するためにオーバーライドする必要がある、プロバイダーデリゲート () にはいくつかのメソッドがあります。

### <a name="system-restrictions"></a>システムの制限

IOS 10 VOIP アプリを実行している iOS デバイスの現在の状態に基づいて、特定のシステム制限が適用される場合があります。

たとえば、次の場合、着信 VOIP 通話はシステムによって制限されます。

1. を呼び出す人は、ユーザーのブロックされた呼び出し元の一覧にあります。
2. ユーザーの iOS デバイスは、応答不可モードになっています。

これらのいずれかの状況によって VOIP 通話が制限されている場合は、次のコードを使用してそれを処理します。

```csharp
public class ProviderDelegate : CXProviderDelegate
{
...

    public void ReportIncomingCall (NSUuid uuid, string handle)
    {
        // Create update to describe the incoming call and caller
        var update = new CXCallUpdate ();
        update.RemoteHandle = new CXHandle (CXHandleType.Generic, handle);

        // Report incoming call to system
        Provider.ReportNewIncomingCall (uuid, update, (error) => {
            // Was the call accepted
            if (error == null) {
                // Yes, report to call manager
                CallManager.Calls.Add (new ActiveCall (uuid, handle, false));
            } else {
                // Report error to user here
                if (error.Code == (int)CXErrorCodeIncomingCallError.CallUuidAlreadyExists) {
                    // Handle duplicate call ID
                } else if (error.Code == (int)CXErrorCodeIncomingCallError.FilteredByBlockList) {
                    // Handle call from blocked user
                } else if (error.Code == (int)CXErrorCodeIncomingCallError.FilteredByDoNotDisturb) {
                    // Handle call while in do-not-disturb mode
                } else {
                    // Handle unknown error
                }
            }
        });
    }

}
```

### <a name="voip-audio"></a>VOIP オーディオ

CallKit は、iOS 10 VOIP アプリがライブ VOIP 通話中に必要とするオーディオリソースを処理するためのいくつかの利点を提供します。 最大の利点の1つは、iOS 10 での実行時にアプリのオーディオセッションで優先順位が引き上げられることです。 これは、組み込みの Phone および FaceTime アプリと同じ優先度レベルで、この拡張優先度レベルにより、他の実行中のアプリが VOIP アプリのオーディオセッションを中断するのを防ぐことができます。

さらに、CallKit は、ユーザー設定やデバイスの状態に基づいてライブ通話中に、パフォーマンスを向上させ、VOIP オーディオを特定の出力デバイスにインテリジェントにルーティングできる他のオーディオルーティングヒントにアクセスできます。 たとえば、bluetooth ヘッドホンなどの接続されたデバイスに基づいて、live CarPlay 接続やユーザー補助の設定を行います。

CallKit を使用した一般的な VOIP 通話のライフサイクル中に、アプリは CallKit によって提供されるオーディオストリームを構成する必要があります。 次の例を見てみましょう。

[![Start 呼び出しアクションシーケンス](callkit-images/callkit09.png)](callkit-images/callkit09.png#lightbox)

1. 開始呼び出しアクションは、着信呼び出しに応答するためにアプリによって受信されます。
2. このアクションがアプリによって実行される前に、に必要な構成が提供され `AVAudioSession` ます。
3. アプリは、アクションが実行されたことをシステムに通知します。
4. 呼び出しを接続する前に、CallKit では、 `AVAudioSession` アプリが要求した構成に一致する優先度が高くなります。 アプリは、のメソッドを使用して通知され `DidActivateAudioSession` `CXProviderDelegate` ます。

## <a name="working-with-call-directory-extensions"></a>Call directory 拡張機能の使用

CallKit を使用する場合、_呼び出しディレクトリ拡張機能_を使用して、ブロックされた呼び出し番号を追加し、特定の VOIP アプリに固有の番号を iOS デバイスの Contact アプリの連絡先に識別することができます。

### <a name="implementing-a-call-directory-extension"></a>Call directory 拡張機能の実装

Xamarin iOS アプリで Call Directory 拡張機能を実装するには、次の手順を実行します。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

1. Visual Studio for Mac でアプリのソリューションを開きます。
2. **ソリューションエクスプローラー**でソリューション名を右クリックし、[**追加**] [  >  **新しいプロジェクト**] の順に選択します。
3. [ **IOS**  >  **拡張機能**] [  >  **ディレクトリ拡張機能を呼び出す**] を選択し、[**次へ**] ボタンをクリックします。

    [![新しい Call Directory 拡張機能を作成する](callkit-images/calldir01.png)](callkit-images/calldir01.png#lightbox)
4. 拡張機能の**名前**を入力し、[**次へ**] ボタンをクリックします。

    [![拡張機能の名前を入力する](callkit-images/calldir02.png)](callkit-images/calldir02.png#lightbox)
5. 必要に応じて**プロジェクト名**または**ソリューション名**を調整し、[**作成**] ボタンをクリックします。

    [![プロジェクトの作成](callkit-images/calldir03.png)](callkit-images/calldir03.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. Visual Studio でアプリのソリューションを開きます。
2. **ソリューションエクスプローラー**でソリューション名を右クリックし、[**追加**] [  >  **新しいプロジェクト**] の順に選択します。
3. [ **IOS**  >  **拡張機能**] [  >  **ディレクトリ拡張機能を呼び出す**] を選択し、[**次へ**] ボタンをクリックします。

    [![新しい Call Directory 拡張機能を作成する](callkit-images/calldir01w.png)](callkit-images/calldir01.png#lightbox)
4. 拡張機能の**名前**を入力し、[ **OK** ] ボタンをクリックします。

-----

これにより、 `CallDirectoryHandler.cs` 次のようなクラスがプロジェクトに追加されます。

```csharp
using System;

using Foundation;
using CallKit;

namespace MonkeyCallDirExtension
{
    [Register ("CallDirectoryHandler")]
    public class CallDirectoryHandler : CXCallDirectoryProvider, ICXCallDirectoryExtensionContextDelegate
    {
        #region Constructors
        protected CallDirectoryHandler (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void BeginRequest (CXCallDirectoryExtensionContext context)
        {
            context.Delegate = this;

            if (!AddBlockingPhoneNumbers (context)) {
                Console.WriteLine ("Unable to add blocking phone numbers");
                var error = new NSError (new NSString ("CallDirectoryHandler"), 1, null);
                context.CancelRequest (error);
                return;
            }

            if (!AddIdentificationPhoneNumbers (context)) {
                Console.WriteLine ("Unable to add identification phone numbers");
                var error = new NSError (new NSString ("CallDirectoryHandler"), 2, null);
                context.CancelRequest (error);
                return;
            }

            context.CompleteRequest (null);
        }
        #endregion

        #region Private Methods
        private bool AddBlockingPhoneNumbers (CXCallDirectoryExtensionContext context)
        {
            // Retrieve phone numbers to block from data store. For optimal performance and memory usage when there are many phone numbers,
            // consider only loading a subset of numbers at a given time and using autorelease pool(s) to release objects allocated during each batch of numbers which are loaded.
            //
            // Numbers must be provided in numerically ascending order.

            long [] phoneNumbers = { 14085555555, 18005555555 };

            foreach (var phoneNumber in phoneNumbers)
                context.AddBlockingEntry (phoneNumber);

            return true;
        }

        private bool AddIdentificationPhoneNumbers (CXCallDirectoryExtensionContext context)
        {
            // Retrieve phone numbers to identify and their identification labels from data store. For optimal performance and memory usage when there are many phone numbers,
            // consider only loading a subset of numbers at a given time and using autorelease pool(s) to release objects allocated during each batch of numbers which are loaded.
            //
            // Numbers must be provided in numerically ascending order.

            long [] phoneNumbers = { 18775555555, 18885555555 };
            string [] labels = { "Telemarketer", "Local business" };

            for (var i = 0; i < phoneNumbers.Length; i++) {
                long phoneNumber = phoneNumbers [i];
                string label = labels [i];
                context.AddIdentificationEntry (phoneNumber, label);
            }

            return true;
        }
        #endregion

        #region Public Methods
        public void RequestFailed (CXCallDirectoryExtensionContext extensionContext, NSError error)
        {
            // An error occurred while adding blocking or identification entries, check the NSError for details.
            // For Call Directory error codes, see the CXErrorCodeCallDirectoryManagerError enum.
            //
            // This may be used to store the error details in a location accessible by the extension's containing app, so that the
            // app may be notified about errors which occurred while loading data even if the request to load data was initiated by
            // the user in Settings instead of via the app itself.
        }
        #endregion
    }
}
```

`BeginRequest`呼び出しディレクトリハンドラーのメソッドは、必要な機能を提供するように変更する必要があります。 上記のサンプルの場合は、VOIP アプリの連絡先データベースでブロックされている数と使用可能な番号の一覧を設定しようとします。 何らかの理由でいずれかの要求が失敗した場合は、を作成して `NSError` エラーを説明し、 `CancelRequest` クラスのメソッドに渡し `CXCallDirectoryExtensionContext` ます。

ブロックされた番号を設定するには、 `AddBlockingEntry` クラスのメソッドを使用し `CXCallDirectoryExtensionContext` ます。 メソッドに指定する数値は、数値の昇順にする_必要があり_ます。 多数の電話番号がある場合のパフォーマンスとメモリの最適な使用方法については、特定の時点で数値のサブセットを読み込み、autorelease pool を使用して、読み込まれた番号の各バッチの間に割り当てられたオブジェクトを解放することを検討してください。

VOIP アプリに知られている連絡先番号のアプリに連絡するには、 `AddIdentificationEntry` クラスのメソッドを使用 `CXCallDirectoryExtensionContext` し、番号と識別ラベルの両方を指定します。 ここでも、メソッドに指定する数値は、数値の昇順にする_必要があり_ます。 多数の電話番号がある場合のパフォーマンスとメモリの最適な使用方法については、特定の時点で数値のサブセットを読み込み、autorelease pool を使用して、読み込まれた番号の各バッチの間に割り当てられたオブジェクトを解放することを検討してください。

## <a name="summary"></a>まとめ

この記事では、Apple が iOS 10 でリリースした新しい CallKit API と、Xamarin iOS VOIP アプリで実装する方法について説明しました。 CallKit では、アプリを iOS システムに統合する方法、組み込みアプリ (Phone など) との機能の同等性を提供する方法、Siri の相互作用と連絡先アプリを使用して、ロックやホーム画面などの場所でアプリの可視性を向上させる方法について説明しました。

## <a name="related-links"></a>関連リンク

- [iOS 10 のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS10)
