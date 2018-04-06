---
title: MVVM
ms.prod: xamarin
ms.assetid: dd8c1813-df44-4947-bcee-1a1ff2334b87
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 32a7a7dd50edcc3eefe76429ddb1e5581447993e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="mvvm"></a>MVVM

Xamarin.Forms 開発者エクスペリエンスには、通常、XAML では、ユーザー インターフェイスを作成し、ユーザー インターフェイスで動作する分離コードが含まれます。 アプリでは、変更すると、し、サイズとスコープの拡張、メンテナンスも複雑問題が発生することができます。 これらの問題には、UI コントロールと UI の変更を行うと、このようなコードの単体テストの難易度をするためのコストが増加するビジネス ロジックの間の密結合が含まれます。

モデル View-viewmodel (MVVM) パターンは、そのユーザー インターフェイス (UI) からのアプリケーションのビジネスやプレゼンテーション ロジックを明確に分離するのに役立ちます。 アプリケーション ロジックと UI の間で明確に分離を維持し、さまざまな開発上の問題に対処するのに役立つ、やすく、アプリケーションのテスト、保守、および進化します。 コードを再利用機会も大幅に向上し、開発者ができるし、それぞれの部品のアプリを開発するときに、UI 設計者は、複数が容易に共同作業します。

## <a name="the-mvvm-pattern"></a>MVVM パターン

MVVM パターンでは次の 3 つのコア コンポーネントがある: モデル、ビュー、およびビューのモデル。 各は、個別の目的を果たします。 図 2-1 は、次の 3 つのコンポーネント間の関係を示しています。

![](mvvm-images/mvvm.png "MVVM パターン")

**図 2-1**:「MVVM パターン

各コンポーネントの役割を理解するだけでなくも互いとやり取りする方法を理解しておく必要です。 大まかに言えば、ビュー「認識」ビュー モデル ビュー モデル「認識」、モデルがモデルは、ビュー モデルを認識せず、ビュー モデルは、ビューの認識しません。 そのため、ビュー モデル、モデルからビューを分離でき、モデル、ビューとは無関係に発展させる。

MVVM パターンを使用する利点は次のとおりです。

-   既存のビジネス ロジックをカプセル化する既存のモデルの実装がある場合は、困難または変更する危険性の高い、できます。 このシナリオでは、ビュー モデルは、モデル クラスのアダプターとして機能し、モデルのコードに大幅な変更しないようにすることができます。
-   開発者は、ビューを使用せず、ビュー モデルと、モデルの単体テストを作成できます。 ビュー モデルの単体テスト機能は、ビューで使用されるとまったく同じ操作を行えます。
-   アプリの UI は、XAML で、ビューが実装されていること、コードを変更することがなく再設計することができます。 そのため、ビューの新しいバージョンは、既存のビュー モデルに機能する必要があります。
-   設計者と開発者できます上の作業とは独立してと同時に、コンポーネント開発プロセス中にします。 デザイナーは、開発者の作業ができ、ビュー モデルとモデルのコンポーネントの中に、ビューに専念できます。

MVVM を効果的に使用するキーに正しいクラスでは、アプリ コードのファクタリングする方法を理解するうえで、クラスの対話方法を理解するうえでの範囲です。 次のセクションでは、MVVM パターン内のクラスのそれぞれの役割について説明します。

### <a name="view"></a>表示

ビューは、構造、レイアウト、およびユーザーが画面に表示の外観を定義します。 理想的には、各ビューは、限られた分離コードでビジネス ロジックを含まない、XAML で定義されます。 ただし、分離コードについては、アニメーションなどの XAML では、express が困難に視覚的な動作を実装する UI ロジック含めることがあります。

Xamarin.Forms のアプリケーションでは、ビューは通常、 [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)-派生または[ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/)-クラスを派生します。 ただし、ビューの表示されているオブジェクトを視覚的に表現するために使用する UI 要素を指定、データのテンプレートでは、表すこともできます。 ビューとしてデータ テンプレートでは、その分離コードではなく、特定のビュー モデルの種類にバインドするよう設計されています。

