---
title: 構成管理
description: この章では、eShopOnContainers モバイル アプリがアプリの設定とユーザー設定を提供する構成管理を実装する方法について説明します。
ms.prod: xamarin
ms.assetid: 50d6e780-e768-47f8-9361-3af11e56b87b
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: d6cd9771760bc2932345fec24887842ce1c47376
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "35243952"
---
# <a name="configuration-management"></a>構成管理

設定は、アプリをリビルドしなくても変更する動作を許可する、コードからのアプリの動作を構成するデータの分離を許可します。 設定の 2 種類があります: アプリの設定、およびユーザー設定。

アプリの設定は、アプリを作成および管理されるデータです。 固定の web サービスのエンドポイント、API キー、およびランタイム状態などのデータに含めることができます。 アプリの設定は、アプリの存在の有無に関係し、そのアプリにわかりやすいのみです。

ユーザー設定は、アプリの動作に影響し、頻繁に再調整を必要としないアプリのカスタマイズ可能な設定です。 たとえば、アプリは、ユーザーから、データを取得する場所と、画面に表示する方法を指定を使用できます可能性があります。

Xamarin.Forms には、設定データの格納に使用できる永続的なディクショナリが含まれています。 このディクショナリを使用してアクセスできる、 [ `Application.Current.Properties` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.Properties/)アプリ、スリープ状態になり、アプリが再開されるかを再び起動するときに、復元時にプロパティ、およびそこに配置されているすべてのデータを保存します。 さらに、 [ `Application` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Application/)クラスもあります、 [ `SavePropertiesAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.SavePropertiesAsync()/)メソッドを使用して、その設定のために必要なときに保存するアプリをします。 このディクショナリの詳細については、次を参照してください。[プロパティ ディクショナリ](~/xamarin-forms/app-fundamentals/application-class.md#Properties_Dictionary)です。

Xamarin.Forms の永続的なディクショナリを使用してデータを格納する欠点は、れていない簡単にデータ バインドです。 EShopOnContainers モバイル アプリがから利用可能な Xam.Plugins.Settings ライブラリを使用するため、 [NuGet](https://www.nuget.org/packages/Xam.Plugins.Settings/)です。 このライブラリは、永続化し、各プラットフォームで提供されるネイティブの設定の管理を使用しているときにアプリとユーザーの設定を取得するための一貫した、タイプ セーフ、クロスプラット フォームのアプローチを提供します。 さらに、これはデータ バインディングを使用して、ライブラリによって公開されている設定データをアクセスする簡単です。

> [!NOTE]
> Xam.Plugin.Settings ライブラリは、アプリとユーザーの両方の設定を保存できる、これによりなし、2 つの違い。

## <a name="creating-a-settings-class"></a>設定クラスを作成します。

Xam.Plugins.Settings ライブラリを使用する場合、1 つの静的クラスを記述をアプリに必要なアプリとユーザーの設定にが含まれます。 次のコード例では、eShopOnContainers モバイル アプリで設定クラスを示しています。

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

設定を読み取るし、書き込む、 `ISettings` Xam.Plugins.Settings ライブラリによって提供される API。 このライブラリを提供する、API にアクセスするために使用するシングルトン`CrossSettings.Current`、およびアプリの設定クラスを使用してこのシングルトンを公開する必要があります、`ISettings`プロパティです。

> [!NOTE]
> Plugin.Settings および Plugin.Settings.Abstractions 名前空間の using ディレクティブは、Xam.Plugins.Settings ライブラリの型へのアクセスを必要とするクラスに追加する必要があります。

## <a name="adding-a-setting"></a>設定を追加

各設定は、キーを既定値、およびプロパティで構成されます。 次のコード例では、eShopOnContainers モバイル アプリに接続するオンライン サービスのベース URL を表すユーザー設定の 3 つすべての項目を示します。

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

キーは、キー名を定義する定数の文字列では常に、設定の既定値が必要な型の静的な読み取り専用値です。 既定値を提供することにより、有効な値が設定されていない設定が取得される場合に使用できます。

`UrlBase`静的プロパティから 2 つのメソッドを使用して、`ISettings`設定値を読み取ったり書き込んだりする API。 `ISettings.GetValueOrDefault`プラットフォーム固有の記憶域の設定の値を取得するメソッドを使用します。 設定の値が定義されていない場合は、代わりに、既定値が取得されます。 同様に、`ISettings.AddOrUpdateValue`にプラットフォーム固有の記憶域の設定の値を永続化するメソッドを使用します。

内の既定値を定義するではなく、`Settings`クラス、`UrlBaseDefault`文字列から値を取得する、`GlobalSetting`クラスです。 次のコード例は、`BaseEndpoint`プロパティおよび`UpdateEndpoint`このクラスのメソッド。

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

毎回、`BaseEndpoint`プロパティが設定されて、`UpdateEndpoint`メソッドが呼び出されます。 このメソッドは、一連のうちすべてに基づいているプロパティを更新、`UrlBase`によって提供されるユーザー設定、 `Settings` eShopOnContainers モバイル アプリに接続する別のエンドポイントを表すクラス。

## <a name="data-binding-to-user-settings"></a>ユーザーの設定へのデータ バインディング

EShopOnContainers のモバイル アプリで、 `SettingsView` 2 つのユーザー設定を公開します。 これらの設定には、かどうか、アプリからデータを取得して、Docker コンテナーとして展開されている microservices またはアプリが、インターネット接続を必要としないモック サービスからデータを取得する必要があるかどうかの構成ができるようにします。 コンテナー化 microservices からデータを取得する場合、microservices の基本のエンドポイント URL を指定する必要があります。 図 7-1 は、`SettingsView`コンテナー化 microservices からデータを取得するユーザーが選択された場合。

![](configuration-management-images/settings-endpoint.png "EShopOnContainers モバイル アプリによって公開されているユーザーの設定")

**図 7-1**: eShopOnContainers モバイル アプリによって公開されているユーザーの設定

データ バインディングを取得し、によって公開されている設定を使用することができます、`Settings`クラスです。 これはのプロパティにアクセスするモデル プロパティを表示するビューのバインド上のコントロールで実現、`Settings`クラス、およびプロパティを発生させる変更、通知の設定値が変更された場合。 EShopOnContainers モバイル アプリでのビューの作成方法に関する情報をモデル化にビューに関連付けられます、参照してください。[ビュー モデル ロケーターにビュー モデルを自動的に作成する](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically_creating_a_view_model_with_a_view_model_locator)です。

次のコード例は、 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)から制御、`SettingsView`ユーザー コンテナー化 microservices の基本のエンドポイント URL を入力することができます。

```xaml
<Entry Text="{Binding Endpoint, Mode=TwoWay}" />
```

これは、 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)コントロールのバインド先、`Endpoint`のプロパティ、`SettingsViewModel`クラス、双方向のバインドを使用します。 次のコード例は、エンドポイントのプロパティを示しています。

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

ときに、`Endpoint`プロパティが設定されて、`UpdateEndpoint`メソッドが呼び出されると、指定された値が有効となるプロパティは変更通知が発生します。 次のコード例は、`UpdateEndpoint`メソッド。

```csharp
private void UpdateEndpoint(string endpoint)  
{  
    Settings.UrlBase = endpoint;  
}
```

このメソッドは、更新、`UrlBase`プロパティに、`Settings`ベース エンドポイント URL の値を持つクラスは、プラットフォーム固有の記憶域に永続化される場合は、そのユーザーが入力しました。

ときに、`SettingsView`への移動が、`InitializeAsync`メソッドで、`SettingsViewModel`クラスを実行します。 次のコード例では、このメソッドを示します。

```csharp
public override Task InitializeAsync(object navigationData)  
{  
    ...  
    Endpoint = Settings.UrlBase;  
    ...  
}
```

メソッドのセット、`Endpoint`プロパティの値を`UrlBase`プロパティに、`Settings`クラスです。 アクセス、`UrlBase`プロパティにより、Xam.Plugins.Settings ライブラリ プラットフォーム固有の記憶域から設定値を取得します。 方法に関する情報の`InitializeAsync`メソッドが呼び出されを参照してください[ナビゲーション中にパラメーターを渡す](~/xamarin-forms/enterprise-application-patterns/navigation.md#passing_parameters_during_navigation)です。

このメカニズムによって、こと、ユーザーが、SettingsView に移動、されるたびにユーザーの設定はプラットフォーム固有の記憶域から取得され、示さデータ バインディングを使用します。 次に、ユーザーは、設定の値を変更、データ バインディングにより場合をすぐに永続化するプラットフォーム固有の記憶域にされます。

## <a name="summary"></a>まとめ

設定は、アプリをリビルドしなくても変更する動作を許可する、コードからのアプリの動作を構成するデータの分離を許可します。 アプリの設定は、アプリを作成および管理するには、データとユーザー設定は、アプリの動作に影響し、頻繁に再調整を必要としないアプリのカスタマイズ可能な設定です。

ライブラリで作成した設定にアクセスする永続化し、アプリとユーザー設定、およびデータ バインディングを取得するためのクロスプラット フォームのアプローチを使用できます、Xam.Plugins.Settings ライブラリが一貫性のある、タイプ セーフを提供します。


## <a name="related-links"></a>関連リンク

- [電子 (2 Mb PDF) のダウンロードします。](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (サンプル)](https://github.com/dotnet-architecture/eShopOnContainers)
