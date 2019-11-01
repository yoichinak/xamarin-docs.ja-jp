---
title: ViewPager とフラグメント
description: ViewPager は、gestural ナビゲーションを実装できるレイアウトマネージャーです。 Gestural ナビゲーションを使用すると、ユーザーは左右にスワイプしてデータのページをステップ実行できます。 このガイドでは、データページとしてフラグメントを使用して ViewPager で swipeable UI を実装する方法について説明します。
ms.prod: xamarin
ms.assetid: 62B6286F-3680-48F3-B91B-453692E457E5
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: d0fdfa44ce6caa0c5f0e0aa2eed6406e606eacc4
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029032"
---
# <a name="viewpager-with-fragments"></a>ViewPager とフラグメント

_ViewPager は、gestural ナビゲーションを実装できるレイアウトマネージャーです。Gestural ナビゲーションを使用すると、ユーザーは左右にスワイプしてデータのページをステップ実行できます。このガイドでは、データページとしてフラグメントを使用して ViewPager で swipeable UI を実装する方法について説明します。_

## <a name="overview"></a>概要

`ViewPager` は、`ViewPager`内の各ページのライフサイクルを管理しやすくするために、フラグメントと組み合わせて使用されることがよくあります。 このチュートリアルでは、`ViewPager` を使用して、フラッシュカードに一連の計算問題を示す**FlashCardPager**という名前のアプリを作成します。 各フラッシュカードは、フラグメントとして実装されます。 ユーザーは、フラッシュカードを左右にスワイプし、数値演算の問題をタップしてその答えを明らかにします。 このアプリは、各フラッシュカードの `Fragment` インスタンスを作成し、`FragmentPagerAdapter`から派生したアダプターを実装します。 [Viewpager とビュー](~/android/user-interface/controls/view-pager/viewpager-and-views.md)では、ほとんどの作業は `MainActivity` ライフサイクルメソッドで行われました。 **FlashCardPager**では、ほとんどの作業はライフサイクルメソッドの1つで `Fragment` によって行われます。 

このガイドでは、フラグメントの基本については説明しません。 Xamarin Android のフラグメントについてまだ詳しくない場合は、フラグメントの概要に関する記事[を参照し](~/android/platform/fragments/index.md)てください &ndash;。 

## <a name="start-an-app-project"></a>アプリプロジェクトを開始する

**FlashCardPager**という名前の新しい Android プロジェクトを作成します。 次に、NuGet パッケージマネージャーを起動します (NuGet パッケージのインストールの詳細については、「[チュートリアル: プロジェクトに nuget を含める](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)」を参照してください)。 「 [Viewpager とビュー](~/android/user-interface/controls/view-pager/viewpager-and-views.md)」で説明されているように、 **Xamarin. Android. サポート**パックを見つけてインストールします。 

## <a name="add-an-example-data-source"></a>サンプルデータソースを追加する

**FlashCardPager**では、データソースは `FlashCardDeck` クラスによって表されるフラッシュカードのデッキです。このデータソースは、項目の内容を含む `ViewPager` を提供します。 `FlashCardDeck` には、計算問題と回答の準備が整ったコレクションが含まれています。 `FlashCardDeck` コンストラクターには引数は必要ありません。 

```csharp
FlashCardDeck flashCards = new FlashCardDeck();
```

`FlashCardDeck` のフラッシュカードのコレクションは、各フラッシュカードがインデクサーによってアクセスできるように整理されています。 たとえば、次のコード行は、デッキの4番目のフラッシュカードの問題を取得します。 

```csharp
string problem = flashCardDeck[3].Problem;
```

次のコード行は、前の問題への対応する回答を取得します。

```csharp
string answer = flashCardDeck[3].Answer;
```

