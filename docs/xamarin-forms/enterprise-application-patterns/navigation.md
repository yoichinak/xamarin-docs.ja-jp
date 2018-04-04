---
title: ナビゲーション
ms.prod: xamarin
ms.assetid: 4cad57b5-7fe4-4527-a988-d9b60c9620b4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: aa2e2858e3bb8e435ec3f38bb3d5b249eaa6cba4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="navigation"></a>ナビゲーション

Xamarin.Forms には、ページ ナビゲーションは、通常、UI でのユーザーの操作とは、アプリ自体のロジックに基づく内部の状態を変更した結果からの結果のサポートが含まれています。 ただし、ナビゲーションは複雑で、次の課題を満たす必要があると、モデル View-viewmodel (MVVM) パターンを使用するアプリで実装できます。

-   密結合とビュー間の依存関係を提供しませんなアプローチを使用して、移動できない場合に、ビューを特定する方法。
-   移動できない場合に、ビューをインスタンス化および初期化するプロセスを調整する方法。 MVVM を使用する場合、ビューとビューのモデルをインスタンス化され、ビューのバインディング コンテキストを使用して相互に関連付けられている必要があります。 アプリが依存性の注入コンテナーを使用しているときに、ビューとビューのモデルのインスタンス化は特定の構築のメカニズムを必要があります。
-   最初のビューのナビゲーションを実行またはモデル優先ナビゲーションを表示するかどうか。 最初のビューのナビゲーションを使用して、ページに移動するはビューの種類の名前を示します。 ナビゲーション中に、指定されたビューがインスタンス化、およびその対応するビュー モデルとその他の依存するサービスです。 その他の方法では、ビュー モデル優先のナビゲーションを使用して、場所に移動するページを参照して、ビュー モデルの種類の名前にします。
-   どのクリーンにするには、ビューとビューのモデルの間で、アプリのナビゲーション動作を区切ります。 MVVM パターンでは、アプリの UI、プレゼンテーション、およびビジネス ロジックの間の分離を提供します。 ただし、アプリのナビゲーション動作は多くの場合、アプリの UI とプレゼンテーションの部分を します。 ユーザーがから見ると、ナビゲーションを開始して多くの場合、し、ナビゲーションの結果として、ビューが置き換えられます。 ただし、ナビゲーションでは、開始またはからビュー モデル内で調整する必要がありますもよくあります。
-   初期化のためにナビゲーション中にパラメーターを渡す方法です。 たとえば、ユーザーは、注文の詳細を更新するためのビューに移動する場合、は、注文データが、正しいデータを表示できるように、ビューに渡される必要があります。
-   コーディネート ナビゲーションは、特定のビジネス ルールが従うことを確認する方法。 たとえば、ユーザーは、無効なデータを修正できるまたは送信またはビュー内で行われたデータの変更を破棄するように求められますできるように、ビューから移動する前に求められます可能性があります。

この章では、これらの課題は提示することにより、`NavigationService`ビュー モデル最初のページ ナビゲーションの実行に使用されるクラスです。

> [!NOTE]
> `NavigationService`によって使用される、アプリがコンテンツ ページ インスタンス間で階層的なナビゲーションを実行するためだけに設計されています。 サービスを使用して、その他のページの種類間を移動すると、予期しない動作が発生する可能性があります。

## <a name="navigating-between-pages"></a>ページ間を移動します。

ナビゲーションのロジックでは、ビューの分離コード内に存在したり、データのビュー モデルをバインドすることができます。 ビューでナビゲーション ロジックを配置する最も簡単な方法があります、単体テストを簡単にテストが容易なことはできません。 ビューでナビゲーション ロジックを配置するモデル クラスは、単体テストを使用、ロジックを実行できることを意味します。 さらに、モデルの表示コントロールのナビゲーションに特定のビジネス ルールが適用されていることを確認するためのロジックを実装できます。 たとえば、アプリできないことがあります最初にページから移動するユーザー入力されたデータが有効であることを確認します。

A`NavigationService`クラスは、通常テストの容易性を昇格するモデルの表示から呼び出されます。 ただし、モデルの表示からビューへの移動には、参照ビュー、およびビューのアクティブなビュー モデルが関連付けられていない、これはお勧めしませんが特にへのビュー モデルが必要です。 したがって、`NavigationService`表示は、ここに移動するターゲットとしてビュー モデルの種類を指定します。

