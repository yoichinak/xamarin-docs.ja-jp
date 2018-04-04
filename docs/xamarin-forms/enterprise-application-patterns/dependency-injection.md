---
title: 依存関係の挿入
ms.prod: xamarin
ms.assetid: a150f2d1-06f8-4aed-ab4e-7a847d69f103
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 8db8e5b756fe770bdf292ec03c28eb5ed54acf9e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="dependency-injection"></a>依存関係の挿入

通常、オブジェクトをインスタンス化するとき、クラス コンス トラクターが呼び出され、オブジェクト必要のある任意の値がコンス トラクターに引数として渡されます。 これは、依存関係の挿入の例と呼ばれる具体的には*コンス トラクター インジェクション*です。 コンス トラクターには、オブジェクトが必要な依存関係が挿入されます。

依存関係を指定するインターフェイスの種類として、依存関係の挿入により具象型これらの型に依存するコードからを分離できます。 一般に、登録とインターフェイスおよび抽象型は、間のマッピングの一覧を保持するコンテナーと実装またはこれらの型を拡張する具象型を使用します。

他の種類のある依存関係の挿入など*プロパティ set アクセス操作子インジェクション*、および*メソッド呼び出し挿入*、頻度の低いに表示されますが、します。 そのため、この章で依存性の注入コンテナー コンス トラクターの挿入を実行するだけに焦点を当てます。

<a name="introduction_to_dependency_injection" />

## <a name="introduction-to-dependency-injection"></a>依存関係の挿入の概要

依存関係の挿入は、必要な依存関係を取得するプロセスを反転されている懸念がここでは、制御の反転 (IoC) パターンの特殊化されたバージョンです。 依存関係の挿入と別のクラスを実行時にオブジェクトに依存関係を挿入します。 次のコード例に示す方法、`ProfileViewModel`依存関係の挿入を使用する場合、クラスが構造化します。

```csharp
public class ProfileViewModel : ViewModelBase  
{  
    private IOrderService _orderService;  

    public ProfileViewModel(IOrderService orderService)  
    {  
        _orderService = orderService;  
    }  
    ...  
}
```

`ProfileViewModel`コンス トラクターを受け取る、`IOrderService`インスタンスを別のクラスによって、挿入、引数として。 のみの依存関係、`ProfileViewModel`クラスがインターフェイス型にします。 したがって、`ProfileViewModel`クラスをインスタンス化するための知識がない、`IOrderService`オブジェクト。 クラスをインスタンス化するため、`IOrderService`オブジェクト、および挿入、`ProfileViewModel`クラスと呼ばれる、*依存性の注入コンテナー*です。

依存性の注入コンテナーは、クラスのインスタンスをインスタンス化し、コンテナーの構成に基づいてその有効期間を管理する機能を提供することによってオブジェクト間の結合を削減します。 オブジェクトの作成時に、コンテナーは、そこにオブジェクトを必要とするすべての依存関係を挿入します。 これらの依存関係がまだ作成されていない場合、コンテナーを作成し、その依存関係を最初に解決されます。

> [!NOTE]
> 依存関係の挿入は、ファクトリを使用して手動で実装することもできます。 ただし、コンテナーを使用して継続時間管理とスキャン アセンブリを介して登録などの他の機能を提供します。

これには依存性の注入コンテナーを使用するいくつかの利点があります。

-   コンテナーは、その依存関係を見つけて、その有効期間を管理するクラスの必要性を削除します。
-   コンテナーは、クラスの影響を与えずに実装されている依存関係のマッピングを許可します。
-   コンテナーは、モックを作成する依存関係を許可することで、テストの容易性を容易にします。
-   コンテナーでは、アプリを簡単に追加する新しいクラスを許可することで保守性が向上します。

MVVM を使用した Xamarin.Forms アプリのコンテキストで依存性の注入コンテナーは通常使用されます登録およびモデルの表示を解決するため、サービスを登録すると、モデルの表示にそれらを挿入します。

