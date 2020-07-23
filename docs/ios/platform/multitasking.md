---
title: Xamarin の iPad のマルチタスキング
description: iOS 9 では、スライドオーバービューまたは分割ビューを使用して、同時に実行される2つのアプリをサポートしています。 動画を再生するビデオもサポートしています。
ms.prod: xamarin
ms.assetid: 0F2266D7-21FF-404D-A148-0CFDE76B12AA
ms.technology: xamarin-ios
ms.custom: xamu-video
author: davidortinau
ms.author: daortin
ms.date: 03/20/2017
ms.openlocfilehash: b86f3a159a144f02ea13663bfddb41ed0100f740
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86931405"
---
# <a name="multitasking-for-ipad-in-xamarinios"></a>Xamarin の iPad のマルチタスキング

_iOS 9 では、スライドオーバービューまたは分割ビューを使用して、同時に実行される2つのアプリをサポートしています。動画を再生するビデオもサポートしています。_

![分割画面の例](multitasking-images/about02-sml.png) ![画像内の例](multitasking-images/about03-sml.png)

iOS 9 では、特定の iPad ハードウェアで同時に2つのアプリを実行するためのマルチタスキングサポートが追加されています。 IPad のマルチタスキングは、次の機能によってサポートされています。

- [**スライドオーバー**](#Slide-Over) -ユーザーが、現在実行されているメインアプリの約25% をカバーするスライドアウトパネル (言語の方向に基づいて、画面の右側または左側) で2番目の iOS アプリを一時的に実行できるようにします。 スライドショーは、iPad Pro、iPad Air、iPad Air 2、iPad ミニ2、iPad ミニ3、iPad ミニ4でのみ利用できます。
- [**分割ビュー**](#Split-View) -サポートされている ipad ハードウェア (ipad Air 2、ipad ミニ4、ipad Pro のみ) では、ユーザーは2つ目のアプリを選択して、現在実行中のアプリと並べて分割画面モードで実行できます。 ユーザーは、各アプリが占めるメイン画面の割合を制御できます。
- [**画像の画像-ビデオ**](#Picture-in-Picture)コンテンツを再生するアプリでは、iOS デバイスで現在実行されている他のアプリの周囲にある、移動してサイズ変更が可能なウィンドウでビデオを再生できるようになりました。 ユーザーは、このウィンドウのサイズと位置を完全に制御できます。 画像の画像は、iPad Pro、iPad Air、iPad Air 2、iPad ミニ2、iPad ミニ3、iPad ミニ4でのみ使用できます。

[アプリでマルチタスキングをサポート](#Supporting-Multitasking-in-your-App)するには、次のようないくつかの点を考慮する必要があります。

- [画面のサイズと向き](#Screen-Size-Considerations)
- [カスタムハードウェアのキーボードショートカット](#Custom-Hardware-Keyboard-Shortcuts)
- [リソース管理](#Resource-Management-Considerations)

アプリ開発者は、 [PIP ビデオ再生の無効化](#Disabling-PIP-Video-Playback)など、[マルチタスク](#Opting-Out-of-Multitasking)を無効にすることもできます。

この記事では、Xamarin iOS アプリがマルチタスク環境で正常に動作するようにするために必要な手順、またはアプリが最適でない場合にマルチタスクを無効にする方法について説明します。

> [!VIDEO https://youtube.com/embed/GctYAozoLr8]

**IPad 用のマルチタスキングビデオ**

<a name="Multitasking-QuickStart"></a>

## <a name="multitasking-quickstart"></a>マルチタスキングのクイックスタート

**スライドオーバー**ビューまたは**分割ビュー**をサポートするには、アプリで次の操作を行う必要があります。

- IOS 9 (またはそれ以降) に対してビルドされます。
- イメージアセットではなく、その起動画面にストーリーボードを使用します。
- ストーリーボードとサイズのクラスの UI を使用します。
- 4つすべての iOS デバイスの向き (縦、反転、縦、横 & 横) をサポートします。

<a name="Multitasking"></a>

## <a name="about-multitasking-for-ipad"></a>IPad のマルチタスキングについて

iOS 9 では、iPad での新しいマルチタスク機能を提供しています。これには、_スライドオーバー_、_分割ビュー_ (ipad Air 2、iPad ミニ4、ipad Pro のみ) と_画像の画像が含ま_れています。 これらの機能については、次のセクションで詳しく説明します。

<a name="Slide-Over"></a>

### <a name="slide-over"></a>スライドオーバー

スライドオーバー機能を使用すると、ユーザーは2つ目のアプリを選択し、それを小さいスライド式パネルに表示して、すばやく対話することができます。 パネル上のスライドは一時的なものであり、ユーザーが再びメインアプリの操作に戻ると閉じられます。

[![パネル上のスライド](multitasking-images/about01.png)](multitasking-images/about01.png#lightbox)

重要なのは、ユーザーが2つのアプリをサイドバイサイドで実行することと、開発者がこのプロセスを制御できないことを決定することです。 結果として、Xamarin の iOS アプリがパネル上のスライドで正しく動作するようにするために、次のことを行う必要があります。

- **[オートレイアウトとサイズのクラスを使用**する] —スライドアウト側のパネルで Xamarin iOS アプリを実行できるようになったため、デバイス、画面のサイズ、または UI のレイアウトの向きに依存しなくなりました。 アプリによってインターフェイスが適切に拡張されるようにするには、オートレイアウトとサイズのクラスを使用する必要があります。 詳細については、統合されたストーリーボードのドキュメントの[概要に](~/ios/user-interface/storyboards/unified-storyboards.md)関する記事をご覧ください。
- **リソースを効率的に使用**する—アプリは、実行中の別のアプリとシステムを共有できるようになったため、アプリではシステムリソースを効率的に使用することが重要です。 メモリがスパースになると、最も多くのメモリを消費しているアプリがシステムによって自動的に終了されます。 詳細については、「 [IOS アプリ向けの Apple のエネルギー効率ガイド](https://developer.apple.com/library/prerelease/ios/documentation/Performance/Conceptual/EnergyGuide-iOS/index.html#//apple_ref/doc/uid/TP40015243)」を参照してください。

スライドショーは、iPad Pro、iPad Air、iPad Air 2、iPad ミニ2、iPad ミニ3、iPad ミニ4でのみ利用できます。 スライドオーバー用のアプリの準備の詳細については、「iPad のドキュメントでの、Apple[によるマルチタスキングの機能強化の導入](https://developer.apple.com/library/prerelease/ios/documentation/WindowsViews/Conceptual/AdoptingMultitaskingOniPad/index.html#//apple_ref/doc/uid/TP40015145)」を参照してください。

<a name="Split-View"></a>

### <a name="split-view"></a>分割ビュー

サポートされている iPad ハードウェア (iPad Air 2、iPad ミニ4、iPad Pro のみ) では、ユーザーは2つ目のアプリを選択して、現在実行中のアプリと並べて分割画面モードで実行できます。 ユーザーは、画面上の区切り線をドラッグすることによって、各アプリが占めるメイン画面の割合を制御できます。

[![分割ビュー](multitasking-images/about02.png)](multitasking-images/about02.png#lightbox)

スライドオーバーと同様に、ユーザーは2つのアプリを同時に実行することを決定します。開発者はこのプロセスを制御できません。 その結果、分割ビューでは、次のような要件が Xamarin. iOS アプリに配置されます。

- **[オートレイアウトとサイズのクラスを使用する**]: Xamarin iOS アプリは、ユーザーが指定したサイズで分割画面モードで実行できるため、デバイス、画面のサイズ、または UI のレイアウトに依存しなくなります。 アプリによってインターフェイスが適切に拡張されるようにするには、オートレイアウトとサイズのクラスを使用する必要があります。 詳細については、統合されたストーリーボードのドキュメントの[概要に](~/ios/user-interface/storyboards/unified-storyboards.md)関する記事をご覧ください。
- **リソースを効率的に使用**する—アプリは、実行中の別のアプリとシステムを共有できるようになったため、アプリではシステムリソースを効率的に使用することが重要です。 メモリがスパースになると、最も多くのメモリを消費しているアプリがシステムによって自動的に終了されます。 詳細については、「 [IOS アプリ向けの Apple のエネルギー効率ガイド](https://developer.apple.com/library/prerelease/ios/documentation/Performance/Conceptual/EnergyGuide-iOS/index.html#//apple_ref/doc/uid/TP40015243)」を参照してください。

分割ビュー用にアプリを準備する方法の詳細については、iPad ドキュメントの「Apple[によるマルチタスキングの機能強化の導入](https://developer.apple.com/library/prerelease/ios/documentation/WindowsViews/Conceptual/AdoptingMultitaskingOniPad/index.html#//apple_ref/doc/uid/TP40015145)」を参照してください。

<a name="Picture-in-Picture"></a>

### <a name="picture-in-picture"></a>画像の画像

ピクチャの新しい画像機能 ( _PIP_とも呼ばれます) を使用すると、ユーザーは、実行中の他のアプリの上に画面上の任意の場所に配置できる小さなフローティングウィンドウでビデオを視聴できます。

[![Picture フローティングウィンドウの画像の例](multitasking-images/about03.png)](multitasking-images/about03.png#lightbox)

スライドオーバービューおよび分割ビューの場合と同様に、ユーザーは画像内のビデオを画像モードで視聴する操作を完全に制御できます。 アプリの主要な機能がビデオを視聴する場合は、PIP モードで正しく動作するように変更を加える必要があります。 それ以外の場合、PIP をサポートするために変更は必要ありません。

アプリでユーザーの要求に PIP ビデオを表示するには、 _Avkit_または_AV Foundation api_を使用している必要があります。 Media Player framework は、iOS 9 では償却されており、PIP はサポートされていません。

画像の画像は、iPad Pro、iPad Air、iPad Air 2、iPad ミニ2、iPad ミニ3、iPad ミニ4でのみ使用できます。 詳細については、Picture [Inpicture サンプルアプリ](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS9)と Apple の[画像クイックスタートの](https://developer.apple.com/library/prerelease/ios/documentation/WindowsViews/Conceptual/AdoptingMultitaskingOniPad/QuickStartForPictureInPicture.html#//apple_ref/doc/uid/TP40015145-CH14)ドキュメントを参照してください。

<a name="Supporting-Multitasking-in-your-App"></a>

## <a name="supporting-multitasking-in-your-app"></a>アプリでのマルチタスキングのサポート

既存の Xamarin iOS アプリについては、アプリが Apple の設計ガイドと iOS 8 のベストプラクティスに従っている限り、マルチタスクのサポートは透過的なタスクです。 つまり、アプリでは、ユーザーインターフェイスのレイアウトに対して、オートレイアウトとサイズのクラスのストーリーボードを使用する必要があります (詳細については、「統合された[ストーリーボードの概要」を](~/ios/user-interface/storyboards/unified-storyboards.md)参照してください)。

これらのアプリでは、マルチタスキングをサポートし、その中で適切に動作するように変更する必要はほとんどありません。 アプリの UI が、C# コードで UI 要素の直接配置やサイズ変更などの他のメソッドを使用して作成された場合、または特定のデバイスの画面サイズまたは向きに依存している場合は、iOS 9 マルチタスクを正しくサポートするために大幅な変更が必要になります。

新しい Xamarin. iOS アプリで iOS 9 のマルチタスキングをサポートするには、アプリのすべてのユーザーインターフェイスのレイアウトに対して、オートレイアウトとサイズのクラスでストーリーボードを再び使用し、次のセクションの手順を実装します。

<a name="Screen-Size-Considerations"></a>

### <a name="screen-size-and-orientation-considerations"></a>画面のサイズと向きに関する考慮事項

IOS 9 より前では、特定のデバイスの画面サイズと向きに合わせてアプリを設計できました。 アプリは、スライドアウトパネルまたは分割ビューモードで実行できるようになったため、デバイスの物理的な向きや画面のサイズに関係なく、iPad 上のコンパクトまたは標準の水平方向のサイズクラスで実行されていることがわかります。

[![画面のサイズと向きに関する考慮事項](multitasking-images/sizeclasses01.png)](multitasking-images/sizeclasses01.png#lightbox)

IPad では、全画面アプリには、標準の水平方向と垂直方向のサイズのクラスがあります。 IPhone 6 Plus および iPhone 6s Plus では、すべての iPhones が、双方向にコンパクトなサイズクラスを備えています。 IPhone 6 Plus および iPhone 6s Plus と横モードには、通常の水平方向のサイズクラスとコンパクトな垂直方向のサイズクラス (iPad ミニなど) があります。

スライドオーバーおよび分割ビューをサポートする Ipad では、最終的に次の組み合わせを使用できます。

| **細かく** | **プライマリアプリ** | **セカンダリアプリ** |
|--- |--- |--- |
| **縦** |画面の75%<br />水平方向に圧縮<br />標準縦|画面の25%<br />水平方向に圧縮<br />標準縦|
| **横** |画面の75%<br />標準の水平方向<br />標準縦|画面の25%<br />水平方向に圧縮<br />標準縦|
| **横** |画面の50%<br />水平方向に圧縮<br />標準縦|画面の50%<br />水平方向に圧縮<br />標準縦|

例の[MuliTask](https://docs.microsoft.com/samples/xamarin/ios-samples/ios9-multitask)アプリでは、iPad on the ランドスケープモードで全画面表示を実行すると、リストと詳細ビューの両方が同時に表示されます。

[![一覧と詳細ビューが同時に表示されます。](multitasking-images/sizeclasses03.png)](multitasking-images/sizeclasses03.png#lightbox)

パネル上のスライドで同じアプリが実行されている場合は、水平方向のサイズの小さいクラスとしてレイアウトされ、一覧のみが表示されます。

[![デバイスが横にあるときに表示されるリストのみ](multitasking-images/sizeclasses04.png)](multitasking-images/sizeclasses04.png#lightbox)

このような状況でアプリが正しく動作するようにするには、特徴コレクションをサイズクラスと共に採用し、インターフェイスとインターフェイスに準拠させる必要があり `IUIContentContainer` `IUITraitEnvironment` ます。 詳細については、「Apple の[Uitraitcollection クラスリファレンス](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UITraitCollection_ClassReference/index.html#//apple_ref/doc/uid/TP40014202)」と「[統合ストーリーボード](~/ios/user-interface/storyboards/unified-storyboards.md)ガイドの概要」を参照してください。

さらに、アプリの表示領域を定義するためにデバイスの画面境界に依存しなくなりました。代わりに、アプリのウィンドウ境界を使用する必要があります。 ウィンドウの境界はユーザーの制御下にあるため、プログラムを使用して調整したり、ユーザーがこれらの境界を変更できないようにしたりすることはできません。

最後に、アプリは、一連の .png イメージファイルを使用するのではなく、ストーリーボードファイルを使用して起動画面を表示する必要があります **。** また、4つのインターフェイスの向き (縦、反転、縦、横、横) をすべてサポートして、パネル上または分割ビューモードで実行することを検討します。

<a name="Custom-Hardware-Keyboard-Shortcuts"></a>

### <a name="custom-hardware-keyboard-shortcuts"></a>カスタムハードウェアのキーボードショートカット

IPad で実行されている iOS 9 では、Apple はハードウェアキーボードのサポートを拡張しています。 Ipad には、Bluetooth 経由の基本的な外部キーボードサポートが常に含まれています。また、一部のキーボード製造元は、iOS 固有のハードワイヤードキーを含むキーボードを作成しました。

IOS 9 では、アプリで独自のカスタムキーボードショートカットを作成できます。 また、基本的なキーボードショートカットの中には、**コマンド C** (copy)、**コマンド-X** (切り取り)、**コマンド V** (貼り付け)、**コマンドラインシフト-H** (ホーム) などのようなものもあります。このショートカットキーは、特にアプリケーションが応答しません。

次の**コマンド**を実行すると、Mac OS のように、ユーザーがキーボードからアプリをすばやく切り替えることができるアプリスイッチャーが表示されます。

[![アプリスイッチャー](multitasking-images/keyboard01.png)](multitasking-images/keyboard01.png#lightbox)

IOS 9 アプリにキーボードショートカットが含まれている場合、ユーザーは**コマンド**、**オプション**、または**制御**キーを押して、ポップアップに表示できます。

[![キーボードショートカットのポップアップ](multitasking-images/keyboard02.png)](multitasking-images/keyboard02.png#lightbox)

#### <a name="defining-custom-keyboard-shortcuts"></a>カスタムショートカットキーの定義

アプリのビューコントローラーまたはビューコントローラーに次のコードを追加すると、そのビューまたはコントローラーが表示されるときに、カスタムキーボードショートカットが使用できるようになります。

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

まず、プロパティをオーバーライド `CanBecomeFirstResponder` し、 `true` ビューまたはビューコントローラーがキーボード入力を受け取ることができるように戻ります。 

次に、プロパティをオーバーライド `KeyCommands` し、 `UIKeyCommand` **コマンド N**キーストローク用に新しいを作成します。 キーストロークがアクティブになったら、 `NewEntry` (コマンドを使用して iOS 9 に公開する) メソッドを呼び出して、 `Export` 要求されたアクションを実行します。

ハードウェアキーボードが接続されている iPad でこのアプリを実行し、ユーザーが**コマンド**を入力すると、新しいエントリが一覧に追加されます。 ユーザーが**コマンド**キーを押したままになっている場合は、ショートカットの一覧が表示されます。

[![キーボードショートカットのポップアップ](multitasking-images/keyboard03.png)](multitasking-images/keyboard03.png#lightbox)

実装の例については、サンプルのマルチ[タスキングアプリ](https://docs.microsoft.com/samples/xamarin/ios-samples/ios9-multitask)を参照してください。

<a name="Resource-Management-Considerations"></a>

### <a name="resource-management-considerations"></a>リソース管理に関する考慮事項

IOS 8 の設計ガイドとベストプラクティスを既に使用しているアプリの場合でも、効率的なリソース管理は依然として問題になる可能性があります。 IOS 9 では、アプリはメモリ、CPU、またはその他のシステムリソースを排他的に使用しなくなりました。

そのため、システムリソースを効果的に使用するために Xamarin の iOS アプリを微調整するか、メモリ不足の状況下で終了する必要があります。 これは、マルチタスクを無効にするアプリにも当てはまります。これは、2つ目のアプリケーションがパネル上のスライドで実行されるか、または画像ウィンドウの画像によって追加のリソースが必要になるか、またはリフレッシュレートが60フレーム/秒未満になる可能性があるためです。

次のユーザー操作とその影響について考えてみましょう。

- **パネル上のスライドにテキストを入力**する-アプリにテキスト入力がない場合でも、システムキーボードを UI の上に表示できるようになりました。 その結果、アプリはキーボード表示通知 (キーボードの表示や非表示など) に応答する必要がある場合があります。
- **パネル上のスライドで2つ目のアプリを実行**する-新しいアプリはフォアグラウンドで実行され、メモリや CPU サイクルなどのシステムリソースの既存のアプリと競合します。
- **PIP ウィンドウでビデオを再生**する-このウィンドウがアプリのインターフェイスの一部をカバーするだけでなく、ビデオを起動したアプリがバックグラウンドで実行されていて、CPU とメモリのリソースを消費している場合もあります。

アプリがリソースを効率的に使用していることを確認するには、次の手順を実行する必要があります。

- **インストルメント化されたアプリのプロファイル**-メモリリーク、overt CPU 使用率、アプリがメインスレッドをブロックしている可能性がある領域を確認します。
- **状態遷移メソッドに応答**します。 **AppDelegate.cs**ファイルでは、アプリがバックグラウンドで入力または返すなどの状態変更メソッドに対してオーバーライドおよび応答を行います。 イメージ、データ、ビュー、ビューコントローラーなど、不要なアセットを解放します。
- **メモリを集中的に使用するアプリとのサイドバイサイドのテスト**-マップ (サテライトビューモード) などのメモリを集中的に使用するアプリを使用して、物理 iOS ハードウェア上のスライドアウトと分割ビューの両方を使用してアプリを実行し、両方のアプリの応答性が維持され、クラッシュしないことをテストします。

リソース管理の詳細については、「 [IOS アプリ用の Apple のエネルギー効率ガイド](https://developer.apple.com/library/prerelease/ios/documentation/Performance/Conceptual/EnergyGuide-iOS/index.html#//apple_ref/doc/uid/TP40015243)」を参照してください。

<a name="Opting-Out-of-Multitasking"></a>

## <a name="opting-out-of-multitasking"></a>マルチタスキングの無効化

Apple は、すべての iOS 9 アプリでマルチタスキングがサポートされていることを示唆していますが、全画面を正しく動作させる必要があるゲームやカメラアプリなど、アプリについても非常に具体的な理由が考えられます。

Xamarin iOS アプリが、スライドアウトパネルまたは分割ビューモードで実行されないようにするには、プロジェクトの**情報 plist**ファイルを編集し、[**全画面表示が必要]** チェックボックスをオンにします。

[![マルチタスキングの無効化](multitasking-images/fullscreen01.png)](multitasking-images/fullscreen01.png#lightbox)

> [!IMPORTANT]
> マルチタスクを無効にしても、アプリがスライドアウトまたは分割ビューで実行されるのを防ぐことはできませんが、別のアプリがスライドアウトしたり、画像の画像がアプリと共に表示されたりするのを防ぐことはできません。

<a name="Disabling-PIP-Video-Playback"></a>

### <a name="disabling-pip-video-playback"></a>PIP ビデオ再生の無効化

ほとんどの場合、アプリは画像に表示されているビデオコンテンツを、画像のフローティングウィンドウに再生することをユーザーに許可する必要があります。 ただし、game cut シーンビデオなど、このような状況が望ましくない場合もあります。

PIP ビデオの再生を無効にするには、アプリで次の操作を行います。

- を使用してビデオを表示する場合は、 `AVPlayerViewController` `AllowsPictureInPicturePlayback` プロパティをに設定し `false` ます。
- を使用してビデオを表示している場合は、を `AVPlayerLayer` インスタンス化しないで `AVPictureInPictureController` ください。
- を使用してビデオを表示する場合は、 `WKWebView` `AllowsPictureInPictureMediaPlayback` プロパティをに設定し `false` ます。

<a name="Summary"></a>

## <a name="summary"></a>まとめ

この記事では、Ipad の新しいマルチタスキング機能である Xamarin iOS アプリを正常に動作させるために必要な手順について説明しました。 さらに、最適ではないアプリのマルチタスクのオプトアウトについても説明しています。

## <a name="related-links"></a>関連リンク

- [iOS 9 のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS9)
- [マルチタスク (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios9-multitask)
- [統合されたストーリーボードの概要](~/ios/user-interface/storyboards/unified-storyboards.md)
- [iOS 9 (開発者向け)](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [IPad でのマルチタスキング機能強化の採用](https://developer.apple.com/library/prerelease/ios/documentation/WindowsViews/Conceptual/AdoptingMultitaskingOniPad/index.html#//apple_ref/doc/uid/TP40015145)
- [ブログ投稿](https://blog.xamarin.com/using-auto-layouts-for-ios-9-splitview/)