EShopOnContainers モバイル アプリの使用、`NavigationService`ビュー モデル優先のナビゲーションを提供するクラス。 このクラスは、実装、`INavigationService`インターフェイスは、次のコード例に示されています。

```csharp
public interface INavigationService  
{  
    ViewModelBase PreviousPageViewModel { get; }  
    Task InitializeAsync();  
    Task NavigateToAsync<TViewModel>() where TViewModel : ViewModelBase;  
    Task NavigateToAsync<TViewModel>(object parameter) where TViewModel : ViewModelBase;  
    Task RemoveLastFromBackStackAsync();  
    Task RemoveBackStackAsync();  
}
```

このインターフェイスは、実装するクラスが、次のメソッドを提供する必要がありますを指定します。

|メソッド|目的|
|--- |--- |
|`InitializeAsync`|アプリが起動されたときに、2 つのページのいずれかへのナビゲーションを実行します。|
|`NavigateToAsync`|指定したページへの階層の移動を実行します。|
|`NavigateToAsync(parameter)`|パラメーターを渡す、指定したページへの階層の移動を実行します。|
|`RemoveLastFromBackStackAsync`|ナビゲーション スタックから前のページを削除します。|
|`RemoveBackStackAsync`|ナビゲーション履歴から以前のすべてのページを削除します。|

さらに、`INavigationService`インターフェイスを実装するクラスを提供する必要がありますを指定します、`PreviousPageViewModel`プロパティです。 このプロパティは、ナビゲーション スタックで前のページに関連付けられているビューのモデルの種類を返します。

> [!NOTE]
> `INavigationService`インターフェイス本来あるべき指定も、`GoBackAsync`メソッドで、プログラムで、ナビゲーション スタックで前のページに返すために使用します。 ただし、このメソッドは eShopOnContainers モバイル アプリから不足しているため、その必要はありません。

### <a name="creating-the-navigationservice-instance"></a>NavigationService インスタンスを作成します。

`NavigationService`クラスを実装する、`INavigationService`インターフェイスでは、次のコード例に示すように、Autofac 依存性の注入コンテナーでシングルトンとして登録します。

```csharp
builder.RegisterType<NavigationService>().As<INavigationService>().SingleInstance();
```

`INavigationService`インターフェイスでは解決されて、`ViewModelBase`クラス コンス トラクター、次のコード例で示したようにします。

```csharp
NavigationService = ViewModelLocator.Resolve<INavigationService>();
```

