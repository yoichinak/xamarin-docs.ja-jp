---
title: エンタープライズアプリのナビゲーション
description: この章では、eShopOnContainers モバイルアプリがビューモデルからビューモデルファーストナビゲーションを実行する方法について説明します。
ms.prod: xamarin
ms.assetid: 4cad57b5-7fe4-4527-a988-d9b60c9620b4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 0f523c7149366cff85164f26f3f47b87801002cb
ms.sourcegitcommit: eedc6032eb5328115cb0d99ca9c8de48be40b6fa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/07/2020
ms.locfileid: "78916495"
---
# <a name="enterprise-app-navigation"></a>エンタープライズアプリのナビゲーション

Xamarin. フォームには、ページナビゲーションのサポートが含まれています。これは通常、ユーザーが UI を操作したとき、またはロジックに基づく状態が内部的に変更された結果としてアプリ自体から発生した場合に発生します。 ただし、次のような問題が満たされる必要があるため、ナビゲーションは、モデルビューモデル (MVVM) パターンを使用するアプリで実装するのが複雑になる可能性があります。

- ビュー間の密結合や依存関係を導入しない方法を使用して、移動先のビューを識別する方法。
- 移動先のビューをインスタンス化して初期化するプロセスを調整する方法。 MVVM を使用する場合、ビューとビューモデルは、ビューのバインドコンテキストを使用してインスタンス化され、相互に関連付けられている必要があります。 アプリが依存関係挿入コンテナーを使用している場合、ビューおよびビューモデルのインスタンス化には特定の構築メカニズムが必要になることがあります。
- ビュー優先ナビゲーションを実行するか、モデル優先ナビゲーションを表示するかを指定します。 ビュー優先ナビゲーションでは、移動先のページはビューの種類の名前を参照します。 ナビゲーション中に、指定されたビューがインスタンス化され、対応するビューモデルおよびその他の依存サービスもインスタンス化されます。 別の方法として、ビューモデル-first ナビゲーションを使用する方法があります。このナビゲーションでは、移動先のページはビューモデルの種類の名前を表します。
- ビューモデルとビューモデルでアプリのナビゲーション動作を明確に分離する方法。 MVVM パターンでは、アプリの UI とそのプレゼンテーションとビジネスロジックが分離されています。 ただし、アプリのナビゲーション動作は、多くの場合、アプリの UI とプレゼンテーションの部分にまたがります。 多くの場合、ユーザーはビューからナビゲーションを開始し、ナビゲーションの結果としてビューが置き換えられます。 ただし、通常、ナビゲーションはビューモデル内から開始または調整する必要がある場合もあります。
- ナビゲーション中に初期化のためにパラメーターを渡す方法。 たとえば、ユーザーが注文の詳細を更新するためにビューに移動した場合、正しいデータを表示できるように、注文データをビューに渡す必要があります。
- 特定のビジネスルールが確実に cache-control されるようにするための、ナビゲーションを併置する方法。 たとえば、無効なデータを修正したり、ビュー内で行われたデータの変更を送信または破棄するように求めるメッセージが表示されるように、ビューから移動する前にユーザーに確認を求めることができます。

この章では、ビューモデルの最初のページナビゲーションを実行するために使用される `NavigationService` クラスを提示することで、これらの課題に対処します。

> [!NOTE]
> アプリで使用される `NavigationService` は、ContentPage インスタンス間で階層ナビゲーションを実行するためだけに設計されています。 サービスを使用して他の種類のページ間を移動すると、予期しない動作が発生する可能性があります。

## <a name="navigating-between-pages"></a>ページ間の移動

ナビゲーションロジックは、ビューの分離コードまたはデータバインドビューモデルに配置できます。 ビューにナビゲーションロジックを配置するのは最も簡単な方法ですが、単体テストを使用して簡単にテストすることはできません。 ビューモデルクラスにナビゲーションロジックを配置すると、そのロジックを単体テストで実行できるようになります。 さらに、ビューモデルは、ナビゲーションを制御するロジックを実装して、特定のビジネスルールを確実に適用することができます。 たとえば、アプリでは、入力したデータが有効であることを最初に確認せずに、ユーザーがページから離れることを許可しない場合があります。

`NavigationService` クラスは、通常、テストの容易性を高めるためにビューモデルから呼び出されます。 ただし、ビューモデルからビューに移動するには、ビューモデルがビューを参照する必要があります。また、特に、アクティブなビューモデルが関連付けられていないビューを表示する必要があります。これは推奨されません。 したがって、ここに示されている `NavigationService` では、移動先としてビューモデルの種類を指定します。

