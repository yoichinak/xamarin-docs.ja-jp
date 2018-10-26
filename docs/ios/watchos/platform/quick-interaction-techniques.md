---
title: Xamarin で watchOS 3 用のクイック操作手法
description: この記事が相互作用の簡単な手法では Apple は watchOS 3 と Apple Watch の Xamarin.iOS でそれらを実装する方法に追加します。
ms.prod: xamarin
ms.assetid: 26697F68-AF7E-4A36-988F-85E2674A4DD1
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 6a8e74860efd606ae6dd565ea7e3f67884eefc11
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50103219"
---
# <a name="quick-interaction-techniques-for-watchos-3-in-xamarin"></a>Xamarin で watchOS 3 用のクイック操作手法

_この記事が相互作用の簡単な手法では Apple は watchOS 3 と Apple Watch の Xamarin.iOS でそれらを実装する方法に追加します。_

提供の簡単なユーザー操作は、説得力のある Apple Watch アプリや複雑さを作成するために不可欠です。 新しい watchos 3、Apple がジェスチャ レコグナイザー、デジタル クラウンと新しいユーザー通知とナビゲーション手法へのアクセスのサポートを追加します。 これ、と共に SceneKit とある SpriteKit の両方のサポートを追加できるように、開発者は高速かつ応答性の高い豊富な glanceable のインターフェイスを簡単に作成します。

## <a name="what-are-quick-interactions"></a>クイック操作とは

IOS または macOS (ユーザーは、アプリとの対話が費やす時間の量は分または時間単位) のアプリケーションの作成に使用する開発者、設計アプリが正常に Apple Watch は難しい作業と異なる必要がありますアプローチです。

WatchOS で、ユーザーは、通常、手首を発生させる (通常は簡単ないくつかの秒単位)、アプリと対話し、手首を削除し、行ってきたあれを続行するのには。

次に、Apple Watch 上の標準的なクイックやり取りのいくつかの例を示します。

- タイマーを開始しています。
- 天気予報をチェックします。
- Todo リストから項目をマークします。

これらの目標を達成するために、Apple Watch 上のアプリがあります。

- **Glanceable** -ユーザーの概要 で、簡単なことを意味は、必要な情報を取得できる必要があります。 
- **アクションにつながる**-ユーザーのことを意味することができるよう迅速で適切に十分な情報に基づいて決定します。
- **応答性の高い**-、つまり、ユーザーまたは必要に応じてアクションを実現するために必要な情報を受信することはありません待機する必要があります。

### <a name="quick-interactions-length"></a>クイック操作の長さ

Apple がクイック操作の理想的な長さが 2 秒間にあることを提案 glanceable Apple Watch アプリの性質、またはそれ以下。 この 2 つの 2 番目の制限の結果として、開発者は、かなりの設計と、Apple Watch アプリの実装の両方の時間を費やす必要があります。 

## <a name="new-watchos-3-features-and-apis"></a>新しい watchOS 3 の機能と Api

Apple には、Apple Watch アプリ クイック操作を追加することで、開発者を支援するために、WatchKit をいくつかの新機能と Api が追加します。

- watchOS 3 は、ユーザー入力などの新しい種類へのアクセスを提供します。
    - ジェスチャ レコグナイザー
    - デジタル クラウンの回転 
- watchOS 3 には、表示して、情報の更新などの新しい方法が用意されています。
    - テーブル ナビゲーションの強化
    - 新しいユーザー通知フレームワークのサポート
    - SpriteKit と SceneKit の統合

これらの新機能を実装すると、開発者できることを確認、watchOS 3 アプリ Glanceable、Actionable およびレスポンシブ。

### <a name="gesture-recognizer-support"></a>ジェスチャ認識エンジンのサポート

開発者には、iOS でジェスチャ レコグナイザーが実装されている場合は、watchOS 3 でジェスチャ レコグナイザーのしくみをよく理解が必要があります。 更新するには、ジェスチャ レコグナイザーは、低レベルのタッチ イベントを識別しやすく、定義済みのジェスチャに解析するオブジェクトです。

watchOS 3、4 つの次のジェスチャ レコグナイザーをサポートします。

