---
title: フラグメントの作成
ms.prod: xamarin
ms.assetid: F2997242-BC29-1440-7F1A-CFC447CD73FA
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/07/2018
ms.openlocfilehash: 339de4930242e35c40b034af2ce6ba47fe1543af
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50122466"
---
# <a name="creating-a-fragment"></a>フラグメントの作成

フラグメントを作成するにはクラスから継承する必要があります`Android.App.Fragment`をオーバーライドし、`OnCreateView`メソッド。 `OnCreateView` 画面で、フラグメントを格納する時間が返されますことと、ホスティングのアクティビティによって呼び出される、`View`します。 一般的な`OnCreateView`に作成されます`View`レイアウト ファイルを膨張させることと、親コンテナーにアタッチしています。 コンテナーの特性は、Android は、フラグメントの UI に、親のレイアウトのパラメーターを適用するので重要です。 次に例を示します。

```csharp
public override View OnCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
{
    return inflater.Inflate(Resource.Layout.Example_Fragment, container, false);
}
```

上記のコードは、ビューの膨張`Resource.Layout.Example_Fragment`、として子ビューを追加し、`ViewGroup`コンテナー。


> [!NOTE]
> フラグメントのサブクラスは、パブリックの既定値を引数のコンス トラクターがありませんが必要です。

## <a name="adding-a-fragment-to-an-activity"></a>フラグメントをアクティビティに追加します。

フラグメントをホストする可能性がある 2 つの方法は、アクティビティの内部。

-   **宣言によって**&ndash;フラグメントは、宣言内で使用できる`.axml`レイアウト ファイルを使用して、`<Fragment>`タグ。

-   **プログラムで**&ndash;フラグメントを使用して動的にインスタンス化もできます、`FragmentManager`クラスの API。

使用してプログラムによる使用状況、`FragmentManager`クラスは、このガイドの後半で説明します。

### <a name="using-a-fragment-declaratively"></a>宣言によって、フラグメントを使用します。

レイアウト内のフラグメントを追加する場合を使用する必要があります、`<fragment>`タグとし、フラグメントを識別するいずれかの操作を提供することで、`class`属性または`android:name`属性。 次のスニペットを使用する方法を示しています、`class`を宣言する属性、 `fragment`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<fragment class="com.xamarin.sample.fragments.TitlesFragment"
            android:id="@+id/titles_fragment"
            android:layout_width="fill_parent"
            android:layout_height="fill_parent" />
```

この次のスニペットは、宣言する方法を示します、`fragment`を使用して、`android:name`フラグメント クラスを識別する属性。

```xml
<?xml version="1.0" encoding="utf-8"?>
<fragment android:name="com.xamarin.sample.fragments.TitlesFragment"
            android:id="@+id/titles_fragment"
            android:layout_width="fill_parent"
            android:layout_height="fill_parent" />
