---
title: 依存関係の挿入
description: この章では、eShopOnContainers モバイルアプリが依存関係の挿入を使用して、これらの型に依存するコードから具象型を分離する方法について説明します。
ms.prod: xamarin
ms.assetid: a150f2d1-06f8-4aed-ab4e-7a847d69f103
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 72ffa4508f2c8f050f505313a28ce8278f2570b4
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70760266"
---
# <a name="dependency-injection"></a>依存関係の挿入

通常、クラスコンストラクターは、オブジェクトをインスタンス化するときに呼び出され、オブジェクトが必要とする値は、コンストラクターに引数として渡されます。 これは依存関係の挿入の例であり、特に*コンストラクターインジェクション*と呼ばれています。 オブジェクトが必要とする依存関係は、コンストラクターに挿入されます。

依存関係をインターフェイス型として指定することにより、依存関係の挿入によって、これらの型に依存するコードから具象型を分離できます。 一般的には、インターフェイスと抽象型の間の登録とマッピングのリストを保持するコンテナーと、これらの型を実装または拡張する具象型を使用します。

また、*プロパティセッターの挿入*や*メソッド呼び出しの挿入*など、他の種類の依存関係の注入もありますが、あまり一般的ではありません。 したがって、この章では、依存関係挿入コンテナーを使用したコンストラクターの挿入についてのみ説明します。

<a name="introduction_to_dependency_injection" />

## <a name="introduction-to-dependency-injection"></a>依存関係の挿入の概要

依存関係の挿入は、制御の反転 (IoC) パターンの特殊なバージョンであり、逆の懸念は、必要な依存関係を取得するプロセスです。 依存関係の挿入では、実行時に別のクラスがオブジェクトに依存関係を挿入します。 次のコード例では、 `ProfileViewModel`依存関係の挿入を使用する場合にクラスがどのように構造化されているかを示します。

```csharp
public class ProfileViewModel : ViewModelBase  
{  
    private IOrderService _orderService;  

    public ProfileViewModel(IOrderService orderService)  
    {  
        _orderService = orderService;  
    }  
    ...  
}
```

コンストラクター `ProfileViewModel`は、別`IOrderService`のクラスによって挿入されたインスタンスを引数として受け取ります。 `ProfileViewModel`クラスの唯一の依存関係は、インターフェイス型にあります。 そのため、 `ProfileViewModel`クラスには、 `IOrderService`オブジェクトのインスタンス化を行うクラスに関する知識がありません。 `IOrderService`オブジェクトのインスタンス化と`ProfileViewModel`クラスへの挿入を行うクラスは、*依存関係の挿入コンテナー*と呼ばれます。

依存関係挿入コンテナーは、クラスインスタンスをインスタンス化し、コンテナーの構成に基づいて有効期間を管理する機能を提供することで、オブジェクト間の結合を減らします。 オブジェクトの作成中に、コンテナーは、オブジェクトに必要なすべての依存関係を挿入します。 これらの依存関係がまだ作成されていない場合、コンテナーはまず依存関係を作成して解決します。

> [!NOTE]
> 依存関係の挿入は、ファクトリを使用して手動で実装することもできます。 ただし、コンテナーを使用すると、有効期間管理、アセンブリスキャンによる登録などの追加機能が提供されます。

依存関係挿入コンテナーを使用することには、いくつかの利点があります。

- コンテナーを使用すると、クラスの依存関係を特定し、その有効期間を管理する必要がなくなります。
- コンテナーでは、クラスに影響を与えることなく、実装された依存関係のマッピングを行うことができます。
- コンテナーは、依存関係をモックできるようにすることで、テストの容易性を向上させます。
- コンテナーは、新しいクラスを簡単にアプリに追加できるようにすることで、保守性を高めます。

MVVM を使用する Xamarin. Forms アプリのコンテキストでは、通常、ビューモデルの登録と解決、およびサービスの登録とビューモデルへの挿入に、依存関係の挿入コンテナーが使用されます。

