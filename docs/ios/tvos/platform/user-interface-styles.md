---
title: Xamarin の tvOS ユーザーインターフェイススタイル
description: この記事では、Apple が tvOS 10 に追加した軽量の UI テーマと、tvOS アプリでそれらを実装する方法について説明します。
ms.prod: xamarin
ms.assetid: 8BC37683-AD9E-45CD-BE40-96965618AD1D
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: e779b874cda016a0cd6cc0444ff42a761ee7483e
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86934681"
---
# <a name="tvos-user-interface-styles-in-xamarin"></a>Xamarin の tvOS ユーザーインターフェイススタイル

_この記事では、Apple が tvOS 10 に追加した軽量の UI テーマと、tvOS アプリでそれらを実装する方法について説明します。_

tvOS 10 では、ユーザーの設定に基づいて、すべての組み込み UIKit コントロールが自動的に対応するように、ダークおよびライトの両方のユーザーインターフェイステーマをサポートするようになりました。 また、開発者は、ユーザーが選択したテーマに基づいて UI 要素を手動で調整し、特定のテーマをオーバーライドできます。

<a name="About-the-New-User-Interface-Styles"></a>

## <a name="about-the-new-user-interface-styles"></a>新しいユーザーインターフェイスのスタイルについて

前述のように、tvOS 10 では、ユーザーの設定に基づいて、すべての組み込み UIKit コントロールが自動的にに適応する、ダークおよびライトの両方のユーザーインターフェイステーマがサポートされるようになりました。

このテーマを切り替えるには、[**設定**] [全般] [全般] に移動し、[明るい] と [ダーク] を切り替え  >  **General**  >  **Appearance**ます**Light** **Dark**。

