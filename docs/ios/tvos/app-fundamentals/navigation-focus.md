---
title: "ナビゲーションとフォーカスの操作"
description: "この記事では、フォーカス イベントとその使用を提示して Xamarin.tvOS アプリ内でナビゲーションを処理する方法の概念について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: DD72E95F-AE9B-47D2-B132-5FA5FBD8026E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: fe1358d330c2a0fd94016853cedeabe094c394da
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/22/2018
---
# <a name="working-with-navigation-and-focus"></a>ナビゲーションとフォーカスの操作

_この記事では、フォーカス イベントとその使用を提示して Xamarin.tvOS アプリ内でナビゲーションを処理する方法の概念について説明します。_


この記事の概念を説明する[フォーカス](#Focus-and-Selection)の処理に使用する方法と[ナビゲーション](#Navigation)Xamarin.tvOS アプリのユーザー インターフェイスにします。 Xamarin.tvOS アプリのユーザー インターフェイスのナビゲーションを提供する、組み込み tvOS ナビゲーション コントロールがフォーカスを強調表示と選択を使用する方法を説明します。

[![](navigation-focus-images/intro01.png "tvOS アプリ ユーザー インターフェイスのナビゲーション")](navigation-focus-images/intro01.png#lightbox)

フォーカスを使用する方法を見てみましょう次に、[視差](#Focus-and-Parallax)と*層イメージ*エンドユーザーに現在のナビゲーション状態を視覚的手がかりを提供します。

最後に見ていきます操作[フォーカス](#Working-with-Focus)、[フォーカス更新](#Working-with-Focus-Updates)、[フォーカス ガイド](#Working-with-Focus-Guides)、[コレクションでのフォーカス](#Working-with-Focus-in-Collections)と[視差を有効にする](#Enabling-Parallax)Xamarin.tvOS アプリ内のイメージ ビューに対するです。

<a name="Navigation" />

## <a name="navigation"></a>ナビゲーション

Xamarin.tvOS アプリのユーザーがいないするインターフェイスとの対話の直接として ios をタップしたいないデバイスの画面上のイメージが直接からルームを使用して、全体では、 [Siri リモート](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote)です。 ことに留意フロー必然的に、まだ Apple TV のエクスペリエンスに従事してユーザーを維持できるように、アプリのユーザー インターフェイスを設計するときにする必要があります。

成功した tvOS アプリでは、スムーズにアプリの目的は、およびナビゲーション自体に注意を呼び出さずに提示データの構造をサポートする方法のナビゲーションを実装します。 自然な使い慣れたユーザー インターフェイスを占有しているか、コンテンツとアプリケーションのユーザー エクスペリエンスからフォーカスを描画なしを感じるようには、ナビゲーションをデザインします。

[![](navigation-focus-images/nav01.png "TvOS 設定アプリ")](navigation-focus-images/nav01.png#lightbox)

中に、通常、Apple テレビ]、[ユーザーを使用する移動、画面のスタックされたセットを通じて、特定のコンテンツのセットを表示する各します。 さらに、新しい画面のすべてが生じるなどの標準的な UI コントロールを使用するコンテンツを 1 つまたは複数のサブ画面[ボタン](~/ios/tvos/user-interface/buttons.md)、[タブ バー](~/ios/tvos/user-interface/tab-bars.md)、テーブル、[コレクション ビュー](~/ios/tvos/user-interface/collection-views.md)または[分割ビュー](~/ios/tvos/user-interface/split-views.md)です。

データの新しい各画面で、ユーザーが移動するより深い画面のこのスタックにします。 使用して、**メニュー**ボタン Siri リモコンでは、前の画面またはメイン メニューに戻るには、スタックから後方移動できます。

Apple では、tvOS アプリのナビゲーションを設計するとき、次を念頭に置きを示しています。

- **レイアウト、ナビゲーション、検索するコンテンツの高速で簡単に**-タップの最小数に、アプリ内でコンテンツにアクセスしたいユーザーをクリックし、できるだけを読み取ります。 ナビゲーションを簡略化し、画面の最小の番号にコンテンツを編成します。
- **流体インターフェイスを使用して Touch 作成**-ユーザー間の移動できることを確認してください_フォーカス可能な項目_最小限の数の可能なジェスチャを使用して、摩擦が最小限に抑える。
- **注意フォーカスのあるデザイン**-Siri リモコンを使用してやり取りする前に、ユーザー インターフェイス項目にフォーカスを移動する必要があります、部屋のコンテンツを持つユーザーが対話して、ためです。 それらの目標を達成するが多すぎますジェスチャが必要な場合は、ユーザーがアプリに不満に表示されます。
- **メニュー ボタンを使用してナビゲーション後方**- 後方 Siri リモートを使用して移動するように、簡単で使い慣れたエクスペリエンスを作成する**メニュー**ボタンをクリックします。 キーを押して、**メニュー**ボタンが、前の画面に戻る常に、またはアプリのメイン メニューに戻る必要があります。 アプリのトップ レベルでキーを押して、**メニュー** Apple テレビ ホーム画面にボタンが返されます。
- **[戻る] ボタンを表示しないように通常**- キーを押してため、**メニュー** Siri リモコンのボタンが下位画面スタックを移動する、この動作を複製する追記のコントロールが表示されないようにします。 この規則の例外は、画面や破壊などのアクション (コンテンツを削除する) での購入の場所、**キャンセル**ボタンがも表示されます。
- **多くの代わりに、1 つの画面に大規模なコレクションを表示する**-Siri リモート クイック コンテンツの大規模なコレクションを移動し、容易なジェスチャを使用して設計されました。 フォーカス可能な項目の大規模なコレクションを使用して、アプリが動作する場合は、ユーザーの一部で複数のナビゲーションを必要とする多くの画面に分割するのではなく 1 つの画面上に維持することを検討します。
- **ナビゲーションの標準コントロールを使用して**にもう一度、可能な場合は、簡単で使い慣れたユーザー エクスペリエンスを作成するに使用して組み込み`UIKit`ページ コントロール、タブ バー、セグメント化されたコントロール、テーブルのビュー、コレクション ビューおよび分割などのコントロールアプリのナビゲーションのビューです。 これらの要素に慣れているユーザー、のでが直感的にできるアプリを移動します。
- **水平方向のコンテンツ ナビゲーションを優先**-Apple TV の性質上、Siri リモコンの左から右への読み取りがより自然より上下です。 アプリのコンテンツのレイアウトを設計するときは、このオプションを検討してください。

<a name="Focus-and-Selection" />

## <a name="focus-and-selection"></a>フォーカスと選択

Apple TV のイメージ、ボタンまたは他の UI 要素と見なされます_フォーカス_現在のナビゲーションのターゲットである場合。

[![](navigation-focus-images/focus01.png "フォーカスと選択の例")](navigation-focus-images/focus01.png#lightbox)

異なり、ここでそのユーザーがデバイスのタッチ スクリーン上の要素と直接やり取りする iOS デバイス Siri リモコンを使用して、部屋の向こう側のユーザーが対話 tvOS 要素を使用します。 Apple TV を使用するには存在し、このユーザーの対話処理を_フォーカス_ベース モデルです。

押してジェスチャとボタンを使用して、 [Siri リモート](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote)ユーザーでは、1 つの UI 要素からフォーカスを移動します。 目的の要素にフォーカスがシフト、したら、コンテンツを選択して、またはその要素で表されるアクションをアクティブ化するユーザーをクリックします。

フォーカスが変更された場合、現在フォーカスがあるユーザー インターフェイス項目を明確に識別する繊細なアニメーションと (イメージに視差効果) などの効果が使用されます。

Apple では、フォーカスと選択を操作するための以下の推奨事項があります。

- **モーション効果の組み込みの UI コントロールを使用して**を使用して`UIKit`フォーカス API のユーザー インターフェイス、フォーカスのモデルに自動的に適用されます、既定のアニメーションと視覚効果の UI 要素とします。 これと思われるアプリ ネイティブや Apple TV のプラットフォームのユーザーになじみなりフォーカス可能な項目間のスムーズになり、直感的な移動できます。
- **期待どおりの方向にフォーカスを移動する**-Apple TV のほぼすべての要素は、間接の操作を使用します。 たとえば、ユーザーでは、Siri リモコンを使用して、フォーカスを移動して、システムは、現在フォーカスがある項目を表示したままにインターフェイスを自動的にスクロールします。 アプリでは、この種類の対話を実装する場合は、ユーザーのジェスチャの方向にフォーカスを移動することを確認します。 したがって、ユーザーが権限をスワイプ Siri リモート フォーカスする必要があります (左にスクロールする画面が発生する可能性があります) を右に移動します。 このルールに 1 つの例外は、全画面表示の項目 (場所をスワイプ上方向へ移動した要素) を直接操作に使用します。
- **フォーカスされた項目が Obvious であることを確認**-ため遠方から、UI 要素には、ユーザーが対話する、その重要では、現在フォーカスがある項目が目立ちます。通常これは自動的に処理組み込みで`UIKit`要素。 カスタム コントロールの項目のサイズまたは shadow などの機能を使用して、フォーカスを表示します。
- **ください重点を置いた項目応答を視差を使用して**、小さな - Siri リモート循環ジェスチャがフォーカスのあるアイテムの移動を少し、リアルタイムで結果します。 これは、[視差効果](#Focus-and-Parallax)に組み込まれて`UIKit`_層イメージ_フォーカスのあるアイテムへの接続の意味をユーザーに提供します。
- **適切なサイズのフォーカス可能な項目を作成**-十分な間隔での大規模な項目が選択し、小さい項目よりも移動しやすくします。
- **検索良いか重点を置いてまたは Unfocused する UI 要素をデザイン**-通常 Apple TV のサイズを増やすことでフォーカスされた項目を表します。 アプリの UI 要素のプレゼンテーション サイズを問わず、見栄えし、必要な場合、入力するサイズの大きい要素の資産を確認します。
- **フォーカス変更流動的を表す**-アニメーションを使用して、項目間をスムーズにフェードする**Focused**と**Unfocused**少し目障りな感じが遷移を保持する状態。
- **カーソルを表示しない**-ユーザーがフォーカスを使用して、アプリの UI を移動するには、画面の周りのカーソルの移動ではなくです。 ユーザー インターフェイスには、して一貫性のあるユーザー エクスペリエンスを提供フォーカス モデルを常に使用する必要があります。

<a name="Working-with-Focus" />

### <a name="working-with-focus"></a>フォーカスの操作

フォーカスを設定できる項目となるカスタム コントロールを作成する時間である可能性があります。 場合ためオーバーライド、`CanBecomeFocused`プロパティと戻り`true`それ以外の場合に戻り、`false`です。 例えば:

```csharp
public class myView : UIView
{
    public override bool CanBecomeFocused {
        get {return true;}
    }
}
```

いつでも行うこともできます、`Focused`のプロパティ、`UIKit`それが現在の項目であるかどうかを制御します。 場合`true`UI 項目現在フォーカスがある、それ以外の場合はありません。 例えば:

```csharp
// Is my view in focus?
if (myView.Focused) {
    // Do something
    ...
}
```

UI 要素を設定して、画面が読み込まれるときに最初にフォーカスを取得を指定するにはコードを使用して、他の UI 要素にフォーカスを移動することはできません直接、中にその`PreferredFocusedView`プロパティを`true`です。 例えば:

```csharp
// Make the play button the starting focus item
playButton.PreferredFocusedView = true;
```

<a name="Working-with-Focus-Updates" />

### <a name="working-with-focus-updates"></a>フォーカスの更新プログラムの操作 

ユーザーがフォーカスを 1 つの UI 要素から移動 (たとえば、Siri リモート ジェスチャに応答) 内の別がどのように発生する場合、_フォーカス更新イベント_フォーカスを失う項目とフォーカスを得た項目の両方に送信されます。

継承するカスタムの要素の`UIView`または`UIViewController`フォーカスの更新イベントを使用するいくつかのメソッドをオーバーライドすることができます。

- **DidUpdateFocus** -このメソッドにいつでも、ビューを取得したりがフォーカスを失ったが呼び出されます。
- **ShouldUpdateFocus** -このメソッドを使用して、フォーカスの移動が許可されているかを定義します。

フォーカスがバックアップ要求エンジンは、フォーカスが移動する、 `PreferredFocusedView` UI 要素を呼び出し、`SetNeedsUpdateFocus`ビュー コント ローラーのメソッドです。

> [!IMPORTANT]
> 呼び出す`SetNeedsUpdateFocus`に対して呼び出されたビュー コント ローラーには、現在フォーカスがあるビューが含まれている場合にのみ効果がします。




<a name="Working-with-Focus-Guides" />

### <a name="working-with-focus-guides"></a>フォーカスのガイドを使用した作業

TvOS に組み込まれたフォーカス エンジンは、水平方向および垂直のグリッドで該当する項目の間の移動を処理するに最適です。 通常、インターフェイスの設計とフォーカス エンジンに UI 要素は開発者の介入なしには、その要素間の移動を自動的に処理を追加するのではより、何もする必要があります。

ただし、ある可能性があります、回、UI 設計の必要なもののため UI 要素が水平および垂直のグリッドで分類できないし、アクセスできない互いに斜めいるため、します。 これは、フォーカス エンジンがダウンしている項目のみを UI 間の Left と Right の移動、単純な処理に設計されていたために発生します。

例については、次の UI のレイアウトを実行します。

 [![](navigation-focus-images/guide01.png "フォーカス ガイドの例の操作")](navigation-focus-images/guide01.png#lightbox)
 
**詳細**ボタンと水平および垂直のグリッドには当てはまらない、**購入**ではない、ユーザーがアクセスできるボタンをクリックします。 ただし、これを簡単に修正するを使用して、_フォーカス ガイド_フォーカス エンジンへの移動のヒントを提供します。 

フォーカス ガイド (`UIFocusGuide`) フォーカス エンジンにフォーカスを別のビューにリダイレクトできるように Focusable としてビューの非表示領域を公開します。

したがって、上記の例を解決するために、次のコードに追加できませんでした間でフォーカス ガイドを作成するビュー コント ローラー、**詳細**と**購入**ボタン。

```csharp
public UIFocusGuide FocusGuide = new UIFocusGuide ();
...

public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Add Focus Guide to layout
    View.AddLayoutGuide (FocusGuide);

    // Define Focus Guide that will allow the user to move
    // between the More Info and Buy buttons.
    FocusGuide.LeftAnchor.ConstraintEqualTo (BuyButton.LeftAnchor).Active = true;
    FocusGuide.TopAnchor.ConstraintEqualTo (MoreInfoButton.TopAnchor).Active = true;
    FocusGuide.WidthAnchor.ConstraintEqualTo (BuyButton.WidthAnchor).Active = true;
    FocusGuide.HeightAnchor.ConstraintEqualTo (MoreInfoButton.HeightAnchor).Active = true;
}
```

最初に、新しい`UIFocusGuide`が作成され、ビューのレイアウト ガイドを使用してコレクションに追加された、`AddLayoutGuide`メソッドです。

に対して相対的なフォーカス ガイドの Top、Left、幅、高さアンカーを次に、調整、**詳細**と**購入**それらの間に配置するためにボタン。 参照トピック

[![](navigation-focus-images/guide02.png "例のフォーカス ガイド")](navigation-focus-images/guide02.png#lightbox)

新しい制約されているアクティブ化されるように設定して作成されたに注意する必要も、`Active`プロパティを`true`:

```csharp
FocusGuide.LeftAnchor.ConstraintEqualTo (...).Active = true;
```

ガイドを使用して、新しいフォーカスが確立され、ビューをビュー コント ローラーの追加`DidUpdateFocus`メソッドをオーバーライドして、間を移動する、次のコードを追加、**詳細**と**購入**ボタン。

```csharp
public override void DidUpdateFocus (UIFocusUpdateContext context, UIFocusAnimationCoordinator coordinator)
{
    base.DidUpdateFocus (context, coordinator);

    // Get next focusable item from context
    var nextFocusableItem = context.NextFocusedView;

    // Anything to process?
    if (nextFocusableItem == null) return;

    // Decide the next focusable item based on the current
    // item with focus
    if (nextFocusableItem == MoreInfoButton) {
        // Move from the More Info to Buy button
        FocusGuide.PreferredFocusedView = BuyButton;
    } else if (nextFocusableItem == BuyButton) {
        // Move from the Buy to the More Info button
        FocusGuide.PreferredFocusedView = MoreInfoButton;
    } else {
        // No valid move
        FocusGuide.PreferredFocusedView = null;
    }
}
```

最初に、このコードの取得の`NextFocusedView`から、`UIFocusUpdateContext`に渡されました (`context`)。 このビューは、する場合`null`処理が必要でないと、メソッドを終了しました。

次に、`nextFocusableItem`が評価されます。 いずれかと一致する場合、**詳細**または**購入**ボタン、フォーカス ガイドを使用して、[逆] ボタンにフォーカスが送信`PreferredFocusedView`プロパティです。 例えば:

```csharp
// Move from the More Info to Buy button
FocusGuide.PreferredFocusedView = BuyButton;
```

ボタンもフォーカス シフトのソースであること、`PreferredFocusedView`プロパティをクリアします。

```csharp
// No valid move
FocusGuide.PreferredFocusedView = null;
```

<a name="Working-with-Focus-in-Collections" />

### <a name="working-with-focus-in-collections"></a>コレクション内のフォーカスの操作

個々 の項目でフォーカスを設定することができるかどうかを決定する際、`UICollectionView`または`UITableView`のメソッドをオーバーライドします、`UICollectionViewDelegate`または`UITableViewDelegate`それぞれします。 例えば:

```csharp
public class CardHandDelegate : UICollectionViewDelegateFlowLayout
{
    ...
    public override bool CanFocusItem (UICollectionView collectionView, NSIndexPath indexPath)
    {
        if (indexPath == null) {
            return false;
        } else {
            var controller = collectionView as CardHandViewController;
            return !controller.Hand [indexPath.Row].IsFaceDown;
        }
    }
}
```

`CanFocusItem`メソッドを返します。`true`場合は、現在のアイテムは、フォーカスすることができます、それ以外を返します`false`です。

場合は、`UICollectionView`または`UITableView`に記憶でき、フォーカスを戻す最後の項目失くし、フォーカスを得たときに、設定、`RemembersLastFocusedIndexPath`プロパティを`true`です。

<a name="Focus-and-Parallax" />

## <a name="focus-and-parallax"></a>フォーカス イベントと視差

前述のように、ユーザー インターフェイス要素と見なされます_フォーカス_ナビゲーション イベント中に、現在のアイテムである場合。 項目にフォーカスが移ったらと通常のサイズが多少増加し、影の使用は視覚的に昇格します。 

した場合、ユーザーが低速で循環ジェスチャ Siri リモート、フォーカスされた項目がこの移動への応答でリアルタイムかきたてるされます。 発生して、影響を与える、明るい光沢が際立つに表示される画面を作成、イメージに適用されます。 非アクティブな時間が一定時間後に焦点のコンテンツをすべて使用できなくなります、Focused 項目をそれ以上に拡張されます。 

これらの効果は連携してテレビ画面上のコンテンツと、ソファから Apple TV と対話するユーザーの間、頭の中の接続を提供します。

Apple TV を 3D 奥行きとフォーカス設定項目を dynamics の意味を伝えるために、システム全体でこの視差効果が使用されます。 tvOS が透明の系列を使用して[層イメージ](~/ios/tvos/app-fundamentals/icons-images.md#Layered-Images)移動され、この特殊効果を作成するのに動的にスケールを設定します。

画像のレイヤーは、tvOS アプリのアイコンに必要なし、上部棚の動的なコンテンツのサポートされます。 必須ではありません Apple を強くお勧めアプリの UI にフォーカスを設定できるその他の項目でのイメージの階層型の実装です。

### <a name="enabling-parallax"></a>視差を有効にします。

`UIImageView`コントロール (または任意のコントロールから継承する`UIImageView`) 視差効果を自動的にサポートします。 既定では、このサポートは無効になっている、次のコードを使用することを有効にします。

```csharp
myImageView.AdjustsImageWhenAncestorFocused = true;
```

このプロパティ設定して`true`イメージのビューを自動的に取得視差効果とフォーカスのあるユーザーが選択されています。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、フォーカス イベントとその使用 Xamarin.tvOS アプリのユーザー インターフェイスでのナビゲーションを処理する方法の概念について説明しました。 これは、ナビゲーションを提供する、組み込み tvOS ナビゲーション コントロールがフォーカスを強調表示と選択を使用する方法を確認します。 次に、エンドユーザーに現在のナビゲーション状態を視覚的手がかりを提供するフォーカスを視差とイメージの層で使用する方法が説明しました。 最後に、フォーカス、フォーカスの更新プログラム、コレクションと視差の有効化にフォーカスがある作業を確認します。




## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマン インターフェイス ガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS のアプリケーション プログラミング ガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
