---
title: ViewPager とフラグメント
description: ViewPager は、gestural ナビゲーションを実装できるレイアウトマネージャーです。 Gestural ナビゲーションを使用すると、ユーザーは左右にスワイプしてデータのページをステップ実行できます。 このガイドでは、データページとしてフラグメントを使用して ViewPager で swipeable UI を実装する方法について説明します。
ms.prod: xamarin
ms.assetid: 62B6286F-3680-48F3-B91B-453692E457E5
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: 42035abb6b2cebc862d9c1ad0027d11dd6f47e44
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91457303"
---
# <a name="viewpager-with-fragments"></a>ViewPager とフラグメント

_ViewPager は、gestural ナビゲーションを実装できるレイアウトマネージャーです。Gestural ナビゲーションを使用すると、ユーザーは左右にスワイプしてデータのページをステップ実行できます。このガイドでは、データページとしてフラグメントを使用して ViewPager で swipeable UI を実装する方法について説明します。_

## <a name="overview"></a>概要

`ViewPager` は、の各ページのライフサイクルを簡単に管理できるように、フラグメントと組み合わせて使用されることがよくあり `ViewPager` ます。 このチュートリアルでは、を `ViewPager` 使用して、 **FlashCardPager** という名前のアプリを作成し、フラッシュカードに一連の計算問題を提示します。 各フラッシュカードは、フラグメントとして実装されます。 ユーザーは、フラッシュカードを左右にスワイプし、数値演算の問題をタップしてその答えを明らかにします。 このアプリは、 `Fragment` 各フラッシュカードのインスタンスを作成し、から派生したアダプターを実装し `FragmentPagerAdapter` ます。 [Viewpager とビュー](~/android/user-interface/controls/view-pager/viewpager-and-views.md)では、ほとんどの作業はライフサイクルメソッドで実行されていました `MainActivity` 。 **FlashCardPager**では、ほとんどの作業は `Fragment` ライフサイクルメソッドの1つでによって行われます。 

このガイドでは、 &ndash; Xamarin Android のフラグメントについてまだよく知らない場合のフラグメントの基本につい[Fragments](~/android/platform/fragments/index.md)ては説明しません。フラグメントの概要については、「フラグメント」を参照してください。 

## <a name="start-an-app-project"></a>アプリプロジェクトを開始する

**FlashCardPager**という名前の新しい Android プロジェクトを作成します。 次に、NuGet パッケージマネージャーを起動します (NuGet パッケージのインストールの詳細については、「 [チュートリアル: プロジェクトに nuget を含める](/visualstudio/mac/nuget-walkthrough)」を参照してください)。 「 [Viewpager とビュー](~/android/user-interface/controls/view-pager/viewpager-and-views.md)」で説明されているように、 **Xamarin. Android. サポート**パックを見つけてインストールします。 

## <a name="add-an-example-data-source"></a>サンプルデータソースを追加する

**FlashCardPager**では、データソースはクラスによって表されるフラッシュカードのデッキです `FlashCardDeck` 。このデータソースは、 `ViewPager` に項目の内容を提供します。 `FlashCardDeck` 数値演算の問題と回答の既製のコレクションが含まれています。 コンストラクターには `FlashCardDeck` 引数は必要ありません。 

```csharp
FlashCardDeck flashCards = new FlashCardDeck();
```

のフラッシュカードのコレクション `FlashCardDeck` は、各フラッシュカードがインデクサーによってアクセスできるように編成されています。 たとえば、次のコード行は、デッキの4番目のフラッシュカードの問題を取得します。 

```csharp
string problem = flashCardDeck[3].Problem;
```

次のコード行は、前の問題への対応する回答を取得します。

```csharp
string answer = flashCardDeck[3].Answer;
```