```

アクティビティが作成されるときに、Android は、レイアウト ファイルで指定された各フラグメントをインスタンス化しから作成されるビューを挿入`OnCreateView`の代わりに、`Fragment`要素。
宣言的アクティビティに追加されるフラグメントは、静的およびが破棄されるまでは、アクティビティに残ります場合によっては、動的に交換またはがアタッチされているアクティビティの有効期間中にこのようなフラグメントを削除することはできません。

各フラグメントには、一意の識別子を割り当てる必要があります。

-  **android: id** &ndash;レイアウト ファイル内の他の UI 要素と、これは一意の id。

-  **android: タグ**&ndash;この属性は、一意の文字列。

前の 2 つのメソッドのどちらを使用する場合、フラグメントはコンテナー ビューの ID と想定されます。 次の例では、どちらも`android:id`も`android:tag`が指定されて、Android、ID が割り当てられます`fragment_container`フラグメントに。

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

### <a name="package-name-case"></a>パッケージ名の場合

Android がパッケージ名の大文字の文字を許可しません。パッケージ名に大文字の文字が含まれている場合、ビューを展開しようとするとき、例外がスローされます。 ただし、Xamarin.Android、寛容であり、名前空間内の大文字の文字が許容。

たとえば、Xamarin.Android で次のスニペットの両方は動作します。 ただし、2 番目のスニペットが生じる、`android.view.InflateException`純粋な Java ベースの Android アプリケーションによってスローされます。

```xml
<fragment class="com.example.DetailsFragment" android:id="@+id/fragment_content" android:layout_width="match_parent" android:layout_height="match_parent" />
```

OR

```xml
<fragment class="Com.Example.DetailsFragment" android:id="@+id/fragment_content" android:layout_width="match_parent" android:layout_height="match_parent" />
```


## <a name="fragment-lifecycle"></a>フラグメントのライフ サイクル

フラグメントは、あまり依存がまだ影響を受けることは、独自のライフ サイクルがある、[ホスティングのアクティビティのライフ サイクル](~/android/app-fundamentals/activity-lifecycle/index.md)します。
たとえば、アクティビティが一時停止したときに、関連付けられているフラグメントのすべてが一時停止します。 次の図では、フラグメントのライフ サイクルについて説明します。

[![フラグメントのライフ サイクルを示すフロー図](creating-a-fragment-images/fragment-lifecycle.png)](creating-a-fragment-images/fragment-lifecycle.png#lightbox)


### <a name="fragment-creation-lifecycle-methods"></a>フラグメントの作成のライフ サイクル メソッド

以下の一覧が作成されると、フラグメントのライフ サイクルのさまざまなコールバックのフローを示します。

-   **`OnInflate()`** &ndash; ビューのレイアウトの一部として、フラグメントが作成されるときに呼び出されます。 これは、レイアウトの XML ファイルからフラグメントを宣言によって作成された直後に呼び出されます場合があります。 フラグメントはそのアクティビティに関連付けられていない、まだが、**アクティビティ**、**バンドル**、および**です**ビューから階層はパラメーターとして渡すには。 解析するためにこのメソッドが適して、**です**および属性を保存する場合がありますで使用するフラグメント。

-   **`OnAttach()`** &ndash; フラグメントがアクティビティに関連付けられた後に呼び出されます。 これは、フラグメントが使用する準備ができたときに実行する最初のメソッドです。 一般に、フラグメントがコンス トラクターを実装しない、または既定のコンス トラクターをオーバーライドする必要があります。 フラグメントに必要なすべてのコンポーネントは、このメソッドで初期化する必要があります。

-   **`OnCreate()`** &ndash; フラグメントを作成するアクティビティによって呼び出されます。 このメソッドが呼び出されると、ホスティングのアクティビティの階層の表示可能性があります完全にインスタンス化できません、ため、フラグメントがフラグメントのライフ サイクルの後半で、アクティビティのビュー階層の任意の部分に依存する必要があります。 たとえば、メソッドを使用しないこの調整またはアプリケーションの ui の調整を実行します。 これは、開始位置として、フラグメント可能性があります、必要なデータを収集する最も早い時刻です。 フラグメントが UI スレッドでこの時点で実行されている、そのため、時間のかかる処理を回避またはバック グラウンド スレッドでその処理を実行します。 このメソッドがスキップされる場合**SetRetainInstance(true)** が呼び出されます。
    この方法については、以下で詳しく説明します。

-   **`OnCreateView()`** &ndash; フラグメントのビューを作成します。
    このメソッドは 1 回、アクティビティの**OnCreate()** メソッドが完了します。 この時点では、アクティビティの階層の表示と対話しても安全です。 このメソッドは、フラグメントによって使用されるビューを返す必要があります。

-   **`OnActivityCreated()`** &ndash; 後に呼び出される**Activity.OnCreate**ホスティングのアクティビティが完了します。
    この時点でのユーザー インターフェイスの最終的な調整を実行する必要があります。

-   **`OnStart()`** &ndash; 含まれるアクティビティが再開された後に呼び出されます。 これにより、フラグメントのユーザーに表示します。 多くの場合、フラグメントが内にあるコードを含む、 **OnStart()** アクティビティのメソッド。

-   **`OnResume()`** &ndash; これは、最後のフラグメントと、ユーザーが対話する前に呼び出すメソッドです。 このメソッドで実行されるコードの種類の例に、ユーザーが対話、カメラなどのデバイスの機能が有効にするを位置情報サービス。 など、これらのサービスが過剰なバッテリの消耗、可能性、およびアプリケーションには、バッテリの寿命を保持するために使用が最小限に抑える必要があります。


### <a name="fragment-destruction-lifecycle-methods"></a>フラグメントの破棄のライフ サイクル メソッド

[次へ] の一覧には、フラグメントが破棄されると呼び出されるライフ サイクル メソッドについて説明します。

-   **`OnPause()`** &ndash; ユーザーは、フラグメントと対話することはありません。 その他の操作のフラグメントが、このフラグメントを変更するか、ホスティングのアクティビティが一時停止しているために、このような状況が存在します。 このフラグメントをホストしているアクティビティが表示されること、つまり、フォーカスのあるアクティビティは部分的に透明または全画面表示を占有しないことができます。 このメソッドがアクティブになると、ユーザーが、フラグメントのままで、最初の兆候になります。 フラグメントは、変更を保存する必要があります。

-   **`OnStop()`** &ndash; フラグメントは、表示ではなくなりました。 アクティビティのホストを停止する可能性があります、またはフラグメントの操作を使用すると、アクティビティの変更はします。 このコールバック機能と同じ目的**Activity.OnStop**します。

-   **`OnDestroyView()`** &ndash; このメソッドは、ビューに関連付けられたリソースをクリーンアップします。 これは、フラグメントに関連付けられているビューが破棄されたときに呼び出されます。

-   **`OnDestroy()`** &ndash; このメソッドは、フラグメントが使用中でなくなったときに呼び出されます。 活動にまだ関連付けられているが、フラグメントは機能しません。 このメソッドはなど、フラグメントによって使用されているリソースを解放する必要があります、 [ **SurfaceView** ](https://developer.xamarin.com/api/type/Android.Views.SurfaceView/)を使用して、カメラの可能性があります。 このメソッドがスキップされる場合**SetRetainInstance(true)** が呼び出されます。 この方法については、以下で詳しく説明します。

-   **`OnDetach()`** &ndash; このメソッドは、フラグメントは、アクティビティに関連付けられて、不要になった直前に呼び出されます。 フラグメントのビュー階層が存在しないと、この時点で、フラグメントによって使用されているすべてのリソースを解放する必要があります。


### <a name="using-setretaininstance"></a>SetRetainInstance を使用します。

フラグメントこと、完全に破棄させない活動が再作成されている場合を指定することができます。 `Fragment`クラス、メソッドを提供`SetRetainInstance`この目的のためです。 場合`true`フラグメントの同じインスタンスを使用するアクティビティが再起動されると、このメソッドに渡されます。 このような場合を除くすべてのコールバック メソッドが呼び出される、`OnCreate`と`OnDestroy`ライフ サイクルのコールバック。 このプロセスは、(緑色の点線) で、前に示したライフ サイクルの図に示します。


## <a name="fragment-state-management"></a>フラグメントの状態管理

月のフラグメントを保存しのインスタンスを使用して、フラグメントのライフ サイクル中にその状態を復元、`Bundle`します。 バンドルにはキー/値ペアとしてデータを保存するフラグメントができは単純なデータ量のメモリを必要としないに便利です。 フラグメントはへの呼び出しでその状態を保存することができます`OnSaveInstanceState`:

```csharp
public override void OnSaveInstanceState(Bundle outState)
{
    base.OnSaveInstanceState(outState);
    outState.PutInt("current_choice", _currentCheckPosition);
}
```

フラグメントの新しいインスタンスが作成された日時に保存された状態、`Bundle`経由での新しいインスタンスが利用できる、 `OnCreate`、 `OnCreateView`、および`OnActivityCreated`メソッドの新しいインスタンス。
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

オーバーライドする`OnSaveInstanceState`向きの変更の間でフラグメント内の一時的なデータをよう保存するための適切なメカニズムは、`current_choice`の上記の例の値。 ただし、既定の実装の`OnSaveInstanceState`id が割り当てられたすべてのビューの UI に一時的なデータの保存を行います。 たとえば、あるアプリケーションを見て、 `EditText` XML で次のように定義された要素。

```xml
<EditText android:id="@+id/myText"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"/>
```

以降、`EditText`コントロールが、 `id` 、割り当てられているフラグメントは、自動的にデータを保存、ウィジェットでとき`OnSaveInstanceState`が呼び出されます。


### <a name="bundle-limitations"></a>バンドルの制限事項

使用しても`OnSaveInstanceState`によりいくつかの制限がこのメソッドを使用して一時的なデータを保存することが容易。

-  フラグメントが戻るスタックに追加されていないかどうかは、ユーザーが押したときの状態は復元されません、**戻る**ボタンをクリックします。

-  バンドルを使用すると、データを保存すると、そのデータがシリアル化されます。 処理の遅延が生じる。


## <a name="contributing-to-the-menu"></a>メニューに貢献します。

フラグメントは、ホスティング、アクティビティのメニューに項目を投稿可能性があります。
アクティビティは、最初のメニュー項目を処理します。 アクティビティは、ハンドラーを持たない場合、イベントに渡される、フラグメントは、それを処理し、します。

アクティビティのメニューに項目を追加するには、フラグメントは、2 つのことを行う必要があります。
フラグメントがメソッドを実装する必要があります最初に、`OnCreateOptionsMenu`し、次のコードに示すように、メニューにそのアイテムを配置します。

```csharp
public override void OnCreateOptionsMenu(IMenu menu, MenuInflater menuInflater)
{
    menuInflater.Inflate(Resource.Menu.menu_fragment_vehicle_list, menu);
    base.OnCreateOptionsMenu(menu, menuInflater);
}
```

次の XML ファイルから以前のコード スニペット メニューを拡大しても`menu_fragment_vehicle_list.xml`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
  <item android:id="@+id/add_vehicle"
        android:icon="@drawable/ic_menu_add_data"
        android:title="@string/add_vehicle" />
</menu>
```

次に、フラグメントを呼び出す必要があります`SetHasOptionsMenu(true)`します。 このメソッドの呼び出しは、フラグメントが、[オプション] メニューに貢献するメニュー項目を Android 発表します。 このメソッドの呼び出しが行われた場合を除き、フラグメントのメニュー項目は、アクティビティのオプション メニューには追加されません。 これは通常のライフ サイクル メソッドで行います`OnCreate()`の次のコード スニペットに示しますように。

```csharp
public override void OnCreate(Bundle savedState)
{
    base.OnCreate(savedState);
    SetHasOptionsMenu(true);
}
```

このメニューの外観は、次の画面を示しています。

[![メニュー項目を表示するマイ旅行アプリのスクリーン ショットの例](creating-a-fragment-images/fragment-menu-example.png)](creating-a-fragment-images/fragment-menu-example.png#lightbox)
