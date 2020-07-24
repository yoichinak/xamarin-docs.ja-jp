---
title: 構成管理
description: この章では、eShopOnContainers モバイルアプリが構成管理を実装してアプリ設定とユーザー設定を提供する方法について説明します。
ms.prod: xamarin
ms.assetid: 50d6e780-e768-47f8-9361-3af11e56b87b
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 6def31ebfe2a619f1d5db1e0fba071494afbcd3e
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86931964"
---
# <a name="configuration-management"></a>構成管理

設定を使用すると、アプリの動作を構成するデータをコードから分離できるため、アプリを再構築しなくても動作を変更できます。 設定には、アプリ設定とユーザー設定の2種類があります。

アプリ設定は、アプリによって作成および管理されるデータです。 これには、固定 web サービスエンドポイント、API キー、ランタイム状態などのデータを含めることができます。 アプリの設定はアプリの存在に関連付けられており、そのアプリに対してのみ意味があります。

ユーザー設定は、アプリの動作に影響するアプリのカスタマイズ可能な設定であり、頻繁に再調整する必要はありません。 たとえば、アプリでは、ユーザーがデータの取得元を指定したり、画面に表示したりすることができます。

Xamarin.Formsには、設定データを格納するために使用できる永続的なディクショナリが含まれています。 このディクショナリには、プロパティを使用してアクセスできます。このディクショナリに格納されて [`Application.Current.Properties`](xref:Xamarin.Forms.Application.Properties) いるデータは、アプリがスリープ状態になったときに保存され、アプリの再開時または再起動時に復元されます。 また、クラスに [`Application`](xref:Xamarin.Forms.Application) は、 [`SavePropertiesAsync`](xref:Xamarin.Forms.Application.SavePropertiesAsync) 必要に応じてアプリの設定を保存できるようにするメソッドもあります。 このディクショナリの詳細については、「 [Properties dictionary](~/xamarin-forms/app-fundamentals/application-class.md#properties-dictionary)」を参照してください。

永続ディクショナリを使用してデータを格納する Xamarin.Forms ことの欠点は、に簡単にデータをバインドできないことです。 そのため、eShopOnContainers モバイルアプリでは、 [NuGet](https://www.nuget.org/packages/Xam.Plugins.Settings/)から入手できる Xam. プラグインライブラリを使用します。 このライブラリは、アプリとユーザー設定の永続化と取得を行うための一貫したタイプセーフなクロスプラットフォームアプローチを提供します。また、各プラットフォームによって提供されるネイティブ設定管理を使用します。 また、データバインディングを使用して、ライブラリによって公開される設定データにアクセスするのも簡単です。

> [!NOTE]
> 各設定ライブラリには、アプリとユーザーの両方の設定を保存できますが、2つの設定は区別されません。

## <a name="creating-a-settings-class"></a>設定クラスの作成

設定ライブラリを使用する場合は、アプリに必要なアプリとユーザー設定を含む単一の静的クラスを作成する必要があります。 次のコード例は、eShopOnContainers モバイルアプリの Settings クラスを示しています。

```csharp
public static class Settings  
{  
    private static ISettings AppSettings  
    {  
        get  
        {  
            return CrossSettings.Current;  
        }  
    }  
    ...  
}
```

設定は、この API を使用して読み取りおよび書き込みを行うことができます。 `ISettings` これは、設定ライブラリによって提供されます。 このライブラリには、API へのアクセスに使用できるシングルトンが用意され `CrossSettings.Current` ています。また、アプリの設定クラスは、プロパティを介してこのシングルトンを公開する必要があり `ISettings` ます。

> [!NOTE]
> プラグインにディレクティブを使用してください。設定とプラグインの名前空間は、Xam. プラグインライブラリの種類へのアクセスを必要とするクラスに追加する必要があります。

## <a name="adding-a-setting"></a>設定の追加

各設定は、キー、既定値、およびプロパティで構成されます。 次のコード例では、eShopOnContainers モバイルアプリが接続するオンラインサービスのベース URL を表す、ユーザー設定の3つの項目すべてを示しています。

```csharp
public static class Settings  
{  
    ...  
    private const string IdUrlBase = "url_base";  
    private static readonly string UrlBaseDefault = GlobalSetting.Instance.BaseEndpoint;  
    ...  

    public static string UrlBase  
    {  
        get  
        {  
            return AppSettings.GetValueOrDefault<string>(IdUrlBase, UrlBaseDefault);  
        }  
        set  
        {  
            AppSettings.AddOrUpdateValue<string>(IdUrlBase, value);  
        }  
    }  
}
```

キーは常に、キー名を定義する const 文字列です。設定の既定値は、必要な型の静的な読み取り専用の値です。 既定値を指定すると、設定が解除された場合に有効な値が使用できるようになります。

`UrlBase`静的プロパティは、API の2つのメソッドを使用して、 `ISettings` 設定値の読み取りまたは書き込みを行います。 `ISettings.GetValueOrDefault`メソッドは、プラットフォーム固有のストレージから設定の値を取得するために使用されます。 設定に値が定義されていない場合は、代わりにその既定値が取得されます。 同様に、 `ISettings.AddOrUpdateValue` メソッドは、設定の値をプラットフォーム固有のストレージに永続化するために使用されます。

クラス内の既定値を定義するのではなく、 `Settings` `UrlBaseDefault` 文字列はクラスから値を取得し `GlobalSetting` ます。 次のコード例は、 `BaseEndpoint` このクラスのプロパティとメソッドを示してい `UpdateEndpoint` ます。

```csharp
public class GlobalSetting  
{  
    ...  
    public string BaseEndpoint  
    {  
        get { return _baseEndpoint; }  
        set  
        {  
            _baseEndpoint = value;  
            UpdateEndpoint(_baseEndpoint);  
        }  
    }  
    ...  

    private void UpdateEndpoint(string baseEndpoint)  
    {  
        RegisterWebsite = string.Format("{0}:5105/Account/Register", baseEndpoint);  
        CatalogEndpoint = string.Format("{0}:5101", baseEndpoint);  
        OrdersEndpoint = string.Format("{0}:5102", baseEndpoint);  
        BasketEndpoint = string.Format("{0}:5103", baseEndpoint);  
        IdentityEndpoint = string.Format("{0}:5105/connect/authorize", baseEndpoint);  
        UserInfoEndpoint = string.Format("{0}:5105/connect/userinfo", baseEndpoint);  
        TokenEndpoint = string.Format("{0}:5105/connect/token", baseEndpoint);  
        LogoutEndpoint = string.Format("{0}:5105/connect/endsession", baseEndpoint);  
        IdentityCallback = string.Format("{0}:5105/xamarincallback", baseEndpoint);  
        LogoutCallback = string.Format("{0}:5105/Account/Redirecting", baseEndpoint);  
    }  
}
```

`BaseEndpoint`プロパティが設定されるたびに、 `UpdateEndpoint` メソッドが呼び出されます。 このメソッドは、一連のプロパティを更新します。これらはすべて、 `UrlBase` `Settings` eShopOnContainers モバイルアプリが接続しているさまざまなエンドポイントを表すクラスによって提供されるユーザー設定に基づいています。

## <a name="data-binding-to-user-settings"></a>ユーザー設定へのデータバインド

EShopOnContainers モバイルアプリでは、は `SettingsView` 2 つのユーザー設定を公開しています。 これらの設定により、アプリが Docker コンテナーとしてデプロイされているマイクロサービスからデータを取得する必要があるかどうか、またはアプリがインターネット接続を必要としないモックサービスからデータを取得する必要があるかどうかを構成できます。 コンテナー化されたマイクロサービスからデータを取得することを選択する場合は、マイクロサービスのベースエンドポイント URL を指定する必要があります。 図7-1 は、コンテナー化された `SettingsView` マイクロサービスからデータを取得することをユーザーが選択した場合を示しています。

![EShopOnContainers モバイルアプリによって公開されるユーザー設定](configuration-management-images/settings-endpoint.png)

**図 7-1**: eShopOnContainers モバイルアプリによって公開されるユーザー設定

データバインディングを使用して、クラスによって公開される設定を取得および設定でき `Settings` ます。 これを実現するには、ビューバインディングのコントロールを使用して、クラスのプロパティにアクセスするモデルプロパティを表示 `Settings` し、設定値が変更された場合にプロパティの変更通知を発生させます。 EShopOnContainers mobile アプリがビューモデルを構築してビューに関連付ける方法については、「[ビューモデルロケーターを使用したビューモデルの自動作成](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically-creating-a-view-model-with-a-view-model-locator)」を参照してください。

次のコード例は [`Entry`](xref:Xamarin.Forms.Entry) 、コンテナー化さ `SettingsView` れたマイクロサービスのベースエンドポイント URL をユーザーが入力できるようにする、のコントロールを示しています。

```xaml
<Entry Text="{Binding Endpoint, Mode=TwoWay}" />
```

この [`Entry`](xref:Xamarin.Forms.Entry) コントロールは、 `Endpoint` 双方向のバインディングを使用して、クラスのプロパティにバインドさ `SettingsViewModel` れます。 次のコード例は、エンドポイントプロパティを示しています。

```csharp
public string Endpoint  
{  
    get { return _endpoint; }  
    set  
    {  
        _endpoint = value;  

        if(!string.IsNullOrEmpty(_endpoint))  
        {  
            UpdateEndpoint(_endpoint);  
        }  

        RaisePropertyChanged(() => Endpoint);  
    }  
}
```

プロパティが設定されている場合、 `Endpoint` `UpdateEndpoint` 指定された値が有効であると、メソッドが呼び出され、プロパティの変更通知が発生します。 次のコード例は、`UpdateEndpoint` メソッドを示しています。

```csharp
private void UpdateEndpoint(string endpoint)  
{  
    Settings.UrlBase = endpoint;  
}
```

このメソッドは、 `UrlBase` `Settings` ユーザーによって入力された基本エンドポイント URL 値を使用して、クラスのプロパティを更新します。これにより、プラットフォーム固有のストレージに永続化されます。

`SettingsView`に移動すると、クラスの `InitializeAsync` メソッド `SettingsViewModel` が実行されます。 以下のコード例はこのメソッドを示しています。

```csharp
public override Task InitializeAsync(object navigationData)  
{  
    ...  
    Endpoint = Settings.UrlBase;  
    ...  
}
```

メソッドは、 `Endpoint` プロパティをクラスのプロパティの値に設定し `UrlBase` `Settings` ます。 プロパティにアクセス `UrlBase` すると、プラットフォーム固有のストレージから設定値が取得されます。 メソッドの呼び出し方法の詳細については `InitializeAsync` 、「[ナビゲーション中のパラメーターの引き渡し](~/xamarin-forms/enterprise-application-patterns/navigation.md#passing-parameters-during-navigation)」を参照してください。

このメカニズムにより、ユーザーが SettingsView に移動するたびに、ユーザー設定がプラットフォーム固有のストレージから取得され、データバインディングによって表示されるようになります。 その後、ユーザーが設定値を変更すると、データバインディングによって、それらの値が直ちにプラットフォーム固有のストレージに永続化されます。

## <a name="summary"></a>まとめ

設定を使用すると、アプリの動作を構成するデータをコードから分離できるため、アプリを再構築しなくても動作を変更できます。 アプリ設定はアプリによって作成および管理されるデータです。ユーザー設定は、アプリの動作に影響するアプリのカスタマイズ可能な設定であり、頻繁に再調整する必要はありません。

アプリケーションとユーザー設定を永続化および取得するための一貫性のあるタイプセーフなクロスプラットフォームアプローチを提供します。また、データバインディングを使用して、ライブラリで作成された設定にアクセスできます。

## <a name="related-links"></a>関連リンク

- [電子ブックのダウンロード (2 Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (サンプル)](https://github.com/dotnet-architecture/eShopOnContainers)
