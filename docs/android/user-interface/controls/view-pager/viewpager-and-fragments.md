---
title: "フラグメントの ViewPager"
description: "ViewPager は、レイアウト マネージャーで、ジェスチャー ナビゲーションを実装することができます。 ジェスチャー ナビゲーションにより、左と右のデータ ページの手順をスワイプするユーザー。 このガイドでは、データ ページとフラグメントを使用して、ViewPager で swipeable UI を実装する方法について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 62B6286F-3680-48F3-B91B-453692E457E5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: cd71617cce209ef0127023f69c2b503fee031e43
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="viewpager-with-fragments"></a>フラグメントの ViewPager

_ViewPager は、レイアウト マネージャーで、ジェスチャー ナビゲーションを実装することができます。ジェスチャー ナビゲーションにより、左と右のデータ ページの手順をスワイプするユーザー。このガイドでは、データ ページとフラグメントを使用して、ViewPager で swipeable UI を実装する方法について説明します。_

 
## <a name="overview"></a>概要

`ViewPager` 多くの場合と組み合わせて使用フラグメント内の各ページのライフ サイクルを管理しやすいように、`ViewPager`です。 このチュートリアルで`ViewPager`の作成に使用するアプリと呼ばれる**FlashCardPager**フラッシュ カード上で、一連の計算問題を表示します。 各フラッシュ カードは、フラグメントとして実装されます。 ユーザーは、left と right がフラッシュ カードを読み取ります、その回答を表示するために計算問題をタップします。 このアプリを作成、`Fragment`アダプターから派生した各フラッシュ カードと実装のインスタンス`FragmentPagerAdapter`です。 [Viewpager とビュー](~/android/user-interface/controls/view-pager/viewpager-and-views.md)でのほとんどの作業が行われていた`MainActivity`ライフ サイクル メソッドです。 **FlashCardPager**、ほとんどの作業は、によって行われます、`Fragment`のライフ サイクル メソッドのいずれか。 

このガイドではフラグメントの基本については説明しません&ndash;Xamarin.Android におけるフラグメントについて熟知がない場合は、次を参照してください。[フラグメント](~/android/platform/fragments/index.md)フラグメントを使用した作業を開始するためです。 



## <a name="start-an-app-project"></a>アプリ プロジェクトを開始します。

いう新しい Android プロジェクトを作成する**FlashCardPager**です。 次に、NuGet パッケージ マネージャーを起動 (NuGet パッケージのインストールに関する詳細については、次を参照してください。[チュートリアル: プロジェクトで、NuGet を含む](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough))。 検索し、インストール、 **Xamarin.Android.Support.v4**で説明したようにパッケージ化[Viewpager とビュー](~/android/user-interface/controls/view-pager/viewpager-and-views.md)です。 



## <a name="add-an-example-data-source"></a>例のデータ ソースを追加します。

**FlashCardPager**、データ ソースが 1 組のフラッシュ カードで表される、`FlashCardDeck`クラスです。 このデータ ソースを提供、`ViewPager`項目の内容にします。 `FlashCardDeck` 計算問題と回答の既製のコレクションが含まれています。 `FlashCardDeck`コンス トラクターに引数が必要ないです。 

```csharp
FlashCardDeck flashCards = new FlashCardDeck();
```

フラッシュ カードのコレクション`FlashCardDeck`構成により、インデクサーによって各フラッシュ カードにアクセスできます。 たとえば、次のコード行は、山札の 4 番目のフラッシュ カード問題を取得します。 

```csharp
string problem = flashCardDeck[3].Problem;
```

次のコード行は、前の問題に対応する応答を取得します。

```csharp
string answer = flashCardDeck[3].Answer;
```

