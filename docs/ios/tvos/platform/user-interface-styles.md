---
title: tvOS Xamarin でのユーザー インターフェイスのスタイル
description: 光を取り上げており、10 および Xamarin.tvOS アプリでそれらを実装する方法が、濃い UI のテーマを Apple を tvOS に追加します。
ms.prod: xamarin
ms.assetid: 8BC37683-AD9E-45CD-BE40-96965618AD1D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 43bfac29acb8b465fd1f3cdfd53c7664adeae18f
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789172"
---
# <a name="tvos-user-interface-styles-in-xamarin"></a>tvOS Xamarin でのユーザー インターフェイスのスタイル

_光を取り上げており、10 および Xamarin.tvOS アプリでそれらを実装する方法が、濃い UI のテーマを Apple を tvOS に追加します。_

tvOS 10 サポート Dark とライト ユーザー インターフェイスの両方のテーマを自動的にコントロールのすべてのビルドで UIKit を合わせて、これは、ユーザーの設定に基づいています。 さらに、開発者は、ユーザーが選択したテーマに基づく UI 要素を手動で調整でき、指定されたテーマをオーバーライドできます。

<a name="About-the-New-User-Interface-Styles" />

## <a name="about-the-new-user-interface-styles"></a>新しいユーザー インターフェイスのスタイルに関する

前述のように、tvOS 10 サポート Dark とライト ユーザー インターフェイスの両方のテーマを自動的にコントロールのすべてのビルドで UIKit を合わせて、これは、ユーザーの設定に基づいています。

移動して、ユーザーがこのテーマを切り替えることができます**設定** > **全般** > **外観**との間の切り替え**ライト**と**濃い**:

