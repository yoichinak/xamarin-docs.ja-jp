---
title: モデルビュービューモデルパターン
description: この章では、eShopOnContainers モバイルアプリが MVVM パターンを使用して、ユーザーインターフェイスからアプリのビジネスロジックとプレゼンテーションロジックを明確に分離する方法について説明します。
ms.prod: xamarin
ms.assetid: dd8c1813-df44-4947-bcee-1a1ff2334b87
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: cc0528e7696130a772e93be67526ea9d6b10373f
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86936527"
---
# <a name="the-model-view-viewmodel-pattern"></a>モデルビュービューモデルパターン

通常、開発者の経験では、 Xamarin.Forms XAML でユーザーインターフェイスを作成し、ユーザーインターフェイスで動作する分離コードを追加する必要があります。 アプリが変更され、サイズと範囲が拡大するにつれて、複雑なメンテナンスの問題が発生する可能性があります。 これらの問題には、ui コントロールとビジネスロジックとの密結合が含まれます。これにより、UI の変更にかかるコストが増加し、このようなコードを単体テストすることが困難になります。

モデルビュービューモデル (MVVM) パターンは、アプリケーションのビジネスロジックとプレゼンテーションロジックをユーザーインターフェイス (UI) から明確に分離するのに役立ちます。 アプリケーションロジックと UI を明確に分離することによって、さまざまな開発上の問題に対処し、アプリケーションを簡単にテスト、保守、および進化させることができます。 また、コードの再利用の機会を大幅に向上させることができ、開発者や UI デザイナーはアプリの各部分を開発する際により簡単に共同作業を行うことができます。

## <a name="the-mvvm-pattern"></a>MVVM パターン

MVVM パターンには、モデル、ビュー、ビューモデルという3つの主要なコンポーネントがあります。 それぞれが個別の目的を果たします。 図2-1 は、3つのコンポーネント間の関係を示しています。

![MVVM パターン](mvvm-images/mvvm.png)

**図 2-1**: MVVM パターン

各コンポーネントの役割について理解することに加えて、相互にやり取りする方法を理解することも重要です。 大まかに言えば、ビューはビューモデルを "認識" し、ビューモデルではモデルを認識しますが、モデルはビューモデルを認識せず、ビューモデルはビューを認識しません。 したがって、ビューモデルはビューをモデルから分離し、ビューとは無関係にモデルを進化させることができます。

MVVM パターンを使用する利点は次のとおりです。

- 既存のビジネスロジックをカプセル化する既存のモデルの実装がある場合は、それを変更することは困難であるか、または危険である可能性があります。 このシナリオでは、ビューモデルはモデルクラスのアダプターとして機能し、モデルコードに大きな変更を加えないようにします。
- 開発者はビューを使用せずに、ビューモデルとモデルの単体テストを作成できます。 ビューモデルの単体テストでは、ビューで使用されるのとまったく同じ機能を使用できます。
- ビューが XAML で完全に実装されていれば、コードに触れることなくアプリの UI を再設計できます。 したがって、ビューの新しいバージョンは、既存のビューモデルで動作する必要があります。
- デザイナーと開発者は、開発プロセス中に、コンポーネントに対して個別に、または同時に作業を行うことができます。 デザイナーはビューに専念できますが、開発者はビューモデルとモデルコンポーネントを操作できます。

MVVM を効果的に使用するための鍵は、アプリケーションコードを適切なクラスに分類する方法と、クラスがどのように対話するかを理解することです。 以下のセクションでは、MVVM パターンの各クラスの役割について説明します。

### <a name="view"></a>表示

ビューでは、画面に表示される内容の構造、レイアウト、および外観を定義します。 各ビューは XAML で定義されており、ビジネスロジックを含まない分離された分離コードがあることが理想的です。 ただし、場合によっては、アニメーションなど、XAML で表現しにくい視覚的な動作を実装する UI ロジックが分離コードに含まれていることがあります。

