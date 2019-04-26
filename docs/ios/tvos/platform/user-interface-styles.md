---
title: tvOS Xamarin でのユーザー インターフェイスのスタイル
description: 光を取り上げており、10 および Xamarin.tvOS アプリでそれらを実装する方法が、濃い UI テーマを Apple を tvOS に追加します。
ms.prod: xamarin
ms.assetid: 8BC37683-AD9E-45CD-BE40-96965618AD1D
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 2536ca5d3bff3f5b7962bc4fcf58b31a130fd03c
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61270883"
---
# <a name="tvos-user-interface-styles-in-xamarin"></a>tvOS Xamarin でのユーザー インターフェイスのスタイル

_光を取り上げており、10 および Xamarin.tvOS アプリでそれらを実装する方法が、濃い UI テーマを Apple を tvOS に追加します。_

tvOS 10 に、サポートを自動的に制御するすべてのビルドの UIKit ダークおよびライト ユーザー インターフェイスの両方のテーマの適応ここでは、ユーザーの設定に基づいています。 さらに、開発者は、ユーザーが選択したテーマに基づいて UI 要素を手動で調整でき、指定されたテーマをオーバーライドできます。

<a name="About-the-New-User-Interface-Styles" />

## <a name="about-the-new-user-interface-styles"></a>新しいユーザー インターフェイスのスタイルについて

前述のように、ユーザーの設定に基づいています tvOS 10 に、サポートを自動的に制御するすべてのビルドの UIKit ダークおよびライト ユーザー インターフェイスの両方のテーマの適応ようになりました。

移動して、ユーザーがこのテーマを切り替えることができます**設定** > **全般** > **外観**との間の切り替え**ライト**と**濃い**:

[![](user-interface-styles-images/theme01.png "[設定] アプリ")](user-interface-styles-images/theme01.png#lightbox)

ときに、**濃い**テーマが選択されている場合、すべてのユーザー インターフェイス要素は、暗い背景に明るいテキストに切り替わります。

[![](user-interface-styles-images/theme02.png "ダーク テーマ")](user-interface-styles-images/theme02.png#lightbox)

ユーザーは、いつでも、テーマを切り替えることを備え、Apple TV が配置されている現在のアクティビティまたは 1 日の時刻に基づくために行うことがあります。

ライトの UI テーマとは、既定のテーマと tvOS ダーク テーマを活用するために 10 に変更されない限りは、既存のすべての tvOS アプリにユーザーの設定に関係なく、明るい色のテーマは使用もします。 TvOS 10 アプリでは、現在のテーマを変更し、常にその UI の一部またはすべてのライトまたは濃色のテーマを使用する機能もあります。

<a name="Adopting-the-Light-and-Dark-Themes" />

## <a name="adopting-the-light-and-dark-themes"></a>ライトと暗いテーマを採用します。

この機能をサポートするために Apple の新しい API が追加、`UITraitCollection`クラスおよび tvOS アプリをする必要がありますオプトイン暗い外観をサポートするために (設定を使用してその`Info.plist`ファイル)。

オプトインする光とダーク テーマのサポートを次の操作を行います。

1. **ソリューション エクスプローラー**で `Info.plist` ファイルをダブルクリックして、編集用に開きます。
2. 選択、**ソース**(エディターの下部にある) から表示します。
3. 新しいキーを追加し、それを呼び出す`UIUserInterfaceStyle`: 

    [![](user-interface-styles-images/theme03.png "UIUserInterfaceStyle キー")](user-interface-styles-images/theme03.png#lightbox)
4. 種類の設定のままに`String`の値を入力して`Automatic`: 

    [![](user-interface-styles-images/theme04.png "自動を入力します。")](user-interface-styles-images/theme04.png#lightbox)
5. 変更内容をファイルに保存します。

3 つの値がある、`UIUserInterfaceStyle`キー。

- **Light**の明るい色のテーマを常に使用する、tvOS アプリの UI を強制します。
- **濃い**-常にダーク テーマを使用する、tvOS アプリの UI を強制します。
- **自動**-設定で、ユーザーの設定に基づいて、光とダーク テーマを切り替えます。 これは、優先される設定です。

<a name="UIKit-Theme-Support" />

### <a name="uikit-theme-support"></a>UIKit テーマのサポート

標準的な組み込み tvOS アプリが使用されている場合`UIView`ベースのコントロール、UI テーマ、開発者の介入なしに自動的に応答されます。

さらに、`UILabel`と`UITextView`選択 UI テーマに基づいて、色を自動的に変更されます。

- テキストは、ライト テーマで黒になります。
- テキストは、ダーク テーマで白になります。

これまで、開発者は手動でテキストの色を変更し、(、ストーリー ボードまたはコードで) 場合、UI テーマに基づく色の変更が処理されます。

<a name="New-Blur-Effects" />

### <a name="new-blur-effects"></a>新しいぼかし効果

TvOS 10 アプリで、ライト テーマと暗いテーマをサポートするため Apple が 2 つの新しいぼかし効果を追加します。 これらの新しい効果では、ユーザーが次のように選択した UI テーマに基づいてぼかしが自動的に調整します。

- `UIBlurEffectStyleRegular` ダーク テーマでは、光はライト テーマと暗いぼかしがぼかし。
- `UIBlurEffectStyleProminent` -ライト テーマで extra-light のぼかしとダーク テーマでの濃い超のぼかしを使用します。

<a name="Working-with-Trait-Collections" />

## <a name="working-with-trait-collections"></a>特徴であるコレクションの操作

新しい`UserInterfaceStyle`のプロパティ、`UITraitCollection`クラスは、現在選択されている UI テーマを取得するために使用しては、`UIUserInterfaceStyle`列挙型の値は次のいずれか。

- **Light** -ライト UI テーマを選択します。
- **濃い**-濃い UI テーマを選択します。
- **指定されていない**-まだ、画面に、ビューが表示されているされませんので、現在の UI テーマが不明です。

さらに、特徴であるコレクションでは、tvOS 10 で、次の機能があります。
 
- に基づいて外観プロキシをカスタマイズすることができます、`UserInterfaceStyle`の指定された`UITraitCollection`テーマに基づく色の項目をイメージなどを変更します。
- TvOS アプリは、オーバーライドすることでの特徴であるコレクションの変更を処理できる、`TraitCollectionDidChange`のメソッド、`UIView`または`UIViewController`クラス。

> [!IMPORTANT]
> TvOS 10 の Xamarin.tvOS の早期プレビュー版を完全にサポートしない`UIUserInterfaceStyle`の`UITraitCollection`まだします。 完全なサポートは、将来のリリースで追加されます。




<a name="Customizing-Appearance-Based-on-Theme" />

### <a name="customizing-appearance-based-on-theme"></a>テーマに基づいて外観のカスタマイズ

外観のプロキシをサポートするユーザー インターフェイス要素、それらの外観を調整できますが特徴であるコレクションの UI テーマに基づいて。 そのため、UI 要素を開発者と指定できますライト テーマの色を濃色のテーマ別の色。

```csharp
button.SetTitleColor (UIColor.Red, UIControlState.Normal);

// TODO - Pseudocode because this isn't currently supported in the preview bindings.
var light = new UITraitCollection(UIUserInterfaceStyle.Light);
var dark = new UITraitCollection(UIUserInterfaceStyle.Dark);

button.ForTraitCollection(light).SetTitleColor (UIColor.Red, UIControlState.Normal);
button.ForTraitCollection(dark).SetTitleColor (UIColor.White, UIControlState.Normal);
```

> [!IMPORTANT]
> TvOS 10 の Xamarin.tvOS プレビューを完全にサポートしない残念ながら、`UIUserInterfaceStyle`の`UITraitCollection`ので、この種類のカスタマイズはまだ使用できません。 完全なサポートは、将来のリリースで追加されます。

<a name="Responding-to-Theme-Changes-Directly" />

### <a name="responding-to-theme-changes-directly"></a>テーマの変更に直接応答

開発者で、UI 要素の外観を細かく管理が選択されている UI テーマに基づく、これらをオーバーライドできますが必要です、`TraitCollectionDidChange`のメソッド、`UIView`または`UIViewController`クラス。

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

TvOS アプリの設計に基づきがあります、開発者が特定のユーザー インターフェイス要素の特徴であるコレクションをオーバーライドして、それを常に特定の UI テーマを使用する必要がある場合。

これを使用して、`SetOverrideTraitCollection`メソッドを`UIViewController`クラス。 例:

```csharp
// Create new trait and configure it
var trait = new UITraitCollection ();
...

// Apply new trait collection
SetOverrideTraitCollection (trait, this);
```

詳細についてを参照してください、 [Traits](~/ios/user-interface/storyboards/unified-storyboards.md)と[オーバーライド Traits](~/ios/user-interface/storyboards/unified-storyboards.md)のセクションでは、 [Unified ストーリー ボードの概要](~/ios/user-interface/storyboards/unified-storyboards.md)ドキュメント。

<a name="Trait-Collections-and-Storyboards" />

### <a name="trait-collections-and-storyboards"></a>特徴であるコレクションとストーリー ボード

TvOS 10、特徴であるコレクションに対応するアプリのストーリー ボードを設定することができ、多くの UI 要素にできるライトとダーク テーマに注意してください。 TvOS 10 の現在の Xamarin.tvOS 早期プレビュー機能が利用できないこのインターフェイス デザイナーで、ストーリー ボードは回避策として、Xcode の Interface Builder で編集する必要があります。

特徴であるコレクションのサポートを有効にするには、次の操作を行います。

1. ストーリー ボード ファイルを右クリックし、**ソリューション エクスプ ローラー**選択**プログラムから開く** > **Xcode の Interface Builder**: 

    [![](user-interface-styles-images/theme05.png "Xcode インターフェイス ビルダーで開く")](user-interface-styles-images/theme05.png#lightbox) 
2. 特徴であるコレクションのサポートを有効にするに切り替える、**ファイル インスペクター**を確認し、**特性のバリエーションを使用して**プロパティ、**インターフェイス ビルダー ドキュメント**セクション。 

    [![](user-interface-styles-images/theme06.png "特徴であるコレクションのサポートを有効にします。")](user-interface-styles-images/theme06.png#lightbox)
3. 特性のバリエーションを使用する変更を確認します。 

    [![](user-interface-styles-images/theme07.png "特性のバリエーションのアラートの使用")](user-interface-styles-images/theme07.png#lightbox)
4. ストーリー ボード ファイルに変更を保存します。

Apple は Interface Builder での tvOS ストーリー ボードを編集するときに次の機能を追加しました。

* 開発者は UI テーマに基づくユーザー インターフェイス要素のさまざまなバリエーションを指定できます、**属性インスペクター**:
    
    * いくつかのプロパティのようになりましたが、 **+** の横に UI テーマの特定のバージョンを追加するときにクリックします。 

        [![](user-interface-styles-images/theme08.png "UI テーマの特定のバージョンを追加します。")](user-interface-styles-images/theme08.png#lightbox) 
    
    * 開発者は、新しいプロパティを指定したり をクリックして、 **x**それを削除するボタンをクリックします。 

        [![](user-interface-styles-images/theme09.png "新しいプロパティを指定するか、それを削除する [x] ボタンをクリックします。")](user-interface-styles-images/theme09.png#lightbox)
* 開発者は、Interface Builder でライトまたはダーク テーマでの UI デザインをプレビューできます。
    
    * デザイン画面の下部にあるは、開発者は、現在の UI テーマの切り替えを使用できます。 

        [![](user-interface-styles-images/theme10.png "デザイン画面の下部にあります。")](user-interface-styles-images/theme10.png#lightbox)
        
    * インターフェイス ビルダーで新しいテーマが表示され、すべての特徴であるコレクション固有の調整が表示されます。 

        [![](user-interface-styles-images/theme11.png "インターフェイス ビルダーで表示されるテーマ")](user-interface-styles-images/theme11.png#lightbox)

さらに、tvOS シミュレーターは、キーボード ショートカットをすばやく切り替える、ライト テーマとダーク テーマ tvOS アプリをデバッグするときに開発者を許可するようになりましたが。 使用して、**コマンド-d Shift**キーボードのライトとダークを切り替えるシーケンス。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、ライトをカバーされており、10 および Xamarin.tvOS アプリでそれらを実装する方法が、濃い UI テーマを Apple を tvOS に追加。



## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://developer.xamarin.com/samples/tvos/all/)
- [TvOS 10 の新機能新機能](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
