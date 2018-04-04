---
title: 単体テスト
ms.prod: xamarin
ms.assetid: 4af82e52-f99b-4cad-b278-1745f190c240
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 57201a32f5ffc0ae962f6db851a25a737e1cb17d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="unit-testing"></a>単体テスト

モバイル アプリでは、デスクトップと web ベースのアプリケーションについて心配する必要がない一意の問題があります。 モバイル ユーザーを使用する、ネットワーク接続によって、サービスの可用性、およびその他の要因の範囲、デバイスによって異なります。 そのため、その品質、信頼性、およびパフォーマンスを向上させるために、実際の使用は、モバイル アプリをテストしてください。 単体テスト、統合テスト、テスト、単体テストのテストの最も一般的な形式の中のユーザー インターフェイスなどのアプリで実行されるテストの多くの種類があります。

単体テスト メソッドでは通常、アプリの小さい単位を取得するには、コードの残りの部分から分離および期待どおりに動作することを確認します。 その目的は、機能の各単位と期待どおりに、エラーは、アプリ全体に伝達されないように確認します。 これが発生したバグを検出する方が効率的間接的にセカンダリ障害発生時点でバグの影響を確認します。

単体テストは、ソフトウェア開発ワークフローの重要な一部であるときにコードの品質に最大の効果がします。 メソッドが書き込まれるとすぐに、コードによる明示的または暗黙的な前提標準、境界、および正しくない入力データは、への応答内のメソッドとそのチェックの動作を確認する単体テストを記述する必要があります。 代わりに、テスト駆動開発には、単体テストは、コードの前に書き込まれます。 このシナリオでは、単体テストは、設計ドキュメントと機能仕様の両方として機能します。

> [!NOTE]
> 単体テストは、回帰 – 機能するために使用が障害のある更新プログラムによって乱されるされている機能は、に対して非常に効果的です。

単体テストは、通常 arrange、act アサート パターンを使用します。

-   *整列*単体テスト メソッドのセクションでは、オブジェクトを初期化し、テスト対象のメソッドに渡されるデータの値を設定します。
-   *動作*セクションが必須の引数でテスト対象のメソッドを呼び出します。
-   *アサート*セクションでは、テスト対象のメソッドの操作が期待どおりに動作することを検証します。

このパターンに従って単体テストは、読み取り可能で、一貫性のあることを確認します。

## <a name="dependency-injection-and-unit-testing"></a>依存関係挿入と単体テスト

疎結合アーキテクチャを採用する動機の 1 つは、単体テストが容易にします。 Autofac に登録されている型の 1 つは、`OrderService`クラスです。 次のコード例は、このクラスの概要を示しています。

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

`OrderDetailViewModel`クラスに依存している、`IOrderService`コンテナー解決がインスタンス化する型、`OrderDetailViewModel`オブジェクト。 ただし、作成する代わりに、`OrderService`単体テストをオブジェクト、`OrderDetailViewModel`クラスが代わりに、置換、`OrderService`をテストするためにモック オブジェクト。 図 10-1 では、この関係を示します。

![](unit-testing-images/unittesting.png "IOrderService インターフェイスを実装するクラス")

**図 10-1:** IOrderService インターフェイスを実装するクラス

このアプローチにより、`OrderService`に渡されるオブジェクト、`OrderDetailViewModel`クラス、実行時に、およびテストの容易性の利益のことができます、`OrderMockService`クラスに渡される、`OrderDetailViewModel`テスト時にクラスです。 このアプローチの主な利点は、web サービス、またはデータベースなどの扱いリソースを必要とせずに実行される単体テストを使用できることです。

## <a name="testing-mvvm-applications"></a>MVVM アプリケーションのテスト

モデルおよび MVVM アプリケーションからビューのモデルのテストは、他のクラスをテストして同じ同じツールと – 単体テストとモックなどの手法を使用できます。 ただしは一部のモデルに一般的なパターンとビューのモデル クラスを特定のユニット テスト手法を活用できます。

> [!TIP]
> 各単体テストを 1 つの点をテストします。 演習ユニットの動作の 1 つ以上の側面をテスト単位を作成しようと考える必要はありません。 これは読み取りし、更新するが困難なテストにつながります。 障害を解釈する場合に、混乱を招くこと可能性もします。

