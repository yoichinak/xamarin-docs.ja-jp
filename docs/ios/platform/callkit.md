---
title: CallKit
description: この記事では、新しい CallKit API その Apple iOS 10 と Xamarin.iOS VOIP アプリで実装する方法でリリースについて説明します。
ms.prod: xamarin
ms.assetid: 738A142D-FFD2-4738-B3ED-57C273179848
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/15/2017
ms.openlocfilehash: 67c761aa6656b571f16632dd1a076ff11737a424
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="callkit"></a>CallKit

_この記事では、新しい CallKit API その Apple iOS 10 と Xamarin.iOS VOIP アプリで実装する方法でリリースについて説明します。_


IOS 10 で新しい CallKit API では、VOIP アプリ iPhone UI と統合し際に使い慣れたインターフェイスを提供し、エンドユーザーに発生するための手段を提供します。 この API ではユーザーが表示し、iOS デバイスのロック画面からの VOIP 呼び出しとの対話および連絡先の電話アプリの使用を管理**お気に入り**と**最近**ビュー。

## <a name="about-callkit"></a>CallKit について

Apple にに従って CallKit は、ファースト パーティ iOS 10 でのエクスペリエンスにサード パーティ製音声経由で IP (VOIP) アプリを昇格する新しいフレームワークです。 CallKit API は、iPhone UI との統合し、際に使い慣れたインターフェイスを提供するエンド ユーザー エクスペリエンスに VOIP アプリを使用します。 組み込みの Phone アプリと同じように、ユーザーが表示し、iOS デバイスのロック画面からの VOIP 呼び出しとの対話および連絡先の電話アプリの使用を管理**お気に入り**と**最近**ビュー。

さらに、CallKit API は、ことができます (呼び出し元の ID) の名前と電話番号を関連付けるか (呼び出しをブロックして)、番号がなるべきときに、システムがブロックされているアプリ拡張機能を作成する機能を提供します。

### <a name="the-existing-voip-app-experience"></a>既存の VOIP アプリ エクスペリエンス

新しい CallKit API とその機能を説明する前に、第 3 の現在のユーザー エクスペリエンスを調べてパーティの iOS でアプリを VOIP 9 (およびいずれか小さいほう) MonkeyCall と呼ばれる架空 VOIP アプリの使用します。 MonkeyCall は、ユーザーが送受信するように、既存の iOS Api を使用して、VOIP 呼び出しできる簡単なアプリです。

現時点では、MonkeyCall 着信呼び出しの受信は、ユーザーが iPhone がロックされている場合は、ロック画面で受信した通知は通知の他の種類と区別 (などのメッセージから、またはアプリなど)。

ユーザーは、呼び出しに応答する場合に、アプリを開き、呼び出しをそのまま使用する可能性があります、メッセージ交換を開始する前に、電話のロックを解除する、パスコード (またはタッチ ID のユーザー) を入力する MonkeyCall 通知をスライドにする必要があります。

操作は、携帯電話のロックが解除されている場合に均等に面倒です。 もう一度、着信 MonkeyCall 呼び出しは、標準的な通知バナーで、画面の上部からスライドとして表示されます。 通知は一時的であるためには、通知センターを開きに応答する、呼び出しまたは検索 MonkeyCall アプリの起動を手動で特定の通知を検索するユーザーに強制することで簡単に見逃さことができます。

### <a name="the-callkit-voip-app-experience"></a>CallKit VOIP アプリ エクスペリエンス

MonkeyCall アプリの新しい CallKit Api を実装すると、着信 VOIP 呼び出しでのユーザー エクスペリエンスを 10、iOS でが大幅に向上することができます。 上記の電話がロックされているときに、VOIP 電話を受けるユーザーの例を実行します。 CallKit を実装すると、呼び出しは組み込み Phone アプリから、全画面表示、ネイティブ UI および標準的な方向にスワイプの解答機能を使用、呼び出しを受け取ったされている場合と同じように、iPhone のロック画面に表示されます。

もう一度、MonkeyCall VOIP 呼び出しを受信したときに、iPhone がロックされているない場合は、同じの全画面表示、ネイティブ UI および、組み込みの Phone アプリが表示されると MonkeyCall の標準のスワイプの回答とタップ-拒否機能が含まカスタム着信音の再生のオプション.

