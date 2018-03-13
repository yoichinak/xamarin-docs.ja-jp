---
title: "WatchOS 3 のクイックの相互作用手法"
description: "この記事は、クイック相互作用手法を説明 Apple が watchOS 3 および Apple Watch の Xamarin.iOS でそれらを実装する方法で追加します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 26697F68-AF7E-4A36-988F-85E2674A4DD1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: bf93744914a0caf4f6599fc333ae200468d66e48
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="quick-interaction-techniques-for-watchos-3"></a>WatchOS 3 のクイックの相互作用手法

_この記事は、クイック相互作用手法を説明 Apple が watchOS 3 および Apple Watch の Xamarin.iOS でそれらを実装する方法で追加します。_

提供する簡易ユーザー操作は、説得力のある Apple Watch アプリや複雑さの一部を作成するのに不可欠です。 新しい watchOS 3 に、Apple のジェスチャ レコグナイザー、デジタル クラウンおよび新しいユーザーに通知してナビゲーション手法へのアクセスのサポートが追加されました。 これには、SceneKit と SpriteKit の両方のサポートを追加および開発者ができるようにクイックと応答性の両方である、豊富なドリルインのインターフェイスを簡単に作成します。

## <a name="what-are-quick-interactions"></a>クイック操作とは

IOS または macOS (ユーザーがアプリとの対話に費やした時間は分または時間単位で測定) のアプリケーションの作成に使用する開発者は、Apple Watch の成功したアプリの設計が難しくなるし、異なる必要がありますアプローチです。

WatchOS で、ユーザーは、通常、手首を発生させる、すばやく (通常は、簡単ないくつかの秒数)、アプリと対話し、drop、手首およびした元の作業を続行するがします。

Apple Watch での一般的なクイック相互作用の例をいくつかを次に示します。

- タイマーを開始しています。
- 気象を確認しています。
- Todo リストから項目をマークします。

これらの目標を達成するのには、Apple Watch 上のアプリ必要があります。

- **ドリルイン**-ユーザーの概要と、簡単なことを意味は、必要な情報を取得できなければなりません。 
- **実用的な**-つまり、ユーザーは、クイック、適切な情報に基づいて意思決定を行うことにする必要があります。
- **応答性の高い**-、つまり、ユーザーや、必要に応じてアクションを実現するために必要な情報を受信することはありません待機する必要があります。

### <a name="quick-interactions-length"></a>クイック操作の長さ

クイック操作の理想的な長さが 2 秒になることを Apple ドリルイン Apple Watch アプリの性質、提案以下です。 この 2 つの 2 番目の制限の結果として、開発者は、かなりの両方の設計と、Apple Watch アプリを実装する時間を費やす必要があります。 

## <a name="new-watchos-3-features-and-apis"></a>新しい watchOS 3 の機能と Api

Apple には、クイック相互作用を Apple Watch アプリに追加するのには開発者を支援するために WatchKit をいくつかの新機能と Api が追加します。

- watchOS 3 は、ユーザー入力などの新しい種類へのアクセスを提供します。
    - ジェスチャ レコグナイザー
    - デジタル クラウン回転 
- watchOS 3 には、表示してなどについては、の更新の新しい方法が用意されています。
    - 拡張されたテーブルのナビゲーション
    - 新しいユーザーの通知フレームワークのサポート
    - SpriteKit と SceneKit の統合

これらの新機能を実装すると、開発者は、watchOS 3 アプリが Glanceable、Actionable している Responsive を確認することができます。

### <a name="gesture-recognizer-support"></a>ジェスチャ レコグナイザーのサポート

開発者には、iOS でジェスチャ レコグナイザーが実装されている場合は、ジェスチャ レコグナイザーを watchOS 3 で作業する方法をよく理解が必要があります。 更新するには、ジェスチャ レコグナイザーは、認識可能な定義済みのジェスチャに低レベルのタッチ イベントを解析するオブジェクトです。

watchOS 3、4 つの次のジェスチャ レコグナイザーをサポートします。

- 不連続のジェスチャの種類:
    - スワイプ ジェスチャ (`WKSwipeGestureRecognizer`)。
    - タップ ジェスチャ (`WKTapGestureRecognizer`)。
