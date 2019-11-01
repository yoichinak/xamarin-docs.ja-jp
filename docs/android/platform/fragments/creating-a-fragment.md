---
title: フラグメントの作成
ms.prod: xamarin
ms.assetid: F2997242-BC29-1440-7F1A-CFC447CD73FA
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/07/2018
ms.openlocfilehash: 0e8d3748c7ddd337cf2f27f5b272b208e79d503a
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027504"
---
# <a name="creating-a-fragment"></a>フラグメントの作成

フラグメントを作成するには、クラスが `Android.App.Fragment` から継承してから、`OnCreateView` メソッドをオーバーライドする必要があります。 `OnCreateView` は、画面にフラグメントを配置する時間が経過すると、ホストアクティビティによって呼び出され、`View`を返します。 一般的な `OnCreateView` では、レイアウトファイルを拡大してから親コンテナーにアタッチすることによって、この `View` を作成します。 Android では、親のレイアウトパラメーターがフラグメントの UI に適用されるため、コンテナーの特性は重要です。 次に例を示します。

```csharp
public override View OnCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
{
    return inflater.Inflate(Resource.Layout.Example_Fragment, container, false);
}
```

上記のコードでは、ビュー `Resource.Layout.Example_Fragment`を拡大し、`ViewGroup` コンテナーに子ビューとして追加します。

> [!NOTE]
> フラグメントサブクラスには、パブリックな既定の引数コンストラクターを指定することはできません。

## <a name="adding-a-fragment-to-an-activity"></a>アクティビティへのフラグメントの追加

アクティビティ内でフラグメントをホストする方法は2つあります。

- **宣言**&ndash; フラグメントは、`<Fragment>` タグを使用することにより、`.axml` レイアウトファイル内で宣言によって使用できます。

- **プログラムによっ**て &ndash; フラグメントは、`FragmentManager` クラスの API を使用して動的にインスタンス化することもできます。

`FragmentManager` クラスを使用したプログラムによる使用方法については、このガイドの後半で説明します。

### <a name="using-a-fragment-declaratively"></a>フラグメントの宣言による使用

レイアウト内にフラグメントを追加するには、`<fragment>` タグを使用し、`class` 属性または `android:name` 属性のいずれかを指定してフラグメントを識別する必要があります。 次のスニペットは、`class` 属性を使用して `fragment`を宣言する方法を示しています。

```xml
<?xml version="1.0" encoding="utf-8"?>
<fragment class="com.xamarin.sample.fragments.TitlesFragment"
            android:id="@+id/titles_fragment"
            android:layout_width="fill_parent"
            android:layout_height="fill_parent" />
```

次のスニペットは、`android:name` 属性を使用してフラグメントクラスを識別することによって `fragment` を宣言する方法を示しています。

```xml
<?xml version="1.0" encoding="utf-8"?>
<fragment android:name="com.xamarin.sample.fragments.TitlesFragment"
            android:id="@+id/titles_fragment"
            android:layout_width="fill_parent"
            android:layout_height="fill_parent" />
```

アクティビティの作成時に、Android はレイアウトファイルで指定された各フラグメントをインスタンス化し、`Fragment` 要素の代わりに `OnCreateView` から作成されたビューを挿入します。
アクティビティに宣言的に追加されたフラグメントは静的であり、破棄されるまでアクティビティに保持されます。このようなフラグメントがアタッチされているアクティビティの有効期間中は、そのフラグメントを動的に置換または削除することはできません。

各フラグメントには一意の識別子を割り当てる必要があります。

- **android: id** &ndash; レイアウトファイル内の他の UI 要素と同様に、一意の id です。

- **android: タグ**&ndash; この属性は一意の文字列です。

前の2つのメソッドがどちらも使用されていない場合、フラグメントはコンテナービューの ID を想定します。 次の例では、`android:id` も `android:tag` も指定されていませんが、Android はこのフラグメントに ID `fragment_container` を割り当てます。

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

たとえば、次のスニペットはどちらも Xamarin. Android で動作します。 ただし、2番目のスニペットでは、純粋な Java ベースの Android アプリケーションによって `android.view.InflateException` がスローされます。

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