[![](user-interface-styles-images/theme01.png "設定アプリ")](user-interface-styles-images/theme01.png#lightbox)

ときに、**濃い**テーマを選択すると、すべてのユーザー インターフェイス要素は、暗い背景で明るいテキストに切り替えます。

[![](user-interface-styles-images/theme02.png "ダーク テーマ")](user-interface-styles-images/theme02.png#lightbox)

ユーザーがいつでも、テーマを切り替えるオプションし、Apple TV が配置されている現在のアクティビティと 1 日の時刻に基づくためなどに行います。

ライト UI のテーマとは、既定のテーマ、tvOS の既存のアプリはもを使用して、ユーザーの設定に関係なく、明るい色のテーマ tvOS ダーク テーマを活用するために 10 に変更されない限りです。 TvOS 10 アプリは、現在のテーマを変更し、常に、UI の一部またはすべての明るいまたは暗いテーマを使用する機能もあります。

<a name="Adopting-the-Light-and-Dark-Themes" />

## <a name="adopting-the-light-and-dark-themes"></a>明るいテーマとダーク テーマを採用します。

この機能をサポートするために Apple が追加された新しい API を`UITraitCollection`クラスおよび tvOS アプリ必要がありますオプトイン暗い外観をサポートするために (の設定を使用してその`Info.plist`ファイル)。

オプトインする信号と暗いテーマのサポートを次の操作を行います。

1. **ソリューション エクスプローラー**で `Info.plist` ファイルをダブルクリックして、編集用に開きます。
2. 選択、**ソース**(エディターの下部にある) から表示します。
3. 新しいキーを追加し、それを呼び出す`UIUserInterfaceStyle`: 

    [![](user-interface-styles-images/theme03.png "UIUserInterfaceStyle キー")](user-interface-styles-images/theme03.png#lightbox)
4. 種類に設定のままにして`String`の値を入力して`Automatic`: 

    [![](user-interface-styles-images/theme04.png "自動を入力します。")](user-interface-styles-images/theme04.png#lightbox)
5. 変更内容をファイルに保存します。

3 つの値がある、`UIUserInterfaceStyle`キー。

- **ライト**の明るい色のテーマを常に使用する、tvOS アプリの UI を強制します。
- **濃い**-ダーク テーマを常に使用する、tvOS アプリの UI を強制します。
- **自動**-設定で、ユーザーの設定に基づいて、光とダーク テーマを切り替えます。 これは、推奨される設定です。

<a name="UIKit-Theme-Support" />

### <a name="uikit-theme-support"></a>UIKit テーマのサポート

TvOS アプリは、標準的な組み込みを使用する場合`UIView`それらが自動的に応答する開発者の介入がなくても、UI のテーマをコントロールに基づいています。

さらに、`UILabel`と`UITextView`選択 UI のテーマに基づいて色を自動的に変更されます。

- テキストは明るい色のテーマには黒になります。
- ダーク テーマで白いテキストになります。

これまで、開発者は手動でテキストの色を変更し、(、ストーリー ボードまたはコードでも) 場合、UI のテーマに基づいて色の変更の処理を担当するされます。

<a name="New-Blur-Effects" />

### <a name="new-blur-effects"></a>新しいぼかし効果

TvOS 10 アプリでは、ライト テーマとダーク テーマをサポートするため Apple が 2 つの新しいぼかし効果を追加します。 これらの新しい効果は、ユーザーが次のように選択した UI のテーマに基づくぼかしを自動的に調整します。

- `UIBlurEffectStyleRegular` ダーク テーマでは、ライトが明るい色のテーマと暗いのぼかしをぼかすです。
- `UIBlurEffectStyleProminent` -ライト テーマで極めてライトのぼかしとダーク テーマで極めて暗いぼかしを使用します。

<a name="Working-with-Trait-Collections" />

## <a name="working-with-trait-collections"></a>特徴であるコレクションの操作

新しい`UserInterfaceStyle`のプロパティ、`UITraitCollection`クラスは、現在選択されている UI のテーマを取得するために使用しては、`UIUserInterfaceStyle`列挙型値は次のいずれかの。

- **ライト**-光の UI のテーマを選択します。
- **濃い**-濃い UI のテーマを選択します。
- **指定されていない**-まだ、画面に、ビューが表示されていないため、現在の UI のテーマは既知ではありません。

またの特徴であるコレクションでは、tvOS 10 で、次の機能があります。
 
- に基づいて外観プロキシをカスタマイズすることができます、`UserInterfaceStyle`の指定された`UITraitCollection`品目のテーマに基づいて色を画像などを変更します。
- TvOS アプリは、オーバーライドすることで、特徴であるコレクションの変更を処理できる、`TraitCollectionDidChange`のメソッド、`UIView`または`UIViewController`クラスです。

> [!IMPORTANT]
> TvOS 10 の Xamarin.tvOS の早期プレビュー版を完全にサポートしない`UIUserInterfaceStyle`の`UITraitCollection`まだです。 完全なサポートは、将来のリリースで追加されます。




<a name="Customizing-Appearance-Based-on-Theme" />

### <a name="customizing-appearance-based-on-theme"></a>テーマに基づく外観のカスタマイズ

外観のプロキシをサポートするユーザー インターフェイス要素、その外観を調整できます、特徴であるコレクションの UI のテーマに基づいて。 そのため、指定された UI 要素の開発者と指定できます明るい色のテーマの色を濃色のテーマ別の色。

```csharp
button.SetTitleColor (UIColor.Red, UIControlState.Normal);

// TODO - Pseudocode because this isn't currently supported in the preview bindings.
var light = new UITraitCollection(UIUserInterfaceStyle.Light);
var dark = new UITraitCollection(UIUserInterfaceStyle.Dark);

button.ForTraitCollection(light).SetTitleColor (UIColor.Red, UIControlState.Normal);
button.ForTraitCollection(dark).SetTitleColor (UIColor.White, UIControlState.Normal);
```

> [!IMPORTANT]
> 残念ながら、Xamarin.tvOS プレビュー tvOS 10 を完全にサポートしていない`UIUserInterfaceStyle`の`UITraitCollection`ので、この種類のカスタマイズはまだ使用できません。 完全なサポートは、将来のリリースで追加されます。

<a name="Responding-to-Theme-Changes-Directly" />

### <a name="responding-to-theme-changes-directly"></a>テーマの変更に直接応答

開発者が必要に、UI 要素の外観をより深いコントロールで選択された UI のテーマに基づく、オーバーライドすることができます、`TraitCollectionDidChange`のメソッド、`UIView`または`UIViewController`クラスです。

例えば:

```csharp
public override void TraitCollectionDidChange (UITraitCollection previousTraitCollection)
{
    base.TraitCollectionDidChange (previousTraitCollection);
    
    // Take action based on the Light or Dark theme
    ...
}
```

<a name="Responding-to-Theme-Changes-Directly" />

### <a name="overriding-a-trait-collection"></a>特徴であるコレクションをオーバーライドします。

TvOS アプリの設計に基づき、あります、開発者が指定されたユーザー インターフェイス要素の特徴であるコレクションをオーバーライドして、それを常に、特定の UI のテーマを使用する必要がある場合。

これを使用して、`SetOverrideTraitCollection`メソッドを`UIViewController`クラスです。 例えば:

```csharp
// Create new trait and configure it
var trait = new UITraitCollection ();
...

// Apply new trait collection
SetOverrideTraitCollection (trait, this);
```

詳細についてを参照してください、[特徴 (traits)](~/ios/user-interface/storyboards/unified-storyboards.md)と[特徴 (traits) をオーバーライドする](~/ios/user-interface/storyboards/unified-storyboards.md)のセクションでは、 [Unified ストーリー ボードの概要](~/ios/user-interface/storyboards/unified-storyboards.md)ドキュメント。

<a name="Trait-Collections-and-Storyboards" />

### <a name="trait-collections-and-storyboards"></a>特徴であるコレクションとストーリー ボード

TvOS 10 での特徴であるコレクションに応答する、アプリのストーリー ボードを設定することができ、多数の UI 要素にできる信号とダーク テーマに注意してください。 TvOS 10 の現在の Xamarin.tvOS 初期プレビュー機能が利用できないこのインターフェイス デザイナーでまだ、ストーリー ボードは回避策として Xcode のインターフェイスのビルダーで編集する必要があります。

特徴であるコレクションのサポートを有効にするには、次の操作を行います。

1. ストーリー ボード ファイルを右クリックし、**ソリューション エクスプ ローラー**選択**ファイルを開く** > **Xcode インターフェイス ビルダー**: 

    [![](user-interface-styles-images/theme05.png "Xcode インターフェイス ビルダーで開く")](user-interface-styles-images/theme05.png#lightbox) 
2. 切り替えるの特徴であるコレクションのサポートを有効にする、**ファイル インスペクター**を確認し、**使用の特徴であるバリエーション**プロパティに、**インターフェイス ビルダー ドキュメント**セクション。 

    [![](user-interface-styles-images/theme06.png "特徴であるコレクションのサポートを有効にします。")](user-interface-styles-images/theme06.png#lightbox)
3. 特徴であるバリエーションを使用する変更を確認します。 

    [![](user-interface-styles-images/theme07.png "特徴であるバリエーションのアラートを使用します。")](user-interface-styles-images/theme07.png#lightbox)
4. ストーリー ボード ファイルに変更を保存します。

インターフェイスのビルダーで、tvOS のストーリー ボードの編集時に、Apple によって次の機能が追加されます。

* 開発者がで UI のテーマに基づくユーザー インターフェイス要素のさまざまなバリエーションを指定する、**属性インスペクター**:
    
    * いくつかのプロパティのようになりましたが、 **+** の横に UI のテーマの特定のバージョンを追加するときにクリックします。 

        [![](user-interface-styles-images/theme08.png "UI のテーマの特定のバージョンを追加します。")](user-interface-styles-images/theme08.png#lightbox) 
    
    * 開発者は、新しいプロパティを指定したり をクリックして、 **x**を削除するボタンをクリックします。 

        [![](user-interface-styles-images/theme09.png "新しいプロパティを指定するか、削除する [x] をクリックしてください")](user-interface-styles-images/theme09.png#lightbox)
* 開発者は、インターフェイスのビルダー内から、明るいまたは暗いテーマの UI の設計をプレビューできます。
    
    * デザイン画面の下部には、現在の UI のテーマを切り替えるには、開発者が使用できます。 

        [![](user-interface-styles-images/theme10.png "デザイン画面の下部にあります。")](user-interface-styles-images/theme10.png#lightbox)
        
    * インターフェイスのビルダーに表示される新しいテーマとすべての特徴であるコレクションの特定の調整が表示されます。 

        [![](user-interface-styles-images/theme11.png "インターフェイスのビルダーに表示されるテーマ")](user-interface-styles-images/theme11.png#lightbox)

さらに、tvOS シミュレーターなりましたをすばやく切り替える、明るいテーマとダーク テーマ、tvOS アプリをデバッグするときに、開発者のためのキーボード ショートカット。 使用して、**コマンド-d shift キーを押し**キーボードの薄いおよび濃い色の間で切り替えるシーケンス。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事は、光をカバーされており、10 および Xamarin.tvOS アプリでそれらを実装する方法が、濃い UI のテーマを Apple を tvOS に追加します。



## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://developer.xamarin.com/samples/tvos/all/)
- [TvOS 10 の新機能](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
