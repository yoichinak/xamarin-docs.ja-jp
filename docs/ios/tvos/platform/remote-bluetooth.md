---
title: Siri のリモート コンピューターと Bluetooth コント ローラー
description: この記事では、Xamarin.tvOS アプリで新しい Siri リモート コンピューターと Bluetooth ゲーム コント ローラーのサポートについて説明します。
ms.prod: xamarin
ms.assetid: BDB9894A-236B-424B-9032-ACD12A6C5720
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 5b5893278acad999efd94c89f1ca923100f5cf7c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="siri-remote-and-bluetooth-controllers"></a>Siri のリモート コンピューターと Bluetooth コント ローラー

_この記事では、Xamarin.tvOS アプリで新しい Siri リモート コンピューターと Bluetooth ゲーム コント ローラーのサポートについて説明します。_


Xamarin.tvOS アプリのユーザーがいないするインターフェイスとの対話の直接として ios をタップしたいないデバイスの画面上のイメージが直接からルームを使用して、全体では、 [Siri リモート](#The-Siri-Remote)です。

アプリがゲームの場合は、することができます必要に応じて作成用のサード パーティのサポートの iOS ビルド (MFI) [Bluetooth ゲーム コント ローラー](#Bluetooth-Game-Controllers)もアプリにします。

[![](remote-bluetooth-images/intro01.png "Bluetooth Remote、およびゲーム コント ローラー")](remote-bluetooth-images/intro01.png#lightbox)

この記事の内容について説明します、 [Siri リモート](#The-Siri-Remote)、[タッチ画面ジェスチャ](#Touch-Surface-Gestures)と[Siri リモート ボタン](#Siri-Remote-Buttons)経由でそれらを使用する方法を示しています[ジェスチャとストーリー ボード](#Gestures-and-Storyboards)、[ジェスチャ、およびコード](#Gestures-and-Code)と[低レベルのイベント処理](#Low-Level-Event-Handling)です。 最後に、についても説明[ゲーム コントローラを備えた作業](#Working-with-Game-Controllers)Xamarin.tvOS アプリでします。

<a name="The-Siri-Remote" />

## <a name="the-siri-remote"></a>Siri リモート

ユーザーは Apple TV および Xamarin.tvOS アプリと対話するでを主な方法は、含まれる Siri リモートです。 Apple には、ソファおよびテレビ画面で、部屋の向こう側表示された Apple TV のユーザー インターフェイスに座ってユーザー間の距離を埋めるには、リモコンが設計されています。

TvOS アプリ開発者として、問題点が Siri リモートのタッチ画面、加速度計、ジャイロスコープおよびボタンを利用したクイック、使いやすいおよび視覚的に説得力のあるユーザー インターフェイスの作成。

[![](remote-bluetooth-images/remote01.png "Siri リモート")](remote-bluetooth-images/remote01.png#lightbox)

Siri リモコンには、次の機能と、tvOS アプリ内で予想される使用法があります。

|機能|一般的なアプリの使用状況|ゲームのアプリの使用状況|
|---|---|---|
|**タッチ画面**<br />移動では、キーを押して選択し、コンテキスト メニューを保持する方向にスワイプします。|**Tap/スワイプ**<br />フォーカス可能な項目間のナビゲーションを UI。<br /><br />**をクリックします。**<br />選択 (フォーカス設定) 項目をアクティブにします。|**Tap/スワイプ**<br />ゲームの設計に依存していて、端の順にタップして方向パッドとして使用できます。<br /><br />**をクリックします。**<br />プライマリ ボタンの機能を実行します。|
|**Menu**<br />前の画面またはメニューに戻るにはキーを押します。|前の画面に返し、メインのアプリの画面から Apple テレビ ホーム画面を終了します。|一時停止し、前の画面に戻ると終了します。 Apple テレビ ホーム画面にメインのアプリの画面から、ゲームを再開します。|
|**Siri/検索**<br />Siri の国で押し音声コントロールは、その他の国、検索画面が表示されます。|N/A|N/A|
|**再生/一時停止**<br />再生しメディアを一時停止またはアプリ内のセカンダリ関数を提供します。|メディアの再生および一時停止/再開の再生を開始します。|副ボタンの機能を実行するか、または手順の概要ビデオをスキップ (場合が存在する)。|
|**Home**<br />ホーム画面に戻るにはキーを押して、実行中のアプリケーションを表示、プレス アンド ホールド デバイスがスリープ状態にする をダブルクリックします。|N/A|N/A|
|**ボリューム**<br />コントロールは、オーディオ/ビデオ機器のボリュームをアタッチします。|N/A|N/A|

<a name="Touch-Surface-Gestures" />

## <a name="touch-surface-gestures"></a>タッチ画面ジェスチャ

Siri リモートのタッチ画面では、さまざまな Xamarin.tvOS アプリ内に応答できる 1 本指のジェスチャを検出するためにできます。

|スワイプ|クリック|タップ|
|---|---|---|
|![](remote-bluetooth-images/Gesture01.png)|![](remote-bluetooth-images/Gesture02.png)|![](remote-bluetooth-images/Gesture03.png)|
|画面上の UI 要素の間での選択 (フォーカス) に移動 (up、下向き、左向き右)。 コンテンツを使用した迅速慣性の大規模なリストをスクロールするスワイプを使用できます。|選択 (フォーカス設定) 項目をアクティブ化またはゲームでは、主ボタンと同様に動作します。 クリックし、保持するには、コンテキスト メニューまたはセカンダリ関数がアクティブ化できます。|方向パッド、上、下、左、またはタップ領域によっては右にフォーカスを移動上方向ボタンと同様に機能軽く、タッチ画面の端をタップします。 非表示コントロールを表示するためには、アプリによってを使用できます。|

Apple では、画面をタッチ ジェスチャを使用して操作するための次の提案を提供します。

* **クリックしてタップ区別**-をクリックすると、ユーザーが意図的なアクションは、選択、アクティブ化、およびゲームの主ボタンに適しています。 タップは、微弱慎重に使用、ユーザーが手の形で Siri リモート保持している多くの場合、誤ってアクティブ化できますが、Tap イベント簡単にします。
* **標準のジェスチャを再定義しない**-ユーザーが特定のジェスチャでは、特定の操作を実行するという期待アプリでこれらのジェスチャの機能または意味を再定義しないでください。 1 つの例外は、アクティブなプレイ中にゲームのアプリです。
* **新しいジェスチャなので、慎重に定義**-ユーザーが特定のジェスチャでは、特定の操作を実行するという期待がもう一度、します。 標準的なアクションを実行するカスタム ジェスチャを定義するを避ける必要があります。 もう一度、ゲームは、最も一般的な例外、ゲームに、カスタム ジェスチャが楽しい、実体験の再生を追加するためです。
* **必要に応じて、方向パッド タップ応答**タッチ画面の端を左、下にゲーム コント ローラーのフォーカスを移動または、方向パッドと同じように対処または右上隅に軽くタップ - です。 必要に応じて、アプリやゲームのこれらのジェスチャに応答する必要があります。

<a name="Siri-Remote-Buttons" />

## <a name="siri-remote-buttons"></a>Siri のリモコン ボタン

タッチ画面でジェスチャをに加えて、アプリはタッチ画面をクリックするか、[再生/一時停止] ボタンを押すと、ユーザーに応答できます。 ゲーム コント ローラーのフレームワークを使用して、Siri リモートにアクセスする場合は、メニュー ボタンが押されるも検出できます。 

ジェスチャ レコグナイザーを使用して、標準のメニュー ボタンを押すを検出できるさらに、`UIKit`要素。 メニュー ボタンが押されるをインターセプトする場合、現在のビューとビューのコント ローラーの終了を行うことをし、1 つ前に戻ります。

> [!IMPORTANT]
> 行う必要があります**常に**リモコンの再生/一時停止ボタンに関数を割り当てます。 非機能的 ボタンを持つと、エンドユーザーに壊れた参照、アプリがなることができます。 このボタンの有効な関数をお持ちでない場合は、主ボタン (タッチ画面をクリックする) と同じ機能を割り当てます。




<a name="Gestures-and-Storyboards" />

## <a name="gestures-and-storyboards"></a>ジェスチャとストーリー ボード

Xamarin.tvOS アプリで Siri リモコンを使用する最も簡単な方法では、ジェスチャ レコグナイザーをインターフェイス デザイナーのビューに追加します。

ジェスチャ レコグナイザーを追加するには、次の操作を行います。

1. **ソリューション エクスプ ローラー**をダブルクリックして、`Main.storyboard`ファイルし、ファイルを開いてインターフェイス デザイナーを編集します。
2. ドラッグ、**タップ ジェスチャ レコグナイザー**から、**ライブラリ**し、ビュー上にドロップします。 

    [![](remote-bluetooth-images/storyboard01.png "タップ ジェスチャ レコグナイザー")](remote-bluetooth-images/storyboard01.png#lightbox)
3. 確認**選択**で、**ボタン**のセクションで、**属性インスペクター**: 

    [![](remote-bluetooth-images/storyboard02.png "選択を確認します。")](remote-bluetooth-images/storyboard02.png#lightbox)
4. **選択**ジェスチャがユーザーのクリックしてに応答することを意味、**タッチ画面**Siri リモートです。 応答のオプションもある、**メニュー**、**再生/一時停止**、**を**、**ダウン**、**左**と**右**ボタン。
5. 次に、ネットワーク上で、**アクション**から、**タップ ジェスチャ レコグナイザー**および呼び出し`TouchSurfaceClicked`: 

    [![](remote-bluetooth-images/storyboard03.png "タップ ジェスチャ レコグナイザーからアクション")](remote-bluetooth-images/storyboard03.png#lightbox)
6. 変更を保存し、for mac を Visual Studio に戻る

ビュー、コント ローラーの編集 (例`FirstViewController.cs`) ファイルし、トリガーされています。 ジェスチャを処理する次のコードを追加します。

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

ストーリー ボードの使用の詳細についてを参照してください、[こんにちは、tvOS クイック スタート ガイド](~/ios/tvos/get-started/hello-tvos.md)です。 具体的には、[ユーザー インターフェイスの作成](~/ios/tvos/get-started/hello-tvos.md#Creating-the-User-Interface)と[コンセントとアクションを使用してコードを記述](~/ios/tvos/get-started/hello-tvos.md#Writing-the-Code)セクションです。

<a name="Gestures-and-Code" />

## <a name="gestures-and-code"></a>ジェスチャ、およびコード

必要に応じて、c# コードで直接ジェスチャを作成でき、ユーザー インターフェイスでのビューに追加することができます。 たとえば、スワイプ ジェスチャ レコグナイザーの系列を追加するには、ビュー、コント ローラーを編集し、次のコードを追加します。

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

基づくカスタム型を作成する場合は`UIKit`Xamarin.tvOS アプリで (たとえば`UIView`)、経由でのボタンを押すの低レベルの処理を提供する機能も必要がある`UIPress`イベント。 

A`UIPress`イベントがどのような tvOS には、`UITouch`イベントが、iOS を除くが`UIPress`Siri リモコンのボタンについての情報を押すか、他の Bluetooth デバイス (ゲーム コント ローラー) が接続されたを返します。 `UIPress` イベントは、ボタンが押されると、状態 (開始、取り消し、変更または終了) について説明します。 

アナログ ボタン Bluetooth ゲーム コント ローラーなどのデバイスに対して`UIPress`も強制ボタンに適用されている量を返します。 `Type`のプロパティ、`UIPress`イベントを定義する物理的なボタンが変更された状態、残りのプロパティの中に発生した変更について説明します。

次のコードが低レベルの処理の例を示します`UIPress`イベントを`UIView`:

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

同様に`UITouch`イベントのいずれかを実装する必要がある場合、`UIPress`イベントの上書き、4 つすべてを実装する必要があります。

<a name="Bluetooth-Game-Controllers" />

## <a name="bluetooth-game-controllers"></a>Bluetooth ゲーム コント ローラー

Apple TV、サード パーティ、iOS の作成に付属する標準的な Siri リモートに加えて (MFI) Bluetooth ゲーム コント ローラー Apple TV と組み合わせて使用して、Xamarin.tvOS アプリを制御するために使用します。

[![](remote-bluetooth-images/game01.png "Bluetooth ゲーム コント ローラー")](remote-bluetooth-images/game01.png#lightbox)

ゲームを拡張し、ゲームの世界の意味を提供するゲーム コント ローラーを使用できます。 また、使用は、リモート コンピューターと、コント ローラーの間で切り替える必要はありませんので、Apple TV の標準のインターフェイスを制御するも使用できます。

> [!IMPORTANT]
> Bluetooth ゲーム コント ローラーは、エンドユーザーに対して行うことが省略可能な購買、アプリは、いずれかを購入するユーザーを強制できません。 アプリは、ゲーム コント ローラーをサポートしている場合必要がありますもサポート Siri リモート ゲームは、Apple TV のすべてのユーザーによって使用できるようにします。

ゲーム コント ローラーには、次の機能と、tvOS アプリ内で予想される使用法があります。

|機能|一般的なアプリの使用状況|ゲームのアプリの使用状況|
|---|---|---|
|**D-Pad**|UI 要素 (フォーカス) を移動します。|ゲームに依存します。|
|**A**|選択 (フォーカス設定) 項目をアクティブにします。|主ボタンの機能を実行し、ダイアログの動作を確認します。|
|**B**|アプリのメイン画面の場合は、ホーム画面を終了するか、または前の画面に返します。|副ボタンの機能を実行するか、前の画面に返されます。|
|**X**|メディアの再生または一時停止/再開の再生を開始します。|ゲームに依存します。|
|**Y**|N/A|ゲームに依存します。|
|**Menu**|アプリのメイン画面の場合は、ホーム画面を終了するか、または前の画面に返します。|一時停止/再開前の画面に戻りますかホーム画面を終了するゲームをプレイ、アプリのメイン画面の場合。|
|**左側のそばのボタン**|左に移動します。|ゲームに依存します。|
|**左のトリガー**|左に移動します。|ゲームに依存します。|
|**右側のそばのボタン**|右に移動します。|ゲームに依存します。|
|**右側のトリガー**|右に移動します。|ゲームに依存します。|
|**左のサムスティック**|UI 要素 (フォーカス) を移動します。|ゲームに依存します。|
|**右スティック**|N/A|ゲームに依存します。|

Apple では、ゲーム コント ローラーを使用するための次の提案を提供します。

- **ゲーム コント ローラーの接続を確認**-tvOS アプリを起動し、エンドユーザーがいつでも停止できます。 常にアプリの起動または起動状態の時間にゲーム コント ローラーの有無を確認して、必要に応じてアクションを実行する必要があります。
- **Siri リモートおよびゲーム コント ローラーの両方で、アプリの動作を確認してください**-ユーザーがアプリを使用するには、Siri リモートおよびゲーム コント ローラーの間で切り替えるは必要ありません。 両方の種類すべて簡単に移動し、期待どおりに動作を確保するコント ローラーの多くの場合、アプリをテストします。
- **バックアップする方法を提供**-前の画面に常に返すメニュー ボタンを押す必要があります。 ユーザーは、メインのアプリ 画面では、メニュー ボタンはそれらに戻ります Apple テレビ ホーム画面です。 メニュー ボタンには、ゲーム、中にゲームを一時停止/再開、またはメイン メニューに戻るにことをユーザーに提供アラートが表示されます。

<a name="Working-with-Game-Controllers" />

## <a name="working-with-game-controllers"></a>ゲーム コント ローラーの使用

前述のように Siri でリモート接続付属している Apple TV、ユーザーはアタッチできます必要に応じて、標準以外にはサード パーティ、行われた iOS (MFI) Bluetooth ゲーム コント ローラーのし Xamarin.tvOS アプリを制御します。

Apple を使用できる場合は、アプリには、低レベルのコント ローラーの入力が必要な[ゲーム コント ローラー Framework](https://developer.apple.com/library/prerelease/tvos/documentation/ServicesDiscovery/Conceptual/GameControllerPG/Introduction/Introduction.html#//apple_ref/doc/uid/TP40013276) tvOS の次の変更は。

- マイクロ ゲーム コント ローラー プロファイル (`GCMicroGamepad`) Siri リモートの対象に追加されました。
- 新しい`GCEventViewController`クラスは、アプリを使って、ゲーム コント ローラー イベントのルーティングに使用できます。 参照してください、[ゲーム コント ローラーの入力を決定する](#Determining-Game-Controller-Input)詳細については、後述の「します。

<a name="Game-Controller-Support-Requirements" />

### <a name="game-controller-support-requirements"></a>ゲーム コント ローラーのサポート要件

Apple では、Xamarin.tvOS アプリは、ゲーム コント ローラーをサポートしている場合に満たす必要があるいくつかの特定の要件があります。

- **Siri リモコンをサポートする必要があります**-Siri リモートを常にサポートする必要があります。 ゲームには、サード パーティのゲーム コント ローラーを再生することはできませんが必要です。
- **拡張コントロールのレイアウトをサポートする必要があります**-すべて tvOS ゲーム コント ローラーは非 formfitting コント ローラーを拡張します。
- **ゲームは、スタンドアロンのコント ローラーと Playable をする必要があります**-そのゲーム コント ローラーでのみ再生する必要がありますが、アプリは、拡張のゲーム コント ローラーをサポートする場合。
- **再生/一時停止 ボタンをサポートする必要があります**-プレイ、中に再生/一時停止ボタンを押す場合ゲームを一時停止/再開する機能をユーザーに提供警告を表示するか、メイン メニューに戻る。

<a name="Enabling-Game-Controller-Support" />

### <a name="enabling-game-controller-support"></a>ゲーム コント ローラーのサポートを有効にします。

Xamarin.tvOS アプリでのゲーム コント ローラーのサポートを有効にするをダブルクリック、`Info.plist`ファイルで、**ソリューション エクスプ ローラー**ファイルを開いて編集します。

[![](remote-bluetooth-images/game02.png "Info.plist エディター")](remote-bluetooth-images/game02.png#lightbox)

下にある、**ゲーム コント ローラー**セクションで、チェック ボックスをオンして**ゲーム コント ローラーを有効にする**、すべてのアプリでサポートされるゲーム コント ローラーの種類を確認します。

<a name="Using-the-Siri-Remote-as-a-Game-Controller" />

### <a name="using-the-siri-remote-as-a-game-controller"></a>ゲーム コント ローラーとして Siri リモコンを使用します。

Apple TV に付属している Siri リモコンは、限られたゲーム コント ローラーとして使用できます。 他のゲーム コント ローラーのようにそれで表示、ゲーム コント ローラーとして Framework、`GCController`オブジェクトをサポートしており、`GCMotion`と`GCMicroGamepad`プロファイル。

Siri リモコンでは、ゲーム コント ローラーとして使用されている場合に次の特徴があります。

- タッチ画面は、アナログの入力データを提供する方向パッドとして使用できます。
- リモートを縦または横方向で使用できるし、アプリが、そのプロファイル オブジェクトが入力データを自動的に反転する必要があるかどうかを決定します。
- ボタンを押すと同様に、タッチ画面をクリックすると**A**ゲーム コント ローラーにします。
- ボタンと同様に、再生/一時停止ボタン**X**ゲーム コント ローラーにします。
- メニュー ボタンには、ゲームを一時停止/再開、またはメイン メニューに戻るにことをユーザーに提供アラートを表示する必要があります。

<a name="Summary" />

### <a name="determining-game-controller-input"></a>ゲーム コント ローラーの入力を決定します。

高度な配信をすべて低レベルのイベントを処理する tvOS タッチ イベントと並列でゲーム コント ローラーのイベントを受け取ることが、iOS とは異なり`UIKit`イベント。 その結果、ゲーム コント ローラーの低レベルのイベントにアクセスする場合は、する必要がありますをオフにする`UIKit`の既定の動作です。

TvOS を使用する必要があります。 直接ゲーム コント ローラーの入力を処理するときに、 `GCEventViewController` (またはサブクラス) のユーザー インターフェイスのゲームを表示します。 ときに、`GCEventViewController`は、*最初レスポンダー*、ゲーム コント ローラーの入力をキャプチャし、ゲーム コント ローラーのフレームワークを使用してアプリに配信します。

使用することができます、`UserInteractionEnabled`のプロパティ、`GCEventViewController`イベントの処理方法と処理方法を切り替えるクラスです。

ゲーム コント ローラーのサポートを実装する方法の詳細については、Apple を参照してください[ゲーム コントローラを備えた作業](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/WorkingwithGameControllers.html)」の「、 [tvOS のアプリケーション プログラミング ガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/index.html)と[ゲーム コント ローラープログラミング ガイド](https://developer.apple.com/library/prerelease/tvos/documentation/ServicesDiscovery/Conceptual/GameControllerPG/Introduction/Introduction.html)です。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事は、Apple TV、画面をタッチ ジェスチャと Siri リモコン ボタンに付属している新しい Siri リモートをについて説明します。 次に、ジェスチャとストーリー ボード、ジェスチャ、コード、および低レベルのイベントの操作を説明します。 最後に、ゲーム コント ローラーの操作説明されている場合。



## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマン インターフェイス ガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS のアプリケーション プログラミング ガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
