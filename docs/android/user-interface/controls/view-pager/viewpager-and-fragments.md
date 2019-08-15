---
title: ViewPager とフラグメント
description: ViewPager は、gestural ナビゲーションを実装できるレイアウトマネージャーです。 Gestural ナビゲーションを使用すると、ユーザーは左右にスワイプしてデータのページをステップ実行できます。 このガイドでは、データページとしてフラグメントを使用して ViewPager で swipeable UI を実装する方法について説明します。
ms.prod: xamarin
ms.assetid: 62B6286F-3680-48F3-B91B-453692E457E5
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: 90bffc2360654f571728f76810f144e702a81e57
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68646103"
---
# <a name="viewpager-with-fragments"></a>ViewPager とフラグメント

_ViewPager は、gestural ナビゲーションを実装できるレイアウトマネージャーです。Gestural ナビゲーションを使用すると、ユーザーは左右にスワイプしてデータのページをステップ実行できます。このガイドでは、データページとしてフラグメントを使用して ViewPager で swipeable UI を実装する方法について説明します。_

 
## <a name="overview"></a>概要

`ViewPager`は、の各ページのライフサイクルを簡単に管理できるように、 `ViewPager`フラグメントと組み合わせて使用されることがよくあります。 このチュートリアルでは`ViewPager` 、を使用して、 **FlashCardPager**という名前のアプリを作成し、フラッシュカードに一連の計算問題を提示します。 各フラッシュカードは、フラグメントとして実装されます。 ユーザーは、フラッシュカードを左右にスワイプし、数値演算の問題をタップしてその答えを明らかにします。 このアプリは、 `Fragment`各フラッシュカードのインスタンスを作成し、から`FragmentPagerAdapter`派生したアダプターを実装します。 [Viewpager とビュー](~/android/user-interface/controls/view-pager/viewpager-and-views.md)では、ほとんどの作業はライフサイクル`MainActivity`メソッドで実行されていました。 **FlashCardPager**では、ほとんどの作業はライフサイクルメソッドの`Fragment` 1 つでによって行われます。 

このガイドでは、Xamarin Android のフラグメント&ndash;についてまだよく知らない場合のフラグメントの基本については説明しません。フラグメントの概要については、「フラグメント」[を参照し](~/android/platform/fragments/index.md)てください。 



## <a name="start-an-app-project"></a>アプリプロジェクトを開始する

**FlashCardPager**という名前の新しい Android プロジェクトを作成します。 次に、nuget パッケージマネージャーを起動します (nuget パッケージのインストールの詳細[については、「チュートリアル:プロジェクト](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)に NuGet を含めます)。 「 [Viewpager とビュー](~/android/user-interface/controls/view-pager/viewpager-and-views.md)」で説明されているように、 **Xamarin. Android. サポート**パックを見つけてインストールします。 



## <a name="add-an-example-data-source"></a>サンプルデータソースを追加する

**FlashCardPager**では、データソースは`FlashCardDeck`クラスによって表されるフラッシュカードのデッキです。このデータソースは、 `ViewPager`に項目の内容を提供します。 `FlashCardDeck`数値演算の問題と回答の既製のコレクションが含まれています。 コンストラクター `FlashCardDeck`には引数は必要ありません。 

```csharp
FlashCardDeck flashCards = new FlashCardDeck();
```

の`FlashCardDeck`フラッシュカードのコレクションは、各フラッシュカードがインデクサーによってアクセスできるように編成されています。 たとえば、次のコード行は、デッキの4番目のフラッシュカードの問題を取得します。 

```csharp
string problem = flashCardDeck[3].Problem;
```

次のコード行は、前の問題への対応する回答を取得します。

```csharp
string answer = flashCardDeck[3].Answer;
```

