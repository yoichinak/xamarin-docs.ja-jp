---
title: Xamarin.iOS で CallKit
description: この記事では、その Apple の iOS 10 と Xamarin.iOS VOIP アプリでの実装方法でリリースされた新しい CallKit API について説明します。
ms.prod: xamarin
ms.assetid: 738A142D-FFD2-4738-B3ED-57C273179848
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/15/2017
ms.openlocfilehash: bb70dac34847cf46bd06cc20b87df8ea5f72105a
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50115186"
---
# <a name="callkit-in-xamarinios"></a>Xamarin.iOS で CallKit

IOS 10 で新しい CallKit API は、VOIP アプリを iPhone UI と統合し使い慣れたインターフェイスを提供し、エンドユーザーに発生するための手段を提供します。 この API を使用したユーザーが表示し、iOS デバイスのロック画面から VOIP 通話との対話および連絡先の Phone アプリの使用を管理**お気に入り**と**も最近使ったもの**ビュー。

## <a name="about-callkit"></a>CallKit について

Apple、に従って CallKit のファースト パーティ iOS 10 でのエクスペリエンスにサード パーティ製音声経由で IP (VOIP) のアプリを昇格する新しいフレームワークです。 CallKit API は、VOIP アプリを iPhone UI 統合し使い慣れたインターフェイスを提供し、エンドユーザーに操作を使用できます。 組み込みの Phone アプリと同様、ユーザーが表示し、iOS デバイスのロック画面から VOIP 通話との対話および連絡先の Phone アプリの使用を管理**お気に入り**と**も最近使ったもの**ビュー。

さらに、CallKit API は、データベースを名前 (呼び出し元の ID) と電話番号を関連付けることができます、または (呼び出しをブロックして)、番号がなるべきときに、システムがブロックされているに通知するアプリの拡張機能を作成する機能を提供します。

### <a name="the-existing-voip-app-experience"></a>既存の VOIP アプリケーション エクスペリエンス

新しい CallKit API とその機能を説明する、する前に、サードの現在のユーザー エクスペリエンスを見ては、9 (またはそれ以下) MonkeyCall という架空の VOIP アプリを使用して iOS でアプリを VOIP でパーティします。 MonkeyCall とは、ユーザーが既存の iOS Api を使用して、VOIP 通話を送受信できる単純なアプリです。

現時点では、MonkeyCall の着信呼び出しの受信は、ユーザー、iPhone がロックされている場合は、ロック画面で受信した通知は他の種類の通知 (メッセージからようまたはアプリをたとえばメール)。

呼び出しに応答する場合、ユーザーは、MonkeyCall 通知、アプリを開き、パスコード (または Touch ID のユーザー) の呼び出しをそのまま使用する可能性がありますの会話を始める前に、電話のロックを解除する入力をスライドにする必要があります。

操作は、電話がロックされている場合に均等に面倒です。 ここでも、着信 MonkeyCall 呼び出しは、標準的な通知バナーが画面の上部からスライドとして表示されます。 通知が一時的なのでは、通知センターを開き、回答し、呼び出しまたは検索 MonkeyCall アプリを手動で起動して特定の通知を検索することを強制して、ユーザーが簡単にできなかんだことができます。

### <a name="the-callkit-voip-app-experience"></a>CallKit VOIP アプリのエクスペリエンス

MonkeyCall アプリの新しい CallKit Api を実装すると、iOS 10 での VOIP の着信通話をユーザーのエクスペリエンスが大幅に向上することができます。 上から電話がロックされているときに、VOIP 通話を受信するユーザーの例を実行します。 CallKit を実装すると、呼び出しは全画面表示、ネイティブ UI をスワイプの回答の標準的な機能と、組み込みの Phone アプリからの呼び出しが受信されている場合と同様に、iPhone のロック画面に表示されます。

