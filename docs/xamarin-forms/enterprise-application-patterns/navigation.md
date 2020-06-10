---
title: "エンタープライズアプリのナビゲーション" の説明: "この章では、eShopOnContainers モバイルアプリがビューモデルからビューモデルの最初のナビゲーションを実行する方法について説明します。"
ms. 製品: xamarin ms. assetid: 4cad57b5-7fe4-4527-a988-d9b60c9620b4: xamarin-forms author: davidbritch ms. author: dabritch ms. date: 08/07/2017 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="enterprise-app-navigation"></a>エンタープライズアプリのナビゲーション

Xamarin.Formsでは、ページナビゲーションがサポートされています。これは通常、ユーザーが UI を操作したとき、またはロジックに基づく状態が変更された結果としてアプリ自体から発生した場合に発生します。 ただし、次のような問題が満たされる必要があるため、ナビゲーションは、モデルビューモデル (MVVM) パターンを使用するアプリで実装するのが複雑になる可能性があります。

- ビュー間の密結合や依存関係を導入しない方法を使用して、移動先のビューを識別する方法。
- 移動先のビューをインスタンス化して初期化するプロセスを調整する方法。 MVVM を使用する場合、ビューとビューモデルは、ビューのバインドコンテキストを使用してインスタンス化され、相互に関連付けられている必要があります。 アプリが依存関係挿入コンテナーを使用している場合、ビューおよびビューモデルのインスタンス化には特定の構築メカニズムが必要になることがあります。
- ビュー優先ナビゲーションを実行するか、モデル優先ナビゲーションを表示するかを指定します。 ビュー優先ナビゲーションでは、移動先のページはビューの種類の名前を参照します。 ナビゲーション中に、指定されたビューがインスタンス化され、対応するビューモデルおよびその他の依存サービスもインスタンス化されます。 別の方法として、ビューモデル-first ナビゲーションを使用する方法があります。このナビゲーションでは、移動先のページはビューモデルの種類の名前を表します。
- ビューモデルとビューモデルでアプリのナビゲーション動作を明確に分離する方法。 MVVM パターンでは、アプリの UI とそのプレゼンテーションとビジネスロジックが分離されています。 ただし、アプリのナビゲーション動作は、多くの場合、アプリの UI とプレゼンテーションの部分にまたがります。 多くの場合、ユーザーはビューからナビゲーションを開始し、ナビゲーションの結果としてビューが置き換えられます。 ただし、通常、ナビゲーションはビューモデル内から開始または調整する必要がある場合もあります。
- ナビゲーション中に初期化のためにパラメーターを渡す方法。 たとえば、ユーザーが注文の詳細を更新するためにビューに移動した場合、正しいデータを表示できるように、注文データをビューに渡す必要があります。
- 特定のビジネスルールが確実に cache-control されるようにするための、ナビゲーションを併置する方法。 たとえば、無効なデータを修正したり、ビュー内で行われたデータの変更を送信または破棄するように求めるメッセージが表示されるように、ビューから移動する前にユーザーに確認を求めることができます。

この章では、 `NavigationService` ビューモデルの最初のページナビゲーションを実行するために使用されるクラスを提示することで、これらの課題に対処します。

> [!NOTE]
> `NavigationService`アプリで使用されるは、ContentPage インスタンス間で階層ナビゲーションを実行するためだけに設計されています。 サービスを使用して他の種類のページ間を移動すると、予期しない動作が発生する可能性があります。

## <a name="navigating-between-pages"></a>ページ間の移動

ナビゲーションロジックは、ビューの分離コードまたはデータバインドビューモデルに配置できます。 ビューにナビゲーションロジックを配置するのは最も簡単な方法ですが、単体テストを使用して簡単にテストすることはできません。 ビューモデルクラスにナビゲーションロジックを配置すると、そのロジックを単体テストで実行できるようになります。 さらに、ビューモデルは、ナビゲーションを制御するロジックを実装して、特定のビジネスルールを確実に適用することができます。 たとえば、アプリでは、入力したデータが有効であることを最初に確認せずに、ユーザーがページから離れることを許可しない場合があります。

クラスは、 `NavigationService` 通常、テストの容易性を高めるためにビューモデルから呼び出されます。 ただし、ビューモデルからビューに移動するには、ビューモデルがビューを参照する必要があります。また、特に、アクティブなビューモデルが関連付けられていないビューを表示する必要があります。これは推奨されません。 したがって、ここに示されているでは、 `NavigationService` 移動先としてビューモデルの種類を指定しています。