EShopOnContainers モバイルアプリでは、`NavigationService` クラスを使用して、ビューモデルの最初のナビゲーションを提供します。 このクラスは、次のコード例に示すように、`INavigationService` インターフェイスを実装します。

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

|方法|目的|
|--- |--- |
|`InitializeAsync`|アプリが起動されると、2つのページのいずれかへの移動を実行します。|
|`NavigateToAsync`|指定されたページへの階層ナビゲーションを実行します。|
|`NavigateToAsync(parameter)`|パラメーターを渡して、指定されたページへの階層移動を実行します。|
|`RemoveLastFromBackStackAsync`|ナビゲーションスタックから前のページを削除します。|
|`RemoveBackStackAsync`|ナビゲーションスタックから、前のすべてのページを削除します。|

また、`INavigationService` インターフェイスは、実装するクラスが `PreviousPageViewModel` プロパティを提供する必要があることを指定します。 このプロパティは、ナビゲーションスタック内の前のページに関連付けられたビューモデルの型を返します。

> [!NOTE]
> `INavigationService` インターフェイスは通常、`GoBackAsync` メソッドも指定します。このメソッドは、ナビゲーションスタックの前のページにプログラムによって戻るために使用されます。 ただし、この方法は必須ではないため、eShopOnContainers モバイルアプリにはありません。

### <a name="creating-the-navigationservice-instance"></a>NavigationService インスタンスを作成しています

`INavigationService` インターフェイスを実装する `NavigationService` クラスは、次のコード例に示すように、Autofac 依存関係挿入コンテナーを使用してシングルトンとして登録されます。

```csharp
builder.RegisterType<NavigationService>().As<INavigationService>().SingleInstance();
```

`INavigationService` インターフェイスは、次のコード例に示すように、`ViewModelBase` クラスコンストラクターで解決されます。

```csharp
NavigationService = ViewModelLocator.Resolve<INavigationService>();
```

これにより、Autofac 依存関係挿入コンテナーに格納されている `NavigationService` オブジェクトへの参照が返されます。これは、`App` クラスの `InitNavigation` メソッドによって作成されます。 詳細については、「[アプリの起動時の移動](#navigating_when_the_app_is_launched)」を参照してください。

`ViewModelBase` クラスは、`NavigationService` インスタンスを `INavigationService`型の `NavigationService` プロパティに格納します。 したがって、`ViewModelBase` クラスから派生するすべてのビューモデルクラスは、`NavigationService` プロパティを使用して、`INavigationService` インターフェイスによって指定されたメソッドにアクセスできます。 これにより、Autofac 依存関係挿入コンテナーから各ビューモデルクラスに `NavigationService` オブジェクトを挿入するオーバーヘッドを回避できます。

### <a name="handling-navigation-requests"></a>ナビゲーション要求の処理

Xamarin には[`NavigationPage`](xref:Xamarin.Forms.NavigationPage)クラスが用意されています。このクラスは、ユーザーが必要に応じて、ページ間を移動したり、前後に移動したりできる階層型のナビゲーションエクスペリエンスを実装します。 階層ナビゲーションの詳細については、「[階層ナビゲーション](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)」を参照してください。

EShopOnContainers アプリは、 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)クラスを直接使用するのではなく、次のコード例に示すように、`CustomNavigationView` クラスの `NavigationPage` クラスをラップします。

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

このラップの目的は、クラスの XAML ファイル内の[`NavigationPage`](xref:Xamarin.Forms.NavigationPage)インスタンスのスタイルを簡単に設定することです。

ナビゲーションは、次のコード例に示すように、`NavigateToAsync` メソッドの1つを呼び出して、移動先のページのビューモデルの種類を指定して、ビューモデルクラス内で実行されます。

```csharp
await NavigationService.NavigateToAsync<MainViewModel>();
```

次のコード例は、`NavigationService` クラスによって提供される `NavigateToAsync` メソッドを示しています。

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