- 不連続のジェスチャの種類:
    - スワイプ ジェスチャ (`WKSwipeGestureRecognizer`)。
    - タップ ジェスチャ (`WKTapGestureRecognizer`)。
- 継続的なジェスチャの種類:
    - パン ジェスチャ (`WKPanGestureRecognizer`)。
    - 時間の長い-キーを押してジェスチャ (`WKLongPressGestureRecognizer`)。

新しいジェスチャ レコグナイザーのいずれかを実装するには、Mac の iOS Designer が Visual Studio でのデザイン画面にドラッグし、そのプロパティを構成します。

コードでは、ユーザーによってトリガーされるジェスチャを処理するために、認識エンジンのアクションに応答します。 ここでも、これは、同じ方法でが iOS で処理するようにします。

#### <a name="discrete-gesture-states"></a>個別のジェスチャ状態

ジェスチャが認識されると、状態を個別のジェスチャのアクションが呼び出されます (`WKGestureRecognizerState`) として割り当てられています。

[![](quick-interaction-techniques-images/quick01.png "個別のジェスチャ状態")](quick-interaction-techniques-images/quick01.png#lightbox)

すべての不連続ジェスチャから作業を開始、`Possible`状態といずれかへの移行、`Failed`または`Recognized`状態。 不連続のジェスチャを使用する場合、開発者一般的には直接扱う状態。 代わりに、ジェスチャがのみ認識されるときに呼び出されるアクションに依存しています。

#### <a name="continuous-gesture-states"></a>ジェスチャの継続的な状態

継続的なジェスチャとは、ジェスチャが認識されているように、アクションで複数回に呼び出された個別のジェスチャとは若干異なります。

[![](quick-interaction-techniques-images/quick02.png "ジェスチャの継続的な状態")](quick-interaction-techniques-images/quick02.png#lightbox)

ここでも、継続的なジェスチャで開始、`Possible`状態が、複数の更新プログラムの進捗します。 ここで、開発者は、認識エンジンの状態を検討し、中に、アプリの UI を更新する必要は、`Changed`ジェスチャが最後になるまでのフェーズ`Recognized`または`Canceled`します。

#### <a name="gesture-recognizer-usage-tips"></a>ジェスチャ認識エンジンの使用状況に関するヒント

WatchOS 3 でジェスチャ レコグナイザーを使用する場合に、Apple は、次をお勧めします。

- ジェスチャ レコグナイザーを個々 のコントロールではなくグループの要素に追加します。 要素をグループ化を大きくする傾向があります Apple Watch の物理的な画面サイズを小さくので、およびキーを押すのターゲットを容易にします。 また、ジェスチャ レコグナイザーと競合することのジェスチャ、ネイティブ UI コントロールの既に組み込まれています。
- Watch アプリのストーリー ボードでは、依存関係を設定します。
- いくつかのジェスチャは、次の他の種類のジェスチャを優先順位を実行します。
    - スクロール
    - Force Touch
 
### <a name="digital-crown-rotation"></a>デジタル クラウンの回転

WatchOS 3 アプリのデジタル クラウンのサポートを実装すると、開発者を提供できますナビゲーションの向上、ユーザーの速度と精度の相互作用。

Apple Watch アプリの使用 watchOS 2、以降、`WKInterfacePicker`オブジェクトの一覧を提供することによって、デジタル クラウンへアクセス`WKPickerItems`とピッカーのスタイル (リスト、積み上げ横またはイメージのシーケンス)。 watchOS では、デジタル クラウンを使用して、一覧から項目を選択するユーザーが許可されます。

使用する場合、 `WKInterfacePicker`WatchKit が、作業のほとんどを処理します。

- 一覧と、個々 のインターフェイス要素を描画します。
- デジタル クラウンのイベントを処理します。
- 項目が選択されている場合に、アクションを呼び出します。

新しい watchOS 3、開発者今すぐに直接アクセス権のデジタル クラウンの回転のイベントを回転値に対応する独自の UI 要素を作成することができます。

デジタル クラウン アクセスは、次の要素で提供されます。

- `WKCrownSequencer` -1 秒あたりの回転へのアクセスを提供します。
- `WKCrownDelegate` -回転デルタ イベントへのアクセスを提供します。

#### <a name="rotations-per-second"></a>1 秒あたりの回転

物理運動の使用ベースのアニメーション場合は、デジタル クラウンから秒あたりの回転にアクセスすると便利です。 秒あたりの回転にアクセスするには、使用、`CrownSequencer`のプロパティ、`WKInterfaceController`ウォッチ拡張機能の。 例えば:

```csharp
var rotationsPerSecond = CrownSequencer.RotationsPerSecond;
```

#### <a name="rotational-deltas"></a>回転の差分

デジタル クラウンからデルタを回転を使用して、回転の数をカウントします。 使用して、`CrownDidRotate`のメソッドをオーバーライド、`WKCrownDelegate`回転デルタへのアクセスにします。 例えば:

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

ここで、アプリがアキュムレータを維持 (`AccumulatedRotations`) の回転の数を決定します。 デジタル クラウンの完全な回転を 1 つは、累積のデルタに等しい`1.0`半分回転あり`0.5`します。

Apple を開発者が回転数が更新される UI 要素の変更の機密度に対応させる方法を決定するまで左が。

サインイン (`+/-`) 回転デルタのユーザーがデジタル クラウンを有効にする方向を示します。

[![](quick-interaction-techniques-images/quick03.png "回転の差分の符号は、ユーザーがデジタル クラウンを有効にする方向を示します")](quick-interaction-techniques-images/quick03.png#lightbox)


ユーザーがスクロールする、WatchKit では、正の値のデルタと下へスクロールして、し、負のデルタが返されます、向き、ユーザーは、ソックスを着けずにでウォッチに関係なくを返します。

#### <a name="digital-crown-focus"></a>デジタル クラウン フォーカス

他のインターフェイス要素と同じようには、デジタル クラウンはフォーカスの概念が。 ユーザーがウォッチと対話している方法に基づいて他のインターフェイス要素にデジタル クラウンからこのフォーカスを移動できます。 

たとえば、デジタル クラウンのフォーカスを盗み、次のコントロールのいずれかでした。

- ピッカー
- スライダー
- コント ローラーのスクロール

カスタム インターフェイス要素は、デジタル クラウンのフォーカスをする必要がある時期を決定する開発者の責任です。 Apple では、新しいジェスチャ レコグナイザーを使用してカスタム UI 要素にフォーカスを取得するをお勧めします。

### <a name="vertical-paging"></a>垂直方向のページング

ユーザーが watchOS アプリでのテーブル ビューを移動する標準的な方法は、目的のデータまでスクロールし、詳細なビューを表示するのには、詳細の表示が終了 [戻る] ボタンをタップします特定の行をタップし、その他の情報の処理を繰り返しますが、。y は、テーブル内の関心は。

[![](quick-interaction-techniques-images/quick04.png "テーブルと詳細ビューの間の移動")](quick-interaction-techniques-images/quick04.png#lightbox)

新しい watchos 3、開発者で有効にできる垂直方向のページング、テーブルのビュー コントロール。 この機能を有効にすると、ユーザーがスクロールをテーブル ビューの行を見つけて、前にその詳細を表示する行をタップします。 ただしができるようになりました上方向にスワイプの表に、または前の行を選択するように、次の行を選択します (またはデジタル クラウン)、テーブル ビューに戻ることがなくすべて最初に。

[![](quick-interaction-techniques-images/quick05.png "テーブルと詳細ビューの間を移動およびスワイプがあります、上下の他の行に移動するには")](quick-interaction-techniques-images/quick05.png#lightbox)

このモードを有効にする Xcode で、watchOS アプリのストーリー ボードを開いて編集テーブル ビューを選択し、確認、**垂直詳細ページング**チェック ボックスをオンします。

[![](quick-interaction-techniques-images/quick06.png "垂直方向の詳細のページングのチェック ボックスをオンします。")](quick-interaction-techniques-images/quick06.png#lightbox)

詳細ビューを表示し、ストーリー ボードに変更を保存して Visual Studio for Mac を同期に戻り、テーブルが Segues を使用していることを確認します。

開発者取り組むことができますプログラムで垂直ページング テーブル ビューに対して次のコードを使用して特定の行に。

```csharp
// Segue into Vertical Paging and select the first row
MenuTable.PerformSegue (0);
```

垂直方向のページングを使用する場合、開発者は、WatchKit はコント ローラーのプリロードを自動的に処理し、UI が実際に表示される前のいくつかのコント ローラーのライフ サイクル メソッドの呼び出し結果、ことに注意してください必要があります。

### <a name="notification-enhancements"></a>通知の機能強化

通知は、クイック対話をユーザーが通常 watchOS で発生しているは、プライマリの形式し、最初の Apple Watch と watchOS 1 以降使用できました。

一般的な通知のクイック操作は次のとおりです。

1. 新しい通知が受信したときに、ユーザーはハプティクス通知と判断します。
2. 通知の短い検索インターフェイスを表示するには、その手首を起動するとします。
3. 発生した、手首、引き続き、watchOS は自動的に長い検索通知インターフェイスに遷移します。

ユーザー通知に応答できるいくつかの方法はあります。

- 定義されているし、通知を表示、ユーザーは何もしないし、単に、通知を破棄します。
- WatchOS アプリを起動する通知のタップした可能性があります。
- カスタム アクションをサポートするための通知、ユーザーがカスタム アクションのいずれかでが選択します。 これらは指定できますか。
    - **フォア グラウンド アクション**-これらアクションを実行するアプリを起動します。
    - **操作をバック グラウンド**- watchOS 2 で iPhone を常にルーティングされましたが、watchOS 3 で watchApp にルーティングできます。

WatchOS 3: の新機能

* 通知は、すべてのプラットフォーム (iOS、watchOS、tvOS、および macOS) で同様の API を使用します。
* Apple Watch では、ローカル通知をスケジュールできます。
* バック グラウンド通知は、Apple Watch でスケジュールされた場合、アプリの拡張機能にルーティングされます。

#### <a name="notification-scheduling-and-delivery"></a>通知のスケジュールと配信

ユーザーの iPhone からの通知は、次のようにときも、Apple Watch に前方になります。

* IPhone の画面は off です。
* Apple Watch が装着されてと、ロック解除されました。

WatchOS 3、ローカル通知は、Apple Watch でスケジュールでき、ウォッチにのみ提供されます。 アプリで必要な場合は、対応する iPhone 通知をスケジュールする開発者です。

Apple Watch と通知の iPhone バージョンの両方で同じ通知 Id を含めることによって、ウォッチに表示される通知の重複を防ぎます。 通知の Apple Watch のバージョンは、iPhone バージョンの上に優先順位にかかります。

WatchOS 3 を使用して同じため`UINotification`として 10、iOS API フレームワークは、iOS 10 を参照してください[ユーザー通知フレームワーク](~/ios/platform/user-notifications/index.md)詳細についてはドキュメントです。

### <a name="using-spritekit-and-scenekit"></a>SpriteKit と SceneKit を使用してください。

新しい watchos 3、開発者使えるようになりました SpritKit と SceneKit の両方のオブジェクト、アプリのユーザー インターフェイス デザインの両方の 2D および 3D グラフィックスを表示します。

この機能をサポートするために、2 つの新しいインターフェイス クラスが追加されました。

- `WKInterfaceSKScene` -の SpriteKit 2D グラフィックスを操作します。
- `WKInterfaceSCNScene` -の SceneKit 3D グラフィックスを操作します。

これらのオブジェクトを使用する単に Xcode の Interface Builder のストーリー ボードを watch アプリの内部でデザイン サーフェイスにドラッグし、を使用して、 **Attributes Inspector**設定を構成します。

ここから、SpriteKit または SceneKit のいずれかのシーンの操作、同じは同様に機能を iOS アプリ内で。 Watch アプリが表示されます、`WKInterfaceSKScene`のいずれかを呼び出すことによって、`Present`メソッド。 ある SceneKit に設定するだけ、`Scene`のプロパティ、`WKInterfaceSCNScene`オブジェクト。

## <a name="actionable-complications"></a>実践的な問題

WatchOS 2 では、Apple は、サード パーティ製アプリの複雑な問題を紹介します。 WatchOS 3、Apple は開発者が WatchKit コンプリケーションに含めることができる能力を拡大しました。 

さらに、詳細は、組み込みのウォッチで顔は今すぐコンプリケーションを含めるし、既存の監視に直面して既にサポートされている複雑な問題がさらに多くの複雑な問題を含めることができますようになりました。

また新しい機能です。 ユーザーの左側または右側に、Apple Watch にインストールされている、腕時計型インターフェイスのすべての遷移を迅速にスワイプします。 Apple Watch のコンパニオンの iPhone アプリの新しいギャラリーを使用して、ユーザーは追加し、新しい腕時計型インターフェイスと複雑さに含めるのいずれかをカスタマイズします。

これらの新機能のためは、Apple は、Apple Watch 上のすべてのアプリケーションでは、少なくとも 1 つコンプリケーションを含める必要がありますが、そのため、ネイティブ Apple Watch アプリのようになりましたコンプリケーションを提案します。

複雑な問題は、アプリには、次の機能を提供します。

- ウォッチの文字盤の上に存在するが常にいるため高 glanceable です。
- 複雑な問題は、watchOS で頻繁に更新されます。 ユーザーの現在表示されているウォッチの文字盤で複雑なを含むすべてのアプリは、1 時間に 2 回以上に更新されます。
- ユーザーの現在表示されているウォッチの文字盤を合わせると、すべてのアプリは、アプリをすばやく起動を行い、アプリからの応答の速度が向上するメモリに保持されます。
- 複雑な問題を簡単に watchOS アプリで特定の機能を起動するユーザー。

## <a name="glanceable-notification"></a>Glanceable 通知

Apple Watch に通知は、イベントまたは受信メッセージまたはトレーニング アプリでの目的を達成するなどの新しい情報のユーザーに迅速に通知する優れた、カスタマイズ可能な方法を提供します。

通知を使用すると、貴重な情報を迅速に、ユーザーに表示することができます。 多くの状況で適切に設計された通知が実際にアプリを起動するユーザーの必要性を削除できます。

新しい watchos 3、すべての通知になりました。

- SpriteKit
- SceneKit
- インライン ビデオ

## <a name="enhanced-ui-with-spritekit-and-scenekit"></a>SpriteKit と SceneKit 拡張 UI

通常、ゲームの UI は、SpriteKit と SceneKit が示されているときに、開発者と思うかもしれません。 ただし、両方 SpriteKit および SceneKit できます非ゲーム カスタマイズされたレイアウト、コンテンツを含む Ui やそれ以外の場合、WatchKit だけで可能なないアニメーションを作成するのに役立ちます。

たとえば、写真を共有するアプリからのユーザー通知では、実際のイメージとユーザー エクスペリエンスを拡充するその他のカスタマイズされた情報と画像を投稿したユーザーを含めることによって、リッチなユーザー エクスペリエンスを提供するのに SpriteKit を使用できます。

さらに、アプリのユーザー インターフェイスのデザインで標準の WatchKit UI 要素を持つ SpriteKit と SceneKit の両方を混在させることができます。

## <a name="simple-navigation"></a>単純なナビゲーション

watchOS 3 はいくつかの方法を開発者が新しいなど、watchOS アプリ内のナビゲーションを簡略化できます[垂直ページング](#Vertical-Paging)、[ジェスチャ認識エンジン サポート](#Gesture-Recognizer-Support)と[デジタル クラウン回転](#Digital-Crown-Rotation)上の機能が表示されます。

デジタル クラウンは、Apple Watch に固有のナビゲーションを簡略化するさまざまな方法で使用できます。 たとえば、タイマー アプリケーションはを通じて使用可能なタイマーの長さをスクラブするデジタル クラウンを使用できます。

カスタム ジェスチャでは、watch アプリと対話するユーザーの一意の新しい方法を提示できるし、アプリのナビゲーションしやすいようにも使用できます。

Apple は watchOS 3 豊富な簡単かつ watchOS アプリのインターフェイスを使用するクイックを提示するで追加された新しいクイック対話機能のすべてを結合する方法を探してお勧めします。

## <a name="finishing-the-quick-interaction"></a>クイック操作の完了

クイック操作を適切に設計されたエクスペリエンスは、手首を削除 (およびアプリの改ざん) に確実にユーザーに与えるときに現在の操作が完了します。

具体的にはなります問題は、watch アプリの任意の種類のネットワーク接続を行うまたはそのコンパニオンの iPhone アプリと情報を共有します。 これは、トランザクションの実行中、クイック操作中には望ましくありませんが、待機インジケーターにつながります多くの場合。 次の例を参照してください。

[![](quick-interaction-techniques-images/quick07.png "ネットワーク接続を行い、そのコンパニオンの iPhone アプリと情報を共有、watch アプリの図")](quick-interaction-techniques-images/quick07.png#lightbox)

1. ユーザーは、watch で購入するアイテムを選択します。
2. これらは、購入ボタンをタップします。
3. アプリでは、ネットワーク トランザクションを開始し、読み込みインジケーターが表示されます。
4. しばらくしては、トランザクションが完了したし、アプリが発注確認を表示します。
5. ユーザーは、手首を削除し、アプリで解除します。

トランザクションが完了するまで、ユーザーが購入ボタンをタップした時間から読み込みインジケーターを見て発生手首は自分があります。 このような状況を解決するためには、Apple は、開発者が読み込みインジケーターではなく、ユーザーに即座にフィードバックを提供する必要がありますを提案します。 

Apple の推奨モデルを使用して、同じクイック操作をもう一度で参照してください。

[![](quick-interaction-techniques-images/quick08.png "りんご推奨モデルのダイアグラム")](quick-interaction-techniques-images/quick08.png#lightbox)

1. ユーザーは、watch で購入するアイテムを選択します。
2. これらは、購入ボタンをタップします。
3. アプリは、ネットワーク トランザクションを開始し、発注が正常に開始されたことを示すメッセージが表示されます。
4. ユーザーは、手首を削除し、アプリで解除します。
5. トランザクションには、しばらく正常に完了すると、アプリに正常に購入したユーザーに通知するローカル通知が表示されます。

このとき、ユーザーが購入が開始されたこと、自信を持って、手首を削除し、この時点で、クイック操作を終了することを示すメッセージが表示される、[購入] ボタンをタップするとすぐには。 後で、ユーザー通知でのトランザクションの成否が通知されします。 これにより、ユーザーがアプリとプロセスの"active"フェーズ中にのみ対話します。

ネットワー キングを実行しているアプリで使用できる、バック グラウンド`NSURLSession`ダウンロード タスクとのネットワーク通信を処理します。 これで、アプリをダウンロードした情報を処理するバック グラウンドでウェイク アップします。 バック グラウンド処理を必要とするアプリでは、必要な処理を処理するためにバック グラウンド タスクのアサーションを使用します。

## <a name="quick-interaction-design-tips"></a>クイック操作のデザインに関するヒント

以下またはをクイック操作の希望の長さは 2 秒であるために、開発者は設計プロセスの最初からアプリの相互作用のデザインに焦点する必要があります。 それらの相互作用が簡略化できます (上で示した手法を使用) と、watchOS 3 の新機能を使用して、アプリを短時間で応答性の高い領域を検索します。

Apple は、次のいずれかを示します。

- アプリの最も使用されている機能が持ち込までクイックの相互作用に注目します。
- 共通の特徴と機能を表示するのにには、複雑な問題およびユーザーへの通知を使用します。
- SceneKit と SpriteKit glanceable、豊富なユーザー インターフェイスを作成します。
- 可能であれば、アプリ内のナビゲーションを簡略化します。
- ユーザーの待機、手首を削除し、アプリをできるだけ早く中止することを許可することはありません。

## <a name="summary"></a>まとめ

この記事では、クイック操作の手法をカバーされて Apple は watchOS 3 と Apple Watch の Xamarin.iOS でそれらを実装する方法に追加します。

## <a name="related-links"></a>関連リンク

- [watchOS のサンプル](https://developer.xamarin.com/samples/watchos/all/)