多くの依存関係の注入コンテナーは、Autofac を使用してモデルの表示のインスタンス化を管理し、アプリ内のクラスをサービスする eShopOnContainers モバイル アプリで使用できます。 Autofac、疎結合アプリの構築を容易に、すべての型マッピングおよびオブジェクトのインスタンスを登録する方法など、依存関係の注入コンテナーによく見られる機能オブジェクトを解決するには、オブジェクトの有効期間を管理および挿入解決するオブジェクトのコンス トラクターに依存するオブジェクト。 Autofac の詳細については、次を参照してください。 [Autofac](http://autofac.readthedocs.io/en/latest/index.html) readthedocs.io にします。

Autofac で、`IContainer`インターフェイスには、依存性の注入コンテナーが用意されています。 図 3-1 は、このコンテナーは、インスタンス化を使用する場合に、依存関係を示しています、`IOrderService`オブジェクトを挿入するには`ProfileViewModel`クラスです。

![](dependency-injection-images/dependencyinjection.png "依存関係の挿入を使用する場合、依存関係の例")

**図 3-1:**依存関係の挿入を使用する場合の依存関係

実行時に、コンテナーがのどの実装を知る必要があります、`IOrderService`インターフェイスには、インスタンス化をインスタンス化する前に、`ProfileViewModel`オブジェクト。 これは、ためには。

-   実装するオブジェクトをインスタンス化する方法を決定するコンテナー、`IOrderService`インターフェイスです。 これは呼ば*登録*です。
-   実装するオブジェクトをインスタンス化するコンテナー、`IOrderService`インターフェイス、および`ProfileViewModel`オブジェクト。 これは呼ば*解決*です。

最終的には、アプリが完了を使用して、`ProfileViewModel`オブジェクトはガベージ コレクションの使用可能になる予定です。 ガベージ コレクターがこの時点では、破棄する必要があります、`IOrderService`インスタンスの他のクラスは、同じインスタンスを共有していない場合。

> [!TIP]
> コンテナーに依存しないコードを記述します。 常を使用している特定の依存関係のコンテナーからアプリを分離するコンテナーに依存しないコードを記述してください。

## <a name="registration"></a>登録

依存関係は、オブジェクトに挿入されることができます、前に、依存関係の種類をコンテナーに登録されて最初必要があります。 通常のタイプを登録するには、インターフェイスおよびインターフェイスを実装する具象型は、コンテナーを渡すことが含まれます。

コードにより、そのコンテナーの型とオブジェクトを登録する次の 2 つの方法があります。

-   型またはマッピングをコンテナーに登録します。 必要な場合、コンテナーは、指定した型のインスタンスを構築します。
-   シングルトンとして、コンテナー内の既存のオブジェクトを登録します。 必要な場合、コンテナーは、既存のオブジェクトへの参照を返します。

> [!TIP]
> 依存関係の注入コンテナーは、常に対応できません。 依存関係の挿入には、複雑さが増大しできない可能性がある適切なまたは有用な小規模のアプリに要件が導入されています。 クラスはしていない場合、依存関係または他の種類の依存関係ではありません、いない賢明かもしれませんコンテナーに配置します。 さらに、クラスには 1 つの種類に不可欠な依存関係の設定は決して変化しないと、必要がありますいない、コンテナーに配置します。

アプリケーションでは、1 つのメソッドで必要な依存関係の挿入の種類の登録を実行して、アプリがそのクラス間の依存関係の対応することを確認するアプリのライフ サイクルの早い段階では、このメソッドを呼び出す必要があります。 EShopOnContainers モバイル アプリでこれを行う、`ViewModelLocator`クラス、どのビルド、`IContainer`オブジェクトであり、そのオブジェクトへの参照を保持するアプリで唯一のクラスです。 次のコード例は、eShopOnContainers モバイル アプリの宣言を示しています、`IContainer`内のオブジェクト、`ViewModelLocator`クラス。

```csharp
private static IContainer _container;
```

型とインスタンスに登録されて、`RegisterDependencies`メソッドで、`ViewModelLocator`クラスです。 これは、最初に作成することで実現、`ContainerBuilder`インスタンスで、次のコード例に示します。

```csharp
var builder = new ContainerBuilder();
```

型とインスタンスに登録されてから、`ContainerBuilder`オブジェクト、および次のコード例は、型の登録の最も一般的な形式を示します。

```csharp
builder.RegisterType<RequestProvider>().As<IRequestProvider>();
```

`RegisterType`ここに示したメソッドは、具象型にインターフェイスの型をマップします。 コンテナーのインスタンスを作成するか、`RequestProvider`オブジェクトのインジェクションを必要とするオブジェクトをインスタンス化時に、`IRequestProvider`コンス トラクターでします。

具象型は次のコード例のように、インターフェイス型、マッピングせずに直接登録することも。

```csharp
builder.RegisterType<ProfileViewModel>();
```

ときに、`ProfileViewModel`型が解決された場合、コンテナーが必要な依存関係を挿入します。

Autofac では、ここで、コンテナーは、型のシングルトン インスタンスへの参照の管理を担当、インスタンスを登録することもできます。 たとえば、次のコード例に示します eShopOnContainers モバイル アプリがときに使用する具体的な型を登録する方法、`ProfileViewModel`インスタンスが必要です、`IOrderService`インスタンス。

```csharp
builder.RegisterType<OrderService>().As<IOrderService>().SingleInstance();
```

`RegisterType`ここに示したメソッドは、具象型にインターフェイスの型をマップします。 `SingleInstance`メソッドは、すべての依存オブジェクトが同じ共有インスタンスが受信できるように、登録を構成します。 そのため、1 つだけ`OrderService`インスタンスは、挿入を必要とするオブジェクトで共有されると、コンテナー内存在は、`IOrderService`コンス トラクターでします。

インスタンスの登録を実行することも、`RegisterInstance`メソッドで、次のコード例に示します。

```csharp
builder.RegisterInstance(new OrderMockService()).As<IOrderService>();
```

`RegisterInstance`次に示すメソッドを作成、新しい`OrderMockService`をインスタンス化し、コンテナーに登録します。 そのため、1 つだけ`OrderMockService`は、挿入を必要とするオブジェクトで共有されると、コンテナー内のインスタンスが存在する、`IOrderService`コンス トラクターでします。

次の種類とインスタンスの登録、`IContainer`は次のコード例で示されているオブジェクトを構築する必要があります。

```csharp
_container = builder.Build();
```

呼び出す、`Build`メソッドを`ContainerBuilder`インスタンスが行われた登録を含む新しい依存関係の挿入コンテナーを構築します。

>💡 **ヒント:**: を検討してください、`IContainer`変更可能な低いものとして。 Autofac を提供しますが、`Update`可能な限り、このメソッドを呼び出すと、既存のコンテナー内での登録を更新する方法を避ける必要があります。 コンテナーが使用されている場合に特に、作成された後、コンテナーの変更のリスクがあります。 詳細については、次を参照してください。[変更不可としてのコンテナーを検討してください](http://docs.autofac.org/en/latest/best-practices/#consider-a-container-as-immutable)readthedocs.io にします。

<a name="resolution" />

## <a name="resolution"></a>解像度

型が登録されると、解決またはの依存関係として挿入することができます。 型を解決すると、コンテナーが新しいインスタンスを作成する必要がありますのインスタンスにすべての依存関係を挿入します。

一般に、型が解決されたら、次の 3 つの 1 つ発生します。

1.  型が登録されていない場合、コンテナーは例外をスローします。
1.  種類は、シングルトンとして登録されている、コンテナーは、シングルトン インスタンスを返します。 初めて、型が呼び出される場合は、コンテナーが必要な場合、それを作成しへの参照を保持します。
1.  種類は、シングルトンとして登録されていない、コンテナーは新しいインスタンスを返しますされへの参照を保持しません。

次のコード例に示す方法、 `RequestProvider` Autofac に既に登録された型を解決できます。

```csharp
var requestProvider = _container.Resolve<IRequestProvider>();
```

この例では、Autofac が具象型を解決するように求め、`IRequestProvider`と共にすべての依存関係の種類。 通常、`Resolve`特定の種類のインスタンスが必要な場合、メソッドが呼び出されます。 解決済みのオブジェクトの有効期間を制御する方法の詳細については、次を参照してください。[を解決するオブジェクトの有効期間を管理する](#managing_the_lifetime_of_resolved_objects)です。

次のコード例は、ビュー モデルの種類とその依存関係 eShopOnContainers モバイル アプリをインスタンス化する方法を示しています。

```csharp
var viewModel = _container.Resolve(viewModelType);
```

この例で Autofac が要求されたビュー モデルのビュー モデルの型を解決するように依頼し、コンテナーは、依存関係を解決してもします。 解決するときに、`ProfileViewModel`の種類、依存関係を解決するのには、`IOrderService`オブジェクト。 したがって、Autofac を構築した、`OrderService`オブジェクトし、のコンス トラクターに渡します、`ProfileViewModel`クラスです。 EShopOnContainers モバイル アプリでのビューの作成方法の詳細については、モデル化し、ビューに関連付けられますを参照してください[ビュー モデル ロケーターにビュー モデルを自動的に作成する](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically_creating_a_view_model_with_a_view_model_locator)です。

> [!NOTE]
> 登録して、コンテナー型の解決、パフォーマンスの依存関係はアプリでは、各ページ ナビゲーションの再構築されている場合に特にのリフレクションの各種類を作成するためのコンテナーの使用のためのコストがあります。 多数のまたは詳細な依存関係がある場合は、作成のコストが大幅に増加することができます。

<a name="managing_the_lifetime_of_resolved_objects" />

## <a name="managing-the-lifetime-of-resolved-objects"></a>解決済みのオブジェクトの有効期間を管理します。

型を登録すると、Autofac の既定の動作は、インスタンスを作成する新しい登録済みの型のたびに、型が解決されると、依存関係のメカニズムは他のクラスにインスタンスを挿入するとします。 このシナリオでは、コンテナーが解決されたオブジェクトへの参照を保持しません。 ただし、インスタンスを登録するときに Autofac の既定の動作はシングルトンとしてオブジェクトの有効期間を管理するには したがってのインスタンスは、コンテナーのスコープ内にあるし、コンテナーがスコープ外に出るし、ガベージ コレクション、破棄されるときに、またはコードに明示的に破棄することも、コンテナー スコープに残ります。

登録済みの型から Autofac を作成するオブジェクトの単一の動作を指定する Autofac インスタンス スコープを使用できます。 Autofac インスタンスのスコープは、コンテナーによってインスタンス化されるオブジェクトの有効期間を管理します。 インスタンスの既定のスコープ、`RegisterType`メソッドは、`InstancePerDependency`スコープ。 ただし、`SingleInstance`スコープで使用できる、`RegisterType`メソッド、コンテナーを作成するかを呼び出すときに型のシングルトン インスタンスを返すように、`Resolve`メソッドです。 次のコード例は、Autofac がのシングルトン インスタンスを作成するように指示する方法を示しています、`NavigationService`クラス。

```csharp
builder.RegisterType<NavigationService>().As<INavigationService>().SingleInstance();
```

初めて発生したとき、`INavigationService`インターフェイスは、新しいコンテナーを作成、解決`NavigationService`オブジェクトおよびオブジェクトへの参照を保持します。 後続の解像度で、`INavigationService`インターフェイス、コンテナーへの参照を返します、`NavigationService`以前に作成されたオブジェクト。

> [!NOTE]
> SingleInstance スコープは、コンテナーが破棄されるときに作成されたオブジェクトを破棄します。

Autofac には、追加のインスタンスのスコープが含まれています。 詳細については、次を参照してください。[インスタンス スコープ](http://autofac.readthedocs.io/en/latest/lifetime/instance-scope.html)readthedocs.io にします。

## <a name="summary"></a>まとめ

依存関係の挿入は、これらの型に依存するコードの具体的な種類の分離を使用できます。 通常、登録とインターフェイスおよび抽象型は、間のマッピングの一覧を保持するコンテナーと実装またはこれらの型を拡張する具象型を使用します。

Autofac、疎結合アプリの構築を容易に、すべての型マッピングおよびオブジェクトのインスタンスを登録する方法など、依存関係の注入コンテナーによく見られる機能オブジェクトを解決するには、オブジェクトの有効期間を管理および挿入解決されるオブジェクトのコンス トラクターに依存するオブジェクト。


## <a name="related-links"></a>関連リンク

- [電子 (2 Mb PDF) のダウンロードします。](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (サンプル)](https://github.com/dotnet-architecture/eShopOnContainers)
