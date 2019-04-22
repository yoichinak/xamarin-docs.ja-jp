---
title: モデル-ビュー-ビューモデル パターン
description: この章では、eShopOnContainers のモバイル アプリで、MVVM パターンを使用して、そのユーザー インターフェイスから、アプリのビジネスとプレゼンテーション ロジックを明確に分離する方法について説明します。
ms.prod: xamarin
ms.assetid: dd8c1813-df44-4947-bcee-1a1ff2334b87
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 87448c556c66ea086db70699848227e1f671792b
ms.sourcegitcommit: 3489c281c9eb5ada2cddf32d73370943342a1082
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/18/2019
ms.locfileid: "59019387"
---
# <a name="the-model-view-viewmodel-pattern"></a>モデル-ビュー-ビューモデル パターン

Xamarin.Forms の開発者エクスペリエンスには、通常、XAML でのユーザー インターフェイスを作成し、ユーザー インターフェイスで動作する分離コードが含まれます。 アプリでは、変更すると、し、規模と範囲が大きくなったり、複雑なメンテナンスの問題が発生することができます。 これらの問題には、UI コントロールと UI の変更、およびそのようなコードの単体テストの難しさのためのコストが増加するビジネス ロジックの間の密結合が含まれます。

モデル-ビュー-ビューモデル (MVVM) パターンは、そのユーザー インターフェイス (UI) からアプリケーションのビジネスとプレゼンテーション ロジックを明確に分離するのに役立ちます。 アプリケーション ロジックと UI の明確な分離を維持し、開発のさまざまな問題に対処するのに役立つ、し、アプリケーションのテスト、保守、および進化を簡単にします。 コードの再利用機会も大幅に向上し、により、開発者とより UI デザイナーは、それぞれの部品のアプリを開発するときに簡単に共同作業します。

## <a name="the-mvvm-pattern"></a>MVVM パターン

MVVM パターンでは次の 3 つのコア コンポーネントがある: モデル、ビュー、およびビュー モデルです。 それぞれには、個別の目的が機能します。 図 2-1 は、次の 3 つのコンポーネント間の関係を示しています。

![](mvvm-images/mvvm.png "MVVM パターン")

**図 2-1**:MVVM パターン

各コンポーネントの役割を理解するだけでなく、相互作用する方法を理解しておく必要がもします。 大まかに言えば、ビュー「認識」ビュー モデル、ビュー モデル「認識」のモデルがモデルはビュー モデルを認識しないとビュー モデルはビューの認識しません。 したがって、ビュー モデルは、モデルからビューを分離し、によりモデル、ビューに独立して進化を。

MVVM パターンを使用する利点は次のとおりです。

-   既存のビジネス ロジックをカプセル化する既存のモデルの実装がある場合は、困難または変更するリスクの高い、できます。 このシナリオでは、ビュー モデルは、モデル クラスのアダプターとして機能し、モデルのコードに主要な変更を加えるを回避することができます。
-   開発者は、ビューを使用せず、ビュー モデルとモデルの単体テストを作成できます。 ビュー モデルの単体テストでは、ビューで使用したのとまったく同じ機能を実行できます。
-   アプリの UI は、全体を XAML でビューが実装されていること、コードを変更することがなく再設計することができます。 そのため、ビューの新しいバージョンは、既存のビュー モデルを使用する必要があります。
-   設計者や開発者で作業できるとは独立して、同時に、コンポーネント開発プロセス中に。 デザイナーは、開発者は、ビュー モデルとモデルのコンポーネントで作業できるときに、ビューに専念できます。

正しいクラスにアプリのコードをファクタリングする方法を理解するうえで、クラスの相互作用を理解することで、MVVM を効果的に使用するキーがあります。 次のセクションでは、MVVM パターンのクラスのそれぞれの役割について説明します。

### <a name="view"></a>表示

ビューは、構造、レイアウト、およびユーザーが画面に表示の外観を定義する責任を負います。 理想的には、それぞれのビューは、限られた分離コードを含むビジネス ロジックを含まない、XAML で定義されます。 ただし、場合によっては、分離コードがアニメーションなど、XAML で表現するは困難を視覚的な動作を実装する UI ロジックを含めることがあります。