`FlashCardDeck` の実装の詳細は `ViewPager`を理解するためのものではないため、`FlashCardDeck` コードはここには記載されていません。
`FlashCardDeck` するソースコードは、 [FlashCardDeck.cs](https://github.com/xamarin/monodroid-samples/blob/master/UserInterface/FlashCardPager/FlashCardPager/FlashCardDeck.cs)で入手できます。
このソースファイルをダウンロードして (またはコードをコピーして新しい**FlashCardDeck.cs**ファイルに貼り付け)、プロジェクトに追加します。

## <a name="create-a-viewpager-layout"></a>ViewPager レイアウトを作成する

**Resources/layout/Main**を開き、その内容を次の xml に置き換えます。

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v4.view.ViewPager
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/viewpager"
    android:layout_width="match_parent"
    android:layout_height="match_parent" >

    </android.support.v4.view.ViewPager>
```

この XML は、画面全体を占有する `ViewPager` を定義します。 `ViewPager` はサポートライブラリにパッケージされているので、完全修飾名の android... **ViewPager**を使用する必要があることに注意してください。 `ViewPager` は、 [Android サポートライブラリ v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)からのみ使用できます。Android SDK では使用できません。

## <a name="set-up-viewpager"></a>ViewPager の設定

**MainActivity.cs**を編集し、次の `using` ステートメントを追加します。

```csharp
using Android.Support.V4.View;
using Android.Support.V4.App;
```

`FragmentActivity`から派生するように `MainActivity` クラス宣言を変更します。

```csharp
public class MainActivity : FragmentActivity
```

`MainActivity` は`FragmentActivity` (`Activity`ではなく) から派生しています。これは、フラグメントのサポートを管理する方法が `FragmentActivity` であるためです。 `OnCreate` メソッドを次のコードで置き換えます。 

```csharp
protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);
    SetContentView(Resource.Layout.Main);
    ViewPager viewPager = FindViewById<ViewPager>(Resource.Id.viewpager);
    FlashCardDeck flashCards = new FlashCardDeck();
}
```

このコードでは、次のことを行います。

1. **メインの axml**レイアウトリソースからビューを設定します。

2. レイアウトから `ViewPager` への参照を取得します。

3. 新しい `FlashCardDeck` をデータソースとしてインスタンス化します。

このコードをビルドして実行すると、次のスクリーンショットのような画面が表示されます。 

[空の ViewPager を含む FlashCardPager アプリのスクリーンショット![](viewpager-and-fragments-images/01-initial-screen-sml.png)](viewpager-and-fragments-images/01-initial-screen.png#lightbox)

この時点では、`ViewPager`にデータを設定するために使用されるフラグメントが不足しているため、`ViewPager` は空になります。また、 **FlashCardDeck**のデータからこれらのフラグメントを作成するためのアダプターが不足しています。 

次のセクションでは、各フラッシュカードの機能を実装するための `FlashCardFragment` を作成し、`FlashCardDeck`のデータから作成されたフラグメントに `ViewPager` を接続するための `FragmentPagerAdapter` を作成します。 

## <a name="create-the-fragment"></a>フラグメントを作成する

各フラッシュカードは、`FlashCardFragment`と呼ばれる UI フラグメントによって管理されます。 `FlashCardFragment`のビューには、1つのフラッシュカードに含まれる情報が表示されます。 `FlashCardFragment` の各インスタンスは、`ViewPager`によってホストされます。 
`FlashCardFragment`のビューは、フラッシュカードの問題のテキストを表示する `TextView` で構成されます。 このビューには、ユーザーがフラッシュカードの質問をタップしたときに回答を表示するために `Toast` を使用するイベントハンドラーが実装されます。 

### <a name="create-the-flashcardfragment-layout"></a>FlashCardFragment レイアウトを作成する

`FlashCardFragment` を実装するには、そのレイアウトを定義する必要があります。 このレイアウトは、1つのフラグメントのフラグメントコンテナーのレイアウトです。 **Flashcard_layout**という名前の**リソース/レイアウト**に新しい Android レイアウトを追加します。 **Resources/layout/flashcard_layout**を開き、内容を次のコードに置き換えます。 

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

このレイアウトでは、1つのフラッシュカードフラグメントを定義します。各フラグメントは、large (100sp) フォントを使用して数値演算の問題を表示する `TextView` で構成されます。 このテキストは、フラッシュカードの垂直方向および水平方向に中央揃えで配置されます。 

### <a name="create-the-initial-flashcardfragment-class"></a>最初の FlashCardFragment クラスを作成する

**FlashCardFragment.cs**という名前の新しいファイルを追加し、その内容を次のコードに置き換えます。

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

このコードは、フラッシュカードを表示するために使用される重要な `Fragment` 定義をスタブします。 `FlashCardFragment` は `Android.Support.V4.App.Fragment`で定義されている `Fragment` のサポートライブラリバージョンから派生したものであることに注意してください。 コンストラクターは空であるため、`newInstance` ファクトリメソッドを使用して、コンストラクターの代わりに新しい `FlashCardFragment` を作成します。 

`OnCreateView` ライフサイクルメソッドは、`TextView`を作成して構成します。 フラグメントの `TextView` のレイアウトを増えし、拡大した `TextView` を呼び出し元に返します。 レイアウトを拡張できるように、`LayoutInflater` と `ViewGroup` が `OnCreateView` に渡されます。 `savedInstanceState` バンドルには、保存された状態から `TextView` を再作成するために `OnCreateView` が使用するデータが含まれています。 

フラグメントのビューは、`inflater.Inflate`の呼び出しによって明示的に拡大されます。 `container` 引数はビューの親であり、`false` フラグは inflater に対して、ビューの親に対して拡大されたビューの追加を避けるように指示します (このチュートリアルで後ほど、アダプターの `GetItem` メソッドを `ViewPager` 呼び出したときに追加されます)。 

### <a name="add-state-code-to-flashcardfragment"></a>FlashCardFragment に状態コードを追加する

アクティビティと同様に、フラグメントには、その状態を保存および取得するために使用される `Bundle` があります。 **FlashCardPager**では、この `Bundle` は、関連付けられているフラッシュカードの質問と回答のテキストを保存するために使用されます。 **FlashCardFragment.cs**で、次の `Bundle` キーを `FlashCardFragment` クラス定義の先頭に追加します。 

```csharp
private static string FLASH_CARD_QUESTION = "card_question";
private static string FLASH_CARD_ANSWER = "card_answer";
```

`newInstance` ファクトリメソッドを変更して、`Bundle` オブジェクトを作成し、上記のキーを使用して、渡された質問と応答テキストをインスタンス化した後にフラグメントに格納します。 

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

`OnCreateView` フラグメントライフサイクルメソッドを変更して、渡されたバンドルからこの情報を取得し、`TextBox`に質問テキストを読み込みます。 

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

ここでは `answer` 変数は使用されませんが、後でイベントハンドラーコードをこのファイルに追加するときに使用されます。 

## <a name="create-the-adapter"></a>アダプターを作成する

`ViewPager` は、`ViewPager` とデータソースの間にあるアダプターコントローラーオブジェクトを使用します (ViewPager[アダプター](~/android/user-interface/controls/view-pager/index.md#adapter)の記事の図を参照してください)。 このデータにアクセスするには、`ViewPager` が `PagerAdapter`から派生したカスタムアダプターを提供する必要があります。 この例ではフラグメントを使用しているため、`PagerAdapter`から派生した `FragmentPagerAdapter` &ndash; `FragmentPagerAdapter` を使用します。 
`FragmentPagerAdapter` は、ユーザーがページに戻ることができる限り、フラグメントマネージャーに永続的に保持される `Fragment` として各ページを表します。 `ViewPager`のページを介してユーザーがスワイプと、`FragmentPagerAdapter` はデータソースから情報を抽出し、それを使用して `ViewPager` を表示するための `Fragment`を作成します。 

`FragmentPagerAdapter`を実装する場合は、次のものをオーバーライドする必要があります。

- 使用可能なビュー (ページ) の数を返す読み取り専用プロパティ &ndash;**カウント**します。

- **GetItem** &ndash; 指定されたページに表示するフラグメントを返します。

**FlashCardDeckAdapter.cs**という名前の新しいファイルを追加し、その内容を次のコードに置き換えます。

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

このコードは、重要な `FragmentPagerAdapter` 実装をスタブします。 次のセクションでは、これらの各メソッドが作業コードに置き換えられています。 コンストラクターの目的は、フラグメントマネージャーを `FlashCardDeckAdapter`の基本クラスコンストラクターに渡すことです。 

### <a name="implement-the-adapter-constructor"></a>アダプターコンストラクターを実装する

アプリによって `FlashCardDeckAdapter`がインスタンス化されると、フラグメントマネージャーとインスタンス化された `FlashCardDeck`への参照が提供されます。 **FlashCardDeckAdapter.cs**の `FlashCardDeckAdapter` クラスの先頭に、次のメンバー変数を追加します。 

```csharp
public FlashCardDeck flashCardDeck;
```

`FlashCardDeckAdapter` コンストラクターに次のコード行を追加します。 

```csharp
this.flashCardDeck = flashCards;
```

このコード行は、`FlashCardDeckAdapter` が使用する `FlashCardDeck` インスタンスを格納します。 

### <a name="implement-count"></a>実装数

`Count` の実装は比較的単純で、フラッシュカードデッキのフラッシュカードの数を返します。 `Count` を次のコードに置き換えます。 

```csharp
public override int Count
{
    get { return flashCardDeck.NumCards; }
}
```

`FlashCardDeck` の `NumCards` プロパティは、データセット内のフラッシュカード (フラグメントの数) の数を返します。 

### <a name="implement-getitem"></a>GetItem を実装する

`GetItem` メソッドは、指定された位置に関連付けられているフラグメントを返します。 フラッシュカードデッキの位置に対して `GetItem` が呼び出されると、その位置でフラッシュカードの問題を表示するように構成された `FlashCardFragment` が返されます。 `GetItem` メソッドを次のコードで置き換えます。 

```csharp
public override Android.Support.V4.App.Fragment GetItem(int position)
{
    return (Android.Support.V4.App.Fragment)
        FlashCardFragment.newInstance (
            flashCardDeck[position].Problem, flashCardDeck[position].Answer);
}
```

このコードでは、次のことを行います。

1. 指定された位置について、`FlashCardDeck` デッキ内の数値演算の問題文字列を検索します。 

2. 指定された位置の `FlashCardDeck` デッキ内の回答文字列を検索します。 

3. `FlashCardFragment` ファクトリメソッド `newInstance`を呼び出し、フラッシュカードの問題と応答文字列を渡します。 

4. 新しいフラッシュカード `Fragment` を作成して返します。このカードには、その位置の質問と回答のテキストが含まれています。 

`ViewPager` によって `position`で `Fragment` がレンダリングされると、フラッシュカードデッキの `position` にある算術問題文字列を含む `TextBox` が表示されます。 

## <a name="add-the-adapter-to-the-viewpager"></a>ViewPager にアダプターを追加する

`FlashCardDeckAdapter` が実装されたので、次に `ViewPager`に追加します。 **MainActivity.cs**で、`OnCreate` メソッドの末尾に次のコード行を追加します。

```csharp
FlashCardDeckAdapter adapter =
    new FlashCardDeckAdapter(SupportFragmentManager, flashCards);
