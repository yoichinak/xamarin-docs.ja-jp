---
title: エンタープライズ アプリのナビゲーション
description: この章では、eShopOnContainers のモバイル アプリがビュー モデルからビュー モデル優先のナビゲーションを実行する方法について説明します。
ms.prod: xamarin
ms.assetid: 4cad57b5-7fe4-4527-a988-d9b60c9620b4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: d306b0c1c0d08129671e27b96911ec771acb658e
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994771"
---
# <a name="enterprise-app-navigation"></a>エンタープライズ アプリのナビゲーション

Xamarin.Forms には、ページ ナビゲーションは、通常、UI を使用したユーザーの相互作用やロジックに基づく内部の状態を変更した結果、アプリ自体からの結果のサポートが含まれています。 ただし、ナビゲーションは複雑で、次の課題を満たす必要があると、モデル-ビュー-ビューモデル (MVVM) パターンを使用するアプリで実装できます。

-   密結合とビュー間の依存関係はいない方式を使用して、移動するビューを識別する方法。
-   ナビゲートするビューをインスタンス化および初期化するプロセスを調整する方法。 MVVM を使用する場合、ビューとビュー モデルをインスタンス化でき、ビューのバインド コンテキストを使用して、互いに関連付けられている必要があります。 アプリが依存関係の注入コンテナーを使用する場合、ビューとビュー モデルのインスタンス化は特定の構築のメカニズムを必要があります。
-   ビューの 1 つ目のナビゲーションを実行またはモデル優先のナビゲーションを表示するかどうか。 1 つ目のビューのナビゲーションで、ページに移動するは、ビューの種類の名前を表します。 ナビゲーション中に、指定されたビューはインスタンスを作成し、対応するビュー モデルとその他の依存サービス。 その他の方法では、場所に移動するページを参照して、ビュー モデルの型の名前をビュー モデル優先のナビゲーションを使用します。
-   方法を明確にするには、ビューとビュー モデルの間で、アプリのナビゲーション動作を区切ります。 MVVM パターンでは、アプリの UI、プレゼンテーション、およびビジネス ロジックを分離を提供します。 ただし、アプリのナビゲーション動作は多くの場合、アプリの UI とプレゼンテーションの部分を します。 ユーザーは、ビューからナビゲーションを開始して多くの場合、およびナビゲーションの結果として、ビューが置き換えられます。 ただし、ナビゲーションは開始されるか、ビュー モデル内の調整にも多くの場合、必要があります。
-   初期化のためにナビゲーション時にパラメーターを渡す方法。 たとえば、ユーザーは、注文の詳細を更新するためのビューに移動して、注文データが適切なデータを表示できるように、ビューに渡される必要があります。
-   座標のナビゲーション、特定のビジネス ルールを obeyed ことを確認する方法。 たとえば、ユーザーは、無効なデータを修正できますが、または送信するか、ビュー内で行われたデータの変更を破棄するように求められますようにビューを離れる前に求められます可能性があります。

この章ではこれらの課題を提示して説明を`NavigationService`ビュー モデルの最初のページ ナビゲーションを実行するために使用するクラス。

> [!NOTE]
> `NavigationService`で使用される、アプリが ContentPage インスタンス間の階層型ナビゲーションを実行するためだけに設計されています。 サービスを使用して、その他のページの種類間を移動すると、予期しない動作可能性があります。

## <a name="navigating-between-pages"></a>ページ間を移動

ナビゲーション ロジックでは、ビューの分離コード内に存在したり、データのビュー モデルをバインドすることができます。 ビューでナビゲーション ロジックを配置する最も簡単な方法があります、単体テストを簡単にテストできることはできません。 ビューでナビゲーション ロジックを配置するモデル クラスは、ロジックを単体テストを実行できることを意味します。 さらに、ビュー モデル コントロールへのナビゲーションを特定のビジネス ルールが適用されていることを確認するためのロジックを実装できます。 たとえば、アプリで可能性があります最初にページから移動するユーザーを許可しない入力されたデータが有効であることを確認します。

A`NavigationService`クラスが通常テストの容易性を促進する、ビュー モデルから呼び出されます。 ただし、ビュー モデルからビューに移動すると、ビュー モデルの参照ビュー、およびビューのアクティブ ビュー モデルが関連付けられていないは推奨されませんが特に必要があります。 そのため、`NavigationService`表示は、ここに移動するターゲットとしてビュー モデルの種類を指定します。