[フラグメントのライフサイクルを示す![フローダイアグラム](creating-a-fragment-images/fragment-lifecycle.png)](creating-a-fragment-images/fragment-lifecycle.png#lightbox)

### <a name="fragment-creation-lifecycle-methods"></a>フラグメント作成ライフサイクルメソッド

次の一覧は、フラグメントの作成時のライフサイクルのさまざまなコールバックのフローを示しています。

- **`OnInflate()`** &ndash; は、フラグメントがビューレイアウトの一部として作成されているときに呼び出されます。 このは、フラグメントが XML レイアウトファイルから宣言によって作成された直後に呼び出される場合があります。 フラグメントはまだアクティビティに関連付けられていませんが、ビュー階層からの**アクティビティ**、**バンドル**、および**attributeset**はパラメーターとして渡されます。 このメソッドは、 **Attributeset**を解析し、後でフラグメントによって使用される可能性がある属性を保存する場合に最適です。

- **`OnAttach()`** &ndash;、フラグメントがアクティビティに関連付けられた後に呼び出されます。 これは、フラグメントを使用する準備が整ったときに実行される最初のメソッドです。 一般に、フラグメントはコンストラクターを実装したり、既定のコンストラクターをオーバーライドしたりしないでください。 このフラグメントに必要なコンポーネントは、このメソッドで初期化する必要があります。

- フラグメントを作成するためにアクティビティによって呼び出される **`OnCreate()`** &ndash;。 このメソッドが呼び出されると、ホストアクティビティのビュー階層が完全にインスタンス化されていない可能性があるため、フラグメントは、フラグメントのライフサイクルが終了するまで、アクティビティのビュー階層のどの部分にも依存しないようにする必要があります。 たとえば、このメソッドを使用して、アプリケーションの UI に対して微調整や調整を行うことはできません。 これは、フラグメントが必要なデータの収集を開始する最も早い時刻です。 このフラグメントは、この時点で UI スレッドで実行されているため、時間のかかる処理を避けたり、バックグラウンドスレッドで処理を実行したりします。 **SetRetainInstance (true)** が呼び出された場合、このメソッドはスキップされる可能性があります。
    この代替方法については、以下で詳しく説明します。

- **`OnCreateView()`** &ndash; フラグメントのビューを作成します。
    このメソッドは、アクティビティの**OnCreate ()** メソッドが完了した後に呼び出されます。 この時点で、アクティビティのビュー階層を操作するのは安全です。 このメソッドは、フラグメントによって使用されるビューを返す必要があります。

- **`OnActivityCreated()`** &ndash;、ホスティングアクティビティによって**OnCreate**が完了した後に呼び出されます。
    ユーザーインターフェイスに対する最終的な調整は、この時点で実行する必要があります。

- **`OnStart()`** &ndash; は、格納しているアクティビティが再開された後に呼び出されます。 これにより、フラグメントがユーザーに表示されます。 多くの場合、フラグメントには、アクティビティの**OnStart ()** メソッドに含まれるコードが含まれます。

- **`OnResume()`** &ndash; これは、ユーザーがフラグメントを操作できるようになる前に最後に呼び出されたメソッドです。 このメソッドで実行する必要があるコードの種類の例としては、ユーザーが対話できるデバイス (ロケーションサービスのカメラなど) の機能を有効にすることが考えられます。 ただし、このようなサービスを使用するとバッテリが過剰に消費される可能性があり、アプリケーションではバッテリ寿命を節約するために使用を最小限に抑える必要があります。

### <a name="fragment-destruction-lifecycle-methods"></a>フラグメント破棄ライフサイクルメソッド

次の一覧では、フラグメントとして呼び出されるライフサイクルメソッドについて説明します。

- **`OnPause()`** 、ユーザーがフラグメントを操作できなく &ndash; ます。 このような状況が発生するのは、他のフラグメント操作によってこのフラグメントが変更されているか、ホスティングアクティビティが一時停止されているためです。 このフラグメントをホストしているアクティビティがまだ表示されている可能性があります。つまり、フォーカスされているアクティビティが部分的に透明になっているか、全画面が占有されていない可能性があります。 このメソッドがアクティブになると、ユーザーがフラグメントから出ていることが最初に示されます。 フラグメントは、変更を保存します。

- **`OnStop()`** &ndash; フラグメントが表示されなくなります。 ホストアクティビティが停止しているか、アクティビティ内でフラグメント操作によって変更されている可能性があります。 このコールバックは、**アクティビティ OnStop**と同じ目的を果たします。

- **`OnDestroyView()`** &ndash; このメソッドは、ビューに関連付けられているリソースをクリーンアップするために呼び出されます。 このは、フラグメントに関連付けられたビューが破棄されたときに呼び出されます。

- **`OnDestroy()`** &ndash; このメソッドは、フラグメントが使用されなくなったときに呼び出されます。 それでもアクティビティに関連付けられていますが、フラグメントは機能しなくなりました。 このメソッドは、カメラに使用される可能性のある[**SurfaceView**](xref:Android.Views.SurfaceView)など、フラグメントによって使用されているすべてのリソースを解放する必要があります。 **SetRetainInstance (true)** が呼び出された場合、このメソッドはスキップされる可能性があります。 この代替方法については、以下で詳しく説明します。

- **`OnDetach()`** &ndash; このメソッドは、フラグメントがアクティビティに関連付けられなくなる直前に呼び出されます。 フラグメントのビュー階層は存在しなくなり、フラグメントによって使用されるすべてのリソースがこの時点で解放されます。

### <a name="using-setretaininstance"></a>SetRetainInstance の使用

フラグメントは、アクティビティを再作成する場合に完全に破棄しないように指定することができます。 `Fragment` クラスは、この目的のためのメソッド `SetRetainInstance` を提供します。 このメソッドに `true` が渡された場合、アクティビティが再開されると、そのフラグメントの同じインスタンスが使用されます。 このような場合は、`OnCreate` と `OnDestroy` ライフサイクルコールバックを除き、すべてのコールバックメソッドが呼び出されます。 このプロセスは、上に示したライフサイクル図 (緑色の点線) で示されています。

## <a name="fragment-state-management"></a>フラグメント状態の管理

フラグメントは、`Bundle`のインスタンスを使用して、フラグメントのライフサイクル中に状態を保存および復元できます。 このバンドルを使用すると、フラグメントはキーと値のペアとしてデータを保存できます。これは、大量のメモリを必要としない単純なデータに役立ちます。 フラグメントは、`OnSaveInstanceState`の呼び出しを使用して状態を保存できます。

```csharp
public override void OnSaveInstanceState(Bundle outState)
{
    base.OnSaveInstanceState(outState);
    outState.PutInt("current_choice", _currentCheckPosition);
}
```

フラグメントの新しいインスタンスが作成されると、新しいインスタンスの `OnCreate`、`OnCreateView`、および `OnActivityCreated` の各メソッドを使用して、`Bundle` に保存されている状態が新しいインスタンスで使用できるようになります。
次の例は、`Bundle`から値 `current_choice` を取得する方法を示しています。

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

`OnSaveInstanceState` のオーバーライドは、前の例の `current_choice` の値など、方向の変化にわたって、フラグメントに一時的なデータを保存するための適切なメカニズムです。 ただし、`OnSaveInstanceState` の既定の実装では、ID が割り当てられているすべてのビューの UI に、一時的なデータが保存されます。 たとえば、次のように XML で定義された `EditText` 要素を持つアプリケーションを見てみましょう。

```xml
<EditText android:id="@+id/myText"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"/>
```

`EditText` コントロールには `id` が割り当てられているため、`OnSaveInstanceState` が呼び出されると、フラグメントによってデータがウィジェットに自動的に保存されます。

### <a name="bundle-limitations"></a>バンドルの制限事項

`OnSaveInstanceState` を使用すると、一時的なデータを簡単に保存できますが、このメソッドの使用にはいくつかの制限があります。

- フラグメントが戻るスタックに追加されていない場合、ユーザーが **[戻る]** ボタンを押しても、その状態は復元されません。

- バンドルを使用してデータを保存すると、そのデータがシリアル化されます。 これにより、処理の遅延が発生する可能性があります。

## <a name="contributing-to-the-menu"></a>メニューに貢献する

フラグメントは、ホストアクティビティのメニューに項目を投稿する場合があります。
アクティビティは、メニュー項目を最初に処理します。 アクティビティにハンドラーがない場合は、イベントがフラグメントに渡され、それによって処理されます。

アクティビティのメニューに項目を追加するには、フラグメントが2つの処理を行う必要があります。
まず、次のコードに示すように、フラグメントはメソッド `OnCreateOptionsMenu` を実装し、その項目をメニューに配置する必要があります。

```csharp
public override void OnCreateOptionsMenu(IMenu menu, MenuInflater menuInflater)
{
    menuInflater.Inflate(Resource.Menu.menu_fragment_vehicle_list, menu);
    base.OnCreateOptionsMenu(menu, menuInflater);
}
```

前のコードスニペットのメニューは、ファイル `menu_fragment_vehicle_list.xml`にある次の XML から大きくなっています。

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
  <item android:id="@+id/add_vehicle"
        android:icon="@drawable/ic_menu_add_data"
        android:title="@string/add_vehicle" />
</menu>
```

次に、フラグメントは `SetHasOptionsMenu(true)`を呼び出す必要があります。 このメソッドへの呼び出しは、オプションメニューに寄与するメニュー項目がフラグメントにあることを Android に通知します。 このメソッドの呼び出しが行われない限り、フラグメントのメニュー項目はアクティビティの [オプション] メニューに追加されません。 これは通常、次のコードスニペットに示すように、ライフサイクルメソッド `OnCreate()`で実行されます。

```csharp
public override void OnCreate(Bundle savedState)
{
    base.OnCreate(savedState);
    SetHasOptionsMenu(true);
}
```

次の画面は、このメニューがどのように表示されるかを示しています。

[![メニュー項目を表示している旅行アプリのスクリーンショットの例](creating-a-fragment-images/fragment-menu-example.png)](creating-a-fragment-images/fragment-menu-example.png#lightbox)