> [!TIP]
> 有効にして、分離コード内の UI 要素を無効化しないでください。 モデルの表示が、ビューの表示するかどうか、コマンドを使用、または保留中の操作を示す値などのいくつかの側面に影響する論理的な状態変更の定義を担当することを確認します。 そのため、有効にし、モデルのプロパティではなくを有効にして、分離コードで無効にすることを表示するバインディングによって UI 要素を無効にします。

ボタンのクリックしてなど、ビューのモデル、ビュー上の相互作用への応答でコードを実行するか、項目の選択のいくつかのオプションがあります。 コントロールは、コマンド、コントロールをサポートしている場合`Command`プロパティがありますにデータ バインド、`ICommand`ビュー モデルのプロパティです。 コントロールのコマンドが呼び出されたときに、ビュー モデル内のコードが実行されます。 だけでなく、コマンドは、動作は、ビュー内のオブジェクトにアタッチでき、呼び出されるコマンドまたはイベントが発生するのリッスンできます。 応答として、動作は呼び出すことができますし、`ICommand`ビュー モデルまたはモデルの表示のメソッドにします。

### <a name="viewmodel"></a>ViewModel

ビュー モデルでは、プロパティと、ビューがデータ バインドをするコマンドを実装し、変更通知イベントを使用して、状態変更の表示を通知します。 プロパティとビューのモデルを提供するコマンドは、UI で提供される機能を定義しますが、ビューでは、その機能の表示方法を決定します。

> [!TIP]
> UI の非同期操作の応答性を維持します。 モバイル アプリには、ユーザーのパフォーマンスに対する認識を向上させるためにブロック解除、UI スレッドを保持する必要があります。 したがって、モデルでは、ビュー、I/O 操作の非同期メソッドを使用して、プロパティの変更のビューを非同期的に通知するイベントを発生させます。

ビュー モデルもを必要とされるすべてのモデル クラスとビューの相互作用を調整します。 ビュー モデルとモデル クラス間の一対多リレーションシップは一般的です。 ビュー モデルは、ビュー内のコントロールに直接データをバインドできるようにすることは、ビューに直接モデル クラスを公開することもできます。 この場合、モデルのクラスは、データ バインドをサポートし、変更通知イベントを設計する必要があります。

各ビューのモデルでは、ビューを簡単に使用できる形式のモデルからデータを提供します。 ビュー モデルには、これを実現するには、データの変換もを実行します。 ビューにバインドできるプロパティを提供するためは、ビュー モデルにこのデータの変換を配置することをお勧めします。 たとえば、ビュー モデルには、ビューで表示を容易にできるように 2 つのプロパティの値を組み合わせる可能性があります。

> [!TIP]
> 変換レイヤーでのデータ変換を集中します。 ビュー モデルとビューの間に位置する別のデータ変換レイヤーとしてコンバーターを使用することもできます。 これは、できます必要に応じて、たとえば、データは、ビュー モデルを提供しない特殊な書式設定する必要があります。

ビュー モデル、ビューとの双方向データ バインドに参加するためには、そのプロパティを上げる必要があります、`PropertyChanged`イベント。 モデルの表示が実装することによってこの要件を満たす、`INotifyPropertyChanged`インターフェイス、およびさせると、`PropertyChanged`プロパティが変更されたときにイベント。

コレクションの場合、ビューに適した`ObservableCollection<T>`は提供します。 このコレクションが変更されたコレクションの通知、開発者を実装する手間を削減を実装して、`INotifyCollectionChanged`コレクションのインターフェイスです。

### <a name="model"></a>モデル

モデル クラスは、アプリのデータをカプセル化するクラスを非表示です。 そのため、モデル見なすことができますの通常ビジネスおよび検証ロジックとデータ モデルが含まれますアプリのドメイン モデルを表しているとします。 モデル オブジェクトの例には、データ転送オブジェクト (Dto)、Plain Old CLR Object (した poco から)、および生成されたエンティティおよびプロキシ オブジェクトが含まれます。

モデル クラスは通常、サービスまたはデータ アクセスとキャッシュをカプセル化のリポジトリと組み合わせて使用されます。

## <a name="connecting-view-models-to-views"></a>ビューにモデルの表示を接続します。