Xamarin.Forms アプリケーションのビューは通常、 [ `Page` ](xref:Xamarin.Forms.Page)-派生または[ `ContentView` ](xref:Xamarin.Forms.ContentView)-クラスを派生します。 ただし、ビューの視覚的に表示されているオブジェクトを表すために使用する UI 要素を指定するデータ テンプレートでは、表すこともできます。 ビューとして、データ テンプレートでは、任意コード分離を持たないし、特定のビュー モデルの種類にバインドしています。

> [!TIP]
> 有効にして、分離コードで UI 要素を無効化しないでください。 モデルの表示が、ビューの表示するかどうか、コマンドは、使用、または保留中の操作を示す値などのいくつかの側面に影響する論理状態の変更を定義する責任を負いますであることを確認します。 そのため、有効にし、モデルのプロパティではなく有効にして、分離コードで無効にすることを表示するバインドによって UI 要素を無効にします。

ボタンのクリックしてなど、対話は、ビューへの応答にビュー モデルでコードを実行または項目の選択のいくつかのオプションがあります。 コントロールは、コマンド、コントロールをサポートしている場合`Command`プロパティがデータにバインドできます、`ICommand`ビュー モデルのプロパティ。 コントロールのコマンドが呼び出されたときに、ビュー モデル内のコードが実行されます。 コマンドだけでなく動作は、ビュー内のオブジェクトに接続できるし、呼び出されるコマンドまたはイベントが発生するをリッスンできます。 応答して、動作は呼び出すことができますし、`ICommand`ビュー モデルまたはモデルの表示のメソッドにします。

### <a name="viewmodel"></a>ViewModel

ビュー モデルでは、プロパティやコマンド、ビューは、データ バインドを実装し、ビューに変更通知イベントを通じて状態変更を通知します。 プロパティとビュー モデルを提供するコマンドは、UI によって提供される機能を定義しますが、ビューは、その機能が表示される方法を決定します。

> [!TIP]
> UI の非同期操作の応答性を維持します。 モバイル アプリでは、ユーザーのパフォーマンスに対する認識を向上させるためにブロック解除、UI スレッドを保持する必要があります。 したがって、ビュー モデルで I/O 操作の非同期メソッドを使用して、プロパティの変更のビューに非同期的に通知するイベントを発生させます。

ビュー モデルは、ビューのやり取りに必要なすべてのモデル クラスを調整するためも担当します。 通常はビュー モデルとモデル クラス間の一対多リレーションシップです。 ビュー モデルは、それらに直接データをバインドできるようにすること、ビュー内のコントロール ビューに直接モデル クラスを公開することができます。 この場合、モデル クラスは、データ バインディングをサポートし、通知イベントを変更するよう設計する必要があります。

各ビュー モデルは、ビューを簡単に利用できる形式でのモデルからデータを提供します。 これを行うことがあります、ビュー モデルがデータ変換を実行します。 ビューにバインドできるプロパティを提供するためは、ビュー モデルでこのデータの変換を配置することをお勧めします。 たとえば、ビュー モデルはビューで表示を容易にできるように 2 つのプロパティの値を組み合わせることがあります。

> [!TIP]
> 変換レイヤーのデータ変換を一元化します。 ビュー モデルとビューの間に位置する別のデータ変換レイヤーとしてコンバーターを使用することもできます。 ときにできます必要に応じて、たとえば、データは、ビュー モデルを提供しない特殊な書式設定が必要です。

ビュー モデル、ビューとの双方向データ バインドに参加するためには、そのプロパティを上げる必要があります、`PropertyChanged`イベント。 ビュー モデルが実装することによってこの要件を満たす、`INotifyPropertyChanged`インターフェイス、および発生、`PropertyChanged`プロパティが変更されたときにイベント。

コレクションの場合、ビューに適した`ObservableCollection<T>`提供されます。 このコレクションには、コレクション変更通知、軽減を実装することから、開発者が実装して、`INotifyCollectionChanged`コレクション上のインターフェイス。

### <a name="model"></a>モデル

モデル クラスは、アプリのデータをカプセル化する非ビジュアル クラスです。 そのため、モデルは、通常、ビジネスおよび検証ロジックとデータ モデルが含まれるアプリのドメイン モデルを表すとして考えることができます。 モデル オブジェクトの例には、データ転送オブジェクト (Dto)、Plain Old CLR Object (Poco)、および生成されたエンティティおよびプロキシ オブジェクトが含まれます。

モデル クラスは通常、サービスまたはデータ アクセスとキャッシュをカプセル化するリポジトリと組み合わせて使用されます。

