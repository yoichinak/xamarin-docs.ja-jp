---
title: "IPad のマルチタスク"
description: "iOS 9 には、同時に、スライドを使用して、上または分割ビューで実行されている 2 つのアプリがサポートされています。 ビデオの再生ピクチャ ピクチャもサポートします。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 0F2266D7-21FF-404D-A148-0CFDE76B12AA
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 8e5bb4747811729adf5363b0a893b0f85108b220
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="multitasking-for-ipad"></a>IPad のマルチタスク

_iOS 9 には、同時に、スライドを使用して、上または分割ビューで実行されている 2 つのアプリがサポートされています。ビデオの再生ピクチャ ピクチャもサポートします。_

![](multitasking-images/about02-sml.png "画面の例を分割") ![ ](multitasking-images/about03-sml.png "ピクチャ ピクチャの使用例")

iOS 9 iPad の特定のハードウェアで同時に 2 個のアプリを実行するには、マルチタスク処理サポートを追加します。 IPad のマルチタス キングは、次の機能を使用してサポートされます。

- [**スライド上**](#Slide-Over) -現在実行されているメインのアプリの約 25% がカバーする (言語方向に基づいて、画面の右側または左側にいずれか) パネルをスライドに一時的に 2 つ目の iOS アプリを実行することができます。 スライドは iPad Pro、iPad 空気、iPad 空気 2、iPad ミニ 2、iPad ミニ 3、または iPad ミニ 4 でのみ使用できます。
- [**分割ビュー** ](#Split-View) -iPad のサポートされているハードウェア (iPad 空気 2、iPad ミニ 4 および iPad Pro のみ)、ユーザーは、2 番目のアプリを選択し、分割画面表示モードで現在実行中のアプリとサイド バイ サイドを実行します。 ユーザーは、各アプリを占有するメイン画面の割合を制御できます。
- [**画像に画像**](#Picture-in-Picture) - アプリの移動とサイズを変更できる ウィンドウで、iOS デバイスで現在実行されている他のアプリで再生するビデオ コンテンツの再生、ビデオできるようになりました。 ユーザーは、このウィンドウの位置とサイズを完全に制御を持ちます。 図の画像は iPad Pro、iPad 空気、iPad 空気 2、iPad ミニ 2、iPad ミニ 3、または iPad ミニ 4 でのみ使用できます。

多くのときに考慮することがある[マルチタスク処理をサポートするアプリで](#Supporting-Multitasking-in-your-App)など。

- [画面のサイズと向き](#Screen-Size-Considerations)
- [カスタムのハードウェア キーボード ショートカット](#Custom-Hardware-Keyboard-Shortcuts)
- [リソース管理](#Resource-Management-Considerations)

アプリの開発者とすることもできます[マルチタスクのオプトアウト](#Opting-Out-of-Multitasking)など、 [PIP ビデオの再生を無効にする](#Disabling-PIP-Video-Playback)です。

この記事では、マルチタスク処理環境で正しく Xamarin.iOS アプリの実行中またはをアプリに合わせて十分でない場合は、マルチタスク処理から除外する方法のことを確認するための手順を説明します。

<a name="Multitasking-QuickStart" />

## <a name="multitasking-quickstart"></a>マルチタスクのクイック スタート

サポートする**スライド上**または**分割ビュー**アプリは、次を行う必要があります。

 - IOS 9 (またはそれ以上) に対してビルドされます。
 - 起動画面のストーリー ボードを使用して (およびイメージ資産)。
 - ストーリー ボードを Autolayout とサイズのクラスとその UI を使用します。
 - すべて 4 iOS デバイスの向き (縦方向、上下縦、横左と横の右) をサポートします。

<a name="Multitasking" />

## <a name="about-multitasking-for-ipad"></a>IPad のマルチタスクについて

iOS 9 iPad の導入に伴いで新しいマルチタスク処理能力を提供する_スライド上_、_分割ビュー_ (iPad 空気 2、iPad ミニ 4 および iPad Pro のみ) および_の画像に画像_します。 これらの機能について詳しく見て、次のセクションに移動します。

<a name="Slide-Over" />

### <a name="slide-over"></a>経由でスライド ショー

スライド上機能により、2 番目のアプリを選択し、クイック操作を提供する場合は、小さなスライディング パネルに表示します。 スライド上パネルは、一時的なとメインのアプリをもう一度使用するユーザーが戻るときに閉じられます。

[ ![](multitasking-images/about01.png "スライド上パネル")](multitasking-images/about01.png)

重要なは、ユーザーは、サイド バイ サイドと、開発者にこのプロセスが細かく制御が含まれていないことを実行する 2 つのアプリです。 その結果はスライド上のパネルで、Xamarin.iOS アプリが正常に実行することを確認するために必要な点があります。

- **Autolayout とサイズのクラスを使用して**— Xamarin.iOS アプリは、スライド ショー アウト側パネルの 今すぐ実行できる、ため、不要になったを使用すると、デバイス、その画面のサイズまたはレイアウトの方向、UI。 アプリが正しくのインターフェイスを拡張することには、自動レイアウトとサイズのクラスを使用する必要があります。 詳細については、次を参照してください。 この[Unified ストーリー ボードの概要](~/ios/user-interface/storyboards/unified-storyboards.md)ドキュメント。
- **効率的にリソースを使用して**— ためアプリできるようになりました共有すること、システム別の実行中のアプリ、その重要では、アプリがシステム リソースを効率的に使用しています。 メモリには、スパースのようになったら、システムは自動的に最も多くのメモリを消費しているアプリを終了します。 Apple を参照してください[iOS アプリのエネルギー効率ガイド](https://developer.apple.com/library/prerelease/ios/documentation/Performance/Conceptual/EnergyGuide-iOS/index.html#//apple_ref/doc/uid/TP40015243)詳細についてはします。

スライドは iPad Pro、iPad 空気、iPad 空気 2、iPad ミニ 2、iPad ミニ 3、または iPad ミニ 4 でのみ使用できます。 スライド上のアプリを準備する方法の詳細についてを参照してください Apple の[iPad でマルチタスク拡張機能の採用](https://developer.apple.com/library/prerelease/ios/documentation/WindowsViews/Conceptual/AdoptingMultitaskingOniPad/index.html#//apple_ref/doc/uid/TP40015145)ドキュメント。

<a name="Split-View" />

### <a name="split-view"></a>分割ビュー

IPad のサポートされているハードウェア (iPad 空気 2、iPad ミニ 4 および iPad Pro のみ) では、ユーザーは、2 番目のアプリを取得し、分割画面表示モードで現在実行中のアプリでサイド バイ サイド実行します。 ユーザーは、各アプリをドラッグすることで占有するメイン画面の割合を制御できます、画面に表示される境界線です。

[ ![](multitasking-images/about02.png "分割ビュー")](multitasking-images/about02.png)

スライドの上と同様に、ユーザーを決定する 2 つのアプリがサイド バイ サイドを実行する、もう一度、開発者にはこのプロセスが細かく制御はありません。 その結果、分割ビューは、Xamarin.iOS アプリで同様の要件を配置します。

- **Autolayout とサイズのクラスを使用して**— Xamarin.iOS アプリは、ユーザーの指定したサイズで分割画面表示モードで今すぐ実行できる、ため、不要になったを使用すると、デバイス、その画面のサイズまたはレイアウトの方向、UI。 アプリが正しくのインターフェイスを拡張することには、自動レイアウトとサイズのクラスを使用する必要があります。 詳細については、次を参照してください。 この[Unified ストーリー ボードの概要](~/ios/user-interface/storyboards/unified-storyboards.md)ドキュメント。
- **効率的にリソースを使用して**— ためアプリできるようになりました共有すること、システム別の実行中のアプリ、その重要では、アプリがシステム リソースを効率的に使用しています。 メモリには、スパースのようになったら、システムは自動的に最も多くのメモリを消費しているアプリを終了します。 Apple を参照してください[iOS アプリのエネルギー効率ガイド](https://developer.apple.com/library/prerelease/ios/documentation/Performance/Conceptual/EnergyGuide-iOS/index.html#//apple_ref/doc/uid/TP40015243)詳細についてはします。

分割ビュー用アプリの準備に関する詳細についてを参照してください Apple の[iPad でマルチタスク拡張機能の採用](https://developer.apple.com/library/prerelease/ios/documentation/WindowsViews/Conceptual/AdoptingMultitaskingOniPad/index.html#//apple_ref/doc/uid/TP40015145)ドキュメント。

<a name="Picture-in-Picture" />

### <a name="picture-in-picture"></a>図の画像

画像の機能で新しい画像 (とも呼ばれる_PIP_) 小さな、浮動小数点 ウィンドウで、ユーザーの他の実行中のアプリ上の画面で任意の位置できますでビデオを見ることができます。

[ ![](multitasking-images/about03.png "フローティング ウィンドウの画像に画像の使用例")](multitasking-images/about03.png)

スライド上と分割ビューとして、ユーザーはの画像に画像モードでビデオを視聴完全制御できます。 場合は、アプリの主な機能は、ビデオを見るには、いくらか修正 PIP モードで正しく動作する必要があります。 それ以外の場合、PIP をサポートするために変更は必要ありません。

ユーザーの要求に PIP ビデオを表示する、アプリのいずれかを使用する必要がある_AVKit_または_AV Foundation Api_です。 Media Player フレームワークでは、iOS 9 で非推奨になっているし、PIP をサポートしていません。

図の画像は iPad Pro、iPad 空気、iPad 空気 2、iPad ミニ 2、iPad ミニ 3、または iPad ミニ 4 でのみ使用できます。 詳細についてを参照してください、 [PictureInPicture サンプル アプリ](https://developer.xamarin.com/samples/ios/iOS9/)と Apple の[クイック スタートの画像に画像](https://developer.apple.com/library/prerelease/ios/documentation/WindowsViews/Conceptual/AdoptingMultitaskingOniPad/QuickStartForPictureInPicture.html#//apple_ref/doc/uid/TP40015145-CH14)ドキュメント。

<a name="Supporting-Multitasking-in-your-App" />

## <a name="supporting-multitasking-in-your-app"></a>アプリでマルチタスク処理をサポートします。

すべての既存の Xamarin.iOS アプリ マルチタスク処理をサポートするは透過的なタスク、アプリは、既に Apple の設計ガイドおよび iOS 8 のベスト プラクティスに従う限りです。 つまり、アプリ必要がありますを使用することのストーリー ボード Autolayout とサイズのクラスのユーザー インターフェイスのレイアウトの場合 (を参照してください、 [Unified ストーリー ボードの概要](~/ios/user-interface/storyboards/unified-storyboards.md)詳細については)。

これらのアプリのほとんどまたはまったく変更は、マルチタスク処理をサポートするためにおよびも内での動作に必要です。 アプリの UI は、直接位置および c# コードまたは特定のデバイスの画面サイズや向きに依存しているかどうかの UI 要素のサイズなどの他のメソッドを使用して作成されている場合は、iOS 9 マルチタスクを正しくサポートするために大幅な変更を必要になります。

新しい Xamarin.iOS アプリを iOS 9 マルチタスク処理をサポートするには、もう一度 Autolayout とサイズのクラスで、アプリのユーザー インターフェイスのレイアウトのすべてのストーリー ボードを使用し、次のセクションの手順を実装します。

<a name="Screen-Size-Considerations" />

### <a name="screen-size-and-orientation-considerations"></a>画面のサイズと向きに関する考慮事項

IOS 9 の場合は、前に、アプリ agains 特定のデバイスの画面サイズと向きを設計する可能性があります。 アプリは、スライドをパネル、または分割ビュー モードで今すぐ実行できる、ため、それ自体で実行されているいずれかで水平方向のサイズを圧縮または正規クラス、デバイスの物理的な印刷の向きや画面サイズに関係なく、iPad を検索できます。

[ ![](multitasking-images/sizeclasses01.png "画面のサイズと向きに関する考慮事項")](multitasking-images/sizeclasses01.png)

全画面アプリでは、iPad で、正規の水平および垂直方向のサイズのクラスがあります。 すべての Iphone が iPhone 6 Plus と iPhone 6 s Plus は任意の方向、双方向のコンパクト サイズ クラスを設定します。 IPhone 6 Plus と iPhone 6 s Plus 横モードで正規の水平方向サイズ クラスと Compact 垂直方向サイズ クラス (非常によく似た iPad ミニ) があります。

スライド上と分割ビューをサポートする Ipad では、上の次の組み合わせ終了できます。

<table width=100% border="1px">
    <tr>
        <td><b>[方向]</b></td>
        <td><b>プライマリ アプリ</b></td>
        <td><b>セカンダリのアプリ</b></td>
    </tr>
    <tr>
        <td><b>縦向き</b></td>
        <td>画面の 75%<br/>Compact の水平方向<br/>正規の垂直方向</td>
        <td>画面の 25%<br/>Compact の水平方向<br/>正規の垂直方向</td>
    </tr>
    <tr>
        <td><b>ランドス ケープ</b></td>
        <td>画面の 75%<br/>正規の水平方向<br/>正規の垂直方向</td>
        <td>画面の 25%<br/>Compact の水平方向<br/>正規の垂直方向</td>
    </tr>
    <tr>
        <td><b>ランドス ケープ</b></td>
        <td>画面の 50%<br/>Compact の水平方向<br/>正規の垂直方向</td>
        <td>画面の 50%<br/>Compact の水平方向<br/>正規の垂直方向</td>
    </tr>
</table>

この例で[MuliTask](https://developer.xamarin.com/samples/monotouch/ios9/MultiTask/)アプリで横モードで iPad で全画面表示を実行した場合、リストと同時に詳細の表示の両方が表示されます。

[ ![](multitasking-images/sizeclasses03.png "リストと同時に表示される詳細ビュー")](multitasking-images/sizeclasses03.png)

スライド上のパネルで、同じアプリを実行すると場合、コンパクトな水平方向サイズ クラスとしてレイアウトされ、一覧のみが表示されます。

[ ![](multitasking-images/sizeclasses04.png "デバイスが水平方向に表示されるリストのみ")](multitasking-images/sizeclasses04.png)

このような場合、アプリが正しく動作させるには、サイズ クラスと共にの特徴であるコレクションを採用してくださいに準拠している、`IUIContentContainer`と`IUITraitEnvironment`インターフェイスです。 Apple を参照してください[UITraitCollection クラス参照](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UITraitCollection_ClassReference/index.html#//apple_ref/doc/uid/TP40014202)と当社[Unified ストーリー ボードの概要](~/ios/user-interface/storyboards/unified-storyboards.md)についてガイドします。

さらに、使用すると、不要になったアプリの可視領域を定義するデバイスの画面の境界に、代わりに、アプリのウィンドウの境界を使用する必要があります。 ウィンドウの境界は、ユーザーの制御下で完全には、ためそれらを調整またはユーザーがこれらの境界を変更することを防ぐプログラムでできません。

最後に、アプリは、のセットを使用してではなく、起動画面を表示するのストーリー ボード ファイルを使用する必要があります**.png**イメージ ファイルとすべての 4 つのインターフェイス向き (縦向き、上下縦、横左右ランドス ケープ) のサポートスライド上のパネルで、または分割ビュー モードで実行されているのと見なされます。

<a name="Custom-Hardware-Keyboard-Shortcuts" />

### <a name="custom-hardware-keyboard-shortcuts"></a>カスタムのハードウェア キーボード ショートカット

IOS 9 iPad で実行されている、Apple には、ハードウェア キーボードの延長サポートします。 Ipad には、Bluetooth や有線 iOS に固有のキーが含まれている一部のキーボード作成の製造元のキーボードを使用して外部キーボードの基本的なサポートが常に含まれます。

ここで、ios 9 の場合は、アプリは、独自のカスタムのキーボード ショートカットを作成できます。 さらに、いくつかの基本的なキーボード ショートカットがあるような**コマンド C** (コピー)、**コマンド X** (切り取り)、**コマンド V** (貼り付け) と**コマンド-h シフト** (ホーム)、それらには、具体的には書き込まれた応答をされているアプリなし。

**コマンド タブ**を Mac OS と同様に、キーボードからアプリをすばやく切り替えるユーザーを許可するアプリ スイッチャーが表示されます。

[ ![](multitasking-images/keyboard01.png "アプリのスイッチャー")](multitasking-images/keyboard01.png)

ユーザーが押しことができますが iOS 9 アプリには、キーボード ショートカットが含まれている場合、**コマンド**、**オプション**または**コントロール**ポップアップで表示するキー。

[ ![](multitasking-images/keyboard02.png "キーボード ショートカットのポップアップ")](multitasking-images/keyboard02.png)

#### <a name="defining-custom-keyboard-shortcuts"></a>カスタム ショートカット キーを定義します。

追加する場合、次のコード ビューまたはビュー コント ローラー、アプリケーション内でそのビューまたはコント ローラーが表示されるときに、カスタムのキーボード ショートカットが提供されます。

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

最初に、オーバーライド、`CanBecomeFirstResponder`プロパティと戻り`true`ビューまたはビュー コント ローラーは、キーボード入力を受信できるようにします。 

次に、オーバーライド、`KeyCommands`プロパティされ、新しい作成`UIKeyCommand`の**コマンド N**キーを入力します。 呼び出して、キーストロークをアクティブになると、`NewEntry`メソッド (を使用して iOS 9 に公開している、`Export`コマンド)、要求されたアクションを実行します。

IPad で添付ハードウェア キーボードとユーザー タイプでこのアプリを実行お場合**コマンド N**、新しいエントリを一覧に追加されます。 下へ、ユーザーに保持している場合、**コマンド**キー、ショートカットの一覧が表示されます。

[ ![](multitasking-images/keyboard03.png "キーボード ショートカットのポップアップ")](multitasking-images/keyboard03.png)

このサンプルを参照してください[MultiTask アプリ](http://developer.xamarin.com/samples/monotouch/ios9/MultiTask/)実装例についてはします。

<a name="Resource-Management-Considerations" />

### <a name="resource-management-considerations"></a>リソース管理に関する考慮事項

IOS 8 の設計ガイドとベスト プラクティス、既に使用しているアプリ、に対しても効率的なリソースの管理も問題があります。 Ios 9 の場合は、アプリでは、メモリ、CPU、またはその他のシステム リソースを排他的に使用が不要になったがあります。

その結果、システム リソースを効果的に使用する Xamarin.iOS アプリを微調整する必要があります。 またはに直面してメモリ不足の状況で終了します。 これは、マルチタスクの脱退するアプリの場合は true。 同様に、2 台目以降アプリ可能性がありますも実行スライド上パネルまたは 1 秒あたり 60 フレームを下回るに余分なリソースを必要とする、リフレッシュ レートを原因または画像 ウィンドウで画像。

次のユーザーの操作とその関連事項を考慮してください。

- **スライド上のパネルでテキストを入力する**-UI を上にシステム キーボード アプリに入力されたテキストがあるない場合でも表示ようになりましたことができます。 その結果、アプリはキーボードおよびキーボードを非表示) などの通知の表示に応答する必要があります。
- **スライド上のパネルで 2 つ目のアプリを実行して**-新しいアプリは、フォア グラウンドで実行されているとメモリおよび CPU サイクルなどのシステム リソースの既存のアプリと競合しないようになりました。
- **PIP のウィンドウでビデオを再生する**- ないだけでこのウィンドウで、アプリのインターフェイスの一部を含めることができますが、ビデオを起動したアプリが引き続きバック グラウンドで実行されていると、CPU およびメモリ リソースを消費します。

アプリがリソースを効率的に使用していることを確認するには、するには、次の操作を行う必要があります。

- **アプリをプロファイリングする機器と**-メモリ リーク、明白な CPU 使用率と、アプリが止まってメイン スレッドの領域を確認します。
- **状態遷移メソッドへの応答**-、 **<code>appdelegate.cs</code>**ファイルの上書きと状態への応答を入力するか、バック グラウンドから返すアプリなどの方法を変更します。 イメージ、データやビューおよびビュー コント ローラーなどの任意の不要なアセットを解放します。
- **サイド バイ サイド メモリ負荷の高いアプリを使用してテスト**- メモリ処理を要するアプリ (サテライト ビュー モード) ではマップなどでスライド Out および物理的な iOS のハードウェアで分割ビューの両方を使用して、アプリを実行し、両方のアプリが応答性を維持し、クラッシュしないことをテストします。

Apple を参照してください[iOS アプリのエネルギー効率ガイド](https://developer.apple.com/library/prerelease/ios/documentation/Performance/Conceptual/EnergyGuide-iOS/index.html#//apple_ref/doc/uid/TP40015243)リソース管理の詳細についてはします。

<a name="Opting-Out-of-Multitasking" />

## <a name="opting-out-of-multitasking"></a>マルチタスクを無効にします。

Apple では、すべての iOS 9 アプリは、マルチタスク処理をサポートからわかるように、中にある可能性があります、アプリケーションのための特別な理由なりすぎないようにゲームやカメラ アプリを正常に動作する全画面表示を必要とするなど。

Xamarin.iOS アプリのいずれかをスライド パネル、または分割ビュー モードで実行中のオプトアウトを編集、プロジェクトの**Info.plist**ファイルし、チェック**全画面表示の必要があります**:

[ ![](multitasking-images/fullscreen01.png "マルチタスクを無効にします。")](multitasking-images/fullscreen01.png)

> [!IMPORTANT]
> **注:** Opting アウト マルチタスク処理には、スライドまたは分割ビューで実行されてから、アプリが防止されますが、これは**いない**と共に表示からスライドまたは画像のビデオでは画像で実行されてから別のアプリを防ぐため、アプリです。




<a name="Disabling-PIP-Video-Playback" />

### <a name="disabling-pip-video-playback"></a>PIP のビデオ再生を無効にします。

ほとんどの場合、アプリがフローティング ウィンドウの画像に画像の表示、ビデオのコンテンツを再生するユーザーを許可する必要があります。 ただし、ここでこのいないする必要があります、ゲームのカット シーン ビデオなどの状況があります。

オプトアウト PIP ビデオの再生、アプリに次の操作を行います。

 - 使用している場合、`AVPlayerViewController`ビデオを表示する設定、`AllowsPictureInPicturePlayback`プロパティを`false`です。
 - 使用している場合、`AVPlayerLayer`表示するにはビデオのインスタンスを作成しない、`AVPictureInPictureController`です。
 - 使用している場合、`WKWebView`ビデオを表示する設定、`AllowsPictureInPictureMediaPlayback`プロパティを`false`です。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、Xamarin.iOS アプリが実行され、iOS 9 の新しいマルチタスク機能 Ipad で正しく機能することを確認するための手順について説明しました。 さらに、アプリのことが適合マルチタスク処理を無効にするアウトを説明します。



## <a name="related-links"></a>関連リンク

- [iOS 9 のサンプル](https://developer.xamarin.com/samples/ios/iOS9/)
- [マルチタスク (サンプル)](http://developer.xamarin.comhttps://developer.xamarin.com/samples/monotouch/ios9/MultiTask/)
- [統一されたストーリー ボードの概要](~/ios/user-interface/storyboards/unified-storyboards.md)
- [開発者向けの iOS 9](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [IPad でマルチタスク拡張機能を採用します。](https://developer.apple.com/library/prerelease/ios/documentation/WindowsViews/Conceptual/AdoptingMultitaskingOniPad/index.html#//apple_ref/doc/uid/TP40015145)
- [ブログの投稿](https://blog.xamarin.com/using-auto-layouts-for-ios-9-splitview/)
