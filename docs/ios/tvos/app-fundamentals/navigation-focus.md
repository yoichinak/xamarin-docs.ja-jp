---
title: ナビゲーションとフォーカス Xamarin で tvOS の操作
description: この記事では、フォーカスとその使用を提示して Xamarin.tvOS アプリ内でナビゲーションを処理する方法の概念について説明します。
ms.prod: xamarin
ms.assetid: DD72E95F-AE9B-47D2-B132-5FA5FBD8026E
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 3cb8d1c1d92146e70056c6cf562f2fa1cb028e7c
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61416449"
---
# <a name="working-with-tvos-navigation-and-focus-in-xamarin"></a>ナビゲーションとフォーカス Xamarin で tvOS の操作

_この記事では、フォーカスとその使用を提示して Xamarin.tvOS アプリ内でナビゲーションを処理する方法の概念について説明します。_


この記事の概念を紹介します[フォーカス](#Focus-and-Selection)の処理に使用する方法と[ナビゲーション](#Navigation)Xamarin.tvOS アプリのユーザー インターフェイスでします。 Xamarin.tvOS アプリのユーザー インターフェイスのナビゲーションを提供する、組み込みの tvOS ナビゲーション コントロールがフォーカスを強調表示と選択を使用する方法について説明します。

[![](navigation-focus-images/intro01.png "tvOS アプリのユーザー インターフェイスのナビゲーション")](navigation-focus-images/intro01.png#lightbox)

フォーカスを使用する方法を見てをみましょう次に、[視差](#Focus-and-Parallax)と*階層型イメージ*をユーザーがナビゲーションの現在の状態を視覚的な手がかりを提供します。

使用方法について説明します最後に、[フォーカス](#Working-with-Focus)、[フォーカス更新](#Working-with-Focus-Updates)、[フォーカス ガイド](#Working-with-Focus-Guides)、[コレクション内のフォーカス](#Working-with-Focus-in-Collections)と[視差効果を有効にする](#enabling-parallax)Xamarin.tvOS アプリでのイメージ ビュー。

<a name="Navigation" />

## <a name="navigation"></a>ナビゲーション

Xamarin.tvOS アプリのユーザーがいないするインターフェイスとの対話の直接として iOS の場所をタップしたいないデバイスの画面で、イメージが直接からルームを使用して、全体では、 [Siri のリモート](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote)します。 フローを当然ながら、まだユーザーの Apple TV のエクスペリエンスに専念した状態を維持するために、アプリのユーザー インターフェイスを設計するときに注意してくださいこれを保持する必要があります。

成功した tvOS アプリをスムーズに、アプリの目的は、およびナビゲーション自体に注意を呼び出さずに提示データの構造をサポートする方法でナビゲーションを実装します。 自然かつ使い慣れたユーザー インターフェイスを占有することや、コンテンツとアプリのユーザー エクスペリエンスからフォーカスを描画を感じるようにナビゲーションを設計します。

[![](navigation-focus-images/nav01.png "TvOS 設定アプリ")](navigation-focus-images/nav01.png#lightbox)

中に、ユーザーの Apple TV が、通常、使用するが移動、画面のスタックされたセット各な特定の一連のコンテンツを表示します。 さらに、すべての新しい画面などの標準の UI コントロールを使用するコンテンツを 1 つまたは複数のサブ画面にあります[ボタン](~/ios/tvos/user-interface/buttons.md)、[タブ バー](~/ios/tvos/user-interface/tab-bars.md)、テーブル、[コレクション ビュー](~/ios/tvos/user-interface/collection-views.md)または。[分割ビュー](~/ios/tvos/user-interface/split-views.md)します。

データの新しい各画面で、ユーザーでは画面のこのスタックにさらに移動します。 使用して、**メニュー**ボタン Siri のリモートで前の画面またはメイン メニューに戻るスタックを後方移動できます。

Apple では、tvOS アプリのナビゲーションを設計する際に注意してください、次を維持する方法を示しています。

- **レイアウト、ナビゲーション、検索コンテンツ高速かつ容易に**-などの最小数で、アプリ内でコンテンツにアクセスするユーザーが求めるをクリックし、できるだけを読み取ります。 ナビゲーションを簡素化し、画面の最小限の番号へのコンテンツを整理します。
- **作成、流体インターフェイスを使用してタッチ**-ユーザー間の移動できることを確認します。_フォーカスを設定できる項目_可能なジェスチャの最小数を使用して、最小限の手間で。
- **デザインを考慮してフォーカスを持つ**-Siri のリモートを使用して通信する前に、ユーザー インターフェイス項目にフォーカスを移動する必要があります、ため、ユーザーが部屋のコンテンツを操作する、します。 多すぎるのジェスチャの目標を実現することが必要な場合は、ユーザーがアプリに不満を感じるに表示されます。
- **メニュー ボタンを使用して旧バージョンとナビゲーションを提供**下位 Siri のリモートを使用して移動するように、使いやすく、使い慣れたエクスペリエンスを作成する -**メニュー**ボタンをクリックします。 キーを押して、**メニュー**ボタンが、前の画面に戻ります常に、またはアプリのメイン メニューに戻る必要があります。 レベル、アプリの上部にあるキーを押して、**メニュー**ボタンは、Apple TV ホーム画面に返す必要があります。
- **戻るボタンを表示しないように通常**ためキーを押してに、**メニュー** Siri リモコンのボタンに戻ります画面スタックを通じて、この動作を複製する余分なコントロールを表示しないようにします。 このルールの例外は、画面または画面 (コンテンツを削除する) などの破壊的な操作での購入の場所、**キャンセル**もボタンを表示する必要があります。
- **多くの代わりに、1 つの画面に大規模なコレクションを表示する**-Siri リモート クイック コンテンツの大規模なコレクションを移動およびジェスチャを使用して簡単に設計されました。 フォーカスを設定できる項目の大規模なコレクションを使用して、アプリが動作する場合は、それらを複数のナビゲーション、ユーザーの一部を必要とする多くの画面に互換性に影響することではなく 1 つの画面上に保持することを検討してください。
- **標準のコントロールを使用して、ナビゲーションの**-もう一度、可能な限り、使いやすく、使い慣れたユーザー エクスペリエンスを作成するに組み込みの使用`UIKit`ページ コントロール、タブ バー、セグメント化されたコントロール、テーブルのビュー、コレクション ビューや分割などのコントロールアプリのナビゲーションのビュー。 ユーザーがこれらの要素でを理解しているためにが直感的にできるアプリを移動します。
- **優先するコンテンツの水平ナビゲーション**-Apple TV の性質上をアップ/ダウンより自然には Siri のリモートで左から右へスワイプがあります。 アプリのコンテンツのレイアウトを設計するときに、このオプションを検討してください。

<a name="Focus-and-Selection" />

## <a name="focus-and-selection"></a>フォーカスと選択

Apple TV のイメージ、ボタン、またはその他の UI 要素と見なされます_フォーカス_の現在のナビゲーション ターゲットがある場合。

[![](navigation-focus-images/focus01.png "フォーカスと選択の例")](navigation-focus-images/focus01.png#lightbox)

異なり、デバイスのタッチ スクリーン上の要素と、ユーザーが直接やり取りする、iOS デバイス、部屋の Siri リモコンを使用してユーザーが対話 tvOS 要素を使用します。 Apple TV を使用するには存在し、このユーザーの操作を処理、_フォーカス_ベースのモデル。

押すジェスチャとボタンを使用して、 [Siri のリモート](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote)ユーザーでは、1 つの UI 要素からフォーカスを移動します。 目的の要素にフォーカスの移動、したら、コンテンツを選択するか、その要素によって表されるアクションをアクティブ化するユーザーをクリックします。

フォーカスの変更として繊細なアニメーションと (イメージの視差効果) などの効果は、現在フォーカスがあるユーザー インターフェイス項目を明確に識別するために使用されます。

Apple では、フォーカスと選択を操作するための次の推奨事項があります。

- **組み込みの UI コントロールを使用して、アニメーション効果**- を使用して`UIKit`フォーカス モデルのユーザー インターフェイスにフォーカス API は自動的に適用、既定のアニメーションと視覚効果、UI 要素にします。 これにより、ネイティブおよび Apple TV のプラットフォームのユーザーが使い慣れていると思われるアプリし、フォーカスを設定できる項目間の滑らかで直感的な移動できます。
- **期待どおりの方向にフォーカスを移動する**-Apple TV では、ほぼすべての要素は、間接の操作を使用します。 たとえば、ユーザーでは、Siri のリモートを使用して、フォーカスを移動して、システムは、現在フォーカスがある項目を表示したままにインターフェイスを自動的にスクロールします。 アプリでは、この種類の対話を実装する場合は、ユーザーのジェスチャの方向にフォーカスを移動するを確認します。 したがって、ユーザーが適切にスワイプする場合は、Siri のリモート フォーカスが (考えられる原因を左にスクロール画面) を右に移動する必要があります。 このルールの 1 つの例外は、(上方向にスワイプ移動する要素) を直接操作を使用して全画面表示の項目です。
- **フォーカスされた項目が Obvious であることを確認**-が現在フォーカスがある項目が目立つ重要遠方から UI 要素と、ユーザーの対話、ためです。通常、これによって処理される自動的に組み込み`UIKit`要素。 カスタム コントロールは、アイテムのサイズや影などの機能を使用して、フォーカスを表示します。
- **視差、重点を置いた項目応答性を使用して**小さな - Siri のリモートでの循環のジェスチャはフォーカスがあるアイテムの移動を穏やかな、リアルタイムで結果します。 これは、[視差効果](#Focus-and-Parallax)に組み込まれている`UIKit`_階層型イメージ_にフォーカスがあるアイテムへの接続の意味をユーザーに与えます。
- **適切なサイズのフォーカスを設定できる項目を作成する**-十分な間隔での大規模な項目が選択し、小さい項目よりも移動しやすくします。
- **検索よいか、重点を置いたまたは Unfocused に UI 要素をデザイン**-Apple TV 通常のサイズを増やすことで、重点を置いた項目を表します。 いることを確認、アプリの UI 要素、プレゼンテーションのサイズで美しく、必要な場合は、サイズの大きい要素のために資産を指定します。
- **流動的にフォーカスの変更を表す**-項目の間でスムーズにフェードインするアニメーションを使用して**Focused**と**Unfocused**少し目障りな感じが遷移を保持する状態。
- **カーソルを表示しない**-ユーザー フォーカスを使用して、アプリの UI を移動することを期待し、画面の周りのカーソルの移動ではなく。 ユーザー インターフェイスは、一貫性のあるユーザー エクスペリエンスを提供するのにフォーカス モデルを常に使用する必要があります。

<a name="Working-with-Focus" />

### <a name="working-with-focus"></a>フォーカスの操作

フォーカスを設定できる項目となるカスタム コントロールを作成し時間である可能性があります。 場合これをオーバーライド、`CanBecomeFocused`プロパティと戻り`true`, それ以外の場合、return`false`します。 例えば:

```csharp
public class myView : UIView
{
    public override bool CanBecomeFocused {
        get {return true;}
    }
}
```

いつでも使用することができます、`Focused`のプロパティを`UIKit`それが現在の項目であるかどうかを制御します。 場合`true`UI 項目現在フォーカスがある、それ以外の場合そうでないです。 例:

```csharp
// Is my view in focus?
if (myView.Focused) {
    // Do something
    ...
}
```

UI 要素は、設定して、画面が読み込まれるときに最初にフォーカスを取得を指定するには直接コードを使用して別の UI 要素にフォーカスを移動することはできません、その`PreferredFocusedView`プロパティを`true`します。 例:

```csharp
// Make the play button the starting focus item
playButton.PreferredFocusedView = true;
```

<a name="Working-with-Focus-Updates" />

### <a name="working-with-focus-updates"></a>フォーカスの更新プログラムの操作 

ユーザーがフォーカスを別 (たとえば、Siri のリモートのジェスチャに応答) の 1 つの UI 要素から移動によってがされる場合、_フォーカス更新イベント_項目のフォーカスを失うと、項目がフォーカスを得たの両方に送信されます。

継承するカスタム要素の`UIView`または`UIViewController`フォーカスの更新イベントを使用するいくつかのメソッドをオーバーライドすることができます。

- **DidUpdateFocus** -このメソッドは、ビューを取得したりがフォーカスを失ったときに呼び出します。
- **ShouldUpdateFocus** -このメソッドを使用して、フォーカスの移動が許可された場所を定義します。

フォーカスが戻りますフォーカス エンジンが移動を要求する、 `PreferredFocusedView` UI 要素を呼び出し、`SetNeedsUpdateFocus`ビュー コント ローラーのメソッド。

> [!IMPORTANT]
> 呼び出す`SetNeedsUpdateFocus`に対して呼び出されたビュー コント ローラーには、現在フォーカスがあるビューが含まれている場合にのみ機能します。




<a name="Working-with-Focus-Guides" />

### <a name="working-with-focus-guides"></a>フォーカスのガイドの使用

TvOS に組み込まれているフォーカス エンジンは水平および垂直のグリッドに該当する項目間の移動を処理する場合に最適です。 通常、インターフェイスのデザインとフォーカス エンジンに UI 要素には開発者が関与せず、それらの要素間の移動が自動的に処理を追加するよりも、何もする必要があります。

ただし、ありますあります、ため、UI 設計のための UI 要素を水平および垂直のグリッドに分類されないし、アクセスできない互いに斜線が。 これは、フォーカス エンジンは、下、左と右の移動 UI 項目のみの間で、単純に処理するために設計されていたために発生します。

例については、次の UI レイアウトを実行します。

 [![](navigation-focus-images/guide01.png "フォーカスのガイドの例の操作")](navigation-focus-images/guide01.png#lightbox)
 
**さらに情報**ボタンを水平および垂直のグリッドに当てはまらない、**購入**できなくなる、ユーザーがアクセスできるボタンをクリックします。 ただし、これは簡単に修正を使用して、_フォーカス ガイド_フォーカス エンジンへの移動のヒントを提供します。 

フォーカス ガイド (`UIFocusGuide`) Focusable にフォーカスを別のビューにリダイレクトできます。 つまり、フォーカス エンジンとして、ビューの非表示の領域を公開します。

そのため、上記の例を解決するために、次のコードに追加できるの間でフォーカスのガイドを作成するビュー コント ローラー、**詳細**と**購入**ボタン。

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

最初に、新しい`UIFocusGuide`が作成され、ビューのレイアウト ガイドを使用してコレクションに追加、`AddLayoutGuide`メソッド。

基準とするフォーカス ガイドの一番上、左、幅、高さのアンカーを次に、調整、**詳細**と**購入**それらの間に配置するボタン。 参照トピック

[![](navigation-focus-images/guide02.png "例のフォーカス ガイド")](navigation-focus-images/guide02.png#lightbox)

設定して作成される新しい制約をアクティブ化は注意しても、`Active`プロパティを`true`:

```csharp
FocusGuide.LeftAnchor.ConstraintEqualTo (...).Active = true;
```

新しいフォーカス ガイドが確立され、ビュー、ビュー コント ローラーの追加と`DidUpdateFocus`メソッドをオーバーライドしての間を移動する、次のコードを追加、**詳細**と**購入**ボタン。

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

最初に、このコードの取得の`NextFocusedView`から、`UIFocusUpdateContext`で渡された (`context`)。 このビューは、する場合`null`処理は必要ありませんし、メソッドを終了しました。

次に、`nextFocusableItem`が評価されます。 いずれかと一致する場合、**詳細**または**購入**ボタン、フォーカスはフォーカスのガイドを使用して逆ボタンに送信される`PreferredFocusedView`プロパティ。 例えば:

```csharp
// Move from the More Info to Buy button
FocusGuide.PreferredFocusedView = BuyButton;
```

ボタンもフォーカス shift キーを押しのソースであること、`PreferredFocusedView`プロパティをクリアします。

```csharp
// No valid move
FocusGuide.PreferredFocusedView = null;
```

<a name="Working-with-Focus-in-Collections" />

### <a name="working-with-focus-in-collections"></a>コレクション内のフォーカスの操作

個々 の項目にフォーカスを設定できるできるかどうかを決定する際に、`UICollectionView`または`UITableView`のメソッドをオーバーライドします、`UICollectionViewDelegate`または`UITableViewDelegate`それぞれします。 例:

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

`CanFocusItem`メソッドを返します。`true`場合は、現在の項目は、フォーカスは、それ以外の場合を返します`false`します。

場合は、`UICollectionView`または`UITableView`を記憶し、フォーカスを復元する最後の項目を失くし、フォーカスを得たとき、設定、`RemembersLastFocusedIndexPath`プロパティを`true`します。

<a name="Focus-and-Parallax" />

## <a name="focus-and-parallax"></a>フォーカスと視差効果

前述のように、ユーザー インターフェイス要素と見なされます_フォーカス_ナビゲーション イベント中に、現在の項目が。 項目にフォーカスが移ったら、通常はのサイズが若干増加し、影を使用しては視覚的に昇格します。 

ユーザーが低速、循環ジェスチャを行った Siri のリモート、重点を置いた項目がこの移動への応答でリアルタイム sway されます。 Sway が発生すると、明るい光沢が真価を発揮する表示画面を作成、イメージに適用されます。 一定の非アクティブですが後の焦点のずれたコンテンツが淡色表示し、Focused 項目がさらに大きく成長します。 

これらの効果が連携して、テレビ画面上のコンテンツと、ソファから Apple TV と対話するユーザーの間の頭の中の接続を提供します。

Apple TV では、この視差効果が 3D の奥行きとフォーカスのアイテムに dynamics の意味を伝達するために、システム全体で使用されます。 tvOS の透過的な場合は、一連の使用[階層型イメージ](~/ios/tvos/app-fundamentals/icons-images.md#Layered-Images)移動され、この効果を作成するを動的にスケーリングします。

階層化されたイメージは、tvOS アプリのアイコンに必要なし、トップ シェルフの動的なコンテンツではサポートされています。 必須ではありません、Apple は、アプリの UI の他のフォーカスを設定できる項目のイメージの層を実装する強くお勧めします。

### <a name="enabling-parallax"></a>視差効果を有効にします。

`UIImageView`コントロール (または任意のコントロールから継承する`UIImageView`) 視差効果を自動的にサポートします。 既定では、次のコードを使用することを有効にする、このサポートを無効。 にします。

```csharp
myImageView.AdjustsImageWhenAncestorFocused = true;
```

このプロパティ設定して`true`、イメージ ビューが自動的に取得視差効果を選択し、ユーザーがフォーカスを設定します。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、フォーカスと Xamarin.tvOS アプリのユーザー インターフェイスでのナビゲーションを処理するために使用される方法の概念について説明しました。 ナビゲーションを提供する、組み込みの tvOS ナビゲーション コントロールがフォーカスを強調表示と選択を使用する方法を確認します。 次に、エンドユーザーに現在のナビゲーションの状態を視覚的手がかりを提供するフォーカスを視差とイメージの階層化を使用する方法が説明しました。 最後に、フォーカス、フォーカスの更新プログラム、コレクションと視差効果を有効にするフォーカスの操作を調査します。




## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマン インターフェイス ガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS 用のアプリのプログラミング ガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