## <a name="connecting-view-models-to-views"></a>ビュー モデルをビューに接続します。

ビュー モデルは、Xamarin.Forms のデータ バインディング機能を使用して、ビューに接続できます。 ビューを構築し、モデルの表示し、実行時に関連付けるに使用できる多くの方法はあります。 これらの方法は、ビューの最初の構成とビュー モデルの最初の構成と呼ばれる 2 つのカテゴリに分類されます。 ビューの最初の構成とモデルの最初の合成 基本設定と複雑さの問題は、ビューの選択。 ただし、すべての方法は、その BindingContext プロパティに割り当てられているビュー モデルがビューには同じという目的を共有します。

ビュー最初コンポジション、アプリは概念的にはビューに依存しているビュー モデルに接続するので構成されます。 このアプローチの主な利点ことですがの疎結合を作成する簡単な単体テストが容易なアプリのビュー モデルはビュー自体への依存を持たないためです。 アプリの構造を理解するには、クラスを作成し、関連付けられている方法を理解するコードが実行を追跡する必要がなく、その視覚的な構造を次に簡単です。 さらに、ビューの最初の構築は複雑で、プラットフォームとのずれは、ビュー モデルの最初の構成をナビゲーションが発生したときにページの構築を担当する Xamarin.Forms ナビゲーション システムを配置します。

ビュー モデルの最初の合成、アプリは概念的にはで構成されますビュー モデル、ビュー モデルのビューを検索するために縛られるサービスと。 ビュー モデルの最初の構成は、ビューを作成できますが抽象、アプリの UI 以外の論理構造に重点を置くようにするため、一部の開発者にとってより自然なと考えています。 さらに、他のビュー モデルによって作成されるモデルの表示を許可します。 ただし、この方法は複雑な多くの場合、アプリのさまざまな部分の作成し、関連付けられている方法を理解するが困難になることができます。

> [!TIP]
> ビュー モデルとビューを独立して保持します。 データ ソースのプロパティをビューのバインドで、対応するビュー モデル、ビューのプリンシパルの依存関係があります。 など、ビューの種類を参照しない具体的には、 [ `Button` ](xref:Xamarin.Forms.Button)と[ `ListView` ](xref:Xamarin.Forms.ListView)、ビュー モデルから。 ここで説明した原則に従うと、分離、スコープを制限することで、ソフトウェアの不具合が発生する可能性を軽減するためにビュー モデルをテストできます。

次のセクションでは、ビュー モデルをビューに接続するための主な方法について説明します。

### <a name="creating-a-view-model-declaratively"></a>宣言によって、ビュー モデルを作成します。

最も簡単な方法は、宣言的 XAML 内の対応するビュー モデルをインスタンス化するビュー用です。 ビューが作成されるときに、対応するビュー モデル オブジェクトに構築することもできます。 このアプローチは、次のコード例について説明します。

```xaml
<ContentPage ... xmlns:local="clr-namespace:eShop">  
    <ContentPage.BindingContext>  
        <local:LoginViewModel />  
    </ContentPage.BindingContext>  
    ...  
</ContentPage>
```

