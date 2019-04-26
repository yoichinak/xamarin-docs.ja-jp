---
title: Xamarin.iOS で iPad のマルチタス キング
description: iOS 9 では、同時に、上のスライドを使用してまたは分割ビューで実行されている 2 つのアプリをサポートします。 ビデオの再生ピクチャ イン ピクチャもサポートしています。
ms.prod: xamarin
ms.assetid: 0F2266D7-21FF-404D-A148-0CFDE76B12AA
ms.technology: xamarin-ios
ms.custom: xamu-video
author: lobrien
ms.author: laobri
ms.date: 03/20/2017
ms.openlocfilehash: 68c2ae6eace2669d2ea6c77d72f4476d767c0a7d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61426313"
---
# <a name="multitasking-for-ipad-in-xamarinios"></a>Xamarin.iOS で iPad のマルチタス キング

_iOS 9 では、同時に、上のスライドを使用してまたは分割ビューで実行されている 2 つのアプリをサポートします。ビデオの再生ピクチャ イン ピクチャもサポートしています。_

![](multitasking-images/about02-sml.png "画面の例を分割") ![](multitasking-images/about03-sml.png "ピクチャ ピクチャの使用例")

iOS 9 iPad の特定のハードウェア上で同時に 2 つのアプリを実行するためのマルチタス キングのサポートを追加します。 IPad のマルチタス キングは、次の機能を使用してサポートされています。