アプリケーションでは、 Xamarin.Forms ビューは通常、 [`Page`](xref:Xamarin.Forms.Page) 派生クラスまたは [`ContentView`](xref:Xamarin.Forms.ContentView) 派生クラスです。 ただし、ビューはデータテンプレートで表すこともできます。これは、表示されたときにオブジェクトを視覚的に表現するために使用される UI 要素を指定します。 ビューとしてのデータテンプレートには分離コードがないため、特定のビューモデル型にバインドするように設計されています。

> [!TIP]
> 分離コードで UI 要素を有効または無効にすることは避けてください。 ビューモデルが、ビューの表示のいくつかの側面 (コマンドが使用可能かどうか、または操作が保留中であることを示すなど) に影響する論理的な状態の変更を定義する責任があることを確認します。 そのため、コードビハインドで有効または無効にするのではなく、ビューモデルのプロパティにバインドして、UI 要素を有効または無効にします。

ビューモデルでコードを実行するためのオプションはいくつかあります。これには、ボタンのクリックや項目の選択など、ビューの相互作用に応答します。 コントロールでコマンドがサポートされている場合、コントロールのプロパティは、 `Command` ビューモデルのプロパティにデータバインドでき `ICommand` ます。 コントロールのコマンドが呼び出されると、ビューモデルのコードが実行されます。 コマンドに加えて、動作はビュー内のオブジェクトにアタッチすることができ、コマンドを呼び出すか、イベントを発生させるかを待機できます。 これに対して、動作はビューモデル `ICommand` またはビューモデルのメソッドでを呼び出すことができます。

### <a name="viewmodel"></a>モデル

ビューモデルでは、ビューのデータバインド先となるプロパティとコマンドが実装され、変更通知イベントによって状態の変化がビューに通知されます。 ビューモデルに用意されているプロパティとコマンドは、UI によって提供される機能を定義しますが、ビューはその機能をどのように表示するかを決定します。

> [!TIP]
> 非同期操作で UI の応答性を維持します。 モバイルアプリでは、UI スレッドのブロックを解除して、ユーザーによるパフォーマンスの認識を向上させる必要があります。 したがって、ビューモデルでは、i/o 操作に非同期メソッドを使用し、イベントを発生させて、プロパティの変更をビューに非同期的に通知します。

ビューモデルは、必要なモデルクラスとビューの相互作用を調整する役割も担います。 ビューモデルとモデルクラスの間には、通常、一対多のリレーションシップがあります。 ビューモデルでは、モデルクラスをビューに直接公開して、ビュー内のコントロールが直接データをバインドできるようにすることができます。 この場合、モデルクラスは、データバインディングと変更通知イベントをサポートするように設計されている必要があります。

各ビューモデルは、ビューが簡単に使用できる形式のモデルのデータを提供します。 これを実現するために、ビューモデルはデータ変換を実行することがあります。 ビューをバインドできるプロパティを提供するため、このデータ変換をビューモデルに配置することをお勧めします。 たとえば、ビューモデルでは、2つのプロパティの値を組み合わせて、ビューでの表示を容易にすることができます。

> [!TIP]
> 変換レイヤーでのデータ変換を一元化します。 また、ビューモデルとビューの間にある独立したデータ変換層としてコンバーターを使用することもできます。 これは、たとえば、データがビューモデルで提供されていない特殊な書式設定を必要とする場合に必要になることがあります。

ビューモデルがビューを使用した双方向のデータバインディングに参加するためには、そのプロパティがイベントを発生させる必要があり `PropertyChanged` ます。 モデルを表示するには、 `INotifyPropertyChanged` インターフェイスを実装し、 `PropertyChanged` プロパティが変更されたときにイベントを発生させることによって、この要件を満たします。

コレクションの場合、ビューフレンドリ `ObservableCollection<T>` が提供されます。 このコレクションは、コレクションの変更通知を実装します。これにより、開発者は、インターフェイスをコレクションに実装する必要がなく `INotifyCollectionChanged` なります。

### <a name="model"></a>モデル

