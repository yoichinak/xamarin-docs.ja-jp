---
title: Xamarin. iOS の検索バー
description: このドキュメントでは、Xamarin の検索バーの使用方法について説明します。 検索バーをプログラムおよびストーリーボードで作成する方法について説明します。
ms.prod: xamarin
ms.assetid: 22A8249A-19C6-4734-8331-E49FE3170771
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 07/11/2017
ms.openlocfilehash: 36e339139a0a7f853a770fdb188b5f03ee93f7ee
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70283356"
---
# <a name="search-bars-in-xamarinios"></a>Xamarin. iOS の検索バー

UISearchBar は、値のリストを検索するために使用されます。

次の3つの主要なコンポーネントが含まれています。

- テキストを入力するために使用するフィールド。 ユーザーはこれを利用して、検索用語を入力できます。
- 検索フィールドからテキストを削除するためのクリアボタン。
- 検索機能を終了するための [キャンセル] ボタン。

![検索バー](searchbar-images/image1.png)

## <a name="implementing-the-search-bar"></a>検索バーの実装

検索バーを実装するには、最初に新しいものをインスタンス化します。

```csharp
searchBar = new UISearchBar();
```

次に、それを配置します。 次の例では、これをナビゲーションバーまたはテーブルの HeaderView に配置する方法を示しています。

```csharp
NavigationItem.TitleView = searchBar;

// or

TableView.TableHeaderView = searchBar;
```

検索バーでのプロパティの設定:

```csharp
 searchBar = new UISearchBar(){
                Placeholder = "Enter your search Item",
                Prompt = "Search Entered here",
                ShowsScopeBar = true,
                ScopeButtonTitles = new string[]{ "Boston", "London", "SF" },
            };
```

![検索バーのプロパティ](searchbar-images/image6.png)

[検索`SearchButtonClicked` ] ボタンが押されたときにイベントを発生させます。 これにより、検索ロジックが呼び出されます。

```csharp
searchBar.SearchButtonClicked += (sender, e) => {
                Search ();
            };
```

検索バーと検索結果の表示を管理する方法の詳細については、「 [Search Controller](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/search-controller) 」を参照してください。

## <a name="using-the-search-bar-in-the-designer"></a>デザイナーでの検索バーの使用

デザイナーには、デザイナーに検索バーを実装するための2つのオプションが用意されています。

- 検索バー
- 検索表示コントローラーのある検索バー (非推奨)

![検索バーコントロール (デザイナーの)](searchbar-images/image2.png)

[プロパティ] パネルを使用して、検索バーにプロパティを設定する

![検索バーのプロパティデザイナー](searchbar-images/image3.png)

これらのプロパティについて以下に説明します。

- **テキスト、プレースホルダー、プロンプト**–これらのプロパティは、ユーザーが検索バーを使用する方法を提示および指示するために使用されます。 たとえば、アプリにストアの一覧が表示されている場合は、"プロンプト" プロパティを使用して、ユーザーが "市区町村、ストーリー名、郵便番号" を入力できるようにすることができます。
- **検索スタイル**–検索バーを**目立つ**ように設定することも、**最小化**することもできます。 目立つを使用すると、検索バーを除く、画面上の他のすべてのものが着色され、検索バーにフォーカスが描画されます。 最小スタイルの検索バーは、その周囲とブレンドされます。
- **機能**: これらのプロパティを有効にすると、UI 要素のみが表示されます。 これらの機能を実装するには、[検索バー API ドキュメント](xref:UIKit.UISearchBar)で詳しく説明されているように、適切なイベントを発生させる必要があります。
  - [検索結果/ブックマーク] ボタンを表示する–検索バーに検索結果またはブックマークアイコンを表示します。
  - [キャンセル] ボタンを表示します。ユーザーは検索機能を終了できます。 これを選択することをお勧めします。
  - [スコープバー] を表示します。ユーザーは検索範囲を制限できます。 たとえば、music アプリ内で検索する場合、ユーザーは特定の楽曲やアーティストの Apple Music を検索するかライブラリを検索するかを選択できます。 さまざまなオプションを表示するには、 **Scopebartitles**プロパティにタイトルの配列を追加します。
  ![検索バーのスコープのタイトル](searchbar-images/image4.png)

- **テキストの動作**–これらのオプションは、入力時にユーザー入力がどのように書式設定されるかを指定するために使用されます。 大文字と小文字を区別すると、各単語または文の先頭、またはすべての文字が大文字に設定されます。 修正とスペルチェックでは、ユーザーに対して、入力された単語のスペルを確認します。
- **キーボード**–入力に表示されるキーボードのスタイルを制御します。したがって、キーボードで使用できるキーを制御します。 これには、テンキー、電話パッド、電子メール、URL、およびその他のオプションが含まれます。
- **[表示]** –キーボードの外観スタイルを制御します。濃いまたは淡色のテーマが適用されます。
- **Return key** –返されるアクションをより正確に反映するために、リターンキーのラベルを変更します。 サポートされる値は、[検索]、[結合]、[次へ]、[ルート]、[完了]、[検索] です。
- **Secure** –入力がマスクされているかどうか (パスワード入力など) を識別します。

## <a name="related-links"></a>関連リンク

- [検索コントローラー](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/search-controller)