EShopOnContainers のモバイル アプリでは、`NavigationService`ビュー モデル優先のナビゲーションを提供するクラス。 このクラスは、実装、`INavigationService`インターフェイスは、次のコード例に示されています。

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

このインターフェイスを実装するクラスが、次のメソッドを提供する必要がありますを指定します。

|メソッド|目的|
|--- |--- |
|`InitializeAsync`|アプリを起動するときは、2 つのページのいずれかへのナビゲーションを実行します。|
|`NavigateToAsync`|指定したページを階層型ナビゲーションを実行します。|
|`NavigateToAsync(parameter)`|指定されたページで、パラメーターを渡すことを階層型ナビゲーションを実行します。|
|`RemoveLastFromBackStackAsync`|前のページをナビゲーション スタックから削除します。|
|`RemoveBackStackAsync`|前のすべてのページをナビゲーション スタックから削除します。|

さらに、`INavigationService`インターフェイスを実装するクラスを提供する必要がありますを指定します、`PreviousPageViewModel`プロパティ。 このプロパティは、ナビゲーション スタックには、前のページに関連付けられているビュー モデルの型を返します。

> [!NOTE]
> `INavigationService`インターフェイスは、通常、指定することも、`GoBackAsync`メソッドで、プログラムでナビゲーション スタックで前のページに返すために使用します。 ただし、このメソッドは不要であるため、eShopOnContainers のモバイル アプリから不足しています。

### <a name="creating-the-navigationservice-instance"></a>NavigationService のインスタンスを作成します。

`NavigationService`クラスを実装、`INavigationService`インターフェイスを次のコード例に示すように、Autofac 依存関係の注入コンテナーをシングルトンとして登録します。

```csharp
builder.RegisterType<NavigationService>().As<INavigationService>().SingleInstance();
```

`INavigationService`インターフェイスでは解決されて、`ViewModelBase`の次のコード例に示すクラスのコンス トラクター。

```csharp
NavigationService = ViewModelLocator.Resolve<INavigationService>();
```

参照が返されます、`NavigationService`によって作成される Autofac 依存関係挿入コンテナーに格納されているオブジェクト、`InitNavigation`メソッドで、`App`クラス。 詳細については、[を移動するときに、アプリが起動される](#navigating_when_the_app_is_launched)を参照してください。

`ViewModelBase`ストア クラス、`NavigationService`インスタンス、`NavigationService`型のプロパティ、`INavigationService`します。 そのため、すべてのビュー モデル クラスから派生する、`ViewModelBase`クラスを使用できる、`NavigationService`プロパティで指定されたメソッドへのアクセス、`INavigationService`インターフェイス。 挿入するオーバーヘッドを回避できますこの、`NavigationService`各ビュー モデル クラスに Autofac 依存関係挿入コンテナーからのオブジェクト。

### <a name="handling-navigation-requests"></a>ナビゲーション要求の処理

Xamarin.Forms の提供、 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage)をユーザーが forwards と backwards、必要に応じて、ページを移動できる階層ナビゲーション エクスペリエンスを実装するクラス。 階層ナビゲーションの詳細については、「[階層ナビゲーション](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)」を参照してください。

使用するのではなく、 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage)クラスを直接、eShopOnContainers アプリケーションのラップ、`NavigationPage`クラス、`CustomNavigationView`クラスに、次のコード例に示すように。

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

このラップの目的は、容易にするためのスタイル設定、 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage)クラスの XAML ファイル内のインスタンス。

ビュー モデル クラス内の 1 つを呼び出すことによってナビゲーションが実行される、`NavigateToAsync`メソッドは、次のコード例に示すように移動されているページのビュー モデルの種類を指定します。

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