モデルの表示は、Xamarin.Forms のデータ バインディング機能を使用してビューに接続することができます。 ビューを構築し、モデルの表示し、実行時に関連付けるに使用できる方法はたくさんあります。 これらのアプローチは、ビューの最初の構成およびビュー モデルの最初の構成と呼ばれる 2 つのカテゴリに分類されます。 ビューの最初の構成とモデルの最初のコンポジション 基本設定と複雑性の問題は、ビューを選択します。 ただし、すべての方法は、その BindingContext プロパティに割り当てられているビュー モデルがビューの場合は、同じ目的を共有します。

ビュー最初コンポジション アプリは概念的には互いに依存するビュー モデルに接続するビューで構成されます。 このアプローチの主な利点は、ことになります、疎結合を作成する簡単な単体テストが容易なアプリのビュー モデルには、ビュー自体への依存があるないためです。 アプリの構造を理解するには、クラスを作成し、関連する方法を理解するコードが実行を追跡するために発生するのではなく、その視覚的な構造を次に簡単です。 さらに、ビューの最初の構築と、Xamarin.Forms ナビゲーション システムが担当構築ページ ナビゲーションが発生したときにビュー モデルの最初の構成を使用すると、複雑なとプラットフォームとのずれを整列させます。

ビューでモデル最初コンポジション アプリは概念的にはモデルの表示のと共に構成されるビュー モデルのビューを検索する役割サービス。 ビュー モデルの最初の構成は、ビューの作成できる抽象化、アプリの UI 以外の論理構造に集中できるようになるため、一部の開発者にとってより自然なと考えています。 さらに、その他のビュー モデルによって作成されるモデルの表示ができます。 ただし、この方法は複雑な多くの場合、アプリのさまざまな部分が作成され、関連付けられている方法を理解するが困難になることができます。

