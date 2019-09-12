---
title: Hello, iOS マルチスクリーン – 詳細
description: このドキュメントでは、Phoneword アプリケーションの拡張についてさらに詳しく説明し、モデル ビュー コントローラー、iOS ナビゲーション、その他の iOS 開発概念について詳しく考察します。
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: c866e5f4-8154-4342-876e-efa0693d66f5
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 10/05/2018
ms.openlocfilehash: 72e421e088a582e4d2de1cf830a0978cca9f45c8
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70762640"
---
# <a name="hello-ios-multiscreen--deep-dive"></a>Hello, iOS マルチスクリーン – 詳細

クイックスタート チュートリアルでは、最初のマルチスクリーン Xamarin.iOS アプリケーションを構築し、実行しました。 次は、iOS のナビゲーションとアーキテクチャについて理解を深めましょう。

このガイドでは、iOS のアーキテクチャとナビゲーションにおける*モデル ビュー コントローラー (MVC)* パターンとその役割について紹介します。
次にナビゲーション コントローラーを、iOS で使い慣れたナビゲーション エクスペリエンスを提供するために利用する方法について掘り下げ説明します。

## <a name="model-view-controller-mvc"></a>モデル ビュー コントローラー (MVC)

「[Hello, iOS](~/ios/get-started/hello-ios/index.md)」チュートリアルでは、iOS アプリケーションには、ウィンドウに*コンテンツ ビュー階層*を読み込む処理を行う*ウィンドウ*が 1 つのみあることを学びました。 2 つ目の Phoneword チュートリアルでは、以下の図のように、アプリケーションに 2 つ目の画面を追加し、一部のデータ (電話番号の一覧) を 2 つの画面間で渡しました。

 [![](hello-ios-multiscreen-deepdive-images/08.png "この図は、2 つの画面間のデータの受け渡しを示しています")](hello-ios-multiscreen-deepdive-images/08.png#lightbox)

この例では、1 つ目の画面でデータが収集され、1 つ目のビュー コントローラーから 2 つ目に渡され、2 つ目の画面に表示されました。 この画面の分離、ビュー コントローラー、およびデータは、*モデル ビュー コントローラー (MVC)* パターンに従っています。 以降のセクションでは、MVC パターンの利点、コンポーネント、Phoneword アプリケーションで使用する方法について説明します。

### <a name="benefits-of-the-mvc-pattern"></a>MVC パターンの利点

モデル ビュー コントローラーは*設計パターン*です。つまり、コードの一般的な問題やユース ケースに再利用可能なソリューションです。 MVC は、*グラフィカル ユーザー インターフェイス (GUI)* が含まれるアプリケーションのアーキテクチャです。 アプリケーション内のオブジェクトに、*モデル* (データまたはアプリケーション ロジック)、*ビュー* (ユーザー インターフェイス)、*コントローラー* (分離コード) という 3 つの役割のいずれかを割り当てます。 次の図は、3 つの MVC パターンとユーザー間の関係を示しています。

 [![](hello-ios-multiscreen-deepdive-images/00.png "この図は、3 つの MVC パターンとユーザー間の関係を示しています。")](hello-ios-multiscreen-deepdive-images/00.png#lightbox)

MVC パターンを使用すると、GUI アプリケーションの各部分を論理的に分けることができ、コードとビューを再利用しやすくなります。 3 つの各役割について詳しく見てみましょう。

> [!NOTE]
> 大枠でとらえると、MVC パターンは ASP.NET ページや WPF アプリケーションの構造と似ています。 これらの例では、ビューは、実際には UI を説明する処理を担当するコンポーネントです。ASP.NET では ASPX (HTML) ページ、WPF アプリケーションでは XAML に対応します。 コントローラーは、ビューの管理を担当するコンポーネントです。ASP.NET または WPF ではコード分離に対応します。

### <a name="model"></a>モデル

通常、モデル オブジェクトは、ビューに表示または入力されるデータのアプリケーション固有の表現です。 多くの場合、モデルは大まかに定義されています。たとえば、**Phoneword_iOS** アプリでは、(文字列の一覧として表現される) 電話番号の一覧はモデルです。 クロスプラットフォーム アプリケーションを構築していた場合は、iOS アプリケーションと Android アプリケーション間で **PhonewordTranslator** コードを共有することもできます。 この共有コードはモデルと考えることもできます。

MVC は、モデルの*データの永続性*と*アクセス*をまったく認識していません。 つまり、MVC は、データの種類やデータの保存方法に配慮せず、データの*表現*方法のみに配慮しています。 たとえば、データを SQL データベースに格納する方法、何らかのクラウド ストレージ メカニズムに保存する方法、または単に `List<string>` を使用する方法を選択できます。 MVC の目的のために、データ表現自体のみがパターンに含まれます。

場合によっては、MVC のモデル部分は空の可能性があります。 たとえば、電話の翻訳機能のしくみ、機能を作成した理由、バグを報告する窓口について説明する何らかの静的ページをアプリに追加する場合があります。 このようなアプリ画面は、ビューとコントローラーを使用して作成されますが、実際のモデル データはありません。

> [!NOTE]
> 資料によっては、MVC パターンのモデル部分は、UI に表示されるデータだけでなく、アプリケーション バックエンド全体を指すことがあります。 このガイドでは、モデルの最新の解釈を使用しますが、違いは特に重要ではありません。

### <a name="view"></a>View

ビューは、ユーザー インターフェイスのレンダリングを担当するコンポーネントです。 MVC パターンを使用するほぼすべてのプラットフォームで、ユーザー インターフェイスは複数ビューの階層で構成されます。 MVC のビューは、階層の最上位に単一のビュー (ルート ビューと呼ばれます) があり、その下位に任意の数の子ビュー (サブビューと呼ばれます) があるビュー階層と考えることができます。 iOS では、画面のコンテンツ ビュー階層は MVC のビュー コンポーネントに対応します。

### <a name="controller"></a>コントローラー

コントローラー オブジェクトは、すべてのものをまとめるコンポーネントです。iOS では、`UIViewController` で表現されます。 コントローラーは、画面または一連のビューのバッキング コードと考えることができます。 コントローラーは、ユーザーからの要求をリッスンし、適切なビュー階層を返す処理を担当します。 また、ビューからの要求 (ボタンのクリック、テキストの入力など) をリッスンし、適切な処理、ビューの変更、ビューの再読み込みを実行します。 コントローラーは、アプリケーションに存在する任意のバッキング データ ストアからモデルを作成または取得する処理や、そのデータを使用してビューを作成する処理も担当します。

また、コントローラーは他のコントローラーを管理することもできます。 たとえば、別の画面を表示する必要がある場合や、コントローラーのスタックを管理して順序と切り替えを監視する必要がある場合に、あるコントローラーが別のコントローラーを読み込むことがあります。 次のセクションでは、他のコントローラーを管理する*ナビゲーション コントローラー*という特殊な種類の iOS ビュー コントローラーの例を紹介します。

## <a name="navigation-controller"></a>ナビゲーション コントローラー

Phoneword アプリケーションでは、複数の画面間のナビゲーションを管理するためにナビゲーション コントローラーを使用しました。 ナビゲーション コントローラーは、`UINavigationController` クラスで表現される特殊な `UIViewController` です。 ナビゲーション コントローラーでは、単一のコンテンツ ビュー階層を管理するのではなく、他のビュー コントローラーと独自の特殊なコンテンツ ビュー階層を、タイトル、戻るボタンなどのオプション機能を含むナビゲーション ツールバー形式で管理します。

ナビゲーション コントローラーは iOS アプリでは一般的であり、以下のスクリーンショットの**設定**アプリのような定番の iOS アプリケーションのナビゲーションを行います。

 [![](hello-ios-multiscreen-deepdive-images/01.png "ナビゲーション コントローラーは、ここで示す設定アプリのような iOS アプリケーションのナビゲーションを行います")](hello-ios-multiscreen-deepdive-images/01.png#lightbox)

ナビゲーション コントローラーには、主に次の 3 つの機能があります。

- **転送ナビゲーション用のフック機能**: ナビゲーション コントローラーは、コンテンツ ビュー階層が*ナビゲーション スタック*に*プッシュ*される階層ナビゲーションの象徴を使用しています。 ナビゲーション スタックは、積み重ねたトランプと考えることができます。次の図のように、一番上のカードのみが見えます。  

    [![](hello-ios-multiscreen-deepdive-images/02.png "この図は、ナビゲーションを積み重ねたカードとして示しています")](hello-ios-multiscreen-deepdive-images/02.png#lightbox)

- **オプションの戻るボタン機能**: 新しい項目をナビゲーション スタックにプッシュすると、タイトル バーに*戻るボタン*が自動的に表示されます。ユーザーはこのボタンを押して戻る操作を行うことができます。 戻るボタンを押すと、ナビゲーション スタックから現在のビュー コントローラーが*ポップ*され、前のコンテンツ ビュー階層がウィンドウに読み込まれます。  

    [![](hello-ios-multiscreen-deepdive-images/03.png "この図は、スタックからのカードの 'ポップ' を示しています")](hello-ios-multiscreen-deepdive-images/03.png#lightbox)

- **タイトル バー機能**: ナビゲーション コントローラーの上部は*タイトル バー*と呼ばれます。 次の図のように、ビュー コントローラーのタイトルを表示する処理を担当します。  

    [![](hello-ios-multiscreen-deepdive-images/04.png "タイトル バーは、ビュー コントローラーのタイトルを表示します")](hello-ios-multiscreen-deepdive-images/04.png#lightbox)

### <a name="root-view-controller"></a>ルート ビュー コントローラー

ナビゲーション コントローラーはコンテンツ ビュー階層を管理しないため、ナビゲーション コントローラー自体では何も表示しません。
その代わりに、ナビゲーション コントローラーは*ルート ビュー コントローラー*と組み合わされています。

 [![](hello-ios-multiscreen-deepdive-images/05.png "ナビゲーション コントローラーはルート ビュー コントローラーと組み合わされています")](hello-ios-multiscreen-deepdive-images/05.png#lightbox)

ルート ビュー コントローラーは、ナビゲーション コントローラーのスタックで最初のビュー コントローラーを表します。ルート ビュー コントローラーのコンテンツ ビュー階層は、ウィンドウに最初に読み込まれるコンテンツ ビュー階層です。 ナビゲーション コントローラーのスタックにアプリケーション全体を配置するには、Phoneword アプリの場合と同様に、ソースレス セグエをナビゲーション コントローラーに移動し、最初の画面のビュー コントローラーをルート ビュー コントローラーとして設定します。

 [![](hello-ios-multiscreen-deepdive-images/06.png "ソースレス セグエは、最初の画面のビュー コントローラーをルート ビュー コントローラーとして設定します")](hello-ios-multiscreen-deepdive-images/06.png#lightbox)

### <a name="additional-navigation-options"></a>その他のナビゲーション オプション

ナビゲーション コントローラーは、iOS でナビゲーションを処理する一般的な方法ですが、唯一のオプションではありません。 たとえば、[タブ バー コントローラー](~/ios/user-interface/controls/creating-tabbed-applications.md)では、アプリケーションをさまざまな機能領域に分割でき、[分割ビュー コントローラー](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/split_view/use_split_view_to_show_two_controllers)では、マスター/詳細ビューを作成できます。 これらの他のナビゲーション パラダイムをナビゲーション コントローラーと組み合わせると、iOS で多数の柔軟な方法でコンテンツを表示したり移動したりできます。

## <a name="handling-transitions"></a>切り替えの処理

Phoneword チュートリアルでは、2 つの異なる方法で 2 つのコントローラー間の切り替えを処理しました。1 つ目はストーリーボードのセグエを使用する方法で、2 つ目はプログラムを使用する方法です。 これら 2 つのオプションについて、詳しく見てみましょう。

### <a name="prepareforsegue"></a>PrepareForSegue

**表示**アクションを使用してセグエをストーリーボードに追加するときに、2 つ目のビュー コントローラーをナビゲーション コントローラーのスタックにプッシュするように iOS に指示します。

 [![](hello-ios-multiscreen-deepdive-images/09.png "ドロップダウン リストからのセグエの種類の設定")](hello-ios-multiscreen-deepdive-images/09.png#lightbox)

セグエをストーリーボードに追加するだけで、画面間の簡単な切り替えを作成できます。 ビュー コントローラー間でデータを渡すには、`PrepareForSegue` メソッドをオーバーライドし、データ自体を処理する必要があります。

```csharp
public override void PrepareForSegue (UIStoryboardSegue segue, NSObject sender)
{
    base.PrepareForSegue (segue, sender);
    ...
}
```

iOS は、切り替えが発生する直前に `PrepareForSegue` を呼び出し、ストーリーボードで作成したセグエをメソッドに渡します。
この時点で、セグエの対象ビュー コントローラーを手動で設定する必要があります。 次のコードでは、対象ビュー コントローラーへのハンドルを取得し、適切なクラス (ここでは CallHistoryController) にキャストします。

```csharp
CallHistoryController callHistoryController = segue.DestinationViewController as CallHistoryController;
```

最後に、`CallHistoryController` の `PhoneHistory` プロパティを詳細な電話番号の一覧に設定することで、`ViewController` の電話番号の一覧 (モデル) を `CallHistoryController` に渡します。

```csharp
callHistoryController.PhoneNumbers = PhoneNumbers;
```

セグエを使用してデータを渡す完全なコードは次のとおりです。

```csharp
public override void PrepareForSegue (UIStoryboardSegue segue, NSObject sender)
{
    base.PrepareForSegue (segue, sender);

    var callHistoryController = segue.DestinationViewController as CallHistoryController;

    if (callHistoryController != null) {
         callHistoryController.PhoneNumbers = PhoneNumbers;
    }
 }
```

### <a name="navigation-without-segues"></a>セグエなしのナビゲーション

コードで 1 つ目のビュー コントローラーから 2 つ目に遷移する処理は、セグエと同じですが、一部の手順は手動で実行する必要があります。
まず、`this.NavigationController` を使用して現在のスタックであるナビゲーション コントローラーの参照を取得します。 次に、ナビゲーション コントローラーの `PushViewController` メソッドを使用して、次のビュー コントローラーを手動でスタックにプッシュし、ビュー コントローラーとオプションに渡して、切り替えをアニメーション化します (`true` に設定します)。

次のコードは、Phoneword 画面から通話履歴画面への切り替えを処理します。

```csharp
this.NavigationController.PushViewController (callHistory, true);
```

次のビュー コントローラーに切り替えする前に、`this.Storyboard.InstantiateViewController` を呼び出し、`CallHistoryController` のストーリーボード ID を渡すことで、手動でインスタンス化する必要があります。

```csharp
CallHistoryController callHistory =
this.Storyboard.InstantiateViewController
("CallHistoryController") as CallHistoryController;
```

最後に、`CallHistoryController` の `PhoneHistory` プロパティを詳細な電話番号の一覧に設定することで、`ViewController` の電話番号の一覧 (モデル) を `CallHistoryController` に渡します。これはセグエを使用して切り替えを処理した場合と同様の方法です。

```csharp
callHistory.PhoneNumbers = PhoneNumbers;
```

プログラムによる切り替えの完全なコードは次のとおりです。

```csharp
CallHistoryButton.TouchUpInside += (object sender, EventArgs e) => {
    // Launches a new instance of CallHistoryController
    CallHistoryController callHistory = this.Storyboard.InstantiateViewController ("CallHistoryController") as CallHistoryController;
    if (callHistory != null) {
     callHistory.PhoneNumbers = PhoneNumbers;
     this.NavigationController.PushViewController (callHistory, true);
    }
};
```

## <a name="additional-concepts-introduced-in-phoneword"></a>Phoneword で導入されているその他の概念

Phoneword アプリケーションでは、このガイドでは説明していない概念がいくつか導入されています。 たとえば、次のような概念です。

- **ビュー コントローラーの自動作成**: **Properties Pad** でビュー コントローラーのクラス名を入力すると、iOS デザイナーは、クラスが存在するかどうかを確認し、ビュー コントローラー バッキング クラスを自動的に生成します。 この機能と他の iOS デザイナー機能の詳細については、「[Introduction to the iOS Designer](~/ios/user-interface/designer/introduction.md)」(iOS デザイナーの概要) ガイドを参照してください。
- **テーブル ビュー コントローラー**: `CallHistoryController` はテーブル ビュー コントローラーです。 テーブル ビュー コントローラーには、iOS でよく使われるレイアウトおよびデータ表示ツールであるテーブル ビューが含まれています。 テーブルはこのガイドで取り扱う範囲を超えているので触れません。 テーブル ビュー コントローラーの詳細については、「[Working with Tables and Cells](~/ios/user-interface/controls/tables/index.md)」(テーブルとセルの操作) ガイドを参照してください。
- **ストーリーボード ID**: ストーリーボード ID を設定すると、ストーリーボードにビュー コントローラーの分離コードを含む Objective-C のビュー コントローラー クラスが作成されます。 ここでは、ストーリーボード ID を使用して Objective-C クラスを検出し、ストーリーボードでビュー コントローラーをインスタンス化します。 ストーリーボード ID の詳細については、「[Introduction to Storyboards](~/ios/user-interface/storyboards/index.md)」(ストーリーボードの概要) ガイドを参照してください。

## <a name="summary"></a>まとめ

これで最初のマルチスクリーン iOS アプリケーションが完成しました。

このガイドでは、MVC パターンについて紹介し、マルチスクリーン アプリケーションの作成に利用しました。 また、iOS ナビゲーションを強化するために利用できるナビゲーション コントローラーとその役割についても説明しました。 独自の Xamarin.iOS アプリケーションを開発するために必要な、強固な基盤ができました。

次は、「[Introduction to Mobile Development](~/cross-platform/get-started/introduction-to-mobile-development.md)」(モバイル開発の概要) ガイドと「[Building Cross-Platform Applications](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)」(クロスプラットフォーム アプリケーションの構築) ガイドを参照して、Xamarin を使用してクロスプラットフォーム アプリケーションを構築する方法を学びましょう。

## <a name="related-links"></a>関連リンク

- [Hello, iOS (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/hello-ios)
- [iOS ヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/Introduction/Introduction.html)
- [iOS プロビジョニング ポータル](https://developer.apple.com/ios/manage/overview/index.action)
