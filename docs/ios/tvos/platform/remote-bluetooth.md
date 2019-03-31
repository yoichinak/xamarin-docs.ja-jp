---
title: Siri のリモートおよび Xamarin で tvOS の Bluetooth コント ローラー
description: この記事では、Siri のリモートおよびゲーム コント ローラーと、Xamarin で記述された tvOS アプリで Bluetooth を使用する方法について説明します。
ms.prod: xamarin
ms.assetid: BDB9894A-236B-424B-9032-ACD12A6C5720
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 79022f7a454ea423fa3112a4c4ade2bcd471fbb8
ms.sourcegitcommit: 946ce514fd6575aa6b93ff24181e02a60b24b106
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/30/2019
ms.locfileid: "58677951"
---
# <a name="siri-remote-and-bluetooth-controllers-for-tvos-in-xamarin"></a>Siri のリモートおよび Xamarin で tvOS の Bluetooth コント ローラー

Xamarin.tvOS アプリのユーザーがいないするインターフェイスとの対話の直接として iOS の場所をタップしたいないデバイスの画面で、イメージが直接からルームを使用して、全体では、 [Siri のリモート](#The-Siri-Remote)します。

アプリがゲームの場合は、することができます必要に応じて行われるため、サード パーティのサポートの iOS ビルド (MFI) [Bluetooth ゲーム コント ローラー](#Bluetooth-Game-Controllers)もアプリでします。

[![](remote-bluetooth-images/intro01.png "Bluetooth のリモートおよびゲーム コント ローラー")](remote-bluetooth-images/intro01.png#lightbox)

この記事で説明します、 [Siri のリモート](#The-Siri-Remote)、[画面のタッチ ジェスチャ](#Touch-Surface-Gestures)と[Siri のリモート ボタン](#Siri-Remote-Buttons)しを使用して操作する方法を示しています[ジェスチャと。ストーリー ボード](#Gestures-and-Storyboards)、[ジェスチャとコード](#Gestures-and-Code)と[低レベルのイベント処理](#Low-Level-Event-Handling)します。 最後に、について説明しています[ゲーム コント ローラーの操作](#Working-with-Game-Controllers)Xamarin.tvOS アプリ。

<a name="The-Siri-Remote" />

## <a name="the-siri-remote"></a>Siri のリモート

ユーザーは、Apple TV と Xamarin.tvOS アプリでは、対話を主な方法は、含まれる Siri のリモートです。 Apple では、ソファと、部屋テレビ画面に表示される Apple TV のユーザー インターフェイス上のユーザーの間の距離を埋めるには、リモコンが設計されています。

TvOS アプリの開発者の課題は、Siri のリモートのタッチ画面、加速度計、ジャイロスコープなどがあります、ボタンを利用する簡単な使いやすく、視覚的に説得力のあるユーザー インターフェイスの作成です。

[![](remote-bluetooth-images/remote01.png "Siri のリモート")](remote-bluetooth-images/remote01.png#lightbox)

Siri のリモートでは、次の機能と tvOS アプリ内の予想される使用法があります。

|機能|一般的なアプリの使用状況|ゲーム アプリの使用状況|
|---|---|---|
|**タッチ画面**<br />移動し、キーを押して選択し、コンテキスト メニューの保持にスワイプします。|**タップ/スワイプ**<br />フォーカスを設定できる項目間のナビゲーションを UI。<br /><br />**をクリックします。**<br />選択 (フォーカス設定) の項目をアクティブにします。|**タップ/スワイプ**<br />ゲームの設計に依存しの端をタップして、パッドとして使用できます。<br /><br />**をクリックします。**<br />プライマリ ボタンの機能を実行します。|
|**Menu**<br />前の画面またはメニューに戻るには、このキーを押します。|前の画面に戻り、メイン アプリの画面を Apple TV ホーム画面に終了しました。|一時停止し、前の画面に戻り値と、アプリのメイン画面から Apple TV ホーム画面を終了するときに、ゲームプレイを再開します。|
|**Siri/検索**<br />Siri の国で押し音声コントロール、その他の国、検索画面が表示されます。|N/A|N/A|
|**再生/一時停止**<br />再生しメディアを一時停止、またはアプリで第 2 の機能を提供します。|メディアの再生および一時停止/再開の再生を開始します。|セカンダリのボタンの機能を実行するか、紹介ビデオをスキップします (場合は存在します)。|
|**Home**<br />ホーム画面に戻るキーを押して、実行中のアプリを表示、プレス アンド ホールド デバイスがスリープ状態にする をダブルクリックします。|N/A|N/A|
|**ボリューム**<br />コントロールには、オーディオ/ビデオ機器のボリュームが接続されています。|N/A|N/A|

<a name="Touch-Surface-Gestures" />

## <a name="touch-surface-gestures"></a>タッチ ジェスチャの画面

Siri のリモートのタッチ画面は、さまざまな Xamarin.tvOS アプリに対応できる、1 本指のジェスチャを検出できません。

|スワイプ|ここを|タップします|
|---|---|---|
|![](remote-bluetooth-images/Gesture01.png)|![](remote-bluetooth-images/Gesture02.png)|![](remote-bluetooth-images/Gesture03.png)|
|画面上の UI 要素の間での選択 (フォーカス) を移動します (、上下左右右)。 慣性による処理を使用して迅速にコンテンツの大規模なリストをスクロールする方向のスワイプ操作を使用できます。|選択 (フォーカス設定) 項目をアクティブ化またはゲームでは、主ボタンのように機能します。 クリックし、保持するには、コンテキスト メニューまたはセカンダリの関数がアクティブ化できます。|軽く、タッチの画面の端をタップして、パッド、上、下、左、またはタップ領域権利に応じて、フォーカスを移動の方向ボタンと同様します。 非表示のコントロールを表示するアプリによってを使用できます。|

Apple では、画面のタッチ ジェスチャを操作するための次の推奨事項を提供します。

* **クリックやタップ区別**-ユーザーが意図的なアクションは、クリックし、選択、アクティブ化、およびゲームのプライマリ ボタンに適しています。 タップは、微妙な控えめ Siri のリモートがその手の形で保持している多くの場合と、ユーザーと簡単に、タップ イベントを誤ってアクティブ化のため、使用する必要があります。
* **標準のジェスチャを再定義しない**-ユーザーが特定のジェスチャが特定のアクションを実行するという期待を持ってアプリで意味やこれらのジェスチャの機能を再定義しないでください。 1 つの例外は、アクティブなゲーム プレイ中にゲーム アプリです。
* **新しいジェスチャ慎重に定義**-ユーザー、特定のジェスチャが特定のアクションを実行する予測しています。 標準アクションを実行するカスタム ジェスチャを定義するを避ける必要があります。 ここでも、ゲームは最も一般的な例外、ゲームに、カスタム ジェスチャが楽しく、没入型のプレイを追加する場所。
* **該当する場合はパッド タップに応答**- 軽く、タッチ画面の端は react など、ゲーム コント ローラーのフォーカスを移動または、方向パッド下、左または右上隅の順にタップします。 該当する場合は、アプリやゲームでこれらのジェスチャに応答する必要があります。

<a name="Siri-Remote-Buttons" />

## <a name="siri-remote-buttons"></a>Siri のリモート ボタン

タッチ画面でのジェスチャだけでなく、アプリは、クリックして、タッチ画面または再生/一時停止 ボタンを押すと、ユーザーに対応できます。 ゲーム コント ローラー フレームワークを使用して Siri のリモートにアクセスする場合は、メニュー ボタンが押されたも検出できます。 

ジェスチャ レコグナイザーを使用して、標準のメニュー ボタンを押すようを検出できるさらに、`UIKit`要素。 押されたメニュー ボタンをインターセプトする場合は、現在のビューとビュー コント ローラーを閉じるを担当して 1 つ前に戻るします。

> [!IMPORTANT]
> 必要があります**常に**リモコンの再生/一時停止ボタンに関数を割り当てます。 非機能的なボタンを持つことが、アプリをエンドユーザーに表示できます。 このボタンの有効な関数を持っていない場合は、主ボタン (タッチ画面をクリック) と同じ機能を割り当てます。

<a name="Gestures-and-Storyboards" />

## <a name="gestures-and-storyboards"></a>ジェスチャとストーリー ボード

Siri のリモート Xamarin.tvOS アプリで使用する最も簡単な方法では、ジェスチャ レコグナイザーをインターフェイス デザイナー内でビューを追加します。

ジェスチャ認識エンジンを追加するには、次の操作を行います。

1. **ソリューション エクスプ ローラー**、ダブルクリックして、`Main.storyboard`ファイルを開き、インターフェイス デザイナーを編集用に開きます。
2. ドラッグ、**タップ ジェスチャ レコグナイザー**から、**ライブラリ**し、ビューにドロップします。 

    [![](remote-bluetooth-images/storyboard01.png "タップ ジェスチャ認識エンジン")](remote-bluetooth-images/storyboard01.png#lightbox)
3. 確認**選択**で、**ボタン**のセクション、**属性インスペクター**: 

    [![](remote-bluetooth-images/storyboard02.png "選択を確認してください。")](remote-bluetooth-images/storyboard02.png#lightbox)
4. **選択**ジェスチャがユーザーのクリックしてに応答することを意味、**タッチ画面**Siri のリモートにします。 対応のオプションがあることも、**メニュー**、**再生/一時停止**、**を**、**ダウン**、**左**と**右**ボタン。
5. 次に、接続、**アクション**から、**タップ ジェスチャ レコグナイザー** 、呼び出す`TouchSurfaceClicked`: 

    [![](remote-bluetooth-images/storyboard03.png "タップ ジェスチャ認識エンジンからアクション")](remote-bluetooth-images/storyboard03.png#lightbox)
6. 変更を保存し、Visual studio for mac を返す

ビュー コント ローラーの編集 (例`FirstViewController.cs`) ファイルを開き、トリガーされてジェスチャを処理するために次のコードを追加します。

```csharp
using System;
using UIKit;

namespace tvRemote
{
    public partial class FirstViewController : UIViewController
    {
        ...

        #region Custom Actions
        partial void TouchSurfaceClicked (Foundation.NSObject sender) {
            // Handle click here
            ...
        }
        #endregion
    }
}
```

ストーリー ボードの操作方法の詳細についてを参照してください、[はじめての tvOS クイック スタート ガイド](~/ios/tvos/get-started/hello-tvos.md)します。 具体的には、[ユーザー インターフェイスを作成する](~/ios/tvos/get-started/hello-tvos.md#Creating-the-User-Interface)と[outlet と action を使用してコードを記述](~/ios/tvos/get-started/hello-tvos.md#Writing-the-Code)セクション。

<a name="Gestures-and-Code" />

## <a name="gestures-and-code"></a>ジェスチャとコード

必要に応じて、作成ジェスチャで直接C#コードし、ユーザー インターフェイス内のビューに追加します。 たとえば、一連のスワイプ ジェスチャ レコグナイザーを追加するには、ビュー コント ローラーを編集し、次のコードを追加します。

```csharp
using System;
using UIKit;

namespace tvRemote
{
    public partial class SecondViewController : UIViewController
    {
        #region Constructors
        public SecondViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();    

            // Wire-up gestures
            var upGesture = new UISwipeGestureRecognizer (() => {
                RemoteView.ArrowPressed = "Up";
                ButtonLabel.Text = "Swiped Up";
            }) {
                Direction = UISwipeGestureRecognizerDirection.Up
            };
            this.View.AddGestureRecognizer (upGesture);

            var downGesture = new UISwipeGestureRecognizer (() => {
                RemoteView.ArrowPressed = "Down";
                ButtonLabel.Text = "Swiped Down";
            }) {
                Direction = UISwipeGestureRecognizerDirection.Down
            };
            this.View.AddGestureRecognizer (downGesture);

            var leftGesture = new UISwipeGestureRecognizer (() => {
                RemoteView.ArrowPressed = "Left";
                ButtonLabel.Text = "Swiped Left";
            }) {
                Direction = UISwipeGestureRecognizerDirection.Left
            };
            this.View.AddGestureRecognizer (leftGesture);

            var rightGesture = new UISwipeGestureRecognizer (() => {
                RemoteView.ArrowPressed = "Right";
                ButtonLabel.Text = "Swiped Right";
            }) {
                Direction = UISwipeGestureRecognizerDirection.Right
            };
            this.View.AddGestureRecognizer (rightGesture);
        }
        #endregion
    }
}
```

<a name="Low-Level-Event-Handling" />

## <a name="low-level-event-handling"></a>低レベルのイベント処理

基づくカスタム型を作成する場合は`UIKit`Xamarin.tvOS アプリで (たとえば`UIView`)、低レベルの処理を使用してボタンを押すを提供する機能もある`UIPress`イベント。 

A`UIPress`イベントがどのような tvos が、`UITouch`イベントが以外に、iOS が`UIPress`Siri リモコンのボタンに関する情報を押すか、Bluetooth デバイス (ゲーム コント ローラー) のような他の接続されたを返します。 `UIPress` イベントは、ボタンが押されると、状態 (開始、取り消し、変更または終了) について説明します。 

Bluetooth ゲーム コント ローラーなどのデバイスのアナログのボタンの`UIPress`も強制ボタンに適用されている量を返します。 `Type`のプロパティ、`UIPress`イベントを定義する物理的なボタンが変更された状態、残りのプロパティの中に発生した変更について説明します。

次のコードでは、低レベルの処理の例は`UIPress`イベントを`UIView`:

```csharp
using System;
using Foundation;
using UIKit;

namespace tvRemote
{
    public partial class EventView : UIView
    {
        #region Computed Properties
        public override bool CanBecomeFocused {
            get {
                return true;
            }
        }
        #endregion

        #region 
        public EventView (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void PressesBegan (NSSet<UIPress> presses, UIPressesEvent evt)
        {
            base.PressesBegan (presses, evt);

            foreach (UIPress press in presses) {
                // Was the Touch Surface clicked?
                if (press.Type == UIPressType.Select) {
                    BackgroundColor = UIColor.Red;
                }
            }
        }

        public override void PressesCancelled (NSSet<UIPress> presses, UIPressesEvent evt)
        {
            base.PressesCancelled (presses, evt);

            foreach (UIPress press in presses) {
                // Was the Touch Surface clicked?
                if (press.Type == UIPressType.Select) {
                    BackgroundColor = UIColor.Clear;
                }
            }
        }

        public override void PressesChanged (NSSet<UIPress> presses, UIPressesEvent evt)
        {
            base.PressesChanged (presses, evt);
        }

        public override void PressesEnded (NSSet<UIPress> presses, UIPressesEvent evt)
        {
            base.PressesEnded (presses, evt);

            foreach (UIPress press in presses) {
                // Was the Touch Surface clicked?
                if (press.Type == UIPressType.Select) {
                    BackgroundColor = UIColor.Clear;
                }
            }
        }
        #endregion
    }
}
```

同様`UITouch`のいずれかを実装する必要がある場合は、イベント、`UIPress`イベントの上書きには、4 つすべてを実装する必要があります。

<a name="Bluetooth-Game-Controllers" />

## <a name="bluetooth-game-controllers"></a>Bluetooth のゲーム コント ローラー

Apple TV、サード パーティ、iOS の作成に付属する標準の Siri のリモートだけでなく (MFI) Bluetooth ゲーム コント ローラーを Apple TV と組み合わせて使用と Xamarin.tvOS アプリを制御するために使用することができます。

[![](remote-bluetooth-images/game01.png "Bluetooth のゲーム コント ローラー")](remote-bluetooth-images/game01.png#lightbox)

ゲーム コント ローラーは、ゲームプレイを強化し、ゲームで immersion 感の提供に使用できます。 また、使用は、リモートとコント ローラー間で切り替える必要はありませんので、Apple TV の標準のインターフェイスを制御するためも使用できます。

> [!IMPORTANT]
> Bluetooth のゲーム コント ローラーには、エンドユーザーを使用する省略可能で購入した、アプリは、いずれかを購入するユーザーを強制できません。 アプリは、ゲーム コント ローラーをサポートする場合は、Siri のリモートもサポート ゲームが Apple TV のすべてのユーザーが使用できるようにする必要があります。

ゲーム コント ローラーには、次の機能と tvOS アプリ内の予想される使用法があります。

|機能|一般的なアプリの使用状況|ゲーム アプリの使用状況|
|---|---|---|
|**D-Pad**|UI 要素 (フォーカス) を移動します。|ゲームに依存します。|
|**A**|選択 (フォーカス設定) 項目をアクティブにします。|プライマリ ボタンの機能を実行し、ダイアログの動作を確認します。|
|**B**|前の画面を返すか、アプリのメイン画面の場合、ホーム画面を終了します。|セカンダリのボタンの機能を実行しますか、前の画面に返します。|
|**X**|メディアの再生や一時停止/再開の再生を開始します。|ゲームに依存します。|
|**Y**|N/A|ゲームに依存します。|
|**Menu**|前の画面を返すか、アプリのメイン画面の場合、ホーム画面を終了します。|一時停止/再開ゲームプレイを前の画面に戻りますかホーム画面を終了するアプリのメイン画面の場合。|
|**左肩ボタン**|左に移動します。|ゲームに依存します。|
|**左のトリガー**|左に移動します。|ゲームに依存します。|
|**適切なショルダー ボタン**|右に移動します。|ゲームに依存します。|
|**適切なトリガー**|右に移動します|ゲームに依存します。|
|**左のサムスティック**|UI 要素 (フォーカス) を移動します。|ゲームに依存します。|
|**右スティック**|N/A|ゲームに依存します。|

Apple では、ゲーム コント ローラーを操作するための次の推奨事項を提供します。

- **ゲーム コント ローラーの接続を確認します。** -tvOS アプリを開始およびエンドユーザーが、いつでも停止できます。 常にアプリの起動時または起動状態の時間にゲーム コント ローラーの有無を確認し、必要に応じてアクションを実行する必要があります。
- **Siri のリモートおよびゲーム コント ローラーの両方で、アプリの動作を確認します。** -ユーザー、アプリを使用するには、Siri のリモートおよびゲーム コント ローラー間で切り替えるには必要ありません。 どちらの種類すべて簡単に移動し、期待どおりに動作を確保するコント ローラーの多くの場合、アプリをテストします。
- **バックアップする方法を提供**-いつでも、前の画面に戻ってメニュー ボタンを押す必要があります。 ユーザーは、アプリのメイン画面では場合、メニュー ボタン Apple TV ホーム画面には戻す必要があります。 、ゲームプレイ中には、メニュー ボタンがゲームプレイを一時停止/再開またはメイン メニューに戻ることをユーザーに提供、アラートを表示する必要があります。

<a name="Working-with-Game-Controllers" />

## <a name="working-with-game-controllers"></a>ゲーム コント ローラーの操作

前述のように、だけでなく、標準ユーザーであり、Apple TV で Siri リモートに付属していることができます必要に応じて接続サード パーティ、iOS (MFI) Bluetooth ゲーム コントローラを行ったし、Xamarin.tvOS アプリを制御します。

アプリにコント ローラーの低レベルの入力が必要な場合は、Apple を使用できます[ゲーム コント ローラー フレームワーク](https://developer.apple.com/library/prerelease/tvos/documentation/ServicesDiscovery/Conceptual/GameControllerPG/Introduction/Introduction.html#//apple_ref/doc/uid/TP40013276)tvOS の次の変更が含まれています。

- マイクロ ゲーム コント ローラー プロファイル (`GCMicroGamepad`) Siri のリモートの対象に追加されました。
- 新しい`GCEventViewController`クラスは、アプリ、ゲーム コント ローラー イベントのルーティングに使用できます。 参照してください、[ゲーム コント ローラーの入力を決定する](#determining-game-controller-input)詳細については後述します。

<a name="Game-Controller-Support-Requirements" />

### <a name="game-controller-support-requirements"></a>ゲーム コント ローラーのサポート要件

Apple では、Xamarin.tvOS アプリは、ゲーム コント ローラーをサポートしている場合に満たす必要があるいくつかの特定の要件があります。

- **Siri のリモートをサポートする必要があります**-Siri のリモートを常にサポートする必要があります。 ゲームことはできません、サード パーティのプレイするゲーム コント ローラーが必要です。
- **拡張コントロールのレイアウトをサポートする必要があります**-すべての tvOS ゲーム コント ローラーは非 formfitting コント ローラーを拡張します。
- **ゲーム コント ローラーのスタンドアロンと Playable をする必要があります**-そのゲーム コント ローラーのみを目的と再生する必要がありますが、アプリは、ゲーム、拡張コント ローラーをサポートする場合。
- **再生/一時停止 ボタンをサポートする必要があります**-、ゲームプレイ中に、ユーザーが再生/一時停止 ボタンを押した場合する必要がありますゲームを一時停止/再開する機能をユーザーに提供する、アラートを表示またはメイン メニューに戻ります。

<a name="Enabling-Game-Controller-Support" />

### <a name="enabling-game-controller-support"></a>ゲーム コント ローラーのサポートを有効にします。

Xamarin.tvOS アプリでのゲーム コント ローラーのサポートを有効にする をダブルクリックします、`Info.plist`ファイル、**ソリューション エクスプ ローラー**編集用に開きます。

[![](remote-bluetooth-images/game02.png "Info.plist エディター")](remote-bluetooth-images/game02.png#lightbox)

下、**ゲーム コント ローラー**セクションで、チェック ボックスをオンして**ゲーム コント ローラーを有効にする**、すべてのアプリでサポートされるゲーム コント ローラーの種類を確認します。

<a name="Using-the-Siri-Remote-as-a-Game-Controller" />

### <a name="using-the-siri-remote-as-a-game-controller"></a>ゲーム コント ローラーとして Siri のリモートを使用します。

Apple TV に付属する Siri のリモートは、限られたゲーム コント ローラーとして使用できます。 他のゲーム コント ローラーのように表示されます、ゲーム コント ローラーのフレームワークとして、`GCController`オブジェクトとサポート、`GCMotion`と`GCMicroGamepad`プロファイル。

Siri のリモートでは、ゲーム コント ローラーとして使用されている場合、次の特徴があります。

- タッチ画面は、アナログの入力データを提供する方向パッドとして使用できます。
- リモートを縦または横方向で使用でき、アプリは、プロファイル オブジェクトが入力データを自動的に反転する必要があるかどうかを決定します。
- ボタンを押すと同様、タッチ画面をクリックすると**A**ゲーム コント ローラーにします。
- 再生/一時停止ボタンは、ボタンのように機能します**X**ゲーム コント ローラーにします。
- メニュー ボタンには、一時停止/再開のゲームプレイまたはメイン メニューに戻る機能をユーザーに提供、アラートを表示する必要があります。

<a name="Summary" />

### <a name="determining-game-controller-input"></a>ゲーム コント ローラーの入力を決定します。

TvOS が低レベルのすべてのイベントを高度な配信を処理するタッチ イベントと並行して、ゲーム コント ローラーのイベントを受け取ることが、iOS とは異なり`UIKit`イベント。 その結果、ゲーム コント ローラーの低レベルのイベントへのアクセスを必要がある場合は、する必要がありますをオフに`UIKit`の既定の動作。

TvOS を使用する必要がある直接ゲーム コント ローラーの入力を処理するときに、 `GCEventViewController` (またはそのサブクラス) のユーザー インターフェイスのゲームを表示します。 たびに、`GCEventViewController`は、*最初レスポンダー*、ゲーム コント ローラーの入力をキャプチャし、ゲーム コント ローラーのフレームワークを使用してアプリケーションに配信します。

使用することができます、`UserInteractionEnabled`のプロパティ、`GCEventViewController`クラスをイベントの処理方法と処理方法を切り替えます。

ゲーム コント ローラーのサポートを実装する方法の詳細については、Apple を参照してください[ゲーム コント ローラーの操作](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/WorkingwithGameControllers.html)セクション、[の tvOS アプリのプログラミング ガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/index.html)と[ゲーム コント ローラープログラミング ガイド](https://developer.apple.com/library/prerelease/tvos/documentation/ServicesDiscovery/Conceptual/GameControllerPG/Introduction/Introduction.html)します。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、Apple TV、画面のタッチ ジェスチャと Siri のリモートのボタンに付属する新しい Siri のリモートをについて説明します。 次に、ジェスチャとストーリー ボード、ジェスチャとコード、および低レベルのイベントの操作を説明しました。 最後に、ゲーム コント ローラーの操作について説明している場合。



## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマン インターフェイス ガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS 用のアプリのプログラミング ガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
