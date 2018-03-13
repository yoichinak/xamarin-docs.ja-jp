---
title: "フラグメントの作成"
ms.topic: article
ms.prod: xamarin
ms.assetid: F2997242-BC29-1440-7F1A-CFC447CD73FA
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/07/2018
ms.openlocfilehash: 415c3a5e9446c5db545b62272f3b90a9ac73e401
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2018
---
# <a name="creating-a-fragment"></a>フラグメントの作成

フラグメントを作成するにはクラスから継承する必要があります`Android.App.Fragment`をオーバーライドし、`OnCreateView`メソッドです。 `OnCreateView` 返されますが、画面にフラグメントを配置するときにホストのアクティビティによって呼び出されますが、`View`です。 一般的な`OnCreateView`これに作成`View`レイアウト ファイルを拡大して、親コンテナーにアタッチします。 コンテナーの特性は、重要なは、Android がフラグメントの UI に親のレイアウトのパラメーターを適用されます。 次に例を示します。

```csharp
public override View OnCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
{
    return inflater.Inflate(Resource.Layout.Example_Fragment, container, false);
}
```

上記のコードは、ビューを拡大`Resource.Layout.Example_Fragment`、として子ビューを追加し、`ViewGroup`コンテナーです。


> [!NOTE]
> フラグメントのサブクラスでは、コンス トラクターの引数がない、パブリックの既定値が必要です。

## <a name="adding-a-fragment-to-an-activity"></a>アクティビティにフラグメントを追加します。

2 つの方法がフラグメントをホストすることも、アクティビティ内で。

-   **宣言的**&ndash;フラグメントを宣言内で使用できる`.axml`レイアウト ファイルを使用して、`<Fragment>`タグ。

-   **プログラムによって**&ndash;を使用して動的にインスタンス化もフラグメントもできる、`FragmentManager`クラスの API です。

経由でプログラムによる使用状況、`FragmentManager`クラスは、このガイドで後で説明します。

### <a name="using-a-fragment-declaratively"></a>フラグメントを宣言して使用します。

レイアウト内のフラグメントを追加する場合を使用する必要があります、`<fragment>`タグとし、いずれかの操作を提供することによって、フラグメントを識別、`class`属性または`android:name`属性。 次のスニペットを使用する方法を示しています、`class`を宣言する属性、 `fragment`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<fragment class="com.xamarin.sample.fragments.TitlesFragment"
            android:id="@+id/titles_fragment"
            android:layout_width="fill_parent"
            android:layout_height="fill_parent" />
```

この次のスニペットは、宣言する方法を示しています、`fragment`を使用して、`android:name`フラグメント クラスを識別する属性。

```xml
<?xml version="1.0" encoding="utf-8"?>
<fragment android:name="com.xamarin.sample.fragments.TitlesFragment"
            android:id="@+id/titles_fragment"
            android:layout_width="fill_parent"
            android:layout_height="fill_parent" />
```

アクティビティが作成されているときに、Android はレイアウト ファイルで指定された各フラグメントをインスタンス化しから作成されるビューを挿入`OnCreateView`の代わりに、`Fragment`要素。
アクティビティを宣言によって追加されるフラグメントが静的し、が破棄されるまでは、アクティビティに残ります動的に置き換えたりが接続されているアクティビティの有効期間中にこのようなフラグメントを削除することはできません。

各フラグメントには、一意の識別子を割り当てる必要があります。

-  **android: id** &ndash;と同様、レイアウト ファイル内の他の UI 要素を導入すると、この一意の id。

-  **android: タグ**&ndash;この属性は、一意の文字列。

前の 2 つのメソッドのどちらを使用する場合、フラグメントはコンテナーのビューの ID と見なされます。 次の例では、どちらも`android:id`も`android:tag`は、提供、Android、ID が割り当てられます`fragment_container`フラグメントに。

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
                android:id="+@id/fragment_container"
                android:orientation="horizontal"
                android:layout_width="match_parent"
                android:layout_height="match_parent">

        <fragment class="com.example.android.apis.app.TitlesFragment"
                android:layout_width="match_parent"
                android:layout_height="match_parent" />
</LinearLayout>
```

