---
title: エンタープライズアプリの単体テスト
description: この章では、eShopOnContainers モバイルアプリでの単体テストの実行方法について説明します。
ms.prod: xamarin
ms.assetid: 4af82e52-f99b-4cad-b278-1745f190c240
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: dbc863c6f0de49c809de9d13dc9c682b43e2befe
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93373953"
---
# <a name="unit-testing-enterprise-apps"></a>エンタープライズアプリの単体テスト

> [!NOTE]
> この電子ブックは2017の spring で公開されており、その後、更新されていません。 本は貴重なものですが、一部のマテリアルは古くなっています。

モバイルアプリには、デスクトップと web ベースのアプリケーションが心配する必要がない固有の問題があります。 モバイルユーザーは、使用するデバイス、ネットワーク接続、サービスの可用性、およびその他のさまざまな要因によって異なります。 そのため、モバイルアプリは、品質、信頼性、およびパフォーマンスを向上させるために、実際の環境で使用されるため、テストする必要があります。 単体テスト、統合テスト、ユーザーインターフェイステストなど、アプリで実行する必要があるテストの種類は多数あります。単体テストは、最も一般的なテスト形式です。

単体テストでは、アプリの小さな単位 (通常はメソッド) を受け取り、それをコードの残りの部分から分離し、想定どおりに動作することを確認します。 その目的は、機能の各単位が想定どおりに動作することを確認することです。これにより、エラーがアプリ全体に伝達されることがなくなります。 障害が発生したときにバグを検出する方が効率的であるため、障害のセカンダリポイントでバグの影響を間接的に観察することができます。

単体テストは、ソフトウェア開発ワークフローの不可欠な要素である場合に、コード品質に大きな影響を与えます。 メソッドが作成されるとすぐに、標準、境界、および正しくない入力データのケースに応じてメソッドの動作を検証し、コードによって行われた明示的または暗黙的な仮定をチェックする単体テストを記述する必要があります。 また、テスト駆動開発では、コードの前に単体テストが記述されます。 このシナリオでは、単体テストは設計ドキュメントと機能仕様の両方として機能します。

> [!NOTE]
> 単体テストは、問題のある更新プログラムによって処理されたものの、機能的な回帰に対して非常に効果的です。

単体テストでは、通常、次のように配置パターンを使用します。

- 単体テストメソッドの *配置* セクションでは、オブジェクトを初期化し、テスト対象のメソッドに渡されるデータの値を設定します。
- *Act* セクションでは、必須の引数を使用してテスト対象のメソッドを呼び出します。
- *Assert* セクションでは、テスト対象のメソッドのアクションが想定どおりに動作することを確認します。

このパターンに従うことで、単体テストが読み取り可能で一貫したものになります。

## <a name="dependency-injection-and-unit-testing"></a>依存関係の挿入と単体テスト

疎結合アーキテクチャを採用する動機の1つは、単体テストを容易にすることです。 Autofac に登録されている型の1つは、 `OrderService` クラスです。 次のコード例は、このクラスの概要を示しています。

```csharp
public class OrderDetailViewModel : ViewModelBase  
{  
    private IOrderService _ordersService;  

    public OrderDetailViewModel(IOrderService ordersService)  
    {  
        _ordersService = ordersService;  
    }  
    ...  
}
```

クラスは、 `OrderDetailViewModel` `IOrderService` オブジェクトをインスタンス化するときに、コンテナーが解決する型に依存してい `OrderDetailViewModel` ます。 ただし、 `OrderService` クラスの単体テストを行うためのオブジェクトを作成するのではなく、 `OrderDetailViewModel` `OrderService` オブジェクトをテストの目的のモックに置き換えます。 図10-1 は、この関係を示しています。

![IOrderService インターフェイスを実装するクラス](unit-testing-images/unittesting.png)

**図 10-1:** IOrderService インターフェイスを実装するクラス

この方法では、 `OrderService` 実行時にオブジェクトをクラスに渡すことができます `OrderDetailViewModel` 。また、テストの容易性のために、クラス `OrderMockService` をテスト時にクラスに渡すことができ `OrderDetailViewModel` ます。 この方法の主な利点は、web サービスやデータベースなどの扱いにくいリソースを必要とせずに、単体テストを実行できることです。

## <a name="testing-mvvm-applications"></a>MVVM アプリケーションのテスト

MVVM アプリケーションからモデルをテストし、モデルを表示することは、他のクラスをテストすることと同じです。単体テストやモックなど、同じツールと手法を使用できます。 ただし、モデルクラスとビューモデルクラスには、特定の単体テスト手法の恩恵を受けることができるパターンがいくつかあります。

> [!TIP]
> 単体テストごとに1つのテストを行います。 単体テストでは、単位の動作の複数の側面を使用しないようにしてください。 そうすることで、読み取りと更新が困難なテストにつながります。 また、エラーを解釈するときに混乱を招く可能性もあります。

