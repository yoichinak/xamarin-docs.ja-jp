---
title: 構成管理
description: この章では、eShopOnContainers のモバイル アプリでアプリ設定とユーザー設定を提供する構成管理の実装方法について説明します。
ms.prod: xamarin
ms.assetid: 50d6e780-e768-47f8-9361-3af11e56b87b
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 6f32d8f328232bdfc644da57bdb3201c60010063
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995361"
---
# <a name="configuration-management"></a>構成管理

設定は、コードから、アプリの動作を構成するデータの分離を許可する、アプリを再構築せずに変更する動作。 設定の 2 種類があります。 アプリの設定、およびユーザー設定。

アプリの設定は、アプリを作成および管理するデータです。 固定 web サービス エンドポイント、API キー、およびランタイムの状態などのデータに含めることができます。 アプリの設定は、アプリケーションの存在に関連付けられているされ、そのアプリにとって意味のあるのみです。

ユーザー設定は、アプリの動作に影響を頻繁に再調整を必要としないアプリのカスタマイズ可能な設定です。 たとえば、アプリ ユーザーが、データの取得方法と、画面上に表示する方法を指定することができます。

Xamarin.Forms には、設定データの格納に使用できる永続的なディクショナリが含まれています。 このディクショナリを使用してアクセスできる、 [ `Application.Current.Properties` ](xref:Xamarin.Forms.Application.Properties)プロパティ、およびそこに配置されているすべてのデータには、アプリが、スリープ状態になるし、アプリが再び起動または再開時に復元するときに保存されます。 さらに、 [ `Application` ](xref:Xamarin.Forms.Application)クラスがあります、 [ `SavePropertiesAsync` ](xref:Xamarin.Forms.Application.SavePropertiesAsync)アプリの設定の保存に必要な場合に許可するメソッド。 このディクショナリの詳細については、次を参照してください。 [Properties ディクショナリ](~/xamarin-forms/app-fundamentals/application-class.md#Properties_Dictionary)します。

Xamarin.Forms の永続的なディクショナリを使用してデータを格納する欠点は簡単にバインドされたデータです。 EShopOnContainers のモバイル アプリがから利用可能な Xam.Plugins.Settings ライブラリを使用するため、 [NuGet](https://www.nuget.org/packages/Xam.Plugins.Settings/)します。 このライブラリは、永続化すると、各プラットフォームで提供されるネイティブの設定の管理を使用しているときにアプリとユーザーの設定を取得する一貫した、タイプ セーフ、クロス プラットフォームのアプローチを提供します。 さらに、これは、データ バインディングを使用して、ライブラリによって公開される設定データにアクセスする簡単です。

> [!NOTE]
> Xam.Plugin.Settings ライブラリは、アプリとユーザーの両方の設定を保存できるが 2 つの区別、ありませんなります。

## <a name="creating-a-settings-class"></a>設定クラスを作成します。

Xam.Plugins.Settings ライブラリを使用する場合、1 つの静的クラスが作成をアプリに必要なアプリとユーザー設定が格納されます。 次のコード例では、eShopOnContainers のモバイル アプリで設定クラスを示しています。

```csharp
public static class Settings  
{  
    private static ISettings AppSettings  
    {  
        get  
        {  
            return CrossSettings.Current;  
        }  
    }  
    ...  
}
```

設定を読み取り、書き込むことができます、 `ISettings` Xam.Plugins.Settings ライブラリによって提供される API。 このライブラリは、API にアクセスするために使用するシングルトン`CrossSettings.Current`、およびアプリの設定クラスを使用してこのシングルトンを公開する必要があります、`ISettings`プロパティ。

> [!NOTE]
> Plugin.Settings と Plugin.Settings.Abstractions 名前空間の using ディレクティブは、Xam.Plugins.Settings ライブラリの型へのアクセスを必要とするクラスに追加する必要があります。

## <a name="adding-a-setting"></a>設定を追加

各設定は、キー、既定値、およびプロパティで構成されます。 次のコード例では、eShopOnContainers のモバイル アプリに接続するオンライン サービスのベース URL を表すユーザー設定の 3 つすべての項目を示します。

```csharp
public static class Settings  
{  
    ...  
    private const string IdUrlBase = "url_base";  
    private static readonly string UrlBaseDefault = GlobalSetting.Instance.BaseEndpoint;  
    ...  

    public static string UrlBase  
    {  
        get  
        {  
            return AppSettings.GetValueOrDefault<string>(IdUrlBase, UrlBaseDefault);  
        }  
        set  
        {  
            AppSettings.AddOrUpdateValue<string>(IdUrlBase, value);  
        }  
    }  
}
```

キーは常に、const 文字列キーの名前を定義する、設定の既定値は、必要な型の静的読み取り専用値です。 既定値を提供することにより、有効な値が未設定の設定が取得された場合に使用できます。

`UrlBase`静的プロパティから 2 つのメソッドを使用して、`ISettings`設定値を読み書きする API。 `ISettings.GetValueOrDefault`メソッドを使用して、プラットフォーム固有の記憶域から設定の値を取得します。 設定の値が定義されていない場合は、代わりに、既定値が取得されます。 同様に、`ISettings.AddOrUpdateValue`メソッドを使用してプラットフォーム固有のストレージ設定の値を保持します。

内の既定値を定義するではなく、`Settings`クラス、`UrlBaseDefault`文字列から値を取得する、`GlobalSetting`クラス。 次のコード例は、`BaseEndpoint`プロパティと`UpdateEndpoint`このクラスのメソッド。

```csharp
public class GlobalSetting  
{  
    ...  
    public string BaseEndpoint  
    {  
        get { return _baseEndpoint; }  
        set  
        {  
            _baseEndpoint = value;  
            UpdateEndpoint(_baseEndpoint);  
        }  
    }  
    ...  

    private void UpdateEndpoint(string baseEndpoint)  
    {  
        RegisterWebsite = string.Format("{0}:5105/Account/Register", baseEndpoint);  
        CatalogEndpoint = string.Format("{0}:5101", baseEndpoint);  
        OrdersEndpoint = string.Format("{0}:5102", baseEndpoint);  
        BasketEndpoint = string.Format("{0}:5103", baseEndpoint);  
        IdentityEndpoint = string.Format("{0}:5105/connect/authorize", baseEndpoint);  
        UserInfoEndpoint = string.Format("{0}:5105/connect/userinfo", baseEndpoint);  
        TokenEndpoint = string.Format("{0}:5105/connect/token", baseEndpoint);  
        LogoutEndpoint = string.Format("{0}:5105/connect/endsession", baseEndpoint);  
        IdentityCallback = string.Format("{0}:5105/xamarincallback", baseEndpoint);  
        LogoutCallback = string.Format("{0}:5105/Account/Redirecting", baseEndpoint);  
    }  
}
```

毎回、`BaseEndpoint`プロパティが設定されて、`UpdateEndpoint`メソッドが呼び出されます。 このメソッドは、一連のに基づいてすべてのプロパティを更新、`UrlBase`によって提供されるユーザー設定、 `Settings` eShopOnContainers のモバイル アプリに接続するさまざまなエンドポイントを表すクラスです。

## <a name="data-binding-to-user-settings"></a>ユーザー設定へのデータ バインディング

EShopOnContainers のモバイル アプリで、 `SettingsView` 2 つのユーザー設定を公開します。 これらの設定は、アプリが、Docker コンテナーとして展開されているマイクロ サービスからデータを取得する必要があるかどうかや、アプリがインターネットに接続を必要としないモック サービスからデータを取得する必要があるかどうかの構成を許可します。 コンテナー化されたマイクロ サービスからデータを取得する場合、マイクロ サービスのベース エンドポイント URL を指定する必要があります。 図 7-1 は、`SettingsView`コンテナー化されたマイクロ サービスからデータを取得するときにユーザーを選択します。

![](configuration-management-images/settings-endpoint.png "EShopOnContainers のモバイル アプリによって公開されているユーザーの設定")

**図 7-1**: eShopOnContainers のモバイル アプリによって公開されているユーザーの設定

データ バインディングを使用し、によって公開されている設定を取得することができます、`Settings`クラス。 プロパティにアクセスするモデル プロパティを表示するビューのバインド上のコントロールがこれは、`Settings`クラス、およびプロパティを発生させる変更、通知の設定値が変更された場合。 モデルし、ビューに関連付けられますは eShopOnContainers のモバイル アプリでビューを作成する方法についてを参照してください。[自動的にビュー モデルを作成するビュー モデル ロケーターと](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically_creating_a_view_model_with_a_view_model_locator)します。

次のコード例は、 [ `Entry` ](xref:Xamarin.Forms.Entry)コントロールから、`SettingsView`コンテナー化されたマイクロ サービスのベース エンドポイント URL を入力するユーザーを許可します。

```xaml
<Entry Text="{Binding Endpoint, Mode=TwoWay}" />
```

これは、 [ `Entry` ](xref:Xamarin.Forms.Entry)コントロールがバインド、`Endpoint`のプロパティ、`SettingsViewModel`クラス、双方向のバインドを使用します。 次のコード例では、エンドポイントのプロパティを示します。

```csharp
public string Endpoint  
{  
    get { return _endpoint; }  
    set  
    {  
        _endpoint = value;  

        if(!string.IsNullOrEmpty(_endpoint))  
        {  
            UpdateEndpoint(_endpoint);  
        }  

        RaisePropertyChanged(() => Endpoint);  
    }  
}
```

ときに、`Endpoint`プロパティが設定されて、`UpdateEndpoint`メソッドが呼び出されると、指定された値が有効で、プロパティ変更通知が発生します。 次のコード例は、`UpdateEndpoint`メソッド。

```csharp
private void UpdateEndpoint(string endpoint)  
{  
    Settings.UrlBase = endpoint;  
}
```

このメソッドは、更新、`UrlBase`プロパティ、`Settings`ベース エンドポイント URL の値を持つクラスは、プラットフォーム固有の記憶域に保存すると、そのユーザーが入力しました。

ときに、`SettingsView`への移動が、`InitializeAsync`メソッドで、`SettingsViewModel`クラスを実行します。 次のコード例では、このメソッドは示しています。

```csharp
public override Task InitializeAsync(object navigationData)  
{  
    ...  
    Endpoint = Settings.UrlBase;  
    ...  
}
```

メソッドのセット、`Endpoint`プロパティの値を`UrlBase`プロパティ、`Settings`クラス。 アクセス、`UrlBase`プロパティにより、Xam.Plugins.Settings ライブラリは、プラットフォーム固有の記憶域の設定値を取得します。 方法については`InitializeAsync`メソッドが呼び出されるは、「[ナビゲーション中にパラメーターを渡す](~/xamarin-forms/enterprise-application-patterns/navigation.md#passing_parameters_during_navigation)します。

このメカニズムにより、SettingsView に移動した、ときにユーザー設定はプラットフォームに固有の記憶域から取得され、データ バインドを通じて表示されます。 次に、ユーザーに、設定値が変更された場合データ バインディングによりプラットフォーム固有の記憶域には保存すぐに。

## <a name="summary"></a>まとめ

設定は、コードから、アプリの動作を構成するデータの分離を許可する、アプリを再構築せずに変更する動作。 アプリの設定は、アプリを作成および管理するには、データとユーザー設定は、アプリの動作に影響を頻繁に再調整を必要としないアプリのカスタマイズ可能な設定です。

ライブラリを使用した作成の設定にアクセス、永続化およびアプリとユーザー設定、およびデータ バインディングを取得するためのクロスプラット フォーム対応方法を使用できます、Xam.Plugins.Settings ライブラリが一貫性のある、タイプ セーフを提供します。


## <a name="related-links"></a>関連リンク

- [(2 Mb の PDF) 電子ブックをダウンロードします。](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (サンプル)](https://github.com/dotnet-architecture/eShopOnContainers)
