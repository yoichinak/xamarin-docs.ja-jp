---
title: 依存関係の挿入
description: この章では、eShopOnContainers のモバイル アプリで依存関係の挿入を使用して、具象型これらの型に依存するコードからを分離する方法について説明します。
ms.prod: xamarin
ms.assetid: a150f2d1-06f8-4aed-ab4e-7a847d69f103
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 97b95ccb3e756f02c945adc63b9e173a9f9e0226
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2019
ms.locfileid: "67832683"
---
# <a name="dependency-injection"></a>依存関係の挿入

通常、クラス コンス トラクターが呼び出されるは、オブジェクトをインスタンス化するときと、オブジェクト必要のある任意の値がコンス トラクターに引数として渡されます。 これは、依存関係挿入の例と呼ばれる具体的には*コンス トラクターの挿入*します。 オブジェクトが必要な依存関係は、コンス トラクターに挿入されます。

依存関係を指定するとインターフェイスの種類として、依存関係の挿入により、これらの型に依存するコードの具体的な種類の分離します。 通常、登録とインターフェイスと抽象型は、間のマッピングの一覧を保持するコンテナーと実装またはこれらの型を拡張する具象型を使用します。

他の種類の依存関係の挿入など*プロパティ セッターの挿入*、および*メソッド呼び出しの挿入*、出現頻度の低いが、します。 そのため、この章で依存関係の注入コンテナーのコンス トラクターの挿入を実行するだけに焦点を当てます。

<a name="introduction_to_dependency_injection" />

## <a name="introduction-to-dependency-injection"></a>依存関係の挿入の概要

依存関係の挿入は、反転されている問題が必要な依存関係を取得するプロセスは、制御の反転 (IoC) パターンの特殊化されたバージョンです。 依存関係の挿入を別のクラスが実行時にオブジェクトに依存関係の注入を担当します。 次のコード例に示す方法、`ProfileViewModel`依存関係の挿入を使用する場合、クラスが構造化します。

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

`ProfileViewModel`コンス トラクターは、`IOrderService`別のクラスによって、挿入、引数としてのインスタンス。 のみの依存関係、`ProfileViewModel`クラスがインターフェイス型。 そのため、`ProfileViewModel`クラスをインスタンス化するための知識がない、`IOrderService`オブジェクト。 クラスをインスタンス化するため、`IOrderService`オブジェクト、およびに挿入すると、`ProfileViewModel`クラスと呼ばれますが、*依存関係注入コンテナー*します。

依存関係注入コンテナーでは、クラスのインスタンスをインスタンス化し、コンテナーの構成に基づいて、有効期間を管理する機能を提供することでオブジェクト間の結合を弱めます。 オブジェクトの作成時に、コンテナーは、これに、オブジェクトが必要なすべての依存関係を挿入します。 これらの依存関係は、まだ作成されていない、コンテナーは作成し、最初にその依存関係を解決します。

> [!NOTE]
> ファクトリを使用して手動で依存関係の注入を実装することもできます。 ただし、コンテナーを使用してスキャン アセンブリ登録の有効期間管理などの他の機能を提供します。

これには、依存関係注入コンテナーを使用するいくつかの利点があります。

-   コンテナーは、その依存関係を見つけて、その有効期間を管理するクラスの必要性を削除します。
-   コンテナーでは、クラスの影響を与えずに実装されている依存関係のマッピングができます。
-   コンテナーは、モックを作成する依存関係を許可することで、テストの容易性を容易にします。
-   コンテナーでは、アプリに簡単に追加する新しいクラスを許可することで保守性が向上します。

MVVM を使用した Xamarin.Forms アプリのコンテキストで依存関係の注入コンテナーは通常使用の登録およびモデルの表示を解決するため、サービスを登録すると、ビュー モデルに挿入すること。