> [!TIP]
> モデルの表示とビューを独立した保持します。 データ ソースのプロパティにビューのバインディングは、対応するビューのモデルのビューのプリンシパルの依存関係をする必要があります。 などの種類のビューを参照しない具体的には、 [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/)と[ `ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)モデルの表示からです。 ここで説明した原則により、分離、そのため、スコープを制限することによって、ソフトウェアの不具合の可能性を減らすことでビュー モデルをテストできます。

次のセクションでは、ビューにモデルの表示を接続する主な方法について説明します。

### <a name="creating-a-view-model-declaratively"></a>宣言によってビュー モデルを作成します。

簡単な方法は、XAML では、対応するビュー モデルを宣言してインスタンス化するビューです。 ビューを作成するとき、対応するビューのモデル オブジェクトは作成もできます。 この方法は、次のコード例について説明します。

```xaml
<ContentPage ... xmlns:local="clr-namespace:eShop">  
    <ContentPage.BindingContext>  
        <local:LoginViewModel />  
    </ContentPage.BindingContext>  
    ...  
</ContentPage>
```

ときに、 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)は、インスタンスを作成、`LoginViewModel`が自動的に構築され、ビューの設定[ `BindingContext`](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/)です。

この宣言型の構築と割り当てビューでビュー モデルの利点は、単純、されているが、欠点もビュー モデルの既定の (パラメーターなし) コンス トラクターが必要であることがあります。

### <a name="creating-a-view-model-programmatically"></a>ビュー モデルをプログラムで作成します。

ビューは、結果に割り当てられているモデルの表示、分離コード ファイルでコードを持つことができます、 [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/)プロパティです。 多くの場合、これは、ビューのコンス トラクターで次のコード例に示すように。

```csharp
public LoginView()  
{  
    InitializeComponent();  
    BindingContext = new LoginViewModel(navigationService);  
}
```

プログラムによる構築と割り当てビューの分離コード内でビュー モデルの単純であるという利点があります。 ただし、このアプローチの主な欠点は、ビューが必要な依存関係をビュー モデルを提供する必要があることにします。 依存関係性の注入コンテナーを使用して疎結合のビューとビューのモデルの間で維持するために役立ちます。 詳細については、次を参照してください。[依存性の注入](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md)です。

### <a name="creating-a-view-defined-as-a-data-template"></a>データ テンプレートとして定義されているビューを作成します。

ビューのデータ テンプレートとして定義されているし、ビュー モデルの種類に関連付けられていることができます。 データ テンプレートは、リソースとして定義することができます。 またはビューのモデルを表示するコントロールではインラインで定義されている可能性があります。 コントロールの内容は、ビュー モデル インスタンスし、データ テンプレートが視覚的に表現するために使用されます。 この手法をビュー モデルがインスタンス化される最初に、ビューの作成を続けて状況の例を示します。

<a name="automatically_creating_a_view_model_with_a_view_model_locator" />

### <a name="automatically-creating-a-view-model-with-a-view-model-locator"></a>ビュー モデル ロケーターにビュー モデルを自動的に作成します。

ビュー モデル ロケーターは、モデルの表示とビューへの関連付けのインスタンス化を管理するカスタム クラスです。 EShopOnContainers のモバイル アプリで、`ViewModelLocator`クラスには、添付プロパティ`AutoWireViewModel`モデルの表示をビューに関連付けるために使用されます。 ビューの XAML では、この添付プロパティは次のコード例のように、ビュー モデルが、ビューを自動的に接続されているを示す true に設定します。

```xaml
viewModelBase:ViewModelLocator.AutoWireViewModel="true"
```

`AutoWireViewModel`プロパティは、バインド可能なプロパティを false に初期化して、その値が変更されたとき、`OnAutoWireViewModelChanged`イベント ハンドラーが呼び出されます。 このメソッドは、ビューのビュー モデルを解決します。 次のコード例では、これを実現する方法について示しています。

```csharp
private static void OnAutoWireViewModelChanged(BindableObject bindable, object oldValue, object newValue)  
{  
    var view = bindable as Element;  
    if (view == null)  
    {  
        return;  
    }  

    var viewType = view.GetType();  
    var viewName = viewType.FullName.Replace(".Views.", ".ViewModels.");  
    var viewAssemblyName = viewType.GetTypeInfo().Assembly.FullName;  
    var viewModelName = string.Format(  
        CultureInfo.InvariantCulture, "{0}Model, {1}", viewName, viewAssemblyName);  

    var viewModelType = Type.GetType(viewModelName);  
    if (viewModelType == null)  
    {  
        return;  
    }  
    var viewModel = _container.Resolve(viewModelType);  
    view.BindingContext = viewModel;  
}
```

`OnAutoWireViewModelChanged`メソッドは、規則ベースのアプローチを使用して、ビュー モデルの解決を試みます。 この規則は、ことを前提とします。

-   モデルの表示は、ビューの種類と同じアセンブリでです。
-   ビューは、します。ビューの子名前空間です。
-   モデルの表示は、します。ViewModels 子名前空間です。
-   ビューのモデル名は、ビューの名前と対応して、"ViewModel"で終わります。

最後に、`OnAutoWireViewModelChanged`メソッドのセット、 [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/)解決済みのビュー モデルの種類にビューの種類のです。 ビューのモデル型の解決の詳細については、次を参照してください。[解決](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#resolution)です。

この方法には、アプリがモデルの表示とビューへの接続をインスタンス化は、1 つのクラスを持つことの利点があります。

> [!TIP]
> 置換の容易にするためには、ビュー モデル ロケーターを使用します。 ビュー モデル ロケーターこともできます、依存関係の代替実装のための置換のポイントとしてなどの単体テストまたはデザイン時のデータ。

## <a name="updating-views-in-response-to-changes-in-the-underlying-view-model-or-model"></a>基になる変更に合わせて更新ビュー モデルまたはモデルの表示

すべてのビュー モデルとビューにアクセスできるモデル クラスを実装する必要があります、`INotifyPropertyChanged`インターフェイスです。 基になるプロパティの値が変更されたときに、変更通知をビュー内のデータ バインド コントロールを提供するクラスをビュー モデルまたはモデルのクラスでこのインターフェイスを実装できます。

アプリは、次の要件を満たすことによってプロパティの変更通知の正しい使用用に設計されて必要があります。

-   常にさせると、`PropertyChanged`イベント パブリック プロパティの値が変更された場合。 そのさせるとは限らない、`PropertyChanged`イベントは XAML バインディングが発生する方法に関する知識のために無視できます。
-   常にさせると、`PropertyChanged`イベントのいずれかの値を持つがビュー内の他のプロパティで使用されるプロパティを計算するモデルまたはモデル。
-   常にさせると、`PropertyChanged`により、プロパティの変更、または安全な状態にあるオブジェクトがわかっている場合、メソッドの最後のイベントです。 イベントを発生させるイベントのハンドラーを同期的に呼び出すことによって、操作を中断します。 操作の途中でこのような場合、部分的に更新される安全でない状態であるときに、コールバック関数をオブジェクトを公開可能性があります。 カスケード型の変更によってトリガーされる可能性がさらに、`PropertyChanged`イベント。 カスケード型の変更には、一般にカスケード型の変更が安全に実行する前に完了する更新プログラムが必要です。
-   発生させることはありません、`PropertyChanged`イベント プロパティが変更されていない場合。 つまり、発生させる前に、古い値と新しい値を比較する必要があります、`PropertyChanged`イベント。
-   発生させることはありません、`PropertyChanged`プロパティを初期化する場合、ビュー モデルのコンス トラクターの中にイベント。 ビュー内のデータ バインド コントロールはこの時点での変更通知を受信するサブスクライブしているされません。
-   1 つ以上を発生させることはありません`PropertyChanged`イベント クラスのパブリック メソッドの 1 つの同期呼び出し内で同じプロパティ名引数とします。 例として、`NumberOfItems`プロパティがバッキング ストアとして、`_numberOfItems`フィールドで、メソッド刻み`_numberOfItems`50 回ループの実行中にのみを発生させるプロパティの変更通知で、`NumberOfItems`プロパティに 1 回すべての作業が完了します。 非同期メソッドは、生成、`PropertyChanged`イベントを非同期的な継続チェーンの各同期セグメントに指定されたプロパティ名。

EShopOnContainers モバイル アプリの使用、`ExtendedBindableObject`を提供するクラスは次のコード例で示した、通知を変更します。

```csharp
public abstract class ExtendedBindableObject : BindableObject  
{  
    public void RaisePropertyChanged<T>(Expression<Func<T>> property)  
    {  
        var name = GetMemberInfo(property).Name;  
        OnPropertyChanged(name);  
    }  

    private MemberInfo GetMemberInfo(Expression expression)  
    {  
        ...  
    }  
}
```

Xamarin.Form の[ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/)クラスが実装する、`INotifyPropertyChanged`インターフェイス、および提供、 [ `OnPropertyChanged` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.OnPropertyChanged/p/System.String/)メソッドです。 `ExtendedBindableObject`クラスを提供、`RaisePropertyChanged`プロパティを呼び出すメソッドを変更の通知とこれには、によって提供される機能を使用して、`BindableObject`クラスです。

EShopOnContainers モバイル アプリで各ビューのモデル クラスから派生、`ViewModelBase`から派生するクラス、`ExtendedBindableObject`クラスです。 したがって、各ビューのモデル クラスを使用して、`RaisePropertyChanged`メソッドで、`ExtendedBindableObject`プロパティの変更通知を提供するクラス。 次のコード例は、eShopOnContainers モバイル アプリがラムダ式を使用してプロパティの変更通知を起動する方法を示しています。

```csharp
public bool IsLogin  
{  
    get  
    {  
        return _isLogin;  
    }  
    set  
    {  
        _isLogin = value;  
        RaisePropertyChanged(() => IsLogin);  
    }  
}
```

この方法でラムダ式の使用コストが、ラムダ式は呼び出しごとに評価される必要があるためパフォーマンスが多少の含まれることに注意してください。 パフォーマンス コストが小さいと、アプリが通常影響を受けない、ですが、多くの変更通知がある場合に、コストできます計上されます。 ただし、このアプローチの利点は、コンパイル時の型の安全性とプロパティを変更するとリファクタリングのサポートを提供します。

## <a name="ui-interaction-using-commands-and-behaviors"></a>コマンドの動作を使用して UI の相互作用

モバイル アプリの場合は、アクションは通常、分離コード ファイルで、イベント ハンドラーを作成することで実装できるボタンのクリックなどのユーザー アクションへの応答で呼び出されます。 ただし、パターンでは、MVVM、モデルでは、表示、操作を実装する責任があるし、分離コードで配置するコードを避ける必要があります。

コマンドは、ui コントロールにバインドできるアクションを表す便利な方法を提供します。 アクションを実装するコードをカプセル化し、ビューでのビジュアル表現から分離されたことを維持するためです。 Xamarin.Forms は、コマンドを宣言によって接続されているコントロールを備えたし、ユーザーがコントロールを操作するときにこれらのコントロールがコマンドを呼び出します。

動作では、宣言的に接続しているコマンド コントロールこともできます。 ただし、動作は、さまざまなコントロールによって発生したイベントに関連付けられているアクションを呼び出すために使用できます。 そのため、動作に対処、コマンドが有効なコントロールとして同じシナリオの多くは高い柔軟性と制御を提供しつつです。 さらに、動作は、コマンドとの対話を具体的にはデザインされていないコントロールにコマンド オブジェクトやメソッドを関連付けることも使用できます。

### <a name="implementing-commands"></a>コマンドを実装します。

モデルの表示が通常を実装するオブジェクトのインスタンスであるビューで、バインディングのコマンド プロパティを公開、`ICommand`インターフェイスです。 Xamarin.Forms コントロールの多くを提供して、`Command`プロパティは、データにバインドされる、`ICommand`ビュー モデルが提供するオブジェクト。 `ICommand`インターフェイスを定義、`Execute`操作自体をカプセル化するメソッド、 `CanExecute` 、メソッド、およびコマンドを呼び出すことができます、かどうかを示す`CanExecuteChanged`変更に影響するのかどうか発生時に発生するイベントコマンドを実行する必要があります。 [ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/)と[ `Command<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) Xamarin.Forms、によって提供されるクラスを実装、`ICommand`インターフェイス、場所`T`への引数の型は、`Execute`と`CanExecute`です。