の実装の詳細`FlashCardDeck`理解するには関係ない`ViewPager`、`FlashCardDeck`コードが記載されていません。
ソース コードを`FlashCardDeck`は、「 [FlashCardDeck.cs](https://github.com/xamarin/monodroid-samples/blob/master/UserInterface/FlashCardPager/FlashCardPager/FlashCardDeck.cs)です。
このソース ファイルをダウンロードする (またはコピーして、新しいコードを貼り付けます**FlashCardDeck.cs**ファイル) をプロジェクトに追加します。



## <a name="create-a-viewpager-layout"></a>ViewPager レイアウトを作成します。

開いている**Resources/layout/Main.axml**しその内容を次の XML に置き換えます。

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v4.view.ViewPager
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/viewpager"
    android:layout_width="match_parent"
    android:layout_height="match_parent" >

    </android.support.v4.view.ViewPager>
```

この XML を定義、`ViewPager`画面全体を占めることです。 完全修飾名を使用する必要があります**android.support.v4.view.ViewPager**ため`ViewPager`サポート ライブラリ内にパッケージ化します。 `ViewPager` のみ使用できますが、 [Android のサポート ライブラリ v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/); は、Android SDK で使用できません。


## <a name="set-up-viewpager"></a>ViewPager を設定します。

編集**MainActivity.cs**し、以下の追加`using`ステートメント。

```csharp
using Android.Support.V4.View;
using Android.Support.V4.App;
```

変更、`MainActivity`から派生したことができるように、クラス宣言`AppCompatActivity`:

```csharp
public class MainActivity : FragmentActivity
```

`MainActivity` 派生した`FragmentActivity`(なく`Activity`) ため`FragmentActivity`をフラグメントのサポートを管理する方法を認識しています。 `OnCreate` メソッドを次のコードで置き換えます。 

```csharp
protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);
    SetContentView(Resource.Layout.Main);
    ViewPager viewPager = FindViewById<ViewPager>(Resource.Id.viewpager);
    FlashCardDeck flashCards = new FlashCardDeck();
}
```

このコードは次のとおり

1.  ビューを設定、 **Main.axml**レイアウト リソース。

2.  参照を取得、`ViewPager`レイアウトからです。

3.  新しいインスタンスを作成`FlashCardDeck`データ ソースとして。

ビルドし、このコードを実行すると、次のスクリーン ショットのような画面が表示されます。 

[![空の ViewPager でスクリーン ショットの FlashCardPager アプリ](viewpager-and-fragments-images/01-initial-screen-sml.png)](viewpager-and-fragments-images/01-initial-screen.png#lightbox)

この時点で、`ViewPager`が空で使用される、フラグメントが不足しているためにの設定、 `ViewPager`、それが不足しているアダプター内のデータからこれらのフラグメントを作成するため、 **FlashCardDeck**です。 

次のセクションで、`FlashCardFragment`作成して各フラッシュ カードの機能を実装するには、`FragmentPagerAdapter`接続するため、`ViewPager`内のデータから作成されたフラグメントに、`FlashCardDeck`です。 



## <a name="create-the-fragment"></a>フラグメントを作成します。

呼ばれる UI フラグメントによって管理される各フラッシュ カード`FlashCardFragment`です。 `FlashCardFragment`1 つのフラッシュ カードに含まれる情報が表示されます。 各インスタンス`FlashCardFragment`によってホストされる、`ViewPager`です。 
`FlashCardFragment`ビューでは、`TextView`フラッシュ カードの問題のテキストを表示します。 このビューを使用するイベント ハンドラーを実装する`Toast`をユーザーがフラッシュ カード質問をタップしたときに、応答を表示します。 



### <a name="create-the-flashcardfragment-layout"></a>FlashCardFragment レイアウトを作成します。

前に`FlashCardFragment`できる実装されると、そのレイアウトが定義する必要があります。 このレイアウトは、1 つのフラグメントのフラグメント コンテナー レイアウトです。 新しい Android レイアウトを追加**リソース/レイアウト**と呼ばれる**flashcard_layout.axml**です。 開いている**Resources/layout/flashcard_layout.axml**しその内容を次のコードに置き換えます。 

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

このレイアウト定義単一フラッシュ カード フラグメントです。各フラグメントから成ります、`TextView`大きな (100sp) フォントを使用して計算問題を表示します。 このテキストは、フラッシュ カードで垂直および水平方向に中央揃えです。 



### <a name="create-the-initial-flashcardfragment-class"></a>初期 FlashCardFragment クラスを作成します。

という名前の新しいファイルを追加**FlashCardFragment.cs**しその内容を次のコードに置き換えます。

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

このコードをスタブして、必要な`Fragment`フラッシュ カードを表示するために使用されるを定義します。 なお`FlashCardFragment`のサポート ライブラリのバージョンから派生した`Fragment`で定義されている`Android.Support.V4.App.Fragment`です。 コンス トラクターが空ように、`newInstance`ファクトリ メソッドの作成に使用する新しい`FlashCardFragment`コンス トラクターの代わりにします。 

`OnCreateView`ライフ サイクル メソッドが作成され、構成、`TextView`です。 フラグメントのレイアウトを拡張`TextView`を高め、返します`TextView`呼び出し元にします。 `LayoutInflater` および`ViewGroup`に渡される`OnCreateView`レイアウトを拡大できるようにします。 `savedInstanceState`バンドルには、データが含まれています。 を`OnCreateView`は再作成を使用して、`TextView`から保存された状態です。 

フラグメントのビューがへの呼び出しによって大きく明示的に`inflater.Inflate`です。 `container`引数は、ビューの親と`false`フラグを高めのビューをビューの親に追加しないでくださいを超過するように指示 (に追加されます場合`ViewPager`呼び出しのアダプターの`GetItem`これ以降のメソッド。チュートリアルの場合)。 



### <a name="add-state-code-to-flashcardfragment"></a>FlashCardFragment に状態コードを追加します。

アクティビティと同様に、フラグメントがあります、`Bundle`を保存し、その状態の取得を使用することです。 **FlashCardPager**、この`Bundle`質問を保存し、関連付けられているフラッシュ カードのテキストに応答するために使用します。 **FlashCardFragment.cs**、次の追加`Bundle`の最上位にキー、`FlashCardFragment`クラス定義。 

```csharp
private static string FLASH_CARD_QUESTION = "card_question";
private static string FLASH_CARD_ANSWER = "card_answer";
```

変更、`newInstance`ファクトリ メソッドを作成するため、`Bundle`オブジェクトを渡された質問を保存してがインスタンス化された後に、フラグメント内のテキストを応答に上記のキーを使用します。 

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

フラグメントのライフ サイクル メソッドの変更`OnCreateView`を渡されたバンドルからこの情報を取得しに質問のテキストを読み込む、 `TextBox`: 

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

`answer`変数はここでは、使用されませんが、イベント ハンドラーのコードがこのファイルに追加されたときに後で使用するされます。 


## <a name="create-the-adapter"></a>アダプターを作成します。

`ViewPager` 間に位置するアダプター コント ローラー オブジェクトを使用して、`ViewPager`とデータ ソース (、ViewPager の図を参照してください[アダプター](~/android/user-interface/controls/view-pager/index.md#adapter)アーティクル)。 このデータにアクセスする`ViewPager`から派生したカスタム アダプターを指定する必要があります`PagerAdapter`です。 この例では、フラグメントを使用するため、使用、 `FragmentPagerAdapter` &ndash; `FragmentPagerAdapter`から派生した`PagerAdapter`です。 
`FragmentPagerAdapter` として各ページを表す、`Fragment`は永続的のフラグメント manager でのまま保持、ユーザーがページに戻ることができます。 ページをユーザーのカードとして、 `ViewPager`、`FragmentPagerAdapter`データ ソースから情報を抽出し、それを使用して作成`Fragment`の s、`ViewPager`を表示します。 

実装する場合、`FragmentPagerAdapter`次をオーバーライドする必要があります。

-   **カウント**&ndash;読み取り専用のプロパティを使用可能なビュー (ページ) の数を返します。

-   **GetItem** &ndash;指定したページに表示するフラグメントを返します。

という名前の新しいファイルを追加**FlashCardDeckAdapter.cs**しその内容を次のコードに置き換えます。

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

このコードをスタブして、必要な`FragmentPagerAdapter`実装します。 次のセクションでこれらの各メソッドは作業中のコードで置き換えられます。 コンス トラクターの目的にフラグメント manager を通過する、`FlashCardDeckAdapter`の基底クラス コンス トラクターです。 



### <a name="implement-the-adapter-constructor"></a>アダプター コンス トラクターを実装します。

アプリがインスタンス化、 `FlashCardDeckAdapter`、フラグメント マネージャーと、インスタンス化への参照を提供`FlashCardDeck`です。 先頭に次のメンバー変数を追加、`FlashCardDeckAdapter`クラス**FlashCardDeckAdapter.cs**: 

```csharp
public FlashCardDeck flashCardDeck;
```

次のコード行を追加、`FlashCardDeckAdapter`コンス トラクター。 

```csharp
this.flashCardDeck = flashCards;
```

コードのストアの行、`FlashCardDeck`インスタンスが、`FlashCardDeckAdapter`が使用されます。 



### <a name="implement-count"></a>実装のカウント

`Count`実装が比較的単純な: フラッシュ カード デッキ上のフラッシュ カードの数を返します。 `Count` を次のコードに置き換えます。 

```csharp
public override int Count
{
    get { return flashCardDeck.NumCards; }
}
```


`NumCards`プロパティ`FlashCardDeck`データ セット内でフラッシュ カード (フラグメントの数) の数を返します。 



### <a name="implement-getitem"></a>GetItem を実装します。

`GetItem`メソッドは、指定した位置に関連付けられているフラグメントを返します。 ときに`GetItem`が呼び出されると、フラッシュ カードの山札内の位置を返します、`FlashCardFragment`その位置にあるフラッシュ カードの問題を表示するように構成します。 `GetItem` メソッドを次のコードで置き換えます。 

```csharp
public override Android.Support.V4.App.Fragment GetItem(int position)
{
    return (Android.Support.V4.App.Fragment)
        FlashCardFragment.newInstance (
            flashCardDeck[position].Problem, flashCardDeck[position].Answer);
}
```

このコードは次のとおり

1.  文字列を検索、数式の問題で、`FlashCardDeck`指定した位置のデッキです。 

2.  応答文字列を検索、`FlashCardDeck`指定した位置のデッキです。 

3.  呼び出し、`FlashCardFragment`ファクトリ メソッド`newInstance`フラッシュ カードの問題と応答の文字列を渡して、します。 

4.  作成して新しいフラッシュ カードを返します`Fragment`その位置の質問と回答のテキストを格納しています。 

ときに、`ViewPager`レンダリング、`Fragment`で`position`、表示、`TextBox`に存在する計算問題文字列を含む`position`フラッシュ カード デッキ上の。 



## <a name="add-the-adapter-to-the-viewpager"></a>アダプター、ViewPager を追加します。

これで、`FlashCardDeckAdapter`に追加するとき、実装は、`ViewPager`です。 **MainActivity.cs**の末尾に次のコード行を追加、`OnCreate`メソッド。

```csharp
FlashCardDeckAdapter adapter =
    new FlashCardDeckAdapter(SupportFragmentManager, flashCards);