参照が返されます、`NavigationService`によって作成される Autofac 依存関係挿入コンテナーに格納されているオブジェクト、`InitNavigation`メソッドで、`App`クラスです。 詳細については、次を参照してください。[を移動するときに、アプリが起動した](#navigating_when_the_app_is_launched)です。

`ViewModelBase`ストアをクラス、`NavigationService`インスタンス、`NavigationService`型のプロパティ、`INavigationService`です。 そのため、すべてのビューのモデルのクラスから派生する、`ViewModelBase`クラス、使用できる、`NavigationService`プロパティで指定されたメソッドへのアクセス、`INavigationService`インターフェイスです。 挿入のオーバーヘッドを回避できますこの、`NavigationService`各ビューのモデル クラスに Autofac 依存性の注入コンテナーからのオブジェクト。

### <a name="handling-navigation-requests"></a>ナビゲーション要求の処理

Xamarin.Forms では提供、 [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)をユーザーの forwards と backwards、必要に応じて、ページ間を移動することが階層的なナビゲーション操作を実装するクラス。 階層ナビゲーションの詳細については、「[階層ナビゲーション](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)」を参照してください。

使用するのではなく、 [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) eShopOnContainers アプリ ラップでクラスを直接、`NavigationPage`クラス内で、`CustomNavigationView`クラスに、次のコード例に示すようにします。

```csharp
public partial class CustomNavigationView : NavigationPage  
{  
    public CustomNavigationView() : base()  
    {  
        InitializeComponent();  
    }  

    public CustomNavigationView(Page root) : base(root)  
    {  
        InitializeComponent();  
    }  
}
```

この折り返しの目的は、スタイルが容易な[ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)クラスの XAML ファイル内のインスタンス。

ナビゲーションがビュー モデル クラス内のいずれかを呼び出すことによって実行される、`NavigateToAsync`メソッドは、次のコード例に示すように移動されているページのビュー モデルの型を指定します。

```csharp
await NavigationService.NavigateToAsync<MainViewModel>();
```

次のコード例は、`NavigateToAsync`によって提供されるメソッド、`NavigationService`クラス。

```csharp
public Task NavigateToAsync<TViewModel>() where TViewModel : ViewModelBase  
{  
    return InternalNavigateToAsync(typeof(TViewModel), null);  
}  

public Task NavigateToAsync<TViewModel>(object parameter) where TViewModel : ViewModelBase  
{  
    return InternalNavigateToAsync(typeof(TViewModel), parameter);  
}
```

各メソッドは、任意のビュー モデルから派生するクラスを使用する、`ViewModelBase`を呼び出すことによって階層的なナビゲーションを実行するクラス、`InternalNavigateToAsync`メソッドです。 さらに、2 番目`NavigateToAsync`メソッドにより、ナビゲーション データを移動先、使用されている場所通常初期化を実行するビュー モデルに渡される引数として指定できます。 詳細については、次を参照してください。[ナビゲーション中にパラメーターを渡す](#passing_parameters_during_navigation)です。

`InternalNavigateToAsync`メソッドは、ナビゲーション要求を実行し、次のコード例に示します。

```csharp
private async Task InternalNavigateToAsync(Type viewModelType, object parameter)  
{  
    Page page = CreatePage(viewModelType, parameter);  

    if (page is LoginView)  
    {  
        Application.Current.MainPage = new CustomNavigationView(page);  
    }  
    else  
    {  
        var navigationPage = Application.Current.MainPage as CustomNavigationView;  
        if (navigationPage != null)  
        {  
            await navigationPage.PushAsync(page);  
        }  
        else  
        {  
            Application.Current.MainPage = new CustomNavigationView(page);  
        }  
    }  

    await (page.BindingContext as ViewModelBase).InitializeAsync(parameter);  
}  

private Type GetPageTypeForViewModel(Type viewModelType)  
{  
    var viewName = viewModelType.FullName.Replace("Model", string.Empty);  
    var viewModelAssemblyName = viewModelType.GetTypeInfo().Assembly.FullName;  
    var viewAssemblyName = string.Format(  
                CultureInfo.InvariantCulture, "{0}, {1}", viewName, viewModelAssemblyName);  
    var viewType = Type.GetType(viewAssemblyName);  
    return viewType;  
}  

private Page CreatePage(Type viewModelType, object parameter)  
{  
    Type pageType = GetPageTypeForViewModel(viewModelType);  
    if (pageType == null)  
    {  
        throw new Exception($"Cannot locate page type for {viewModelType}");  
    }  

    Page page = Activator.CreateInstance(pageType) as Page;  
    return page;  
}
```

`InternalNavigateToAsync`メソッドは、最初の呼び出しによってビュー モデルへのナビゲーションを実行、`CreatePage`メソッドです。 このメソッドを指定されたビュー モデルの種類に対応し、作成してこのビューの種類のインスタンスを返す、ビューを検索します。 ビュー モデルの種類に対応するビューを検索する前提条件規則ベースのアプローチを使用します。

-   ビューは、ビュー モデルの型と同じアセンブリでです。
-   ビューは、します。ビューの子名前空間です。
-   モデルの表示は、します。ViewModels 子名前空間です。
-   「モデル」の削除とモデル名を表示するビューの名前が対応しています。

ビューがインスタンス化されるときに、対応するビュー モデルに関連付けられています。 このしくみの詳細については、次を参照してください。[ビュー モデル ロケーターにビュー モデルを自動的に作成する](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically_creating_a_view_model_with_a_view_model_locator)です。

作成されるビューの場合は、`LoginView`の新しいインスタンスの内部にラップ、`CustomNavigationView`クラスおよびに割り当てられている、 [ `Application.Current.MainPage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.MainPage/)プロパティです。 それ以外の場合、`CustomNavigationView`インスタンスの取得、および null でないことを指定、 [ `PushAsync` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)ナビゲーション スタック上に作成されるビューをプッシュするメソッドが呼び出されます。 ただし場合、取得した`CustomNavigationView`インスタンスが`null`の新しいインスタンス内に作成されているビューがラップされた、`CustomNavigationView`クラスおよびに割り当てられている、`Application.Current.MainPage`プロパティです。 このメカニズムによりナビゲーション中に、ページに追加されます正しくナビゲーション スタック、空であるときやデータが含まれている場合。

> [!TIP]
> ページのキャッシュを検討してください。 ページが現在表示されていないビューのメモリ使用量の結果をキャッシュします。 ただし、ページのキャッシュを使用しない、わけでは XAML の解析と、ページおよびそのビュー モデルの構築が発生するたびに新しいページへの移動が、複雑なページのパフォーマンスに影響があります。 過度に多くのコントロールを使用しない適切にデザインされたページのパフォーマンスを十分にする必要があります。 ただし、ページ キャッシュに役立つ低速ページ読み込み時間が発生した場合。

ビューが作成されに移動した後、`InitializeAsync`ビューの関連するビューのモデルのメソッドを実行します。 詳細については、次を参照してください。[ナビゲーション中にパラメーターを渡す](#passing_parameters_during_navigation)です。

<a name="navigating_when_the_app_is_launched" />

### <a name="navigating-when-the-app-is-launched"></a>移動するときに、アプリを起動します。

アプリが起動されたときに、`InitNavigation`メソッドで、`App`クラスが呼び出されます。 次のコード例では、このメソッドを示します。

```csharp
private Task InitNavigation()  
{  
    var navigationService = ViewModelLocator.Resolve<INavigationService>();  
    return navigationService.InitializeAsync();  
}
```

メソッドが新たに作成`NavigationService`Autofac 依存性の注入コンテナー内のオブジェクトを呼び出す前への参照を返しますとその`InitializeAsync`メソッドです。

> [!NOTE]
> ときに、`INavigationService`によってインターフェイスが解決される、`ViewModelBase`クラス、コンテナーへの参照を返します、 `NavigationService` InitNavigation メソッドが呼び出されたときに作成されたオブジェクト。

次のコード例は、 `NavigationService` `InitializeAsync`メソッド。

```csharp
public Task InitializeAsync()  
{  
    if (string.IsNullOrEmpty(Settings.AuthAccessToken))  
        return NavigateToAsync<LoginViewModel>();  
    else  
        return NavigateToAsync<MainViewModel>();  
}
```

`MainView`アプリの認証に使用されるキャッシュされたアクセス トークンがある場合に移動します。 それ以外の場合、`LoginView`へ移動します。

Autofac 依存性の注入コンテナーの詳細については、次を参照してください。[依存関係の挿入の概要](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection)です。

<a name="passing_parameters_during_navigation" />

### <a name="passing-parameters-during-navigation"></a>ナビゲーション中にパラメーターの引き渡し

1 つ、`NavigateToAsync`によって指定されたメソッド、`INavigationService`インターフェイス、移動先、使用されている場所通常初期化を実行するビュー モデルに渡される引数として指定する、ナビゲーション データ。

たとえば、`ProfileViewModel`クラスが含まれています、`OrderDetailCommand`でユーザーが注文を選択したときに実行される、`ProfileView`ページ。 さらに、実行し、実行、`OrderDetailAsync`メソッドは、次のコード例に示されています。

```csharp
private async Task OrderDetailAsync(Order order)  
{  
    await NavigationService.NavigateToAsync<OrderDetailViewModel>(order);  
}
```

このメソッドはへのナビゲーション、`OrderDetailViewModel`渡す、`Order`でユーザーが選択した順序を表すインスタンス、`ProfileView`ページ。 ときに、`NavigationService`クラスを作成、 `OrderDetailView`、`OrderDetailViewModel`クラスがインスタンス化され、ビューに割り当てられている[ `BindingContext`](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/)です。 移動した後、 `OrderDetailView`、`InternalNavigateToAsync`メソッドの実行、`InitializeAsync`ビューのメソッドのビュー モデルに関連します。

`InitializeAsync`でメソッドが定義されている、`ViewModelBase`クラスとしてメソッドをオーバーライドすることができます。 このメソッドを指定します、`object`ナビゲーション操作中に、ビュー モデルに渡されるデータを表す引数。 したがって、ナビゲーション操作からデータを受信するビューのモデル クラスを提供して独自の実装の`InitializeAsync`必要な初期化を実行するメソッド。 次のコード例は、`InitializeAsync`メソッドから、`OrderDetailViewModel`クラス。

```csharp
public override async Task InitializeAsync(object navigationData)  
{  
    if (navigationData is Order)  
    {  
        ...  
        Order = await _ordersService.GetOrderAsync(  
                        Convert.ToInt32(order.OrderNumber), authToken);  
        ...  
    }  
}
```

このメソッドは、取得、`Order`からナビゲーション操作中に、ビュー モデルに渡されたされ、それを使用してフルの順序を取得するインスタンスの詳細、`OrderService`インスタンス。

<a name="invoking_navigation_using_behaviors" />

### <a name="invoking-navigation-using-behaviors"></a>動作を使用して呼び出し元のナビゲーション

ナビゲーションは通常、ユーザーの操作によって、ビューからにトリガーされます。 たとえば、`LoginView`は次の認証が成功したナビゲーションを実行します。 次のコード例は、動作を使用して、ナビゲーションを呼び出す方法を示しています。

```xaml
<WebView ...>  
    <WebView.Behaviors>  
        <behaviors:EventToCommandBehavior  
            EventName="Navigating"  
            EventArgsConverter="{StaticResource WebNavigatingEventArgsConverter}"  
            Command="{Binding NavigateCommand}" />  
    </WebView.Behaviors>  
</WebView>
```

実行時に、`EventToCommandBehavior`との対話に応答する、 [ `WebView`](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/)です。 ときに、`WebView`で web ページに移動、 [ `Navigating` ](https://developer.xamarin.com/api/event/Xamarin.Forms.WebView.Navigating/)を実行するイベントは起動、`NavigateCommand`で、`LoginViewModel`です。 既定では、イベントのイベント引数は、コマンドに渡されます。 このデータは、ソースとターゲット間で指定された、コンバーターで渡される、変換、`EventArgsConverter`プロパティが返されます、 [ `Url` ](https://developer.xamarin.com/api/property/Xamarin.Forms.WebNavigationEventArgs.Url/)から、 [ `WebNavigatingEventArgs`](https://developer.xamarin.com/api/type/Xamarin.Forms.WebNavigatingEventArgs/)です。 したがって、ときに、`NavigationCommand`が実行すると、web ページの Url に渡されるパラメーターとして登録された`Action`です。

さらに、`NavigationCommand`実行、`NavigateAsync`メソッドは、次のコード例に示されています。

```csharp
private async Task NavigateAsync(string url)  
{  
    ...          
    await NavigationService.NavigateToAsync<MainViewModel>();  
    await NavigationService.RemoveLastFromBackStackAsync();  
    ...  
}
```

このメソッドはへのナビゲーション、 `MainViewModel`、し、次のナビゲーション、削除、`LoginView`ナビゲーション スタックからのページです。

### <a name="confirming-or-cancelling-navigation"></a>確認するか、ナビゲーションをキャンセルします。

アプリは、ユーザーのことを確認またはナビゲーションをキャンセルできるようにする、ナビゲーション操作中にユーザーと対話する必要があります。 これは、必要があります、たとえば、ユーザーがデータ入力ページが完全に完了する前に移動しようとするとします。 この状況では、アプリはにより、ユーザーは、ページから移動したり発生前にナビゲーション操作をキャンセルしたりすることを示す通知を提供する必要があります。 これは、ナビゲーションが呼び出されるかどうかを制御する通知からの応答を使用して、ビュー モデル クラスで実現できます。

## <a name="summary"></a>まとめ

Xamarin.Forms には、ページ ナビゲーションは、通常、UI でのユーザーの操作とは、アプリ自体のロジックに基づく内部の状態を変更した結果からの結果のサポートが含まれています。 ただし、ナビゲーションを MVVM パターンを使用するアプリケーションで実装する複雑になることができます。

この章の説明、`NavigationService`クラスは、モデルの表示からビュー モデル優先のナビゲーションを実行するために使用します。 ビューでナビゲーション ロジックを配置するモデルのクラスは、ロジックを自動テストを実行できることを意味します。 さらに、モデルの表示コントロールのナビゲーションに特定のビジネス ルールが適用されていることを確認するためのロジックを実装できます。


## <a name="related-links"></a>関連リンク

- [電子 (2 Mb PDF) のダウンロードします。](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (サンプル)](https://github.com/dotnet-architecture/eShopOnContainers)
