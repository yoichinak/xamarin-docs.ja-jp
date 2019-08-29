---
title: Xamarin の tvOS 用 siri リモートおよび Bluetooth コントローラー
description: この記事では、Xamarin で記述された tvOS アプリで Siri リモートおよび Bluetooth ゲームコントローラーを操作する方法について説明します。
ms.prod: xamarin
ms.assetid: BDB9894A-236B-424B-9032-ACD12A6C5720
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 1e1e86c6301214c7117b8f3b21b19554499d7fbd
ms.sourcegitcommit: 1dd7d09b60fcb1bf15ba54831ed3dd46aa5240cb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/28/2019
ms.locfileid: "70121441"
---
# <a name="siri-remote-and-bluetooth-controllers-for-tvos-in-xamarin"></a>Xamarin の tvOS 用 siri リモートおよび Bluetooth コントローラー

TvOS アプリのユーザーは、iOS と直接やり取りするのではなく、デバイスの画面上のイメージをタップしますが、 [Siri リモート](#The-Siri-Remote)を使用してルーム間で間接的に移動します。

アプリがゲームの場合は、必要に応じて、アプリで iOS (MFI) [Bluetooth ゲームコントローラー](#Bluetooth-Game-Controllers)用に作成されたサードパーティのサポートを組み込むこともできます。

[![](remote-bluetooth-images/intro01.png "Bluetooth リモートコントローラーとゲームコントローラー")](remote-bluetooth-images/intro01.png#lightbox)

この記事では、 [Siri リモコン](#The-Siri-Remote)、[タッチスクリーンのジェスチャ](#Touch-Surface-Gestures)、および[siri のリモートボタン](#Siri-Remote-Buttons)について説明し、[ジェスチャ、ストーリーボード](#Gestures-and-Storyboards)、[ジェスチャ、コード](#Gestures-and-Code)、[低レベルのイベント処理](#Low-Level-Event-Handling)を使用してそれらを操作する方法を示します。 最後に、tvOS アプリでの[ゲームコントローラーの操作](#Working-with-Game-Controllers)について説明します。

<a name="The-Siri-Remote" />

## <a name="the-siri-remote"></a>Siri リモート

ユーザーが Apple TV と tvOS アプリを操作する主な方法は、付属の Siri リモートを使用することです。 Apple は、ソファに座っているユーザーと、テレビ画面の部屋に表示される Apple TV のユーザーインターフェイスの距離をリモートでブリッジするように設計されています。

TvOS アプリ開発者としての課題は、Siri リモコンのタッチスクリーン、加速度計、ジャイロスコープ、およびボタンを活用する、簡単で使いやすく、視覚的に説得力のあるユーザーインターフェイスを作成することです。

[![](remote-bluetooth-images/remote01.png "Siri リモート")](remote-bluetooth-images/remote01.png#lightbox)

Siri リモートには、tvOS アプリ内で次の機能と想定される使用法があります。

|機能|一般的なアプリの使用状況|ゲームアプリの使用状況|
|---|---|---|
|**タッチ画面**<br />スワイプして移動し、を押してコンテキストメニューを選択して保持します。|**タップ/スワイプ**<br />フォーカスがある項目間の UI ナビゲーション。<br /><br />**ここを**<br />選択した (フォーカスされている) 項目をアクティブにします。|**タップ/スワイプ**<br />ゲームの設計によって異なり、端をタップして D パッドとして使用できます。<br /><br />**ここを**<br />主ボタンの機能を実行します。|
|**Menu**<br />前の画面またはメニューに戻るには、キーを押します。|前の画面に戻り、メインアプリ画面から Apple TV ホーム画面に出ます。|ゲームプレイを一時停止して再開し、前の画面に戻り、メインアプリ画面から Apple TV ホーム画面に出ます。|
|**Siri/検索**<br />Siri を使用している国では、音声制御のためにプレスアンドホールドを行うと、他のすべての国で検索画面が表示されます。|N/A|N/A|
|**再生/一時停止**<br />メディアを再生または一時停止するか、アプリでセカンダリ機能を提供します。|メディアの再生を開始し、再生を一時停止/再開します。|2番目のボタン関数を実行します。または、導入ビデオ (存在する場合) をスキップします。|
|**Home**<br />を押してホーム画面に戻り、ダブルクリックして実行中のアプリを表示し、スリープデバイスに押したままにします。|N/A|N/A|
|**体積**<br />接続されているオーディオ/ビデオ機器ボリュームを制御します。|N/A|N/A|

<a name="Touch-Surface-Gestures" />

## <a name="touch-surface-gestures"></a>タッチスクリーンのジェスチャ

Siri リモコンのタッチ画面は、tvOS アプリで応答できる、次のような単一の指ジェスチャを検出できます。

|スワイプ|ここを|タップ|
|---|---|---|
|![](remote-bluetooth-images/Gesture01.png)|![](remote-bluetooth-images/Gesture02.png)|![](remote-bluetooth-images/Gesture03.png)|
|画面上の UI 要素 (上、左、右) の間で選択 (フォーカス) を移動します。 スワイプを使用すると、慣性を使用して、コンテンツの大きなリストをすばやくスクロールできます。|選択した (フォーカスされている) 項目をアクティブにします。または、ゲームの主ボタンと同じように動作します。 クリックして保持すると、コンテキストメニューまたはセカンダリ関数をアクティブにすることができます。|端のタッチサーフェスを軽くタップすると、D パッドの方向ボタンと同じように動作し、タップされた領域に応じて上下左右左右に移動します。 アプリによっては、を使用して非表示のコントロールを表示できます。|

Apple では、タッチスクリーンのジェスチャを操作するための次の提案が提供されています。

- **クリックとタップを区別**する-クリックはユーザーによる意図的な操作であり、選択、アクティブ化、およびゲームの主なボタンに適しています。 タップはより微妙であり、ユーザーが通常は Siri リモートを保持し、誤って Tap イベントを簡単にアクティブ化できるため、控えめに使用する必要があります。
- **標準ジェスチャを再定義しないでください**。特定のジェスチャによって特定のアクションが実行されることを見込んで、アプリでこれらのジェスチャの意味や機能を再定義することはできません。 1つの例外は、アクティブなプレイゲーム中のゲームアプリです。
- **新しいジェスチャを控えめに定義**します。ここでも、特定のジェスチャによって特定のアクションが実行されることを見込んでいます。 標準アクションを実行するためのカスタムジェスチャを定義することは避けてください。 また、ゲームは、カスタムジェスチャがゲームに楽しい、イマーシブゲームを追加できるという、最も一般的な例外です。
- **必要に応じて、d パッドタップに応答**します。タッチサーフェイスのコーナー端を軽くタップすると、ゲームコントローラー上の d パッドのように反応します。これは、フォーカスを移動するか、上下左右に移動します。 必要に応じて、アプリまたはゲームでこれらのジェスチャに応答する必要があります。

<a name="Siri-Remote-Buttons" />

## <a name="siri-remote-buttons"></a>Siri のリモートボタン

タッチ画面でのジェスチャに加えて、アプリはタッチ画面をクリックするか、[再生/一時停止] ボタンを押すと、ユーザーに応答できます。 Game Controller フレームワークを使用して Siri リモートにアクセスしている場合は、押されたメニューボタンを検出することもできます。 

また、メニューボタンの押下は、標準`UIKit`の要素でジェスチャ認識エンジンを使用して検出できます。 押されたメニューボタンをインターセプトした場合は、現在のビューとビューコントローラーを閉じ、前のビューに戻る必要があります。

> [!IMPORTANT]
> **常**に、リモートの再生/一時停止ボタンに関数を割り当てる必要があります。 機能しないボタンを使用すると、アプリがエンドユーザーに表示されることを防ぐことができます。 このボタンに有効な関数がない場合は、プライマリボタンと同じ機能を割り当てます (タッチスクリーンのクリック)。

<a name="Gestures-and-Storyboards" />

## <a name="gestures-and-storyboards"></a>ジェスチャとストーリーボード

TvOS アプリで Siri リモートを操作する最も簡単な方法は、インターフェイスデザイナーでビューにジェスチャレコグナイザーを追加することです。

ジェスチャ認識エンジンを追加するには、次の手順を実行します。

1. **ソリューションエクスプローラー**で、 `Main.storyboard`ファイルをダブルクリックして開き、インターフェイスデザイナーを編集します。
2. **タップジェスチャレコグナイザー**を**ライブラリ**からドラッグし、ビューにドロップします。 

    [![](remote-bluetooth-images/storyboard01.png "タップジェスチャレコグナイザー")](remote-bluetooth-images/storyboard01.png#lightbox)
3. **属性インスペクター**の **[ボタン]** セクションで **[選択] を選択し**ます。 

    [![](remote-bluetooth-images/storyboard02.png "選択のチェック")](remote-bluetooth-images/storyboard02.png#lightbox)
4. **選択**すると、ユーザーに対してジェスチャが Siri リモートの**タッチ画面**をクリックしたときに応答します。 **メニュー**、**再生/一時停止**、**上**、**下**、**左**、**右**の各ボタンに応答するオプションも用意されています。
5. 次に、**タップジェスチャ認識エンジン**から**アクション**を接続して、 `TouchSurfaceClicked`次のように呼び出します。 

    [![](remote-bluetooth-images/storyboard03.png "タップジェスチャ認識エンジンからのアクション")](remote-bluetooth-images/storyboard03.png#lightbox)
6. 変更内容を保存し、Visual Studio for Mac に戻ります。

ビューコントローラー (例`FirstViewController.cs`) ファイルを編集し、トリガーされるジェスチャを処理する次のコードを追加します。

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

ストーリーボードの操作の詳細については、「 [Hello, tvOS クイックスタートガイド](~/ios/tvos/get-started/hello-tvos.md)」を参照してください。 具体的には、[ユーザーインターフェイスを作成](~/ios/tvos/get-started/hello-tvos.md#Creating-the-User-Interface)し、[アウトレットとアクションのセクションを含むコードを記述](~/ios/tvos/get-started/hello-tvos.md#Writing-the-Code)します。

<a name="Gestures-and-Code" />

## <a name="gestures-and-code"></a>ジェスチャとコード

必要に応じて、コード内でC#直接ジェスチャを作成し、ユーザーインターフェイスのビューに追加することができます。 たとえば、一連のスワイプジェスチャレコグナイザーを追加するには、ビューコントローラーを編集し、次のコードを追加します。

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

TvOS アプリ (など`UIKit` `UIView`) でに基づいてカスタム型を作成する場合は、イベントによる`UIPress`ボタンの押下を低レベルで処理することもできます。 

イベントは、iOS に対して`UITouch`イベントを tvOS することです`UIPress` 。ただし、siri リモートまたは接続されている他の Bluetooth デバイス (ゲームコントローラーなど) では、ボタンの押下に関する情報は返されません。 `UIPress` `UIPress`イベントは、押されているボタンとその状態 (開始、キャンセル、変更、または終了) を記述します。 

Bluetooth ゲームコントローラーなどのデバイス上のアナログボタン`UIPress`の場合は、ボタンに適用されているフォースの量も返されます。 イベントのプロパティは`Type` 、状態が変更された物理ボタンを定義します。プロパティの残りの部分は、発生した変更を示します。 `UIPress`

次のコードは、 `UIPress` `UIView`の下位レベルのイベントを処理する例を示しています。

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

イベントと`UITouch`同様に、任意`UIPress`のイベントオーバーライドを実装する必要がある場合は、4つすべてを実装する必要があります。

<a name="Bluetooth-Game-Controllers" />

## <a name="bluetooth-game-controllers"></a>Bluetooth ゲームコントローラー

Apple TV に同梱されている標準 Siri リモートに加え、iOS (MFI) Bluetooth ゲームコントローラー用に作成されたサードパーティは、Apple TV とペアリングし、tvOS アプリを制御するために使用できます。

[![](remote-bluetooth-images/game01.png "Bluetooth ゲームコントローラー")](remote-bluetooth-images/game01.png#lightbox)

ゲームコントローラーを使用してゲームプレイを強化し、ゲームで immersion を実現できます。 また、標準の Apple TV インターフェイスを制御するために使用することもできます。そのため、リモートとコントローラーを使用する必要はありません。

> [!IMPORTANT]
> Bluetooth ゲームコントローラーは、エンドユーザーが行うことができるオプションの購入で、アプリはユーザーに購入を強制することはできません。 アプリでゲームコントローラーがサポートされている場合は、すべての Apple TV ユーザーがゲームを利用できるように、Siri リモートもサポートする必要があります。

ゲームコントローラーには、tvOS アプリ内で次の機能と想定される使用法があります。

|機能|一般的なアプリの使用状況|ゲームアプリの使用状況|
|---|---|---|
|**D-Pad**|UI 要素をナビゲートします (変更にフォーカスがあります)。|ゲームに依存します。|
|**A**|選択した (フォーカスされている) 項目をアクティブにします。|プライマリボタン関数を実行し、ダイアログアクションを確認します。|
|**B**|アプリのメイン画面で、前の画面に戻るか、ホーム画面を終了します。|2番目のボタンの機能を実行するか、前の画面に戻ります。|
|**X**|メディアの再生を開始するか、再生を一時停止/再開します。|ゲームに依存します。|
|**Y**|N/A|ゲームに依存します。|
|**Menu**|アプリのメイン画面で、前の画面に戻るか、ホーム画面を終了します。|ゲームプレイを一時停止/再開する、前の画面に戻る、またはアプリのメイン画面にある場合はホーム画面を終了します。|
|**左ショルダーボタン**|左に移動します。|ゲームに依存します。|
|**左のトリガー**|左に移動します。|ゲームに依存します。|
|**右ショルダーボタン**|右に移動します。|ゲームに依存します。|
|**右トリガー**|右に移動|ゲームに依存します。|
|**左サムスティック**|UI 要素をナビゲートします (変更にフォーカスがあります)。|ゲームに依存します。|
|**右スティック**|N/A|ゲームに依存します。|

Apple では、ゲームコントローラーを操作するための次の推奨事項を提供しています。

- **ゲームコントローラー接続の確認**-tvOS アプリは、エンドユーザーがいつでも開始および停止できます。 常に、アプリの開始時または起動時にゲームコントローラーの存在を確認し、必要に応じてアクションを実行する必要があります。
- **アプリケーションが Siri リモートコントローラーとゲームコントローラーの両方で動作することを確認**します。ユーザーがアプリを使用するには、Siri リモートとゲームコントローラーを切り替える必要はありません。 両方の種類のコントローラーでアプリを頻繁にテストすることで、すべてを簡単に移動して期待どおりに動作させることができます。
- メニューボタンを**戻る方法を指定すると**、常に前の画面に戻るようになります。 ユーザーがメインアプリ画面にある場合、メニューボタンは Apple TV ホーム画面に戻ります。 ゲームプレイ中にメニューボタンをクリックすると、ユーザーがゲームプレイを一時停止/再開したり、メインメニューに戻ることができるようにするアラートが表示されます。

<a name="Working-with-Game-Controllers" />

## <a name="working-with-game-controllers"></a>ゲームコントローラーの操作

前述のように、Apple TV に同梱されている標準の Siri リモートに加えて、ユーザーは必要に応じて、iOS (MFI) Bluetooth ゲームコントローラー用のサードパーティを接続し、それを使用して tvOS アプリを制御できます。

アプリで低レベルのコントローラー入力が必要な場合は、Apple の[ゲームコントローラーフレームワーク](https://developer.apple.com/library/prerelease/tvos/documentation/ServicesDiscovery/Conceptual/GameControllerPG/Introduction/Introduction.html#//apple_ref/doc/uid/TP40013276)を使用できます。これには、tvOS に対して次のような変更があります。

- Siri リモートをターゲットに`GCMicroGamepad`するために、マイクロゲームコントローラープロファイル () が追加されました。
- 新しい`GCEventViewController`クラスを使用して、アプリを通じてゲームコントローラーイベントをルーティングできます。 詳細については、後述の「[ゲームコントローラーの入力の決定](#determining-game-controller-input)」を参照してください。

<a name="Game-Controller-Support-Requirements" />

### <a name="game-controller-support-requirements"></a>ゲームコントローラーのサポート要件

TvOS アプリでゲームコントローラーがサポートされている場合、Apple にはいくつかの特定の要件を満たす必要があります。

- **Siri リモートをサポートする必要があり**ます。 Siri リモートを常にサポートする必要があります。 ゲームでは、サードパーティ製のゲームコントローラーを再生可能にする必要がありません。
- **拡張コントロールレイアウトをサポートする必要があり**ます。すべての TvOS Game controller は非フォーム継ぎ手で、拡張コントローラーです。
- **ゲームはスタンドアロンコントローラーで再生できる必要があり**ます。アプリが拡張ゲームコントローラーをサポートしている場合は、そのゲームコントローラーだけを再生可能にする必要があります。
- **再生/一時停止ボタンがサポートされている必要があり**ます。ゲームプレイ中に、ユーザーが [再生/一時停止] ボタンを押すと、ユーザーがゲームを一時停止/再開したり、メインメニューに戻ったりすることを許可するアラートが表示されます。

<a name="Enabling-Game-Controller-Support" />

### <a name="enabling-game-controller-support"></a>ゲームコントローラーのサポートを有効にする

TvOS アプリでゲームコントローラーのサポートを有効にするには、**ソリューションエクスプローラー**内`Info.plist`のファイルをダブルクリックして、編集用に開きます。

[![](remote-bluetooth-images/game02.png "情報 plist エディター")](remote-bluetooth-images/game02.png#lightbox)

**[Game Controller]** セクションで、[game controller] を**有効**にして、アプリでサポートされるすべてのゲームコントローラーの種類を確認します。

<a name="Using-the-Siri-Remote-as-a-Game-Controller" />

### <a name="using-the-siri-remote-as-a-game-controller"></a>ゲームコントローラーとしての Siri リモートの使用

Apple TV に付属する Siri リモートは、限定されたゲームコントローラーとして使用できます。 他のゲームコントローラーと同様に、ゲームコントローラーフレームワークには`GCController`オブジェクトとして表示され、 `GCMicroGamepad`プロファイルとプロファイルの`GCMotion`両方をサポートします。

Siri リモートは、ゲームコントローラーとして使用される場合、次の特性を持ちます。

- タッチサーフェスは、アナログ入力データを提供する D パッドとして使用できます。
- リモートは、縦向きまたは横方向のどちらかで使用できます。アプリは、プロファイルオブジェクトが入力データを自動的に反転するかどうかを決定します。
- タッチ画面をクリックすると、ゲームコントローラーでボタン**A を**押すのと同じように動作します。
- 再生/一時停止ボタンは、ゲームコントローラーのボタン**X**のように機能します。
- メニューボタンをクリックすると、ユーザーがゲームプレイを一時停止/再開したり、メインメニューに戻ったりできるようにするアラートが表示されます。

<a name="Summary" />

### <a name="determining-game-controller-input"></a>ゲームコントローラーの入力を確認する

ゲームコントローラーのイベントをタッチイベントと並行して受信できる iOS とは異なり、tvOS はすべての低レベルイベントを処理`UIKit`して、高レベルのイベントを配信します。 そのため、低レベルのゲームコントローラーイベントにアクセスする必要がある場合は、の既定の動作`UIKit`をオフにする必要があります。

TvOS でゲームコントローラーの入力を直接処理する場合は、(またはサブ`GCEventViewController`クラス) を使用してゲームのユーザーインターフェイスを表示する必要があります。 が最初のレスポンダーの場合、ゲームコントローラーの入力がキャプチャされ、game controller フレームワークを介してアプリに配信されます。 `GCEventViewController`

クラスのプロパティ`UserInteractionEnabled`を使用して、イベントの処理方法と処理方法を切り替えることができます。 `GCEventViewController`

ゲームコントローラーサポートの実装の詳細については、「[アプリプログラミングガイド (tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/index.html)および[ゲームコントローラープログラミングガイド](https://developer.apple.com/library/prerelease/tvos/documentation/ServicesDiscovery/Conceptual/GameControllerPG/Introduction/Introduction.html))」の「Apple の[ゲームコントローラーの操作](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/WorkingwithGameControllers.html)」セクションを参照してください。

<a name="Summary" />

## <a name="summary"></a>Summary

この記事では、Apple TV、タッチスクリーンジェスチャ、Siri リモートボタンに付属する新しい Siri リモコンについて説明しました。 次に、ジェスチャとストーリーボード、ジェスチャ、コードおよび低レベルのイベントの操作について説明します。 最後に、ゲームコントローラーの操作について説明します。



## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマンインターフェイスガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS のアプリプログラミングガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