- 継続的なジェスチャの種類:
    - パン ジェスチャ (`WKPanGestureRecognizer`)。
    - 時間の長いキーを押してジェスチャ (`WKLongPressGestureRecognizer`)。

新しいジェスチャ レコグナイザーのいずれかを実装するには、には、Mac の iOS の Visual Studio のデザイナーのデザイン サーフェイスにドラッグし、そのプロパティを構成します。

コードでは、ユーザーによってジェスチャを処理する認識エンジンのアクションに応答します。 ここでも、これは、同じ方法で iOS で処理するようにします。

#### <a name="discrete-gesture-states"></a>個別のジェスチャ状態

ジェスチャが認識されると、状態、不連続のジェスチャでは、アクションが呼び出されます (`WKGestureRecognizerState`) として割り当てられます。

[![](quick-interaction-techniques-images/quick01.png "個別のジェスチャ状態")](quick-interaction-techniques-images/quick01.png#lightbox)

すべての不連続ジェスチャ作業を開始、`Possible`状態および遷移にするか、`Failed`または`Recognized`状態です。 不連続のジェスチャを使用する場合、開発者、通常しないを直接処理状態。 代わりに、依存しているジェスチャがのみ認識されるときに呼び出されるアクション。

#### <a name="continuous-gesture-states"></a>継続的なジェスチャの状態

継続的なジェスチャは不連続ジェスチャをジェスチャが認識されているように、アクションで複数回に呼び出されてから若干異なります。

[![](quick-interaction-techniques-images/quick02.png "継続的なジェスチャの状態")](quick-interaction-techniques-images/quick02.png#lightbox)

ここでも、継続的なジェスチャで開始、`Possible`状態が、複数の更新プログラムの進捗します。 開発者は認識エンジンの状態を考慮し、中に、アプリの UI を更新する必要がありますここで、`Changed`ジェスチャが最後になるまでのフェーズ`Recognized`または`Canceled`です。

#### <a name="gesture-recognizer-usage-tips"></a>ジェスチャ レコグナイザー使用上のヒント

WatchOS 3 でジェスチャ レコグナイザーを操作するときに、Apple は、次を勧めします。

- ジェスチャ レコグナイザーを個別のコントロールではなくグループ要素に追加します。 要素をグループ化を拡大する傾向があります Apple Watch の物理的な画面サイズを小さくので、しを押すをターゲットに容易にします。 また、ジェスチャ レコグナイザーできますと競合する、ネイティブ UI コントロールに既にジェスチャに組み込まれています。
- Watch アプリのストーリー ボードで依存関係を設定します。
- 一部のジェスチャよりも優先他の種類のジェスチャなど。
    - スクロール
    - 強制的にタッチ
 
### <a name="digital-crown-rotation"></a>デジタル クラウン回転

デジタル クラウン サポートを実装すると、watchOS 3 アプリで、開発者を提供できます増加ナビゲーション ユーザー速度と精度の相互作用。

Apple Watch アプリの使用 watchOS 2、以降、`WKInterfacePicker`オブジェクトの一覧を提供することによって、デジタル クラウンへアクセス`WKPickerItems`とピッカー スタイル (リスト、積み上げ横またはイメージ シーケンス)。 watchOS では、一覧から項目を選択するデジタル クラウンを使用するユーザーが許可されます。

使用する場合、 `WKInterfacePicker`WatchKit がによって作業のほとんどを処理します。

- 一覧および各インターフェイス要素を描画します。
- デジタル クラウンのイベントを処理します。
- 項目が選択されている場合に、アクションを呼び出します。

新しい watchOS 3 に、開発者がデジタル クラウン回転イベントに直接アクセスする回転の値に応答する、独自の UI 要素を作成することができます。

デジタル クラウン アクセスは、次の要素によって提供されます。

- `WKCrownSequencer` -1 秒あたりの回転へのアクセスを提供します。
- `WKCrownDelegate` -回転デルタ イベントへのアクセスを提供します。

#### <a name="rotations-per-second"></a>1 秒あたりの回転

デジタル クラウンから秒あたりの回転へのアクセスは、物理操作は、アニメーションをベースする場合に便利です。 秒あたりの回転にアクセスするには、使用、`CrownSequencer`のプロパティ、`WKInterfaceController`ウォッチ拡張機能のです。 例:

```csharp
var rotationsPerSecond = CrownSequencer.RotationsPerSecond;
```

#### <a name="rotational-deltas"></a>回転のデルタ

デジタル クラウンから回転デルタを使用して、回転の数をカウントします。 使用して、`CrownDidRotate`のメソッドをオーバーライドして、`WKCrownDelegate`回転デルタにアクセスします。 例:

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

ここで、アプリがアキュムレータを保持 (`AccumulatedRotations`) の回転数を決定します。 デジタル クラウンの 1 つの完全回転がの累積のデルタに等しい`1.0`半分回転、`0.5`です。

Apple がのための状態で更新されている UI 要素の変更の秘密度の回転数の対応方法を決定する開発者です。

符号 (`+/-`) 回転デルタのユーザーがデジタル クラウンを回転する方向を示します。

[![](quick-interaction-techniques-images/quick03.png "回転の差分の符号は、ユーザーがデジタル クラウンを回転する方向を示します")](quick-interaction-techniques-images/quick03.png#lightbox)


ユーザーは、上方向にスクロール、WatchKit では、正の値のデルタと下方向にスクロールし、負のデルタが返されます、向きユーザー影響でウォッチに関係なくを返します。

#### <a name="digital-crown-focus"></a>デジタル クラウン フォーカス

その他のインターフェイス要素と同じようには、デジタル クラウンは、フォーカスの概念がします。 このフォーカスは、ウォッチと、ユーザーが対話する方法に基づくその他のインターフェイス要素がデジタル クラウンから離れた場所に移動されたことができます。 

たとえば、デジタル クラウンのフォーカスを盗み、次のコントロールのいずれか。

- ピッカー
- スライダー
- コント ローラーをスクロール

それぞれのカスタム インターフェイス要素がデジタル クラウンのフォーカスをする必要がある場合を決定する、開発者の責任です。 Apple では、新しいジェスチャ レコグナイザーを使用してカスタムの UI 要素にフォーカスを獲得するをお勧めします。

### <a name="vertical-paging"></a>垂直方向のページング

標準的な方法、ユーザーが watchOS アプリ内のテーブル ビューを移動することは、目的のデータまでスクロールし、詳細ビューを表示するのには、詳細を確認したら、戻る をタップして特定の行をタップし、その他の情報の処理を繰り返しますをy に関心のあるから、テーブル内で。

[![](quick-interaction-techniques-images/quick04.png "テーブルや詳細ビュー間を移動")](quick-interaction-techniques-images/quick04.png#lightbox)

新しい watchOS 3、開発者を有効に垂直方向のページングのテーブル ビュー コントロールにします。 この機能を有効にすると、ユーザーは、テーブル ビューの行を見つけて、前にその詳細を表示する行をタップしてスクロールできます。 ただしができるようになりました上方向にスワイプの表に、または前の行を選択するように、次の行を選択 (またはデジタル クラウン)、テーブルのビューに戻るにことがなくすべて最初。

[![](quick-interaction-techniques-images/quick05.png "テーブルや詳細ビュー間を移動およびスワイプ上または下の他の行に移動するには")](quick-interaction-techniques-images/quick05.png#lightbox)

このモードを有効にするを Xcode で watchOS アプリのストーリー ボードを開いて編集テーブル ビューを選択し、確認、**垂直詳細ページング** チェック ボックス。

[![](quick-interaction-techniques-images/quick06.png "チェック ボックスを垂直方向の詳細のページング")](quick-interaction-techniques-images/quick06.png#lightbox)

詳細なビューを表示し、ストーリー ボードに変更を保存して同期する Mac 用の Visual Studio に戻り、テーブルが Segues を使用していることを確認します。

開発者取り組むことができますプログラムで垂直ページング テーブル ビューに対して次のコードを使用して特定の行にします。

```csharp
// Segue into Vertical Paging and select the first row
MenuTable.PerformSegue (0);
```

垂直方向のページングを使用する場合、開発者を WatchKit はコント ローラーのプリロードを自動的に処理し、その結果、UI が実際に表示される前にいくつかのコント ローラー ライフ サイクル メソッドを呼び出すことができることに注意する必要があります。

### <a name="notification-enhancements"></a>通知の機能強化

通知は、ユーザーが通常 watchOS で発生しているクイックの相互作用のプライマリの形式し、最初の Apple Watch と watchOS 1 から利用できましたがします。

一般的な通知のクイック操作は次のとおりです。

1. ユーザーは、新しい通知が受信したときに通知 Haptic と考えています。
2. 通知の短い検索インターフェイスを表示するには、その手首を起動するとします。
3. それらを引き続き維持し発生、手首、watchOS は自動的に長い検索通知インターフェイスに移行します。

これにはユーザー通知に応答できるいくつかの方法があります。

- 定義され、通知の表示も、ユーザーは何もしないし、単に、通知を破棄します。
- WatchOS アプリを起動する通知のタップした可能性があります。
- カスタム アクションをサポートする、通知のカスタム アクションのいずれかのユーザーが選択する可能性があります。 これらいずれかになります。
    - **フォア グラウンド アクション**-これらを起動して、操作を実行するアプリです。
    - **操作をバック グラウンド**- watchOS 2 で iPhone に常にルーティングされましたが、watchOS 3 で watchApp にルーティングできます。

WatchOS 3: の新機能

* 通知は、すべてのプラットフォーム (iOS、watchOS、tvOS および macOS) 間で同様の API を使用します。
* ローカル通知は、Apple Watch でスケジュールできます。
* バック グラウンド通知は、Apple Watch でスケジュールされた場合、アプリの拡張機能にルーティングされます。

#### <a name="notification-scheduling-and-delivery"></a>通知のスケジュールと配信

ユーザーの iPhone からの通知は、次が発生すると Apple Watch を前方になります。

* IPhone の画面がオフです。
* Apple Watch では、装着されていると、ロック解除されました。

WatchOS 3、ローカル通知は Apple Watch でスケジュールでき、ウォッチにのみ提供されます。 開発者はアプリで必要な場合は、対応する iPhone 通知をスケジュールします。

Apple Watch と iPhone のバージョンの通知の両方で同じ通知 Id を含めることによって、ウォッチに表示される通知の重複を防ぎます。 通知の Apple Watch バージョン iPhone のバージョンより優先順位になります。

WatchOS 3 を使用して同じので`UINotification`API フレームワークとして iOS 10、10、iOS を参照してください[ユーザー通知フレームワーク](~/ios/platform/user-notifications/index.md)詳細についてはドキュメントです。

### <a name="using-spritekit-and-scenekit"></a>SpriteKit および SceneKit を使用してください。

新しい watchOS 3 に、開発者使えるようになりました SpritKit と SceneKit の両方のオブジェクト、アプリのユーザー インターフェイスの設計で両方の 2D および 3D グラフィックスを表示します。

この機能をサポートするために、2 つの新しいインターフェイス クラスが追加されました。

- `WKInterfaceSKScene` -の SpriteKit 2 次元グラフィックスを使用します。
- `WKInterfaceSCNScene` -の SceneKit 3 D グラフィックスを使用します。

これらのオブジェクトを使用する単に Xcode のインターフェイスのビルダーで、watch アプリのストーリー ボードの内部で、デザイン画面にドラッグしを使用して、**属性インスペクター**これらを構成します。

ここから、SpriteKit または SceneKit シーンの扱いと同様に機能が iOS アプリ内であるか。 Watch アプリが表示されます、`WKInterfaceSKScene`のいずれかを呼び出すことによって、`Present`メソッドです。 SceneKit、単に設定、`Scene`のプロパティ、`WKInterfaceSCNScene`オブジェクト。

## <a name="actionable-complications"></a>実用的な複雑な問題

WatchOS 2 では、Apple は、サード パーティ製アプリの複雑さの一部を紹介します。 WatchOS 3 で Apple が WatchKit コンプリケーションに含めることができます開発者の能力を拡張します。 

さらに、複数の組み込みウォッチで面できるようになりました複雑さの一部と、既存の監視に直面して既にサポートされている複雑な問題がさらに多くの複雑な問題を含めることができますようになりました。

また新しい機能です、ユーザーの左側または右側に、Apple Watch にインストールされている、ウォッチ面のすべての遷移を迅速にスワイプします。 新しいギャラリーを使用して、Apple Watch のコンパニオンの iPhone アプリで、ユーザーは追加し、新しいウォッチ面と複雑さの一部を含むことのいずれかをカスタマイズします。

これらの新機能のため、Apple は、Apple Watch でのすべてのアプリでは、少なくとも 1 つコンプリケーションを含める必要があります、そのため、ネイティブ Apple Watch アプリのようになりました複雑さの一部を提案します。

複雑さの一部は、アプリには、次の機能を提供します。

- 以降では、常にウォッチの文字盤高ドリルインです。
- 複雑さの一部は、watchOS して頻繁に更新されます。 ユーザーの現在表示されているウォッチの文字盤で問題を含むすべてのアプリには、1 時間には、少なくとも 2 倍が更新されます。
- ユーザーの現在表示されているウォッチの文字盤でコンプリケーションのすべてのアプリは、すばやくを起動して、アプリになり、アプリからの応答の処理速度が向上するメモリに保持されます。
- 複雑さの一部を使用すると、ユーザーが watchOS アプリで特定の機能を起動するため簡単。

## <a name="glanceable-notification"></a>ドリルイン通知

Apple Watch で通知は、イベント、または受信メッセージまたはトレーニング アプリでの目的を達成するなどの新しい情報のユーザーに迅速に通知する優れた、カスタマイズ可能な方法を提供します。

通知を使用すると、貴重な情報を簡単にユーザーに提示することです。 多くの状況で適切に設計された通知は、ユーザーが実際にアプリを起動するための必要性を削除できます。

WatchOS 3、すべての通知で現在サポートする新しい。

- SpriteKit
- SceneKit
- ビデオ クリップの再生

## <a name="enhanced-ui-with-spritekit-and-scenekit"></a>SpriteKit と SceneKit 拡張 UI

通常、開発者と考えることがゲーム UI SpriteKit と SceneKit が示されているときにします。 ただし、SpriteKit と両方 SceneKit できますゲーム非 Ui をカスタマイズしたレイアウト、コンテンツを含む、それ以外の場合は単独で WatchKit で不可能なアニメーションを作成するのに役立ちます。

たとえば、写真を共有するアプリからユーザー通知は、実際の画像およびユーザーの操作性を拡充するカスタマイズされたその他の情報と共に写真を掲載されているユーザーを含めることによって、機能豊富なユーザー エクスペリエンスを提供するのに SpriteKit を使用できます。

さらに、アプリのユーザー インターフェイスの設計で標準の WatchKit UI 要素の SpriteKit と SceneKit の両方を混在させることができます。

## <a name="simple-navigation"></a>簡単なナビゲーション

watchOS 3 はいくつかの方法を開発者が、新しいなどが watchOS アプリ内のナビゲーションを簡略化できます[垂直ページング](#Vertical-Paging)、[ジェスチャ レコグナイザー サポート](#Gesture-Recognizer-Support)と[デジタル クラウン回転](#Digital-Crown-Rotation)上機能が表示されます。

デジタル クラウンは、Apple Watch に固有のナビゲーションを簡単にさまざまな方法で使用できます。 たとえば、タイマー アプリケーションは、使用可能なタイマーの長さをスクラブするデジタル クラウンを使用できます。

カスタム ジェスチャは watch アプリと対話するユーザーの新しい独自の方法を提供できるし、アプリ ナビゲーションを簡単に使用することもできます。

Apple では、すべて watchOS 豊富な簡単かつ watchOS アプリのインターフェイスを使用してクイックを提示する 3 で追加された新しいクイック対話機能を結合する方法を探してをお勧めします。

## <a name="finishing-the-quick-interaction"></a>クイック操作を完了

クイック操作を適切に設計されたエクスペリエンスは、信頼度を削除して、手首 (アプリの固定を解除) に、ユーザーに付与すると現在の操作が完了します。

具体的になります問題 watch アプリが任意の種類のネットワーク接続を行うか、コンパニオン iPhone アプリでの情報共有です。 多くの場合につながります待機インジケーター、トランザクションの実行中、これは、クイック対話中に望ましくないです。 次の例を参照してください。

[![](quick-interaction-techniques-images/quick07.png "ネットワーク接続を行い、そのガイド 』 の iPhone アプリでの情報共有、watch アプリのダイアグラム")](quick-interaction-techniques-images/quick07.png#lightbox)

1. ユーザーは、ウォッチ上を購入する項目を選択します。
2. これらは、購入ボタンをタップします。
3. アプリでは、ネットワーク トランザクションを開始し、読み込みインジケーターを表示します。
4. しばらくしては、トランザクションの完了し、アプリが発注確認を表示します。
5. ユーザーは、手首を削除し、アプリを解除します。

トランザクションが完了するまで、ユーザーが、[購入] ボタンをタップしたときから読み込みインジケーターを見て発生手首はそのがあります。 このような状況を解決するためには、Apple は、開発者が即時のフィードバックを読み込みインジケーターではなく、ユーザーに表示することを提案します。 

Apple の推奨モデルを使用して、見て同じクイック操作をもう一度。

[![](quick-interaction-techniques-images/quick08.png "リンゴの推奨モデル ダイアグラム")](quick-interaction-techniques-images/quick08.png#lightbox)

1. ユーザーは、ウォッチ上を購入する項目を選択します。
2. これらは、購入ボタンをタップします。
3. アプリでは、ネットワーク トランザクションを開始し、購入が正常に開始されたことを示すメッセージを表示します。
4. ユーザーは、手首を削除し、アプリを解除します。
5. トランザクションには、しばらく正常に完了すると、アプリには、成功した注文書のユーザーに通知するローカル通知が表示されます。

今回は、ユーザーが、注文書が開始されたことを手首を削除し、クイック操作をこの時点で終了することが高い信頼性を示すメッセージが表示される購入ボタンをタップするとすぐにします。 後でのユーザーの通知内のトランザクションの成否が通知します。 これにより、ユーザーがのみ、アプリとプロセスの"active"フェーズ中に対話します。

ネットワークを実行しているアプリの場合は、使用できるバック グラウンド`NSURLSession`ダウンロードのタスクとのネットワーク通信を処理します。 これにより、ダウンロードした情報を処理するバック グラウンドでウェイク アップするアプリが許可されます。 バック グラウンド処理を必要とするアプリの場合に、必要な処理を処理するのにバック グラウンド タスク アサーションを使用できます。

## <a name="quick-interaction-design-tips"></a>クイック操作のデザインに関するヒント

クイック相互作用の希望の長さは 2 秒以内、開発者がデザイン プロセスの先頭から、アプリの相互作用の設計に重視必要があります。 それらの相互作用が簡略化できます (前述の手法を使用) と、クイックや応答性を行うと、アプリを watchOS 3 の新機能を使用して領域を見つけます。

Apple は、次のいずれかを示します。

- アプリの最も使用されている機能が持ち込まによってクイック相互作用に注目します。
- 共通の機能と機能を表示するのにには、複雑な問題およびユーザーへの通知を使用します。
- SceneKit と SpriteKit ドリルイン、豊富なユーザー インターフェイスを作成します。
- 可能な限り、アプリ内のナビゲーションを簡略化します。
- しばらくお待ち、そのリストを削除し、できるだけ早くのアプリの固定を解除できるように、ユーザーを行うことはありません。

## <a name="summary"></a>まとめ

この記事は、クイック対話テクニックをカバーされて Apple が watchOS 3 および Apple Watch の Xamarin.iOS でそれらを実装する方法で追加します。

## <a name="related-links"></a>関連リンク

- [watchOS のサンプル](https://developer.xamarin.com/samples/watchos/all/)
