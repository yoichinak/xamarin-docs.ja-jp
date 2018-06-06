---
title: TvOS 分割 Xamarin でコント ローラーの表示の操作
description: このドキュメントでは、Xamarin でビルドされたアプリ内でビューを分割 tvOS を操作する方法について説明します。 分割ビュー コント ローラーの概要を示します、ストーリー ボードなど、マスター/詳細のビューへのアクセスし表示とマスター ビューを非表示でそれらを使用する方法です。
ms.prod: xamarin
ms.assetid: 21248CFB-5A94-4C19-B223-C72E0DC5F1D5
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 2dd07cd8a4e92d6d39be50ba670441d965ed4d13
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789432"
---
# <a name="working-with-tvos-split-view-controllers-in-xamarin"></a>TvOS 分割 Xamarin でコント ローラーの表示の操作

分割ビュー コント ローラーは、表示し、マスターと詳細ビューのコント ローラーによってサイドで、同時に画面上を管理します。 分割ビューのコント ローラーはマスター ビュー (左側の小さなセクション) に永続的にフォーカス可能なコンテンツを表示するために、関連する詳細ビュー (右側の大規模なセクション) の詳細。

[![](split-views-images/intro01.png "分割ビューのサンプル")](split-views-images/intro01.png#lightbox)

<a name="About-Split-View-Controllers" />

## <a name="about-split-view-controllers"></a>コント ローラーの表示の分割について

前述のように、マスター/詳細ビュー コント ローラーが左側、右側のうち、大きい方の詳細の小さなビュー マスターとサイド バイ サイドに提示する分割ビュー コント ローラーを管理します。 

マスター ビューのコント ローラーはの非表示または表示されているさらに、必要に応じて。 

[![](split-views-images/intro02.png "非表示にマスター ビュー コント ローラー")](split-views-images/intro02.png#lightbox)

分割ビュー コント ローラーは、マスター ビューのカテゴリと詳細ビューで、フィルター選択された結果を含む、フィルターを適用できるコンテンツの一覧を提供するよく使用します。 これが通常、左側のテーブル ビューとして表示されます、[コレクション ビュー](~/ios/tvos/user-interface/collection-views.md)右側にします。

分割ビュー コント ローラーを要求するユーザー インターフェイスを設計する際は、マスターおよび (だけ内容が変更された、構造体ではない) を変更しないで詳細ビューのコント ローラーを使用して Apple がお勧めします。 コント ローラーの表示をスワップ アウトする場合は、マスター (詳細) を変更する必要があるビュー コント ローラーのベースとしてナビゲーション コント ローラーを使用することをお勧めします。

Apple では、コント ローラーの表示の分割を操作するための以下の推奨事項があります。

- **正しい分割の割合を使用して**-既定では分割ビュー コント ローラーは、マスター ビューのコント ローラーの画面の 3 分の 1 と 3 分の 2 の詳細ビュー コント ローラー。 必要に応じて、50/50 の分割を使用することができます。 画面に分散されたコンテンツ、表示する正確な割合を選択します。
- **メインの選択を永続化**- コンテンツの中に、詳細ビューは変更は、マスター ビューで、ユーザーの選択への応答は、マスター ビューのコンテンツを修正してください。 さらに、マスター ビューで現在選択されている項目を明確に示す必要があります。
- **1 つのタイトルの統合を使用して**-通常、詳細データとマスター ビューの両方のタイトルではなく、詳細ビューで、1 つの中央のタイトルを使用してにしておきます。

<a name="Split-View-Controllers-and-Storyboards" />

## <a name="split-view-controllers-and-storyboards"></a>コント ローラーの表示とストーリー ボードを分割します。

Xamarin.tvOS アプリでは、コント ローラーの表示の分割を使用する最も簡単な方法では、iOS デザイナーを使用して、アプリの UI に追加します。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **ソリューション パッド**をダブルクリックして、`Main.storyboard`ファイルし、ファイルを開いて編集します。
1. ドラッグ、**コント ローラーの表示の分割**から、**ツールボックス**し、ビュー上にドロップします。 

    [![](split-views-images/activity01.png "分割ビュー コント ローラー")](split-views-images/activity01.png#lightbox)
1. 既定では、iOS デザイナーは、マスター ビューでナビゲーション コント ローラーとビューのコント ローラーがインストールされます。 これは、アプリの要件には一致しない、単に削除してください。
1. 既定のマスター ビューを削除すると、デザイン サーフェイスに新しいビュー コント ローラーをドラッグします。 

    [![](split-views-images/activity02.png "ビューのコント ローラー")](split-views-images/activity02.png#lightbox)
1. コントロールをクリックし、分割ビュー コント ローラーから、新しいマスター ビュー コント ローラーにドラッグします。 
1. 選択**マスター**から、**ポップアップ メニュー**: 

    [![](split-views-images/activity03.png "ポップアップ メニューからマスターを選択します。")](split-views-images/activity03.png#lightbox)
1. マスターと詳細ビューの内容をデザインします。 

    [![](split-views-images/activity04.png "レイアウトの例")](split-views-images/activity04.png#lightbox)
1. 割り当てる**名**で、**ウィジェット タブ**の**プロパティ パッド**c# コードで UI コントロールを使用します。
1. 変更を保存し、for mac を Visual Studio に戻る

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. **ソリューション エクスプ ローラー**をダブルクリックして、`Main.storyboard`ファイルし、ファイルを開いて編集します。
1. ドラッグ、**コント ローラーの表示の分割**から、**ツールボックス**し、ビュー上にドロップします。 

    [![](split-views-images/activity01-vs.png "分割ビュー コント ローラー")](split-views-images/activity01-vs.png#lightbox)
1. 既定では、iOS デザイナーは、マスター ビューでナビゲーション コント ローラーとビューのコント ローラーが追加されます。 これは、アプリの要件には一致しない、単に削除してください。
1. 既定のマスター ビューを削除すると、デザイン サーフェイスに新しいビュー コント ローラーをドラッグします。 

    [![](split-views-images/activity02-vs.png "ビューのコント ローラー")](split-views-images/activity02-vs.png#lightbox)
1. コントロールをクリックし、分割ビュー コント ローラーから、新しいマスター ビュー コント ローラーにドラッグします。 
1. 選択**マスター**から、**ポップアップ メニュー**: 

    [![](split-views-images/activity03-vs.png "ポップアップ メニューからマスターを選択します。")](split-views-images/activity03-vs.png#lightbox)
1. マスターと詳細ビューの内容をデザインします。 

    [![](split-views-images/activity04.png "コンテンツのレイアウト")](split-views-images/activity04.png#lightbox)
1. 割り当てる**名**で、**ウィジェット タブ**の**プロパティ エクスプ ローラー** c# コードで UI コントロールを使用します。
1. 変更内容を保存します。
    
-----

ストーリー ボードの使用の詳細についてを参照してください、[こんにちは、tvOS クイック スタート ガイド](~/ios/tvos/get-started/hello-tvos.md)です。

<a name="Working-with-Split-View-Controllers" />

## <a name="working-with-split-view-controllers"></a>分割ビュー コント ローラーの使用

前述のように、分割ビュー コント ローラーがユーザーにフィルター選択されたコンテンツを表示する場合によく使用されます。 マスター ビューの左側の主なカテゴリが表示され、ユーザーの選択に基づいて、詳細ビューで、右側のフィルター選択された結果。

<a name="Accessing-Master-and-Detail" />

### <a name="accessing-master-and-detail"></a>マスター/詳細へのアクセス

Master およびコント ローラーの詳細の表示をプログラムでアクセスする必要がある場合、`ViewControllers `分割ビュー コント ローラーのプロパティです。 例えば:

```csharp
// Gain access to master and detail view controllers
var masterController = ViewControllers [0] as MasterViewController;
var detailController = ViewControllers [1] as DetailViewController;
```

ここで、マスター ビュー コント ローラーには、(0) の最初の要素と 2 番目の要素 (1) は、詳細を配列として表示されます。

<a name="Accessing-Detail-from-Master" />

### <a name="accessing-detail-from-master"></a>マスターから詳細へのアクセス

詳細ビューで、マスターでユーザーの選択内容に基づいて、詳細な情報を表示している通常、ためマスタから詳細情報にアクセスする手段が必要があります。

これを行う最も簡単な方法は、たとえば、マスター ビューのコント ローラー クラスのプロパティを公開します。

```csharp
public DetailViewController DetailController { get; set;}
```

分割ビュー、コント ローラーでは、オーバーライド、 `ViewDidLoad` 2 つを一緒に表示メソッドおよび同順位です。 例えば:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Gain access to master and detail view controllers
    var masterController = ViewControllers [0] as MasterViewController;
    var detailController = ViewControllers [1] as DetailViewController;

    // Wire-up views
    masterController.SplitViewController = this;
    masterController.DetailController = detailController;
    detailController.SplitViewController = this;
}
```

必要に応じて新しいデータを表示するマスターが使用できる詳細ビュー コント ローラーのプロパティとメソッドを公開できます。

<a name="Showing-and-Hiding-Master" />

### <a name="showing-and-hiding-master"></a>表示と非表示にするマスター

必要に応じて、表示し、マスター ビューのコント ローラーを使用して、非表示にする、`PreferredDisplayMode`分割ビュー コント ローラーのプロパティです。 例えば:

```csharp
// Show hide split view
if (SplitViewController.DisplayMode == UISplitViewControllerDisplayMode.PrimaryHidden) {
    SplitViewController.PreferredDisplayMode = UISplitViewControllerDisplayMode.AllVisible;
} else {
    SplitViewController.PreferredDisplayMode = UISplitViewControllerDisplayMode.PrimaryHidden;
}
```

`UISplitViewControllerDisplayMode`列挙型では、次の 1 つとして、マスター ビュー コント ローラーの表示方法を定義します。

- **自動**-tvOS がマスターと詳細ビューの表示を制御します。
- **PrimaryHidden** -このマスター ビュー コント ローラーを非表示にします。
- **AllVisible** -マスターと、コント ローラーの詳細表示サイド バイ サイドの両方が表示されます。 これは、通常、既定の表示方法です。
- **PrimaryOverlay** -詳細ビュー コント ローラーで、拡張し、マスターでカバーされます。

現在のプレゼンテーションの状態を取得する、`DisplayMode`分割ビュー コント ローラーのプロパティです。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事は、設計と Xamarin.tvOS アプリ内でコント ローラーの表示の分割操作について説明しました。



## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマン インターフェイス ガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS のアプリケーション プログラミング ガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
