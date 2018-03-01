---
title: "検索バー"
ms.topic: article
ms.prod: xamarin
ms.assetid: 22A8249A-19C6-4734-8331-E49FE3170771
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/11/2017
ms.openlocfilehash: 03f90a506d22752c9158650de3f3a109d07b4396
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="search-bar"></a>検索バー

UISearchBar を使用すると、値のリストを検索します。 

次の 3 つの主要なコンポーネントが含まれています。 

- テキストを入力するためのフィールドです。 ユーザーは、検索用語を追加するときに利用できます。
- クリア ボタン、検索 フィールドから任意のテキストを削除します。
- [キャンセル] ボタン、検索機能を終了します。

![検索バー](searchbar-images/image1.png)

## <a name="implementing-the-search-bar"></a>検索バーを実装します。

新しいものをインスタンス化して、検索バーの開始を実装します。

```csharp
searchBar = new UISearchBar();
```

それを配置します。 次の例では、ナビゲーション バーで、またはテーブルの HeaderView 内に配置する方法を示します。

```csharp
NavigationItem.TitleView = searchBar;

\\or

TableView.TableHeaderView = searchBar;
```

検索バーのプロパティを設定します。

```csharp
 searchBar = new UISearchBar(){
                Placeholder = "Enter your search Item",
                Prompt = "Search Entered here",
                ShowsScopeBar = true,
                ScopeButtonTitles = new string[]{ "Boston", "London", "SF" },
            };
```

![検索バーのプロパティ](searchbar-images/image6.png)

発生させる、`SearchButtonClicked`イベントは、検索ボタンが押されるとします。 これは、検索ロジックを呼び出します。

```csharp
searchBar.SearchButtonClicked += (sender, e) => {
                Search ();
            };
```

検索バー、検索結果のプレゼンテーションを管理する方法の詳細についてを参照してください、[検索コント ローラー](https://developer.xamarin.com/recipes/ios/content_controls/search-controller/)レシピします。

## <a name="using-the-search-bar-in-the-designer"></a>デザイナーでの検索バーの使用

デザイナーには、デザイナーで、検索バーを実装するための 2 つのオプションが用意されています

- 検索バー
- (非推奨) 検索ディスプレイのコント ローラーを使用して検索バー

![デザイナーでの検索バー コントロール](searchbar-images/image2.png)

プロパティ パネルを使用して、検索バーのプロパティを設定

![検索バー プロパティ デザイナー](searchbar-images/image3.png)

これらのプロパティについて詳しく説明します。

- **テキスト、プレース ホルダー、プロンプト**– これらのプロパティを使用して、提案することや、ユーザーが検索バーを使用する方法を指示します。 たとえば、アプリ ストアの一覧を表示する場合はでしたプロパティを使用するプロンプト「を入力したり、city、ストーリー名、または郵便番号」に通知するには
- **スタイルを検索**– 存在するか、検索バーを設定することができます**Prominent**または**最小限**です。 使用する目立つは画面で、検索バー、検索バーを描画するフォーカスの原因を除く他のすべてに付けます。 最小限のスタイルの検索バーは、周囲 blend はします。
- **機能**– これらのプロパティを有効にするのみ、UI 要素が表示されます。 詳しく説明されている適切なイベントを発生させることによってこれらの機能を実装する必要があります、[検索バーの API ドキュメント](https://developer.xamarin.com/api/type/UIKit.UISearchBar/)
    - 検索結果を示していますブックマーク ボタンをクリック – は、検索バーで検索結果やブックマーク アイコンを表示/。
    - ユーザーが検索機能を終了する-[キャンセル] ボタンを示します。 これが選択されていることをお勧めします。
    - これにより、検索のスコープを制限するユーザーをスコープ バー – に表示されます。 たとえば、音楽アプリで検索するときに、ユーザーは Apple の音楽またはそのライブラリの特定の曲またはアーティストを検索するかどうかを選択できます。 さまざまなオプションを表示するには、タイトルの配列を追加、 **ScopeBarTitles**プロパティです。
    ![バーのスコープのタイトルの検索](searchbar-images/image4.png)

- **テキストの動作**– 入力するときに、ユーザー入力が書式設定方法に対応するこれらのオプションを使用します。 大文字と小文字は各単語または文の開始を設定またはすべての文字を大文字に変換するとします。 修正およびでスペル チェックように入力した単語の候補とユーザーに求めます。
- **キーボード**– 入力には、コントロールのキーボード スタイル表示され、どのようなキーは、キーボードで使用できるためです。 その他のオプションと共に数字パッド、Phone パッド、電子メール、URL が含まれます。
- **外観**– キーボードの外観のスタイルを制御しは、暗いかされる淡色テーマです。
- **キーを返す**: どのようなアクションは実行をより反映するように戻り値のキーのラベルを変更します。 サポートされている値には、移動、結合、次へ、Route、完了、および検索が含まれます。
- **セキュリティで保護された**– 入力をマスクするかどうかを示します (パスワードの入力など)。

## <a name="related-links"></a>関連リンク

- [コント ローラーの検索](https://developer.xamarin.com/recipes/ios/content_controls/search-controller/)