### <a name="package-name-case"></a>パッケージ名の大文字/

Android でのパッケージ名の大文字はできません。例外は、パッケージ名には、大文字の文字が含まれている場合、ビューを展開しようとするときとスローされます。 ただし、Xamarin.Android より安全であり、許容名前空間に大文字を使用します。

たとえば、次のスニペットの両方の連携 Xamarin.Android させます。 ただし、2 番目のスニペットがなります、`android.view.InflateException`ピュア Java ベースの Android アプリケーションでスローされます。

```xml
<fragment class="com.example.DetailsFragment" android:id="@+id/fragment_content" android:layout_width="match_parent" android:layout_height="match_parent" />
```

OR

```xml
<fragment class="Com.Example.DetailsFragment" android:id="@+id/fragment_content" android:layout_width="match_parent" android:layout_height="match_parent" />
```


## <a name="fragment-lifecycle"></a>フラグメントのライフ サイクル

フラグメントは、多少の独立したが、によっても影響を受ける、独自のライフ サイクルがある、[ホスティングのアクティビティのライフ サイクル](~/android/app-fundamentals/activity-lifecycle/index.md)です。
たとえば、アクティビティが一時停止、その関連付けられているフラグメントのすべてが一時停止します。 次の図では、フラグメントのライフ サイクルについて説明します。