ビュー モデル内で存在する必要があります型のオブジェクト[ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/)または[ `Command<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/)型のビュー モデル内の各パブリック プロパティの`ICommand`します。 `Command`または`Command<T>`コンス トラクターが必要です、`Action`ときに呼び出されたコールバック オブジェクト、`ICommand.Execute`メソッドが呼び出されます。 `CanExecute`メソッド、コンス トラクターの省略可能なパラメーターであり、`Func`を返す、`bool`です。

方法を次のコードに示します、 [ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/)へのデリゲートを指定することによって、登録コマンドを表すインスタンスが構築された、`Register`モデルのメソッドを表示します。

```csharp
public ICommand RegisterCommand => new Command(Register);
```

参照を返すプロパティを通じてビューに、コマンドが公開されている、`ICommand`です。 ときに、`Execute`メソッドが、 [ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/)オブジェクト、メソッドに指定されたデリゲートを使用してビューのモデルでの呼び出しを転送するだけ、`Command`コンス トラクターです。

使用して、コマンドによって、非同期メソッドを呼び出すことができます、`async`と`await`キーワードのコマンドを指定するときに`Execute`を委任します。 これは、コールバックがあることを示します、`Task`れ、待機する必要があります。 たとえば、次のコードはどのように、 [ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/)サイン イン コマンドを表すインスタンスがデリゲートを指定することによって構築された、`SignInAsync`モデルのメソッドを表示。

```csharp
public ICommand SignInCommand => new Command(async () => await SignInAsync());
```

パラメーターに渡すことができます、`Execute`と`CanExecute`アクションを使用して、 [ `Command<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/)コマンドをインスタンス化するクラス。 たとえば、次のコードはどのように、`Command<T>`のインスタンスがあることを示す使用、`NavigateAsync`メソッドに必要な型の引数`string`:

```csharp
public ICommand NavigateCommand => new Command<string>(NavigateAsync);
```

両方で、 [ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/)と[ `Command<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/)クラス、デリゲート、`CanExecute`各コンス トラクターのメソッドは省略可能です。 デリゲートが指定されていない場合、`Command`戻ります`true`の`CanExecute`します。 ただし、ビュー モデルは、コマンドの変化を示すことができます`CanExecute`ステータスを呼び出して、`ChangeCanExecute`メソッドを`Command`オブジェクト。 これにより、`CanExecuteChanged`イベントが発生します。 コマンドにバインドされているコントロールが UI では、データ バインドのコマンドは、の可用性を反映するように、有効な状態を更新し、されます。

#### <a name="invoking-commands-from-a-view"></a>ビューからコマンドを起動

次のコード例に示す方法、 [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/)で、`LoginView`にバインドされて、`RegisterCommand`で、`LoginViewModel`クラスを使用して、 [ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/)インスタンス。

```xaml
<Grid Grid.Column="1" HorizontalOptions="Center">  
    <Label Text="REGISTER" TextColor="Gray"/>  
    <Grid.GestureRecognizers>  
        <TapGestureRecognizer Command="{Binding RegisterCommand}" NumberOfTapsRequired="1" />  
    </Grid.GestureRecognizers>  
</Grid>
```

コマンド パラメーターを定義することも必要に応じてを使用して、 [ `CommandParameter` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TapGestureRecognizer.CommandParameter/)プロパティです。 必要な引数の型が指定された、`Execute`と`CanExecute`メソッドを対象します。 [ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/)ユーザーが接続されているコントロールを操作するときに、ターゲット コマンドを自動的に起動はします。 コマンド パラメーターを指定した場合は、コマンドの引数として渡すは`Execute`を委任します。

<a name="implementing_behaviors" />

### <a name="implementing-behaviors"></a>動作を実装します。

動作は、それらをサブクラス化することがなく、UI コントロールに追加する機能を許可します。 代わりに、機能が動作クラスで実装され、コントロール自体の一部が場合と、コントロールにアタッチされています。 動作を使用するを簡潔に、コントロールにアタッチして 1 つ以上のビューやアプリ間で再利用するためにパッケージ化方法で、コントロールの API と直接やり取りするため、分離コードとして記述する必要が通常のコードを実装することができます。 MVVM のコンテキストでは、動作は、コマンドにコントロールを接続するために有効な方法です。

添付プロパティをコントロールに関連付けられている動作と呼ばれる、*動作をアタッチ*です。 動作は、それが接続されているビューのビジュアル ツリー内の他のコントロールや、そのコントロールの機能を追加する要素の公開されている API を使用できます。 EShopOnContainers モバイル アプリを含む、`LineColorBehavior`クラスは、これは接続されている動作です。 この動作の詳細については、次を参照してください。[妥当性確認エラーを表示する](~/xamarin-forms/enterprise-application-patterns/validation.md#displaying_validation_errors)です。

Xamarin.Forms 動作はから派生したクラス、 [ `Behavior` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/)または[ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/)クラス、場所`T `動作を適用するコントロールの種類です。 これらのクラスを提供`OnAttachedTo`と`OnDetachingFrom`メソッドで、動作にアタッチされているし、コントロールからデタッチされたときに実行するロジックをオーバーライドする必要があります。

EShopOnContainers のモバイル アプリで、`BindableBehavior<T>`から派生したクラス、 [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/)クラスです。 目的、`BindableBehavior<T>`クラスは、Xamarin.Forms 動作が必要である場合、基底クラスを提供する、 [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/)添付コントロールに設定する動作のです。

`BindableBehavior<T>`クラスは、オーバーライド可能な提供`OnAttachedTo`を設定するメソッド、 [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/)の動作と、オーバーライド可能な`OnDetachingFrom`をクリーンアップする方法、`BindingContext`です。 さらに、クラスに接続されているコントロールへの参照を格納、`AssociatedObject`プロパティです。

EShopOnContainers モバイル アプリが含まれています、`EventToCommandBehavior`発生中のイベントへの応答のコマンドを実行するクラス。 このクラスから派生、`BindableBehavior<T>`クラスの動作がバインドして実行できるように、`ICommand`によって指定された、`Command`プロパティを使用すると、動作します。 次に示すのは、`EventToCommandBehavior` クラスのコード例です。

```csharp
public class EventToCommandBehavior : BindableBehavior<View>  
{  
    ...  
    protected override void OnAttachedTo(View visualElement)  
    {  
        base.OnAttachedTo(visualElement);  

        var events = AssociatedObject.GetType().GetRuntimeEvents().ToArray();  
        if (events.Any())  
        {  
            _eventInfo = events.FirstOrDefault(e => e.Name == EventName);  
            if (_eventInfo == null)  
                throw new ArgumentException(string.Format(  
                        "EventToCommand: Can't find any event named '{0}' on attached type",   
                        EventName));  

            AddEventHandler(_eventInfo, AssociatedObject, OnFired);  
        }  
    }  

    protected override void OnDetachingFrom(View view)  
    {  
        if (_handler != null)  
            _eventInfo.RemoveEventHandler(AssociatedObject, _handler);  

        base.OnDetachingFrom(view);  
    }  

    private void AddEventHandler(  
            EventInfo eventInfo, object item, Action<object, EventArgs> action)  
    {  
        ...  
    }  

    private void OnFired(object sender, EventArgs eventArgs)  
    {  
        ...  
    }  
}
```

`OnAttachedTo`と`OnDetachingFrom`メソッドを使用して登録しで定義されているイベントのイベント ハンドラーを登録解除、`EventName`プロパティです。 その後、イベントが発生したとき、`OnFired`メソッドが呼び出され、これは、コマンドを実行します。

使用する利点、`EventToCommandBehavior`イベントが発生したときにコマンドを実行することは、コマンドことコマンドとの対話に設計されていないコントロールに関連付けられています。 さらに、イベント処理コードに移動モデルの表示、単体テストを行うことできます。

#### <a name="invoking-behaviors-from-a-view"></a>ビューからの動作を呼び出す

`EventToCommandBehavior`はコマンドがサポートされていないコントロールにコマンドを関連付ける際に特に便利です。 たとえば、`ProfileView`を使用して、`EventToCommandBehavior`を実行する、`OrderDetailCommand`ときに、 [ `ItemTapped` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemTapped/)でイベントが発生した、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)ように、ユーザーの注文を一覧表示次のコード。

```xaml
<ListView>  
    <ListView.Behaviors>  
        <behaviors:EventToCommandBehavior             
            EventName="ItemTapped"  
            Command="{Binding OrderDetailCommand}"  
            EventArgsConverter="{StaticResource ItemTappedEventArgsConverter}" />  
    </ListView.Behaviors>  
    ...  
</ListView>
```

実行時に、`EventToCommandBehavior`との対話に応答する、 [ `ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)です。 項目が選択した場合、 `ListView`、 [ `ItemTapped` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemTapped/)を実行するイベントは起動、`OrderDetailCommand`で、`ProfileViewModel`です。 既定では、イベントのイベント引数は、コマンドに渡されます。 このデータは、ソースとターゲット間で指定された、コンバーターで渡される、変換、`EventArgsConverter`プロパティが返されます、 [ `Item` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemTappedEventArgs.Item/)の`ListView`から、 [ `ItemTappedEventArgs`](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemTappedEventArgs/). したがって、ときに、`OrderDetailCommand`が実行される、選択した`Order`は登録されているアクションをパラメーターとして渡されます。

動作に関する詳細については、次を参照してください。[動作](~/xamarin-forms/app-fundamentals/behaviors/index.md)です。

## <a name="summary"></a>まとめ

モデル View-viewmodel (MVVM) パターンは、そのユーザー インターフェイス (UI) からのアプリケーションのビジネスやプレゼンテーション ロジックを明確に分離するのに役立ちます。 アプリケーション ロジックと UI の間で明確に分離を維持し、さまざまな開発上の問題に対処するのに役立つ、やすく、アプリケーションのテスト、保守、および進化します。 コードを再利用機会も大幅に向上し、開発者ができるし、それぞれの部品のアプリを開発するときに、UI 設計者は、複数が容易に共同作業します。

パターン、MVVM を使用して、アプリの UI と基になるプレゼンテーションとビジネス ロジックは 3 つの別々 のクラスに分割されます。 UI と UI をカプセル化すると、ビュー ロジックです。プレゼンテーション ロジックと状態をカプセル化されているビュー モデルモデルでは、アプリのビジネス ロジックとデータをカプセル化します。


## <a name="related-links"></a>関連リンク

- [電子 (2 Mb PDF) のダウンロードします。](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (サンプル)](https://github.com/dotnet-architecture/eShopOnContainers)