viewPager.Adapter = adapter;
```

このコードをインスタンス化、`FlashCardDeckAdapter`を渡して、`SupportFragmentManager`を最初の引数。 (、 `SupportFragmentManager` FragmentActivity のプロパティを使用してへの参照を取得、 `FragmentManager` &ndash;の詳細については、`FragmentManager`を参照してください[を管理するフラグメント](~/android/platform/fragments/managing-fragments.md))。 

実装の中核はこれで完了&ndash;をビルドして、アプリを実行します。
左側の [次へ] のスクリーン ショットに示すように画面に表示、フラッシュ カードの山札の最初のイメージが表示されます。 多くのフラッシュ カードを表示する左スワイプ履歴フラッシュ カード デッキ後方へ移動する右方向にスワイプし:

[![ポケットベル インジケーターせず FlashCardPager アプリのスクリーン ショットの例](viewpager-and-fragments-images/02-example-views-sml.png)](viewpager-and-fragments-images/02-example-views.png#lightbox)



## <a name="add-a-pager-indicator"></a>ポケットベル インジケーターを追加します。

この最小`ViewPager`実装、山札に各フラッシュ カードが表示されますが、ユーザーが、山札内にいるかを示す値が用意されていません。 次の手順が追加するには、`PagerTabStrip`です。 `PagerTabStrip`どのような問題に関する数が表示され、前または次のフラッシュ カードのヒントを表示することによってナビゲーション コンテキストを提供をユーザーに通知します。 

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

ビルド アプリを実行すると、空が表示されます`PagerTabStrip`各フラッシュ カードの上部に表示されます。 

[![テキストなし PagerTabStrip のクローズ アップ](viewpager-and-fragments-images/03-empty-pagetabstrip-sml.png)](viewpager-and-fragments-images/03-empty-pagetabstrip.png#lightbox)



### <a name="display-a-title"></a>タイトルを表示します。

各ページのタブにタイトルを追加するには、実装、`GetPageTitleFormatted`アダプター内のメソッドです。 `ViewPager` 呼び出し`GetPageTitleFormatted`(実装される場合) を指定した位置にあるページを表すタイトル文字列を取得します。 次のメソッドを追加、`FlashCardDeckAdapter`クラス**FlashCardDeckAdapter.cs**: 

```csharp
public override Java.Lang.ICharSequence GetPageTitleFormatted(int position)
{
    return new Java.Lang.String("Problem " + (position + 1));
}
```

このコードでは、フラッシュ カード デッキ上の位置を問題数に変換します。 結果の文字列は、Java に変換されます`String`に返される、`ViewPager`です。 この新しいメソッドを使用してアプリを実行するときに各ページ数を表示、問題で、 `PagerTabStrip`: 

[![各ページ上に表示される問題の数を持つ FlashCardPager のスクリーン ショット](viewpager-and-fragments-images/04-pagetabstrip-sml.png)](viewpager-and-fragments-images/04-pagetabstrip.png#lightbox)

各フラッシュ カードの上部に表示されるフラッシュ カード デッキ上の問題の数を表示する前後スワイプことができます。 



## <a name="handle-user-input"></a>ユーザー入力を処理します。

**FlashCardPager** 、一連のフラッシュ カードのフラグメント ベースの`ViewPager`がそれぞれの問題の解答を表示する方法をまだ存在しません。 このセクションで、イベント ハンドラーに追加、`FlashCardFragment`フラッシュ カード問題テキストで、ユーザーがタップしたときに、応答を表示します。 

開いている**FlashCardFragment.cs**の末尾に次のコードを追加し、`OnCreateView`メソッド ビューは、呼び出し元に返される直前に。 

```csharp
questionBox.Click += delegate
{
    Toast.MakeText(Activity.ApplicationContext,
            "Answer: " + answer, ToastLength.Short).Show();
};
```

これは、`Click`イベント ハンドラーは、ユーザーがタップしたときに表示されるトーストで回答を表示、`TextBox`です。 `answer`に渡されたバンドルから状態情報が読み取られたときに、変数が既に初期化されました`OnCreateView`です。 ビルドしアプリを実行し、回答を表示するには、各フラッシュ カード上の問題のテキストをタップします。 

[![スクリーン ショットの FlashCardPager アプリ トースト math 問題がタップされたとき](viewpager-and-fragments-images/05-answer-sml.png)](viewpager-and-fragments-images/05-answer.png#lightbox)

**FlashCardPager**このチュートリアルで説明を使用して、`MainActivity`から派生した`FragmentActivity`、派生することもできますが、`MainActivity`から`AppCompatActivity`(するサポートも提供フラグメントを管理するため)。 表示する、`AppCompatActivity`例を参照してください[FlashCardPager](https://developer.xamarin.com/samples/monodroid/UserInterface%5CFlashCardPager/)サンプル ギャラリーにします。 



## <a name="summary"></a>まとめ

このチュートリアルには、基本的な構築方法の手順の例が提供される`ViewPager`-アプリを使用してベース`Fragment`s。 フラッシュ カード質問と回答を含む例のデータ ソースを表示、`ViewPager`フラッシュ カードを表示するレイアウトと`FragmentPagerAdapter`接続サブクラス、`ViewPager`データ ソースにします。 フラッシュ カード内を移動、ユーザーのために、手順が含まれていたを追加する方法を説明する、`PagerTabStrip`を各ページの上部にある問題の番号を表示します。 最後に、イベント処理コードは、フラッシュ カードの問題にユーザーがタップしたときに、応答を表示する追加されました。 



## <a name="related-links"></a>関連リンク

- [FlashCardPager (サンプル)](https://developer.xamarin.com/samples/monodroid/UserInterface/FlashCardPager)
