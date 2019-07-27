---
title: フラグメントの作成
ms.prod: xamarin
ms.assetid: F2997242-BC29-1440-7F1A-CFC447CD73FA
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/07/2018
ms.openlocfilehash: b20ce0dc76cbe663d35e7fab01d9a4ba943c0cd6
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510616"
---
# <a name="creating-a-fragment"></a>フラグメントの作成

フラグメントを作成するには、クラスがから`Android.App.Fragment`継承してから`OnCreateView` 、メソッドをオーバーライドする必要があります。 `OnCreateView`は、画面にフラグメントを配置する時間が経過すると、ホストアクティビティによって呼び出され、を`View`返します。 一般的`OnCreateView`なでは、 `View`レイアウトファイルを拡大してから親コンテナーにアタッチすることによってこれを作成します。 Android では、親のレイアウトパラメーターがフラグメントの UI に適用されるため、コンテナーの特性は重要です。 次に例を示します。

```csharp
public override View OnCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
{
    return inflater.Inflate(Resource.Layout.Example_Fragment, container, false);
}
```

上記のコードは、ビュー `Resource.Layout.Example_Fragment`を拡大し、子ビューとして`ViewGroup`コンテナーに追加します。


> [!NOTE]
> フラグメントサブクラスには、パブリックな既定の引数コンストラクターを指定することはできません。

## <a name="adding-a-fragment-to-an-activity"></a>アクティビティへのフラグメントの追加

アクティビティ内でフラグメントをホストする方法は2つあります。

-   **宣言**によるフラグメントは、タグを使用`.axml`することによって`<Fragment>` 、レイアウトファイル内で宣言によって使用できます。 &ndash;

-   **プログラムによる**クラスの API を使用して、 `FragmentManager`フラグメントを動的にインスタンス化することもできます。 &ndash;

クラスを使用し`FragmentManager`たプログラムによる使用方法については、このガイドの後半で説明します。

### <a name="using-a-fragment-declaratively"></a>フラグメントの宣言による使用

レイアウト内にフラグメントを追加するには`<fragment>` 、タグを使用し、 `class`属性また`android:name`は属性のいずれかを指定してフラグメントを識別する必要があります。 次のスニペットは、属性を使用`class`してを`fragment`宣言する方法を示しています。

```xml
<?xml version="1.0" encoding="utf-8"?>
<fragment class="com.xamarin.sample.fragments.TitlesFragment"
            android:id="@+id/titles_fragment"
            android:layout_width="fill_parent"
            android:layout_height="fill_parent" />
```

次のスニペットでは、 `fragment` `android:name`属性を使用してを宣言し、フラグメントクラスを識別する方法を示します。

```xml
<?xml version="1.0" encoding="utf-8"?>
<fragment android:name="com.xamarin.sample.fragments.TitlesFragment"
            android:id="@+id/titles_fragment"
            android:layout_width="fill_parent"
            android:layout_height="fill_parent" />
```

アクティビティの作成時に、Android はレイアウトファイルで指定された各フラグメントをインスタンス化し、 `OnCreateView` `Fragment`要素の代わりにから作成されたビューを挿入します。
アクティビティに宣言的に追加されたフラグメントは静的であり、破棄されるまでアクティビティに保持されます。このようなフラグメントがアタッチされているアクティビティの有効期間中は、そのフラグメントを動的に置換または削除することはできません。

各フラグメントには一意の識別子を割り当てる必要があります。

-  **android: id**&ndash;レイアウトファイル内の他の UI 要素と同様に、これは一意の ID です。

-  **android: タグ**&ndash;この属性は一意の文字列です。

前の2つのメソッドがどちらも使用されていない場合、フラグメントはコンテナービューの ID を想定します。 次の例で`android:id` `android:tag`は、とがどちらも指定されてい`fragment_container`ない場合、Android はその ID をフラグメントに割り当てます。

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

### <a name="package-name-case"></a>パッケージ名の大文字小文字の区別

Android では、パッケージ名に大文字を使用することはできません。パッケージ名に大文字が含まれている場合にビューを拡大しようとすると、例外がスローされます。 ただし、Xamarin Android はより厳格であり、名前空間の大文字が許容されます。

たとえば、次のスニペットはどちらも Xamarin. Android で動作します。 ただし、2番目のスニペットは`android.view.InflateException` 、純粋な Java ベースの Android アプリケーションによってがスローされます。

```xml
<fragment class="com.example.DetailsFragment" android:id="@+id/fragment_content" android:layout_width="match_parent" android:layout_height="match_parent" />
```

OR

```xml
<fragment class="Com.Example.DetailsFragment" android:id="@+id/fragment_content" android:layout_width="match_parent" android:layout_height="match_parent" />
```


## <a name="fragment-lifecycle"></a>フラグメントのライフサイクル

フラグメントには、[ホストアクティビティのライフサイクル](~/android/app-fundamentals/activity-lifecycle/index.md)によって多少独立していても影響を受けている独自のライフサイクルがあります。
たとえば、アクティビティが一時停止すると、関連付けられているすべてのフラグメントが一時停止されます。 次の図は、フラグメントのライフサイクルの概要を示しています。