各メソッドは、任意のビュー モデル クラスから派生した、`ViewModelBase`を呼び出すことによって階層型ナビゲーションを実行するクラス、`InternalNavigateToAsync`メソッド。 さらに、2 番目の`NavigateToAsync`メソッドは、これが通常使用されている初期化を実行するには、移動先のビュー モデルに渡される引数として指定するナビゲーション データを使用できます。 詳細については、[ナビゲーション中にパラメーターを渡す](#passing_parameters_during_navigation)を参照してください。

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

`InternalNavigateToAsync`メソッドは、最初の呼び出しでビュー モデルへの移動を実行、`CreatePage`メソッド。 このメソッドを指定されたビュー モデルの種類に対応し、作成してこのビューの種類のインスタンスを返すビューを検索します。 ビュー モデルの型に対応するビューを検索することが前提と規約ベースのアプローチを使用します。

-   ビューは、ビュー モデルの型と同じアセンブリでです。
-   ビューは、します。ビューの子名前空間。
-   ビュー モデルは、します。Viewmodel の子名前空間。
-   削除された「モデル」と、モデル名を表示するビューの名前が対応しています。

ビューがインスタンス化されるときに、対応するビュー モデルに関連付けします。 このしくみの詳細については、[自動的にビュー モデルを作成するビュー モデル ロケーターと](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically_creating_a_view_model_with_a_view_model_locator)を参照してください。

作成されるビューの場合は、 `LoginView`、内の新しいインスタンスでラップされます、`CustomNavigationView`クラスおよびに割り当てられている、 [ `Application.Current.MainPage` ](xref:Xamarin.Forms.Application.MainPage)プロパティ。 それ以外の場合、`CustomNavigationView`インスタンスの取得、および null でないことを指定、 [ `PushAsync` ](xref:Xamarin.Forms.NavigationPage)ナビゲーション スタックに作成されるビューをプッシュするメソッドが呼び出されます。 ただし場合、取得した`CustomNavigationView`インスタンスが`null`の新しいインスタンス内に作成されるビューがラップされた、`CustomNavigationView`クラスおよびに割り当てられている、`Application.Current.MainPage`プロパティ。 このメカニズムによりナビゲーション中に、ページが正しく追加ナビゲーション スタックに空である場合に、データが含まれている場合の両方。

> [!TIP]
> ページのキャッシュを検討してください。 ページが現在表示されていないビューのメモリ使用量の結果をキャッシュします。 ただし、ページのキャッシュなし、わけでは XAML を解析し、ページとそのビュー モデルの構築が発生するたびに新しいページにナビゲートすると、複雑なページのパフォーマンスに影響があります。 過剰な数のコントロールを使用していない適切に設計されたページのパフォーマンスは十分にあります。 ただし、ページがキャッシュに役立つ遅いページ読み込み時間が発生した場合。

ビューが作成されに移動した後、`InitializeAsync`ビューの関連するビュー モデルのメソッドが実行されます。 詳細については、[ナビゲーション中にパラメーターを渡す](#passing_parameters_during_navigation)を参照してください。

<a name="navigating_when_the_app_is_launched" />

### <a name="navigating-when-the-app-is-launched"></a>起動されるときにアプリを移動します。

アプリを起動すると、`InitNavigation`メソッドで、`App`クラスが呼び出されます。 次のコード例では、このメソッドは示しています。

```csharp
private Task InitNavigation()  
{  
    var navigationService = ViewModelLocator.Resolve<INavigationService>();  
    return navigationService.InitializeAsync();  
}
```

新しいメソッドを作成`NavigationService`Autofac 依存関係の注入コンテナー内のオブジェクトを呼び出す前への参照を返しますとその`InitializeAsync`メソッド。

> [!NOTE]
> ときに、`INavigationService`インターフェイスは解決、`ViewModelBase`クラス、コンテナーへの参照を返します、 `NavigationService` InitNavigation メソッドが呼び出されたときに作成されたオブジェクト。

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

`MainView`アプリには、キャッシュされたアクセス トークンは、認証に使用される場合に移動します。 それ以外の場合、`LoginView`にナビゲートするとします。

Autofac 依存関係の注入コンテナーの詳細については、[依存関係の挿入の概要](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection)を参照してください。

<a name="passing_parameters_during_navigation" />

### <a name="passing-parameters-during-navigation"></a>ナビゲーション中にパラメーターの引き渡し

1 つ、`NavigateToAsync`で指定されたメソッド、`INavigationService`インターフェイス、これが通常使用されている初期化を実行するには、移動先のビュー モデルに渡される引数として指定するナビゲーション データを有効にします。

たとえば、`ProfileViewModel`クラスが含まれています、 `OrderDetailCommand` 、ユーザーの注文を選択したときに実行される、`ProfileView`ページ。 これをさらに、実行、`OrderDetailAsync`メソッドは、次のコード例に示されています。

```csharp
private async Task OrderDetailAsync(Order order)  
{  
    await NavigationService.NavigateToAsync<OrderDetailViewModel>(order);  
}
```

このメソッドはへのナビゲーション、`OrderDetailViewModel`を渡して、`Order`インスタンスで、ユーザーが選択されている順序を表す、`ProfileView`ページ。 ときに、`NavigationService`クラスを作成、 `OrderDetailView`、`OrderDetailViewModel`クラスがインスタンス化され、ビューに割り当てられている[ `BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)します。 移動した後に、 `OrderDetailView`、`InternalNavigateToAsync`メソッドが実行される、`InitializeAsync`ビューのメソッドのビュー モデルに関連します。

`InitializeAsync`でメソッドが定義されている、`ViewModelBase`クラスとしてオーバーライド可能なメソッドです。 このメソッドを指定します、`object`ナビゲーション操作中に、ビュー モデルに渡されるデータを表す引数。 そのため、ナビゲーション操作からデータを受信するビュー モデル クラスがの独自の実装を提供、`InitializeAsync`必要な初期化を実行するメソッド。 次のコード例は、`InitializeAsync`からメソッド、`OrderDetailViewModel`クラス。

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

このメソッドは、取得、`Order`を完全な順序を取得する使用して、ナビゲーション操作中に、ビュー モデルに渡されたインスタンスの詳細から、`OrderService`インスタンス。

<a name="invoking_navigation_using_behaviors" />

### <a name="invoking-navigation-using-behaviors"></a>ビヘイビアーを使用して呼び出し側のナビゲーション

ナビゲーションは、通常はビューからユーザーの操作によってトリガーされます。 たとえば、`LoginView`次の認証が成功したナビゲーションを実行します。 次のコード例では、動作によって、ナビゲーションを呼び出す方法を示します。

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

実行時に、`EventToCommandBehavior`との対話に応答が、 [ `WebView`](xref:Xamarin.Forms.WebView)します。 ときに、 `WebView` 、web ページに移動する、 [ `Navigating` ](xref:Xamarin.Forms.WebView.Navigating)が実行されるイベントは起動、`NavigateCommand`で、`LoginViewModel`します。 既定では、イベントのイベント引数は、コマンドに渡されます。 このデータは、ソースとターゲット間で指定されたコンバーターによって渡される、変換、`EventArgsConverter`返すプロパティを[ `Url` ](xref:Xamarin.Forms.WebNavigationEventArgs.Url)から、 [ `WebNavigatingEventArgs`](xref:Xamarin.Forms.WebNavigatingEventArgs)します。 したがって、ときに、`NavigationCommand`が実行すると、web ページの Url に渡されるパラメーターとして、登録済み`Action`します。

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

このメソッドはへのナビゲーション、 `MainViewModel`、し、次のナビゲーションで、削除、`LoginView`ページをナビゲーション スタックから。

### <a name="confirming-or-cancelling-navigation"></a>確認するまたはナビゲーションをキャンセルします。

アプリは、ユーザーが確認またはナビゲーションをキャンセルできるように、ナビゲーション操作中にユーザーと対話する必要があります。 これは、必要があります、たとえば、ユーザーがデータ エントリのページが完全に完了する前に移動しようとしたとき。 このような状況で、アプリは、ことができる、ページから移動するかを起きる前に、ナビゲーション操作を取り消す通知を提供する必要があります。 これは、ナビゲーションが呼び出されるかどうかを制御する通知からの応答を使用して、ビュー モデル クラスで実現できます。

## <a name="summary"></a>まとめ

Xamarin.Forms には、ページ ナビゲーションは、通常、UI を使用したユーザーの相互作用やロジックに基づく内部の状態を変更した結果、アプリ自体からの結果のサポートが含まれています。 ただし、ナビゲーションは MVVM パターンを使用するアプリに実装する複雑になることができます。

この章の記載を`NavigationService`クラスは、ビュー モデルからビュー モデル優先のナビゲーションを実行するために使用します。 ビューでナビゲーション ロジックを配置するモデル クラスは、ロジックを自動テストを実行できることを意味します。 さらに、ビュー モデル コントロールへのナビゲーションを特定のビジネス ルールが適用されていることを確認するためのロジックを実装できます。


## <a name="related-links"></a>関連リンク

- [(2 Mb の PDF) 電子ブックをダウンロードします。](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (サンプル)](https://github.com/dotnet-architecture/eShopOnContainers)