viewPager.Adapter = adapter;
```

このコードは、最初の引数の `SupportFragmentManager` を渡して、`FlashCardDeckAdapter`をインスタンス化します。 (&ndash; `FragmentManager` への参照を取得するには、FragmentActivity の `SupportFragmentManager` プロパティを使用します。 `FragmentManager`の詳細については、「[フラグメントの管理](~/android/platform/fragments/managing-fragments.md)」を参照してください)。 

これで、アプリケーションをビルドして実行 &ndash; コア実装が完成しました。
次のスクリーンショットの左側に示すように、フラッシュカードデッキの最初の画像が画面に表示されます。 左にスワイプしてさらにフラッシュカードを確認し、次にスワイプして、フラッシュカードデッキに戻ります。

[![ポケットベルインジケーターのない FlashCardPager アプリのスクリーンショットの例](viewpager-and-fragments-images/02-example-views-sml.png)](viewpager-and-fragments-images/02-example-views.png#lightbox)

## <a name="add-a-pager-indicator"></a>ページャーインジケーターの追加

この最小 `ViewPager` 実装では、デッキの各フラッシュカードが表示されますが、ユーザーがデッキ内にある場所については示されません。 次の手順では、`PagerTabStrip`を追加します。 `PagerTabStrip` は、どの問題番号が表示されているかをユーザーに通知し、前のフラッシュカードと次のフラッシュカードのヒントを表示することによって、ナビゲーションコンテキストを提供します。 

**Resources/layout/Main**を開き、レイアウトに `PagerTabStrip` を追加します。

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

アプリをビルドして実行すると、各フラッシュカードの上部に空の `PagerTabStrip` が表示されます。 

[テキストを含まない PagerTabStrip の![クローズアップ](viewpager-and-fragments-images/03-empty-pagetabstrip-sml.png)](viewpager-and-fragments-images/03-empty-pagetabstrip.png#lightbox)

### <a name="display-a-title"></a>タイトルを表示する

各ページタブにタイトルを追加するには、アダプターに `GetPageTitleFormatted` メソッドを実装します。 `ViewPager` は `GetPageTitleFormatted` (実装されている場合) を呼び出して、指定した位置にあるページを説明するタイトル文字列を取得します。 **FlashCardDeckAdapter.cs**の `FlashCardDeckAdapter` クラスに次のメソッドを追加します。 

```csharp
public override Java.Lang.ICharSequence GetPageTitleFormatted(int position)
{
    return new Java.Lang.String("Problem " + (position + 1));
}
```

このコードは、flash カードデッキの位置を問題番号に変換します。 結果の文字列は、`ViewPager`に返される Java `String` に変換されます。 この新しい方法でアプリを実行すると、各ページには `PagerTabStrip`の問題番号が表示されます。 

[FlashCardPager のスクリーンショットに、各ページの上に表示されている問題番号を![します。](viewpager-and-fragments-images/04-pagetabstrip-sml.png)](viewpager-and-fragments-images/04-pagetabstrip.png#lightbox)

各フラッシュカードの上部に表示される flash カードデッキの問題番号は、前後にスワイプして確認できます。 

## <a name="handle-user-input"></a>ユーザー入力を処理する

**FlashCardPager**では、一連のフラグメントベースのフラッシュカードを `ViewPager`に提示していますが、各問題の答えを明らかにする方法はまだありません。 このセクションでは、ユーザーがフラッシュカードの問題のテキストをタップしたときに回答を表示するために、イベントハンドラーが `FlashCardFragment` に追加されます。 

**FlashCardFragment.cs**を開き、ビューが呼び出し元に返される直前に、`OnCreateView` メソッドの末尾に次のコードを追加します。 

```csharp
questionBox.Click += delegate
{
    Toast.MakeText(Activity.ApplicationContext,
            "Answer: " + answer, ToastLength.Short).Show();
};
```

この `Click` イベントハンドラーは、ユーザーが `TextBox`をタップしたときに表示されるトーストに回答を表示します。 `answer` 変数は、`OnCreateView`に渡されたバンドルから状態情報が読み取られたときに初期化されました。 アプリをビルドして実行し、各フラッシュカードの問題のテキストをタップして、回答を確認します。 

[算術問題がタップされたときに FlashCardPager app Toasts のスクリーンショットを![](viewpager-and-fragments-images/05-answer-sml.png)](viewpager-and-fragments-images/05-answer.png#lightbox)

このチュートリアルで示されている**FlashCardPager**は、`FragmentActivity`から派生した `MainActivity` を使用しますが、`AppCompatActivity` から `MainActivity` を派生させることもできます (これは、フラグメントの管理もサポートします)。 `AppCompatActivity` の例を表示するには、サンプルギャラリーの[FlashCardPager](https://docs.microsoft.com/samples/xamarin/monodroid-samples/userinterface-flashcardpager)を参照してください。

## <a name="summary"></a>まとめ

このチュートリアルでは、`Fragment`s を使用して基本的な `ViewPager`ベースのアプリを構築する方法の手順について説明しました。 この例では、フラッシュカードの質問と回答、フラッシュカードを表示するための `ViewPager` レイアウト、および `ViewPager` をデータソースに接続する `FragmentPagerAdapter` サブクラスを含むデータソースの例を示しています。 ユーザーがフラッシュカードを移動できるように、各ページの上部に問題番号を表示する `PagerTabStrip` を追加する方法を説明する手順が含まれていました。 最後に、ユーザーがフラッシュカードの問題をタップしたときに回答を表示するために、イベント処理コードが追加されました。 

## <a name="related-links"></a>関連リンク

- [FlashCardPager (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/userinterface-flashcardpager)
