---
title: ViewPager とフラグメント
description: ViewPager は、レイアウト マネージャー ジェスチャー ナビゲーションを実装することができます。 ジェスチャー ナビゲーションでは、左および右のデータ ページの手順をユーザーがスワイプができます。 このガイドでは、データ ページとしてフラグメントを使用して、ViewPager とスワイプ可能な UI を実装する方法について説明します。
ms.prod: xamarin
ms.assetid: 62B6286F-3680-48F3-B91B-453692E457E5
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: 748443352c106fbad88f8eda895cde097ce14a45
ms.sourcegitcommit: dd73477b1bccbd7ca45c1fb4e794da6b36ca163d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/30/2019
ms.locfileid: "66394708"
---
# <a name="viewpager-with-fragments"></a>ViewPager とフラグメント

_ViewPager は、レイアウト マネージャー ジェスチャー ナビゲーションを実装することができます。ジェスチャー ナビゲーションでは、左および右のデータ ページの手順をユーザーがスワイプができます。このガイドでは、データ ページとしてフラグメントを使用して、ViewPager とスワイプ可能な UI を実装する方法について説明します。_

 
## <a name="overview"></a>概要

`ViewPager` 内の各ページのライフ サイクルを管理しやすいように、フラグメントと組み合わせて使用されて多くの場合、`ViewPager`します。 このチュートリアルで`ViewPager`というアプリを作成するために使用**FlashCardPager**フラッシュ カードに一連の数学の問題を表示します。 各フラッシュ カードは、フラグメントとして実装されます。 ユーザーは、フラッシュ カードを左右にスワイプして、応答を表示する数学の問題をタップします。 このアプリを作成、`Fragment`アダプターから派生した各フラッシュ カードと実装のインスタンス`FragmentPagerAdapter`します。 [Viewpager とビュー](~/android/user-interface/controls/view-pager/viewpager-and-views.md)でほとんどの作業が行われていた`MainActivity`ライフ サイクル メソッド。 **FlashCardPager**、作業のほとんどは、によって行われます、`Fragment`そのライフ サイクル メソッドのいずれかでします。 

このガイドでは、フラグメントの基礎については説明しません&ndash;Xamarin.Android でフラグメント知識がない場合は、次を参照してください。[フラグメント](~/android/platform/fragments/index.md)フラグメントを使用できます。 



## <a name="start-an-app-project"></a>アプリ プロジェクトを開始します。

という新しい Android プロジェクトを作成**FlashCardPager**します。 次に、NuGet パッケージ マネージャーを起動 (NuGet パッケージのインストールの詳細については、次を参照してください。[チュートリアル。NuGet をプロジェクトに含める](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough))。 検索し、インストール、 **Xamarin.Android.Support.v4**で説明したようにパッケージ化[Viewpager とビュー](~/android/user-interface/controls/view-pager/viewpager-and-views.md)します。 



## <a name="add-an-example-data-source"></a>例のデータ ソースを追加します。

**FlashCardPager**、データ ソースは、トランプによって表される flash の`FlashCardDeck`クラス。 このデータ ソースを提供、`ViewPager`項目の内容にします。 `FlashCardDeck` 数学の問題とその回答の既製のコレクションが含まれています。 `FlashCardDeck`コンス トラクター引数は必要ありません。 

```csharp
FlashCardDeck flashCards = new FlashCardDeck();
```

フラッシュ カードでのコレクション`FlashCardDeck`は各フラッシュ カードは、インデクサーでアクセスできるように構成されています。 たとえば、次のコード行は、デッキ内の 4 番目のフラッシュ カード問題を取得します。 

```csharp
string problem = flashCardDeck[3].Problem;
```

次のコードは、前の問題に対応する応答を取得します。

```csharp
string answer = flashCardDeck[3].Answer;
```