の実装の詳細は `FlashCardDeck` 理解には関係がないため `ViewPager` 、 `FlashCardDeck` コードはここには記載されていません。
のソースコード `FlashCardDeck` は、 [FlashCardDeck.cs](https://github.com/xamarin/monodroid-samples/blob/master/UserInterface/FlashCardPager/FlashCardPager/FlashCardDeck.cs)で入手できます。
このソースファイルをダウンロードして (またはコードをコピーして新しい **FlashCardDeck.cs** ファイルに貼り付け)、プロジェクトに追加します。

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

この XML は、 `ViewPager` 画面全体を占有するを定義します。 はサポートライブラリにパッケージされているので、完全修飾名の "android... **ViewPager** " を使用する必要があることに注意してください。 `ViewPager` `ViewPager` は、 [Android サポートライブラリ v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)からのみ使用できます。Android SDK では使用できません。

## <a name="set-up-viewpager"></a>ViewPager の設定

**MainActivity.cs**を編集し、次のステートメントを追加し `using` ます。

```csharp
using Android.Support.V4.View;
using Android.Support.V4.App;
```

クラス宣言を変更して、 `MainActivity` から派生するようにし `FragmentActivity` ます。

```csharp
public class MainActivity : FragmentActivity
```

`MainActivity` は `FragmentActivity` `Activity` 、 `FragmentActivity` フラグメントのサポートを管理する方法を知っているので、(ではなく) から派生します。 `OnCreate` メソッドを次のコードに置き換えます。 

```csharp
protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);
    SetContentView(Resource.Layout.Main);
    ViewPager viewPager = FindViewById<ViewPager>(Resource.Id.viewpager);
    FlashCardDeck flashCards = new FlashCardDeck();
}
```

このコードは、次の処理を実行します。

1. **メインの axml**レイアウトリソースからビューを設定します。

2. レイアウトからへの参照を取得 `ViewPager` します。

3. 新しいを `FlashCardDeck` データソースとしてインスタンス化します。

このコードをビルドして実行すると、次のスクリーンショットのような画面が表示されます。 

[![空の ViewPager を使用した FlashCardPager アプリのスクリーンショット](viewpager-and-fragments-images/01-initial-screen-sml.png)](viewpager-and-fragments-images/01-initial-screen.png#lightbox)

この時点では、にデータを設定するために使用されるフラグメントが不足しているため、は空になり `ViewPager` `ViewPager` ます。また、 **FlashCardDeck**のデータからこれらのフラグメントを作成するためのアダプターが不足しています。 

次のセクションでは、 `FlashCardFragment` が作成され、各フラッシュカードの機能が実装されます。また、は、の `FragmentPagerAdapter` `ViewPager` データから作成されたフラグメントに接続するために作成され `FlashCardDeck` ます。 

## <a name="create-the-fragment"></a>フラグメントを作成する

各フラッシュカードは、という UI フラグメントによって管理され `FlashCardFragment` ます。 `FlashCardFragment`のビューには、1つのフラッシュカードに含まれる情報が表示されます。 の各インスタンス `FlashCardFragment` は、によってホストされ `ViewPager` ます。 
`FlashCardFragment`のビューは、フラッシュカードの問題のテキストを表示するで構成され `TextView` ます。 このビューは、 `Toast` ユーザーが flash カードの質問をタップしたときにを使用して回答を表示するイベントハンドラーを実装します。 

### <a name="create-the-flashcardfragment-layout"></a>FlashCardFragment レイアウトを作成する

を実装する前に `FlashCardFragment` 、そのレイアウトを定義する必要があります。 このレイアウトは、1つのフラグメントのフラグメントコンテナーのレイアウトです。 Flashcard_layout という名前の **リソース/レイアウト** に新しい Android レイアウトを追加し **ます**。 **Resources/layout/flashcard_layout**を開き、内容を次のコードに置き換えます。 

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

このレイアウトでは、1つのフラッシュカードフラグメントを定義します。各フラグメントは、 `TextView` large (100sp) フォントを使用して数値演算の問題を表示するで構成されます。 このテキストは、フラッシュカードの垂直方向および水平方向に中央揃えで配置されます。 

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

このコードは、 `Fragment` フラッシュカードを表示するために使用される基本的な定義をスタブにします。 `FlashCardFragment`は、で定義されているのサポートライブラリバージョンから派生 `Fragment` `Android.Support.V4.App.Fragment` します。 コンストラクターは空であるため、 `newInstance` ファクトリメソッドを使用してコンストラクターの代わりに新しいを作成 `FlashCardFragment` できます。 

`OnCreateView`ライフサイクルメソッドは、を作成して構成し `TextView` ます。 この例では、フラグメントののレイアウトを増え、 `TextView` 呼び出し元に大きくなったを返し `TextView` ます。 `LayoutInflater` と `ViewGroup` は、レイアウトを膨張させるためにに渡され `OnCreateView` ます。 バンドルには `savedInstanceState` 、が `OnCreateView` 保存された状態からを再作成するために使用するデータが含まれてい `TextView` ます。 

フラグメントのビューは、の呼び出しによって明示的に拡大され `inflater.Inflate` ます。 `container`引数はビューの親であり、フラグは inflater に対して、 `false` ビューの親に対して拡大されたビューの追加を防止するように指示します (この `ViewPager` チュートリアルの後半でアダプターのメソッドを呼び出したときに追加され `GetItem` ます)。 

### <a name="add-state-code-to-flashcardfragment"></a>FlashCardFragment に状態コードを追加する

アクティビティと同様に、フラグメントには、その `Bundle` 状態を保存および取得するために使用されるがあります。 **FlashCardPager**では、これ `Bundle` は、関連付けられているフラッシュカードの質問と回答のテキストを保存するために使用されます。 **FlashCardFragment.cs**で、 `Bundle` クラス定義の先頭に次のキーを追加し `FlashCardFragment` ます。 

```csharp
private static string FLASH_CARD_QUESTION = "card_question";
private static string FLASH_CARD_ANSWER = "card_answer";
```

`newInstance`オブジェクトを作成し、上記のキーを `Bundle` 使用して、インスタンス化された後に、渡された質問と回答のテキストをフラグメントに格納するように、ファクトリメソッドを変更します。 

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

Fragment ライフサイクルメソッドを変更して `OnCreateView` 、渡されたバンドルからこの情報を取得し、に質問テキストを読み込み `TextBox` ます。 

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

`answer`ここでは変数は使用されませんが、後でイベントハンドラーコードをこのファイルに追加するときに使用されます。 

## <a name="create-the-adapter"></a>アダプターを作成する

`ViewPager` は、データソースとデータソースの間にあるアダプターコントローラーオブジェクトを使用し `ViewPager` ます (ViewPager [アダプター](~/android/user-interface/controls/view-pager/index.md#adapter) の記事の図を参照してください)。 このデータにアクセスするには、 `ViewPager` から派生したカスタムアダプターを指定する必要があり `PagerAdapter` ます。 この例ではフラグメントを使用しているため、 `FragmentPagerAdapter` &ndash; `FragmentPagerAdapter` はから派生したを使用 `PagerAdapter` します。 
`FragmentPagerAdapter``Fragment`ユーザーがページに戻ることができる限り、フラグメントマネージャーに永続的に保持されるとして各ページを表します。 ユーザーがのページをスワイプとすると、 `ViewPager` は `FragmentPagerAdapter` データソースから情報を抽出し、それを使用してを作成し、を `Fragment` 表示し `ViewPager` ます。 

を実装する場合は、 `FragmentPagerAdapter` 次のものをオーバーライドする必要があります。

- **カウント** &ndash; 使用可能なビュー (ページ) の数を返す読み取り専用のプロパティ。

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

このコードは、重要な実装をスタブに `FragmentPagerAdapter` します。 次のセクションでは、これらの各メソッドが作業コードに置き換えられています。 コンストラクターの目的は、フラグメントマネージャーを `FlashCardDeckAdapter` の基底クラスコンストラクターに渡すことです。 

### <a name="implement-the-adapter-constructor"></a>アダプターコンストラクターを実装する

アプリによってがインスタンス化されると `FlashCardDeckAdapter` 、フラグメントマネージャーとインスタンス化されたへの参照が提供さ `FlashCardDeck` れます。 FlashCardDeckAdapter.cs のクラスの先頭に、次のメンバー変数を追加し `FlashCardDeckAdapter` ます。 **FlashCardDeckAdapter.cs** 

```csharp
public FlashCardDeck flashCardDeck;
```

コンストラクターに次のコード行を追加し `FlashCardDeckAdapter` ます。 

```csharp
this.flashCardDeck = flashCards;
```

このコード行は、が `FlashCardDeck` 使用するインスタンスを格納 `FlashCardDeckAdapter` します。 

### <a name="implement-count"></a>実装数

`Count`実装は比較的単純で、フラッシュカードデッキのフラッシュカードの数を返します。 `Count` を次のコードに置き換えます。 

```csharp
public override int Count
{
    get { return flashCardDeck.NumCards; }
}
```

`NumCards`のプロパティは、 `FlashCardDeck` データセット内のフラッシュカード (フラグメントの数) の数を返します。 

### <a name="implement-getitem"></a>GetItem を実装する

メソッドは、 `GetItem` 指定された位置に関連付けられているフラグメントを返します。 `GetItem`フラッシュカードデッキ内の位置に対してが呼び出されると、 `FlashCardFragment` その位置でフラッシュカードの問題を表示するように構成されたを返します。 `GetItem` メソッドを次のコードに置き換えます。 

```csharp
public override Android.Support.V4.App.Fragment GetItem(int position)
{
    return (Android.Support.V4.App.Fragment)
        FlashCardFragment.newInstance (
            flashCardDeck[position].Problem, flashCardDeck[position].Answer);
}
```

このコードは、次の処理を実行します。

1. `FlashCardDeck`指定された位置について、デッキ内の数値演算の問題文字列を検索します。 

2. `FlashCardDeck`指定された位置について、デッキ内の応答文字列を検索します。 

3. `FlashCardFragment`ファクトリメソッドを呼び出し `newInstance` 、フラッシュカードの問題と応答文字列を渡します。 

4. `Fragment`その位置の質問と回答のテキストを含む新しいフラッシュカードを作成して返します。 

が `ViewPager` でレンダリングされると、 `Fragment` `position` `TextBox` フラッシュカードデッキのにある数学問題文字列を含むが表示され `position` ます。 

## <a name="add-the-adapter-to-the-viewpager"></a>ViewPager にアダプターを追加する

が実装されたので、これ `FlashCardDeckAdapter` をに追加し `ViewPager` ます。 **MainActivity.cs**で、メソッドの末尾に次のコード行を追加し `OnCreate` ます。

```csharp
FlashCardDeckAdapter adapter =
    new FlashCardDeckAdapter(SupportFragmentManager, flashCards);
viewPager.Adapter = adapter;
```

このコードは、をインスタンス化し `FlashCardDeckAdapter` 、 `SupportFragmentManager` 1 番目の引数のを渡します。 (の詳細については、 `SupportFragmentManager` `FragmentManager` &ndash; `FragmentManager` 「 [フラグメントの管理](~/android/platform/fragments/managing-fragments.md)」を参照してください)。 

コア実装は、 &ndash; アプリのビルドと実行が完了しました。
次のスクリーンショットの左側に示すように、フラッシュカードデッキの最初の画像が画面に表示されます。 左にスワイプしてさらにフラッシュカードを確認し、次にスワイプして、フラッシュカードデッキに戻ります。

[![ページャーインジケーターのない FlashCardPager アプリのスクリーンショットの例](viewpager-and-fragments-images/02-example-views-sml.png)](viewpager-and-fragments-images/02-example-views.png#lightbox)

## <a name="add-a-pager-indicator"></a>ページャーインジケーターの追加

この最小実装では、 `ViewPager` デッキの各フラッシュカードが表示されますが、ユーザーがデッキ内にある場所については示されません。 次の手順では、を追加し `PagerTabStrip` ます。 は、 `PagerTabStrip` どの問題番号が表示されているかをユーザーに通知し、前のフラッシュカードと次のフラッシュカードのヒントを表示することによって、ナビゲーションコンテキストを提供します。 

**Resources/layout/Main**を開き、を `PagerTabStrip` レイアウトに追加します。

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

アプリをビルドして実行すると、 `PagerTabStrip` 各フラッシュカードの上部に空のが表示されます。 

[![テキストを含まない PagerTabStrip のクローズアップ](viewpager-and-fragments-images/03-empty-pagetabstrip-sml.png)](viewpager-and-fragments-images/03-empty-pagetabstrip.png#lightbox)

### <a name="display-a-title"></a>タイトルを表示する

各ページタブにタイトルを追加するには、 `GetPageTitleFormatted` アダプターにメソッドを実装します。 `ViewPager``GetPageTitleFormatted`(実装されている場合) を呼び出して、指定した位置にあるページを説明するタイトル文字列を取得します。 FlashCardDeckAdapter.cs のクラスに次のメソッドを追加し `FlashCardDeckAdapter` ます。 **FlashCardDeckAdapter.cs** 

```csharp
public override Java.Lang.ICharSequence GetPageTitleFormatted(int position)
{
    return new Java.Lang.String("Problem " + (position + 1));
}
```

このコードは、flash カードデッキの位置を問題番号に変換します。 結果の文字列は `String` 、に返される Java に変換され `ViewPager` ます。 この新しい方法でアプリを実行すると、各ページに問題の番号がに表示され `PagerTabStrip` ます。 

[![各ページの上に表示されている問題番号を含む FlashCardPager のスクリーンショット](viewpager-and-fragments-images/04-pagetabstrip-sml.png)](viewpager-and-fragments-images/04-pagetabstrip.png#lightbox)

各フラッシュカードの上部に表示される flash カードデッキの問題番号は、前後にスワイプして確認できます。 

## <a name="handle-user-input"></a>ユーザー入力を処理する

**FlashCardPager** では、に一連のフラグメントベースのフラッシュカードが示さ `ViewPager` れていますが、各問題の答えを明らかにする方法はまだありません。 このセクションでは、 `FlashCardFragment` ユーザーがフラッシュカードの問題のテキストをタップしたときに回答を表示するために、イベントハンドラーをに追加します。 

**FlashCardFragment.cs**を開き、 `OnCreateView` ビューが呼び出し元に返される直前に、メソッドの末尾に次のコードを追加します。 

```csharp
questionBox.Click += delegate
{
    Toast.MakeText(Activity.ApplicationContext,
            "Answer: " + answer, ToastLength.Short).Show();
};
```

この `Click` イベントハンドラーは、ユーザーがをタップしたときに表示されるトーストに応答を表示し `TextBox` ます。 変数は、 `answer` に渡されたバンドルから状態情報が読み取られたときに、前に初期化されました `OnCreateView` 。 アプリをビルドして実行し、各フラッシュカードの問題のテキストをタップして、回答を確認します。 

[![数値演算の問題がタップされたときの FlashCardPager app Toasts のスクリーンショット](viewpager-and-fragments-images/05-answer-sml.png)](viewpager-and-fragments-images/05-answer.png#lightbox)

このチュートリアルで示されている **FlashCardPager** は、から派生したを使用し `MainActivity` ますが、から派生する `FragmentActivity` こともでき `MainActivity` `AppCompatActivity` ます (これは、フラグメントの管理もサポートします)。 例を確認するには `AppCompatActivity` 、サンプルギャラリーの「 [FlashCardPager](/samples/xamarin/monodroid-samples/userinterface-flashcardpager) 」を参照してください。

## <a name="summary"></a>まとめ

このチュートリアルでは、 `ViewPager` を使用して基本的なベースのアプリを構築する方法の手順について説明しました `Fragment` 。 この例では、フラッシュカードの質問と回答、 `ViewPager` フラッシュカードを表示するレイアウト、を `FragmentPagerAdapter` データソースに接続するサブクラスを含むデータソースの例を紹介して `ViewPager` います。 Flash カード内を移動するために、を追加して `PagerTabStrip` 各ページの上部に問題番号を表示する方法を説明する手順が含まれていました。 最後に、ユーザーがフラッシュカードの問題をタップしたときに回答を表示するために、イベント処理コードが追加されました。 

## <a name="related-links"></a>関連リンク

- [FlashCardPager (サンプル)](/samples/xamarin/monodroid-samples/userinterface-flashcardpager)