の実装の`FlashCardDeck`詳細は理解`ViewPager`には関係がないため`FlashCardDeck` 、コードはここには記載されていません。
のソースコード`FlashCardDeck`は、 [FlashCardDeck.cs](https://github.com/xamarin/monodroid-samples/blob/master/UserInterface/FlashCardPager/FlashCardPager/FlashCardDeck.cs)で入手できます。
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

この XML は、 `ViewPager`画面全体を占有するを定義します。 `ViewPager`はサポートライブラリにパッケージされているので、完全修飾名の "android... **viewpager** " を使用する必要があることに注意してください。 `ViewPager`は、 [Android サポートライブラリ v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)からのみ使用できます。Android SDK では使用できません。


## <a name="set-up-viewpager"></a>ViewPager の設定

**MainActivity.cs**を編集し、次`using`のステートメントを追加します。

```csharp
using Android.Support.V4.View;
using Android.Support.V4.App;
```

クラス宣言`MainActivity`を変更して、から`FragmentActivity`派生するようにします。

```csharp
public class MainActivity : FragmentActivity
```

`MainActivity`は、フラグメント`FragmentActivity`のサポートを管理する`FragmentActivity`方法を知っているので、( `Activity`ではなく) から派生します。 `OnCreate` メソッドを次のコードで置き換えます。 

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

1.  **メインの axml**レイアウトリソースからビューを設定します。

2.  レイアウトからへの`ViewPager`参照を取得します。

3.  新しい`FlashCardDeck`をデータソースとしてインスタンス化します。

このコードをビルドして実行すると、次のスクリーンショットのような画面が表示されます。 

[![空の ViewPager を使用した FlashCardPager アプリのスクリーンショット](viewpager-and-fragments-images/01-initial-screen-sml.png)](viewpager-and-fragments-images/01-initial-screen.png#lightbox)

この時点`ViewPager`では、にデータを`ViewPager`設定するために使用されるフラグメントが不足しているため、は空になります。また、 **FlashCardDeck**のデータからこれらのフラグメントを作成するためのアダプターが不足しています。 

次の`FlashCardFragment`セクションでは、が作成され、各フラッシュカードの機能が実装さ`FragmentPagerAdapter`れ`FlashCardDeck`ます。また、 `ViewPager`は、のデータから作成されたフラグメントに接続するために作成されます。 



## <a name="create-the-fragment"></a>フラグメントを作成する

各フラッシュカードは、という`FlashCardFragment`UI フラグメントによって管理されます。 `FlashCardFragment`のビューには、1つのフラッシュカードに含まれる情報が表示されます。 の`FlashCardFragment`各インスタンスは、 `ViewPager`によってホストされます。 
`FlashCardFragment`のビューは、フラッシュカード`TextView`の問題のテキストを表示するで構成されます。 このビューは、ユーザーが flash カードの質問`Toast`をタップしたときにを使用して回答を表示するイベントハンドラーを実装します。 



### <a name="create-the-flashcardfragment-layout"></a>FlashCardFragment レイアウトを作成する

を`FlashCardFragment`実装する前に、そのレイアウトを定義する必要があります。 このレイアウトは、1つのフラグメントのフラグメントコンテナーのレイアウトです。 **Flashcard_layout**という名前の**リソース/レイアウト**に新しい Android レイアウトを追加します。 **Resources/layout/flashcard_layout**を開き、内容を次のコードに置き換えます。 

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

このレイアウトでは、1つのフラッシュカードフラグメントを定義します。各フラグメントは、large ( `TextView` 100sp) フォントを使用して数値演算の問題を表示するで構成されます。 このテキストは、フラッシュカードの垂直方向および水平方向に中央揃えで配置されます。 



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

このコードは、フラッシュカード`Fragment`を表示するために使用される基本的な定義をスタブにします。 は、 `Fragment` で`Android.Support.V4.App.Fragment`定義されているのサポートライブラリバージョンから派生します。 `FlashCardFragment` コンストラクターは空であるため、 `newInstance`ファクトリメソッドを使用してコンストラクターの`FlashCardFragment`代わりに新しいを作成できます。 

ライフ`OnCreateView`サイクルメソッドは、を`TextView`作成して構成します。 この例では、フラグメントの`TextView`のレイアウトを増え、呼び出し元`TextView`に大きくなったを返します。 `LayoutInflater`と`ViewGroup`は、レイアウト`OnCreateView`を膨張させるためにに渡されます。 バンドルには、 `OnCreateView` `TextView`が保存された状態からを再作成するために使用するデータが含まれています。 `savedInstanceState` 

フラグメントのビューは、の呼び出し`inflater.Inflate`によって明示的に拡大されます。 引数はビューの親であり、フラグは`false` inflater に、ビューの親に対して拡大されたビューを追加しないように指示し`ViewPager`ます (これは、 `GetItem`後でアダプターのメソッドを呼び出したときに追加されます)。 `container`チュートリアル)。 



### <a name="add-state-code-to-flashcardfragment"></a>FlashCardFragment に状態コードを追加する

アクティビティと同様に、フラグメントには`Bundle` 、その状態を保存および取得するために使用されるがあります。 **FlashCardPager**では、 `Bundle`これは、関連付けられているフラッシュカードの質問と回答のテキストを保存するために使用されます。 **FlashCardFragment.cs**で、 `Bundle` `FlashCardFragment`クラス定義の先頭に次のキーを追加します。 

```csharp
private static string FLASH_CARD_QUESTION = "card_question";
private static string FLASH_CARD_ANSWER = "card_answer";
```

オブジェクトを`newInstance` `Bundle`作成し、上記のキーを使用して、インスタンス化された後に、渡された質問と回答のテキストをフラグメントに格納するように、ファクトリメソッドを変更します。 

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

Fragment ライフサイクルメソッド`OnCreateView`を変更して、 `TextBox`渡されたバンドルからこの情報を取得し、に質問テキストを読み込みます。 

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

ここでは変数は使用されませんが、後でイベントハンドラーコードをこのファイルに追加するときに使用されます。 `answer` 


## <a name="create-the-adapter"></a>アダプターを作成する

`ViewPager`は、データソース`ViewPager`とデータソースの間にあるアダプターコントローラーオブジェクトを使用します (viewpager[アダプター](~/android/user-interface/controls/view-pager/index.md#adapter)の記事の図を参照してください)。 このデータにアクセスする`ViewPager`には、から`PagerAdapter`派生したカスタムアダプターを指定する必要があります。 この例ではフラグメントを使用して`FragmentPagerAdapter`いるため、 `PagerAdapter`はから派生したを&ndash; `FragmentPagerAdapter`使用します。 
`FragmentPagerAdapter`ユーザーがページに戻る`Fragment`ことができる限り、フラグメントマネージャーに永続的に保持されるとして各ページを表します。 ユーザーが`ViewPager`のページをスワイプとすると、 `FragmentPagerAdapter`はデータソースから情報を抽出し、それを`Fragment`使用して`ViewPager`を作成し、を表示します。 

を実装`FragmentPagerAdapter`する場合は、次のものをオーバーライドする必要があります。

-   **カウント**&ndash;使用可能なビュー (ページ) の数を返す読み取り専用のプロパティ。

-   **GetItem**&ndash;指定されたページに表示するフラグメントを返します。

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

このコードは、重要`FragmentPagerAdapter`な実装をスタブにします。 次のセクションでは、これらの各メソッドが作業コードに置き換えられています。 コンストラクターの目的は、フラグメントマネージャーを`FlashCardDeckAdapter`の基底クラスコンストラクターに渡すことです。 



### <a name="implement-the-adapter-constructor"></a>アダプターコンストラクターを実装する

アプリによってが`FlashCardDeckAdapter`インスタンス化されると、フラグメントマネージャーとインスタンス化`FlashCardDeck`されたへの参照が提供されます。 `FlashCardDeckAdapter` **FlashCardDeckAdapter.cs**のクラスの先頭に、次のメンバー変数を追加します。 

```csharp
public FlashCardDeck flashCardDeck;
```

`FlashCardDeckAdapter`コンストラクターに次のコード行を追加します。 

```csharp
this.flashCardDeck = flashCards;
```

このコード行は、 `FlashCardDeck` `FlashCardDeckAdapter`が使用するインスタンスを格納します。 



### <a name="implement-count"></a>実装数

`Count`実装は比較的単純で、フラッシュカードデッキのフラッシュカードの数を返します。 `Count` を次のコードに置き換えます。 

```csharp
public override int Count
{
    get { return flashCardDeck.NumCards; }
}
```


`NumCards` の`FlashCardDeck`プロパティは、データセット内のフラッシュカード (フラグメントの数) の数を返します。 



### <a name="implement-getitem"></a>GetItem を実装する

メソッド`GetItem`は、指定された位置に関連付けられているフラグメントを返します。 フラッシュ`GetItem`カードデッキ内の位置に対してが呼び出されると、その`FlashCardFragment`位置でフラッシュカードの問題を表示するように構成されたを返します。 `GetItem` メソッドを次のコードで置き換えます。 

```csharp
public override Android.Support.V4.App.Fragment GetItem(int position)
{
    return (Android.Support.V4.App.Fragment)
        FlashCardFragment.newInstance (
            flashCardDeck[position].Problem, flashCardDeck[position].Answer);
}
```

このコードでは、次のことを行います。

1.  指定された位置について`FlashCardDeck` 、デッキ内の数値演算の問題文字列を検索します。 

2.  指定された位置につい`FlashCardDeck`て、デッキ内の応答文字列を検索します。 

3.  `FlashCardFragment`ファクトリメソッド`newInstance`を呼び出し、フラッシュカードの問題と応答文字列を渡します。 

4.  その位置の質問と回答の`Fragment`テキストを含む新しいフラッシュカードを作成して返します。 

が`ViewPager` で`Fragment` `TextBox` `position`レンダリングされると、フラッシュカードデッキのにある数学問題文字列を含むが表示されます。 `position` 



## <a name="add-the-adapter-to-the-viewpager"></a>ViewPager にアダプターを追加する

が実装されたので、これ`ViewPager`をに追加します。 `FlashCardDeckAdapter` **MainActivity.cs**で、 `OnCreate`メソッドの末尾に次のコード行を追加します。

```csharp
FlashCardDeckAdapter adapter =
    new FlashCardDeckAdapter(SupportFragmentManager, flashCards);
viewPager.Adapter = adapter;
```

このコードは、 `FlashCardDeckAdapter`をインスタンス化し`SupportFragmentManager` 、1番目の引数のを渡します。 (`SupportFragmentManager``FragmentManager` &ndash;`FragmentManager` の詳細については、「[フラグメントの管理](~/android/platform/fragments/managing-fragments.md)」を参照してください。) 

コア実装は、アプリの&ndash;ビルドと実行が完了しました。
次のスクリーンショットの左側に示すように、フラッシュカードデッキの最初の画像が画面に表示されます。 左にスワイプしてさらにフラッシュカードを確認し、次にスワイプして、フラッシュカードデッキに戻ります。

[![ページャーインジケーターのない FlashCardPager アプリのスクリーンショットの例](viewpager-and-fragments-images/02-example-views-sml.png)](viewpager-and-fragments-images/02-example-views.png#lightbox)



## <a name="add-a-pager-indicator"></a>ページャーインジケーターの追加

この最小`ViewPager`実装では、デッキの各フラッシュカードが表示されますが、ユーザーがデッキ内にある場所については示されません。 次の手順では、を`PagerTabStrip`追加します。 は`PagerTabStrip` 、どの問題番号が表示されているかをユーザーに通知し、前のフラッシュカードと次のフラッシュカードのヒントを表示することによって、ナビゲーションコンテキストを提供します。 

**Resources/layout/Main**を開き、を`PagerTabStrip`レイアウトに追加します。

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

アプリをビルドして実行すると、各フラッシュカードの`PagerTabStrip`上部に空のが表示されます。 

[![テキストを含まない PagerTabStrip のクローズアップ](viewpager-and-fragments-images/03-empty-pagetabstrip-sml.png)](viewpager-and-fragments-images/03-empty-pagetabstrip.png#lightbox)



### <a name="display-a-title"></a>タイトルを表示する

各ページタブにタイトルを追加するには、 `GetPageTitleFormatted`アダプターにメソッドを実装します。 `ViewPager`( `GetPageTitleFormatted`実装されている場合) を呼び出して、指定した位置にあるページを説明するタイトル文字列を取得します。 `FlashCardDeckAdapter` **FlashCardDeckAdapter.cs**のクラスに次のメソッドを追加します。 

```csharp
public override Java.Lang.ICharSequence GetPageTitleFormatted(int position)
{
    return new Java.Lang.String("Problem " + (position + 1));
}
```

このコードは、flash カードデッキの位置を問題番号に変換します。 結果の文字列は、 `String` `ViewPager`に返される Java に変換されます。 この新しい方法でアプリを実行すると、各ページに`PagerTabStrip`問題の番号がに表示されます。 

[![各ページの上に表示されている問題番号を含む FlashCardPager のスクリーンショット](viewpager-and-fragments-images/04-pagetabstrip-sml.png)](viewpager-and-fragments-images/04-pagetabstrip.png#lightbox)

各フラッシュカードの上部に表示される flash カードデッキの問題番号は、前後にスワイプして確認できます。 



## <a name="handle-user-input"></a>ユーザー入力を処理する

**FlashCardPager**では`ViewPager`、に一連のフラグメントベースのフラッシュカードが示されていますが、各問題の答えを明らかにする方法はまだありません。 このセクションでは、ユーザーがフラッシュカードの問題`FlashCardFragment`のテキストをタップしたときに回答を表示するために、イベントハンドラーをに追加します。 

**FlashCardFragment.cs**を開き、ビューが呼び出し元に返される直前`OnCreateView`に、メソッドの末尾に次のコードを追加します。 

```csharp
questionBox.Click += delegate
{
    Toast.MakeText(Activity.ApplicationContext,
            "Answer: " + answer, ToastLength.Short).Show();
};
```

この`Click`イベントハンドラーは、ユーザーがを`TextBox`タップしたときに表示されるトーストに応答を表示します。 変数`answer`は、に`OnCreateView`渡されたバンドルから状態情報が読み取られたときに、前に初期化されました。 アプリをビルドして実行し、各フラッシュカードの問題のテキストをタップして、回答を確認します。 

[![数値演算の問題がタップされたときの FlashCardPager app Toasts のスクリーンショット](viewpager-and-fragments-images/05-answer-sml.png)](viewpager-and-fragments-images/05-answer.png#lightbox)

このチュートリアルで示されている FlashCardPager `MainActivity`は、 `FragmentActivity`から派生したを使用`MainActivity`し`AppCompatActivity`ますが、から派生することもできます (これは、フラグメントの管理もサポートします)。 `AppCompatActivity` 例を確認するには、サンプルギャラリーの「[FlashCardPager](https://docs.microsoft.com/samples/xamarin/monodroid-samples/userinterface-flashcardpager)」を参照してください。



## <a name="summary"></a>まとめ

このチュートリアルでは、を使用して`ViewPager` `Fragment`基本的なベースのアプリを構築する方法の手順について説明しました。 この例では、 `ViewPager` `ViewPager`フラッシュカードの質問と回答、フラッシュカード`FragmentPagerAdapter`を表示するレイアウト、をデータソースに接続するサブクラスを含むデータソースの例を紹介しています。 Flash カード内を移動するために、を`PagerTabStrip`追加して各ページの上部に問題番号を表示する方法を説明する手順が含まれていました。 最後に、ユーザーがフラッシュカードの問題をタップしたときに回答を表示するために、イベント処理コードが追加されました。 



## <a name="related-links"></a>関連リンク

- [FlashCardPager (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/userinterface-flashcardpager)