EShopOnContainers モバイルアプリでは、Autofac を使用してアプリ内のビューモデルとサービスクラスのインスタンス化を管理することで、多くの依存関係挿入コンテナーを利用できます。 Autofac は疎結合アプリの構築を容易にし、依存関係挿入コンテナーによく見られるすべての機能を提供します。これには、型マッピングとオブジェクトインスタンスの登録、オブジェクトの解決、オブジェクトの有効期間の管理、挿入などのメソッドが含まれます。解決するオブジェクトのコンストラクターに依存するオブジェクト。 Autofac の詳細については、「 [Autofac](http://autofac.readthedocs.io/en/latest/index.html) on readthedocs.io」を参照してください。

Autofac では、 `IContainer`インターフェイスに依存関係挿入コンテナーが用意されています。 図3-1 は、このコンテナーを使用する場合の依存関係`IOrderService`を示しています。 `ProfileViewModel`これにより、オブジェクトがインスタンス化され、クラスに挿入されます。

![](dependency-injection-images/dependencyinjection.png "依存関係の挿入を使用する場合の依存関係の例")

**図 3-1:** 依存関係の挿入を使用する場合の依存関係

実行時に、コンテナーは、オブジェクトを`IOrderService` `ProfileViewModel`インスタンス化する前に、インスタンス化する必要があるインターフェイスの実装を認識している必要があります。 これには次のものが含まれます。

- インターフェイスを`IOrderService`実装するオブジェクトをインスタンス化する方法を決定するコンテナー。 これは*登録*と呼ばれます。
- `IOrderService`インターフェイスを実装するオブジェクト`ProfileViewModel`とオブジェクトをインスタンス化するコンテナー。 これを*解決*と呼びます。

最終的には、アプリケーションはオブジェクトの`ProfileViewModel`使用を終了し、ガベージコレクションに使用できるようになります。 この時点で、他のクラスが同じインスタンスを`IOrderService`共有していない場合、ガベージコレクターはインスタンスを破棄する必要があります。

> [!TIP]
> コンテナーに依存しないコードを記述します。 使用されている特定の依存関係コンテナーからアプリを分離するために、常にコンテナーに依存しないコードを記述してみてください。

## <a name="registration"></a>登録

依存関係をオブジェクトに挿入する前に、依存関係の型をまずコンテナーに登録する必要があります。 型を登録するには、通常、インターフェイスとインターフェイスを実装する具象型をコンテナーに渡す必要があります。

コードを使用してコンテナーに型とオブジェクトを登録するには、次の2つの方法があります。

- コンテナーに型またはマッピングを登録します。 必要に応じて、コンテナーは、指定された型のインスタンスを構築します。
- コンテナー内の既存のオブジェクトをシングルトンとして登録します。 必要に応じて、コンテナーは既存のオブジェクトへの参照を返します。

> [!TIP]
> 依存関係挿入コンテナーは、常に適切であるとは限りません。 依存関係の注入により、小規模なアプリには適していない、または役に立たない可能性がある追加の複雑さと要件が生じます。 クラスに依存関係がない場合、または他の型に依存していない場合は、コンテナーに配置しても意味がない可能性があります。 さらに、クラスには、型に不可欠な依存関係のセットが1つだけあり、変更されない場合もあります。コンテナーに配置するのが理にかなっているとは限りません。

依存関係の挿入を必要とする型の登録は、アプリの1つのメソッドで実行する必要があります。このメソッドは、アプリがそのクラス間の依存関係を認識できるように、アプリのライフサイクルの早い段階で呼び出す必要があります。 EShopOnContainers mobile アプリでは、これは`ViewModelLocator`クラスによって実行されます。このクラスは、 `IContainer`オブジェクトをビルドします。これは、そのオブジェクトへの参照を保持する、アプリ内の唯一のクラスです。 次のコード例は、eShopOnContainers モバイルアプリが`IContainer` `ViewModelLocator`クラスのオブジェクトを宣言する方法を示しています。

```csharp
private static IContainer _container;
```

型とインスタンスは、 `RegisterDependencies` `ViewModelLocator`クラスのメソッドに登録されます。 これを実現するには、 `ContainerBuilder`最初にインスタンスを作成します。これを次のコード例に示します。

```csharp
var builder = new ContainerBuilder();
```

その後、型とインスタンスが`ContainerBuilder`オブジェクトに登録されます。また、次のコード例は、型登録の最も一般的な形式を示しています。

```csharp
builder.RegisterType<RequestProvider>().As<IRequestProvider>();
```

次`RegisterType`に示すメソッドは、インターフェイス型を具象型にマップします。 このメソッドは、コンストラクターに`RequestProvider` `IRequestProvider`よるの挿入を必要とするオブジェクトをインスタンス化するときに、オブジェクトをインスタンス化するようにコンテナーに指示します。

次のコード例に示すように、具象型は、インターフェイス型からのマッピングなしで直接登録することもできます。

```csharp
builder.RegisterType<ProfileViewModel>();
```

`ProfileViewModel`型が解決されると、コンテナーは必要な依存関係を挿入します。

Autofac では、インスタンスを登録することもできます。この場合、コンテナーは、型のシングルトンインスタンスへの参照を保持します。 たとえば、次のコード例では、インスタンスに`ProfileViewModel` `IOrderService`インスタンスが必要な場合に、eShopOnContainers mobile アプリが具象型を登録して使用する方法を示しています。

```csharp
builder.RegisterType<OrderService>().As<IOrderService>().SingleInstance();
```

次`RegisterType`に示すメソッドは、インターフェイス型を具象型にマップします。 メソッド`SingleInstance`は、すべての依存オブジェクトが同じ共有インスタンスを受け取るように登録を構成します。 したがって、コンテナー内`OrderService`に存在するインスタンスは1つだけです。これは、コンストラクターを介してを`IOrderService`挿入する必要があるオブジェクトによって共有されます。

インスタンスの登録は、次のコード`RegisterInstance`例に示すように、メソッドを使用して実行することもできます。

```csharp
builder.RegisterInstance(new OrderMockService()).As<IOrderService>();
```

次`RegisterInstance`に示すメソッドは、新しい`OrderMockService`インスタンスを作成し、コンテナーに登録します。 したがって、コンテナー内`OrderMockService`に存在するインスタンスは1つだけです。これは、コンストラクターを介して`IOrderService`を挿入する必要があるオブジェクトによって共有されます。

次のコード例に示すよう`IContainer`に、型とインスタンスの登録で、オブジェクトをビルドする必要があります。

```csharp
_container = builder.Build();
```

インスタンスでメソッドを`Build`呼び出すと、作成された登録を含む新しい依存関係挿入コンテナーが作成されます。 `ContainerBuilder`

> [!TIP]
> を`IContainer`変更不可と見なします。 Autofac には既存`Update`のコンテナーの登録を更新するメソッドが用意されていますが、可能な限りこのメソッドを呼び出さないでください。 コンテナーがビルドされた後、特にコンテナーが使用されている場合は、その変更に関するリスクがあります。 詳細については、「[コンテナーを readthedocs.io の変更不可と見なす](http://docs.autofac.org/en/latest/best-practices/#consider-a-container-as-immutable)」を参照してください。

<a name="resolution" />

## <a name="resolution"></a>解決策

型が登録されると、依存関係として解決または挿入されることがあります。 型が解決され、コンテナーが新しいインスタンスを作成する必要がある場合、そのインスタンスに依存関係が挿入されます。

一般に、型が解決されると、次の3つのうちのいずれかが発生します。

1. 型が登録されていない場合、コンテナーは例外をスローします。
1. 型がシングルトンとして登録されている場合、コンテナーはシングルトンインスタンスを返します。 この型がに対して初めて呼び出される場合は、必要に応じてコンテナーによって作成され、参照が保持されます。
1. 型がシングルトンとして登録されていない場合、コンテナーは新しいインスタンスを返し、そのインスタンスへの参照を保持しません。

次のコード例は、以前`RequestProvider`に Autofac に登録されていた型を解決できるかどうかを示しています。

```csharp
var requestProvider = _container.Resolve<IRequestProvider>();
```

この例では、Autofac は、 `IRequestProvider`型の具象型と依存関係を解決するように求められます。 通常、 `Resolve`メソッドは、特定の型のインスタンスが必要な場合に呼び出されます。 解決されたオブジェクトの有効期間の制御については、「[解決済みオブジェクトの有効期間の管理](#managing_the_lifetime_of_resolved_objects)」を参照してください。

次のコード例は、eShopOnContainers モバイルアプリがビューモデルの種類とその依存関係をインスタンス化する方法を示しています。

```csharp
var viewModel = _container.Resolve(viewModelType);
```

この例では、Autofac は、要求されたビューモデルのビューモデルの種類を解決するように求められます。また、コンテナーも依存関係を解決します。 `ProfileViewModel`型を解決する場合、解決する依存関係`IOrderService`はオブジェクトです。 したがって、Autofac は最初`OrderService`にオブジェクトを構築し、次にそのオブジェクト`ProfileViewModel`をクラスのコンストラクターに渡します。 EShopOnContainers mobile アプリがビューモデルを構築してビューに関連付ける方法の詳細については、「[ビューモデルロケーターを使用したビューモデルの自動作成](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically_creating_a_view_model_with_a_view_model_locator)」を参照してください。

> [!NOTE]
> コンテナーで型を登録し、解決すると、パフォーマンス コストが発生します。特に、アプリのページ ナビゲーションごとに依存関係が再構築される場合には、各型を作成するために、コンテナーでリフレクションが使用されるからです。 依存関係が多いか深い場合、作成コストが大幅に増加する可能性があります。

<a name="managing_the_lifetime_of_resolved_objects" />

## <a name="managing-the-lifetime-of-resolved-objects"></a>解決されたオブジェクトの有効期間の管理

型を登録した後、Autofac の既定の動作では、型が解決されるたび、または依存関係メカニズムによって他のクラスにインスタンスが挿入されるときに、登録された型の新しいインスタンスが作成されます。 このシナリオでは、コンテナーは解決されたオブジェクトへの参照を保持していません。 ただし、インスタンスを登録する場合、Autofac の既定の動作では、オブジェクトの有効期間がシングルトンとして管理されます。 したがって、インスタンスはコンテナーがスコープ内にある間はスコープ内に残り、コンテナーがスコープ外に出るか、ガベージコレクションされるか、またはコードによってコンテナーが明示的に破棄されるときに破棄されます。

Autofac インスタンススコープは、Autofac が登録された型から作成したオブジェクトのシングルトン動作を指定するために使用できます。 Autofac インスタンススコープは、コンテナーによってインスタンス化されるオブジェクトの有効期間を管理します。 `RegisterType`メソッドの既定のインスタンススコープは、 `InstancePerDependency`スコープです。 ただし、 `RegisterType`スコープをメソッドと共に使用して、コンテナーがメソッドの`Resolve`呼び出し時に型のシングルトンインスタンスを作成または返すようにすることができます。 `SingleInstance` 次のコード例は、 `NavigationService`クラスのシングルトンインスタンスを作成するように Autofac に指示する方法を示しています。

```csharp
builder.RegisterType<NavigationService>().As<INavigationService>().SingleInstance();
```

最初に`INavigationService`インターフェイスを解決すると、コンテナーは新しい`NavigationService`オブジェクトを作成し、そのオブジェクトへの参照を保持します。 `INavigationService`インターフェイスの後続の解決策では、コンテナーは、以前に作成`NavigationService`されたオブジェクトへの参照を返します。

> [!NOTE]
> SingleInstance スコープは、コンテナーが破棄されるときに、作成されたオブジェクトを破棄します。

Autofac には、追加のインスタンススコープが含まれています。 詳細については、「readthedocs.io の[インスタンススコープ](http://autofac.readthedocs.io/en/latest/lifetime/instance-scope.html)」を参照してください。

## <a name="summary"></a>Summary

依存関係の挿入を使用すると、これらの型に依存するコードから具象型を切り離すことができます。 通常は、インターフェイスと抽象型の間の登録とマッピングのリストを保持するコンテナーと、これらの型を実装または拡張する具象型を使用します。

Autofac は疎結合アプリの構築を容易にし、依存関係挿入コンテナーによく見られるすべての機能を提供します。これには、型マッピングとオブジェクトインスタンスの登録、オブジェクトの解決、オブジェクトの有効期間の管理、挿入などのメソッドが含まれます。解決するオブジェクトのコンストラクターに依存するオブジェクト。

## <a name="related-links"></a>関連リンク

- [電子ブックのダウンロード (2 Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (サンプル)](https://github.com/dotnet-architecture/eShopOnContainers)
