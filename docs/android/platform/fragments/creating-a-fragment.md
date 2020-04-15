---
title: フラグメントの作成
ms.prod: xamarin
ms.assetid: F2997242-BC29-1440-7F1A-CFC447CD73FA
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/07/2018
ms.openlocfilehash: 0e8d3748c7ddd337cf2f27f5b272b208e79d503a
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "73027504"
---
# <a name="creating-a-fragment"></a>フラグメントの作成

フラグメントを作成するには、`Android.App.Fragment` からクラスを継承し、`OnCreateView` メソッドをオーバーライドする必要があります。 `OnCreateView` は、スクリーンにフラグメントを配置するときに、ホスト アクティビティによって呼び出され、`View` を返します。 一般的な `OnCreateView` では、レイアウト ファイルを拡張してから親コンテナーにアタッチすることによって、この `View` を作成します。 Android では、親のレイアウト パラメーターがフラグメントの UI に適用されるため、コンテナーの特性は重要です。 次に例を示します。

```csharp
public override View OnCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
{
    return inflater.Inflate(Resource.Layout.Example_Fragment, container, false);
}
```

上記のコードでは、ビュー `Resource.Layout.Example_Fragment` を拡張し、`ViewGroup` コンテナーに子ビューとして追加します。

> [!NOTE]
> フラグメントのサブクラスには、引数なしの既定の public コンストラクターを指定する必要があります。

## <a name="adding-a-fragment-to-an-activity"></a>アクティビティへのフラグメントの追加

フラグメントをアクティビティ内にホストする方法には、次の 2 つがあります。

- **宣言による方法** &ndash; `.axml` レイアウト ファイル内で `<Fragment>` タグを使用して、宣言によりフラグメントを使用できます。

- **プログラムによる方法** &ndash; `FragmentManager` クラスの API を使用して、フラグメントを動的にインスタンス化できます。

`FragmentManager` クラスを使用したプログラムによる使用法については、このガイドで後述します。

### <a name="using-a-fragment-declaratively"></a>宣言によるフラグメントの使用

レイアウト内にフラグメントを追加するには、`<fragment>` タグを使用し、`class` 属性または `android:name` 属性を指定してフラグメントを識別する必要があります。 次のスニペットは、`class` 属性を使用して `fragment` を宣言する方法を示しています。

```xml
<?xml version="1.0" encoding="utf-8"?>
<fragment class="com.xamarin.sample.fragments.TitlesFragment"
            android:id="@+id/titles_fragment"
            android:layout_width="fill_parent"
            android:layout_height="fill_parent" />
```

次のスニペットは、`android:name` 属性を使用して Fragment クラスを識別することにより、`fragment` を宣言する方法を示しています。

```xml
<?xml version="1.0" encoding="utf-8"?>
<fragment android:name="com.xamarin.sample.fragments.TitlesFragment"
            android:id="@+id/titles_fragment"
            android:layout_width="fill_parent"
            android:layout_height="fill_parent" />
```

アクティビティを作成する際に Android では、レイアウト ファイルに指定された各フラグメントをインスタンス化し、`OnCreateView` か作成されたビューを `Fragment` 要素の代わりに挿入します。
宣言によりアクティビティに追加されたフラグメントは静的であり、破棄されるまではそのアクティビティに残ります。アクティビティにアタッチされているフラグメントを、そのアクティビティの有効期間内に動的に置き換えたり削除したりすることはできません。

各フラグメントには、次のような一意識別子を割り当てる必要があります。

- **android:id** &ndash; レイアウト ファイル内のほかの UI 要素と同様に、一意の ID です。

- **android:tag** &ndash; これは、一意の文字列属性です。

前述の 2 つの方法のいずれも使用しなかった場合は、コンテナー ビューの ID が指定されたとみなされます。 次の例では `android:id` も `android:tag` も指定されていないため、ID `fragment_container` がフラグメントに割り当てられます。

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

### <a name="package-name-case"></a>パッケージ名での大文字と小文字の区別

Android ではパッケージ名に大文字を使用できません。パッケージ名に大文字が含まれている場合、ビューを拡張しようとしたときに例外がスローされます。 ただし、Xamarin.Android は比較的寛容であり、名前空間内の大文字は無視されます。

たとえば、次のスニペットはいずれも、Xamarin.Android で機能します。 ただし、2 つ目のスニペットは、純粋な Java ベースの Android アプリの場合は `android.view.InflateException` がスローされます。

```xml
<fragment class="com.example.DetailsFragment" android:id="@+id/fragment_content" android:layout_width="match_parent" android:layout_height="match_parent" />
```

OR

```xml
<fragment class="Com.Example.DetailsFragment" android:id="@+id/fragment_content" android:layout_width="match_parent" android:layout_height="match_parent" />
```

## <a name="fragment-lifecycle"></a>フラグメントのライフサイクル

フラグメントのライフサイクルは、[ホスト アクティビティのライフサイクル](~/android/app-fundamentals/activity-lifecycle/index.md)に完全には依存しませんが、それでも影響は受けます。
たとえば、アクティビティが一時停止すると、関連付けられたすべてのフラグメントが一時停止します。 次の図は、フラグメントのライフサイクルの概略を示しています。

