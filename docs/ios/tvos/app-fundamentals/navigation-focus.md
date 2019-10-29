---
title: TvOS ナビゲーションを使用して Xamarin にフォーカスを移動する
description: この記事では、フォーカスの概念と、tvOS アプリ内のナビゲーションの表示と処理に使用する方法について説明します。
ms.prod: xamarin
ms.assetid: DD72E95F-AE9B-47D2-B132-5FA5FBD8026E
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: 3c754acc3502d7aa2c47264e734187ffe060c029
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030837"
---
# <a name="working-with-tvos-navigation-and-focus-in-xamarin"></a>TvOS ナビゲーションを使用して Xamarin にフォーカスを移動する

_この記事では、フォーカスの概念と、tvOS アプリ内のナビゲーションの表示と処理に使用する方法について説明します。_

この記事では、[フォーカス](#Focus-and-Selection)の概念と、tvOS アプリのユーザーインターフェイスでの[ナビゲーション](#Navigation)の処理に使用する方法について説明します。 ここでは、組み込みの tvOS ナビゲーションコントロールでフォーカス、強調表示、および選択を使用して、tvOS アプリのユーザーインターフェイスナビゲーションを提供する方法について説明します。

[![](navigation-focus-images/intro01.png "tvOS apps User Interface Navigation")](navigation-focus-images/intro01.png#lightbox)

次に、[視差](#Focus-and-Parallax)およびレイヤー化された*イメージ*でフォーカスを使用して、エンドユーザーに現在のナビゲーション状態を視覚的に把握する方法について説明します。

最後に、[フォーカス](#Working-with-Focus)を操作し、[更新プログラム](#Working-with-Focus-Updates)、[フォーカスガイド](#Working-with-Focus-Guides)、[コレクションに焦点](#Working-with-Focus-in-Collections)を当て、TvOS アプリのイメージビューで[視差を有効にする](#enabling-parallax)方法について説明します。

<a name="Navigation" />

## <a name="navigation"></a>ナビゲーション

TvOS アプリのユーザーは、iOS と直接やり取りするのではなく、デバイスの画面上のイメージをタップしますが、 [Siri リモート](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote)を使用してルーム間で間接的に移動します。 アプリのユーザーインターフェイスをデザインするときは、この点に留意する必要があります。これにより、ユーザーは Apple TV エクスペリエンスに専念ことができます。

成功した tvOS アプリは、アプリケーションの目的をスムーズにサポートするような方法でナビゲーションを実装し、ナビゲーション自体に注意を払わずに表示されるデータの構造を実装します。 ユーザーインターフェイスを占め、またはコンテンツやアプリのユーザーエクスペリエンスに焦点を当てることなく、自然でなじみのあるナビゲーションをデザインします。

[![](navigation-focus-images/nav01.png "The tvOS settings app")](navigation-focus-images/nav01.png#lightbox)

Apple TV を使用する場合、ユーザーは通常、一連の画面に移動し、それぞれが特定のコンテンツのセットを提示します。 また、新しい画面では、[ボタン](~/ios/tvos/user-interface/buttons.md)、[タブバー](~/ios/tvos/user-interface/tab-bars.md)、テーブル、[コレクションビュー](~/ios/tvos/user-interface/collection-views.md) 、[分割ビュー](~/ios/tvos/user-interface/split-views.md)などの標準 UI コントロールを使用して、コンテンツの1つ以上のサブ画面が表示されることがあります。

データの新しい画面が表示されるたびに、ユーザーは画面のこのスタックに深く移動します。 Siri リモートの**メニュー**ボタンを使用すると、スタック内を逆方向に移動して、前の画面またはメインメニューに戻ることができます。

Apple では、tvOS アプリのナビゲーションを設計するときに、次の点に留意することをお勧めします。

- ナビゲーションをレイアウトして、**コンテンツをすばやく簡単に検索できるよう**にします。ユーザーは、可能な限り少ないタップ数で、アプリ内のコンテンツにアクセスする必要があります。 ナビゲーションを単純化し、最小限の数の画面にコンテンツを整理します。
- **タッチインターフェイスを作成**する-ユーザーが可能な限り少ないジェスチャを使用して、フォーカスがある_項目_間を最小限の摩擦で移動できることを確認します。
- **フォーカスを考慮した設計**-ユーザーが部屋全体でコンテンツを操作しているため、Siri リモートを使用して操作する前に、ユーザーインターフェイスの項目にフォーカスを移動する必要があります。 ユーザーの目標を達成するために必要なジェスチャが多すぎると、ユーザーはアプリに不満を感じます。
- **メニューボタン**を使用して後方ナビゲーションを提供する-簡単で使いやすいエクスペリエンスを作成し、Siri リモートの**メニュー**ボタンを使用してユーザーが後方に移動できるようにします。 **メニュー**ボタンを押すと、常に前の画面に戻るか、アプリのメインメニューに戻ります。 アプリの最上位レベルで、**メニュー**ボタンを押すと Apple TV ホーム画面に戻ります。
- **通常、[戻る] ボタンを表示しないよう**にします。これは、Siri リモートの**メニュー**ボタンを押して、画面スタックを後方に移動するためです。この動作を複製する追加のコントロールを表示しないようにしてください。 このルールの例外は、 **[キャンセル**] ボタンが表示される必要がある、(コンテンツの削除などの) 破壊的なアクションを含む画面や画面を購入する場合です。
- **大きなコレクションを1つの画面に表示します。多くの場合**は使用しません。 Siri リモコンは、ジェスチャを使用して、コンテンツの大規模なコレクションをすばやく簡単に移動できるように設計されています。 アプリが、フォーカス可能な項目の大きなコレクションで動作する場合は、ユーザー側でより多くのナビゲーションを必要とする多くの画面に分割するのではなく、1つの画面に保持することを検討してください。
- **ナビゲーションのための標準コントロールを使用**する-ここでも、可能な限り、簡単で使い慣れたユーザーエクスペリエンスを作成するために、ページコントロール、タブバー、セグメント化されたコントロール、テーブルビュー、コレクションビュー、分割ビューなどの組み込みの `UIKit` コントロールを使用します。アプリのナビゲーション。 ユーザーはこれらの要素に既に慣れているため、直感的にアプリ内を移動することができます。
- **水平方向のコンテンツナビゲーションを優先**-Apple TV の性質上、Siri リモートでの左から右へのスワイプは、上下により自然になります。 アプリのコンテンツレイアウトをデザインするときは、このオプションを検討してください。

<a name="Focus-and-Selection" />

## <a name="focus-and-selection"></a>フォーカスと選択

Apple TV では、イメージ、ボタン、またはその他の UI 要素は、現在のナビゲーションのターゲットである場合、_フォーカス_されていると見なされます。

[![](navigation-focus-images/focus01.png "Focus and Selection example")](navigation-focus-images/focus01.png#lightbox)

ユーザーがデバイスのタッチスクリーン上の要素を直接操作している iOS デバイスとは異なり、ユーザーは、Siri リモートを使用して、部屋全体の tvOS 要素と対話します。 このユーザーの操作を提示して処理するために、Apple TV は_フォーカス_ベースのモデルを使用します。

[Siri リモート](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote)でジェスチャとボタンの押下を使用すると、ユーザーは UI 要素間でフォーカスを移動します。 フォーカスが目的の要素にシフトされると、ユーザーはクリックしてコンテンツを選択するか、その要素によって表されるアクションをアクティブ化します。

フォーカスが変更されると、微妙なアニメーションや効果 (画像への視差効果など) を使用して、現在フォーカスがあるユーザーインターフェイス項目を明確に識別できます。

Apple には、フォーカスと選択の操作に関して次のような推奨事項があります。

- **モーション効果に組み込みの UI コントロールを使用する**-ユーザーインターフェイスで `UIKit` とフォーカス API を使用することにより、フォーカスモデルは既定のモーションと視覚効果を UI 要素に自動的に適用します。 これにより、アプリがネイティブで、Apple TV platform のユーザーになじみがあり、フォーカスがある項目間で滑らかで直感的な移動が可能になります。
- **必要な方向にフォーカスを移動**する-Apple TV では、ほぼすべての要素が間接操作を使用します。 たとえば、ユーザーは、Siri リモートを使用してフォーカスを移動すると、現在フォーカスがある項目が表示されるように、システムによって自動的にインターフェイスがスクロールされます。 アプリがこの種の相互作用を実装する場合は、フォーカスがユーザーのジェスチャの方向に移動するようにしてください。 そのため、ユーザーが Siri リモートフォーカスを右にスワイプした場合は、右側に移動します (画面が左にスクロールする場合があります)。 このルールの1つの例外は、直接操作を使用する全画面項目です (スワイプアップによって要素が上に移動します)。
- **フォーカスがある項目が明確である**ことを確認します。ユーザーはアファル語から UI 要素を操作しているため、現在フォーカスされている項目が明らかになっていることが重要です。通常、これは組み込みの `UIKit` 要素によって自動的に処理されます。 カスタムコントロールの場合は、項目サイズや影などの機能を使用してフォーカスを表示します。
- 視差を使用して、フォーカスされた**項目の応答性**を小さくし、Siri リモートの円形のジェスチャを使用して、フォーカスがある項目のリアルタイムの移動を実現します。 この[視差効果](#Focus-and-Parallax)は、レイヤー化された_イメージ_`UIKit` に組み込まれており、フォーカスされている項目への接続がユーザーにわかります。
- **適切なサイズのフォーカス**がある項目を作成する-十分な間隔を持つ大きな項目は、より小さい項目よりも簡単に選択して移動できます。
- **UI 要素をデザインして、フォーカスまたは見るを適切に表示**します。通常、Apple TV は、サイズを大きくすることでフォーカスのある項目を表します。 アプリの UI 要素がどのようなプレゼンテーションサイズでも優れていることを確認し、必要に応じて、サイズの大きい要素の資産を提供します。
- **フォーカス変更を表し**ます。流動的を使用して、フォーカスされて**いる項目と** **見る**状態の間をスムーズにフェードし、遷移が jarring にならないようにします。
- **カーソルを表示しない**-ユーザーは、画面の上にカーソルを移動するのではなく、フォーカスを使用してアプリの UI 内を移動することを想定しています。 ユーザーインターフェイスは、常にフォーカスモデルを使用して、一貫性のあるユーザーエクスペリエンスを提供する必要があります。

<a name="Working-with-Focus" />

### <a name="working-with-focus"></a>フォーカスの操作

フォーカス設定可能な項目になる可能性があるカスタムコントロールを作成することが必要になる場合があります。 その場合は、`CanBecomeFocused` プロパティをオーバーライドして `true`を返します。それ以外の場合は `false`を返します。 次に例を示します。

```csharp
public class myView : UIView
{
    public override bool CanBecomeFocused {
        get {return true;}
    }
}
```

`UIKit` コントロールの `Focused` プロパティを使用して、現在の項目であるかどうかをいつでも確認できます。 UI 項目に現在フォーカスがある `true` 場合は。それ以外の場合は。 次に例を示します。

```csharp
// Is my view in focus?
if (myView.Focused) {
    // Do something
    ...
}
```

コードを使用して別の UI 要素にフォーカスを直接移動することはできませんが、その `PreferredFocusedView` プロパティを `true`に設定することにより、画面が読み込まれるときに最初にフォーカスを取得する UI 要素を指定できます。 次に例を示します。

```csharp
// Make the play button the starting focus item
playButton.PreferredFocusedView = true;
```

<a name="Working-with-Focus-Updates" />

### <a name="working-with-focus-updates"></a>フォーカスの更新の操作 

ユーザーがある UI 要素から別の UI 要素に移動する (たとえば、Siri リモートのジェスチャに応答する) 場合、フォーカス_更新イベント_はフォーカスを失う項目とフォーカスを得る項目の両方に送信されます。

`UIView` または `UIViewController`から継承するカスタム要素の場合、フォーカス更新イベントを処理するいくつかのメソッドをオーバーライドできます。

- **DidUpdateFocus** -このメソッドは、ビューでフォーカスが得られるか、フォーカスが失われるたびに呼び出されます。
- **ShouldUpdateFocus** -このメソッドは、フォーカスを移動できる場所を定義するために使用します。

フォーカスエンジンがフォーカスを `PreferredFocusedView` UI 要素に戻すように要求するには、ビューコントローラーの `SetNeedsUpdateFocus` メソッドを呼び出します。

> [!IMPORTANT]
> `SetNeedsUpdateFocus` の呼び出しは、呼び出し元のビューコントローラーに現在フォーカスがあるビューが含まれている場合にのみ有効です。

<a name="Working-with-Focus-Guides" />

### <a name="working-with-focus-guides"></a>フォーカスガイドの操作

TvOS に組み込まれているフォーカスエンジンは、水平グリッドと垂直グリッドに分類される項目間の移動を処理するのに最適です。 通常は、UI 要素をインターフェイスのデザインに追加するだけではなく、開発者が介入することなく、これらの要素間の移動がフォーカスエンジンによって自動的に処理されます。

ただし、ui 要素が水平方向および垂直方向のグリッドに収まらず、互いに斜めに配置されているためにアクセスできない場合は、UI デザインの virtual-machines-windows-classic-ps-sql-alwayson-availability-groups のために時間がかかることがあります。 これは、フォーカスエンジンが、UI 項目間の単純な上下左右の移動を処理するように設計されているために発生します。

例については、次の UI レイアウトを実行してください。

 [![](navigation-focus-images/guide01.png "Working with Focus Guides example")](navigation-focus-images/guide01.png#lightbox)

**[詳細情報]** ボタンは、 **[購入]** ボタンが付いた水平方向および垂直方向のグリッドには収まらないため、ユーザーはアクセスできません。 ただし、フォーカス_ガイド_を使用すると、フォーカスエンジンに移動ヒントを提供することで、簡単に修正できます。 

フォーカスガイド (`UIFocusGuide`) では、フォーカスエンジンにフォーカスを設定できるビュー領域が表示されないため、フォーカスを別のビューにリダイレクトすることができます。

そのため、上記の例を解決するために、ビューコントローラーに次のコードを追加して、 **[詳細]** ボタンと **[購入]** ボタンの間にフォーカスガイドを作成することができます。

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

まず、`AddLayoutGuide` メソッドを使用して、新しい `UIFocusGuide` を作成し、ビューのレイアウトガイドコレクションに追加します。

次に、フォーカスガイドの 上、左、幅、高さ のアンカーは、**詳細情報** と **購入** ボタンを基準にして調整され、それらの間に配置されます。 参照トピック

[![](navigation-focus-images/guide02.png "Example Focus Guide")](navigation-focus-images/guide02.png#lightbox)

また、新しい制約が作成されるときに、その `Active` プロパティを `true`に設定することによってアクティブ化されていることにも注意する必要があります。

```csharp
FocusGuide.LeftAnchor.ConstraintEqualTo (...).Active = true;
```

新しいフォーカスガイドを作成してビューに追加すると、ビューコントローラーの `DidUpdateFocus` メソッドをオーバーライドし、[More] \ (**詳細**\) ボタンと **[購入]** ボタンの間を移動するために次のコードを追加できます。

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

まず、このコードは、渡された `UIFocusUpdateContext` (`context`) の `NextFocusedView` を取得します。 このビューが `null`場合、処理は必要なく、メソッドは終了します。

次に、`nextFocusableItem` が評価されます。 **[詳細]** ボタンまたは **[購入]** ボタンのいずれかに一致する場合は、フォーカスガイドの `PreferredFocusedView` プロパティを使用して、反対のボタンにフォーカスが送信されます。 次に例を示します。

```csharp
// Move from the More Info to Buy button
FocusGuide.PreferredFocusedView = BuyButton;
```

どちらのボタンもフォーカスシフトのソースではない場合、`PreferredFocusedView` プロパティはクリアされます。

```csharp
// No valid move
FocusGuide.PreferredFocusedView = null;
```

<a name="Working-with-Focus-in-Collections" />

### <a name="working-with-focus-in-collections"></a>コレクションでのフォーカスの使用

`UICollectionView` または `UITableView`で個々の項目にフォーカスを設定できるかどうかを判断する場合は、`UICollectionViewDelegate` または `UITableViewDelegate` のメソッドをそれぞれオーバーライドします。 次に例を示します。

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

`CanFocusItem` メソッドは、現在の項目がフォーカスされている場合は `true` を返し、それ以外の場合は `false`を返します。

`UICollectionView` または `UITableView` が、フォーカスを失って再獲得したときに、最後の項目にフォーカスを置いて復元するには、`RemembersLastFocusedIndexPath` プロパティを `true`に設定します。

<a name="Focus-and-Parallax" />

## <a name="focus-and-parallax"></a>フォーカスと視差

既に説明したように、ユーザーインターフェイス要素は、ナビゲーションイベント中の現在の項目_で_あると見なされます。 通常、項目はフォーカスされているので、サイズが少し増え、影を使用して視覚的に昇格されます。 

ユーザーが Siri リモートで低速で円形のジェスチャを実行した場合、フォーカスがある項目は、この移動に応じてリアルタイムで表示されます。 Sway が発生すると、光の光沢が画像に適用され、表面が光に見えるようになります。 一定量の非アクティブな状態が続くと、フォーカスのないコンテンツが淡色表示され、フォーカスがある項目はさらに大きくなります。 

これらの効果は、テレビの画面上のコンテンツと、ソファから Apple TV と対話するユーザーの間に、接続を確保するために連携して機能します。

Apple TV では、この視差効果がシステム全体にわたって使用され、3D の奥行とダイナミクスが焦点を絞った項目に伝えられます。 tvOS は、一連の透過的な[レイヤー](~/ios/tvos/app-fundamentals/icons-images.md#Layered-Images)化されたイメージを使用して、この効果を作成するために動的に移動およびスケーリングします。

TvOS アプリのアイコンにはレイヤー化されたイメージが必要であり、動的な上位シェルフコンテンツに対してサポートされています。 Apple では必須ではありませんが、アプリの UI 内でフォーカスがある他の項目にレイヤーイメージを実装することを強くお勧めします。

### <a name="enabling-parallax"></a>視差の有効化

`UIImageView` コントロール (または `UIImageView`から継承するコントロール) は、視差効果を自動的にサポートします。 既定では、このサポートは無効になっています。有効にするには、次のコードを使用します。

```csharp
myImageView.AdjustsImageWhenAncestorFocused = true;
```

このプロパティが `true`に設定されている場合、イメージビューは、ユーザーが選択してフォーカスを設定したときに視差効果を自動的に取得します。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、フォーカスの概念と、それを使用して tvOS アプリのユーザーインターフェイスでのナビゲーションを処理する方法について説明しました。 ここでは、組み込みの tvOS ナビゲーションコントロールでフォーカス、強調表示、および選択を使用してナビゲーションを行う方法を確認します。 次に、視差とレイヤー化されたイメージでフォーカスを使用して、エンドユーザーに現在のナビゲーション状態を視覚的に把握する方法について見てきました。 最後に、フォーカスを操作し、更新をフォーカスし、コレクションに焦点を当て、視差を有効にします。

## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマンインターフェイスガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS のアプリプログラミングガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
