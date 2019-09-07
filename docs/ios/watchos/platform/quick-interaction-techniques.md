---
title: Xamarin の watchOS 3 のクイック対話技法
description: この記事では、Apple が watchOS 3 で追加したクイック対話手法と、それらを Xamarin に実装する方法 (Apple Watch) について説明します。
ms.prod: xamarin
ms.assetid: 26697F68-AF7E-4A36-988F-85E2674A4DD1
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/17/2017
ms.openlocfilehash: 6aa5eede658f13a36220398f92192eefa2473bab
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70768589"
---
# <a name="quick-interaction-techniques-for-watchos-3-in-xamarin"></a>Xamarin の watchOS 3 のクイック対話技法

_この記事では、Apple が watchOS 3 で追加したクイック対話手法と、それらを Xamarin に実装する方法 (Apple Watch) について説明します。_

ユーザーとの対話を簡単に行うには、説得力のある Apple Watch アプリや複雑さを作成することが不可欠です。 WatchOS 3 の新機能である Apple では、ジェスチャレコグナイザー、Digital Crown へのアクセス、および新しいユーザー通知およびナビゲーション手法のサポートが追加されました。 これに加えて、SceneKit と SpriteKit の両方のサポートが追加されたため、開発者は迅速かつ迅速に対応できる豊富なインターフェイスを簡単に作成できます。

## <a name="what-are-quick-interactions"></a>クイック操作とは

IOS または macOS 用のアプリケーションを作成するために使用される開発者向け (ユーザーがアプリとの対話に費やした時間が分単位または時間単位で測定される場合)、Apple Watch に対してアプリを正常に設計することは困難であり、異なるしたがっ.

WatchOS では、通常、ユーザーは手首を起動し、アプリをすばやく対話 (通常は数秒) してから、その手首をドロップして、実行していたものを削除したいと考えています。

次に、Apple Watch の一般的なクイック操作の例をいくつか示します。

- タイマーを開始しています。
- 天気を確認しています。
- Todo リストに項目をマークします。

これらの目標を達成するには、Apple Watch 上のアプリが次のようになっている必要があります。

- これは、ユーザーが必要な情報を一目で**把握できるよう**にすることを意味します。 
- 実行可能-ユーザーは、**十分な情報**に基づいた迅速な意思決定を行うことができることを意味します。
- **応答性**-ユーザーは必要な情報を受信したり、必要な操作を実行したりすることができないことを意味します。

### <a name="quick-interactions-length"></a>クイック操作の長さ

Apple Watch アプリの性質上、Apple は、クイック操作の理想的な長さが2秒以下であることを示唆しています。 この2秒間の制限のため、開発者は Apple Watch アプリの設計と実装の両方にかなりの時間を費やす必要があります。 

## <a name="new-watchos-3-features-and-apis"></a>New watchOS 3 の機能と Api

Apple は、開発者が Apple Watch アプリに迅速な対話を追加するのを支援するために、WatchKit にいくつかの新機能と Api を追加しました。

- watchOS 3 では、次のような新しい種類のユーザー入力にアクセスできます。
  - ジェスチャレコグナイザー
  - Digital Crown ローテーション 
- watchOS 3 には、次のような情報を表示および更新する新しい方法が用意されています。
  - 強化されたテーブルのナビゲーション
  - 新しいユーザー通知フレームワークのサポート
  - SpriteKit と SceneKit の統合

これらの新機能を実装することにより、開発者は、watchOS 3 アプリが、実用的で、応答性の高いものであることを確認できます。

### <a name="gesture-recognizer-support"></a>ジェスチャ認識エンジンのサポート

開発者が iOS でジェスチャレコグナイザーを実装している場合は、watchOS 3 でのジェスチャレコグナイザーのしくみについてよく理解している必要があります。 更新するために、ジェスチャレコグナイザーは、低レベルのタッチイベントを認識可能な定義済みのジェスチャに解析するオブジェクトです。

watchOS 3 では、次の4つのジェスチャレコグナイザーがサポートされます。

- 離散ジェスチャの種類:
  - スワイプジェスチャ (`WKSwipeGestureRecognizer`)。
  - Tap ジェスチャ (`WKTapGestureRecognizer`)。