[![フラグメントのライフサイクルを示すフロー図](creating-a-fragment-images/fragment-lifecycle.png)](creating-a-fragment-images/fragment-lifecycle.png#lightbox)


### <a name="fragment-creation-lifecycle-methods"></a>フラグメント作成ライフサイクルメソッド

次の一覧は、フラグメントの作成時のライフサイクルのさまざまなコールバックのフローを示しています。

-   **`OnInflate()`** &ndash;フラグメントがビューレイアウトの一部として作成されているときに呼び出されます。 このは、フラグメントが XML レイアウトファイルから宣言によって作成された直後に呼び出される場合があります。 フラグメントはまだアクティビティに関連付けられていませんが、ビュー階層からの**アクティビティ**、**バンドル**、および**attributeset**はパラメーターとして渡されます。 このメソッドは、 **Attributeset**を解析し、後でフラグメントによって使用される可能性がある属性を保存する場合に最適です。

-   **`OnAttach()`** &ndash;フラグメントがアクティビティに関連付けられた後に呼び出されます。 これは、フラグメントを使用する準備が整ったときに実行される最初のメソッドです。 一般に、フラグメントはコンストラクターを実装したり、既定のコンストラクターをオーバーライドしたりしないでください。 このフラグメントに必要なコンポーネントは、このメソッドで初期化する必要があります。

-   **`OnCreate()`** &ndash;フラグメントを作成するためにアクティビティによって呼び出されます。 このメソッドが呼び出されると、ホストアクティビティのビュー階層が完全にインスタンス化されていない可能性があるため、フラグメントは、フラグメントのライフサイクルが終了するまで、アクティビティのビュー階層のどの部分にも依存しないようにする必要があります。 たとえば、このメソッドを使用して、アプリケーションの UI に対して微調整や調整を行うことはできません。 これは、フラグメントが必要なデータの収集を開始する最も早い時刻です。 このフラグメントは、この時点で UI スレッドで実行されているため、時間のかかる処理を避けたり、バックグラウンドスレッドで処理を実行したりします。 **SetRetainInstance (true)** が呼び出された場合、このメソッドはスキップされる可能性があります。
    この代替方法については、以下で詳しく説明します。

-   **`OnCreateView()`** &ndash;フラグメントのビューを作成します。
    このメソッドは、アクティビティの**OnCreate ()** メソッドが完了した後に呼び出されます。 この時点で、アクティビティのビュー階層を操作するのは安全です。 このメソッドは、フラグメントによって使用されるビューを返す必要があります。

-   **`OnActivityCreated()`** OnCreate がホスティングアクティビティによって完了した後に呼び出されます。  &ndash;
    ユーザーインターフェイスに対する最終的な調整は、この時点で実行する必要があります。

-   **`OnStart()`** &ndash;格納しているアクティビティが再開された後に呼び出されます。 これにより、フラグメントがユーザーに表示されます。 多くの場合、フラグメントには、アクティビティの**OnStart ()** メソッドに含まれるコードが含まれます。

-   **`OnResume()`** &ndash;これは、ユーザーがフラグメントを操作できるようになる前に最後に呼び出されたメソッドです。 このメソッドで実行する必要があるコードの種類の例としては、ユーザーが対話できるデバイス (ロケーションサービスのカメラなど) の機能を有効にすることが考えられます。 ただし、このようなサービスを使用するとバッテリが過剰に消費される可能性があり、アプリケーションではバッテリ寿命を節約するために使用を最小限に抑える必要があります。


### <a name="fragment-destruction-lifecycle-methods"></a>フラグメント破棄ライフサイクルメソッド

次の一覧では、フラグメントとして呼び出されるライフサイクルメソッドについて説明します。

-   **`OnPause()`** &ndash;ユーザーがフラグメントを操作できなくなりました。 このような状況が発生するのは、他のフラグメント操作によってこのフラグメントが変更されているか、ホスティングアクティビティが一時停止されているためです。 このフラグメントをホストしているアクティビティがまだ表示されている可能性があります。つまり、フォーカスされているアクティビティが部分的に透明になっているか、全画面が占有されていない可能性があります。 このメソッドがアクティブになると、ユーザーがフラグメントから出ていることが最初に示されます。 フラグメントは、変更を保存します。

-   **`OnStop()`** &ndash;フラグメントは表示されなくなりました。 ホストアクティビティが停止しているか、アクティビティ内でフラグメント操作によって変更されている可能性があります。 このコールバックは、**アクティビティ OnStop**と同じ目的を果たします。

-   **`OnDestroyView()`** &ndash;このメソッドは、ビューに関連付けられているリソースをクリーンアップするために呼び出されます。 このは、フラグメントに関連付けられたビューが破棄されたときに呼び出されます。

-   **`OnDestroy()`** &ndash;このメソッドは、フラグメントが使用されなくなったときに呼び出されます。 それでもアクティビティに関連付けられていますが、フラグメントは機能しなくなりました。 このメソッドは、カメラに使用される可能性のある[**SurfaceView**](xref:Android.Views.SurfaceView)など、フラグメントによって使用されているすべてのリソースを解放する必要があります。 **SetRetainInstance (true)** が呼び出された場合、このメソッドはスキップされる可能性があります。 この代替方法については、以下で詳しく説明します。

-   **`OnDetach()`** &ndash;このメソッドは、フラグメントがアクティビティに関連付けられなくなる直前に呼び出されます。 フラグメントのビュー階層は存在しなくなり、フラグメントによって使用されるすべてのリソースがこの時点で解放されます。


### <a name="using-setretaininstance"></a>SetRetainInstance の使用

フラグメントは、アクティビティを再作成する場合に完全に破棄しないように指定することができます。 クラス`Fragment`は、この目的`SetRetainInstance`のためのメソッドを提供します。 この`true`メソッドにが渡された場合、アクティビティが再開されると、フラグメントの同じインスタンスが使用されます。 これが発生すると、および`OnCreate` `OnDestroy`ライフサイクルコールバックを除くすべてのコールバックメソッドが呼び出されます。 このプロセスは、上に示したライフサイクル図 (緑色の点線) で示されています。


## <a name="fragment-state-management"></a>フラグメント状態の管理

フラグメントは、のインスタンスを使用して、 `Bundle`フラグメントのライフサイクル中に状態を保存および復元できます。 このバンドルを使用すると、フラグメントはキーと値のペアとしてデータを保存できます。これは、大量のメモリを必要としない単純なデータに役立ちます。 フラグメントは、の呼び出しで状態を`OnSaveInstanceState`保存できます。

```csharp
public override void OnSaveInstanceState(Bundle outState)
{
    base.OnSaveInstanceState(outState);
    outState.PutInt("current_choice", _currentCheckPosition);
}
```

フラグメントの新しいインスタンスが作成されると`Bundle` 、新しいインスタンスの`OnCreate`、 `OnCreateView`、および`OnActivityCreated`の各メソッドを使用して、に保存されている状態を新しいインスタンスで使用できるようになります。
次の例は、 `current_choice` `Bundle`から値を取得する方法を示しています。

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

オーバーライド`OnSaveInstanceState`は、前の例の`current_choice`値などの方向の変化にわたって、フラグメントに一時的なデータを保存するための適切なメカニズムです。 ただし、の既定の`OnSaveInstanceState`実装では、ID が割り当てられているすべてのビューの UI に一時データが保存されます。 たとえば、次のように XML で定義さ`EditText`れた要素を含むアプリケーションを見てみましょう。

```xml
<EditText android:id="@+id/myText"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"/>
```

コントロールにはが`OnSaveInstanceState`割り当てられているため、が呼び出されると、フラグメントはウィジェットにデータを自動的に保存します。 `id` `EditText`


### <a name="bundle-limitations"></a>バンドルの制限事項

を使用`OnSaveInstanceState`すると、一時的なデータを簡単に保存できますが、このメソッドの使用にはいくつかの制限があります。

-  フラグメントが戻るスタックに追加されていない場合、ユーザーが **[戻る]** ボタンを押しても、その状態は復元されません。

-  バンドルを使用してデータを保存すると、そのデータがシリアル化されます。 これにより、処理の遅延が発生する可能性があります。


## <a name="contributing-to-the-menu"></a>メニューに貢献する

フラグメントは、ホストアクティビティのメニューに項目を投稿する場合があります。
アクティビティは、メニュー項目を最初に処理します。 アクティビティにハンドラーがない場合は、イベントがフラグメントに渡され、それによって処理されます。

アクティビティのメニューに項目を追加するには、フラグメントが2つの処理を行う必要があります。
まず、次のコードに示すよう`OnCreateOptionsMenu`に、フラグメントはメソッドを実装し、その項目をメニューに配置する必要があります。

```csharp
public override void OnCreateOptionsMenu(IMenu menu, MenuInflater menuInflater)
{
    menuInflater.Inflate(Resource.Menu.menu_fragment_vehicle_list, menu);
    base.OnCreateOptionsMenu(menu, menuInflater);
}
```

前のコードスニペットのメニューは、ファイル`menu_fragment_vehicle_list.xml`にある次の XML から大きくなっています。

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
  <item android:id="@+id/add_vehicle"
        android:icon="@drawable/ic_menu_add_data"
        android:title="@string/add_vehicle" />
</menu>
```

次に、フラグメントはを`SetHasOptionsMenu(true)`呼び出す必要があります。 このメソッドへの呼び出しは、オプションメニューに寄与するメニュー項目がフラグメントにあることを Android に通知します。 このメソッドの呼び出しが行われない限り、フラグメントのメニュー項目はアクティビティの [オプション] メニューに追加されません。 これは通常、次のコードスニペット`OnCreate()`に示すように、ライフサイクルメソッドで実行されます。

```csharp
public override void OnCreate(Bundle savedState)
{
    base.OnCreate(savedState);
    SetHasOptionsMenu(true);
}
```

次の画面は、このメニューがどのように表示されるかを示しています。

[![マイ旅行アプリのメニュー項目を表示する例のスクリーンショット](creating-a-fragment-images/fragment-menu-example.png)](creating-a-fragment-images/fragment-menu-example.png#lightbox)