[![フラグメントのライフ サイクルを示すフロー ダイアグラム](creating-a-fragment-images/fragment-lifecycle.png)](creating-a-fragment-images/fragment-lifecycle.png#lightbox)


### <a name="fragment-creation-lifecycle-methods"></a>フラグメントの作成のライフ サイクル メソッド

以下の一覧が作成されるよう、フラグメントのライフ サイクルのさまざまなコールバックのフローが示されます。

-   **`OnInflate()`** &ndash; ビューのレイアウトの一部として、フラグメントが作成されているときに呼び出されます。 これは、レイアウトの XML ファイルから、フラグメントを宣言によって作成された直後に呼び出すことができます。 フラグメントがそのアクティビティに関連付けられていない、まだですが、**アクティビティ**、**バンドル**、および**です**ビューから階層はでパラメーターとして渡されました。 このメソッドが解析に使用される最も、**です**for 属性を保存する可能性がありますを使用する後で、フラグメントによって、します。

-   **`OnAttach()`** &ndash; フラグメントは、アクティビティに関連付けられた後に呼び出されます。 これは、フラグメントが使用する準備ができたときに実行する最初のメソッドです。 一般に、フラグメントがコンス トラクターを実装できません、または既定のコンス トラクターをオーバーライドする必要があります。 フラグメントの必要なすべてのコンポーネントは、このメソッドで初期化する必要があります。

-   **`OnCreate()`** &ndash; フラグメントを作成するアクティビティによって呼び出されます。 このメソッドが呼び出されると、ホスティングのアクティビティの階層の表示可能性があります完全にインスタンス化できません、フラグメントは、アクティビティのフラグメントのライフ サイクルの後半で階層の表示の任意の部分に依存しないようにようにします。 たとえば、任意の調整や、アプリケーションの UI を調整を実行するのにこのメソッドは使用しないでください。 これは、フラグメントは、可能性がありますを開始する必要があるデータを収集する最も早い時刻です。 フラグメントが UI スレッドでこの時点で実行されている、そのため、時間のかかる処理を回避するまたはバック グラウンド スレッドでその処理を実行します。 場合、このメソッドをスキップすることがあります**SetRetainInstance(true)**と呼びます。
    この代替手順は、詳細については、以下について説明します。

-   **`OnCreateView()`** &ndash; フラグメントのビューを作成します。
    このメソッドは 1 回、アクティビティの**OnCreate()**メソッドが完了しました。 この時点では、アクティビティの階層の表示と対話する安全でです。 このメソッドは、フラグメントによって使用されるビューを返す必要があります。

-   **`OnActivityCreated()`** &ndash; 後に呼び出される**Activity.OnCreate**ホスティング アクティビティが完了します。
    この時点では、ユーザー インターフェイスに最終的な調整を実行してください。

-   **`OnStart()`** &ndash; 親アクティビティが再開された後に呼び出されます。 これにより、フラグメントのユーザーに表示します。 多くの場合、フラグメントが内にあるコードを含む、 **OnStart()**アクティビティのメソッドです。

-   **`OnResume()`** &ndash; これは、最後のフラグメントと、ユーザーが対話する前に呼び出すメソッドです。 このメソッドで実行されるコードの種類の例に、ユーザーが対話、カメラなど、デバイスの機能が有効にすること、ロケーション サービス。 これらなどのサービスには、過剰なバッテリ ドレイン可能性がありますただし、およびアプリケーションには、バッテリの寿命を保持するために、使用が最小限に抑える必要があります。


### <a name="fragment-destruction-lifecycle-methods"></a>フラグメントの破棄のライフ サイクル メソッド

[次へ] の一覧には、フラグメントが破棄されると呼び出されるライフ サイクル メソッドについて説明します。

-   **`OnPause()`** &ndash; ユーザーは、フラグメントと対話することはありません。 その他の操作のフラグメントには、このフラグメントを変更するか、ホストのアクティビティが一時停止しているために、このような状況が存在します。 このフラグメントをホストしているアクティビティが表示されます、つまり、フォーカスのあるアクティビティは部分的に透過的または全画面表示を占有しません。 このメソッドがアクティブになったときに、ユーザーが、フラグメントのままで、最初に表示します。 フラグメントは、変更を保存する必要があります。

-   **`OnStop()`** &ndash; フラグメントは表示されません。 ホストのアクティビティを停止する可能性があります、またはフラグメント操作を使用すると、アクティビティで変更ができます。 このコールバック機能として同じ目的**Activity.OnStop**です。

-   **`OnDestroyView()`** &ndash; このメソッドは、ビューに関連付けられているリソースをクリーンアップします。 これは、フラグメントに関連付けられているビューが破棄されたときに呼び出されます。

-   **`OnDestroy()`** &ndash; このメソッドは、フラグメントが使用ではなくなったときに呼び出されます。 活動にまだ関連付けられているが、フラグメントは機能しません。 このメソッドはなどで、フラグメントによって使用されているリソースを解放する必要があります、 [ **SurfaceView** ](https://developer.xamarin.com/api/type/Android.Views.SurfaceView/)を使用して、カメラの可能性があります。 場合、このメソッドをスキップすることがあります**SetRetainInstance(true)**と呼びます。 この代替手順は、詳細については、以下について説明します。

-   **`OnDetach()`** &ndash; このメソッドは、フラグメントは、アクティビティに関連付けられてが不要になった直前に呼び出されます。 フラグメントの階層の表示が不要になったが存在し、この時点で、フラグメントによって使用されているすべてのリソースを解放する必要があります。


### <a name="using-setretaininstance"></a>SetRetainInstance を使用します。

フラグメントことに完全に破棄させない活動が再作成される場合を指定することができます。 `Fragment`クラス、メソッドを提供`SetRetainInstance`この目的のためです。 場合`true`は、フラグメントの同じインスタンスを使用するアクティビティを再起動したときにし、このメソッドに渡されます。 このような場合を除くすべてのコールバック メソッドが呼び出される、`OnCreate`と`OnDestroy`ライフ サイクルのコールバック。 このプロセスは、上記 (点線で示されて、緑) ライフ サイクルの図に示します。


## <a name="fragment-state-management"></a>フラグメントの状態の管理

月をフラグメント化状態保存およびリストア、フラグメントのライフ サイクル中のインスタンスを使用して、`Bundle`です。 バンドルは、により、キーと値のペアとしてデータを保存するフラグメントと、多くのメモリを必要としない単純なデータに便利です。 フラグメントへの呼び出しでその状態を保存できます`OnSaveInstanceState`:

```csharp
public override void OnSaveInstanceState(Bundle outState)
{
    base.OnSaveInstanceState(outState);
    outState.PutInt("current_choice", _currentCheckPosition);
}
```

フラグメントの新しいインスタンスが作成された日時に保存された状態、`Bundle`経由での新しいインスタンスが利用できる、 `OnCreate`、 `OnCreateView`、および`OnActivityCreated`新しいインスタンスのメソッドです。
次の例は、値を取得する方法を示します`current_choice`から、 `Bundle`:

```csharp
public override void OnActivityCreated(Bundle savedInstanceState)
{
    base.OnActivityCreated(savedInstanceState);
    if (savedInstanceState != null)
    {
        _currentCheckPosition = savedInstanceState.GetInt("current_choice", 0);
    }
}
```

オーバーライドする`OnSaveInstanceState`などの保存の一時的なデータ フラグメント内向きの変更の間での適切なメカニズムは、`current_choice`上記の例の値。 ただし、既定の実装`OnSaveInstanceState`の ui を割り当てられた ID を持つすべてのビューに一時的なデータの保存を行います。 たとえばを持つアプリケーションを見て、 `EditText` XML で次のように定義された要素。

```xml
<EditText android:id="@+id/myText"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"/>
```

以降、`EditText`コントロールに、`id`割り当てられると、フラグメントは、自動的にデータを保存、ウィジェットで、ときに`OnSaveInstanceState`と呼びます。


### <a name="bundle-limitations"></a>バンドルの制限事項

使用する`OnSaveInstanceState`いくつかの制限が簡単にこのメソッドの使用を一時的なデータを保存するには。

-  フラグメントが、バック スタックに追加されていないかどうかは、ユーザーが押したときの状態は復元されません、**戻る**ボタンをクリックします。

-  バンドルを使用するデータを保存すると、そのデータがシリアル化されます。 これは、処理の遅延する可能性があります。


## <a name="contributing-to-the-menu"></a>メニューに貢献しています。

フラグメントは、そのホストのアクティビティのメニューに項目を投稿可能性があります。
アクティビティは、最初のメニュー項目を処理します。 アクティビティには、ハンドラーがない、し、イベントが渡されます、フラグメントでは、それを処理しにします。

アクティビティのメニューに項目を追加するにはフラグメントは、次の 2 つを行う必要があります。
フラグメントがメソッドを実装する必要があります最初に、`OnCreateOptionsMenu`し、次のコードに示すように、メニューにそのアイテムを配置します。

```csharp
public override void OnCreateOptionsMenu(IMenu menu, MenuInflater menuInflater)
{
    menuInflater.Inflate(Resource.Menu.menu_fragment_vehicle_list, menu);
    base.OnCreateOptionsMenu(menu, menuInflater);
}
```

前のコード スニペットに、メニューがファイルで、次の XML から大きく`menu_fragment_vehicle_list.xml`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
  <item android:id="@+id/add_vehicle"
        android:icon="@drawable/ic_menu_add_data"
        android:title="@string/add_vehicle" />
</menu>
```

次に、フラグメントを呼び出す必要があります`SetHasOptionsMenu(true)`です。 このメソッドの呼び出しは、フラグメントが、[オプション] メニューに貢献するメニュー項目を持つ Android を発表しました。 このメソッドの呼び出しが行われた場合を除き、フラグメントのメニュー項目は、アクティビティのオプション メニューには追加されません。 ライフ サイクル メソッドでこれは、通常`OnCreate()`、次のコード スニペットに示すようにします。

```csharp
public override void OnCreate(Bundle savedState)
{
    base.OnCreate(savedState);
    SetHasOptionsMenu(true);
}
```

このメニューの外観は、次の画面を示しています。

[![メニュー項目を表示する個人用トリップ アプリのサンプルのスクリーン ショット](creating-a-fragment-images/fragment-menu-example.png)](creating-a-fragment-images/fragment-menu-example.png#lightbox)