[![設定アプリ](user-interface-styles-images/theme01.png)](user-interface-styles-images/theme01.png#lightbox)

**ダーク**テーマを選択すると、すべてのユーザーインターフェイス要素が暗い背景で明るいテキストに切り替わります。

[![ダークテーマ](user-interface-styles-images/theme02.png)](user-interface-styles-images/theme02.png#lightbox)

ユーザーは、いつでもテーマを切り替えることができます。また、現在のアクティビティ (Apple TV が配置されている場所または時刻) に基づいて、このオプションを選択することもできます。

明るい UI テーマは既定のテーマであり、既存の tvOS アプリでは、ユーザーの好みに関係なく明るいテーマが使用されます。ただし、tvOS 10 がダークテーマを利用するように変更されている場合は除きます。 TvOS 10 アプリには、現在のテーマをオーバーライドし、UI の一部またはすべてに対して常に淡色またはダークテーマを使用する機能もあります。

<a name="Adopting-the-Light-and-Dark-Themes"></a>

## <a name="adopting-the-light-and-dark-themes"></a>明るいテーマとダークテーマの採用

この機能をサポートするために、Apple はクラスに新しい API を追加 `UITraitCollection` し、tvOS アプリは、(ファイルの設定を使用して) ダークな外観をサポートするためにオプトインする必要があり `Info.plist` ます。

明るいテーマとダークテーマのサポートをオプトインするには、次の手順を実行します。

1. **ソリューション エクスプローラー**で `Info.plist` ファイルをダブルクリックして、編集用に開きます。
2. (エディターの下部から)**ソース**ビューを選択します。
3. 新しいキーを追加し、次のように呼び出し `UIUserInterfaceStyle` ます。

    [![UIUserInterfaceStyle キー](user-interface-styles-images/theme03.png)](user-interface-styles-images/theme03.png#lightbox)
4. 型をに設定したままに `String` し、次のように値を入力し `Automatic` ます。

    [![自動入力](user-interface-styles-images/theme04.png)](user-interface-styles-images/theme04.png#lightbox)
5. 変更をファイルに保存します。

このキーには、次の3つの値を指定でき `UIUserInterfaceStyle` ます。

- **Light** -tvOS アプリの UI が常にライトテーマを使用するように強制します。
- **ダーク**-tvOS アプリの UI が常にダークテーマを使用するように強制します。
- **自動**-[設定] のユーザーの設定に基づいて、ライトとダークのテーマを切り替えます。 これは推奨される設定です。

<a name="UIKit-Theme-Support"></a>

### <a name="uikit-theme-support"></a>UIKit テーマのサポート

TvOS アプリが標準の組み込みベースのコントロールを使用している場合は `UIView` 、開発者の関与なしで UI テーマに自動的に応答します。

さらに、 `UILabel` と `UITextView` は、選択した UI テーマに基づいて自動的に色を変更します。

- ライトテーマでは、テキストは黒になります。
- ダークテーマでは、テキストは白になります。

開発者がテキストの色を (ストーリーボードまたはコードで) 手動で変更した場合は、UI テーマに基づいて色の変更を処理する必要があります。

<a name="New-Blur-Effects"></a>

### <a name="new-blur-effects"></a>新しいぼかし効果

TvOS 10 アプリで明るいテーマとダークテーマをサポートするために、Apple は2つの新しいぼかし効果を追加しました。 これらの新しい効果は、次のようにユーザーが選択した UI テーマに基づいてぼかしを自動的に調整します。

- `UIBlurEffectStyleRegular`-明るいテーマでは薄いぼかし、ダークテーマではダークぼかしを使用します。
- `UIBlurEffectStyleProminent`-明るいテーマでは薄いぼかし、ダークテーマでは濃いぼかしを使用します。

<a name="Working-with-Trait-Collections"></a>

## <a name="working-with-trait-collections"></a>特徴コレクションの操作

クラスの新しいプロパティを使用して、 `UserInterfaceStyle` `UITraitCollection` 現在選択されている UI テーマを取得し、 `UIUserInterfaceStyle` 次のいずれかの値の列挙体にすることができます。

- **Light** -明るい UI テーマが選択されています。
- **ダーク**-暗い UI テーマが選択されています。
- **未指定**-ビューはまだ画面に表示されていないため、現在の UI テーマは不明です。

さらに、特徴コレクションには tvOS 10 の次の特徴があります。

- 表示プロキシは、指定されたのに基づいてカスタマイズし `UserInterfaceStyle` `UITraitCollection` て、テーマに基づいて画像や項目の色などを変更できます。
- TvOS アプリで `TraitCollectionDidChange` は、クラスまたはクラスのメソッドをオーバーライドすることによって、特徴のコレクションの変更を処理でき `UIView` `UIViewController` ます。

> [!IMPORTANT]
> TvOS 10 の tvOS 早期プレビューでは、まだを完全にサポートしていません `UIUserInterfaceStyle` `UITraitCollection` 。 完全なサポートは、今後のリリースで追加される予定です。

<a name="Customizing-Appearance-Based-on-Theme"></a>

### <a name="customizing-appearance-based-on-theme"></a>テーマに基づいて外観をカスタマイズする

外観プロキシをサポートするユーザーインターフェイス要素については、特徴コレクションの UI テーマに基づいて外観を調整できます。 そのため、特定の UI 要素について、開発者は、ライトテーマに対して1色、濃色テーマに別の色を指定できます。

```csharp
button.SetTitleColor (UIColor.Red, UIControlState.Normal);

// TODO - Pseudocode because this isn't currently supported in the preview bindings.
var light = new UITraitCollection(UIUserInterfaceStyle.Light);
var dark = new UITraitCollection(UIUserInterfaceStyle.Dark);

button.ForTraitCollection(light).SetTitleColor (UIColor.Red, UIControlState.Normal);
button.ForTraitCollection(dark).SetTitleColor (UIColor.White, UIControlState.Normal);
```

> [!IMPORTANT]
> 残念ながら、tvOS 10 の tvOS Preview ではが完全にサポートされていない `UIUserInterfaceStyle` `UITraitCollection` ため、この種類のカスタマイズはまだ使用できません。 完全なサポートは、今後のリリースで追加される予定です。

<a name="Responding-to-Theme-Changes-Directly"></a>

### <a name="responding-to-theme-changes-directly"></a>テーマの変更に直接応答する

開発者は、選択された UI テーマに基づいて UI 要素の外観をきめ細かく制御する必要があり `TraitCollectionDidChange` `UIView` ます。また、クラスまたはクラスのメソッドをオーバーライドでき `UIViewController` ます。

次に例を示します。

```csharp
public override void TraitCollectionDidChange (UITraitCollection previousTraitCollection)
{
    base.TraitCollectionDidChange (previousTraitCollection);

    // Take action based on the Light or Dark theme
    ...
}
```

<a name="Responding-to-Theme-Changes-Directly"></a>

### <a name="overriding-a-trait-collection"></a>特徴コレクションのオーバーライド

TvOS アプリの設計に基づいて、開発者が特定のユーザーインターフェイス要素の特徴コレクションをオーバーライドし、常に特定の UI テーマを使用する必要がある場合があります。

これは、クラスのメソッドを使用して行うことができ `SetOverrideTraitCollection` `UIViewController` ます。 次に例を示します。

```csharp
// Create new trait and configure it
var trait = new UITraitCollection ();
...

// Apply new trait collection
SetOverrideTraitCollection (trait, this);
```

詳細については、「統合された[ストーリーボード](~/ios/user-interface/storyboards/unified-storyboards.md)のドキュメントの概要」の[特徴](~/ios/user-interface/storyboards/unified-storyboards.md)と[オーバーライド](~/ios/user-interface/storyboards/unified-storyboards.md)に関するセクションを参照してください。

<a name="Trait-Collections-and-Storyboards"></a>

### <a name="trait-collections-and-storyboards"></a>特徴コレクションとストーリーボード

TvOS 10 では、特徴コレクションに応答するようにアプリのストーリーボードを設定できます。また、多くの UI 要素を淡色とダークテーマに対応させることができます。 TvOS 10 の現在の tvOS 早期プレビューでは、インターフェイスデザイナーでこの機能がまだサポートされていないため、回避策として、ストーリーボードを Xcode の Interface Builder で編集する必要があります。

特徴コレクションのサポートを有効にするには、次の手順を実行します。

1. **ソリューションエクスプローラー**でストーリーボードファイルを右クリックし、[ **Open With**  >  **Xcode Interface Builder**] を選択します。

    [![Xcode を使用して開く Interface Builder](user-interface-styles-images/theme05.png)](user-interface-styles-images/theme05.png#lightbox)
2. 特徴コレクションのサポートを有効にするには、**ファイルインスペクター**に切り替えて、 **Interface Builder ドキュメント**のセクションにある [**特徴のバリエーションを使用する**] プロパティをオンにします。

    [![特徴コレクションのサポートを有効にする](user-interface-styles-images/theme06.png)](user-interface-styles-images/theme06.png#lightbox)
3. 特性のバリエーションを使用するように変更を確認します。

    [![特徴バリエーションの使用のアラート](user-interface-styles-images/theme07.png)](user-interface-styles-images/theme07.png#lightbox)
4. 変更内容をストーリーボードファイルに保存します。

Interface Builder で tvOS Storyboard を編集するときに、Apple は次の機能を追加しました。

- 開発者は、**属性インスペクター**の UI テーマに基づいて、さまざまな種類のユーザーインターフェイス要素を指定できます。

  - いくつかのプロパティの **+** 横に、UI テーマ固有のバージョンを追加するためにクリックすることができるようになりました。

    [![UI テーマ固有のバージョンを追加する](user-interface-styles-images/theme08.png)](user-interface-styles-images/theme08.png#lightbox)

  - 開発者は、新しいプロパティを指定したり、[ **x** ] ボタンをクリックして削除したりできます。

    [![新しいプロパティを指定するか、[x] ボタンをクリックして削除します](user-interface-styles-images/theme09.png)](user-interface-styles-images/theme09.png#lightbox)
- 開発者は、Interface Builder 内の明るいテーマまたはダークテーマで UI デザインをプレビューできます。

  - デザインサーフェイスの下部を使用すると、開発者は現在の UI テーマを切り替えることができます。

    [![デザインサーフェイスの下部](user-interface-styles-images/theme10.png)](user-interface-styles-images/theme10.png#lightbox)

  - 新しいテーマが Interface Builder に表示され、特徴コレクション固有の調整がすべて表示されます。

    [![Interface Builder に表示されるテーマ](user-interface-styles-images/theme11.png)](user-interface-styles-images/theme11.png#lightbox)

さらに、tvOS シミュレーターには、開発者が tvOS アプリをデバッグするときに、明るいテーマと暗いテーマをすばやく切り替えることができるショートカットキーが追加されました。 ライトとダークを切り替えるには、**コマンドライン**のキーボードシーケンスを使用します。

<a name="Summary"></a>

## <a name="summary"></a>まとめ

この記事では、Apple が tvOS 10 に追加した軽量の UI テーマと、tvOS アプリでそれらを実装する方法について説明しました。

## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [TvOS 10 の新機能](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