EShopOnContainers モバイルアプリはクラスを使用して、 `NavigationService` ビューモデルの最初のナビゲーションを提供します。 このクラスは、 `INavigationService` 次のコード例に示すように、インターフェイスを実装しています。

```csharp
public interface INavigationService  
{  
    ViewModelBase PreviousPageViewModel { get; }  
    Task InitializeAsync();  
    Task NavigateToAsync<TViewModel>() where TViewModel : ViewModelBase;  
    Task NavigateToAsync<TViewModel>(object parameter) where TViewModel : ViewModelBase;  
    Task RemoveLastFromBackStackAsync();  
    Task RemoveBackStackAsync();  
}
```

このインターフェイスは、実装するクラスが次のメソッドを提供する必要があることを指定します。

|Method|目的|
|--- |--- |
|`InitializeAsync`|アプリが起動されると、2つのページのいずれかへの移動を実行します。|
|`NavigateToAsync`|指定されたページへの階層ナビゲーションを実行します。|
|`NavigateToAsync(parameter)`|パラメーターを渡して、指定されたページへの階層移動を実行します。|
|`RemoveLastFromBackStackAsync`|ナビゲーションスタックから前のページを削除します。|
|`RemoveBackStackAsync`|ナビゲーションスタックから、前のすべてのページを削除します。|

さらに、インターフェイスは、 `INavigationService` 実装するクラスがプロパティを提供する必要があることを指定し `PreviousPageViewModel` ます。 このプロパティは、ナビゲーションスタック内の前のページに関連付けられたビューモデルの型を返します。

> [!NOTE]
> `INavigationService`インターフェイスは通常、メソッドも指定し `GoBackAsync` ます。このメソッドは、ナビゲーションスタックの前のページにプログラムで戻るために使用されます。 ただし、この方法は必須ではないため、eShopOnContainers モバイルアプリにはありません。

### <a name="creating-the-navigationservice-instance"></a>NavigationService インスタンスを作成しています

`NavigationService`インターフェイスを実装するクラスは、 `INavigationService` 次のコード例に示すように、Autofac 依存関係挿入コンテナーを使用してシングルトンとして登録されます。

```csharp
builder.RegisterType<NavigationService>().As<INavigationService>().SingleInstance();
```

`INavigationService` `ViewModelBase` 次のコード例に示すように、インターフェイスはクラスコンストラクターで解決されます。

```csharp
NavigationService = ViewModelLocator.Resolve<INavigationService>();
```

これに `NavigationService` より、Autofac 依存関係挿入コンテナーに格納されているオブジェクトへの参照が返されます。このコンテナーは、クラスのメソッドによって作成され `InitNavigation` `App` ます。 詳細については、「[アプリの起動時の移動](#navigating-when-the-app-is-launched)」を参照してください。

クラスは、 `ViewModelBase` `NavigationService` 型のプロパティにインスタンスを格納し `NavigationService` `INavigationService` ます。 したがって、クラスから派生するすべてのビューモデルクラス `ViewModelBase` は、プロパティを使用して、 `NavigationService` インターフェイスによって指定されたメソッドにアクセスでき `INavigationService` ます。 これにより、 `NavigationService` Autofac 依存関係挿入コンテナーから各ビューモデルクラスにオブジェクトを挿入するオーバーヘッドを回避できます。

### <a name="handling-navigation-requests"></a>ナビゲーション要求の処理

Xamarin.Formsクラスを提供します。このクラスは、 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) ユーザーが必要に応じて、ページ間を移動したり、前後に移動したりできる階層型のナビゲーションエクスペリエンスを実装します。 階層ナビゲーションの詳細については、「[階層ナビゲーション](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)」を参照してください。

[`NavigationPage`](xref:Xamarin.Forms.NavigationPage) `NavigationPage` `CustomNavigationView` 次のコード例に示すように、クラスを直接使用するのではなく、eShopOnContainers アプリがクラスのクラスをラップします。

```csharp
public partial class CustomNavigationView : NavigationPage  
{  
    public CustomNavigationView() : base()  
    {  
        InitializeComponent();  
    }  

    public CustomNavigationView(Page root) : base(root)  
    {  
        InitializeComponent();  
    }  
}
```