各メソッドでは、`InternalNavigateToAsync` メソッドを呼び出すことによって、`ViewModelBase` クラスから派生したすべてのビューモデルクラスで階層ナビゲーションを実行できます。 また、2つ目の `NavigateToAsync` メソッドを使用すると、ナビゲーションデータを、移動先のビューモデルに渡される引数として指定できます。この引数は通常、初期化の実行に使用されます。 詳細については、「[ナビゲーション中のパラメーターの引き渡し](#passing_parameters_during_navigation)」を参照してください。

`InternalNavigateToAsync` メソッドはナビゲーション要求を実行し、次のコード例に示します。

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

`InternalNavigateToAsync` メソッドは、最初に `CreatePage` メソッドを呼び出すことによって、ビューモデルへのナビゲーションを実行します。 このメソッドは、指定されたビューモデルの型に対応するビューを検索し、このビューの種類のインスタンスを作成して返します。 ビューモデルの種類に対応するビューを見つけるには、次のことを前提とした規約ベースのアプローチを使用します。

- ビューは、ビューモデルの種類と同じアセンブリに含まれています。
- ビューはにあります。子の名前空間を表示します。
- ビューモデルはに含まれています。Viewmodel child 名前空間。
- ビュー名は、モデル名の表示に対応し、"モデル" が削除されています。

ビューがインスタンス化されると、ビューは対応するビューモデルに関連付けられます。 この方法の詳細については、「[ビューモデルロケーターを使用したビューモデルの自動作成](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically_creating_a_view_model_with_a_view_model_locator)」を参照してください。

作成されるビューが `LoginView`である場合は、`CustomNavigationView` クラスの新しいインスタンス内にラップされ、 [`Application.Current.MainPage`](xref:Xamarin.Forms.Application.MainPage)プロパティに割り当てられます。 それ以外の場合は、`CustomNavigationView` インスタンスが取得され、null でない場合は、 [`PushAsync`](xref:Xamarin.Forms.NavigationPage)メソッドが呼び出されて、作成されるビューがナビゲーションスタックにプッシュされます。 ただし、取得した `CustomNavigationView` インスタンスが `null`場合、作成されるビューは `CustomNavigationView` クラスの新しいインスタンス内にラップされ、`Application.Current.MainPage` プロパティに割り当てられます。 このメカニズムにより、ナビゲーション中、ページが空である場合とデータが含まれている場合の両方で、ナビゲーションスタックに正しく追加されます。

> [!TIP]
> ページのキャッシュを検討してください。 ページキャッシュを使用すると、現在表示されていないビューのメモリ消費が発生します。 ただし、ページキャッシュを使用しない場合は、新しいページに移動するたびに、ページとそのビューモデルの XAML の解析と構築が行われることを意味します。これは、複雑なページのパフォーマンスに影響を与える可能性があります。 多数のコントロールを使用しない適切に設計されたページの場合は、パフォーマンスが十分である必要があります。 ただし、ページキャッシュは、ページの読み込みに時間がかかる場合に役立つ可能性があります。

ビューを作成して移動すると、ビューに関連付けられたビューモデルの `InitializeAsync` メソッドが実行されます。 詳細については、「[ナビゲーション中のパラメーターの引き渡し](#passing_parameters_during_navigation)」を参照してください。

<a name="navigating_when_the_app_is_launched" />

### <a name="navigating-when-the-app-is-launched"></a>アプリが起動されたときの移動

アプリが起動されると、`App` クラスの `InitNavigation` メソッドが呼び出されます。 以下のコード例はこのメソッドを示しています。

```csharp
private Task InitNavigation()  
{  
    var navigationService = ViewModelLocator.Resolve<INavigationService>();  
    return navigationService.InitializeAsync();  
}
```

メソッドは、Autofac 依存関係挿入コンテナーに新しい `NavigationService` オブジェクトを作成し、そのオブジェクトへの参照を返します。その後、`InitializeAsync` メソッドを呼び出します。

> [!NOTE]
> `INavigationService` インターフェイスが `ViewModelBase` クラスによって解決されると、このコンテナーは、InitNavigation メソッドが呼び出されたときに作成された `NavigationService` オブジェクトへの参照を返します。

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

アプリに認証に使用されるキャッシュされたアクセストークンがある場合、`MainView` に移動します。 それ以外の場合は、`LoginView` に移動します。

Autofac の依存関係挿入コンテナーの詳細については、「[依存関係の挿入の概要](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection)」を参照してください。

<a name="passing_parameters_during_navigation" />

### <a name="passing-parameters-during-navigation"></a>ナビゲーション中のパラメーターの引き渡し

`INavigationService` インターフェイスによって指定された `NavigateToAsync` メソッドの1つで、ナビゲーションデータを、移動先のビューモデルに渡される引数として指定できます。この引数は通常、初期化の実行に使用されます。

たとえば、`ProfileViewModel` クラスには、ユーザーが `ProfileView` ページで注文を選択したときに実行される `OrderDetailCommand` が含まれています。 次に、次のコード例に示すように、`OrderDetailAsync` メソッドを実行します。

```csharp
private async Task OrderDetailAsync(Order order)  
{  
    await NavigationService.NavigateToAsync<OrderDetailViewModel>(order);  
}
```

このメソッドは、`OrderDetailViewModel`へのナビゲーションを呼び出し、`ProfileView` ページでユーザーが選択した順序を表す `Order` インスタンスを渡します。 `NavigationService` クラスが `OrderDetailView`を作成すると、`OrderDetailViewModel` クラスがインスタンス化され、ビューの[`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)に割り当てられます。 `OrderDetailView`に移動した後、`InternalNavigateToAsync` メソッドはビューに関連付けられたビューモデルの `InitializeAsync` メソッドを実行します。

`InitializeAsync` メソッドは、オーバーライド可能なメソッドとして `ViewModelBase` クラスで定義されます。 このメソッドは、ナビゲーション操作中にビューモデルに渡されるデータを表す `object` 引数を指定します。 したがって、ナビゲーション操作からデータを受信するモデルクラスを表示するには、必要な初期化を実行するための `InitializeAsync` メソッドの独自の実装を提供します。 次のコード例は、`OrderDetailViewModel` クラスの `InitializeAsync` メソッドを示しています。

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

このメソッドは、ナビゲーション操作中にビューモデルに渡された `Order` インスタンスを取得し、それを使用して `OrderService` インスタンスから完全な注文の詳細を取得します。

<a name="invoking_navigation_using_behaviors" />

### <a name="invoking-navigation-using-behaviors"></a>動作を使用したナビゲーションの呼び出し

通常、ナビゲーションはユーザーの操作によってビューからトリガーされます。 たとえば、`LoginView` は、正常に認証された後にナビゲーションを実行します。 次のコード例は、動作によってナビゲーションがどのように呼び出されるかを示しています。

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

実行時には、`EventToCommandBehavior` は[`WebView`](xref:Xamarin.Forms.WebView)との対話に応答します。 `WebView` が web ページに移動すると、 [`Navigating`](xref:Xamarin.Forms.WebView.Navigating)イベントが発生し、`LoginViewModel`内の `NavigateCommand` が実行されます。 既定では、イベントのイベント引数がコマンドに渡されます。 このデータは、ソースとターゲットの間で渡されるときに、`EventArgsConverter` プロパティで指定されたコンバーターによって変換されます。これにより、 [`WebNavigatingEventArgs`](xref:Xamarin.Forms.WebNavigatingEventArgs)から[`Url`](xref:Xamarin.Forms.WebNavigationEventArgs.Url)が返されます。 したがって、`NavigationCommand` が実行されると、web ページの Url は、登録されている `Action`にパラメーターとして渡されます。

次のコード例に示すように、`NavigationCommand` によって `NavigateAsync` メソッドが実行されます。

```csharp
private async Task NavigateAsync(string url)  
{  
    ...          
    await NavigationService.NavigateToAsync<MainViewModel>();  
    await NavigationService.RemoveLastFromBackStackAsync();  
    ...  
}
```

このメソッドは `MainViewModel`へのナビゲーションを呼び出し、次のナビゲーションによってナビゲーションスタックから `LoginView` ページを削除します。

### <a name="confirming-or-cancelling-navigation"></a>ナビゲーションの確認または取り消し

ユーザーがナビゲーションを確認したりキャンセルしたりできるように、アプリはナビゲーション操作中にユーザーと対話することが必要になる場合があります。 たとえば、ユーザーがデータ入力ページを完全に完了する前に移動しようとした場合などに、この操作が必要になることがあります。 このような状況では、アプリは、ユーザーがページから移動したり、操作が行われる前にナビゲーション操作をキャンセルしたりすることを許可する通知を提供する必要があります。 これは、ナビゲーションが呼び出されるかどうかを制御する通知からの応答を使用して、ビューモデルクラスで実現できます。

## <a name="summary"></a>まとめ

Xamarin. フォームには、ページナビゲーションのサポートが含まれています。これは通常、ユーザーが UI と対話するか、アプリ自体から、ロジックに基づく状態の内部的な変更の結果として発生します。 ただし、MVVM パターンを使用するアプリでは、ナビゲーションが複雑になることがあります。

この章では、ビューモデルからビューモデルの最初のナビゲーションを実行するために使用される `NavigationService` クラスについて説明します。 ビューモデルクラスにナビゲーションロジックを配置すると、自動テストでロジックを実行できるようになります。 さらに、ビューモデルは、ナビゲーションを制御するロジックを実装して、特定のビジネスルールを確実に適用することができます。

## <a name="related-links"></a>関連リンク

- [電子ブックのダウンロード (2 Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (サンプル)](https://github.com/dotnet-architecture/eShopOnContainers)