ここでも、MonkeyCall VOIP 呼び出しが受信したときに、iPhone でロックされているない場合、同じの全画面表示、ネイティブ UI と組み込みのアプリが表示される電話と MonkeyCall の標準のスワイプの回答とタップ-拒否する機能がカスタムの着信音の再生のオプション.

CallKit MonkeyCall、その VOIP を許可する追加機能を呼び出し、組み込みの [最近] に表示されるは、他の種類と対話する呼び出しをお気に入りの一覧を提供する、組み込み不可およびブロック機能を使用するには、Siri から MonkeyCall 呼び出しを起動し、ユーザーが、連絡先アプリ内のユーザーに MonkeyCall 呼び出しを割り当てる機能を提供します。

CallKit アーキテクチャをについて説明します、着信および発信の詳細、フローと CallKit API を呼び出します。

## <a name="the-callkit-architecture"></a>CallKit アーキテクチャ

IOS 10 では、Apple が CallKit すべてのシステム サービスを採用が CallKit 経由でシステムの UI に CarPlay、たとえば、に対する呼び出しが認識されるよう。 MonkeyCall CallKit を採用するために、指定された例では、これらの組み込みのシステム サービスと同じ方法でシステムに認識し、同じ機能のすべてを取得します。

[![](callkit-images/callkit01.png "CallKit サービス スタック")](callkit-images/callkit01.png#lightbox)

上の図から MonkeyCall アプリを詳しく見てください。 アプリでは、すべての独自のネットワークとの通信にそのコードが含まれていて、独自のユーザー インターフェイスが含まれます。 システムとの通信に CallKit でリンクします。

[![](callkit-images/callkit02.png "MonkeyCall アプリのアーキテクチャ")](callkit-images/callkit02.png#lightbox)

CallKit、アプリが使用するには、2 つの主要なインターフェイスがあります。

- `CXProvider` -これにより、帯域外の通知が発生した場合のシステムに通知する MonkeyCall アプリです。
- `CXCallController` -ローカル ユーザーのアクションのシステムに通知する MonkeyCall アプリを許可します。

### <a name="the-cxprovider"></a>CXProvider

上記で説明したように`CXProvider`アプリ、帯域外の通知が発生した場合のシステムに伝達に許可します。 これらは、ローカル ユーザーの操作のため実行されませんが、着信呼び出しなどの外部のイベントが原因で発生することを通知します。

アプリを使用する必要があります、`CXProvider`次の。

- システムに着信呼び出しを報告します。
- レポートで、システムに接続されている呼び出しを送信します。
- システムへの呼び出しを終了するリモート ユーザーをレポートします。

使用して、アプリは、システムと通信する必要があるときに、`CXCallUpdate`クラスを使用して、アプリと通信する場合、システム、`CXAction`クラス。

[![](callkit-images/callkit03.png "CXProvider 経由でシステムとの通信")](callkit-images/callkit03.png#lightbox)

### <a name="the-cxcallcontroller"></a>CXCallController

`CXCallController` VOIP 通話を開始するユーザーなどのローカルのユーザー アクションのシステムに通知するアプリを許可します。 実装することによって、`CXCallController`システム内の他の種類の呼び出しの作用をアプリを取得します。 たとえば、既に進行中のアクティブなテレフォニー呼び出しがある`CXCallController`保留中でその呼び出しを配置し、開始または VOIP 呼び出しに応答する VOIP アプリを許可することができます。

アプリを使用する必要があります、`CXCallController`次の。

- ユーザーには、システムに発信呼び出しが開始されたときにレポートします。
- レポート ユーザーがシステムに着信呼び出しに応答します。
- ユーザーがシステムへの呼び出しが終了したときにレポートします。

アプリは、ローカル ユーザーの操作をシステムと通信する場合、使用して、`CXTransaction`クラス。

[![](callkit-images/callkit04.png "CXCallController を使用して、システムに報告します。")](callkit-images/callkit04.png#lightbox)

## <a name="implementing-callkit"></a>CallKit を実装します。

次のセクションでは、Xamarin.iOS VOIP アプリで CallKit を実装する方法を示します。 この例では、架空の MonkeyCall VOIP アプリからのコードに このドキュメントを使用しているされます。 ここで示すコードは、いくつかのサポート クラスを表します、CallKit の特定の部分は、次のセクションで詳しく説明します。

### <a name="the-activecall-class"></a>ActiveCall クラス

`ActiveCall`クラスは、次のように、現在アクティブな VOIP 呼び出しに関する情報がすべて保持するために、MonkeyCall アプリによって使用されます。

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

`ActiveCall` 呼び出しと呼び出しの状態が変更されたときに発生する可能性が 2 つのイベントの状態を定義するいくつかのプロパティを保持します。 これは、例としてのみであるため、開始、応答、および呼び出しを終了するをシミュレートするために使用する 3 つの方法があります。

### <a name="the-startcallrequest-class"></a>StartCallRequest クラス

`StartCallRequest`静的クラスは、出力方向の操作を呼び出すときに使用されるいくつかのヘルパー メソッドを提供します。

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

`CallHandleFromURL`と`CallHandleFromActivity`クラスは、発信通話で呼び出されている担当者の連絡先のハンドルを取得する、AppDelegate で使用されます。 詳細についてを参照してください、[発信呼び出しの処理](#Handling-Outgoing-Calls)以下のセクション。

### <a name="the-activecallmanager-class"></a>ActiveCallManager クラス

`ActiveCallManager`クラスは MonkeyCall アプリで開いているすべての呼び出しを処理します。

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

もう一度のみ、シミュレーションは、このため、`ActiveCallManager`のみのコレクションを保持`ActiveCall`オブジェクトによって指定された呼び出しを検索するためのルーチンであり、`UUID`プロパティ。 開始、終了および、発信通話の保留中の状態を変更するメソッドも含まれています。 詳細についてを参照してください、[発信呼び出しの処理](#Handling-Outgoing-Calls)以下のセクション。

### <a name="the-providerdelegate-class"></a>ProviderDelegate クラス

前に説明したように、`CXProvider`帯域外の通知、アプリとシステムの間の双方向通信を提供します。 開発者がカスタムを提供する必要がある`CXProviderDelegate`アタッチ先と、`CXProvider`帯域外の CallKit イベントを処理するアプリ。 MonkeyCall は、次を使用して`CXProviderDelegate`:

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

            // Get Image Mask
            var maskImage = UIImage.FromFile ("telephone_receiver.png");

            // Setup the initial configurations
            Configuration = new CXProviderConfiguration ("MonkeyCall") {
                MaximumCallsPerCallGroup = 1,
                SupportedHandleTypes = new NSSet<NSNumber> (handleTypes),
                IconMaskImageData = maskImage.AsPNG(),
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
                    Provider.ReportConnectingOutgoingCall (call.UUID, call.StartedConnectingOn.ToNsDate());
                }
            };

            activeCall.ConnectedChanged += (call) => {
                if (call.isConnected) {
                    // Inform system that the call has connected
                    provider.ReportConnectedOutgoingCall (call.UUID, call.ConnectedOn.ToNsDate ());
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

このデリゲートのインスタンスが作成されたときに渡される、`ActiveCallManager`任意のアクティビティの呼び出しを処理するために使用することです。 次に、ハンドルの型が定義されます (`CXHandleType`) を`CXProvider`に応答します。

```csharp
// Define handle types
var handleTypes = new [] { (NSNumber)(int)CXHandleType.PhoneNumber };
```

呼び出しが進行中の場合、アプリのアイコンに適用されるマスクを取得します。

```csharp
// Get Image Mask
var maskImage = UIImage.FromFile ("telephone_receiver.png");
```

これらの値の取得にバンドルされている、`CXProviderConfiguration`構成に使用される、 `CXProvider`:

```csharp
// Setup the initial configurations
Configuration = new CXProviderConfiguration ("MonkeyCall") {
    MaximumCallsPerCallGroup = 1,
    SupportedHandleTypes = new NSSet<NSNumber> (handleTypes),
    IconMaskImageData = maskImage.AsPNG(),
    RingtoneSound = "musicloop01.wav"
};
```

デリゲートは、新しい作成`CXProvider`でこれらの構成自体が接続すると。

```csharp
// Create a new provider
Provider = new CXProvider (Configuration);

// Attach this delegate
Provider.SetDelegate (this, null);
```

CallKit を使用する場合は、アプリの作成し、独自の音声セッションの処理が不要になった、代わりに構成し、システムを作成し、その処理されるオーディオのセッションを使用する必要があります。 

これを実際のアプリの場合、`DidActivateAudioSession`事前構成された呼び出しを開始するメソッドを使用`AVAudioSession`システム提供。

```csharp
public override void DidActivateAudioSession (CXProvider provider, AVFoundation.AVAudioSession audioSession)
{
    // Start the call's audio session here...
}
```

使用することも、`DidDeactivateAudioSession`の最終処理し、システムへの接続を解放するメソッドに提供される音声セッション。

```csharp
public override void DidDeactivateAudioSession (CXProvider provider, AVFoundation.AVAudioSession audioSession)
{
    // End the calls audio session and restart any non-call
    // releated audio
}
```

コードの残りの部分は、次のセクションで詳しく取り上げられます。

### <a name="the-appdelegate-class"></a>AppDelegate クラス

MonkeyCall のインスタンスを保持するために、AppDelegate を使用して、`ActiveCallManager`と`CXProviderDelegate`アプリ全体にわたって使用されます。

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

`OpenUrl`と`ContinueUserActivity`メソッドは、アプリが、発信通話を処理するときに使用します。 上書きします。 詳細についてを参照してください、[発信呼び出しの処理](#Handling-Outgoing-Calls)以下のセクション。

## <a name="handling-incoming-calls"></a>着信通話の処理

いくつかの状態および受信の VOIP 通話を通過できる通常の受信呼び出しワークフロー中になどのプロセスがあります。

- 着信呼び出しが存在しているユーザー (およびシステム) に通知します。
- ユーザーが、呼び出しに応答する場合に通知を受信し、他のユーザーと呼び出しを初期化します。
- ユーザーが、現在の呼び出しを終了するときに、システムとの通信ネットワークを通知します。

次のセクションでは、例としてもう一度 MonkeyCall VOIP アプリを使用して、着信の呼び出しワークフローを処理するために、アプリが CallKit を使用する方法について詳しく説明になります。

### <a name="informing-user-of-incoming-call"></a>着信通話のユーザーに通知

リモート ユーザーはローカル ユーザーと VOIP メッセージ交換を開始して、次の処理が発生します。

[![](callkit-images/callkit05.png "リモート ユーザーが VOIP メッセージ交換を開始します。")](callkit-images/callkit05.png#lightbox)

1. アプリは、その通信ネットワークから通知を受信 VOIP 呼び出しがあることを取得します。
2. アプリでは、`CXProvider`を送信する、`CXCallUpdate`呼び出しの通知システムにします。
3. システムでは、システム UI、システム サービスおよび CallKit を使用して他の VOIP アプリケーションへの呼び出しを発行します。

たとえば、 `CXProviderDelegate`:

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

このコードを作成する新しい`CXCallUpdate`インスタンスし、呼び出し元を識別するハンドルをアタッチします。 次に、使用して、`ReportNewIncomingCall`のメソッド、`CXProvider`呼び出しのシステムに通知するクラス。 呼び出しを追加が成功した場合、アプリのアクティブな呼び出しのコレクションにいない場合は、エラー報告する必要をユーザーにします。

### <a name="user-answering-incoming-call"></a>ユーザー応答の受信呼び出し

受信の VOIP 呼び出しに応答する場合、以下の処理が行われます。

[![](callkit-images/callkit06.png "ユーザーが 受信の VOIP 呼び出し")](callkit-images/callkit06.png#lightbox)

1. システムの UI は、ユーザーが、VOIP 呼び出しに応答することをシステムに通知します。
2. システムは、送信、`CXAnswerCallAction`アプリの`CXProvider`回答目的の通知。
3. アプリは、呼び出しの応答は、ユーザーを VOIP 通話が通常どおり実行されますが通信ネットワークを通知します。

たとえば、 `CXProviderDelegate`:

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

このコードは、まずアクティブな呼び出しの一覧に特定の呼び出しを検索します。 呼び出しが見つからない場合は、システムは通知され、メソッドが終了します。 見つかった場合、`AnswerCall`のメソッド、`ActiveCall`クラスが呼び出しを開始すると呼ばれ、それが成功または失敗した場合、システムは情報。

### <a name="user-ending-incoming-call"></a>ユーザーが着信呼び出しを終了します。

アプリの UI 内からの呼び出しを終了する場合は、次の処理が発生します。

[![](callkit-images/callkit07.png "ユーザーがアプリの UI 内からの呼び出しを終了します")](callkit-images/callkit07.png#lightbox)

1. アプリを作成します`CXEndCallAction`にバンドルを取得する、`CXTransaction`呼び出しが終了することを通知するシステムに送信されます。
2. システムは、末尾呼び出しの目的を検証し、送信、`CXEndCallAction`を使用してアプリに戻り、`CXProvider`します。
3. アプリはその呼び出しが終了になっていることに、通信ネットワークを通知します。

たとえば、 `CXProviderDelegate`:

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

このコードは、まずアクティブな呼び出しの一覧に特定の呼び出しを検索します。 呼び出しが見つからない場合は、システムは通知され、メソッドが終了します。 見つかった場合、`EndCall`のメソッド、`ActiveCall`クラスが通話を終了すると呼ばれ、それが成功または失敗した場合、システムは情報。 成功した場合は、呼び出しがアクティブな呼び出しのコレクションから削除されます。

## <a name="managing-multiple-calls"></a>複数の呼び出しを管理します。

ほとんどの VOIP アプリでは、一度に複数の呼び出しを処理できます。 たとえば、現在アクティブな VOIP 呼び出しがあるし、そこに新しい受信呼び出し、ユーザーがあるアプリの取得の通知が一時停止できる場合または 2 つ目の回答を最初に呼び出すには、[切断]。

システムを送信するのには、上記のような状況により、`CXTransaction`を複数のアクションの一覧に含まれるアプリに (など、 `CXEndCallAction` 、 `CXAnswerCallAction`)。 これらの操作には、システムでは、UI を適切に更新できるように、個別に処理を完了する必要があります。

## <a name="handling-outgoing-calls"></a>送信処理を呼び出す

ユーザーが (電話アプリ) で最近使用した一覧からエントリをタップした場合などは、アプリに属している呼び出しから送信されます、_呼び出す目的の起動_システムで。

[![](callkit-images/callkit08.png "開始の呼び出しの目的の受信")](callkit-images/callkit08.png#lightbox)

1. アプリによって作成され、_呼び出しアクションの開始_開始呼び出す目的に基づいて、システムから受信しました。 
2. アプリを使用して、`CXCallController`システムからの呼び出しを開始アクションを要求します。
3. 使用してアプリに返される、システムが、操作を受け入れる場合、`XCProvider`を委任します。
4. アプリは、その通信ネットワークで発信通話を開始します。

インテントの詳細についてを参照してください、 [Intents および Intents UI 拡張機能](~/ios/platform/sirikit/understanding-sirikit.md)ドキュメント。 

### <a name="the-outgoing-call-lifecycle"></a>送信呼び出しのライフ サイクル

CallKit と発信呼び出しを使用する場合、アプリは、次のライフ サイクル イベントのシステムに通知する必要があります。

1. **開始**-開始しようとして、発信通話であるシステムに伝達します。
2. **開始**-発信呼び出しが開始されたシステムに伝達します。
3. **接続**-発信通話を接続しているシステムに伝達します。
4. **接続されている**-送信を通知する呼び出しが接続されているし、当事者の双方の通信できるようになりました。

たとえば、次のコードが、発信通話が開始されます。

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

作成されます、`CXHandle`構成を使用して、`CXStartCallAction`にバンドルされています、`CXTransaction`を使用して、システムに送信される、`RequestTransaction`のメソッド、`CXCallController`クラス。 呼び出すことによって、`RequestTransaction`メソッド、システムは、ソースに関係なく、既存の呼び出しで保留中を配置できます (FaceTime、VOIP、Phone アプリなど)、新しい呼び出しを開始する前に、します。

発信の VOIP 通話を開始する要求には、Siri、(連絡先アプリ) 内の連絡先カードのエントリなど、さまざまなソースから、または (電話アプリ) で最近使用した一覧からを取得できます。 このような場合は、アプリが送信されます、開始呼び出しインテント内で、`NSUserActivity`して処理する必要があります、AppDelegate と。

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

ここで、`CallHandleFromActivity`ヘルパー クラスのメソッド`StartCallRequest`呼び出されている人に、ハンドルの取得に使用されている (を参照してください[StartCallRequest クラス](#The-StartCallRequest-Class)上)。 

`PerformStartCallAction`のメソッド、 [ProviderDelegate クラス](#The-ProviderDelegate-Class)最後に実際の送信呼び出しを起動し、そのライフ サイクルのシステムに伝達するために使用します。

```csharp
public override void PerformStartCallAction (CXProvider provider, CXStartCallAction action)
{
    // Create new call record
    var activeCall = new ActiveCall (action.CallUuid, action.CallHandle.Value, true);

    // Monitor state changes
    activeCall.StartingConnectionChanged += (call) => {
        if (call.IsConnecting) {
            // Inform system that the call is starting
            Provider.ReportConnectingOutgoingCall (call.UUID, call.StartedConnectingOn.ToNsDate());
        }
    };

    activeCall.ConnectedChanged += (call) => {
        if (call.IsConnected) {
            // Inform system that the call has connected
            Provider.ReportConnectedOutgoingCall (call.UUID, call.ConnectedOn.ToNsDate ());
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

インスタンスを作成、`ActiveCall`クラス (進行中の呼び出しに関する情報を保持) して呼び出される人に設定します。 `StartingConnectionChanged`と`ConnectedChanged`イベントを監視し、送信呼び出しのライフ サイクルの報告に使用されます。 呼び出しが開始され、システム通知アクションが完了しました。

### <a name="ending-an-outgoing-call"></a>発信通話を終了

ユーザーは、発信通話を終了するとしたら、次のコードを使用できます。

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

場合作成、`CXEndCallAction`終了への呼び出しの UUID をバンドルで、`CXTransaction`を使用して、システムに送信、`RequestTransaction`のメソッド、`CXCallController`クラス。 

## <a name="additional-callkit-details"></a>CallKit の詳細

このセクションでは、開発者が CallKit などの操作する際に考慮する必要のある追加の詳細情報を取り上げます。

- プロバイダーの構成
- アクション エラー
- システム上の制限
- VOIP オーディオ

### <a name="provider-configuration"></a>プロバイダーの構成

プロバイダーの構成により、ときに (ネイティブの呼び出しで UI) 内のユーザー エクスペリエンスをカスタマイズする iOS 10 を VOIP アプリ CallKit を使用します。

アプリには、次の種類のカスタマイズを作成できます。

- ローカライズされた名前を表示します。
- ビデオの呼び出しのサポートを有効にします。
- 呼び出しの UI のボタンをカスタマイズするには、マスクされたイメージ アイコンを表示します。 カスタム ボタンを持つユーザーの操作は、処理するアプリに直接送信されます。 

### <a name="action-errors"></a>アクション エラー

CallKit を使用して、iOS 10 VOIP アプリは、適切に失敗したアクションを処理し、アクションの状態を常に通知するユーザーを保持する必要があります。 

次の例を考慮に入れてください。

1. アプリでは、呼び出しの開始アクションが受信し、その通信ネットワークで新しい VOIP 通話を初期化するプロセスが開始されました。
2. 制限付きまたはネットワーク通信する機能もありません、のため、この接続が失敗します。
3. アプリ*する必要があります*送信、**失敗**メッセージ呼び出しを開始アクションを (`Action.Fail()`)、エラーのシステムに通知します。
4. これにより、呼び出しの状態のユーザーに通知するシステムです。 たとえば、障害を呼び出す UI を表示するには、です。

さらに、iOS 10 を VOIP アプリに応答する必要は_タイムアウト エラー_所定の時間内で、予期されるアクションを処理できないときに発生することができます。 CallKit によって提供される各アクションの種類には、関連付けられている最大タイムアウト値があります。 これらのタイムアウト値は、ユーザーによって要求された CallKit アクションが処理される、応答性の高い方法で保持し、OS や応答性の流動性も確認します。

プロバイダー デリゲートにはいくつかの方法があります (`CXProviderDelegate`) もこのタイムアウト状況を適切に処理するをオーバーライドする必要があります。

### <a name="system-restrictions"></a>システム上の制限

IOS 10 の VOIP のアプリを実行している iOS デバイスの現在の状態に基づいて、特定のシステム制限を適用可能性があります。

たとえば場合、は、着信 VOIP 呼び出しをシステムによって制限できます。

1. 相手の呼び出しは、ユーザーのブロックされた呼び出し元の一覧です。
2. ユーザーの iOS デバイスは、Do Not Disturb モードです。

VOIP 呼び出しは、このような状況のいずれかが制限されている場合は、それを処理するために、次のコードを使用します。

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

CallKit は、iOS 10 を VOIP アプリがライブの VOIP 呼び出し中に必要なオーディオのリソースを処理するためのいくつかの利点を提供します。 最大のメリットの 1 つは、アプリの音声セッションが引き上げの優先順位 iOS 10 で実行する場合。 これは、組み込みの電話と同じ優先度レベルと FaceTime アプリとこの強化された優先順位は VOIP アプリの音声セッションを中断して他の実行中のアプリを防止します。

また、CallKit では、パフォーマンスを向上したり、ユーザー設定とデバイスの状態に基づくライブの呼び出し中に特定の出力デバイスに VOIP オーディオをインテリジェントにルーティングするその他のオーディオ ルーティング ヒントへのアクセスがあります。 たとえば、bluetooth ヘッドホン、CarPlay のライブ接続またはユーザー補助の設定など、接続されているデバイスに基づいています。

一般的な VOIP のライフ サイクル中に CallKit を使用して呼び出す、アプリが CallKit が提供するオーディオ Stream を構成する必要があります。 次の例を参照してください。

[![](callkit-images/callkit09.png "呼び出し操作のシーケンスを開始")](callkit-images/callkit09.png#lightbox)

1. 呼び出しの開始アクションは、着信の呼び出しに応答するアプリケーションによって受信されます。
2. このアクションは、アプリによって満たされる、前にの構成が必要なは提供します。 その`AVAudioSession`します。
3. アプリは、アクションが満たされていることをシステムに通知します。
4. CallKit が高優先順位を提供する、呼び出しが接続すると、前に`AVAudioSession`アプリが要求した構成に一致します。 アプリで通知されます、`DidActivateAudioSession`のメソッド、`CXProviderDelegate`します。

## <a name="working-with-call-directory-extensions"></a>呼び出しのディレクトリ拡張機能の使用

CallKit を使用する場合_ディレクトリ拡張機能を呼び出す_ブロックされた呼び出しの番号を追加して、iOS デバイスで連絡先アプリで連絡先を特定の VOIP アプリに固有の番号を確認する方法を提供します。

### <a name="implementing-a-call-directory-extension"></a>呼び出しのディレクトリ拡張機能の実装

Xamarin.iOS アプリでは、呼び出しのディレクトリ拡張機能を実装するには、次の操作を行います。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. Visual studio for mac アプリのソリューションを開きます
2. ソリューション名を右クリックし、**ソリューション エクスプ ローラー**選択**追加** > **新しいプロジェクトの追加**します。
3. 選択**iOS** > **拡張機能** > **ディレクトリ拡張機能を呼び出す** をクリックし、 **次へ**ボタン。 

    [![](callkit-images/calldir01.png "新しい呼び出しディレクトリ拡張機能の作成")](callkit-images/calldir01.png#lightbox)
4. 入力、**名前**拡張機能をクリックして、**次**ボタン。 

    [![](callkit-images/calldir02.png "拡張機能の名前を入力")](callkit-images/calldir02.png#lightbox)
5. 調整、**プロジェクト名**や**ソリューション名**ために必要なクリックすると、**作成**ボタン。 

    [![](callkit-images/calldir03.png "プロジェクトの作成")](callkit-images/calldir03.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Visual Studio で、アプリのソリューションを開きます。
2. ソリューション名を右クリックし、**ソリューション エクスプ ローラー**選択**追加** > **新しいプロジェクトの追加**します。
3. 選択**iOS** > **拡張機能** > **ディレクトリ拡張機能を呼び出す** をクリックし、 **次へ**ボタン。 

    [![](callkit-images/calldir01w.png "新しい呼び出しディレクトリ拡張機能の作成")](callkit-images/calldir01.png#lightbox)
4. 入力、**名前**拡張機能をクリックして、 **OK**ボタン

-----

これは追加、`CallDirectoryHandler.cs`クラスを次のようなプロジェクト。

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

`BeginRequest`メソッドを呼び出すディレクトリ ハンドラーでは、必要な機能を提供するように変更する必要があります。 上記のサンプルの場合、VOIP アプリの連絡先データベースでブロックされていると、使用可能な数値のリストを設定することを試みます。 作成するかは、何らかの理由で失敗を要求している場合、`NSError`にエラーを説明し、渡すこと、`CancelRequest`のメソッド、`CXCallDirectoryExtensionContext`クラス。

ブロックされている数値の使用を設定する、`AddBlockingEntry`のメソッド、`CXCallDirectoryExtensionContext`クラス。 メソッドに提供される数字_する必要があります_数値で昇順にします。 最適なパフォーマンスとメモリ使用量が多くの電話番号、特定の時点で番号のサブセットを読み込むと、各バッチに読み込まれる数値の中に割り当てられたオブジェクトを解放する場合のプールを使用してのみを検討します。

既知の VOIP のアプリに連絡先電話番号の連絡先アプリに通知を使用して、`AddIdentificationEntry`のメソッド、`CXCallDirectoryExtensionContext`クラスし、数と識別のラベルの両方を指定します。 メソッドに提供される数字、もう一度_する必要があります_数値で昇順にします。 最適なパフォーマンスとメモリ使用量が多くの電話番号、特定の時点で番号のサブセットを読み込むと、各バッチに読み込まれる数値の中に割り当てられたオブジェクトを解放する場合のプールを使用してのみを検討します。

## <a name="summary"></a>まとめ

この記事では、Apple の iOS 10 と Xamarin.iOS VOIP アプリでの実装方法でリリースされた新しい CallKit API をについて説明します。 CallKit が iOS システムに統合するアプリを使用する方法について、(電話) などの組み込みアプリを使用して同等の機能を提供する方法、および Siri の相互作用およびを使用して、ロック画面とホーム画面などの場所での iOS でアプリの可視性が増加する方法が示されて連絡先アプリです。

## <a name="related-links"></a>関連リンク

- [iOS 10 のサンプル](https://developer.xamarin.com/samples/ios/iOS10/)