このラップの目的は、 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) クラスの XAML ファイル内のインスタンスを簡単にスタイル設定することです。

ナビゲーションは、次のコード例に示すように、いずれかのメソッドを呼び出して `NavigateToAsync` 、移動先のページのビューモデルの種類を指定して、ビューモデルクラス内で実行されます。

```csharp
await NavigationService.NavigateToAsync<MainViewModel>();
```

次のコード例は、 `NavigateToAsync` クラスによって提供されるメソッドを示してい `NavigationService` ます。

```csharp
public Task NavigateToAsync<TViewModel>() where TViewModel : ViewModelBase  
{  
    return InternalNavigateToAsync(typeof(TViewModel), null);  
}  

public Task NavigateToAsync<TViewModel>(object parameter) where TViewModel : ViewModelBase  
{  
    return InternalNavigateToAsync(typeof(TViewModel), parameter);  
}
```

各メソッドでは、クラスから派生するすべてのビューモデルクラスを使用し `ViewModelBase` て、メソッドを呼び出すことによって階層ナビゲーションを実行でき `InternalNavigateToAsync` ます。 さらに、2番目のメソッドでは、 `NavigateToAsync` ナビゲーションデータを、移動先のビューモデルに渡される引数として指定できます。この引数は、通常、初期化の実行に使用されます。 詳細については、「[ナビゲーション中のパラメーターの引き渡し](#passing-parameters-during-navigation)」を参照してください。

`InternalNavigateToAsync`メソッドはナビゲーション要求を実行し、次のコード例に示します。

```csharp
private async Task InternalNavigateToAsync(Type viewModelType, object parameter)  
{  
    Page page = CreatePage(viewModelType, parameter);  

    if (page is LoginView)  
    {  
        Application.Current.MainPage = new CustomNavigationView(page);  
    }  
    else  
    {  
        var navigationPage = Application.Current.MainPage as CustomNavigationView;  
        if (navigationPage != null)  
        {  
            await navigationPage.PushAsync(page);  
        }  
        else  
        {  
            Application.Current.MainPage = new CustomNavigationView(page);  
        }  
    }  

    await (page.BindingContext as ViewModelBase).InitializeAsync(parameter);  
}  

private Type GetPageTypeForViewModel(Type viewModelType)  
{  
    var viewName = viewModelType.FullName.Replace("Model", string.Empty);  
    var viewModelAssemblyName = viewModelType.GetTypeInfo().Assembly.FullName;  
    var viewAssemblyName = string.Format(  
                CultureInfo.InvariantCulture, "{0}, {1}", viewName, viewModelAssemblyName);  
    var viewType = Type.GetType(viewAssemblyName);  
    return viewType;  
}  

private Page CreatePage(Type viewModelType, object parameter)  
{  
    Type pageType = GetPageTypeForViewModel(viewModelType);  
    if (pageType == null)  
    {  
        throw new Exception($"Cannot locate page type for {viewModelType}");  
    }  

    Page page = Activator.CreateInstance(pageType) as Page;  
    return page;  
}
```

メソッドは、 `InternalNavigateToAsync` 最初にメソッドを呼び出して、ビューモデルへのナビゲーションを実行し `CreatePage` ます。 このメソッドは、指定されたビューモデルの型に対応するビューを検索し、このビューの種類のインスタンスを作成して返します。 ビューモデルの種類に対応するビューを見つけるには、次のことを前提とした規約ベースのアプローチを使用します。

- ビューは、ビューモデルの種類と同じアセンブリに含まれています。
- ビューはにあります。子の名前空間を表示します。
- ビューモデルはに含まれています。Viewmodel child 名前空間。
- ビュー名は、モデル名の表示に対応し、"モデル" が削除されています。

ビューがインスタンス化されると、ビューは対応するビューモデルに関連付けられます。 この方法の詳細については、「[ビューモデルロケーターを使用したビューモデルの自動作成](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically-creating-a-view-model-with-a-view-model-locator)」を参照してください。

作成されるビューがである場合は、 `LoginView` クラスの新しいインスタンス内にラップさ `CustomNavigationView` れ、プロパティに割り当てられ [`Application.Current.MainPage`](xref:Xamarin.Forms.Application.MainPage) ます。 それ以外の場合、インスタンスが取得され、null でない場合は、 `CustomNavigationView` [`PushAsync`](xref:Xamarin.Forms.NavigationPage) メソッドが呼び出されて、作成されたビューがナビゲーションスタックにプッシュされます。 ただし、取得されたインスタンスがの場合、 `CustomNavigationView` `null` 作成されるビューはクラスの新しいインスタンス内にラップされ、 `CustomNavigationView` プロパティに割り当てられ `Application.Current.MainPage` ます。 このメカニズムにより、ナビゲーション中、ページが空である場合とデータが含まれている場合の両方で、ナビゲーションスタックに正しく追加されます。

> [!TIP]
> ページのキャッシュを検討してください。 ページキャッシュを使用すると、現在表示されていないビューのメモリ消費が発生します。 ただし、ページキャッシュを使用しない場合は、新しいページに移動するたびに、ページとそのビューモデルの XAML の解析と構築が行われることを意味します。これは、複雑なページのパフォーマンスに影響を与える可能性があります。 多数のコントロールを使用しない適切に設計されたページの場合は、パフォーマンスが十分である必要があります。 ただし、ページキャッシュは、ページの読み込みに時間がかかる場合に役立つ可能性があります。

ビューを作成して移動すると、 `InitializeAsync` ビューに関連付けられたビューモデルのメソッドが実行されます。 詳細については、「[ナビゲーション中のパラメーターの引き渡し](#passing-parameters-during-navigation)」を参照してください。

### <a name="navigating-when-the-app-is-launched"></a>アプリが起動されたときの移動

アプリが起動されると、 `InitNavigation` クラスのメソッド `App` が呼び出されます。 以下のコード例はこのメソッドを示しています。

```csharp
private Task InitNavigation()  
{  
    var navigationService = ViewModelLocator.Resolve<INavigationService>();  
    return navigationService.InitializeAsync();  
}
```

メソッドは、 `NavigationService` Autofac 依存関係挿入コンテナーに新しいオブジェクトを作成し、メソッドを呼び出す前にそのオブジェクトへの参照を返し `InitializeAsync` ます。

> [!NOTE]
> `INavigationService`クラスによってインターフェイスが解決されると `ViewModelBase` 、コンテナーは、 `NavigationService` initnavigation メソッドが呼び出されたときに作成されたオブジェクトへの参照を返します。

次のコード例は、`NavigationService` `InitializeAsync` メソッドを示しています。

```csharp
public Task InitializeAsync()  
{  
    if (string.IsNullOrEmpty(Settings.AuthAccessToken))  
        return NavigateToAsync<LoginViewModel>();  
    else  
        return NavigateToAsync<MainViewModel>();  
}
```

アプリにキャッシュされた `MainView` アクセストークンがあり、認証に使用される場合は、に移動します。 それ以外の場合 `LoginView` は、に移動します。

Autofac の依存関係挿入コンテナーの詳細については、「[依存関係の挿入の概要](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction-to-dependency-injection)」を参照してください。

### <a name="passing-parameters-during-navigation"></a>ナビゲーション中のパラメーターの引き渡し

`NavigateToAsync`インターフェイスによって指定されたメソッドの1つでは、 `INavigationService` ナビゲーションデータを、移動先のビューモデルに渡される引数として指定できます。この引数は、通常、初期化の実行に使用されます。

たとえば、クラスには、 `ProfileViewModel` `OrderDetailCommand` ユーザーがページで注文を選択したときに実行されるが含まれてい `ProfileView` ます。 次に、 `OrderDetailAsync` 次のコード例に示すように、メソッドを実行します。

```csharp
private async Task OrderDetailAsync(Order order)  
{  
    await NavigationService.NavigateToAsync<OrderDetailViewModel>(order);  
}
```

このメソッドは、へのナビゲーションを呼び出し `OrderDetailViewModel` 、 `Order` ユーザーがページで選択した順序を表すインスタンスを渡し `ProfileView` ます。 クラスによってが作成されると、 `NavigationService` `OrderDetailView` `OrderDetailViewModel` クラスがインスタンス化され、ビューのに割り当てられ [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) ます。 に移動した後 `OrderDetailView` 、 `InternalNavigateToAsync` メソッドは `InitializeAsync` ビューに関連付けられたビューモデルのメソッドを実行します。

メソッドは、 `InitializeAsync` `ViewModelBase` オーバーライド可能なメソッドとしてクラスで定義されます。 このメソッドは、 `object` ナビゲーション操作中にビューモデルに渡されるデータを表す引数を指定します。 したがって、ナビゲーション操作からデータを受信するモデルクラスを表示するには、 `InitializeAsync` 必要な初期化を実行するメソッドの独自の実装を提供します。 次のコード例は、クラスのメソッドを示してい `InitializeAsync` `OrderDetailViewModel` ます。

```csharp
public override async Task InitializeAsync(object navigationData)  
{  
    if (navigationData is Order)  
    {  
        ...  
        Order = await _ordersService.GetOrderAsync(  
                        Convert.ToInt32(order.OrderNumber), authToken);  
        ...  
    }  
}
```

このメソッドは、 `Order` ナビゲーション操作中にビューモデルに渡されたインスタンスを取得し、それを使用してインスタンスから完全な順序の詳細を取得し `OrderService` ます。

### <a name="invoking-navigation-using-behaviors"></a>動作を使用したナビゲーションの呼び出し

通常、ナビゲーションはユーザーの操作によってビューからトリガーされます。 たとえば、は、 `LoginView` 正常に認証された後にナビゲーションを実行します。 次のコード例は、動作によってナビゲーションがどのように呼び出されるかを示しています。

```xaml
<WebView ...>  
    <WebView.Behaviors>  
        <behaviors:EventToCommandBehavior  
            EventName="Navigating"  
            EventArgsConverter="{StaticResource WebNavigatingEventArgsConverter}"  
            Command="{Binding NavigateCommand}" />  
    </WebView.Behaviors>  
</WebView>
```

実行時には、 `EventToCommandBehavior` との対話に応答し [`WebView`](xref:Xamarin.Forms.WebView) ます。 が `WebView` web ページに移動すると、 [`Navigating`](xref:Xamarin.Forms.WebView.Navigating) イベントが発生します。これにより、でが実行され `NavigateCommand` `LoginViewModel` ます。 既定では、イベントのイベント引数がコマンドに渡されます。 このデータは、プロパティで指定されたコンバーターによってソースとターゲットの間で渡されるときに変換されます。これにより、 `EventArgsConverter` からが返され [`Url`](xref:Xamarin.Forms.WebNavigationEventArgs.Url) [`WebNavigatingEventArgs`](xref:Xamarin.Forms.WebNavigatingEventArgs) ます。 このため、 `NavigationCommand` が実行されると、web ページの Url は、登録されたにパラメーターとして渡され `Action` ます。

`NavigationCommand` `NavigateAsync` 次のコード例に示すように、はメソッドを実行します。

```csharp
private async Task NavigateAsync(string url)  
{  
    ...          
    await NavigationService.NavigateToAsync<MainViewModel>();  
    await NavigationService.RemoveLastFromBackStackAsync();  
    ...  
}
```

このメソッドは、へのナビゲーションを呼び出し、次のナビゲーションによってナビゲーション `MainViewModel` `LoginView` スタックからページを削除します。

### <a name="confirming-or-cancelling-navigation"></a>ナビゲーションの確認または取り消し

ユーザーがナビゲーションを確認したりキャンセルしたりできるように、アプリはナビゲーション操作中にユーザーと対話することが必要になる場合があります。 たとえば、ユーザーがデータ入力ページを完全に完了する前に移動しようとした場合などに、この操作が必要になることがあります。 このような状況では、アプリは、ユーザーがページから移動したり、操作が行われる前にナビゲーション操作をキャンセルしたりすることを許可する通知を提供する必要があります。 これは、ナビゲーションが呼び出されるかどうかを制御する通知からの応答を使用して、ビューモデルクラスで実現できます。

## <a name="summary"></a>まとめ

Xamarin.Formsでは、ページナビゲーションがサポートされています。これは通常、ユーザーが UI と対話するか、アプリ自体から、ロジックドリブンの状態が変更された結果として発生します。 ただし、MVVM パターンを使用するアプリでは、ナビゲーションが複雑になることがあります。

この章では `NavigationService` 、ビューモデルからビューモデルの最初のナビゲーションを実行するために使用されるクラスについて説明します。 ビューモデルクラスにナビゲーションロジックを配置すると、自動テストでロジックを実行できるようになります。 さらに、ビューモデルは、ナビゲーションを制御するロジックを実装して、特定のビジネスルールを確実に適用することができます。

## <a name="related-links"></a>関連リンク

- [電子ブックのダウンロード (2 Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (サンプル)](https://github.com/dotnet-architecture/eShopOnContainers)