の実装の詳細`FlashCardDeck`関係を理解することのない`ViewPager`、`FlashCardDeck`コードがここに表示されません。
ソース コードを`FlashCardDeck`いただけます[FlashCardDeck.cs](https://github.com/xamarin/monodroid-samples/blob/master/UserInterface/FlashCardPager/FlashCardPager/FlashCardDeck.cs)します。
このソース ファイルをダウンロード (またはコピーして、新しいコードを貼り付けます**FlashCardDeck.cs**ファイル)、プロジェクトに追加します。



## <a name="create-a-viewpager-layout"></a>ViewPager レイアウトを作成します。

開いている**Resources/layout/Main.axml**内容を次の XML に置き換えます。

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v4.view.ViewPager
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/viewpager"
    android:layout_width="match_parent"
    android:layout_height="match_parent" >

    </android.support.v4.view.ViewPager>
```

この XML を定義、`ViewPager`画面全体を占めるです。 完全修飾名を使用する必要があります**android.support.v4.view.ViewPager**ため`ViewPager`サポート ライブラリにパッケージ化されます。 `ViewPager` のみ使用できますが、 [Android サポート ライブラリ v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/); は、Android SDK では使用できません。


## <a name="set-up-viewpager"></a>ViewPager を設定します。

編集**MainActivity.cs**し、以下の追加`using`ステートメント。

```csharp
using Android.Support.V4.View;
using Android.Support.V4.App;
```

変更、`MainActivity`クラスから派生するように宣言`FragmentActivity`:

```csharp
public class MainActivity : FragmentActivity
```

`MainActivity` 派生`FragmentActivity`(なく`Activity`) ため、`FragmentActivity`フラグメントのサポートを管理する方法を認識します。 `OnCreate` メソッドを次のコードで置き換えます。 

```csharp
protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);
    SetContentView(Resource.Layout.Main);
    ViewPager viewPager = FindViewById<ViewPager>(Resource.Id.viewpager);
    FlashCardDeck flashCards = new FlashCardDeck();
}
```

このコードは、次を行います。

1.  設定から、ビュー、 **Main.axml**レイアウトのリソース。

2.  参照を取得、`ViewPager`レイアウトから。

3.  新しいインスタンスを作成`FlashCardDeck`データ ソースとして。

ビルドして、このコードを実行するときに、次のスクリーン ショットのような画面が表示されます。 

[![空の ViewPager とスクリーン ショットの FlashCardPager アプリ](viewpager-and-fragments-images/01-initial-screen-sml.png)](viewpager-and-fragments-images/01-initial-screen.png#lightbox)

この時点で、`ViewPager`設定されるフラグメントが不足しているために空は、 `ViewPager`、内のデータからこれらのフラグメントを作成するため、アダプターが不足しているが、 **FlashCardDeck**します。 

次のセクションで、`FlashCardFragment`は各フラッシュ カードの機能を実装するために作成、`FragmentPagerAdapter`が接続を作成、`ViewPager`内のデータから作成されたフラグメントを`FlashCardDeck`します。 



## <a name="create-the-fragment"></a>フラグメントを作成します。

呼ばれる UI フラグメントによって管理される各フラッシュ カード`FlashCardFragment`します。 `FlashCardFragment`1 つのフラッシュ カードに含まれる情報が表示されます。 各インスタンス`FlashCardFragment`によってホストされる、`ViewPager`します。 
`FlashCardFragment`ビューでは、`TextView`フラッシュ カードの問題のテキストを表示します。 このビューを使用するイベント ハンドラーを実装する`Toast`フラッシュ カード質問をユーザーがタップしたときに、回答を表示します。 



### <a name="create-the-flashcardfragment-layout"></a>FlashCardFragment レイアウトを作成します。

前に`FlashCardFragment`は、実装されると、そのレイアウトが定義する必要があります。 このレイアウトは、1 つのフラグメントのフラグメント コンテナー レイアウトです。 追加する場合は、新しい Android レイアウト**リソース/レイアウト**と呼ばれる**flashcard_layout.axml**します。 開いている**Resources/layout/flashcard_layout.axml**し、その内容を次のコードに置き換えます。 

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <TextView
        android:id="@+id/flash_card_question"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:gravity="center"
            android:textAppearance="@android:style/TextAppearance.Large"
            android:textSize="100sp"
            android:layout_centerHorizontal="true"
            android:layout_centerVertical="true"
            android:text="Question goes here" />
    </RelativeLayout>
```