EShopOnContainers モバイルアプリでは、 [Xunit](https://xunit.github.io/) を使用して単体テストを実行します。単体テストでは、2種類の単体テストがサポートされています。

- ファクトは常に true であり、インバリアント条件をテストするテストです。
- 理論は、特定のデータセットに対してのみ当てはまるテストです。

EShopOnContainers モバイルアプリに含まれる単体テストはファクトテストであるため、各単体テストメソッドは属性で修飾され `[Fact]` ます。

> [!NOTE]
> xUnit テストはテストランナーによって実行されます。 テストランナーを実行するには、必要なプラットフォームの eShopOnContainers プロジェクトを実行します。

### <a name="testing-asynchronous-functionality"></a>非同期機能のテスト

MVVM パターンを実装する場合、通常、ビューモデルはサービスに対する操作を非同期的に呼び出します。 これらの操作を呼び出すコードのテストでは、通常、モックを実際のサービスの置換として使用します。 次のコード例は、モックサービスをビューモデルに渡すことによって非同期機能をテストする方法を示しています。

```csharp
[Fact]  
public async Task OrderPropertyIsNotNullAfterViewModelInitializationTest()  
{  
    var orderService = new OrderMockService();  
    var orderViewModel = new OrderDetailViewModel(orderService);  

    var order = await orderService.GetOrderAsync(1, GlobalSetting.Instance.AuthToken);  
    await orderViewModel.InitializeAsync(order);  

    Assert.NotNull(orderViewModel.Order);  
}
```

この単体テストでは、 `Order` `OrderDetailViewModel` メソッドが呼び出された後に、インスタンスのプロパティに値が設定されていることを確認 `InitializeAsync` します。 メソッドは、 `InitializeAsync` ビューモデルの対応するビューに移動したときに呼び出されます。 ナビゲーションの詳細については、「 [ナビゲーション](~/xamarin-forms/enterprise-application-patterns/navigation.md)」を参照してください。

インスタンスが作成されるときに `OrderDetailViewModel` は、 `OrderService` インスタンスが引数として指定されることを想定しています。 ただし、は、 `OrderService` web サービスからデータを取得します。 したがって、 `OrderMockService` クラスのモックバージョンであるインスタンスは、 `OrderService` コンストラクターの引数として指定され `OrderDetailViewModel` ます。 次に、ビューモデルの `InitializeAsync` メソッドが呼び出され、操作が呼び出されると、 `IOrderService` web サービスと通信するのではなく、モックデータが取得されます。

### <a name="testing-inotifypropertychanged-implementations"></a>INotifyPropertyChanged 実装のテスト

インターフェイスを実装する `INotifyPropertyChanged` ことで、ビューモデルおよびモデルから生じる変更にビューを対応させることができます。 これらの変更は、コントロールに表示されるデータに限定されません。アニメーションを開始したり、コントロールを無効にしたりするビューモデルの状態など、ビューの制御にも使用されます。

単体テストによって直接更新できるプロパティをテストするには、イベントハンドラーをイベントにアタッチ `PropertyChanged` し、プロパティの新しい値を設定した後にイベントが発生するかどうかを確認します。 次のコード例は、このようなテストを示しています。

```csharp
[Fact]  
public async Task SettingOrderPropertyShouldRaisePropertyChanged()  
{  
    bool invoked = false;  
    var orderService = new OrderMockService();  
    var orderViewModel = new OrderDetailViewModel(orderService);  

    orderViewModel.PropertyChanged += (sender, e) =>  
    {  
        if (e.PropertyName.Equals("Order"))  
            invoked = true;  
    };  
    var order = await orderService.GetOrderAsync(1, GlobalSetting.Instance.AuthToken);  
    await orderViewModel.InitializeAsync(order);  

    Assert.True(invoked);  
}
```

この単体テストでは、 `InitializeAsync` クラスのメソッドを呼び出します `OrderViewModel` 。これにより、プロパティが `Order` 更新されます。 プロパティに対してイベントが発生した場合、単体テストは成功し `PropertyChanged` `Order` ます。

### <a name="testing-message-based-communication"></a>メッセージベースの通信のテスト

クラスを使用して [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) 疎結合クラス間の通信を行うビューモデルは、次のコード例に示すように、テスト対象のコードによって送信されるメッセージをサブスクライブすることによって、単体テストを行うことができます。

```csharp
[Fact]  
public void AddCatalogItemCommandSendsAddProductMessageTest()  
{  
    bool messageReceived = false;  
    var catalogService = new CatalogMockService();  
    var catalogViewModel = new CatalogViewModel(catalogService);  

    Xamarin.Forms.MessagingCenter.Subscribe<CatalogViewModel, CatalogItem>(  
        this, MessageKeys.AddProduct, (sender, arg) =>  
    {  
        messageReceived = true;  
    });  
    catalogViewModel.AddCatalogItemCommand.Execute(null);  

    Assert.True(messageReceived);  
}
```

この単体テストでは、が `CatalogViewModel` `AddProduct` 実行されたことに応じて、がメッセージを公開することを確認し `AddCatalogItemCommand` ます。 クラスは [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) マルチキャストメッセージサブスクリプションをサポートしているので、単体テストはメッセージをサブスクライブ `AddProduct` し、受信に応答してコールバックデリゲートを実行できます。 このコールバックデリゲートは、ラムダ式として指定され、 `boolean` `Assert` テストの動作を検証するためにステートメントによって使用されるフィールドを設定します。

### <a name="testing-exception-handling"></a>例外処理のテスト

次のコード例に示すように、無効なアクションまたは入力に対して特定の例外がスローされることを確認する単体テストを記述することもできます。

```csharp
[Fact]  
public void InvalidEventNameShouldThrowArgumentExceptionText()  
{  
    var behavior = new MockEventToCommandBehavior  
    {  
        EventName = "OnItemTapped"  
    };  
    var listView = new ListView();  

    Assert.Throws<ArgumentException>(() => listView.Behaviors.Add(behavior));  
}
```

この単体テストは例外をスローします。これは、 [`ListView`](xref:Xamarin.Forms.ListView) コントロールにという名前のイベントがないため `OnItemTapped` です。 メソッドは、 `Assert.Throws<T>` `T` が予期される例外の型であるジェネリックメソッドです。 メソッドに渡される引数 `Assert.Throws<T>` は、例外をスローするラムダ式です。 したがって、ラムダ式からがスローされた場合、単体テストは成功し `ArgumentException` ます。

> [!TIP]
> 例外メッセージ文字列を調べる単体テストを記述することは避けてください。 例外メッセージ文字列は時間の経過と共に変わる可能性があります。そのため、その存在に依存する単体テストは、不安定であると見なされます。

### <a name="testing-validation"></a>検証のテスト

検証の実装をテストするには、検証規則が正しく実装されていることをテストし、 `ValidatableObject<T>` クラスが期待どおりに動作することをテストするという2つの側面があります。

通常、検証ロジックは、出力が入力に依存する自己完結型のプロセスであるため、テストが簡単です。 `Validate`次のコード例に示すように、少なくとも1つの関連する検証規則を持つ各プロパティに対してメソッドを呼び出した結果に対してテストを行う必要があります。

```csharp
[Fact]  
public void CheckValidationPassesWhenBothPropertiesHaveDataTest()  
{  
    var mockViewModel = new MockViewModel();  
    mockViewModel.Forename.Value = "John";  
    mockViewModel.Surname.Value = "Smith";  

    bool isValid = mockViewModel.Validate();  

    Assert.True(isValid);  
}
```

この単体テストでは、インスタンスの2つのプロパティの両方にデータがある場合に、検証が成功するかどうかを確認 `ValidatableObject<T>` `MockViewModel` します。

検証単体テストでは、検証が成功したかどうかを確認するだけでなく、各インスタンスの、、およびプロパティの値を確認して、 `Value` `IsValid` `Errors` `ValidatableObject<T>` クラスが想定どおりに動作することを確認する必要もあります。 次のコード例は、これを実行する単体テストを示しています。

```csharp
[Fact]  
public void CheckValidationFailsWhenOnlyForenameHasDataTest()  
{  
    var mockViewModel = new MockViewModel();  
    mockViewModel.Forename.Value = "John";  

    bool isValid = mockViewModel.Validate();  

    Assert.False(isValid);  
    Assert.NotNull(mockViewModel.Forename.Value);  
    Assert.Null(mockViewModel.Surname.Value);  
    Assert.True(mockViewModel.Forename.IsValid);  
    Assert.False(mockViewModel.Surname.IsValid);  
    Assert.Empty(mockViewModel.Forename.Errors);  
    Assert.NotEmpty(mockViewModel.Surname.Errors);  
}
```

この単体テストで `Surname` は、のプロパティにデータが含まれておらず、 `MockViewModel` `Value` `IsValid` `Errors` 各インスタンスの、、およびプロパティ `ValidatableObject<T>` が正しく設定されている場合に、検証が失敗することを確認します。

## <a name="summary"></a>まとめ

単体テストでは、アプリの小さな単位 (通常はメソッド) を受け取り、それをコードの残りの部分から分離し、想定どおりに動作することを確認します。 その目的は、機能の各単位が想定どおりに動作することを確認することです。これにより、エラーがアプリ全体に伝達されることがなくなります。

依存オブジェクトを、依存オブジェクトの動作をシミュレートするモックオブジェクトに置き換えることにより、テスト対象のオブジェクトの動作を分離できます。 これにより、web サービスやデータベースなどの扱いにくいリソースを必要とせずに、単体テストを実行できるようになります。

MVVM アプリケーションからモデルをテストし、モデルを表示することは、他のクラスのテストと同じであり、同じツールと手法を使用できます。

## <a name="related-links"></a>関連リンク

- [電子ブックのダウンロード (2 Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (サンプル)](https://github.com/dotnet-architecture/eShopOnContainers)
