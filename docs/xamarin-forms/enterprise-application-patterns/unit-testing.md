---
title: 単体テストのエンタープライズ アプリ
description: この章では、eShopOnContainers のモバイル アプリでの単体テストの実行方法について説明します。
ms.prod: xamarin
ms.assetid: 4af82e52-f99b-4cad-b278-1745f190c240
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 02aeedd5498c47950e2fbc0d218de05bc0bb3204
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998684"
---
# <a name="unit-testing-enterprise-apps"></a>単体テストのエンタープライズ アプリ

モバイル アプリでは、デスクトップと web ベースのアプリケーションについて心配する必要はありません固有の問題があります。 モバイル ユーザーを使用する、ネットワーク接続、サービスの可用性、およびその他の要因の範囲でのデバイスによって異なります。 そのため、その品質、信頼性、およびパフォーマンスを向上させるために、実際の使用は、モバイル アプリをテストしてください。 これは、多くの種類のテスト、単体テスト、統合テスト、テスト、単体テストのテストの最も一般的な形式の中のユーザー インターフェイスなどのアプリで実行する必要があります。

単体テストはメソッドでは通常、アプリの小さな単位には、コードの残りの部分から分離、および期待どおりに動作することを確認します。 その目標では、エラーは、アプリ全体に伝達しないようにに、機能の各ユニットで予想どおりを実行するを確認します。 効率的なが発生したバグを検出することは、セカンダリ障害点で間接的にバグの影響を観察します。

単体テストはコードの品質に最大の効果は、ソフトウェア開発ワークフローの整数の一部である場合。 メソッドが書き込まれるとすぐに、コードによる明示的または暗黙的な前提は、メソッドは、standard、境界、および入力データの不適切な場合に応答し、そのチェックの動作を検証する単体テストを記述する必要があります。 または、テスト駆動開発では、単体テストはコードの前に書き込まれます。 このシナリオでは、単体テストは、設計ドキュメントと機能仕様の両方として機能します。

> [!NOTE]
> 単体テストは、回帰 – を操作するため、障害のある更新プログラムによって乱されるされましたの機能に対して非常に効果的です。

単体テストは、通常、arrange、act アサート パターンを使用します。

-   *配置*単体テスト メソッドのセクションでは、オブジェクトを初期化し、テスト対象のメソッドに渡されるデータの値を設定します。
-   *機能*セクションが必須の引数でテスト対象のメソッドを呼び出します。
-   *アサート*セクションでは、テスト対象のメソッドの操作が期待どおりに動作することを確認します。

このパターンに従うにより単体テストは、読み取り可能で、一貫性のあるようになります。

## <a name="dependency-injection-and-unit-testing"></a>依存関係の挿入と単体テスト

疎結合アーキテクチャを採用する動機の 1 つは、単体テストが容易にします。 Autofac 登録されている型の 1 つは、`OrderService`クラス。 次のコード例では、このクラスの概要を示します。

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

`OrderDetailViewModel`クラスに依存している、`IOrderService`コンテナー解決がインスタンス化する型、`OrderDetailViewModel`オブジェクト。 ただし、作成するのではなく、`OrderService`単体テストをオブジェクト、`OrderDetailViewModel`クラスが代わりに、置換、`OrderService`でテストするために、モック オブジェクト。 図 10-1 は、この関係を示します。

![](unit-testing-images/unittesting.png "IOrderService インターフェイスを実装するクラス")

**図 10-1:** IOrderService インターフェイスを実装するクラス

このアプローチにより、`OrderService`に渡されるオブジェクトを`OrderDetailViewModel`クラス、実行時およびテストの容易性の利益のことができます、`OrderMockService`クラスに渡される、`OrderDetailViewModel`テスト時にクラス。 このアプローチの主な利点は、web サービスやデータベースなどの扱いリソースを必要とせずに実行される単体テストを行えることです。

## <a name="testing-mvvm-applications"></a>MVVM アプリケーションのテスト

モデルと MVVM アプリケーションからビュー モデルのテストは、他のクラスをテストと同じと同じツールと – 単体テストとモック作成などの手法を使用できます。 ただしがいくつかのモデルに一般的なパターンとビュー モデル クラスの特定の単体テスト テクニックから利点を得られることができます。

> [!TIP]
> 各単体テストを 1 つのことをテストします。 演習ユニットの動作の 1 つ以上の側面をテスト単体をしたくなる必要はありません。 これは、読み取りと更新が困難なテストにつながります。 エラーを解釈するときに、混乱を招くこと可能性もします。