モデルクラスは、アプリのデータをカプセル化する非ビジュアルクラスです。 そのため、モデルはアプリのドメインモデルを表すものと考えることができます。通常、ビジネスおよび検証ロジックと共にデータモデルが含まれます。 モデルオブジェクトの例としては、データ転送オブジェクト (Dto)、Plain Old CLR Objects (POCOs)、生成されたエンティティとプロキシオブジェクトなどがあります。

通常、モデルクラスは、データアクセスとキャッシュをカプセル化するサービスまたはリポジトリと共に使用されます。

## <a name="connecting-view-models-to-views"></a>ビューモデルをビューに接続する

ビューモデルは、のデータバインディング機能を使用してビューに接続でき Xamarin.Forms ます。 ビューを構築してモデルを表示し、実行時に関連付けるために使用できる方法は多数あります。 これらの方法は2つのカテゴリに分類されます。これは、最初のコンポジションの表示と、ビューモデルの最初の構成と呼ばれます。 最初のコンポジションを表示するか、モデルを最初に表示するかを選択することは、優先度と複雑さの問題です。 ただし、すべての方法で同じ目的が共有されます。これは、ビューに BindingContext プロパティに割り当てられたビューモデルがあることを示します。

ビューの最初の構成では、アプリは概念的に、依存するビューモデルに接続するビューで構成されます。 この方法の主な利点は、ビューモデルがビュー自体に依存していないため、疎結合された単体テスト可能なアプリを簡単に作成できることです。 また、ビジュアル構造に従うことで、アプリの構造を簡単に理解することもできます。これは、コードの実行を追跡して、クラスがどのように作成され、関連付けられているかを理解する必要はありません。 また、ビューの最初の構築は、 Xamarin.Forms ナビゲーションが発生したときにページを構築するナビゲーションシステムと整合しています。これにより、ビューモデルが最初に複雑になり、プラットフォームに不整合が生じます。

ビューモデルの最初の構成では、アプリは概念的にビューモデルで構成され、ビューモデルのビューを検索するサービスがあります。 ビューモデルの最初の構成は、一部の開発者にとってより自然なものになります。これは、ビューの作成を抽象化して、アプリの論理的な非 UI 構造に集中できるようにするためです。 また、ビューモデルを他のビューモデルで作成することもできます。 ただし、この方法は複雑になることが多く、アプリのさまざまな部分がどのように作成され、関連付けられているかを理解するのが困難になることがあります。

> [!TIP]
> ビューモデルとビューを独立して保持します。 データソース内のプロパティへのビューのバインドは、対応するビューモデルに対するビューのプリンシパルの依存関係である必要があります。 具体的には、 [`Button`](xref:Xamarin.Forms.Button) ビューモデルからやなどのビューの種類を参照しないで [`ListView`](xref:Xamarin.Forms.ListView) ください。 ここで説明する原則に従うことで、ビューモデルを分離してテストできるため、スコープを制限することでソフトウェアの欠陥の可能性を低減できます。

以下のセクションでは、ビューモデルをビューに接続する主な方法について説明します。

### <a name="creating-a-view-model-declaratively"></a>ビューモデルの宣言による作成

最も簡単な方法は、ビューが XAML で対応するビューモデルを宣言によってインスタンス化することです。 ビューが構築されると、対応するビューモデルオブジェクトも構築されます。 この方法を次のコード例に示します。

```xaml
<ContentPage ... xmlns:local="clr-namespace:eShop">  
    <ContentPage.BindingContext>  
        <local:LoginViewModel />  
    </ContentPage.BindingContext>  
    ...  
</ContentPage>
```

が作成されると、 [`ContentPage`](xref:Xamarin.Forms.ContentPage) のインスタンス `LoginViewModel` が自動的に作成され、ビューのとして設定され [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) ます。

ビューによるビューモデルの宣言型の構築と割り当てには、単純であるという利点がありますが、ビューモデルに既定の (パラメーターのない) コンストラクターが必要であるという欠点があります。

### <a name="creating-a-view-model-programmatically"></a>プログラムによるビューモデルの作成