[![フラグメントのライフサイクルを示すフロー図](creating-a-fragment-images/fragment-lifecycle.png)](creating-a-fragment-images/fragment-lifecycle.png#lightbox)

### <a name="fragment-creation-lifecycle-methods"></a>フラグメント作成のライフサイクル メソッド

次の一覧は、フラグメントが作成されるライフサイクルにおけるさまざまなコールバックのフローを示しています。

- **`OnInflate()`** &ndash; ビュー レイアウトのパーツとしてフラグメントが作成されるときに呼び出されます。 XML レイアウト ファイルで宣言によりフラグメントが作成された直後に呼び出すことができます。 フラグメントはアクティビティにまだ関連付けられていませんが、ビュー階層の **Activity**、**Bundle**、および **AttributeSet** がパラメーターとして渡されます。 このメソッドは、**AttributeSet** の解析と、後でフラグメントが使用する属性を保存する目的で最もよく使用されます。

- **`OnAttach()`** &ndash; フラグメントがアクティビティに関連付けられた後で呼び出されます。 これは、フラグメントの使用準備が整ったときに最初に実行されるメソッドです。 一般に、フラグメントではコンストラクターを実装せず、既定のコンストラクターをオーバーライドしません。 フラグメントに必要なすべての要素はこのメソッドで初期化してください。

- **`OnCreate()`** &ndash; フラグメント作成のためにアクティビティによって呼び出されます。 このメソッドの呼び出し時にホスト アクティビティのビュー階層のインスタンス化が完了していない可能性があるため、フラグメントのライフサイクル内のもっと後の段階になるまでは、アクティビティのビューのパーツにフラグメントが依存しないようにします。 たとえば、このメソッドを使用して、アプリの UI の調整や微調整を行わないでください。 これは、フラグメントに必要なデータの収集を開始する最初の段階です。 この時点ではフラグメントは UI スレッドで実行中であるため、時間のかかる処理は避けるか、バックグラウンド スレッドで実行してください。 このメソッドは、**SetRetainInstance(true)** が呼び出される場合はスキップできます。
    この代替方法については、後ほど詳しく説明します。

- **`OnCreateView()`** &ndash; フラグメントのビューを作成します。
    このメソッドは、アクティビティの **OnCreate()** メソッドが完了すると呼び出されます。 この時点では、アクティビティのビュー階層と安全に対話できます。 このメソッドからは、フラグメントが使用するビューが返されます。

- **`OnActivityCreated()`** &ndash; ホスト アクティビティによる **Activity.OnCreate** の完了後に呼び出されます。
    この時点で、ユーザー インターフェイスの最終的な微調整を行います。

- **`OnStart()`** &ndash; ホスト アクティビティが再開された後に呼び出されます。 フラグメントがユーザーに対して表示されるようになります。 多くの場合、フラグメントには、アクティビティの **OnStart()** メソッドに含まれるようなコードが格納されます。

- **`OnResume()`** &ndash; これは、ユーザーがフラグメントと対話できるようになる前に呼び出される最後のメソッドです。 このメソッドで実行されるコードの種類の例としては、ユーザーが対話するデバイスの機能 (カメラの位置情報サービスなど) を有効にすることです。 ただし、この種の機能はバッテリーの著しい消耗を招く可能性があります。バッテリー寿命を保つため、アプリによるバッテリー消費を最小限に抑えるようにしてください。

### <a name="fragment-destruction-lifecycle-methods"></a>フラグメント破棄のライフサイクル メソッド

フラグメント破棄の際に呼び出されるライフサイクル メソッドの一覧を次に示します。

- **`OnPause()`** &ndash; ユーザーはフラグメントと対話できなくなります。 このような状況は、別のフラグメント操作がこのフラグメントを変更している場合、またはホスト アクティビティが一時停止されている場合に存在します。 このフラグメントをホストしているアクティビティはまだ表示されている (つまり、フォーカスされているアクティビティが部分的に透明であるか、全画面を占有していない) 可能性があります。 このメソッドがアクティブになったときは、ユーザーがこのフラグメントを離れようとしていることを示す最初の兆候です。 フラグメントは、すべての変更を保存します。

- **`OnStop()`** &ndash; フラグメントは表示されなくなります。 ホスト アクティビティが停止されたか、アクティビティ内の別のフラグメント操作がこのフラグメントを変更しています。 このコールバックは、**Activity.OnStop** と同じ目的で用いられます。

- **`OnDestroyView()`** &ndash; このメソッドは、ビューに関連付けられたリソースをクリーンアップするために呼び出されます。 フラグメントに関連付けられたビューが破棄されたときに呼び出されます。

- **`OnDestroy()`** &ndash; このメソッドは、フラグメントが使用されなくなったときに呼び出されます。 フラグメントはアクティビティにまだ関連付けられていますが、機能しなくなっています。 このメソッドは、フラグメントによって使用されていたリソース (カメラに使用される [**SurfaceView**](xref:Android.Views.SurfaceView) など) を解放します。 このメソッドは、**SetRetainInstance(true)** が呼び出される場合はスキップできます。 この代替方法については、後ほど詳しく説明します。

- **`OnDetach()`** &ndash; このメソッドはフラグメントとアクティビティの関連付けが解除される直前に呼び出されます。 フラグメントのビュー階層はもう存在せず、フラグメントが使用していたすべてのリソースはこの時点で解放されます。

### <a name="using-setretaininstance"></a>SetRetainInstance の使用

アクティビティが再作成される予定の場合、フラグメントが完全には破棄されないように指定できます。 `Fragment` クラスには、この目的で使用する `SetRetainInstance` メソッドが用意されています。 このメソッドに `true` を渡した場合、アクティビティが再起動されたときに、同じフラグメントのインスタンスが使用されます。 この状況が発生した場合、`OnCreate` と `OnDestroy` 以外のすべてのライフサイクル コールバック メソッドが呼び出されます。 このプロセスについては、前述のライフサイクル図 (緑色の点線) で示しています。

## <a name="fragment-state-management"></a>フラグメント状態管理

`Bundle` のインスタンスを使用して、フラグメントのライフサイクル中にフラグメントの状態を保存および復元できます。 Bundle を使用するとフラグメントをキーと値のペアとして保存することができるため、多くのメモリを必要としない単純なデータの場合に有用です。 次のように `OnSaveInstanceState` を呼び出すことで、フラグメントの状態を保存できます。

```csharp
public override void OnSaveInstanceState(Bundle outState)
{
    base.OnSaveInstanceState(outState);
    outState.PutInt("current_choice", _currentCheckPosition);
}
```

フラグメントの新しいインスタンスの生成時に、この新しいインスタンスの `OnCreate`、`OnCreateView`、および `OnActivityCreated` の各メソッドを使用して、`Bundle` に保存された状態を使用できるようになります。
次のサンプルでは、`Bundle` から値 `current_choice` を取得する方法を示します。

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

`OnSaveInstanceState` のオーバーライドは、前の例の `current_choice` の値など、向きの変化の前後でのフラグメントの一時的なデータを保存する際に適したメカニズムです。 ただし、`OnSaveInstanceState` の既定の実装では、ID が割り当てられているすべてのビューの UI に、一時的なデータが保存されます。 たとえば、次のように XML で定義された `EditText` 要素を持つアプリを見てみましょう。

```xml
<EditText android:id="@+id/myText"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"/>
```

`EditText` コントロールには `id` が割り当てられているため、`OnSaveInstanceState` が呼び出されると、フラグメントによってウィジェットのデータが自動的に保存されます。

### <a name="bundle-limitations"></a>バンドルの制限事項

`OnSaveInstanceState` を使用すると、一時的なデータを簡単に保存できますが、このメソッドの使用には次のような制限事項があります。

- フラグメントがバック スタックに追加されていない場合、ユーザーが **[戻る]** ボタン を押したときに、その状態は復元されません。

- バンドルを使用してデータを保存するときに、そのデータがシリアル化されます。 これにより、処理の遅延が発生する可能性があります。

## <a name="contributing-to-the-menu"></a>メニュー項目の提供

フラグメントは、ホスト アクティビティのメニューに項目を提供できます。
アクティビティは、メニュー項目を最初に処理します。 アクティビティにハンドラーがない場合は、イベントがフラグメントに渡されて、処理されます。

アクティビティのメニューに項目を追加するには、フラグメントで 2 つの処理を行う必要があります。
1 つ目の処理として、次のコードに示すように、メソッド `OnCreateOptionsMenu` を実装し、その項目をメニューに配置する必要があります。

```csharp
public override void OnCreateOptionsMenu(IMenu menu, MenuInflater menuInflater)
{
    menuInflater.Inflate(Resource.Menu.menu_fragment_vehicle_list, menu);
    base.OnCreateOptionsMenu(menu, menuInflater);
}
```

前のコード スニペットのメニューは、ファイル `menu_fragment_vehicle_list.xml` にある次の XML から拡張されます。

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
  <item android:id="@+id/add_vehicle"
        android:icon="@drawable/ic_menu_add_data"
        android:title="@string/add_vehicle" />
</menu>
```

2 つ目の処理として、フラグメントは `SetHasOptionsMenu(true)` を呼び出す必要があります。 このメソッドの呼び出しにより、オプション メニューに提供するメニュー項目がフラグメントにあることを Android に通知します。 このメソッドの呼び出しが行われない限り、フラグメントのメニュー項目はアクティビティのオプション メニューに追加されません。 この処理は通常、次のコード スニペットに示すように、ライフサイクルの `OnCreate()` メソッドで行われます。

```csharp
public override void OnCreate(Bundle savedState)
{
    base.OnCreate(savedState);
    SetHasOptionsMenu(true);
}
```

このメニューの外観を次のスクリーンに示します。

[![My Trips アプリのメニュー項目を示すスクリーンショットの例](creating-a-fragment-images/fragment-menu-example.png)](creating-a-fragment-images/fragment-menu-example.png#lightbox)