- [**スライド上**](#Slide-Over) -現在実行中のメイン アプリケーションの約 25% をカバーする (言語の方向に基づいた画面の右端または左端にあるいずれか) パネルをスライドに一時的に 2 つ目の iOS アプリを実行するユーザーを許可します。 スライドは iPad Pro、iPad 航空、iPad 空気 2、iPad Mini 2、iPad Mini 3、または iPad Mini 4 でのみ使用できます。
- [**分割ビュー**](#Split-View) -iPad のサポートされているハードウェア (iPad 空気 2、iPad ミニ 4 および iPad Pro のみ)、ユーザーは、2 番目のアプリを選択し、分割画面表示モードで現在実行中のアプリとサイド バイ サイドを実行します。 ユーザーは、各アプリを占有するメイン画面の割合を制御できます。
- [**ピクチャインピクチャ**](#Picture-in-Picture) - アプリ、iOS デバイスで現在実行中の他のアプリから浮遊した移動とサイズ変更可能なウィンドウで再生ビデオ コンテンツを再生、ビデオできるようになりました。 ユーザーは、このウィンドウの位置とサイズを完全に制御を持ちます。 図の画像は iPad Pro、iPad 航空、iPad 空気 2、iPad Mini 2、iPad Mini 3、または iPad Mini 4 でのみ使用できます。

ときに考慮すべき点がいくつか[マルチタスクをサポートしているアプリで](#Supporting-Multitasking-in-your-App)など。

- [画面のサイズと向き](#Screen-Size-Considerations)
- [カスタムのハードウェア キーボード ショートカット](#Custom-Hardware-Keyboard-Shortcuts)
- [リソース管理](#Resource-Management-Considerations)

アプリ開発者としてすることもできます[マルチタスクをオプトアウト](#Opting-Out-of-Multitasking)など、 [PIP ビデオの再生を無効にする](#Disabling-PIP-Video-Playback)します。

この記事では、xamarin ios アプリは、マルチタスク処理環境で正常に実行十分でない場合、マルチタスクをオプトアウトする方法、アプリに適していることを確認するための手順を説明します。

> [!VIDEO https://youtube.com/embed/GctYAozoLr8]

**IPad のマルチタス キングによって[Xamarin University](https://university.xamarin.com)**


<a name="Multitasking-QuickStart" />

## <a name="multitasking-quickstart"></a>マルチタスクのクイック スタート

サポートするために**スライド上**または**分割ビュー**アプリは、次を実行する必要があります。

 - IOS 9 (またはそれ以上) をビルドします。
 - 起動画面のストーリー ボードを使用して (およびイメージ アセットがありません)。
 - 自動レイアウトとサイズ クラス、UI のストーリー ボードを使用します。
 - すべて 4 iOS デバイスの向き (縦向き、上下の縦向き、ランドス ケープの左、横の右) をサポートします。

<a name="Multitasking" />

## <a name="about-multitasking-for-ipad"></a>IPad のマルチタス キングについて

iOS 9 ipad の導入に伴い、マルチタスクの新機能を提供する_スライド上_、_分割ビュー_ (iPad 空気 2、iPad Mini 4 および iPad Pro のみ) と_Picture in Picture_します。 次のセクションでこれらの機能について詳しく見てをみましょう。

<a name="Slide-Over" />

### <a name="slide-over"></a>上のスライド

スライド上機能は、2 つ目のアプリを選択し、クイック操作を提供する場合は、小さなスライディング パネルに表示するユーザーを使用できます。 スライドの上パネルは、一時的なものし、メイン アプリをもう一度使用するユーザーが戻るときに閉じられます。

[![](multitasking-images/about01.png "スライドの上パネル")](multitasking-images/about01.png#lightbox)

主な点は、ユーザーは、サイド バイ サイドでと、開発者にこのプロセスに制御が含まれていないことは、どの 2 つのアプリを実行しているが決定です。 その結果、これにはスライドの上パネルの Xamarin.iOS アプリを正しく実行することを確認するために必要なものがあります。

- **自動レイアウトとサイズ クラスを使用して、** -スライド アウト側のパネルで、Xamarin.iOS アプリを実行できる、ため、不要になったに依存できます、デバイス、その画面サイズ、またはその方向にレイアウト UI。 アプリが正しくのインターフェイスを拡張させるには、自動レイアウトとサイズ クラスを使用する必要があります。 詳細については、次を参照してください。 この[Unified ストーリー ボードの概要](~/ios/user-interface/storyboards/unified-storyboards.md)ドキュメント。
- **効率的にリソースを使用して、** -は、アプリがシステム リソースを効率的に使用する重要なアプリできるようになりました共有すること、システム別の実行中のアプリ、ためです。 メモリが疎になると、システムは自動的に最も多くのメモリを使用するアプリケーションを終了します。 Apple を参照してください。 [iOS アプリのエネルギー効率ガイド](https://developer.apple.com/library/prerelease/ios/documentation/Performance/Conceptual/EnergyGuide-iOS/index.html#//apple_ref/doc/uid/TP40015243)の詳細。

スライドは iPad Pro、iPad 航空、iPad 空気 2、iPad Mini 2、iPad Mini 3、または iPad Mini 4 でのみ使用できます。 スライド上のアプリを準備する方法の詳細については、Apple を参照してください[iPad でマルチタスク機能強化の採用](https://developer.apple.com/library/prerelease/ios/documentation/WindowsViews/Conceptual/AdoptingMultitaskingOniPad/index.html#//apple_ref/doc/uid/TP40015145)ドキュメント。

<a name="Split-View" />

### <a name="split-view"></a>分割ビュー

IPad のサポートされているハードウェア (iPad 空気 2、iPad Mini 4 および iPad Pro のみ) では、ユーザーは、2 つ目のアプリを選択し、分割画面表示モードで現在実行中のアプリをサイド バイ サイドで実行倍にできます。 ユーザーがドラッグして各アプリを占有するメイン画面の割合を制御できます、画面上の区分線。

[![](multitasking-images/about02.png "分割ビュー")](multitasking-images/about02.png#lightbox)

スライドの上のように、ユーザーを決定する 2 つのアプリをサイド バイ サイドで実行される、もう一度、開発者にはこのプロセスを制御はありません。 その結果、分割ビューでは、Xamarin.iOS アプリで同様の要件を配置します。

- **自動レイアウトとサイズ クラスを使用して、** -Xamarin.iOS アプリは、ユーザーの指定したサイズで分割画面表示モードで今すぐ実行できる、ため、不要になったに依存できます、デバイス、その画面サイズ、またはレイアウトの方向、UI。 アプリが正しくのインターフェイスを拡張させるには、自動レイアウトとサイズ クラスを使用する必要があります。 詳細については、次を参照してください。 この[Unified ストーリー ボードの概要](~/ios/user-interface/storyboards/unified-storyboards.md)ドキュメント。
- **効率的にリソースを使用して、** -は、アプリがシステム リソースを効率的に使用する重要なアプリできるようになりました共有すること、システム別の実行中のアプリ、ためです。 メモリが疎になると、システムは自動的に最も多くのメモリを使用するアプリケーションを終了します。 Apple を参照してください。 [iOS アプリのエネルギー効率ガイド](https://developer.apple.com/library/prerelease/ios/documentation/Performance/Conceptual/EnergyGuide-iOS/index.html#//apple_ref/doc/uid/TP40015243)の詳細。

分割ビューのアプリを準備する方法の詳細については、Apple を参照してください[iPad でマルチタスク機能強化の採用](https://developer.apple.com/library/prerelease/ios/documentation/WindowsViews/Conceptual/AdoptingMultitaskingOniPad/index.html#//apple_ref/doc/uid/TP40015145)ドキュメント。

<a name="Picture-in-Picture" />

### <a name="picture-in-picture"></a>Picture in Picture

画像の機能で新しい画像 (とも呼ばれます_PIP_) により、ユーザーの他の実行中のアプリ上の画面で任意の位置は小さなフローティング ウィンドウでビデオを見るユーザー。

[![](multitasking-images/about03.png "フローティング ウィンドウの画像に画像の使用例")](multitasking-images/about03.png#lightbox)

スライド上と分割ビューでは、として、ユーザーは、画像モードの図に、ビデオを見て完全制御できます。 アプリの主な機能は、ビデオを見るには、PIP のモードで正しく動作するいくつかの変更を必要になります。 それ以外の場合、PIP をサポートするために変更は必要ありません。

ユーザーの要求で PIP のビデオを表示するアプリのいずれかを使用する必要があります_AVKit_または_AV Foundation Api_します。 Media Player フレームワークでは、iOS 9 の償却されているし、PIP をサポートしていません。

図の画像は iPad Pro、iPad 航空、iPad 空気 2、iPad Mini 2、iPad Mini 3、または iPad Mini 4 でのみ使用できます。 詳細についてを参照してください、 [PictureInPicture サンプル アプリ](https://developer.xamarin.com/samples/ios/iOS9/)と Apple の[画像のクイック スタートで画像](https://developer.apple.com/library/prerelease/ios/documentation/WindowsViews/Conceptual/AdoptingMultitaskingOniPad/QuickStartForPictureInPicture.html#//apple_ref/doc/uid/TP40015145-CH14)ドキュメント。

<a name="Supporting-Multitasking-in-your-App" />

## <a name="supporting-multitasking-in-your-app"></a>アプリでのマルチタス キングのサポート

すべての既存の Xamarin.iOS アプリでは、マルチタスク処理をサポートしているは、透過的なアプリは、既に Apple の設計ガイドおよび iOS 8 のベスト プラクティスに従う限り、します。 つまり、アプリする必要がありますを使用することのストーリー ボードを 自動レイアウトとサイズ クラスのユーザー インターフェイス レイアウトを (を参照してください、 [Unified ストーリー ボードの概要](~/ios/user-interface/storyboards/unified-storyboards.md)詳細については)。

これらのアプリは、ほとんどまたはまったく変更は、マルチタスク処理をサポートしも内で動作する必要があります。 直接配置や UI 要素をサイズ変更などの他のメソッドを使用してが作成された場合は、アプリの UIC#コードまたは特定のデバイスの画面サイズや向きに依存している場合に iOS 9 のマルチタスクを正しくサポートするために大幅な変更は必要があります。

もう一度新しい Xamarin.iOS アプリを iOS 9 のマルチタスクをサポートするためにオートとサイズ クラスで、アプリのユーザー インターフェイスのレイアウトのすべてのストーリー ボードを使用し、次のセクションの手順を実装します。

<a name="Screen-Size-Considerations" />

### <a name="screen-size-and-orientation-considerations"></a>画面のサイズと向きに関する考慮事項

IOS 9 の場合は、前に、特定のデバイスの画面サイズや向きに対してアプリを設計する可能性があります。 スライド アウト ウィンドウで、または分割ビュー モードで、アプリを実行できるようになりました、ため、それ自体で実行されているいずれかで compact または通常の水平方向のサイズ クラス、デバイスの物理的な印刷の向きや画面サイズに関係なく、iPad を検索できます。

[![](multitasking-images/sizeclasses01.png "画面のサイズと向きに関する考慮事項")](multitasking-images/sizeclasses01.png#lightbox)

Ipad では全画面表示アプリは正規の水平および垂直方向のサイズ クラスがあります。 すべての Iphone は iPhone 6 Plus と iPhone 6 s さらに、任意の方向、双方向にコンパクト サイズ クラスがあります。 IPhone 6 Plus と iPhone 6 s Plus 横モードである通常の水平方向のサイズ クラスと Compact 垂直サイズ クラス (iPad Mini 同様)。

スライド上と分割ビュー サポート付きの場合は、Ipad では、次の組み合わせを使用終了できます。

| **[方向]** | **プライマリのアプリ** | **セカンダリのアプリ** |
|--- |--- |--- |
| **縦向き** |画面の 75%<br />Compact の水平方向<br />通常の垂直|画面の 25%<br />Compact の水平方向<br />通常の垂直|
| **横** |画面の 75%<br />通常の水平方向<br />通常の垂直|画面の 25%<br />Compact の水平方向<br />通常の垂直|
| **横** |画面の 50%<br />Compact の水平方向<br />通常の垂直|画面の 50%<br />Compact の水平方向<br />通常の垂直|

例では[MuliTask](https://developer.xamarin.com/samples/monotouch/ios9/MultiTask/)アプリでは、横置きモードの iPad で全画面表示を実行した場合、リストと同時に、詳細ビューの両方が表示されます。

[![](multitasking-images/sizeclasses03.png "リストと同時に表示される詳細ビュー")](multitasking-images/sizeclasses03.png#lightbox)

スライドの上パネルで、同じアプリを実行した場合、Compact 水平サイズ クラスとしてレイアウトされ、一覧のみが表示されます。

[![](multitasking-images/sizeclasses04.png "デバイスが水平方向に表示されるリストのみ")](multitasking-images/sizeclasses04.png#lightbox)

このような状況で、アプリが正しく動作させるは必要がありますと共にサイズ クラスの特徴であるコレクションを採用しに準拠している、`IUIContentContainer`と`IUITraitEnvironment`インターフェイス。 Apple を参照してください。 [UITraitCollection クラス参照](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UITraitCollection_ClassReference/index.html#//apple_ref/doc/uid/TP40014202)と[Unified ストーリー ボードの概要](~/ios/user-interface/storyboards/unified-storyboards.md)詳細ガイド。

さらに、不要になった、アプリの可視領域を定義するデバイスの画面の境界に依存することができます、代わりに、アプリのウィンドウの境界を使用する必要があります。 ウィンドウの境界は、ユーザーの制御下では完全であるために調整またはユーザーがこれらの境界を変更できないようにプログラムを使用できません。

最後に、アプリのセットを使用してではなく起動画面を提示するストーリー ボード ファイルを使用する必要があります **.png**イメージ ファイルとすべての 4 つのインターフェイスの向き (縦、上下縦、横の左側および横の右) のサポートスライドの上パネルまたは分割ビュー モードで実行されていると見なされます。

<a name="Custom-Hardware-Keyboard-Shortcuts" />

### <a name="custom-hardware-keyboard-shortcuts"></a>カスタムのハードウェア キーボード ショートカット

IOS 9 iPad で実行されている、Apple には、ハードウェア キーボードのサポートを拡張します。 Ipad には、Bluetooth と組み込みの iOS 固有のキーに含まれる一部のキーボードを作成する製造元キーボードを使用して基本的な外付けのキーボードのサポートが常に含まれます。

次に、9、iOS でアプリが、独自のカスタムのキーボード ショートカットを作成できます。 また、いくつかの基本的なキーボード ショートカットはのような利用**コマンド C** (コピー)、**コマンド X** (切り取り)、**コマンド V** (貼り付け) と**コマンド-h Shift** (ホーム)、それらを具体的には書き込まれた応答をされているアプリなし。

**コマンド タブ**を Mac OS と同様に、キーボードからアプリをすばやく切り替えるユーザーを許可するアプリ スイッチャーが表示されます。

[![](multitasking-images/keyboard01.png "アプリケーションのスイッチャー")](multitasking-images/keyboard01.png#lightbox)

、ユーザーが保持できる iOS 9 アプリには、キーボード ショートカットが含まれている場合、**コマンド**、**オプション**または**コントロール**ポップアップで表示するキー。

[![](multitasking-images/keyboard02.png "キーボード ショートカット ポップアップ")](multitasking-images/keyboard02.png#lightbox)

#### <a name="defining-custom-keyboard-shortcuts"></a>カスタム キーボード ショートカットを定義します。

追加する場合、次のコード ビューまたはビュー コント ローラーで、アプリでは、そのビューまたはコント ローラーが表示される、カスタムのキーボード ショートカットが提供されます。

```csharp
#region Custom Keyboard Shortcut
public override bool CanBecomeFirstResponder {
    get { return true; }
}

public override UIKeyCommand[] KeyCommands {
    get {

        var keyCommand = UIKeyCommand.Create (new NSString("n"), UIKeyModifierFlags.Command, new Selector ("NewEntry"), new NSString("New Entry"));
        return new UIKeyCommand[]{ keyCommand };
    }
}

[Export("NewEntry")]
public void NewEntry() {

    // Add a new entry
    ...

}
#endregion
```

最初に、私たちのオーバーライド、`CanBecomeFirstResponder`プロパティと戻り`true`ビューまたはビュー コント ローラーは、キーボード入力を受信できるようにします。 

次に、無効に、`KeyCommands`プロパティされ、新しい作成`UIKeyCommand`の**コマンド N**キー。 呼び出して、キーストロークをアクティブになると、`NewEntry`メソッド (を使用して iOS 9 を公開している`Export`コマンド) 要求のアクションを実行します。

ハードウェア キーボードを接続すると、ユーザーが iPad でこのアプリを実行した場合**コマンド N**、新しいエントリを一覧に追加されます。 ユーザーが保持している場合、**コマンド**キー、ショートカットの一覧が表示されます。

[![](multitasking-images/keyboard03.png "キーボード ショートカット ポップアップ")](multitasking-images/keyboard03.png#lightbox)

このサンプルを参照してください[MultiTask アプリ](https://developer.xamarin.com/samples/monotouch/ios9/MultiTask/)実装例についてはします。

<a name="Resource-Management-Considerations" />

### <a name="resource-management-considerations"></a>リソースの管理に関する考慮事項

IOS 8 のデザイン ガイドおよびベスト プラクティスを既に使用してアプリの場合でも効率的なリソースの管理に、問題ある可能性があります。 Ios 9 でアプリでは、メモリ、CPU または他のシステム リソースを排他的に使用が不要になったがあります。

その結果、システム リソースを効果的に使用する Xamarin.iOS アプリを微調整する必要があります。 またはメモリ不足の状況での終了を向けます。 これは、マルチタスク処理を無効にするアプリの場合は true。 同様に、2 つ目以降アプリ可能性がありますもする実行スライドの上パネルまたは余分なリソースを必要とする、リフレッシュ レートを原因または画像のウィンドウで画像を 1 秒あたり 60 フレームを下回る。

次のユーザーの操作とその関連事項を考慮してください。

- **スライドの上パネルでテキストを入力する**-UI 経由でにシステム キーボード、アプリにテキスト入力があるない場合でも表示ようになりましたことができます。 その結果、アプリは、表示して、キーボードを非表示) などのキーボード表示通知に応答する必要があります。
- **スライドの上パネルで 2 つ目のアプリを実行している**-新しいアプリは、フォア グラウンドで実行されているとメモリと CPU サイクルなどのシステム リソースの既存のアプリと競合するようになりました。
- **PIP のウィンドウでビデオを再生する**- いないのみ、このウィンドウで、アプリのインターフェイスの一部を覆うことができますが、ビデオを起動したアプリは引き続きバック グラウンドで実行されている、CPU とメモリ リソースを消費します。

アプリがリソースを効率的に使用していることを確認するには、するには、次の操作を行う必要があります。

- **Instruments を使用したアプリのプロファイリング**-明白な CPU 使用率と領域で、アプリをブロックしているメイン スレッド、メモリ リークを確認します。
- **状態遷移メソッドに応答**-、 **AppDelegate.cs**ファイルの上書きと状態の応答を入力またはバック グラウンドからのアプリなどのメソッドを変更します。 イメージ、データまたはビューのビュー コント ローラーなど、不要なアセットをリリースします。
- **メモリ集中型アプリを使用して並列でテスト**- スライド アウトとハードウェアの物理的な iOS では Split View の両方を使用して、メモリ集中型アプリ (サテライト ビュー モード) でマップなどでアプリを実行し、両方のアプリが応答性を維持し、クラッシュしていないことをテストします。

Apple を参照してください。 [iOS アプリのエネルギー効率ガイド](https://developer.apple.com/library/prerelease/ios/documentation/Performance/Conceptual/EnergyGuide-iOS/index.html#//apple_ref/doc/uid/TP40015243)リソース管理の詳細についてはします。

<a name="Opting-Out-of-Multitasking" />

## <a name="opting-out-of-multitasking"></a>マルチタスクを無効にします。

Apple では、すべての iOS 9 アプリがマルチタスクをサポートすることからわかるように、中に可能性がある場合、アプリ用の特別な理由すぎない、ゲームやカメラ アプリを正常に動作する全画面表示を必要とするなど。

いずれかのスライド アウト パネルまたは分割ビュー モードで実行されているをオプトアウトする Xamarin.iOS アプリの編集、プロジェクトの**Info.plist**ファイルを開きチェック**全画面表示の必要があります**:

[![](multitasking-images/fullscreen01.png "マルチタスクを無効にします。")](multitasking-images/fullscreen01.png#lightbox)

> [!IMPORTANT]
> マルチタスクを無効にするには、スライド アウトまたは分割ビューで実行されてから、アプリが防止されますが、ことから、アプリと共に表示するスライド アウトまたはビデオの図の画像で実行されているから別のアプリはできます。

<a name="Disabling-PIP-Video-Playback" />

### <a name="disabling-pip-video-playback"></a>PIP のビデオ再生を無効にします。

ほとんどの場合、アプリは、フローティング ウィンドウの画像に画像の表示、ビデオ コンテンツを再生するユーザーを許可する必要があります。 ただし、状況は、この必要がありますいない、ゲームのカット シーン ビデオなどがあります。

PIP のビデオ再生をオプトアウトするには、アプリでは、次を実行します。

 - 使用する場合、`AVPlayerViewController`ビデオを表示するには、設定、`AllowsPictureInPicturePlayback`プロパティを`false`します。
 - 使用する場合、`AVPlayerLayer`ビデオを表示するのインスタンスを作成しない、`AVPictureInPictureController`します。
 - 使用する場合、`WKWebView`ビデオを表示するには、設定、`AllowsPictureInPictureMediaPlayback`プロパティを`false`します。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、Xamarin.iOS アプリが実行され、Ipad 用の iOS 9 の新しいマルチタスク機能で正しく機能することを確認するための手順について説明しました。 さらに、アプリが適合のマルチタス キングのオプトイン アウトがについて説明します。



## <a name="related-links"></a>関連リンク

- [iOS 9 のサンプル](https://developer.xamarin.com/samples/ios/iOS9/)
- [マルチタスク (サンプル)](https://developer.xamarin.com/samples/monotouch/ios9/MultiTask/)
- [統合ストーリー ボードの概要](~/ios/user-interface/storyboards/unified-storyboards.md)
- [iOS 9 開発者向け](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [IPad のマルチタス キングに拡張機能を導入します。](https://developer.apple.com/library/prerelease/ios/documentation/WindowsViews/Conceptual/AdoptingMultitaskingOniPad/index.html#//apple_ref/doc/uid/TP40015145)
- [ブログの投稿](https://blog.xamarin.com/using-auto-layouts-for-ios-9-splitview/)