ビューでは、ビューモデルがプロパティに割り当てられるコードビハインドファイルにコードを含めることができ [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) ます。 これは、次のコード例に示すように、多くの場合、ビューのコンストラクターで実現されます。

```csharp
public LoginView()  
{  
    InitializeComponent();  
    BindingContext = new LoginViewModel(navigationService);  
}
```

ビューの分離コード内でのビューモデルのプログラムによる構築と割り当てには、単純な利点があります。 ただし、この方法の主な欠点は、ビューが必要な依存関係を持つビューモデルを提供する必要があることです。 依存関係挿入コンテナーを使用すると、ビューモデルとビューモデルの間の疎結合を維持するのに役立ちます。 詳細については、「[依存関係の挿入](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md)」を参照してください。

### <a name="creating-a-view-defined-as-a-data-template"></a>データテンプレートとして定義されたビューを作成する

ビューは、データテンプレートとして定義し、ビューモデルの種類に関連付けることができます。 データテンプレートは、リソースとして定義することも、ビューモデルを表示するコントロール内でインラインで定義することもできます。 コントロールの内容はビューモデルインスタンスであり、データテンプレートを使用して視覚的に表現します。 この手法は、最初にビューモデルがインスタンス化され、次にビューの作成が行われる状況の例です。

### <a name="automatically-creating-a-view-model-with-a-view-model-locator"></a>ビューモデルロケーターを使用したビューモデルの自動作成

ビューモデルロケーターは、ビューモデルのインスタンス化とビューへの関連付けを管理するカスタムクラスです。 EShopOnContainers モバイルアプリでは、 `ViewModelLocator` クラスに添付プロパティがあり `AutoWireViewModel` ます。これは、ビューモデルをビューに関連付けるために使用されます。 ビューの XAML では、次のコード例に示すように、この添付プロパティが true に設定されて、ビューモデルがビューに自動的に接続されることを示しています。

```xaml
viewModelBase:ViewModelLocator.AutoWireViewModel="true"
```

`AutoWireViewModel`プロパティは、false に初期化されるバインド可能なプロパティであり、その値が変更されると、 `OnAutoWireViewModelChanged` イベントハンドラーが呼び出されます。 このメソッドは、ビューのビューモデルを解決します。 これを実現する方法を次のコード例に示します。

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

メソッドは、 `OnAutoWireViewModelChanged` 規則ベースの方法を使用して、ビューモデルの解決を試みます。 この規則は、次のことを前提としています。

- ビューモデルは、ビューの種類と同じアセンブリにあります。
- ビューはにあります。子の名前空間を表示します。
- ビューモデルはに含まれています。Viewmodel child 名前空間。
- ビューモデル名は、ビュー名に対応し、"モデルビュー" で終了します。