このレイアウトは、フラッシュ カードの 1 つのフラグメントでは; を定義します。各フラグメントから成りますが、`TextView`大きな (100sp) フォントを使用して、数学の問題を表示します。 このテキストには、垂直方向および水平方向にフラッシュ カードの中央に配置します。 



### <a name="create-the-initial-flashcardfragment-class"></a>初期 FlashCardFragment クラスを作成します。

という新しいファイルを追加**FlashCardFragment.cs**し、その内容を次のコードに置き換えます。

```csharp
using System;
using Android.OS;
using Android.Views;
using Android.Widget;
using Android.Support.V4.App;

namespace FlashCardPager
{
    public class FlashCardFragment : Android.Support.V4.App.Fragment
    {
        public FlashCardFragment() { }

        public static FlashCardFragment newInstance(String question, String answer)
        {
            FlashCardFragment fragment = new FlashCardFragment();
            return fragment;
        }
        public override View OnCreateView (
            LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
        {
            View view = inflater.Inflate (Resource.Layout.flashcard_layout, container, false);
            TextView questionBox = (TextView)view.FindViewById (Resource.Id.flash_card_question);
            return view;
        }
    }
}
```

このコードをスタブして、必要な`Fragment`フラッシュ カードの表示に使用される定義。 なお`FlashCardFragment`のサポート ライブラリのバージョンからは派生`Fragment`で定義されている`Android.Support.V4.App.Fragment`します。 コンス トラクターは空ように、`newInstance`新たに作成するファクトリ メソッドが使用される`FlashCardFragment`コンス トラクターを代わりにします。 

`OnCreateView`ライフ サイクル メソッドを作成し、構成、`TextView`します。 フラグメントのレイアウトを拡張`TextView`を高め、返します`TextView`呼び出し元にします。 `LayoutInflater` `ViewGroup`に渡される`OnCreateView`レイアウトを拡張できるようにします。 `savedInstanceState`バンドルには、データが含まれています。 を`OnCreateView`は再作成を使用して、`TextView`から保存済みの状態。 

フラグメントのビューが明示的にへの呼び出しによって拡大しても`inflater.Inflate`します。 `container`引数は、ビューの親、および`false`フラグは、ビューの親に高めのビューを追加しないように inflater を指示します (ときに追加されます`ViewPager`の呼び出しは、アダプターの`GetItem`後でこのメソッドチュートリアルの場合)。 



### <a name="add-state-code-to-flashcardfragment"></a>FlashCardFragment に状態コードを追加します。

アクティビティと同様に、フラグメントがあります、`Bundle`を使用して保存し、その状態を取得することです。 **FlashCardPager**、この`Bundle`質問を保存し、関連付けられているフラッシュ カードのテキストに応答するために使用します。 **FlashCardFragment.cs**、次の追加`Bundle`キーの先頭に、`FlashCardFragment`クラスの定義。 

```csharp
private static string FLASH_CARD_QUESTION = "card_question";
private static string FLASH_CARD_ANSWER = "card_answer";
```

変更、`newInstance`ファクトリ メソッドを作成するため、`Bundle`オブジェクトがインスタンス化された後に、フラグメント内のテキストを応答を渡された質問を格納および上記のキーを使用しています。 

```csharp
public static FlashCardFragment newInstance(String question, String answer)
{
    FlashCardFragment fragment = new FlashCardFragment();

    Bundle args = new Bundle();
    args.PutString(FLASH_CARD_QUESTION, question);
    args.PutString(FLASH_CARD_ANSWER, answer);
    fragment.Arguments = args;

    return fragment;
}
```

フラグメントのライフ サイクル メソッドを変更`OnCreateView`で渡されるバンドルからこの情報を取得し、質問のテキストを読み込みます、 `TextBox`: 