EShopOnContainers モバイル アプリ使用[xUnit](https://xunit.github.io/)を 2 つの異なる種類の単体テストをサポートしている単体テストを実行します。

-   ファクトは、常に true の場合、あるテスト インバリアント条件をテストします。
-   理論では、のみ特定のデータのセットの有効なテストが。

EShopOnContainers モバイル アプリに含まれている単体テストは、ファクトのテスト、およびで各単体テスト メソッドを装飾するため、`[Fact]`属性。

> [!NOTE]
> テスト ランナーによって、xUnit テストが実行されます。 テスト ランナーを実行するには、必要なプラットフォームの eShopOnContainers.TestRunner プロジェクトを実行します。

### <a name="testing-asynchronous-functionality"></a>非同期機能のテスト

MVVM パターンを実装するときにモデルの表示通常に対する操作を呼び出すサービス、多くの場合、非同期的にします。 通常これらの操作を呼び出すコードのテストは、代わりに、実際のサービスのモックを使用します。 次のコード例では、非同期機能をテストするには、ビュー モデルにモック サービスに渡すことを示しています。

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

この単体テストでは、ことを確認、`Order`のプロパティ、`OrderDetailViewModel`インスタンスが後の値になります、`InitializeAsync`メソッドが呼び出されています。 `InitializeAsync`ビュー モデルの対応するビューを移動するメソッドが呼び出されます。 ナビゲーションの詳細については、次を参照してください。[ナビゲーション](~/xamarin-forms/enterprise-application-patterns/navigation.md)です。

ときに、`OrderDetailViewModel`インスタンスが作成されることを想定する`OrderService`を引数として指定するインスタンス。 ただし、 `OrderService` web サービスからデータを取得します。 したがって、`OrderMockService`モック バージョンでは、インスタンスの`OrderService`クラスへの引数として指定された、`OrderDetailViewModel`コンス トラクターです。 ビュー モデルの`InitializeAsync`メソッドが呼び出され、これを呼び出す`IOrderService`操作、モック データは、web サービスとの通信中ではなく取得します。

### <a name="testing-inotifypropertychanged-implementations"></a>INotifyPropertyChanged 実装のテスト

実装する、`INotifyPropertyChanged`インターフェイスにより、ビューから発信された変更に反応するためのビュー モデルとモデル。 これらの変更がコントロールに表示されるデータに限定されない – を開始できるアニメーションまたは無効にするコントロールを発生させるビュー モデルの状態など、表示の制御に使用されることもできます。

イベント ハンドラーをアタッチすることにより、単体テストで直接更新できるプロパティをテストすることができます、`PropertyChanged`イベントおよびプロパティの新しい値を設定した後、イベントが発生するかどうかをチェックします。 次のコード例では、このようなテストを示します。

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

この単体テストを呼び出す、`InitializeAsync`のメソッド、`OrderViewModel`クラスであり、この結果、`Order`更新するプロパティです。 単体テストが合格で提供される、`PropertyChanged`のイベントは、`Order`プロパティです。

### <a name="testing-message-based-communication"></a>メッセージ ベースの通信のテスト

ビュー モデルを使用する、 [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/)疎結合のクラス間で単体テストできます次のコード例に示すように、テスト対象のコードによって送信されるメッセージをサブスクライブすることによって通信するためにクラス。

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

この単体テストでは、ことを確認、`CatalogViewModel`発行、`AddProduct`への応答メッセージの`AddCatalogItemCommand`実行されています。 [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/)クラスは、マルチキャスト メッセージのサブスクリプションをサポートしている、サブスクライブする単体テスト、`AddProduct`メッセージおよび受信への応答コールバック デリゲートを実行します。 ラムダ式として指定された、このコールバック デリゲートを設定、`boolean`によって使用されるフィールド、`Assert`ステートメントをテストの動作を確認します。

### <a name="testing-exception-handling"></a>テストの例外処理

単体テスト書き込むこともできます無効な操作や、入力の特定の例外がスローされることのチェックの次のコード例に示すようにします。

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

この単体テストのため、例外がスローされます、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)コントロールには、という名前のイベントはありません。`OnItemTapped`です。 `Assert.Throws<T>`メソッドがジェネリック メソッドで`T`予期される例外の種類です。 渡される引数、`Assert.Throws<T>`メソッドは例外をスローする、ラムダ式。 単体テストが成功する、ラムダ式をスローするため、`ArgumentException`です。

>💡 **ヒント:**: 例外のメッセージ文字列を確認する単体テストを記述しないようにします。 例外のメッセージ文字列を時間の経過と共に変更する可能性があり、自分の存在に依存する単体テストが不安定と見なされるようにします。

### <a name="testing-validation"></a>検証のテスト

検証の実装をテストする 2 つの側面があります: 規則が正しく実行されたことをテストおよびテストする、`ValidatableObject<T>`クラスが期待どおりに実行します。

検証ロジックは、出力が入力に依存する、自己完結型のプロセスでは通常ためにをテストするには通常単純です。 呼び出しの結果をテストにする必要がありますが、`Validate`メソッドを次のコード例に示すように、少なくとも 1 つの関連する検証規則を持つ各プロパティを。

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

この単体テストでは、ときに検証が成功したことを確認、2 つ`ValidatableObject<T>`プロパティで、`MockViewModel`両方のインスタンス データが存在します。

検証が成功したことを確認するには、および単体テストを検証する必要がありますもの値を確認、 `Value`、 `IsValid`、および`Errors`の各プロパティ`ValidatableObject<T>`クラスが期待どおりに実行することを確認するインスタンス。 次のコード例では、これを行う単体テストを示しています。

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

この単体テストでは、ときに検証が失敗したことを確認、`Surname`のプロパティ、`MockViewModel`は任意のデータがありません、 `Value`、 `IsValid`、および`Errors`の各プロパティ`ValidatableObject<T>`インスタンスが正しく設定されています。

## <a name="summary"></a>まとめ

単体テスト メソッドでは通常、アプリの小さい単位を取得するには、コードの残りの部分から分離および期待どおりに動作することを確認します。 その目的は、機能の各単位と期待どおりに、エラーは、アプリ全体に伝達されないように確認します。

依存オブジェクトの動作をシミュレートするモック オブジェクトと依存オブジェクトを置き換えることで、テスト対象のオブジェクトの動作を分離することができます。 これにより、web サービス、またはデータベースなどの扱いリソースを必要とせずに実行される単体テストできます。

モデルおよび MVVM アプリケーションからビューのモデルのテストは、他のクラスをテストして同じ、同じツールと手法を使用できます。


## <a name="related-links"></a>関連リンク

- [電子 (2 Mb PDF) のダウンロードします。](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (サンプル)](https://github.com/dotnet-architecture/eShopOnContainers)
