---
title: TvOS Xamarin で分割ビュー コント ローラーの操作
description: このドキュメントでは、Xamarin でビルドされたアプリでビューを分割 tvOS を操作する方法について説明します。 分割ビュー コント ローラーの概要を示しますが、マスター/詳細ビューにアクセスして表示され、マスター ビューを非表示はストーリー ボードでそれらを使用する方法。
ms.prod: xamarin
ms.assetid: 21248CFB-5A94-4C19-B223-C72E0DC5F1D5
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 9f1bd48378faa9ae6a4853083c93377268c38f01
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50122193"
---
# <a name="working-with-tvos-split-view-controllers-in-xamarin"></a>TvOS Xamarin で分割ビュー コント ローラーの操作

分割ビュー コント ローラーでは、表示し、マスターと詳細ビュー コント ローラー-サイド、同時に画面上を管理します。 分割ビュー コント ローラーがマスター ビュー (左側の小さなセクション) で永続的にフォーカスを設定できるコンテンツを表示するために使用し、関連する詳細ビュー (右側の大きなセクション) の詳細。

[![](split-views-images/intro01.png "分割ビューのサンプル")](split-views-images/intro01.png#lightbox)

<a name="About-Split-View-Controllers" />

## <a name="about-split-view-controllers"></a>分割ビュー コント ローラーについて

前述のように、マスターと詳細のビュー コント ローラー、左側、右側のうち、大きい方の詳細より小さいビューをされているマスターと並列で示されている分割ビュー コント ローラーを管理します。 

マスター ビュー コント ローラーは、の非表示または表示されているさらに、必要に応じて。 

[![](split-views-images/intro02.png "非表示にマスター ビュー コント ローラー")](split-views-images/intro02.png#lightbox)

分割ビュー コント ローラーは多くの場合、使用して、マスター ビューのカテゴリと詳細ビューで、フィルター処理された結果をフィルター適用不可能のコンテンツの一覧に表示します。 これは通常、左側のテーブル ビューとして表示されます、[コレクション ビュー](~/ios/tvos/user-interface/collection-views.md)右側にします。

分割ビュー コント ローラーを必要とするユーザー インターフェイスを設計するときは、マスターと詳細のビュー コント ローラー (だけ、コンテンツの変更、構造体ではありません) は変更されないを使用して Apple がお勧めします。 ビュー コント ローラーをスワップ アウトする場合は、ナビゲーション コント ローラーを (マスターまたは詳細) を変更する必要があるビュー コント ローラーのベースとして使用することをお勧めします。

Apple では、分割ビュー コント ローラーを操作するための次の推奨事項があります。

- **正確な分割の割合を使用して、** -既定では分割ビュー コント ローラーを使用してマスター ビュー コント ローラーの画面の 3 分の 1 と 3 分の 2 の詳細ビュー コント ローラー。 必要に応じて、50/50 の分割を使用することができます。 画面上の分散、コンテンツの表示にする正確な割合を選択します。
- **メインの選択を保持**- コンテンツの中に、詳細の表示は変更がマスター ビューで、ユーザーの選択に応じて、マスター ビューのコンテンツを固定する必要があります。 さらに、マスター ビューで現在選択されている項目を明確に示す必要があります。
- **1 つのタイトルの統合を使用して、** -詳細およびマスター ビューの両方のタイトルではなく、詳細ビューで、1 つの中央揃えのタイトルを使用したい通常、します。

<a name="Split-View-Controllers-and-Storyboards" />

## <a name="split-view-controllers-and-storyboards"></a>分割ビュー コント ローラーとストーリー ボード

Xamarin.tvOS アプリで分割ビュー コント ローラーを使用する最も簡単な方法では、iOS デザイナーを使用して、アプリの UI に追加します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. **Solution Pad**、ダブルクリックして、`Main.storyboard`ファイルし、編集用に開きます。
1. ドラッグ、**分割ビュー コント ローラー**から、**ツールボックス**し、ビューにドロップします。 

    [![](split-views-images/activity01.png "分割ビュー コント ローラー")](split-views-images/activity01.png#lightbox)
1. 既定では、iOS Designer はマスター ビューでも、ナビゲーション コント ローラーとビュー コント ローラーにインストールされます。 これには、アプリの要件が収まらない、単に削除してください。
1. 既定のマスター ビューを削除する場合は、デザイン サーフェイスに新しいビュー コント ローラーにドラッグします。 

    [![](split-views-images/activity02.png "ビュー コント ローラー")](split-views-images/activity02.png#lightbox)
1. コントロールをクリックし、分割ビュー コント ローラーから新しいマスター ビュー コント ローラーにドラッグします。 
1. 選択**マスター**から、**ポップアップ メニュー**: 

    [![](split-views-images/activity03.png "ポップアップ メニューからマスターを選択します。")](split-views-images/activity03.png#lightbox)
1. マスターと詳細ビューの内容をデザインするには。 

    [![](split-views-images/activity04.png "レイアウトの例")](split-views-images/activity04.png#lightbox)
1. 割り当てる**名**で、**ウィジェット タブ**の**Properties Pad**で UI コントロールを使用するC#コード。
1. 変更を保存し、Visual studio for mac を返す

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. **ソリューション エクスプ ローラー**、ダブルクリックして、`Main.storyboard`ファイルし、編集用に開きます。
1. ドラッグ、**分割ビュー コント ローラー**から、**ツールボックス**し、ビューにドロップします。 

    [![](split-views-images/activity01-vs.png "分割ビュー コント ローラー")](split-views-images/activity01-vs.png#lightbox)
1. 既定では、iOS Designer はマスター ビューでも、ナビゲーション コント ローラーとビュー コント ローラーに追加されます。 これには、アプリの要件が収まらない、単に削除してください。
1. 既定のマスター ビューを削除する場合は、デザイン サーフェイスに新しいビュー コント ローラーにドラッグします。 

    [![](split-views-images/activity02-vs.png "ビュー コント ローラー")](split-views-images/activity02-vs.png#lightbox)
1. コントロールをクリックし、分割ビュー コント ローラーから新しいマスター ビュー コント ローラーにドラッグします。 
1. 選択**マスター**から、**ポップアップ メニュー**: 

    [![](split-views-images/activity03-vs.png "ポップアップ メニューからマスターを選択します。")](split-views-images/activity03-vs.png#lightbox)
1. マスターと詳細ビューの内容をデザインするには。 

    [![](split-views-images/activity04.png "コンテンツのレイアウト")](split-views-images/activity04.png#lightbox)
1. 割り当てる**名**で、**ウィジェット タブ**の**プロパティ エクスプ ローラー**で UI コントロールを使用するC#コード。
1. 変更内容を保存します。
    
-----

ストーリー ボードの操作方法の詳細についてを参照してください、[はじめての tvOS クイック スタート ガイド](~/ios/tvos/get-started/hello-tvos.md)します。

<a name="Working-with-Split-View-Controllers" />

## <a name="working-with-split-view-controllers"></a>分割ビュー コント ローラーの操作

前述のように、分割ビュー コント ローラーがユーザーにフィルター選択されたコンテンツを表示している場合によく使用されます。 マスター ビューの左側の主なカテゴリが表示され、ユーザーの選択に基づいて、詳細ビューで、右側のフィルター処理された結果。

<a name="Accessing-Master-and-Detail" />

### <a name="accessing-master-and-detail"></a>マスター/詳細へのアクセス

マスターと詳細ビュー コント ローラーをプログラムでアクセスする必要がある場合、`ViewControllers `分割ビュー コント ローラーのプロパティ。 例えば:

```csharp
// Gain access to master and detail view controllers
var masterController = ViewControllers [0] as MasterViewController;
var detailController = ViewControllers [1] as DetailViewController;
```

ここでマスター ビュー コント ローラーには、(0) の最初の要素と 2 番目の要素 (1) は、詳細、配列として表示されます。

<a name="Accessing-Detail-from-Master" />

### <a name="accessing-detail-from-master"></a>マスターからへのアクセスの詳細

マスターでユーザーの選択に基づく詳細ビューの詳細な情報を表示している、通常は、ので、マスターから詳細にアクセスする手段が必要があります。

これを行う最も簡単な方法では、たとえば、マスター ビュー コント ローラー クラスのプロパティを公開します。

```csharp
public DetailViewController DetailController { get; set;}
```

分割ビュー コント ローラーでは、オーバーライド、 `ViewDidLoad` 2 つをまとめて表示メソッドと同順位です。 例えば:

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

### <a name="showing-and-hiding-master"></a>表示と非表示のマスター

必要に応じて、表示し、マスター ビュー コント ローラーを使用して、非表示にする、`PreferredDisplayMode`分割ビュー コント ローラーのプロパティ。 例えば:

```csharp
// Show hide split view
if (SplitViewController.DisplayMode == UISplitViewControllerDisplayMode.PrimaryHidden) {
    SplitViewController.PreferredDisplayMode = UISplitViewControllerDisplayMode.AllVisible;
} else {
    SplitViewController.PreferredDisplayMode = UISplitViewControllerDisplayMode.PrimaryHidden;
}
```

`UISplitViewControllerDisplayMode`列挙型として、次のいずれかのマスター ビュー コント ローラーの表示方法を定義します。

- **自動**-tvOS がマスターと詳細ビューの表示を制御します。
- **PrimaryHidden** -マスター ビュー コント ローラーを非表示にします。
- **AllVisible** -マスターと詳細ビュー コント ローラーによって並列の両方が表示されます。 これは、通常、既定のプレゼンテーションです。
- **PrimaryOverlay** -詳細ビュー コント ローラーは、下を拡張し、マスターでカバーされます。

現在のプレゼンテーションの状態を取得する、`DisplayMode`分割ビュー コント ローラーのプロパティ。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、設計と分割ビュー コント ローラー Xamarin.tvOS アプリ内での操作について説明しました。



## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマン インターフェイス ガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS 用のアプリのプログラミング ガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