最後に、 `OnAutoWireViewModelChanged` メソッドは、 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) ビュー型のを、解決されたビューモデル型に設定します。 ビューモデルの種類の解決の詳細については、「[解決策](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#resolution)」を参照してください。

このアプローチには、ビューモデルのインスタンス化とビューへの接続を行う単一のクラスがアプリにあるという利点があります。

> [!TIP]
> 代替を簡単にするためにビューモデルロケーターを使用します。 ビューモデルロケーターは、単体テストやデザイン時データなどの依存関係の代替実装のために、代替のポイントとして使用することもできます。

## <a name="updating-views-in-response-to-changes-in-the-underlying-view-model-or-model"></a>基になるビューモデルまたはモデルの変更に応じてビューを更新する

ビューにアクセスできるすべてのビューモデルおよびモデルクラスは、インターフェイスを実装する必要があり `INotifyPropertyChanged` ます。 ビューモデルまたはモデルクラスにこのインターフェイスを実装すると、基になるプロパティ値が変更されたときに、クラスはビュー内のデータバインドコントロールに変更通知を提供できます。

次の要件を満たすことで、プロパティ変更通知を適切に使用するために、アプリを設計する必要があります。

- `PropertyChanged`パブリックプロパティの値が変更された場合は、常にイベントを発生させます。 `PropertyChanged`XAML バインディングがどのように行われるかについての知識があるため、イベントの発生を無視できるとは限りません。
- `PropertyChanged`ビューモデルまたはモデル内の他のプロパティで値が使用されているすべての計算プロパティについて、常にイベントを発生させます。
- `PropertyChanged`プロパティを変更するメソッドの最後、またはオブジェクトが安全な状態であることがわかっている場合は、常にイベントを発生させます。 イベントを発生させると、イベントのハンドラーが同期的に呼び出され、操作が中断されます。 これが操作の途中で発生する場合、アンセーフで部分的に更新された状態にあるときに、オブジェクトがコールバック関数に公開されることがあります。 また、イベントによって連鎖変更がトリガーされる可能性があり `PropertyChanged` ます。 カスケード変更を行うには、通常、連鎖変更が安全に実行されるためには更新が完了している必要があります。
- プロパティが変更されない場合、イベントは発生しません `PropertyChanged` 。 これは、イベントを発生させる前に、古い値と新しい値を比較する必要があることを意味し `PropertyChanged` ます。
- `PropertyChanged`プロパティを初期化する場合は、ビューモデルのコンストラクター中にイベントを発生させないでください。 ビュー内のデータバインドコントロールは、この時点で変更通知の受信をサブスクライブしていません。
- `PropertyChanged`クラスのパブリックメソッドの1回の同期呼び出しで、同じプロパティ名引数を持つ複数のイベントを発生させないでください。 たとえば、バッキングストアがフィールドであるプロパティを指定した `NumberOfItems` `_numberOfItems` 場合、ループの実行中にメソッドが50回インクリメントされた場合、 `_numberOfItems` `NumberOfItems` すべての作業が完了した後で、プロパティのプロパティ変更通知だけが発生します。 非同期メソッドの場合は、 `PropertyChanged` 非同期の継続チェーンの各同期セグメントで、指定されたプロパティ名のイベントを発生させます。

EShopOnContainers モバイルアプリでは、クラスを使用して `ExtendedBindableObject` 変更通知を提供します。次のコード例を参照してください。

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

Xamarin. Form [`BindableObject`](xref:Xamarin.Forms.BindableObject) クラスは `INotifyPropertyChanged` インターフェイスを実装し、メソッドを提供します [`OnPropertyChanged`](xref:Xamarin.Forms.BindableObject.OnPropertyChanged(System.String)) 。 クラスは、 `ExtendedBindableObject` `RaisePropertyChanged` プロパティ変更通知を呼び出すためのメソッドを提供します。また、では、クラスによって提供される機能を使用し `BindableObject` ます。

EShopOnContainers モバイルアプリの各ビューモデルクラスは、クラスから派生します。このクラスは、クラス `ViewModelBase` から派生 `ExtendedBindableObject` します。 したがって、各ビューモデルクラスは、クラスのメソッドを使用して `RaisePropertyChanged` `ExtendedBindableObject` プロパティ変更通知を提供します。 次のコード例は、ラムダ式を使用して、eShopOnContainers モバイルアプリがプロパティ変更通知を呼び出す方法を示しています。

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

この方法でラムダ式を使用すると、各呼び出しに対してラムダ式を評価する必要があるため、パフォーマンスがわずかに低下します。 パフォーマンスコストは小さく、通常はアプリには影響しませんが、多くの変更通知がある場合はコストが発生する可能性があります。 ただし、この方法の利点は、プロパティの名前を変更するときに、コンパイル時のタイプセーフとリファクタリングサポートが提供されることです。

## <a name="ui-interaction-using-commands-and-behaviors"></a>コマンドと動作を使用した UI の操作

Mobile apps では、通常、アクションは、ボタンクリックなどのユーザー操作に応答して呼び出されます。これは、分離コードファイルにイベントハンドラーを作成することによって実装できます。 ただし、MVVM パターンでは、アクションを実装する責任はビューモデルにあり、分離コードにコードを配置することは避けてください。

コマンドは、UI 内のコントロールにバインドできるアクションを表す便利な方法を提供します。 アクションを実装するコードをカプセル化し、ビューのビジュアル表現から分離された状態を維持するために役立ちます。 Xamarin.Formsには、コマンドに宣言によって接続できるコントロールが含まれています。これらのコントロールは、ユーザーがコントロールと対話するときにコマンドを呼び出します。

ビヘイビアーを使用すると、コントロールを宣言によってコマンドに接続することもできます。 ただし、ビヘイビアーを使用して、コントロールによって生成されるイベントの範囲に関連付けられたアクションを呼び出すことができます。 そのため、動作は、コマンド対応コントロールと同じシナリオの多くに対応しながら、柔軟性と制御性を高めることができます。 また、ビヘイビアーを使用して、コマンドを操作するために特に設計されていないコントロールにコマンドオブジェクトやメソッドを関連付けることもできます。

### <a name="implementing-commands"></a>コマンドの実装

ビューモデルは、通常、ビューからバインドするためのコマンドプロパティを公開します。これは、インターフェイスを実装するオブジェクトインスタンスです `ICommand` 。 多くのコントロールには、 Xamarin.Forms プロパティが用意され `Command` ています。これは、 `ICommand` ビューモデルによって提供されるオブジェクトにデータをバインドすることができます。 インターフェイスは、 `ICommand` `Execute` 操作自体をカプセル化するメソッド、コマンドを `CanExecute` 呼び出すことができるかどうかを示すメソッド、および `CanExecuteChanged` コマンドを実行する必要があるかどうかに影響を与える変更が発生したときに発生するイベントを定義します。 [`Command`](xref:Xamarin.Forms.Command) [`Command<T>`](xref:Xamarin.Forms.Command) によって提供されるクラスとクラスは、 Xamarin.Forms インターフェイスを実装し `ICommand` ます。ここで、 `T` は、およびの引数の型です `Execute` `CanExecute` 。

ビューモデル内では、型の [`Command`](xref:Xamarin.Forms.Command) [`Command<T>`](xref:Xamarin.Forms.Command) ビューモデルの各パブリックプロパティに対して、型または型のオブジェクトが存在する必要があり `ICommand` ます。 `Command`またはコンストラクターには、 `Command<T>` `Action` メソッドが呼び出されたときに呼び出されるコールバックオブジェクトが必要です `ICommand.Execute` 。 `CanExecute`メソッドは省略可能なコンストラクターパラメーターであり、はを `Func` 返すです `bool` 。

次のコードは、 [`Command`](xref:Xamarin.Forms.Command) レジスタコマンドを表すインスタンスを、ビューモデルメソッドへのデリゲートを指定することによって構築する方法を示してい `Register` ます。

```csharp
public ICommand RegisterCommand => new Command(Register);
```

コマンドは、への参照を返すプロパティを使用してビューに公開され `ICommand` ます。 `Execute`オブジェクトでメソッドが呼び出されると [`Command`](xref:Xamarin.Forms.Command) 、そのメソッドは、コンストラクターで指定されたデリゲートを介して、ビューモデルのメソッドへの呼び出しを単純に転送し `Command` ます。

コマンド `async` のデリゲートを指定するときに、キーワードとキーワードを使用して、コマンドによって非同期メソッドを呼び出すことができ `await` `Execute` ます。 これは、コールバックがであり、待機する必要があることを示し `Task` ます。 たとえば、次のコードは、 [`Command`](xref:Xamarin.Forms.Command) ビューモデルメソッドへのデリゲートを指定することによって、サインインコマンドを表すインスタンスを作成する方法を示してい `SignInAsync` ます。

```csharp
public ICommand SignInCommand => new Command(async () => await SignInAsync());
```

パラメーターは、クラスを使用して `Execute` `CanExecute` コマンドをインスタンス化することにより、アクションとアクションに渡すことができ [`Command<T>`](xref:Xamarin.Forms.Command) ます。 たとえば、次のコードは、メソッドが `Command<T>` 型の引数を必要とすることを示すために、インスタンスを使用する方法を示してい `NavigateAsync` `string` ます。

```csharp
public ICommand NavigateCommand => new Command<string>(NavigateAsync);
```

[`Command`](xref:Xamarin.Forms.Command)クラスとクラスの両方で [`Command<T>`](xref:Xamarin.Forms.Command) 、 `CanExecute` 各コンストラクターのメソッドへのデリゲートは省略可能です。 デリゲートが指定されていない場合、は `Command` `true` を返し `CanExecute` ます。 ただし、ビューモデルでは、 `CanExecute` オブジェクトに対してメソッドを呼び出すことによって、コマンドの状態の変更を示すことができ `ChangeCanExecute` `Command` ます。 これにより、 `CanExecuteChanged` イベントが発生します。 コマンドにバインドされている UI 内のコントロールは、データバインドコマンドの可用性を反映するように、有効な状態を更新します。

#### <a name="invoking-commands-from-a-view"></a>ビューからのコマンドの呼び出し

次のコード例では、インスタンスを使用して、内のをクラスのにバインドする方法を示し [`Grid`](xref:Xamarin.Forms.Grid) `LoginView` `RegisterCommand` `LoginViewModel` [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) ます。

```xaml
<Grid Grid.Column="1" HorizontalOptions="Center">  
    <Label Text="REGISTER" TextColor="Gray"/>  
    <Grid.GestureRecognizers>  
        <TapGestureRecognizer Command="{Binding RegisterCommand}" NumberOfTapsRequired="1" />  
    </Grid.GestureRecognizers>  
</Grid>
```

コマンドパラメーターは、必要に応じてプロパティを使用して定義することもでき [`CommandParameter`](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter) ます。 予期される引数の型は、およびのターゲットメソッドで指定され `Execute` `CanExecute` ます。 [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer)ユーザーが添付コントロールを操作すると、はターゲットコマンドを自動的に呼び出します。 コマンドパラメーターが指定されている場合は、コマンドのデリゲートに引数として渡され `Execute` ます。

### <a name="implementing-behaviors"></a>実装 (動作を)

ビヘイビアーを使用すると、機能を UI コントロールに追加できるようになります。 代わりに、その機能はビヘイビアー クラスで実装され、それがコントロール自体の一部であるかのようにコントロールにアタッチされます。 動作を使用すると、通常は分離コードとして記述する必要のあるコードを実装できます。これは、コントロールに簡潔にアタッチし、複数のビューやアプリで再利用するためにパッケージ化できるように、コントロールの API と直接対話するためです。 MVVM のコンテキストでは、動作は、コントロールをコマンドに接続するための便利な方法です。

添付プロパティを介してコントロールにアタッチされる動作は、アタッチされる*動作*と呼ばれます。 その後、この動作によって、関連付けられている要素の公開された API を使用して、ビューのビジュアルツリー内のコントロールまたはその他のコントロールに機能を追加できます。 EShopOnContainers モバイルアプリには、アタッチされる動作であるクラスが含まれてい `LineColorBehavior` ます。 この動作の詳細については、「[検証エラーの表示](~/xamarin-forms/enterprise-application-patterns/validation.md#displaying-validation-errors)」を参照してください。

Xamarin.Forms動作は、クラスまたはクラスから派生するクラスです [`Behavior`](xref:Xamarin.Forms.Behavior) 。ここで、 [`Behavior<T>`](xref:Xamarin.Forms.Behavior`1) `T` は動作を適用するコントロールの型です。 これらのクラスにはメソッドとメソッドが用意されており、これをオーバーライドして、 `OnAttachedTo` `OnDetachingFrom` 動作がコントロールにアタッチされ、コントロールからデタッチされるときに実行されるロジックを提供する必要があります。

EShopOnContainers モバイルアプリでは、クラスは `BindableBehavior<T>` クラスから派生し [`Behavior<T>`](xref:Xamarin.Forms.Behavior`1) ます。 クラスの目的 `BindableBehavior<T>` は、動作の基底クラスを提供することです。これにより、ビヘイビアー Xamarin.Forms [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) のを添付コントロールに設定する必要があります。

クラスには、 `BindableBehavior<T>` `OnAttachedTo` 動作のを設定するオーバーライド可能なメソッドと、をクリーンアップするオーバーライド可能なメソッドが用意されて [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) `OnDetachingFrom` `BindingContext` います。 さらに、このクラスでは、アタッチされたコントロールへの参照が `AssociatedObject` プロパティに格納されます。

EShopOnContainers モバイルアプリには、 `EventToCommandBehavior` イベントの発生に応じてコマンドを実行するクラスが含まれています。 このクラスは、動作 `BindableBehavior<T>` が使用されるときに、 `ICommand` プロパティによって指定されたをバインドして実行できるように、クラスから派生し `Command` ます。 次に示すのは、`EventToCommandBehavior` クラスのコード例です。

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

`OnAttachedTo`メソッドと `OnDetachingFrom` メソッドは、プロパティで定義されているイベントのイベントハンドラーを登録および登録解除するために使用され `EventName` ます。 次に、イベントが発生すると、 `OnFired` メソッドが呼び出され、コマンドが実行されます。

イベントが発生したときにを使用してコマンドを実行すると、コマンド `EventToCommandBehavior` と対話するように設計されていないコントロールにコマンドを関連付けることができるという利点があります。 さらに、イベント処理コードを移動して、モデルを単体テストできるようにします。

#### <a name="invoking-behaviors-from-a-view"></a>ビューからの動作の呼び出し

は、コマンド `EventToCommandBehavior` をサポートしないコントロールにコマンドをアタッチする場合に特に便利です。 たとえば、では、 `ProfileView` `EventToCommandBehavior` 次の `OrderDetailCommand` [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) [`ListView`](xref:Xamarin.Forms.ListView) コードに示すように、ユーザーの注文を一覧表示するでイベントが発生したときに、を使用してを実行します。

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

実行時には、 `EventToCommandBehavior` との対話に応答し [`ListView`](xref:Xamarin.Forms.ListView) ます。 で項目が選択されると、 `ListView` [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) イベントが発生します。これにより、でが実行され `OrderDetailCommand` `ProfileViewModel` ます。 既定では、イベントのイベント引数がコマンドに渡されます。 このデータは、プロパティで指定されたコンバーターによってソースとターゲットの間で渡されるときに変換されます。これにより、 `EventArgsConverter` からのが返され [`Item`](xref:Xamarin.Forms.ItemTappedEventArgs.Item) `ListView` [`ItemTappedEventArgs`](xref:Xamarin.Forms.ItemTappedEventArgs) ます。 そのため、を実行すると、選択したが、登録された `OrderDetailCommand` `Order` アクションにパラメーターとして渡されます。

動作の詳細については、「[動作](~/xamarin-forms/app-fundamentals/behaviors/index.md)」を参照してください。

## <a name="summary"></a>要約

モデルビュービューモデル (MVVM) パターンは、アプリケーションのビジネスロジックとプレゼンテーションロジックをユーザーインターフェイス (UI) から明確に分離するのに役立ちます。 アプリケーションロジックと UI を明確に分離することによって、さまざまな開発上の問題に対処し、アプリケーションを簡単にテスト、保守、および進化させることができます。 また、コードの再利用の機会を大幅に向上させることができ、開発者や UI デザイナーはアプリの各部分を開発する際により簡単に共同作業を行うことができます。

MVVM パターンを使用すると、アプリの UI と基になるプレゼンテーションとビジネスロジックが、UI と UI ロジックをカプセル化したビューという3つの異なるクラスに分割されます。ビューモデル。プレゼンテーションロジックと状態をカプセル化します。およびモデル。アプリのビジネスロジックとデータをカプセル化します。

## <a name="related-links"></a>関連リンク

- [電子ブックのダウンロード (2 Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (サンプル)](https://github.com/dotnet-architecture/eShopOnContainers)