ときに、 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)は、インスタンスを作成、`LoginViewModel`が自動的に構築され、ビューの設定[ `BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)です。

この宣言型の構築と割り当てビューでビュー モデルの言うは単純ですが、ビュー モデル内の既定 (パラメーターのない) コンス トラクターが必要であるという欠点ことの利点があります。

### <a name="creating-a-view-model-programmatically"></a>ビュー モデルをプログラムで作成します。

ビューに割り当てられているモデルの表示になる分離コード ファイルでコードを持つことができます、 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext)プロパティ。 多くの場合、これは、ビューのコンス トラクターで次のコード例に示すように。

```csharp
public LoginView()  
{  
    InitializeComponent();  
    BindingContext = new LoginViewModel(navigationService);  
}
```

プログラムによる構築と、ビューの分離コード内でビュー モデルの割り当てには、単純であるという利点があります。 ただし、このアプローチの主な欠点は、ビューが必要な依存関係にビュー モデルを提供する必要があることにします。 依存関係の注入コンテナーを使用して疎結合、ビューとビュー モデルを維持するために役立ちます。 詳細については、次を参照してください。[依存関係の注入](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md)します。

### <a name="creating-a-view-defined-as-a-data-template"></a>データ テンプレートとして定義されているビューを作成します。

ビューは、データ テンプレートとして定義されているし、ビュー モデルの種類に関連付けられていることができます。 データ テンプレートは、リソースとして定義できますか、ビュー モデルを表示するコントロール内にインラインで定義されている可能性があります。 コントロールのコンテンツは、ビュー モデルのインスタンスと、データ テンプレートを視覚的に表現するために使用します。 この手法をビュー モデルがインスタンス化される最初に、ビューの作成後に状況の例に示します。

<a name="automatically_creating_a_view_model_with_a_view_model_locator" />

### <a name="automatically-creating-a-view-model-with-a-view-model-locator"></a>ビュー モデルのロケーターを自動的にビュー モデルを作成します。

ビュー モデルのロケーターは、ビュー モデルとビューへの関連付けのインスタンス化を管理するカスタム クラスです。 EShopOnContainers のモバイル アプリで、`ViewModelLocator`クラスには、添付プロパティ`AutoWireViewModel`、ビュー モデルをビューに関連付けるために使用されます。 ビューの XAML では、この添付プロパティは次のコード例で示すように、ビュー モデルが、ビューに自動的に接続されているを示す場合は true に設定します。

```xaml
viewModelBase:ViewModelLocator.AutoWireViewModel="true"
```

`AutoWireViewModel`プロパティは、バインド可能なプロパティを false に初期化して、その値が変更されたとき、`OnAutoWireViewModelChanged`イベント ハンドラーが呼び出されます。 このメソッドは、ビューのビュー モデルを解決します。 次のコード例では、これを実現する方法を示します。

```csharp
private static void OnAutoWireViewModelChanged(BindableObject bindable, object oldValue, object newValue)  
{  
    var view = bindable as Element;  
    if (view == null)  
    {  
        return;  
    }  

    var viewType = view.GetType();  
    var viewName = viewType.FullName.Replace(".Views.", ".ViewModels.");  
    var viewAssemblyName = viewType.GetTypeInfo().Assembly.FullName;  
    var viewModelName = string.Format(  
        CultureInfo.InvariantCulture, "{0}Model, {1}", viewName, viewAssemblyName);  

    var viewModelType = Type.GetType(viewModelName);  
    if (viewModelType == null)  
    {  
        return;  
    }  
    var viewModel = _container.Resolve(viewModelType);  
    view.BindingContext = viewModel;  
}
```

`OnAutoWireViewModelChanged`メソッドは規約ベースのアプローチを使用して、ビュー モデルを解決しようとしています。 この規則には、ことが前提とします。

-   ビュー モデルは、ビューの種類と同じアセンブリでです。
-   ビューは、します。ビューの子名前空間。
-   ビュー モデルは、します。Viewmodel の子名前空間。
-   ビュー モデルの名前は、ビューの名前と対応して、"ViewModel"で終わります。

最後に、`OnAutoWireViewModelChanged`メソッドのセット、 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext)解決済みのビュー モデルの種類にビューの種類の。 ビュー モデルの型を解決する方法の詳細については、次を参照してください。[解決](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#resolution)します。

このアプローチでは、アプリは、ビュー モデルとビューへの接続のインスタンス化を担当する 1 つのクラスであるという利点があります。

> [!TIP]
> 置換の容易にするためには、ビュー モデルのロケーターを使用します。 ビュー モデル ロケーターこともできます、依存関係の代替実装のための置換のポイントとしてなどの単体テストまたはデザイン時のデータ。

## <a name="updating-views-in-response-to-changes-in-the-underlying-view-model-or-model"></a>モデルまたはモデルを表示する、基になる変更に応じて更新ビュー

すべてのビュー モデルとビューにアクセス可能であるモデル クラスを実装する必要があります、`INotifyPropertyChanged`インターフェイス。 基になるプロパティの値が変更されたときに、ビューでデータ バインド コントロールに変更通知を提供するクラスは、ビュー モデルまたはモデルのクラスでこのインターフェイスを実装できます。

次の要件を満たすことによってプロパティの変更通知の正しい使用のアプリを設計する必要があります。

-   常に発生させる、`PropertyChanged`イベント パブリック プロパティの値が変更された場合。 発生すると想定しないで、`PropertyChanged`イベントは、サポート技術情報の XAML バインドが発生したため無視できます。
-   常に発生させる、`PropertyChanged`計算される値をビュー内の他のプロパティを使用するプロパティのいずれかのイベント モデルまたはモデル。
-   常に発生させる、`PropertyChanged`により、プロパティの変更、または安全な状態にあるオブジェクトを認識すると、メソッドの最後のイベント。 イベントを発生させるイベントのハンドラーを同期的に呼び出すことによって、操作を中断します。 操作の途中でこのような場合、安全でない、部分的に更新された状態である場合に、コールバック関数へのオブジェクトを公開可能性があります。 カスケード型の変更によってトリガーされる可能性がさらに、`PropertyChanged`イベント。 カスケード変更は、カスケード型の変更を安全に実行する前に完了する更新プログラムを一般に必要です。
-   発生させることはありません、`PropertyChanged`イベント、プロパティが変更されない場合。 つまり、発生する前に新旧の値を比較する必要があります、`PropertyChanged`イベント。
-   発生させることはありません、`PropertyChanged`プロパティを初期化する場合、ビュー モデルのコンス トラクターの中にイベント。 ビューのデータ バインド コントロールはこの時点で変更通知の受信をサブスクライブしてがいません。
-   1 つ以上の発生しない`PropertyChanged`イベント クラスのパブリック メソッドの 1 つの同期呼び出し内で同じプロパティ名の引数とします。 たとえば、`NumberOfItems`プロパティのバッキング ストアとして、`_numberOfItems`フィールド、メソッドの増分の場合`_numberOfItems`50 回ループの実行中にのみ発生させるプロパティの変更通知で、`NumberOfItems`プロパティを 1 回、すべての作業が完了します。 非同期メソッドは、生成、`PropertyChanged`イベントを非同期継続チェーンの各同期セグメントに指定されたプロパティ名。

EShopOnContainers のモバイル アプリでは、`ExtendedBindableObject`クラスを提供する変更通知、次のコード例に表示されます。

```csharp
public abstract class ExtendedBindableObject : BindableObject  
{  
    public void RaisePropertyChanged<T>(Expression<Func<T>> property)  
    {  
        var name = GetMemberInfo(property).Name;  
        OnPropertyChanged(name);  
    }  

    private MemberInfo GetMemberInfo(Expression expression)  
    {  
        ...  
    }  
}
```

Xamarin.Form の[ `BindableObject` ](xref:Xamarin.Forms.BindableObject)クラスが実装する、`INotifyPropertyChanged`インターフェイスを備え、 [ `OnPropertyChanged` ](xref:Xamarin.Forms.BindableObject.OnPropertyChanged(System.String))メソッド。 `ExtendedBindableObject`クラスには、`RaisePropertyChanged`プロパティを呼び出すメソッドを選択し、変更の通知をによって提供される機能を使用、`BindableObject`クラス。

EShopOnContainers のモバイル アプリでの各ビュー モデル クラスから派生、`ViewModelBase`から派生するクラスを`ExtendedBindableObject`クラス。 そのため、各ビュー モデル クラスを使用して、`RaisePropertyChanged`メソッドで、`ExtendedBindableObject`プロパティの変更通知を提供するクラス。 次のコード例では、eShopOnContainers のモバイル アプリがラムダ式を使用してプロパティの変更通知を起動する方法を示します。

```csharp
public bool IsLogin  
{  
    get  
    {  
        return _isLogin;  
    }  
    set  
    {  
        _isLogin = value;  
        RaisePropertyChanged(() => IsLogin);  
    }  
}
```

ラムダ式を使用して、この方法でわずかなパフォーマンスの各呼び出しに対して評価されるラムダ式があるため、コストの含まれることに注意してください。 パフォーマンス コストが小さいと、アプリが通常影響しない、ですが、多くの変更通知がある場合に、コストが発生することができます。 ただし、このアプローチの利点は、コンパイル時の型の安全性とプロパティを変更するとリファクタリングのサポートが提供されることです。

## <a name="ui-interaction-using-commands-and-behaviors"></a>コマンドとビヘイビアーを使用する UI 操作

モバイル アプリでのアクションは通常、分離コード ファイルで、イベント ハンドラーを作成して実装できるボタンのクリックなどのユーザー アクションへの応答で呼び出されます。 ただし、MVVM パターンでビュー モデルでは、アクションを実装する責任があるし、分離コードで配置するコードを避ける必要があります。

コマンドは、UI コントロールにバインドできるアクションを表す便利な方法を提供します。 アクションを実装するコードをカプセル化され、ビューでのビジュアル表現から切り離すことに保つために役立ちます。 Xamarin.Forms には、コマンドを宣言によって接続されているコントロールが含まれていて、ユーザーがコントロールと対話するときにこれらのコントロールがコマンドを呼び出します。

動作では、コントロール宣言によって、コマンドに接続することもできます。 ただし、動作は、さまざまなコントロールによって発生したイベントに関連付けられているアクションを呼び出すために使用できます。 そのため、動作の多くに対処コマンドが有効なコントロールと同じシナリオよりはるかに柔軟性と制御を提供しながらします。 さらに、動作は、コマンドとの対話を具体的にはデザインされていないコントロールにコマンド オブジェクトやメソッドを関連付けることも使用できます。

### <a name="implementing-commands"></a>コマンドの実装

ビュー モデルは通常を実装するオブジェクトのインスタンスであるビューで、バインドのコマンド プロパティを公開、`ICommand`インターフェイス。 Xamarin.Forms コントロールの多くを提供して、`Command`プロパティは、データにバインドされて、`ICommand`ビュー モデルが提供するオブジェクト。 `ICommand`インターフェイスを定義、`Execute`操作自体をカプセル化するメソッド、`CanExecute`メソッドをおよびコマンドを呼び出すことができるかどうかを示す`CanExecuteChanged`かどうかの変更に影響するが発生したときに発生するイベントコマンドを実行する必要があります。 [ `Command` ](xref:Xamarin.Forms.Command)と[ `Command<T>` ](xref:Xamarin.Forms.Command) 、Xamarin.Forms が提供するクラスの実装、`ICommand`インターフェイス、場所`T`への引数の型は、`Execute`と`CanExecute`します。

ビュー モデル内で必要があります型のオブジェクト[ `Command` ](xref:Xamarin.Forms.Command)または[ `Command<T>` ](xref:Xamarin.Forms.Command)型のビュー モデル内の各パブリック プロパティの`ICommand`します。 `Command`または`Command<T>`コンス トラクターが必要です、`Action`コールバック オブジェクトを呼び出したときに、`ICommand.Execute`メソッドが呼び出されます。 `CanExecute`は省略可能なコンス トラクターのパラメーターを`Func`を返す、`bool`します。

次のコードは、 [ `Command` ](xref:Xamarin.Forms.Command)へのデリゲートを指定することで、登録コマンドを表すインスタンスが構築された、`Register`モデルのメソッドを表示します。

```csharp
public ICommand RegisterCommand => new Command(Register);
```

参照を返すプロパティを通じてビューに、コマンドが公開されている、`ICommand`します。 ときに、`Execute`でメソッドが呼び出される、 [ `Command` ](xref:Xamarin.Forms.Command)オブジェクトで指定されたデリゲートを使用して、ビュー モデル内のメソッドの呼び出しを転送するだけ、`Command`コンス トラクター。

使用して、コマンドによって、非同期メソッドを呼び出すことができます、`async`と`await`キーワードのコマンドを指定するときに`Execute`を委任します。 これは、コールバックがあることを示します、`Task`待機する必要があります。 たとえば、次のコードはどのように、 [ `Command` ](xref:Xamarin.Forms.Command)へのデリゲートを指定することによって、サインイン コマンドを表すインスタンスが構築された、`SignInAsync`モデルのメソッドを表示。

```csharp
public ICommand SignInCommand => new Command(async () => await SignInAsync());
```

パラメーターに渡すことができます、`Execute`と`CanExecute`アクションを使用して、 [ `Command<T>` ](xref:Xamarin.Forms.Command)コマンドをインスタンス化するクラス。 たとえば、次のコードはどのように、`Command<T>`インスタンスがあることを示す使用、`NavigateAsync`メソッドに必要な型の引数`string`:

```csharp
public ICommand NavigateCommand => new Command<string>(NavigateAsync);
```

両方で、 [ `Command` ](xref:Xamarin.Forms.Command)と[ `Command<T>` ](xref:Xamarin.Forms.Command)クラス、デリゲート、`CanExecute`各コンス トラクターのメソッドは省略可能です。 デリゲートが指定されていない場合、`Command`戻ります`true`の`CanExecute`します。 ただし、ビュー モデルは、コマンドの変化を示すことができます`CanExecute`ステータスを呼び出すことによって、`ChangeCanExecute`メソッドを`Command`オブジェクト。 これにより、`CanExecuteChanged`イベントが発生します。 コマンドにバインドされているコントロールが UI には、データ バインドされたコマンドの可用性を反映するように、有効な状態を更新し、されます。

#### <a name="invoking-commands-from-a-view"></a>ビューからのコマンドを呼び出す

次のコード例に示す方法、 [ `Grid` ](xref:Xamarin.Forms.Grid)で、`LoginView`にバインドする、`RegisterCommand`で、`LoginViewModel`クラスを使用して、 [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer)インスタンス。

```xaml
<Grid Grid.Column="1" HorizontalOptions="Center">  
    <Label Text="REGISTER" TextColor="Gray"/>  
    <Grid.GestureRecognizers>  
        <TapGestureRecognizer Command="{Binding RegisterCommand}" NumberOfTapsRequired="1" />  
    </Grid.GestureRecognizers>  
</Grid>
```

コマンドのパラメーター定義することも必要に応じてを使用して、 [ `CommandParameter` ](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter)プロパティ。 必要な引数の型がで指定された、`Execute`と`CanExecute`メソッドを対象します。 [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer)ユーザーが接続されているコントロール ターゲット コマンドを自動的に呼び出します。 コマンド パラメーターを指定されている場合は、コマンドの引数として渡されます`Execute`を委任します。

<a name="implementing_behaviors" />

### <a name="implementing-behaviors"></a>動作を実装します。

動作は、それらをサブクラス化しなくても、UI コントロールに追加する機能を使用できます。 代わりに、その機能はビヘイビアー クラスで実装され、それがコントロール自体の一部であるかのようにコントロールにアタッチされます。 動作を簡潔に、コントロールにアタッチできパッケージには、複数のビューまたはアプリの間で再利用方法で、コントロールの API を直接操作するため、分離コードとして記述する必要が通常のコードを実装できます。 MVVM のコンテキストでは、動作は、コマンドにコントロールを接続するために役立つアプローチです。

添付プロパティをコントロールに関連付けられている動作と呼ばれる、*動作をアタッチ*します。 動作は、それがアタッチされている機能を制御する、またはビューのビジュアル ツリー内の他のコントロールに追加する要素の公開されている API を使用できます。 EShopOnContainers のモバイル アプリが含まれる、`LineColorBehavior`クラスは、これは、接続されている動作です。 この動作の詳細については、次を参照してください。[検証エラーを表示する](~/xamarin-forms/enterprise-application-patterns/validation.md#displaying_validation_errors)します。

Xamarin.Forms の動作はから派生したクラス、 [ `Behavior` ](xref:Xamarin.Forms.Behavior)または[ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1)クラス、`T `動作を適用するコントロールの種類です。 これらのクラスを提供`OnAttachedTo`と`OnDetachingFrom`メソッドで、動作にアタッチされているし、コントロールからデタッチされたときに実行するロジックを提供するオーバーライドする必要があります。

EShopOnContainers のモバイル アプリで、`BindableBehavior<T>`クラスから派生、 [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1)クラス。 目的、`BindableBehavior<T>`を必要とする Xamarin.Forms の動作の基本クラスを提供するクラスは、 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext)のアタッチされたコントロールに設定する動作。

`BindableBehavior<T>`クラスには、オーバーライド可能な`OnAttachedTo`を設定するメソッド、 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext)の動作と、オーバーライド可能な`OnDetachingFrom`をクリーンアップする方法、`BindingContext`します。 さらに、このクラスでは、アタッチされたコントロールへの参照が `AssociatedObject` プロパティに格納されます。

EShopOnContainers のモバイル アプリでは、`EventToCommandBehavior`クラスは、発生中のイベントに応答するコマンドを実行します。 このクラスから派生、`BindableBehavior<T>`クラスの動作にバインドして実行できるように、`ICommand`で指定された、`Command`プロパティを使用すると、動作します。 次に示すのは、`EventToCommandBehavior` クラスのコード例です。

```csharp
public class EventToCommandBehavior : BindableBehavior<View>  
{  
    ...  
    protected override void OnAttachedTo(View visualElement)  
    {  
        base.OnAttachedTo(visualElement);  

        var events = AssociatedObject.GetType().GetRuntimeEvents().ToArray();  
        if (events.Any())  
        {  
            _eventInfo = events.FirstOrDefault(e => e.Name == EventName);  
            if (_eventInfo == null)  
                throw new ArgumentException(string.Format(  
                        "EventToCommand: Can't find any event named '{0}' on attached type",   
                        EventName));  

            AddEventHandler(_eventInfo, AssociatedObject, OnFired);  
        }  
    }  

    protected override void OnDetachingFrom(View view)  
    {  
        if (_handler != null)  
            _eventInfo.RemoveEventHandler(AssociatedObject, _handler);  

        base.OnDetachingFrom(view);  
    }  

    private void AddEventHandler(  
            EventInfo eventInfo, object item, Action<object, EventArgs> action)  
    {  
        ...  
    }  

    private void OnFired(object sender, EventArgs eventArgs)  
    {  
        ...  
    }  
}
```

`OnAttachedTo`と`OnDetachingFrom`メソッドを使用して、登録し、で定義されているイベントのイベント ハンドラーを登録解除、`EventName`プロパティ。 その後、イベントが発生したとき、`OnFired`メソッドが呼び出されるコマンドを実行します。

使用する利点、`EventToCommandBehavior`イベントが発生したときにコマンドを実行するにはコマンドがコマンドと対話するように設計でした。 コントロールに関連付けできます。 さらに、このイベント処理コードに移動ビュー モデルでは、単体テストを行うことできます。

#### <a name="invoking-behaviors-from-a-view"></a>ビューからの起動の動作

`EventToCommandBehavior`はコマンドをコマンドでサポートされていないコントロールにアタッチするために特に便利です。 たとえば、`ProfileView`を使用して、`EventToCommandBehavior`を実行する、`OrderDetailCommand`ときに、 [ `ItemTapped` ](xref:Xamarin.Forms.ListView.ItemTapped)でイベントが発生した、 [ `ListView` ](xref:Xamarin.Forms.ListView)に示すように、ユーザーの注文が一覧表示次のコード。

```xaml
<ListView>  
    <ListView.Behaviors>  
        <behaviors:EventToCommandBehavior             
            EventName="ItemTapped"  
            Command="{Binding OrderDetailCommand}"  
            EventArgsConverter="{StaticResource ItemTappedEventArgsConverter}" />  
    </ListView.Behaviors>  
    ...  
</ListView>
```

実行時に、`EventToCommandBehavior`との対話に応答が、 [ `ListView`](xref:Xamarin.Forms.ListView)します。 項目を選択すると、 `ListView`、 [ `ItemTapped` ](xref:Xamarin.Forms.ListView.ItemTapped)が実行されるイベントは起動、`OrderDetailCommand`で、`ProfileViewModel`します。 既定では、イベントのイベント引数は、コマンドに渡されます。 このデータは、ソースとターゲット間で指定されたコンバーターによって渡される、変換、`EventArgsConverter`返すプロパティを[ `Item` ](xref:Xamarin.Forms.ItemTappedEventArgs.Item)の`ListView`から、 [ `ItemTappedEventArgs`](xref:Xamarin.Forms.ItemTappedEventArgs). そのため、`OrderDetailCommand`が実行される、選択した`Order`は登録されているアクションをパラメーターとして渡されます。

動作の詳細については、次を参照してください。[動作](~/xamarin-forms/app-fundamentals/behaviors/index.md)します。

## <a name="summary"></a>まとめ

モデル-ビュー-ビューモデル (MVVM) パターンは、そのユーザー インターフェイス (UI) からアプリケーションのビジネスとプレゼンテーション ロジックを明確に分離するのに役立ちます。 アプリケーション ロジックと UI の明確な分離を維持し、開発のさまざまな問題に対処するのに役立つ、し、アプリケーションのテスト、保守、および進化を簡単にします。 コードの再利用機会も大幅に向上し、により、開発者とより UI デザイナーは、それぞれの部品のアプリを開発するときに簡単に共同作業します。

パターン、MVVM を使用して、アプリの UI と基になるプレゼンテーションとビジネス ロジックは 3 つの独立したクラスに分割されます。 UI と UI をカプセル化すると、ビュー ロジック。プレゼンテーション ロジックと状態をカプセル化するビュー モデルモデルでは、アプリのビジネス ロジックとデータをカプセル化します。


## <a name="related-links"></a>関連リンク

- [(2 Mb の PDF) 電子ブックをダウンロードします。](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (サンプル)](https://github.com/dotnet-architecture/eShopOnContainers)