EShopOnContainers のモバイル アプリでは[xUnit](https://xunit.github.io/) 2 つの異なる種類の単体テストをサポートしている単体テストを実行します。

-   ファクトが true の場合、常にテストは、一定の条件をテストします。
-   理論は、特定のデータ セットの場合は true。 されるテストです。

EShopOnContainers のモバイル アプリに含まれている単体テストは、ファクトのテスト、およびで各単体テスト メソッドを修飾するため、`[Fact]`属性。

> [!NOTE]
> xUnit テストがテスト ランナーによって実行されます。 テスト ランナーを実行するには、必要なプラットフォームの eShopOnContainers.TestRunner プロジェクトを実行します。

### <a name="testing-asynchronous-functionality"></a>非同期機能をテストします。

MVVM パターンを実装するときにビュー モデルは、通常の操作を呼び出すサービス、多くの場合、非同期的にします。 通常これらの操作を呼び出すコードのテストでは、実際のサービスの代わりにモックを使用します。 次のコード例では、モック サービスをビュー モデルに渡すことによって非同期機能のテストを示しています。

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

この単体テストでは、ことを確認、`Order`のプロパティ、`OrderDetailViewModel`インスタンスの後の値になります、`InitializeAsync`メソッドが呼び出されています。 `InitializeAsync`ビュー モデルの対応するビューを移動するメソッドが呼び出されます。 ナビゲーションの詳細については、[ナビゲーション](~/xamarin-forms/enterprise-application-patterns/navigation.md)を参照してください。

ときに、`OrderDetailViewModel`インスタンスを作成すると、想定を`OrderService`を引数として指定するインスタンス。 ただし、 `OrderService` web サービスからデータを取得します。 そのため、`OrderMockService`モック バージョンでは、インスタンスの`OrderService`クラスへの引数として指定されて、`OrderDetailViewModel`コンス トラクター。 その後、ビュー モデルの`InitializeAsync`メソッドが呼び出されるを呼び出します`IOrderService`操作、モック データは、web サービスと通信するのではなく、取得しました。

### <a name="testing-inotifypropertychanged-implementations"></a>INotifyPropertyChanged の実装をテストします。

実装する、`INotifyPropertyChanged`インターフェイスにより、ビューから発信された変更に対応するためのビュー モデルとモデル。 これらの変更をコントロールに表示されるデータに限定されない – は、ビュー、ビュー モデルの状態を開始するアニメーションまたはコントロールを無効にする原因になるなどの制御にも使用します。

イベント ハンドラーをアタッチすることにより、単体テストで直接更新できるプロパティをテストすることができます、`PropertyChanged`イベントとプロパティの新しい値を設定した後、イベントが発生したかどうかをチェックします。 次のコード例では、このようなテストを示します。

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

この単体テストを呼び出す、`InitializeAsync`のメソッド、`OrderViewModel`クラス、原因となるその`Order`プロパティを更新します。 単体テストが合格で提供される、`PropertyChanged`のイベントは、`Order`プロパティ。

### <a name="testing-message-based-communication"></a>メッセージ ベース通信のテスト

ビュー モデルを使用して、 [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter)疎結合のクラス間で単体テストできる次のコード例に示すように、テスト対象のコードによって送信されるメッセージをサブスクライブして通信するクラス。

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

この単体テストでは、ことを確認、`CatalogViewModel`発行、`AddProduct`への応答メッセージの`AddCatalogItemCommand`実行されています。 [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter)マルチキャストのメッセージのサブスクリプションをサポートするクラス、単体テストを購読できる、`AddProduct`メッセージし、応答を受信するコールバック デリゲートを実行します。 このコールバック デリゲート、ラムダ式として指定された設定を`boolean`によって使用されるフィールド、`Assert`テストの動作を確認するステートメント。

### <a name="testing-exception-handling"></a>例外処理のテスト

単体テストが次のコード例に示すように対して無効な操作や入力、特定の例外がスローされるチェックは記述こともできます。

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

単体テストはこのため、例外がスローされます、 [ `ListView` ](xref:Xamarin.Forms.ListView)コントロールには、という名前のイベントはありません。`OnItemTapped`します。 `Assert.Throws<T>`メソッドがジェネリック メソッドで`T`予期される例外の種類です。 渡される引数、`Assert.Throws<T>`メソッドが例外をスローするラムダ式。 ラムダ式がスローされるときに単体テストがそのため、渡す、`ArgumentException`します。

>💡 **ヒント:**: 例外メッセージ文字列を確認する単体テストを記述しないようにします。 例外メッセージ文字列を時間の経過と共に変更する可能性があり、その存在に依存する単体テストがの脆弱なとして見なされるためです。

### <a name="testing-validation"></a>検証のテスト

検証の実装をテストする 2 つの側面があります: 規則が正しく実行されたことのテストとテストを`ValidatableObject<T>`クラスは、期待どおりに実行します。

検証ロジックをテストするには、通常は単純な出力が入力に依存する、自己完結型のプロセスでは通常です。 呼び出しの結果をテストにする必要がありますが、`Validate`メソッドを次のコード例に示すように、少なくとも 1 つの関連付けられている検証規則を持つ各プロパティ。

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

この単体テストは、ときに検証が成功したことを確認します。 2 つ`ValidatableObject<T>`プロパティで、`MockViewModel`両方のインスタンス データが存在します。

検証が成功したことを確認するには、および単体テストを検証する必要がありますもの値を確認、 `Value`、`IsValid`と`Errors`の各プロパティ`ValidatableObject<T>`インスタンスは、クラスが予想どおりに実行することを確認します。 次のコード例では、これを行う単体テストを示しています。

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

この単体テストは、ときに検証が失敗したことを確認します。、`Surname`のプロパティ、`MockViewModel`任意のデータがないと、 `Value`、 `IsValid`、および`Errors`の各プロパティ`ValidatableObject<T>`インスタンスが正しく設定されています。

## <a name="summary"></a>まとめ

単体テストはメソッドでは通常、アプリの小さな単位には、コードの残りの部分から分離、および期待どおりに動作することを確認します。 その目標では、エラーは、アプリ全体に伝達しないようにに、機能の各ユニットで予想どおりを実行するを確認します。

依存オブジェクトの動作をシミュレートするモック オブジェクトと依存オブジェクトを置き換えることで、テスト対象のオブジェクトの動作を分離することができます。 これにより、web サービスやデータベースなどの扱いリソースを必要とせずに実行される単体テストができます。

モデルと MVVM アプリケーションからビュー モデルのテストは、他のクラスをテストと同じと同じツールと手法を使用できます。


## <a name="related-links"></a>関連リンク

- [(2 Mb の PDF) 電子ブックをダウンロードします。](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (サンプル)](https://github.com/dotnet-architecture/eShopOnContainers)