```csharp
public override View OnCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
{
    string question = Arguments.GetString(FLASH_CARD_QUESTION, "");
    string answer = Arguments.GetString(FLASH_CARD_ANSWER, "");

    View view = inflater.Inflate(Resource.Layout.flashcard_layout, container, false);
    TextView questionBox = (TextView)view.FindViewById(Resource.Id.flash_card_question);
    questionBox.Text = question;

    return view;
}
```

`answer`変数はここでは、使用されませんが、イベント ハンドラーのコードがこのファイルに追加されたときに後で、使用されます。 


## <a name="create-the-adapter"></a>アダプターを作成します。

`ViewPager` 間に位置するアダプター コント ローラー オブジェクトを使用して、`ViewPager`とデータ ソース (、ViewPager の図を参照してください。[アダプター](~/android/user-interface/controls/view-pager/index.md#adapter)記事)。 このデータにアクセスする`ViewPager`から派生したカスタム アダプターを指定する必要があります`PagerAdapter`します。 この例では、フラグメントを使用するため、使用、 `FragmentPagerAdapter` &ndash; `FragmentPagerAdapter`から派生`PagerAdapter`します。 
`FragmentPagerAdapter` として各ページを表す、`Fragment`が永続的に保持されるフラグメントのマネージャーとして、ユーザーがページに戻ることができます。 ページのユーザーのカードとして、 `ViewPager`、`FragmentPagerAdapter`データ ソースから情報を抽出し、使用して作成する`Fragment`の`ViewPager`を表示します。 

実装する場合、`FragmentPagerAdapter`次をオーバーライドする必要があります。

-   **カウント**&ndash;利用可能なビュー (ページ) の数を返す、読み取り専用プロパティ。

-   **GetItem** &ndash;の指定したページを表示するフラグメントを返します。

という新しいファイルを追加**FlashCardDeckAdapter.cs**し、その内容を次のコードに置き換えます。

```csharp
using System;
using Android.Views;
using Android.Widget;
using Android.Support.V4.App;

namespace FlashCardPager
{
    class FlashCardDeckAdapter : FragmentPagerAdapter
    {
        public FlashCardDeckAdapter (Android.Support.V4.App.FragmentManager fm, FlashCardDeck flashCards)
            : base(fm)
        {
        }

        public override int Count
        {
            get { throw new NotImplementedException(); }
        }

        public override Android.Support.V4.App.Fragment GetItem(int position)
        {
            throw new NotImplementedException();
        }
    }
}
```

このコードをスタブして、必要な`FragmentPagerAdapter`実装します。 次のセクションでこれらの各メソッドは作業中のコードに置き換えられます。 コンス トラクターの目的は、フラグメントのマネージャーに渡す、`FlashCardDeckAdapter`の基本クラス コンス トラクター。 



### <a name="implement-the-adapter-constructor"></a>アダプター コンス トラクターを実装します。

アプリがインスタンス化、 `FlashCardDeckAdapter`、フラグメントのマネージャーと、インスタンス化への参照を提供`FlashCardDeck`します。 先頭に次のメンバー変数を追加、`FlashCardDeckAdapter`クラス**FlashCardDeckAdapter.cs**: 

```csharp
public FlashCardDeck flashCardDeck;
```

次のコードの行を追加、`FlashCardDeckAdapter`コンス トラクター。 

```csharp
this.flashCardDeck = flashCards;
```

コードのストアの行、`FlashCardDeck`インスタンスが、`FlashCardDeckAdapter`が使用されます。 



### <a name="implement-count"></a>実装の数

`Count`実装が比較的単純な: フラッシュ カード デッキでフラッシュ カードの数を返します。 `Count` を次のコードに置き換えます。 

```csharp
public override int Count
{
    get { return flashCardDeck.NumCards; }
}
```


`NumCards`プロパティの`FlashCardDeck`データ セット内でフラッシュ カード (フラグメントの数) の数を返します。 



### <a name="implement-getitem"></a>GetItem を実装します。

`GetItem`メソッドは、指定した位置に関連付けられているフラグメントを返します。 ときに`GetItem`が呼び出されると、フラッシュ カード デッキ内の位置を返します、`FlashCardFragment`その位置にあるフラッシュ カードの問題を表示するように構成します。 `GetItem` メソッドを次のコードで置き換えます。 

```csharp
public override Android.Support.V4.App.Fragment GetItem(int position)
{
    return (Android.Support.V4.App.Fragment)
        FlashCardFragment.newInstance (
            flashCardDeck[position].Problem, flashCardDeck[position].Answer);
}
```

このコードは、次を行います。

1.  数学の問題の文字列を検索、`FlashCardDeck`デッキの指定した位置。 

2.  応答文字列を検索、`FlashCardDeck`デッキの指定した位置。 

3.  呼び出し、`FlashCardFragment`ファクトリ メソッド`newInstance`、フラッシュ カードの問題と応答の文字列に渡します。 

4.  作成し、新しいフラッシュ カードを返します`Fragment`その位置の質問と答えのテキストを格納しています。 

ときに、`ViewPager`レンダリング、`Fragment`で`position`が表示されます、`TextBox`に存在する数学の問題の文字列を含む`position`フラッシュ カード デッキでします。 



## <a name="add-the-adapter-to-the-viewpager"></a>アダプター、ViewPager を追加します。

これで、`FlashCardDeckAdapter`が実装されると、いよいよに追加する、`ViewPager`します。 **MainActivity.cs**の末尾に次のコード行を追加、`OnCreate`メソッド。

```csharp
FlashCardDeckAdapter adapter =
    new FlashCardDeckAdapter(SupportFragmentManager, flashCards);
viewPager.Adapter = adapter;
```

このコードをインスタンス化、`FlashCardDeckAdapter`で渡し、`SupportFragmentManager`最初の引数。 (、 `SupportFragmentManager` FragmentActivity のプロパティを使用してへの参照を取得、 `FragmentManager` &ndash;の詳細については、`FragmentManager`を参照してください[管理フラグメント](~/android/platform/fragments/managing-fragments.md))。 

実装の中核はこれで完了&ndash;をビルドして、アプリを実行します。
左側の [次へ] のスクリーン ショットに示すように、画面に表示フラッシュ カード デッキの最初のイメージが表示されます。 左からより多くのフラッシュ カードを参照してください。 スワイプ履歴後方へ移動するフラッシュ カード デッキを右方向にスワイプし:

[![ポケットベル インジケーターせず FlashCardPager アプリのスクリーン ショットの例](viewpager-and-fragments-images/02-example-views-sml.png)](viewpager-and-fragments-images/02-example-views.png#lightbox)



## <a name="add-a-pager-indicator"></a>ポケットベル インジケーターを追加します。

この最小`ViewPager`実装、デッキの各フラッシュ カードが表示されますが、デッキ内のユーザーのいるかを示す値は提供されません。 次の手順が追加するには、`PagerTabStrip`します。 `PagerTabStrip`をどのような問題に関する数が表示され、前後のフラッシュ カードのヒントを表示することによってナビゲーション コンテキストを提供します。 ユーザーに通知します。 

開いている**Resources/layout/Main.axml**を追加し、`PagerTabStrip`レイアウトに。

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v4.view.ViewPager xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/pager"
    android:layout_width="match_parent"
    android:layout_height="match_parent" >

  <android.support.v4.view.PagerTabStrip
      android:layout_width="match_parent"
      android:layout_height="wrap_content"
      android:layout_gravity="top"
      android:paddingBottom="10dp"
      android:paddingTop="10dp"
      android:textColor="#fff" />

</android.support.v4.view.ViewPager>
```

空がわかりますビルド アプリを実行すると、`PagerTabStrip`各フラッシュ カードの上部に表示されます。 

[![テキストなし PagerTabStrip のクローズ アップ](viewpager-and-fragments-images/03-empty-pagetabstrip-sml.png)](viewpager-and-fragments-images/03-empty-pagetabstrip.png#lightbox)



### <a name="display-a-title"></a>タイトルを表示します。

各ページのタブにタイトルを追加するには、実装、`GetPageTitleFormatted`アダプター内のメソッド。 `ViewPager` 呼び出し`GetPageTitleFormatted`(実装される) 場合、指定した位置にあるページを表すタイトル文字列を取得します。 次のメソッドを追加、`FlashCardDeckAdapter`クラス**FlashCardDeckAdapter.cs**: 

```csharp
public override Java.Lang.ICharSequence GetPageTitleFormatted(int position)
{
    return new Java.Lang.String("Problem " + (position + 1));
}
```

このコードでは、フラッシュ カード デッキ内の位置を問題の数に変換します。 結果の文字列は、Java に変換されます`String`に返される、`ViewPager`します。 各ページをこの新しいメソッドをアプリを実行したの問題の数が表示されます、 `PagerTabStrip`: 

[![各ページ上に表示される問題番号 FlashCardPager のスクリーン ショット](viewpager-and-fragments-images/04-pagetabstrip-sml.png)](viewpager-and-fragments-images/04-pagetabstrip.png#lightbox)

各フラッシュ カードの上部に表示されるフラッシュ カード デッキでの問題の数を表示する前後へスワイプできます。 



## <a name="handle-user-input"></a>ユーザー入力を処理します。

**FlashCardPager** 、一連のフラッシュ カードのフラグメント ベースの`ViewPager`が、まだがない各問題の解答を表示する方法。 このセクションでは、イベント ハンドラーに追加されます、`FlashCardFragment`フラッシュ カードの問題のテキストでユーザーがタップしたときに、回答を表示します。 

開いている**FlashCardFragment.cs**の末尾に次のコードを追加し、`OnCreateView`メソッドが呼び出し元に返された、表示する前に。 

```csharp
questionBox.Click += delegate
{
    Toast.MakeText(Activity.ApplicationContext,
            "Answer: " + answer, ToastLength.Short).Show();
};
```

これは、`Click`イベント ハンドラーは、ユーザーがタップしたときに表示されるトーストで回答を表示、`TextBox`します。 `answer`に渡されたバンドルから状態情報が読み取られたときに変数が既に初期化されました`OnCreateView`します。 ビルドして、アプリの実行を応答を表示するには、各フラッシュ カード上の問題のテキストをタップします。 

[![スクリーン ショットの FlashCardPager アプリ数学の問題がタップされたときに、トースト通知](viewpager-and-fragments-images/05-answer-sml.png)](viewpager-and-fragments-images/05-answer.png#lightbox)

**FlashCardPager**このチュートリアルで説明を使用して、`MainActivity`から派生した`FragmentActivity`、派生することもできますが、`MainActivity`から`AppCompatActivity`(もサポートを提供しますフラグメントを管理するため)。 表示する、`AppCompatActivity`例を参照してください[FlashCardPager](https://developer.xamarin.com/samples/monodroid/UserInterface%5CFlashCardPager/)サンプル ギャラリーにします。 



## <a name="summary"></a>まとめ

このチュートリアルには、基本的なを構築する方法の手順の例が提供されている`ViewPager`-を使用してアプリ ベース`Fragment`秒。 フラッシュ カードの質問と回答を格納している例のデータ ソースを表示、 `ViewPager` 、フラッシュ カードを表示するレイアウトと`FragmentPagerAdapter`接続サブクラス、`ViewPager`データ ソースにします。 手順が含まれていたフラッシュ カード間を移動するユーザーのため、追加する方法を説明する、`PagerTabStrip`各ページの上部にある問題の数を表示します。 最後に、イベント処理コードは、フラッシュ カードの問題に、ユーザーがタップしたときに回答の表示に追加されました。 



## <a name="related-links"></a>関連リンク

- [FlashCardPager (サンプル)](https://developer.xamarin.com/samples/monodroid/UserInterface/FlashCardPager)
