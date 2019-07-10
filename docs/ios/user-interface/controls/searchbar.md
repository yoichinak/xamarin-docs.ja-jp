---
title: Xamarin.iOS での検索バー
description: このドキュメントでは、Xamarin.iOS での検索バーを使用する方法について説明します。 これには、プログラムでと、ストーリー ボードでは、検索バーを作成する方法について説明します。
ms.prod: xamarin
ms.assetid: 22A8249A-19C6-4734-8331-E49FE3170771
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 07/11/2017
ms.openlocfilehash: 75abb943dbc56d7b4213e0c36c19ff338182ae8a
ms.sourcegitcommit: 58d8bbc19ead3eb535fb8248710d93ba0892e05d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/09/2019
ms.locfileid: "67674867"
---
# <a name="search-bars-in-xamarinios"></a>Xamarin.iOS での検索バー

値の一覧を検索して、UISearchBar です。 

次の 3 つの主要なコンポーネントが含まれています。 

- テキストを入力するために使用するフィールドです。 ユーザーに、検索語句を入力します。 これを利用できます。
- クリア ボタン、検索フィールドから任意のテキストを削除します。
- [キャンセル] ボタン、検索機能を終了します。

![検索バー](searchbar-images/image1.png)

## <a name="implementing-the-search-bar"></a>検索バーを実装します。

新しくインスタンス化するには、検索バーの開始を実装します。

```csharp
searchBar = new UISearchBar();
```

それを配置します。 次の例では、ナビゲーション バーで、またはテーブルの HeaderView で配置する方法を示します。

```csharp
NavigationItem.TitleView = searchBar;

\\or

TableView.TableHeaderView = searchBar;
```

検索バーで設定のプロパティ:

```csharp
 searchBar = new UISearchBar(){
                Placeholder = "Enter your search Item",
                Prompt = "Search Entered here",
                ShowsScopeBar = true,
                ScopeButtonTitles = new string[]{ "Boston", "London", "SF" },
            };
```

![検索バーのプロパティ](searchbar-images/image6.png)

発生させる、`SearchButtonClicked`イベントの検索 ボタンが押されたときにします。 これは、検索ロジックを呼び出します。

```csharp
searchBar.SearchButtonClicked += (sender, e) => {
                Search ();
            };
```

検索バーと検索結果のプレゼンテーションを管理する方法の詳細についてを参照してください、[検索コント ローラー](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/search-controller)レシピです。

## <a name="using-the-search-bar-in-the-designer"></a>デザイナーで、検索バーの使用

デザイナーには、デザイナーでの検索バーを実装するための 2 つのオプション

- 検索バー
- (非推奨) 検索表示コント ローラーの検索バー

![デザイナーの検索バー コントロール](searchbar-images/image2.png)

プロパティ パネルを使用して、検索バーのプロパティを設定

![検索バーのプロパティのデザイナー](searchbar-images/image3.png)

これらのプロパティについて詳しく説明します。

- **プロンプトのテキスト、プレース ホルダー、** – の提案し、ユーザーが検索バーを使用する方法を指示をこれらのプロパティを使用します。 たとえば、アプリ ストアの一覧を表示する場合はでした、prompt プロパティを使用する「を入力したり、市区町村、ストーリーの名前、または郵便番号/zip Code」ことをお勧め
- **スタイルの検索**– であるか、検索バーを設定する**Prominent**または**最小限**します。 著名なを使用して、画面で、検索バー、検索バーに描画するフォーカスの原因を除く、他のすべてが付けるされます。 最小限のスタイルの検索バーは、周囲ブレンドします。
- **機能**– これらのプロパティを有効にするのみ、UI 要素が表示されます。 詳述するように適切なイベントを発生させることによってこれらの機能を実装する必要があります、[検索バーの API ドキュメント](xref:UIKit.UISearchBar)
    - 検索結果が表示されます、ブックマーク ボタン-検索バーで検索結果やブックマーク アイコンを表示します。
    - ユーザーが検索機能を終了する – [キャンセル] ボタンを示しています。 これが選択されていることをお勧めします。
    - これにより、検索のスコープを制限するユーザーをスコープ バー – に表示されます。 たとえば、music アプリで検索するとき、ユーザーは Apple Music またはそのライブラリの特定の曲またはアーティストを検索するかどうかを選択できます。 さまざまなオプションを表示するには、タイトルの配列を追加、 **ScopeBarTitles**プロパティ。
    ![検索バーのスコープのタイトル](searchbar-images/image4.png)

- **テキストの動作**– にこれらを入力するときに、ユーザー入力が書式設定方法に対処するこれらのオプションを使用します。 大文字と小文字は各単語や文の開始を設定またはすべての文字を大文字に変換として。 補正とでスペル チェックように入力する必要が単語のスペルの候補を持つユーザーを入力します。
- **キーボード**– コントロール キーボード スタイルが、入力に表示され、どのようなキーは、キーボードの使用可能なためです。 その他のオプションと共に数字パッド、パッドの電話、電子メール、URL が含まれます。
- **外観**– キーボードの外観のスタイルを制御およびがいずれかの濃いまたはライト テーマとしました。
- **キーを返す**– どのような操作が実行されるより的確に反映する戻り値のキーのラベルを変更します。 サポートされている値には、Go、結合、[次へ]、終了、ルート、検索が含まれます。
- **セキュリティで保護された**– 入力をマスクするかどうかを識別します (パスワードの入力など)。

## <a name="related-links"></a>関連リンク

- [コント ローラーの検索](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/search-controller)