- 連続するジェスチャの種類:
  - パンジェスチャ (`WKPanGestureRecognizer`)。
  - 長押しジェスチャ (`WKLongPressGestureRecognizer`)。

新しいジェスチャレコグナイザーを実装するには、Visual Studio for Mac の iOS デザイナーのデザイン画面にドラッグして、そのプロパティを構成するだけです。

コードでは、ユーザーによってトリガーされるジェスチャを処理するために、レコグナイザーのアクションに応答します。 ここでも、これは iOS で処理されるのと同じ方法で行われます。

#### <a name="discrete-gesture-states"></a>離散ジェスチャの状態

離散ジェスチャでは、ジェスチャが認識され、状態 (`WKGestureRecognizerState`) が次のように割り当てられると、アクションが呼び出されます。

[![](quick-interaction-techniques-images/quick01.png "離散ジェスチャの状態")](quick-interaction-techniques-images/quick01.png#lightbox)

すべての離散ジェスチャは、 `Possible`状態で開始され、状態`Failed`または`Recognized`状態に遷移します。 離散ジェスチャを使用する場合、開発者は通常、状態を直接処理しません。 代わりに、ジェスチャが認識されるときに呼び出されるアクションに依存します。

#### <a name="continuous-gesture-states"></a>連続するジェスチャの状態

連続ジェスチャは、ジェスチャが認識されるときにアクションが複数回呼び出される不連続のジェスチャとは若干異なります。

[![](quick-interaction-techniques-images/quick02.png "連続するジェスチャの状態")](quick-interaction-techniques-images/quick02.png#lightbox)

この場合も、連続ジェスチャは`Possible`状態で開始されますが、複数の更新プログラムが進行します。 ここでは、開発者が認識エンジンの状態を考慮し、ジェスチャが最後`Changed` `Recognized`または`Canceled`になるまで、フェーズ中にアプリの UI を更新する必要があります。

#### <a name="gesture-recognizer-usage-tips"></a>ジェスチャレコグナイザーの使用方法に関するヒント

Apple では、watchOS 3 でジェスチャレコグナイザーを使用するときに次のことを提案しています。

- ジェスチャレコグナイザーを個々のコントロールではなくグループ要素に追加します。 Apple Watch の物理的な画面サイズが小さいため、グループの要素は、ユーザーがヒットするよりも大きく、より簡単なターゲットになる傾向があります。 また、ジェスチャレコグナイザーは、ネイティブ UI コントロールに既に組み込まれているジェスチャと競合する可能性があります。
- Watch アプリのストーリーボードに依存関係を設定します。
- ジェスチャの中には、次のようなジェスチャの種類が優先されるものがあります。
  - スクロール
  - Force Touch

### <a name="digital-crown-rotation"></a>Digital Crown ローテーション

WatchOS 3 アプリにサポート Digital Crown を実装することにより、開発者はユーザーのナビゲーション速度と精度を向上させることができます。

WatchOS 2 以降、Apple Watch アプリは`WKInterfacePicker` 、オブジェクトを使用して、とピッカースタイル (リスト、積み上げ、またはイメージシーケンス) の`WKPickerItems`リストを提供することによって Digital Crown にアクセスできます。 watchOS は、ユーザーが Digital Crown を使用して一覧から項目を選択できるようにしました。

を使用`WKInterfacePicker`する場合、WatchKit はほとんどの作業を処理します。

- リストと個々のインターフェイス要素を描画します。
- Digital Crown イベントの処理。
- 項目が選択されたときにアクションを呼び出します。

WatchOS 3 を初めて使用する場合、開発者は、ローテーションの値に応答する独自の UI 要素を作成するための、Digital Crown ローテーションイベントに直接アクセスできるようになりました。

Digital Crown アクセスは、次の要素によって提供されます。

- `WKCrownSequencer`-1 秒あたりの回転へのアクセスを提供します。
- `WKCrownDelegate`-回転デルタイベントへのアクセスを提供します。

#### <a name="rotations-per-second"></a>1秒あたりの回転数

Digital Crown からの1秒あたりの回転数へのアクセスは、物理アニメーションを使用する場合に便利です。 1秒あたりの回転数にアクセスする`CrownSequencer`には、 `WKInterfaceController` Watch 拡張機能ののプロパティを使用します。 例えば:

```csharp
var rotationsPerSecond = CrownSequencer.RotationsPerSecond;
```

#### <a name="rotational-deltas"></a>デルタの回転

回転数をカウントするには、Digital Crown からの回転デルタを使用します。 の override メソッドを使用し`WKCrownDelegate`て、回転デルタにアクセスします。 `CrownDidRotate` 例えば:

```csharp
using System;
using WatchKit;
using Foundation;

namespace MonkeyWatch.MonkeySeeExtension
{
  public class CrownDelegate : WKCrownDelegate
  {
    #region Computed Properties
    public double AccumulatedRotations { get; set;}
    #endregion

    #region Constructors
    public CrownDelegate ()
    {
    }
    #endregion

    #region Override Methods
    public override void CrownDidRotate (WKCrownSequencer crownSequencer, double rotationalDelta)
    {
      base.CrownDidRotate (crownSequencer, rotationalDelta);

      // Accumulate rotations
      AccumulatedRotations += rotationalDelta;
    }
    #endregion
  }
}
```

ここでは、アプリケーションはアキュムレータ`AccumulatedRotations`() を保持して、回転の回数を決定します。 Digital Crown の1つの完全ローテーションは、の`1.0`累積デルタと同じであり、半ローテーション`0.5`はになります。

Apple では、更新される UI 要素に対する変更の感度に応じてローテーション数がどのように対応するかを決定するために、開発者に代わっています。

回転デルタの`+/-`符号 () は、ユーザーが Digital Crown をオンにする方向を示します。

[![](quick-interaction-techniques-images/quick03.png "回転デルタの符号は、ユーザーが旋回する方向を示し Digital Crown")](quick-interaction-techniques-images/quick03.png#lightbox)

ユーザーがスクロールしている場合、WatchKit は正のデルタを返し、スクロールすると負のデルタが返されます。ユーザーがどの向きを使用しているかは関係ありません。

#### <a name="digital-crown-focus"></a>Digital Crown フォーカス

他のインターフェイス要素と同様に、Digital Crown にフォーカスの概念があります。 このフォーカスは、ユーザーがどのようにウォッチを操作しているかに基づいて、Digital Crown から他のインターフェイス要素に移すことができます。 

たとえば、次のコントロールのいずれかが Digital Crown のフォーカスを盗み出す可能性があります。

- ピッカー
- スライダー
- コントローラーのスクロール

開発者は、カスタムインターフェイス要素が Digital Crown のフォーカスである必要があるかどうかを判断します。 Apple では、新しいジェスチャレコグナイザーを使用して、カスタム UI 要素にフォーカスを移すことを提案しています。

### <a name="vertical-paging"></a>垂直方向のページング

WatchOS アプリでテーブルビューを移動するには、標準的な方法として、目的のデータをスクロールし、特定の行をタップして詳細ビューを表示します。詳細を確認したら、[戻る] ボタンをタップし、y はテーブル内からの関心があります。

[![](quick-interaction-techniques-images/quick04.png "テーブルと詳細ビューの間の移動")](quick-interaction-techniques-images/quick04.png#lightbox)

WatchOS 3 を初めて使用する場合、開発者はテーブルビューコントロールで垂直ページングを有効にすることができます。 この機能を有効にすると、スクロールしてテーブルビュー行を検索し、行をタップしてその詳細を前と同じように表示できます。 ただし、テーブルビューに戻ることなく、テーブル内の次の行を選択して前の行を選択する (または Digital Crown を使用する) ことができるようになりました。

[![](quick-interaction-techniques-images/quick05.png "テーブルと詳細ビューの間を移動し、他の行の間を移動するためにスワイプして下に移動する")](quick-interaction-techniques-images/quick05.png#lightbox)

このモードを有効にするには、Xcode で watchOS アプリのストーリーボードを開いて編集し、テーブルビューを選択して、 **[垂直方向の詳細ページング]** チェックボックスをオンにします。

[![](quick-interaction-techniques-images/quick06.png "垂直方向の詳細ページングのチェックボックス")](quick-interaction-techniques-images/quick06.png#lightbox)

テーブルでセグエを使用して詳細ビューを表示し、変更をストーリーボードに保存し、Visual Studio for Mac に戻して同期することを確認します。

開発者は、テーブルビューに対して次のコードを使用して、特定の行にプログラムで垂直ページングを行うことができます。

```csharp
// Segue into Vertical Paging and select the first row
MenuTable.PerformSegue (0);
```

垂直ページングを使用する場合、開発者は WatchKit がコントローラーのプリロードを自動的に処理することを認識する必要があります。その結果、UI が実際に表示される前に、一部のコントローラーライフサイクルメソッドを呼び出すことができます。

### <a name="notification-enhancements"></a>通知の機能強化

通知は、ユーザーが通常は watchOS を使用し、最初の Apple Watch と watchOS 1 以降で使用できるようにする、クイック操作の主な形式です。

一般的な通知のクイック操作は次のとおりです。

1. 新しい通知が受信されると、ユーザーは Notification Haptic を感じます。
2. これらのユーザーは手首を上げて、通知の短い外観インターフェイスを確認します。
3. これらのユーザーが手首の発生を継続し続けると、watchOS は長いルック通知インターフェイスに自動的に遷移します。

ユーザーが通知に応答するには、次のようないくつかの方法があります。

- 明確に定義されて表示される通知については、ユーザーは何もせず、単に通知を無視します。
- また、通知をタップして watchOS アプリを起動することもあります。
- カスタムアクションをサポートする通知の場合、ユーザーはカスタムアクションの1つを選択できます。 次のいずれかを指定できます。
  - **フォアグラウンドアクション**-アプリを起動してアクションを実行します。
  - **バックグラウンドアクション**-常に watchOS 2 の iPhone にルーティングされていましたが、watchOS 3 の watchApp にルーティングできます。

WatchOS 3 の新:

- 通知では、すべてのプラットフォーム (iOS、watchOS、tvOS、macOS) で同様の API を使用します。
- ローカル通知は、Apple Watch でスケジュールできます。
- Apple Watch でスケジュールされている場合、バックグラウンド通知はアプリの拡張機能にルーティングされます。

#### <a name="notification-scheduling-and-delivery"></a>通知のスケジュールと配信

ユーザーの iPhone からの通知は、次の場合に Apple Watch に転送されます。

- IPhone の画面がオフになっています。
- Apple Watch は、ロック解除されています。

WatchOS 3 では、ローカル通知は Apple Watch でスケジュールでき、ウォッチでのみ配信されます。 アプリで必要な場合は、対応する iPhone の通知をスケジュールすることをお勧めします。

Apple Watch と iPhone の両方のバージョンの通知に同じ通知識別子を含めることによって、監視に重複する通知が表示されないようにすることができます。 通知の Apple Watch バージョンは、iPhone のバージョンよりも優先されます。

WatchOS 3 では ios 10 `UINotification`と同じ API フレームワークを使用しているため、詳細については、ios 10[ユーザー通知フレームワーク](~/ios/platform/user-notifications/index.md)のドキュメントを参照してください。

### <a name="using-spritekit-and-scenekit"></a>SpriteKit と SceneKit の使用

WatchOS 3 を初めて使用すると、開発者は、アプリのユーザーインターフェイスのデザインで SpritKit と SceneKit の両方のオブジェクトを使用して、2D と3D の両方のグラフィックを表示できるようになりました。

この機能をサポートするために、次の2つの新しいインターフェイスクラスが追加されました。

- `WKInterfaceSKScene`-SpriteKit 2D グラフィックスを操作する場合。
- `WKInterfaceSCNScene`-SceneKit 3D グラフィックスを操作する場合。

これらのオブジェクトを使用するには、Xcode の Interface Builder の watch アプリのストーリーボード内のデザイン画面にドラッグし、**属性インスペクター**を使用してこれらのオブジェクトを構成します。

この時点から、SpriteKit または SceneKit のいずれかのシーンの操作は、iOS アプリの内部と同じように動作します。 Watch アプリでは、 `WKInterfaceSKScene` `Present`メソッドのいずれかを呼び出すことによってが表示されます。 SceneKit の場合は、 `Scene` `WKInterfaceSCNScene`オブジェクトのプロパティを設定するだけです。

## <a name="actionable-complications"></a>実行可能な複雑さ

WatchOS 2 では、Apple はサードパーティ製アプリの複雑さを導入しました。 WatchOS 3 では、Apple は、開発者が WatchKit の複雑な機能を組み込むことができる機能を拡張しました。 

さらに、組み込まれている watch の顔の中には、複雑なものや、既にサポートされているものが含まれています。

また、新しい機能は、ユーザーが Apple Watch にインストールされているすべてのウォッチフェイスをすばやく左右にスワイプできるようにする機能です。 Apple Watch のコンパニオン iPhone アプリで新しいギャラリーを使用することで、ユーザーは新しいウォッチフェイスや、含めることのできる複雑さを追加およびカスタマイズできます。

これらの新機能により、Apple Watch のすべてのアプリに少なくとも1つの複雑なアプリが含まれるようになり、ネイティブ Apple Watch アプリのすべてに複雑さがあるようになりました。

複雑さは、アプリに次の機能を提供します。

- これらは常にウォッチ式に存在するため、非常に優れています。
- 複雑さは、watchOS によって頻繁に更新されます。 ユーザーの現在表示されているウォッチ式を含むすべてのアプリは、少なくとも1時間に2回更新されます。
- ユーザーの現在表示されているウォッチ式を使用しているアプリはメモリに保持されるため、アプリがすばやく起動し、アプリからの応答速度が向上します。
- 複雑さによって、ユーザーは watchOS アプリで特定の機能を簡単に起動できます。

## <a name="glanceable-notification"></a>実現可能な通知

Apple Watch に対する通知は、ユーザーにイベントや新しい情報 (受信メッセージなど) をすばやく通知したり、トレーニングアプリで目標を達成したりするための、優れたカスタマイズ可能な方法を提供します。

通知を使用すると、重要な情報をすぐにユーザーに表示できます。 多くの場合、適切に設計された通知を使用すると、ユーザーがアプリを実際に起動するために必要なことを取り除くことができます。

WatchOS 3 の新機能では、すべての通知がサポートされるようになりました。

- SpriteKit
- SceneKit
- インラインビデオ

## <a name="enhanced-ui-with-spritekit-and-scenekit"></a>SpriteKit と SceneKit を使用した UI の強化

通常、開発者は、SpriteKit と SceneKit が記載されている場合、ゲーム UI を考えることができます。 ただし、SpriteKit と SceneKit はどちらも、WatchKit 単独では不可能なカスタマイズされたレイアウト、コンテンツ、およびアニメーションを含む、ゲーム以外の Ui の作成に役立ちます。

たとえば、写真共有アプリからのユーザー通知では、SpriteKit を使用して、画像を投稿したユーザーと、実際の画像や、ユーザーエクスペリエンスを強化するその他のカスタマイズされた情報を含めることにより、豊富なユーザーエクスペリエンスを提供できます。

さらに、SpriteKit と SceneKit の両方を、アプリのユーザーインターフェイスの設計で、標準の WatchKit UI 要素と組み合わせて使用することもできます。

## <a name="simple-navigation"></a>単純なナビゲーション

watchOS 3 では、新しい[垂直ページング](#vertical-paging)、[ジェスチャレコグナイザーサポート](#gesture-recognizer-support)、上に示した[Digital Crown ローテーション](#digital-crown-rotation)機能など、watchOS アプリ内でのナビゲーションを簡略化するために開発者が行うことができるいくつかの方法が紹介されています。

Digital Crown は Apple Watch に固有であり、ナビゲーションを簡略化するためにさまざまな方法で使用できます。 たとえば、タイマーアプリケーションは、Digital Crown を使用して、使用可能なタイマーの長さをスクラブできます。

カスタムジェスチャでは、ユーザーが watch アプリと対話するための新しい独自の方法が提供され、アプリのナビゲーションを簡素化するためにも使用できます。

Apple では、watchOS 3 に追加されたすべての新しいクイック対話機能を組み合わせて、リッチで使いやすい watchOS アプリインターフェイスを提供する方法をお勧めします。

## <a name="finishing-the-quick-interaction"></a>クイック操作の完了

適切に設計されたクイック操作エクスペリエンスを使用すると、ユーザーは現在の操作を完了したときに、手首 (およびアプリを使用して停止) を削除できます。

特に問題になるのは、watch アプリが任意の種類のネットワーク接続を実行している場合や、関連する iPhone アプリで情報を共有している場合です。 これにより、トランザクションの実行中に待機インジケーターが生じることがよくあります。これは、迅速なやり取りの際には望ましくありません。 次の例を参照してください。

[![](quick-interaction-techniques-images/quick07.png "ネットワーク接続を実行し、関連する iPhone アプリで情報を共有している watch アプリの図")](quick-interaction-techniques-images/quick07.png#lightbox)

1. ユーザーは、ウォッチで購入する項目を選択します。
2. [購入] ボタンをタップします。
3. アプリによってネットワークトランザクションが開始され、読み込みインジケーターが表示されます。
4. しばらくすると、トランザクションが完了し、アプリに購入オファー確認が表示されます。
5. ユーザーは、手首と disengages をアプリにドロップします。

ユーザーは、トランザクションが完了するまで [購入] ボタンをタップしたときに、読み込みインジケーターを見て、手首が表示されます。 この状況を解決するために、Apple は、開発者が読み込みインジケーターを表示するのではなく、ユーザーに即時フィードバックを提示することを提案します。 

Apple の提案されたモデルを使用して、同じクイック対話をもう一度見てみましょう。

[![](quick-interaction-techniques-images/quick08.png "リンゴの提案モデルの図")](quick-interaction-techniques-images/quick08.png#lightbox)

1. ユーザーは、ウォッチで購入する項目を選択します。
2. [購入] ボタンをタップします。
3. アプリによってネットワークトランザクションが開始され、購入が正常に開始されたことを伝えるメッセージが表示されます。
4. ユーザーは、手首と disengages をアプリにドロップします。
5. トランザクションが後でしばらく完了すると、アプリはローカル通知を表示して、ユーザーに購入が成功したことを通知します。

このとき、ユーザーが [購入] ボタンをタップするとすぐに、購入が開始されたことを示すメッセージが表示されます。そのため、この時点では、手首を自信を持ってドロップし、迅速な対話を終了できます。 その後、ユーザー通知でトランザクションが成功したか失敗したかが通知されます。 このようにして、ユーザーはプロセスの "アクティブ" フェーズ中にのみアプリと対話します。

ネットワークを使用しているアプリでは、バックグラウンド`NSURLSession`を使用して、ダウンロードタスクを使用したネットワーク通信を処理することができます。 これにより、アプリをバックグラウンドでウェイクアップして、ダウンロードした情報を処理できます。 バックグラウンド処理を必要とするアプリでは、バックグラウンドタスクアサーションを使用して必要な処理を処理します。

## <a name="quick-interaction-design-tips"></a>クイック操作のデザインのヒント

クイック操作の目的の長さは2秒以下であるため、開発者は、設計プロセスの最初からのアプリの相互作用の設計に焦点を当てる必要があります。 (上記の手法を使用して) これらの相互作用を簡略化できる領域を検索し、watchOS 3 の新機能を使用して、アプリの応答性を向上させることができます。

Apple は次のことを提案します。

- アプリの最もよく使用される機能を利用することで、迅速なやり取りに専念できます。
- 煩雑さとユーザー通知を使用して、共通の機能と機能を公開します。
- SceneKit と SpriteKit を使用して、多彩な機能を備えた充実したユーザーインターフェイスを作成できます。
- 可能な限り、アプリ内でのナビゲーションを簡略化します。
- ユーザーによる待機をしないでください。ユーザーが手首をドロップし、アプリを使用して直ちにオフにすることを許可します。

## <a name="summary"></a>Summary

この記事では、Apple が watchOS 3 で追加したクイック対話手法と、それらを Xamarin に実装する方法 (Apple Watch) について説明しました。

## <a name="related-links"></a>関連リンク

- [watchOS のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+watchOS)