CallKit MonkeyCall、その VOIP を許可する追加機能が組み込まれた最近の表示への呼び出しの他の種類と対話するために呼び出すし、お気に入りの一覧を提供する、組み込みの不可とブロックの機能を使用するには、Siri から MonkeyCall 呼び出しを開始し、ユーザーが、アドレス帳アプリ内のユーザーに MonkeyCall 呼び出しを割り当てる機能を提供します。

次のセクションでは、CallKit アーキテクチャについて説明します、着信および発信の詳細、フロー、および CallKit API を呼び出します。


## <a name="the-callkit-architecture"></a>CallKit アーキテクチャ

10、iOS で Apple を採用していますですべてのシステム サービス CallKit CarPlay、たとえば、に対する呼び出しが CallKit 経由でシステム UI に認識されるようです。 例では、下 MonkeyCall CallKit が採用されていますので、これらの組み込みのシステム サービスと同じ方法でシステムに認識し、すべての同じ機能を取得します。

[![](callkit-images/callkit01.png "CallKit サービス スタック")](callkit-images/callkit01.png#lightbox)

上の図から MonkeyCall アプリについて詳しく見てを実行します。 アプリでは、独自のネットワークと通信するようにコードがすべて含まれます、独自のユーザー インターフェイスが含まれます。 システムと通信する CallKit でリンクします。

[![](callkit-images/callkit02.png "MonkeyCall アプリのアーキテクチャ")](callkit-images/callkit02.png#lightbox)

CallKit アプリを使用するには、2 つのメイン インターフェイスがあります。

- `CXProvider` -これにより、帯域外の通知が発生した場合のシステムに通知する MonkeyCall アプリです。
- `CXCallController` -MonkeyCall アプリのローカルのユーザー アクションのシステムに通知を許可します。

### <a name="the-cxprovider"></a>CXProvider

、前に述べたよう`CXProvider`を使用するアプリが発生した場合、帯域外の通知システムに通知します。 これらは、ローカル ユーザーの操作のため実行されませんが、着信呼び出しなどの外部イベントが原因で発生した通知です。

アプリを使用する必要があります、`CXProvider`以下について。

- システムへの着信呼び出しを報告します。
- レポートで、出力方向の呼び出しが、システムに接続します。
- システムへの呼び出しを終了するリモート ユーザーをレポートします。

使用して、アプリは、システムと通信する必要があるときに、`CXCallUpdate`アプリと通信する場合、システムを使用して、クラス、`CXAction`クラス。

[![](callkit-images/callkit03.png "CXProvider 経由でシステムとの通信")](callkit-images/callkit03.png#lightbox)

### <a name="the-cxcallcontroller"></a>CXCallController

`CXCallController` VOIP 呼び出しを開始するユーザーなどのローカルのユーザー アクションのシステムに通知するアプリを許可します。 実装することによって、`CXCallController`システム内の他の種類の呼び出しの相互作用へのアプリを取得します。 たとえば、既に進行中のアクティブなテレフォニー呼び出しがある`CXCallController`VOIP アプリへの保留中の呼び出しを配置し、開始または VOIP 呼び出しに応答を許可することができます。

アプリを使用する必要があります、`CXCallController`以下について。

- ユーザーがシステムへの送信呼び出しを開始したときにレポートします。
- レポート ユーザー、システムへの着信呼び出しに応答します。
- レポート ユーザーが、システムへの呼び出しが終了するとします。

アプリは、システムにローカル ユーザーの操作を通信する場合、これを使用して、`CXTransaction`クラス。

[![](callkit-images/callkit04.png "CXCallController を使用して、システムに報告します。")](callkit-images/callkit04.png#lightbox)

## <a name="implementing-callkit"></a>CallKit を実装します。

以下のセクションでは、Xamarin.iOS VOIP アプリで CallKit を実装する方法を示します。 この例では、このドキュメントを使用する架空の MonkeyCall VOIP アプリからのコード。 ここで示すコードは、いくつかのサポート クラスを表す、特定の部分は CallKit は、次のセクションで詳しく説明します。

### <a name="the-activecall-class"></a>ActiveCall クラス

`ActiveCall`クラスは、MonkeyCall アプリですべての次のように現在アクティブになっている VOIP 呼び出しに関する情報を保持するために使用します。

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

`ActiveCall` 呼び出しの状態が変更されたときに発生する可能性が 2 つのイベントと呼び出しの状態を定義するいくつかのプロパティを保持します。 これが例のみであるため、開始、応答、および呼び出しを終了するをシミュレートするために使用する 3 つの方法があります。

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

`CallHandleFromURL`と`CallHandleFromActivity`クラスで使用して、AppDelegate を発信呼び出しで呼び出されている人物の連絡先のハンドルを取得します。 詳細についてを参照してください、[呼び出しの送信処理](#Handling-Outgoing-Calls)以下のセクションです。

### <a name="the-activecallmanager-class"></a>ActiveCallManager クラス

`ActiveCallManager`クラスは、MonkeyCall アプリで開いているすべての呼び出しを処理します。

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
                if (call.UUID == uuid) return call;
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

もう一度だけ、シミュレーションは、このため、`ActiveCallManager`だけのコレクションを保持`ActiveCall`オブジェクトによって、特定の呼び出しを検索するためのルーチンがあり、`UUID`プロパティです。 また、開始、終了、および発信の呼び出しの保留中の状態を変更する方法も含まれます。 詳細についてを参照してください、[呼び出しの送信処理](#Handling-Outgoing-Calls)以下のセクションです。

### <a name="the-providerdelegate-class"></a>ProviderDelegate クラス

、前述のとおり、`CXProvider`帯域外の通知のアプリケーションとシステムの間の双方向通信を提供します。 開発者はカスタムを提供する必要があります`CXProviderDelegate`それに接続して、`CXProvider`アプリが帯域外の CallKit イベントを処理するためです。 MonkeyCall は、次を使用して`CXProviderDelegate`:

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

このデリゲートのインスタンスの作成時に渡される、`ActiveCallManager`任意の呼び出し活動を処理に使用することです。 次に、ハンドルの型定義 (`CXHandleType`) を`CXProvider`が応答します。

```csharp
// Define handle types
var handleTypes = new [] { (NSNumber)(int)CXHandleType.PhoneNumber };
```

呼び出しが進行中に、アプリのアイコンに適用するマスクを取得します。

```csharp
// Get Image Mask
var maskImage = UIImage.FromFile ("telephone_receiver.png");
```

これらの値を取得中に含まれて、`CXProviderConfiguration`構成に使用される、 `CXProvider`:

```csharp
// Setup the initial configurations
Configuration = new CXProviderConfiguration ("MonkeyCall") {
    MaximumCallsPerCallGroup = 1,
    SupportedHandleTypes = new NSSet<NSNumber> (handleTypes),
    IconMaskImageData = maskImage.AsPNG(),
    RingtoneSound = "musicloop01.wav"
};
```

デリゲート作成し、新しい`CXProvider`これらの構成とそれ自体が接続すると。

```csharp
// Create a new provider
Provider = new CXProvider (Configuration);

// Attach this delegate
Provider.SetDelegate (this, null);
```

CallKit を使用する場合、アプリの作成し、オーディオのセッションでは、独自の処理は不要になった、代わりに、必要がありますを構成し、システムは作成され、それを処理するオーディオのセッションを使用します。 

これが実際のアプリの場合、`DidActivateAudioSession`事前構成された、通話の開始に使用するメソッド`AVAudioSession`システムが提供されます。

```csharp
public override void DidActivateAudioSession (CXProvider provider, AVFoundation.AVAudioSession audioSession)
{
    // Start the call's audio session here...
}
```

使用する場合と、 `DidDeactivateAudioSession` finalize およびシステムへの接続を解放するメソッドは、オーディオのセッションを提供します。

```csharp
public override void DidDeactivateAudioSession (CXProvider provider, AVFoundation.AVAudioSession audioSession)
{
    // End the calls audio session and restart any non-call
    // releated audio
}
```

コードの残りの部分は、次のセクションで詳しく取り上げます。

### <a name="the-appdelegate-class"></a>AppDelegate クラス

MonkeyCall のインスタンスを保持するために、AppDelegate を使用して、`ActiveCallManager`と`CXProviderDelegate`アプリ全体を使用します。

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

`OpenUrl`と`ContinueUserActivity`オーバーライド メソッドは、アプリの送信呼び出しが処理するときに使用します。 詳細についてを参照してください、[呼び出しの送信処理](#Handling-Outgoing-Calls)以下のセクションです。

## <a name="handling-incoming-calls"></a>着信呼び出しを処理

いくつかの状態と受信の VOIP 呼び出しを通過できる通常の入力方向の呼び出しワークフロー中などのプロセスがあります。

- 着信呼び出しが存在するユーザー (およびシステム) に通知します。
- ユーザーが、呼び出しに応答するときに通知を受信し、他のユーザーの呼び出しを初期化します。
- ユーザーが、現在の呼び出しを終了するときに、システムとの通信ネットワークを通知します。

次のセクションでは、アプリが CallKit を使用して、例として、MonkeyCall VOIP アプリを使用して、着信の呼び出しワークフローを処理する方法についての詳細になります。

### <a name="informing-user-of-incoming-call"></a>着信呼び出しのユーザーに通知

リモート ユーザーには、ローカル ユーザーと VOIP メッセージ交換が開始された、ときに、以下の処理が行われます。

[![](callkit-images/callkit05.png "リモート ユーザーが VOIP メッセージ交換を開始します。")](callkit-images/callkit05.png#lightbox)

1. アプリは、その通信ネットワークから通知を受信 VOIP 呼び出しがあることを取得します。
2. アプリの使用、`CXProvider`を送信する、`CXCallUpdate`通知呼び出しのシステムにします。
3. システムでは、システム UI、システム サービスおよび CallKit を使用してその他の VOIP アプリへの呼び出しを発行します。

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

このコードでは、新しい`CXCallUpdate`インスタンスし、呼び出し元を識別するハンドルをアタッチします。 次に、使用して、`ReportNewIncomingCall`のメソッド、`CXProvider`クラス、呼び出しのシステムに通知します。 呼び出しが追加された場合は、アプリのアクティブな呼び出しのコレクションにいない場合は、エラー必要があるユーザーに報告します。

### <a name="user-answering-incoming-call"></a>ユーザー応答の受信呼び出し

着信の VOIP 呼び出しに応答する場合、以下の処理が行われます。

[![](callkit-images/callkit06.png "ユーザーが着信 VOIP 呼び出しに応答します。")](callkit-images/callkit06.png#lightbox)

1. システム UI は、ユーザーが VOIP 呼び出しに応答することをシステムに通知します。
2. システムは、送信、`CXAnswerCallAction`にアプリの`CXProvider`応答意図の通知です。
3. アプリの通信ネットワークを通知呼び出しの応答は、ユーザーと、VOIP 呼び出しが通常どおり実行されます。

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

このコードは、まず、アクティブな呼び出しの一覧に特定の呼び出しを検索します。 呼び出しが見つからない場合は、システムは通知され、メソッドが終了します。 見つかった場合、`AnswerCall`のメソッド、`ActiveCall`クラスが呼び出しを開始すると呼ばれ、成功または失敗した場合、システムは情報。

### <a name="user-ending-incoming-call"></a>ユーザーが着信呼び出しを終了します。

アプリの UI 内での呼び出しを終了する場合は、次の処理が発生します。

[![](callkit-images/callkit07.png "ユーザーから、アプリの UI 内での呼び出しを終了します。")](callkit-images/callkit07.png#lightbox)

1. アプリを作成`CXEndCallAction`にバンドルを取得する、`CXTransaction`呼び出しが終了することを通知するシステムに送信されます。
2. システムが末尾呼び出しの目的を確認し、送信、`CXEndCallAction`を使用してアプリに戻る、`CXProvider`です。
3. アプリに通知し、通信ネットワーク呼び出しが終了することです。

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

このコードは、まず、アクティブな呼び出しの一覧に特定の呼び出しを検索します。 呼び出しが見つからない場合は、システムは通知され、メソッドが終了します。 見つかった場合、`EndCall`のメソッド、`ActiveCall`クラスが通話を終了すると呼ばれ、成功または失敗した場合、システムは情報。 成功した場合、呼び出しがアクティブな呼び出しのコレクションから削除されます。

## <a name="managing-multiple-calls"></a>複数の呼び出しを管理します。

ほとんどの VOIP アプリでは、一度に複数の呼び出しを処理できます。 例では、現在アクティブな VOIP 呼び出しがある場合がありますが、新しい入力方向の呼び出しで、ユーザー アプリの取得の通知を一時停止できますまたは最初の呼び出し、2 つ目の回答を切断します。

上記のような状況ことで、システムから送信されます、`CXTransaction`アプリを複数のアクションの一覧が含まれます (など、`CXEndCallAction`および`CXAnswerCallAction`)。 これらの操作する必要があります満たされるのか個別に、システムが適切に UI を更新できるようにします。

## <a name="handling-outgoing-calls"></a>発信呼び出しの処理

場合は、ユーザーが最近一覧 (Phone アプリ) からのエントリをタップなど、アプリに属する呼び出しである送信されます、_呼び出して目的の起動_システムで。

[![](callkit-images/callkit08.png "開始の呼び出しの目的の受信")](callkit-images/callkit08.png#lightbox)

1. このアプリによって作成、_呼び出しアクションの開始_開始呼び出す目的に基づいて、システムから受信したことです。 
2. アプリを使用して、`CXCallController`をシステムから開始呼び出しアクションを要求します。
3. 使用してアプリに返されるシステムがアクションを受け入れる場合、`XCProvider`を委任します。
4. アプリは、その通信ネットワークでの送信呼び出しを開始します。

目的の詳細についてを参照してください、[インテント UI 拡張機能のインテントと](~/ios/platform/sirikit/understanding-sirikit.md)ドキュメント。 

### <a name="the-outgoing-call-lifecycle"></a>出力方向の呼び出しのライフ サイクル

CallKit と送信呼び出しを処理するとき、アプリは、次のライフ サイクル イベントのシステムに通知する必要があります。

1. **開始**-送信呼び出しが開始されようとしてがシステムに通知します。
2. **開始**-出力方向のコールが開始されたシステムに通知します。
3. **接続する**-送信呼び出しが接続しているシステムに通知します。
4. **接続されている**-ことを通知し、送信呼び出しが接続している双方の通信できるようになりました。

たとえば、次のコードに呼び出しが開始されます。

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

作成、`CXHandle`構成を使用して、`CXStartCallAction`にバンドルされています、`CXTransaction`を使用して、システムに送信される、`RequestTransaction`のメソッド、`CXCallController`クラスです。 呼び出して、`RequestTransaction`メソッド、システムがソースに関係なく、既存の呼び出しの保留中を配置できます (Phone アプリ、FaceTime、VOIP など)、新しい呼び出しを開始する前に、します。

送信の VOIP 呼び出しを開始する要求には、Siri、(連絡先アプリの場合) で連絡先カード上のエントリなど、いくつかの異なるソースから、または、最近の一覧から (Phone アプリ) を取得できます。 このような場合は、アプリは送信されます、開始呼び出しの目的の内側、 `NSUserActivity` AppDelegate はそれを処理する必要があります。

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

ここで、`CallHandleFromActivity`ヘルパー クラスのメソッド`StartCallRequest`が呼び出されているユーザーに、ハンドルの取得に使用されている (を参照してください[StartCallRequest クラス](#The-StartCallRequest-Class)上)。 

`PerformStartCallAction`のメソッド、 [ProviderDelegate クラス](#The-ProviderDelegate-Class)最後に実際の送信呼び出しを開始し、そのライフ サイクルのシステムに通知するために使用します。

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

インスタンスを作成、 `ActiveCall` (進行中の呼び出しに関する情報を保持) するクラスし、呼び出されるユーザーを追加します。 `StartingConnectionChanged`と`ConnectedChanged`イベントを監視し、送信呼び出しのライフ サイクルを報告する使用されます。 呼び出しが開始され、システム通知アクションが完了しました。

### <a name="ending-an-outgoing-call"></a>出力方向の呼び出しを終了します。

ユーザーは、送信呼び出しで終了することを希望したら、次のコードを使用できます。

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

場合を作成、`CXEndCallAction`終了の呼び出しの uuid、バンドルで、`CXTransaction`を使用して、システムに送信される、`RequestTransaction`のメソッド、`CXCallController`クラスです。 

## <a name="additional-callkit-details"></a>追加 CallKit 詳細

このセクションでは、開発者は CallKit のように使用する際に考慮する必要がある追加の詳細情報について説明します。

- プロバイダーの構成
- アクション エラー
- システム上の制限
- VOIP オーディオ

### <a name="provider-configuration"></a>プロバイダーの構成

プロバイダーの構成を使用すると (ネイティブの呼び出しで UI) 内のユーザー エクスペリエンスをカスタマイズする iOS 10 VOIP アプリ CallKit を使用します。

アプリは、次の種類のカスタマイズを行うことができます。

- ローカライズされた名前を表示します。
- 映像通話のサポートを有効にします。
- マスクされたイメージ アイコンを提示して In 呼び出し UI のボタンをカスタマイズします。 カスタム ボタンを持つユーザーの操作は、処理するアプリに直接送信されます。 

### <a name="action-errors"></a>アクション エラー

CallKit を使用して iOS 10 VOIP アプリは、正常に失敗しているアクションを処理して、ユーザーが常にアクションの状態の通知する必要があります。 

次の例を考慮します。

1. アプリは、呼び出しの開始アクションが受信したし、の通信ネットワークで新しい VOIP 呼び出しの初期化中のプロセスが開始されました。
2. 制限付きまたはネットワークの通信機能がないのため、この接続が失敗します。
3. アプリ*必要があります*送信、**失敗**開始呼び出しアクションに返送メッセージ (`Action.Fail()`) 障害のシステムに通知します。
4. これにより、呼び出しの状態のユーザーに通知するシステムです。 たとえば、呼び出すエラー UI を表示するには、です。

また、iOS 10 VOIP アプリに応答する必要が_タイムアウト エラー_予期されるアクションを一定時間内で処理できないときに発生することができます。 CallKit によって提供される各アクションの種類には、関連付けられている最大タイムアウト値があります。 これらのタイムアウト値は、ユーザーによって要求された CallKit アクションは、処理される、応答性の高い方法でレベルに抑える OS 流動性および応答性もを確認します。

プロバイダー デリゲートではいくつかの方法があります (`CXProviderDelegate`) もこのタイムアウト状況に対処することをオーバーライドする必要があります。

### <a name="system-restrictions"></a>システム上の制限

IOS 10 VOIP アプリを実行している iOS デバイスの現在の状態に基づいて、特定のシステム制限を適用可能性があります。

たとえば場合、システムによって着信 VOIP 呼び出しを制限することができます。

1. 相手の呼び出しは、ユーザーのブロックされた呼び出し元のリストにです。
2. ユーザーの iOS デバイスは、Do Not 応答モードです。

VOIP 呼び出しがこのような状況のいずれかが制限されている場合は、それを処理する次のコードを使用します。

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

CallKit は、iOS 10 VOIP アプリはライブの VOIP 呼び出し中に必要なオーディオ リソースを処理するためのいくつかの利点を提供します。 最大のメリットの 1 つは、アプリのオーディオ セッションは、高度な優先順位で iOS 10 を実行しているときにします。 これは組み込まれた電話と同じ優先度レベルであり、VOIP アプリのオーディオのセッションを中断して他の実行中のアプリを防ぐ FaceTime アプリとこの強化された優先度レベル。

また、CallKit では、他のパフォーマンスを向上したり、ユーザー設定、およびデバイスの状態に基づくライブの呼び出し中に特定の出力デバイスに VOIP オーディオをインテリジェントにルーティングできるオーディオ ルーティング ヒントへのアクセスがあります。 たとえば、bluetooth ヘッドホン、CarPlay のライブ接続またはユーザー補助の設定などの接続されているデバイスに基づいています。

一般的な VOIP のライフ サイクル中に CallKit を使用して呼び出す、アプリが CallKit が提供するオーディオ ストリームを構成する必要があります。 次の例を参照してください。

[![](callkit-images/callkit09.png "呼び出し操作のシーケンスを開始")](callkit-images/callkit09.png#lightbox)

1. 呼び出しの開始アクションは、着信呼び出しに応答するようにアプリケーションによって受信されます。
2. 構成が必要になりますこのアクションは、アプリによって実現は、前にその`AVAudioSession`です。
3. アプリは、アクションが満たされたことをシステムに通知します。
4. 呼び出しが接続すると、CallKit が提供には、優先度の高い`AVAudioSession`アプリを要求した構成と一致します。 アプリで通知されます、`DidActivateAudioSession`のメソッド、`CXProviderDelegate`です。

## <a name="working-with-call-directory-extensions"></a>呼び出しのディレクトリ拡張を機能を使用

CallKit、使用するときに_ディレクトリ拡張機能を呼び出す_ブロックされている電話番号を追加して、iOS デバイスで連絡先アプリで連絡先を特定の VOIP アプリに固有の番号を識別する方法を提供します。

### <a name="implementing-a-call-directory-extension"></a>呼び出しディレクトリ拡張機能の実装

Xamarin.iOS アプリで呼び出すディレクトリ拡張機能を実装するには、次の操作を行います。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Visual studio for mac アプリのソリューションを開く
2. ソリューション名を右クリックし、**ソリューション エクスプ ローラー**選択**追加** > **新しいプロジェクトの追加**です。
3. 選択**iOS** > **拡張** > **ディレクトリ拡張機能の呼び出し** をクリックし、 **次へ**ボタン。 

    [![](callkit-images/calldir01.png "新しい呼び出しディレクトリ拡張機能の作成")](callkit-images/calldir01.png#lightbox)
4. 入力、**名前**拡張機能とクリック、 **[次へ]**ボタン。 

    [![](callkit-images/calldir02.png "拡張機能の名前を入力します。")](callkit-images/calldir02.png#lightbox)
5. 調整、**プロジェクト名**や**ソリューション名**要求されて、をクリックして、**作成**ボタン。 

    [![](callkit-images/calldir03.png "プロジェクトの作成")](callkit-images/calldir03.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Visual Studio でアプリのソリューションを開きます。
2. ソリューション名を右クリックし、**ソリューション エクスプ ローラー**選択**追加** > **新しいプロジェクトの追加**です。
3. 選択**iOS** > **拡張** > **ディレクトリ拡張機能の呼び出し** をクリックし、 **次へ**ボタン。 

    [![](callkit-images/calldir01w.png "新しい呼び出しディレクトリ拡張機能の作成")](callkit-images/calldir01.png#lightbox)
4. 入力してください、**名前**をクリックして、拡張機能の**OK**ボタン

-----


これは追加、`CallDirectoryHandler.cs`を次のようにプロジェクトにクラス。

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

`BeginRequest`呼び出すディレクトリ ハンドラーのメソッドが必要な機能を提供するように変更する必要があります。 上記のサンプルの場合、VOIP アプリの連絡先データベースでブロックされていると使用可能な数値の一覧の設定が試行されます。 何らかの理由で失敗を要求するか場合は、作成、`NSError`に、エラーについて説明し、渡すこと、`CancelRequest`のメソッド、`CXCallDirectoryExtensionContext`クラスです。

設定ブロックされている数字を使用する、`AddBlockingEntry`のメソッド、`CXCallDirectoryExtensionContext`クラスです。 メソッドに渡された数値_必要があります_数値の昇順であります。 最適なパフォーマンスとメモリ使用量の多くの電話番号があるときに特定の時点で番号のサブセットを読み込むと、各バッチに読み込まれる数字の中に割り当てられたオブジェクトを解放する場合のプールを使用してのみを検討します。

連絡先アプリ、連絡先の電話番号、VOIP アプリに正常に通知を使用して、`AddIdentificationEntry`のメソッド、`CXCallDirectoryExtensionContext`クラスし、番号と識別するラベルの両方を指定します。 メソッドに渡された番号、_必要があります_数値の昇順であります。 最適なパフォーマンスとメモリ使用量の多くの電話番号があるときに特定の時点で番号のサブセットを読み込むと、各バッチに読み込まれる数字の中に割り当てられたオブジェクトを解放する場合のプールを使用してのみを検討します。


## <a name="summary"></a>まとめ

この記事は新しい CallKit API その Apple iOS 10 と Xamarin.iOS VOIP アプリで実装する方法でリリースをについて説明します。 示されて CallKit が iOS システムに統合するアプリをできるようにする方法、(電話) などの組み込みアプリを使用して機能パリティを提供する方法、および全体にわたって Siri の相互作用を使用して、および via し、ホーム画面は、ロックなどの場所で iOS アプリの可視性が増加する方法連絡先アプリ。



## <a name="related-links"></a>関連リンク

- [iOS 10 サンプル](https://developer.xamarin.com/samples/ios/iOS10/)