多くの依存関係注入コンテナーは、Autofac を使用してビュー モデルのインスタンス化を管理し、サービス クラスは、アプリで、eShopOnContainers のモバイル アプリで使用できます。 Autofac、疎結合アプリの構築を容易にし、オブジェクトを解決するには、オブジェクトの有効期間を管理および挿入のすべての型のマッピングとオブジェクトのインスタンスを登録する方法など、依存関係注入コンテナーによく見られる機能を提供します。解決されるオブジェクトのコンス トラクターに依存するオブジェクト。 Autofac の詳細については、次を参照してください。 [Autofac](http://autofac.readthedocs.io/en/latest/index.html) readthedocs.io します。

Autofac で、`IContainer`インターフェイスは、依存関係注入コンテナーを提供します。 図 3-1 は、このコンテナーは、インスタンス化を使用する場合に、依存関係を示しています、`IOrderService`オブジェクトし、それを挿入、`ProfileViewModel`クラス。

![](dependency-injection-images/dependencyinjection.png "依存関係の挿入を使用する場合、依存関係の例")

**図 3-1:** 依存関係の挿入を使用する場合、依存関係

実行時に、コンテナーがのどの実装を知る必要があります、`IOrderService`インターフェイスにインスタンスを作成、インスタンスを作成できる前に、`ProfileViewModel`オブジェクト。 これが含まれます。

-   実装するオブジェクトをインスタンス化する方法を決定するコンテナー、`IOrderService`インターフェイス。 これと呼ばれます*登録*します。
-   実装するオブジェクトをインスタンス化するコンテナー、`IOrderService`インターフェイス、および`ProfileViewModel`オブジェクト。 これと呼ばれます*解決*します。

最終的には、アプリが使用を完了するが、`ProfileViewModel`オブジェクトはガベージ コレクションの使用可能になる予定です。 ガベージ コレクターがこの時点では、破棄する必要があります、`IOrderService`インスタンスの他のクラスは、同じインスタンスを共有していない場合。

> [!TIP]
> コンテナーに依存しないコードを記述します。 常に使用されている特定の依存関係コンテナーからアプリを分離するためにコンテナーに依存しないコードを記述してみてください。

## <a name="registration"></a>登録

オブジェクトには、依存関係を挿入することができます、前に、依存関係の種類は、コンテナーを登録する必要があります最初。 通常のタイプを登録するには、インターフェイスとインターフェイスを実装する具象型は、コンテナーを渡す必要があります。

型とオブジェクトを登録するコードをコンテナー内の 2 つの方法はあります。

-   型またはマッピングをコンテナーに登録します。 必要に応じて、コンテナーは、指定した型のインスタンスを作成します。
-   コンテナー内の既存のオブジェクトをシングルトンとして登録します。 必要に応じて、コンテナーは、既存のオブジェクトへの参照を返します。

> [!TIP]
> 依存関係注入コンテナーは、常に対応できません。 依存関係の挿入は、複雑になりできない可能性がある適切なや役立つ小さなアプリへの要件について説明します。 クラスは、すべての依存関係がないか、他の種類の依存関係のない場合、必要がありますいないコンテナーに配置します。 さらに、クラスは、1 つの種類に不可欠な依存関係の設定しは決して変化しない、コンテナーに配置しても意味を行わない可能性があります。

アプリケーションでは、1 つのメソッドに依存関係の挿入を必要とする型の登録を実行するかし、アプリがそのクラス間の依存関係の対応であることを確認して、アプリのライフ サイクルの早い段階では、このメソッドを呼び出す必要があります。 EShopOnContainers のモバイル アプリでこれを行う、`ViewModelLocator`クラス、ビルド、`IContainer`オブジェクトであり、そのオブジェクトへの参照を保持するアプリで唯一のクラス。 次のコード例は、eShopOnContainers のモバイル アプリを宣言する方法を示しています、`IContainer`オブジェクト、`ViewModelLocator`クラス。

```csharp
private static IContainer _container;
```

型とインスタンスに登録されている、`RegisterDependencies`メソッドで、`ViewModelLocator`クラス。 最初に作成してこれは、`ContainerBuilder`インスタンスで、次のコード例に示します。

```csharp
var builder = new ContainerBuilder();
```

型とインスタンスに登録し、`ContainerBuilder`オブジェクト、および次のコード例は、型の登録の最も一般的な形式を示します。

```csharp
builder.RegisterType<RequestProvider>().As<IRequestProvider>();
```

`RegisterType`メソッドは、ここに示すインターフェイス型を具象型にマップされます。 コンテナー インスタンスを作成するよう指示を`RequestProvider`オブジェクトの挿入が必要なオブジェクトをインスタンス化時に、`IRequestProvider`コンス トラクターを使用します。

次のコード例に示すように、具象型をインターフェイス型からマッピングなしで直接登録もできます。

```csharp
builder.RegisterType<ProfileViewModel>();
```

ときに、`ProfileViewModel`型が解決される、コンテナーは、必要な依存関係を挿入します。

Autofac では、コンテナーが、型のシングルトン インスタンスへの参照を保持する責任が、インスタンスを登録することもできます。 たとえば、次のコード例を示します、eShopOnContainers のモバイル アプリがときに使用する具象型を登録する方法、`ProfileViewModel`インスタンスが必要です、`IOrderService`インスタンス。

```csharp
builder.RegisterType<OrderService>().As<IOrderService>().SingleInstance();
```

`RegisterType`メソッドは、ここに示すインターフェイス型を具象型にマップされます。 `SingleInstance`メソッドは、すべての依存オブジェクトが同じ共有インスタンスを受信できるように、登録を構成します。 そのため、1 つだけ`OrderService`インスタンスは、挿入を必要とするオブジェクトが共有コンテナーに存在、`IOrderService`コンス トラクターを使用します。

インスタンスの登録を実行することも、`RegisterInstance`メソッドで、次のコード例に示します。

```csharp
builder.RegisterInstance(new OrderMockService()).As<IOrderService>();
```

`RegisterInstance`ここに示すメソッドが新たに作成`OrderMockService`をインスタンス化され、コンテナーに登録されます。 そのため、1 つだけ`OrderMockService`の挿入を必要とするオブジェクトが共有コンテナーに存在するインスタンス、`IOrderService`コンス トラクターを使用します。

次の種類とインスタンスの登録、`IContainer`次のコード例に示します。 これは、オブジェクトを構築する必要があります。

```csharp
_container = builder.Build();
```

呼び出す、`Build`メソッドを`ContainerBuilder`インスタンスが行われた登録を含む新しい依存関係注入コンテナーをビルドします。

> [!TIP]
> 検討してください、`IContainer`が変更可能なものとして。 Autofac を提供しますが、`Update`可能であれば、このメソッドを呼び出して、既存のコンテナーでの登録を更新する方法は避ける必要があります。 コンテナーが使用されている場合に特に構築されていますが、後にコンテナーを変更するリスクがあります。 詳細については、次を参照してください。[変更不可としてのコンテナーを検討してください。](http://docs.autofac.org/en/latest/best-practices/#consider-a-container-as-immutable) readthedocs.io します。

<a name="resolution" />

## <a name="resolution"></a>解決方法

型が登録されると、解決または依存関係として挿入することができます。 型が解決されると、コンテナーが新しいインスタンスを作成する必要があるのインスタンスにすべての依存関係を挿入します。

一般に、型が解決できたら、次の 3 つのいずれかが発生します。

1.  型が登録されていない場合、コンテナーは例外をスローします。
1.  種類は、シングルトンとして登録されている、コンテナーはシングルトン インスタンスを返します。 初めて、型が呼び出される場合は、コンテナーが必要な場合、それを作成しへの参照を保持します。
1.  種類は、シングルトンとして登録されていない、コンテナーは、新しいインスタンスを返しへの参照は保持されません。

次のコード例に示す方法、 `RequestProvider` Autofac に既に登録済みの型を解決することができます。

```csharp
var requestProvider = _container.Resolve<IRequestProvider>();
```

具象型を解決するのには、この例では Autofac がよく寄せられる、`IRequestProvider`と依存関係の種類。 通常、`Resolve`特定の種類のインスタンスが必要な場合、メソッドが呼び出されます。 解決済みのオブジェクトの有効期間を制御する方法の詳細については、次を参照してください。[解決するオブジェクトの有効期間を管理する](#managing_the_lifetime_of_resolved_objects)します。

次のコード例では、eShopOnContainers のモバイル アプリで、ビュー モデルの種類とその依存関係がどのようにインスタンス化する方法を示します。

```csharp
var viewModel = _container.Resolve(viewModelType);
```

この例では、Autofac が要求されたビュー モデル、ビュー モデルの型を解決するのにはよく寄せられるし、コンテナーは、すべての依存関係も解決されます。 解決するときに、`ProfileViewModel`の種類、依存関係を解決するのには、`IOrderService`オブジェクト。 そのため、Autofac を構築した、`OrderService`オブジェクトし、のコンス トラクターに渡されます、`ProfileViewModel`クラス。 EShopOnContainers のモバイル アプリでビューを作成する方法の詳細については、モデルし、ビューに関連付けられますを参照してください。[自動的にビュー モデルを作成するビュー モデル ロケーターと](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically_creating_a_view_model_with_a_view_model_locator)します。

> [!NOTE]
> 登録して、コンテナーを持つ型の解決は、特に依存関係は、アプリでは、各ページ ナビゲーションの再構築中の場合、各型を作成するためのリフレクションのコンテナーの使用のためコスト パフォーマンスが。 多くまたは詳細な依存関係がある場合は、作成のコストを大幅に向上できます。

<a name="managing_the_lifetime_of_resolved_objects" />

## <a name="managing-the-lifetime-of-resolved-objects"></a>解決済みのオブジェクトの有効期間を管理します。

型を登録すると、Autofac の既定の動作は、インスタンスを作成する新しい登録済みの型のたびに、型が解決されたか、依存関係のメカニズムは他のクラスにインスタンスを挿入するとします。 このシナリオでは、コンテナーが解決されたオブジェクトへの参照を保持しません。 ただし、インスタンスを登録するときに、Autofac の既定の動作は、シングルトンとしてオブジェクトの有効期間を管理するは。 そのため、コンテナーがスコープ内、およびコンテナーがスコープ外になるし、ガベージ コレクション、破棄されるときに、またはコードは、コンテナーを明示的に破棄します。 インスタンスはスコープ内に残ります。

登録済みの型から Autofac を作成するオブジェクトの単一の動作を指定する、Autofac インスタンスのスコープを使用できます。 Autofac インスタンスのスコープは、コンテナーによってインスタンス化されるオブジェクトの有効期間を管理します。 インスタンスの既定のスコープ、`RegisterType`メソッドは、`InstancePerDependency`スコープ。 ただし、`SingleInstance`スコープで使用できる、`RegisterType`メソッドでは、コンテナーを作成またはを呼び出すときに、型のシングルトン インスタンスを返すように、`Resolve`メソッド。 次のコード例は、Autofac のシングルトン インスタンスを作成するように指示する方法を示しています、`NavigationService`クラス。

```csharp
builder.RegisterType<NavigationService>().As<INavigationService>().SingleInstance();
```

最初の時間を`INavigationService`インターフェイスが解決される、新しいコンテナーを作成`NavigationService`オブジェクトし、への参照を保持します。 後続の解像度で、`INavigationService`インターフェイス、コンテナーへの参照を返します、`NavigationService`以前に作成されたオブジェクト。

> [!NOTE]
> SingleInstance スコープは、コンテナーが破棄されるときに作成されたオブジェクトを破棄します。

Autofac には、追加のインスタンスのスコープが含まれています。 詳細については、次を参照してください。[インスタンス スコープ](http://autofac.readthedocs.io/en/latest/lifetime/instance-scope.html)readthedocs.io します。

## <a name="summary"></a>Summary

依存関係の挿入は、これらの型に依存するコードの具体的な種類の分離を使用できます。 通常、登録とインターフェイスと抽象型は、間のマッピングの一覧を保持するコンテナーと実装またはこれらの型を拡張する具象型を使用します。

Autofac、疎結合アプリの構築を容易にし、オブジェクトを解決するには、オブジェクトの有効期間を管理および挿入のすべての型のマッピングとオブジェクトのインスタンスを登録する方法など、依存関係注入コンテナーによく見られる機能を提供します。解決されるオブジェクトのコンス トラクターに依存するオブジェクト。


## <a name="related-links"></a>関連リンク

- [(2 Mb の PDF) 電子ブックをダウンロードします。](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (サンプル)](https://github.com/dotnet-architecture/eShopOnContainers)
